
### 符号总览
- **输入**：$\mathbf{X} \in \mathbb{R}^{N \times L \times B \times F}$（N=股票，L=60天，B=16根bar，F=28字段）
- **观测掩码**：$\mathbf{M} \in \{0,1\}^{N \times L \times B \times F}$（1=真实值，0=缺失/填充）
- **日间隐维**：$d=64$，**日间升级维**：$d_{inter}=128$，**截面回溯天数**：$\tau=8$


### 车间 1：日内精细加工（Bar级 → 日向量）
#### 1.1 多尺度因果滤波（去噪，不改变维度）
对每个字段 $f$、每个尺度 $k \in \{3,7,15\}$，学习一个因果卷积核 $\mathbf{w}^{(k,f)} \in \mathbb{R}^k$（由 logits $\theta$ 经 Softmax 得到，保证 $\sum w_j = 1$）。
滤波输出 $\tilde{\mathbf{X}}$ 是原始信号 + 可学习门控残差：

$$
\tilde{\mathbf{X}}_{t,b,f} = \mathbf{X}_{t,b,f} + \sum_{k} \tanh(\gamma_{k,f}) \cdot 
\frac{\sum_{j=0}^{k-1} w^{(k,f)}_j \cdot \mathbf{M}_{t-j,b,f} \cdot \mathbf{X}_{t-j,b,f}}
{\sum_{j=0}^{k-1} w^{(k,f)}_j \cdot \mathbf{M}_{t-j,b,f} + \epsilon}
$$

> **训练对象**：$\theta$（核权重）和 $\gamma$（门控开关）。

#### 1.2 字段混合层（FieldMix，F → 4F → F）
对滤波后的 $\tilde{\mathbf{X}}$，沿字段维（F）做残差 MLP。设 $\text{LN}$ 为 LayerNorm，$\text{HS}$ 为 Hardswish，则：

$$
\mathbf{Z} = \tilde{\mathbf{X}} + \mathbf{W}_2 \cdot \text{HS}\left(\mathbf{W}_1 \cdot \text{LN}(\tilde{\mathbf{X}})\right)
$$
其中 $\mathbf{W}_1 \in \mathbb{R}^{(4F) \times F}$，$\mathbf{W}_2 \in \mathbb{R}^{F \times (4F)}$。维度完整走了一遍：$F \to 4F \to F$。

> **训练对象**：$\mathbf{W}_1, \mathbf{W}_2$ 的全部神经元。

#### 1.3 日内编码器（核心 MHA 在此）
**目的**：将 $B=16$ 根 Bar 压缩成 1 个日向量。

**步骤 1：展平与投影**。把所有股票和天数的 bar 合并为 $N \cdot L$ 个独立样本：
$$
\mathcal{X} \in \mathbb{R}^{(NL) \times B \times F} \xrightarrow{\text{Linear}(F \to d)} \mathbf{H}_0 \in \mathbb{R}^{(NL) \times B \times d}
$$
加可学习位置编码 $\mathbf{P}_{pos} \in \mathbb{R}^{B \times d}$，并**拼接一个全局令牌** $\mathbf{g} \in \mathbb{R}^d$：
$$
\mathbf{H}_0' = [\mathbf{g}; \mathbf{H}_0 + \mathbf{P}_{pos}] \in \mathbb{R}^{(NL) \times (B+1) \times d}
$$

**步骤 2：带 ALiBi 偏置的多头自注意力（MHA）**。对于第 $l$ 层：
$$
\text{Attn}(\mathbf{Q}, \mathbf{K}, \mathbf{V}) = \text{Softmax}\left( \frac{\mathbf{Q}\mathbf{K}^\top}{\sqrt{d_k}} + \mathbf{B}_{\text{ALiBi}} \right) \mathbf{V}
$$
其中 $\mathbf{B}_{\text{ALiBi}}(i,j) = -m \cdot |i-j|$（距离越远惩罚越大，且对全局令牌位置不施加偏置）。

标准的 Pre-LN Transformer 层（共 2 层）：
$$
\mathbf{H}_{l+1} = \mathbf{H}_l + \text{MHA}\big(\text{LN}(\mathbf{H}_l)\big), \quad
\mathbf{H}_{l+2} = \mathbf{H}_{l+1} + \text{FFN}\big(\text{LN}(\mathbf{H}_{l+1})\big)
$$

**步骤 3：取日向量**。经过 2 层后，取全局令牌位置（索引 0）的输出，并经最终 LayerNorm：
$$
\mathbf{h}_{day} = \text{LN}\left( \mathbf{H}_2[:, 0, :] \right) \in \mathbb{R}^{(NL) \times d}
$$
重塑回日间序列：$\mathbf{H}_{day} \in \mathbb{R}^{N \times L \times d}$，此处 $d=64$。

> **训练对象**：$\mathbf{P}_{pos}, \mathbf{g}$，以及 MHA 中的 $\mathbf{W}_Q, \mathbf{W}_K, \mathbf{W}_V, \mathbf{W}_O$，还有 FFN 和 LN 的缩放/偏移。


### 车间 2：日间趋势追踪（GRU，时序演化）
**输入**：$\mathbf{H}_{day} \in \mathbb{R}^{N \times L \times 64}$。  
**操作**：单层 GRU，将特征维从 64 升至 128。对每个时间步 $t$：
$$
\mathbf{h}_t = \text{GRU}(\mathbf{x}_t, \mathbf{h}_{t-1}; \mathbf{W}_{ih}, \mathbf{W}_{hh}, \mathbf{b})
$$
展开公式（更新门 $\mathbf{z}$、重置门 $\mathbf{r}$、候选 $\tilde{\mathbf{h}}$）：
$$
\mathbf{z}_t = \sigma(\mathbf{W}_z \mathbf{x}_t + \mathbf{U}_z \mathbf{h}_{t-1} + \mathbf{b}_z)
$$
$$
\mathbf{r}_t = \sigma(\mathbf{W}_r \mathbf{x}_t + \mathbf{U}_r \mathbf{h}_{t-1} + \mathbf{b}_r)
$$
$$
\tilde{\mathbf{h}}_t = \tanh(\mathbf{W}_h \mathbf{x}_t + \mathbf{U}_h (\mathbf{r}_t \odot \mathbf{h}_{t-1}) + \mathbf{b}_h)
$$
$$
\mathbf{h}_t = (1 - \mathbf{z}_t) \odot \mathbf{h}_{t-1} + \mathbf{z}_t \odot \tilde{\mathbf{h}}_t
$$
最终输出：$\mathbf{H}_{inter} \in \mathbb{R}^{N \times L \times 128}$。

> **训练对象**：GRU 内部全部矩阵 $\mathbf{W}_*, \mathbf{U}_*, \mathbf{b}_*$。


### 车间 3：截面横向比较（逐时刻 MHA，排列不变）
**步骤 1：切片**。取最近 $\tau=8$ 天：
$$
\mathbf{H}_{\tau} = \mathbf{H}_{inter}[:, L-\tau : L, :] \in \mathbb{R}^{N \times 8 \times 128}
$$

**步骤 2：转置**。为了在“股票维”做注意力，把时间维当成 Batch：
$$
\tilde{\mathbf{H}} \in \mathbb{R}^{8 \times N \times 128} \quad (\text{即 } \tilde{\mathbf{H}} = \mathbf{H}_{\tau}.\text{permute}(1,0,2))
$$

**步骤 3：门控截面注意力（无位置编码，iTransformer 风格）**。对每个时间步 $t \in [1, 8]$：
先做多头注意力（不加位置编码，保证排列不变）：
$$
\mathbf{A}_t = \text{MHA}\big( \tilde{\mathbf{H}}_t, \tilde{\mathbf{H}}_t, \tilde{\mathbf{H}}_t \big)
$$
加**门控残差（近零初始化）**。设 $\sigma$ 为 Sigmoid，$\gamma_{attn} = -5$ 初始：
$$
\tilde{\mathbf{H}}_t' = \tilde{\mathbf{H}}_t + \sigma(\gamma_{attn}) \cdot \mathbf{A}_t
$$
再经过前馈网络的门控残差：
$$
\tilde{\mathbf{H}}_t'' = \tilde{\mathbf{H}}_t' + \sigma(\gamma_{ffn}) \cdot \text{FFN}\big( \text{LN}(\tilde{\mathbf{H}}_t') \big)
$$
**步骤 4：转置回**。  
$$
\mathbf{H}_{cross} \in \mathbb{R}^{N \times 8 \times 128}
$$

> **训练对象**：截面 MHA 的 Q/K/V/O 矩阵，以及门控参数 $\gamma_{attn}, \gamma_{ffn}$。



### 车间 4：时间聚焦与打分
#### 4.1 时序聚合器（末日 Query）
**目的**：用最新一天（第 8 天）作为 Query，聚合前 8 天信息。

设 $\mathbf{H}_{cross}$ 中，最新一天为 $\mathbf{h}_{last} \in \mathbb{R}^{N \times 128}$。

计算 Q、K、V：
$$
\mathbf{Q} = \mathbf{h}_{last} \mathbf{W}_q \in \mathbb{R}^{N \times 128}, \quad
\mathbf{K} = \mathbf{H}_{cross} \mathbf{W}_k \in \mathbb{R}^{N \times 8 \times 128}, \quad
\mathbf{V} = \mathbf{H}_{cross} \mathbf{W}_v \in \mathbb{R}^{N \times 8 \times 128}
$$

缩放点积注意力权重：
$$
\alpha = \text{Softmax}\left( \frac{\mathbf{Q} \cdot \mathbf{K}^\top}{\sqrt{128}} \right) \in \mathbb{R}^{N \times 8}
$$

加权求和得到聚合向量：
$$
\mathbf{h}_{agg} = \alpha \cdot \mathbf{V} \in \mathbb{R}^{N \times 128}
$$

> **训练对象**：$\mathbf{W}_q, \mathbf{W}_k, \mathbf{W}_v$。

#### 4.2 打分头（RankGLU）
**线性直通 + 低秩非线性残差**。设 $\gamma=0.1$ 初始。

先做 LayerNorm：$\mathbf{e} = \text{LN}(\mathbf{h}_{agg})$。

GLU 支路（先压到 64 维，再升回 1 维）：
$$
z = \sigma(\mathbf{e} \mathbf{W}_g) \odot (\mathbf{e} \mathbf{W}_v) \quad (\text{其中 } \mathbf{W}_g, \mathbf{W}_v \in \mathbb{R}^{128 \times 64})
$$
$$
\mathbf{s}_{nonlin} = z \mathbf{W}_{out} \in \mathbb{R}^N
$$

线性直通支路：
$$
\mathbf{s}_{lin} = \mathbf{e} \mathbf{W}_{lin} \in \mathbb{R}^N
$$

最终分数：
$$
\hat{\mathbf{y}} = \mathbf{s}_{lin} + \gamma \cdot \mathbf{s}_{nonlin} \in \mathbb{R}^N
$$

> **训练对象**：$\mathbf{W}_g, \mathbf{W}_v, \mathbf{W}_{out}, \mathbf{W}_{lin}$，以及 LN 参数。


### 损失函数
Batch 就是当天全截面（N 只股票）。标签 $\mathbf{y} \in \mathbb{R}^N$ 是经过风格中性化+截面标准化后的真实收益。

**1. 截面 IC 损失（主项）**：
最大化预测与标签的线性相关系数：
$$
\mathcal{L}_{IC} = 1 - \frac{\sum_{i=1}^N (\hat{y}_i - \bar{\hat{y}})(y_i - \bar{y})}{\sqrt{\sum (\hat{y}_i - \bar{\hat{y}})^2 \sum (y_i - \bar{y})^2}}
$$

**2. MSE 损失（尺度锚定）**：
$$
\mathcal{L}_{MSE} = \frac{1}{N} \sum_{i=1}^N (\hat{y}_i - y_i)^2
$$

**3. ListNet 损失（聚焦头部）**，温度 $T=1$：
$$
\mathcal{L}_{ListNet} = - \sum_{i=1}^N \text{Softmax}(y_i / T) \cdot \log\big( \text{Softmax}(\hat{y}_i / T) \big)
$$

**总损失**：
$$
\mathcal{L}_{total} = \mathcal{L}_{IC} + 0.1 \mathcal{L}_{MSE} + 0.05 \mathcal{L}_{ListNet}
$$

