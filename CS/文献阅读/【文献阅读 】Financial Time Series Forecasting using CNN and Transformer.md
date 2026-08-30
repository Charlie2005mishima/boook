
## 一、论文概述

这篇论文由摩根大通AI研究院的研究团队发表于2023年，提出了一个名为 **CTTS（CNN and Transformer based Time Series modeling）** 的混合模型，用于预测美股（标普500成分股）的日内价格走势方向（上涨/下跌/持平）。

**核心动机**：CNN擅长捕捉局部模式（短期依赖），但由于感受野有限，无法学习长期依赖；Transformer擅长学习全局上下文和长期依赖。CTTS将二者结合，用CNN提取局部时序模式并转化为"token"，再用Transformer建模这些token之间的长期依赖关系。


## 二、整体架构

### 2.1 输入与预处理

设输入时间序列为 $X = (x_1, x_2, ..., x_T) \in \mathbb{R}^T$，其中 $T=80$（用过去80个时间步预测第81步的价格变化方向）。

采用Min-Max标准化将每个时间序列缩放到 $[0, 1]$ 区间：

$$X_{std} = \frac{X - \min(X)}{\max(X) - \min(X)}$$

预测目标 $y$ 是三分类问题：
- 类别1：价格上涨
- 类别2：价格下跌
- 类别3：价格持平（flat）


### 2.2 阶段一：1D CNN Token化（捕捉短期依赖）

这是CTTS的核心创新之一——**用CNN将原始时间序列转换为token序列**，而不是直接将原始序列输入Transformer。

**一维卷积操作**：

对于输入序列 $X_{std} \in \mathbb{R}^T$，使用卷积核 $K \in \mathbb{R}^k$（$k$ 为卷积核大小，论文中 $k=16$），步长 $s=8$，在时间维度上滑动：

$$(X_{std} * K)(t) = \sum_{j=0}^{k-1} K_j \cdot X_{std}(t \cdot s + j)$$

更一般地，如果有 $d$ 个卷积核（输出通道数），则每个卷积核生成一个特征图。设卷积核权重矩阵为 $W_{cnn} \in \mathbb{R}^{d \times k}$，偏置为 $b_{cnn} \in \mathbb{R}^d$，则：

$$Z_i = \text{ReLU}(W_{cnn} \cdot X_{std}[i \cdot s : i \cdot s + k] + b_{cnn})$$

其中 $i = 0, 1, ..., m-1$，$m = \lfloor (T - k) / s \rfloor + 1$ 是token数量。

每个 $Z_i \in \mathbb{R}^d$ 就是一个 **token**，它承载了该局部窗口内的短期模式信息。论文中 $d=128$（embedding维度），$T=80$，$k=16$，$s=8$，所以 $m = \lfloor (80-16)/8 \rfloor + 1 = 9$ 个token。

**CNN需要拟合的参数**：$W_{cnn} \in \mathbb{R}^{d \times k}$ 和 $b_{cnn} \in \mathbb{R}^d$，共计 $d \times k + d = 128 \times 16 + 128 = 2176$ 个参数。

![[WIN_20260719_17_09_04_Pro.jpg|300]]


### 2.3 阶段二：位置编码（注入时序信息）

Transformer本身是置换不变的（permutation invariant）——它不关心token的先后顺序。为了让模型知道token的时间顺序，需要加入位置编码。

论文采用原始Transformer的正弦位置编码：

$$PE_{(pos, 2i)} = \sin\left(\frac{pos}{10000^{2i/d}}\right)$$

$$PE_{(pos, 2i+1)} = \cos\left(\frac{pos}{10000^{2i/d}}\right)$$

其中 $pos$ 是token的位置索引（$0$ 到 $m-1$），$i$ 是维度索引，$d=128$ 是embedding维度。

将位置编码加到token上：

$$H_{pos} = Z + PE_{pos}$$

**位置编码不需要拟合参数**——它是固定的数学函数。


### 2.4 阶段三：Transformer编码器（捕捉长期依赖）

Transformer编码器由 $L=4$ 层组成，每层包含两个核心子层：

#### （1）多头自注意力机制（Multi-Head Self-Attention）

**单头自注意力的数学表达**：

给定输入序列 $H \in \mathbb{R}^{m \times d}$（$m$ 个token，每个维度 $d$），首先通过三个可学习的权重矩阵生成Q、K、V：

$$Q = H W_Q, \quad K = H W_K, \quad V = H W_V$$

其中 $W_Q, W_K, W_V \in \mathbb{R}^{d \times d_k}$，$d_k = d / h$ 是每个注意力头的维度，$h=4$ 是注意力头数。

**缩放点积注意力**：

$$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right) V$$



其中：
- $QK^T \in \mathbb{R}^{m \times m}$：计算每对token之间的相似度（点积）
- $\sqrt{d_k}$：缩放因子，防止点积值过大导致softmax梯度饱和
- $\text{softmax}$：将相似度转换为概率权重（和为1）
- 最终输出是对 $V$ 的加权求和

**多头注意力**：并行运行 $h$ 个注意力头，每个头学习不同的"视角"：

$$\text{MultiHead}(H) = \text{Concat}(head_1, head_2, ..., head_h) W_O$$

其中 $head_i = \text{Attention}(H W_Q^i, H W_K^i, H W_V^i)$，$W_O \in \mathbb{R}^{d \times d}$ 是输出投影矩阵。

**多头注意力需要拟合的参数**：$W_Q^i, W_K^i, W_V^i \in \mathbb{R}^{d \times d_k}$（每个头3个矩阵），以及 $W_O \in \mathbb{R}^{d \times d}$。共计 $h \times 3 \times d \times d_k + d \times d = 4 \times 3 \times 128 \times 32 + 128 \times 128 = 49152 + 16384 = 65536$ 个参数（每层）。

#### （2）前馈网络（Feed-Forward Network, FFN）

每个注意力子层后接一个两层全连接网络：

$$\text{FFN}(x) = \text{ReLU}(x W_1 + b_1) W_2 + b_2$$

其中 $W_1 \in \mathbb{R}^{d \times d_{ff}}$，$W_2 \in \mathbb{R}^{d_{ff} \times d}$。论文未明确给出 $d_{ff}$，但通常 $d_{ff} = 4d = 512$。

**FFN需要拟合的参数**：$W_1 \in \mathbb{R}^{d \times d_{ff}}$，$b_1 \in \mathbb{R}^{d_{ff}}$，$W_2 \in \mathbb{R}^{d_{ff} \times d}$，$b_2 \in \mathbb{R}^d$。共计 $128 \times 512 + 512 + 512 \times 128 + 128 = 65536 + 512 + 65536 + 128 = 131712$ 个参数（每层）。

**残差连接与层归一化**（每子层后）：

$$\text{output} = \text{LayerNorm}(x + \text{Sublayer}(x))$$

LayerNorm的参数（可训练的 $\gamma$ 和 $\beta$）也需要拟合，每层约 $2d = 256$ 个参数，相对较少。


### 2.5 阶段四：分类头（MLP + Softmax）

Transformer编码器输出一个 $m \times d$ 的矩阵（$m=9$ 个token，每个 $d=128$ 维）。论文的做法是将所有token的表示聚合（通常取平均或使用第一个token的表示），然后通过一个MLP多层感知机进行分类：

$$\text{logits} = \text{MLP}(H_{agg})$$

$$\hat{y} = \text{softmax}(\text{logits})$$

其中 $\hat{y} \in \mathbb{R}^3$ 是三个类别的概率分布（和为1）。

**MLP需要拟合的参数**：假设单隐藏层，维度为 $d \rightarrow 64 \rightarrow 3$，则参数为 $128 \times 64 + 64 + 64 \times 3 + 3 = 8192 + 64 + 192 + 3 = 8451$ 个。


### 2.6 参数总量估算

| 模块                                          | 参数数量                                    |
| ------------------------------------------- | --------------------------------------- |
| 1D CNN（token化）                              | ~2,176                                  |
| Transformer × 4层（每层多头注意力 + FFN + LayerNorm） | ~4 × (65,536 + 131,712 + 256) ≈ 790,016 |
| 分类头MLP                                      | ~8,451                                  |
| **总计**                                      | **≈800,000**                            |

这个参数量（约80万）介于10万到1亿之间，对于Transformer架构来说属于**轻量级模型**。


## 三、损失函数与梯度下降

### 3.1 损失函数：交叉熵损失

CTTS是一个三分类问题（涨/跌/持平），采用**交叉熵损失**（Cross-Entropy Loss）：

$$\mathcal{L} = -\frac{1}{N} \sum_{i=1}^{N} \sum_{c=1}^{3} y_{i,c} \log(\hat{y}_{i,c})$$

其中：
- $N$ 是batch size（论文中 $N=64$）
- $y_{i,c} \in \{0, 1\}$ 是第 $i$ 个样本的真实标签（one-hot编码）
- $\hat{y}_{i,c} \in (0, 1)$ 是模型预测的第 $c$ 类的概率


### 3.2 优化器：AdamW

论文使用 **AdamW** 优化器。AdamW是Adam的变体，核心改进是**将权重衰减（L2正则化）与梯度更新解耦**，避免了Adam中L2正则化与自适应学习率相互作用的问题。

**Adam的更新规则**（AdamW在其基础上修改）：

在第 $t$ 步，计算损失对参数 $\theta$ 的梯度 $g_t = \nabla_\theta \mathcal{L}(\theta_{t-1})$。

**一阶矩估计**（梯度的指数移动平均）：

$$m_t = \beta_1 m_{t-1} + (1 - \beta_1) g_t$$

**二阶矩估计**（梯度平方的指数移动平均）：

$$v_t = \beta_2 v_{t-1} + (1 - \beta_2) g_t^2$$

**偏差校正**（因初始时刻 $m_0=0, v_0=0$ 导致估计偏向0）：

$$\hat{m}_t = \frac{m_t}{1 - \beta_1^t}, \quad \hat{v}_t = \frac{v_t}{1 - \beta_2^t}$$

**AdamW的参数更新**（与Adam的区别在于权重衰减的位置）：

$$\theta_t = \theta_{t-1} - \eta \left( \frac{\hat{m}_t}{\sqrt{\hat{v}_t} + \epsilon} + \lambda \theta_{t-1} \right)$$

其中：
- $\eta$：学习率（learning rate）
- $\beta_1, \beta_2$：动量衰减系数（通常 $\beta_1=0.9, \beta_2=0.999$）
- $\epsilon$：数值稳定项（通常 $\epsilon=10^{-8}$）
- $\lambda$：权重衰减系数（L2正则化强度）

**论文中的超参数设置**：
- Batch size：64
- Max epochs：100
- Dropout rate：0.3（防止过拟合）
- AdamW优化器


### 3.3 梯度下降的完整流程

**前向传播**（计算损失）：

$$X_{std} \xrightarrow{\text{1D CNN}} Z \xrightarrow{+PE} H \xrightarrow{\text{Transformer}} H_{agg} \xrightarrow{\text{MLP}} \hat{y} \xrightarrow{\text{CrossEntropy}} \mathcal{L}$$

**反向传播**（计算梯度）：

利用**链式法则**，从损失函数 $\mathcal{L}$ 开始，逐层反向计算梯度：

$$\frac{\partial \mathcal{L}}{\partial \theta} = \frac{\partial \mathcal{L}}{\partial \hat{y}} \cdot \frac{\partial \hat{y}}{\partial H_{agg}} \cdot \frac{\partial H_{agg}}{\partial H} \cdot ... \cdot \frac{\partial Z}{\partial W_{cnn}}$$

**参数更新**（AdamW）：

$$\theta_{t} = \theta_{t-1} - \eta \left( \frac{\hat{m}_t}{\sqrt{\hat{v}_t} + \epsilon} + \lambda \theta_{t-1} \right)$$

对所有可训练参数执行此更新：$W_{cnn}, b_{cnn}$（CNN层），$W_Q^i, W_K^i, W_V^i, W_O$（每层多头注意力），$W_1, b_1, W_2, b_2$（每层FFN），LayerNorm的 $\gamma, \beta$（每层），以及MLP的所有权重和偏置。

**重复**上述前向-反向-更新循环，直到达到最大epoch（100）或验证集性能不再提升。


### 3.4 训练稳定性：Dropout

论文使用 **Dropout率 0.3** 来防止过拟合。Dropout在训练时以概率 $p=0.3$ 随机将神经元输出置零：

$$h_{drop} = h \odot \text{Bernoulli}(1-p)$$

这相当于在每次前向传播时随机"丢弃"30%的神经元，迫使模型不依赖任何单个特征，从而增强泛化能力。在推理（测试）时，所有神经元都保留，但输出会乘以 $(1-p)$ 以保持期望值一致。


## 四、关键设计决策总结

| 设计选择 | 论文取值 | 作用 |
|---------|---------|------|
| CNN卷积核大小 | 16 | 控制局部感受野大小 |
| CNN步长 | 8 | 控制token数量（降采样） |
| Embedding维度 | 128 | token的向量表示维度 |
| Transformer层数 | 4 | 模型深度 |
| 注意力头数 | 4 | 并行"视角"数量 |
| Dropout率 | 0.3 | 防止过拟合 |
| Batch size | 64 | 每步处理的样本数 |
| Max epochs | 100 | 最大训练轮数 |
| 优化器 | AdamW | 自适应学习率 + 解耦权重衰减 |
| 损失函数 | Cross-Entropy | 多分类标准损失 |

**核心思想**：CNN负责"分词"——把连续的时间序列切成有意义的局部片段（token），每个token编码了短期模式；Transformer负责"阅读理解"——理解这些token之间的长期依赖关系。这种分工让CNN和Transformer各司其职，而不是让Transformer直接处理海量的原始时间点。