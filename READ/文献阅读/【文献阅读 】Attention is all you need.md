#  Attention Is All You Need

- **作者**: Ashish Vaswani, Noam Shazeer, Niki Parmar, Jakob Uszkoreit, Llion Jones, Aidan N. Gomez, Łukasz Kaiser, Illia Polosukhin (Google Brain / Google Research / University of Toronto)
- **发表**: NIPS 2017
- **链接**: [ArXiv](https://arxiv.org/abs/1706.03762)
- **标签**: #方法/理论 #NLP #深度学习 #注意力机制 #序列建模

---

## 一、论文概述

这篇由 Google Brain 团队于 2017 年发表的论文，提出了 **Transformer** 架构——第一个完全基于注意力机制、摒弃了循环（RNN）和卷积（CNN）的序列转录模型（sequence transduction model）。

**核心贡献**：在 WMT 2014 英德翻译任务上达到 28.4 BLEU，超越此前最好结果（含集成模型）2 个 BLEU 以上；在英法翻译任务上达到 41.8 BLEU（单模型 SOTA），仅用 8 块 P100 GPU 训练 3.5 天，训练成本仅为当时最佳模型的几分之一。更重要的是，它奠定了 GPT、BERT、LLaMA 等所有现代大语言模型的架构基础。

论文还验证了 Transformer 的泛化能力——在 English Constituency Parsing（英语句法成分解析）任务上同样取得优异结果（WSJ 23 F1=92.7，半监督设置下超过此前所有模型）。

### 作者贡献注记

论文特别标注了八位作者的等贡献声明（"Equal contribution. Listing order is random."）：
- **Jakob** 提出用自注意力替换 RNN 的想法并发起评估工作
- **Ashish** 和 **Illia** 设计并实现了第一个 Transformer 模型
- **Noam** 提出了缩放点积注意力、多头注意力和无参数位置编码
- **Niki** 设计/实现/调参了无数模型变体
- **Llion** 负责初始代码库、高效推理和可视化
- **Łukasz** 和 **Aidan** 设计并实现了 tensor2tensor 框架

---

## 二、为什么需要 Transformer？

### 2.1 研究背景与相关工作

在 Transformer 之前，序列建模的王者是 **RNN/LSTM**[13] 和 **GRU**[7]，它们天然适合处理序列数据。但同时期，减少串行计算的工作也在推进：

| 工作 | 方法 | 特点 |
|------|------|------|
| Extended Neural GPU [16] | 卷积 | 用 CNN 作为基本构建块并行计算 |
| ByteNet [18] | 空洞卷积 | 距离对数缩减，路径长度 $O(\log_k n)$ |
| ConvS2S [9] | 卷积 | 线性增长，路径长度 $O(n/k)$ |

这些模型的共同问题是：两个任意位置之间建立联系的**操作数**随距离增长（ConvS2S 线性增长，ByteNet 对数增长），导致学习长距离依赖变得困难 [12]。

### 2.2 传统模型的困境

**RNN/LSTM 的痛点**：按时间顺序逐步处理序列，隐藏状态 $h_t = f(h_{t-1}, x_t)$。这导致两个问题：

1. **无法并行**：第 $t$ 步必须等前 $t-1$ 步算完，序列越长越严重
2. **长距离依赖困难**：早期信息在多次传递中逐渐丢失（梯度消失 [12]），尽管 LSTM/GRU 有所缓解但未根本解决

**CNN 的痛点**：虽然可以并行，但感受野有限。要让相距 $n$ 的两个位置建立联系，需要堆叠 $O(n/k)$ 层（连续卷积）或 $O(\log_k n)$ 层（空洞卷积）。卷积层通常比循环层贵 $k$ 倍。

### 2.3 Transformer 的突破

Transformer 将**任意两个位置之间的信号路径长度**降为常数 $O(1)$，同时支持完全的并行计算。代价是注意力机制的加权平均降低了"有效分辨率"（effective resolution），但论文通过**多头注意力**来弥补这一问题。

表 1 总结了不同层类型的复杂度对比：

| 层类型 | 每层复杂度 | 最少串行操作数 | 最大路径长度 |
|--------|-----------|---------------|-------------|
| 自注意力 | $O(n^2 \cdot d)$ | $O(1)$ | $O(1)$ |
| 循环 | $O(n \cdot d^2)$ | $O(n)$ | $O(n)$ |
| 卷积 | $O(k \cdot n \cdot d^2)$ | $O(1)$ | $O(\log_k n)$ |
| 自注意力（受限） | $O(r \cdot n \cdot d)$ | $O(1)$ | $O(n/r)$ |

---

## 三、模型架构的数学表达

Transformer 采用标准的**编码器-解码器**结构（沿用自 seq2seq 框架 [5, 2, 35]）。

> 图 1（论文 Figure 1）展示了完整的 Transformer 架构，左侧为编码器，右侧为解码器。

### 3.1 编码器与解码器堆叠

**编码器**由 $N=6$ 个相同层堆叠而成，每层包含**两个**子层：
1. 多头自注意力（Multi-Head Self-Attention）
2. 逐位置全连接前馈网络（Position-wise FFN）

**解码器**也由 $N=6$ 个相同层堆叠而成，每层包含**三个**子层：
1. 带掩码的多头自注意力（Masked Multi-Head Self-Attention）
2. 编码器-解码器多头注意力（Cross-Attention）
3. 逐位置全连接前馈网络

每个子层周围都有**残差连接** [11] + **层归一化** [1]：

$$\text{output} = \text{LayerNorm}(x + \text{Sublayer}(x))$$

为使残差连接可行，所有子层输出统一为 $d_{model}=512$ 维。

### 3.2 输入表示：嵌入 + 位置编码

#### 3.2.1 词嵌入

输入序列的每个 token 通过可学习的嵌入矩阵映射为 $d_{model}=512$ 维向量。论文使用**字节对编码**（Byte-Pair Encoding, BPE [3, 31]），英德数据集共享约 37,000 个 token 的词表。

$$X = (x_1, x_2, ..., x_n), \quad x_i \in \mathbb{R}^{d_{model}}$$

**嵌入层缩放**：论文在嵌入层将权重乘以 $\sqrt{d_{model}}$（$\sqrt{512} \approx 22.6$），因为嵌入权重初始化方差较小，缩放补偿了位置编码加法后的方差变化。这一细节容易被人忽略但影响训练稳定性。

#### 3.2.2 位置编码（Positional Encoding）

由于自注意力本身是**置换不变的**（不关心词的顺序），需要注入位置信息。论文使用正弦/余弦函数：

$$PE_{(pos, 2i)} = \sin\left(\frac{pos}{10000^{2i / d_{model}}}\right)$$

$$PE_{(pos, 2i+1)} = \cos\left(\frac{pos}{10000^{2i / d_{model}}}\right)$$

其中 $pos$ 是位置索引，$i$ 是维度索引。波长从 $2\pi$ 到 $10000 \cdot 2\pi$ 呈几何级数分布。

**为什么用正弦/余弦？**

1. **可学习相对位置**：对于任意固定偏移 $k$，$PE_{pos+k}$ 可表示为 $PE_{pos}$ 的线性函数。这使模型能轻松学习"距离为 $k$ 的两个位置之间的关系"
2. **可外推**：正弦版本可能允许模型泛化到训练期间未见过的更长序列（learned positional embeddings 无法外推）

论文也实验了**可学习位置嵌入**[9]，效果几乎相同（Table 3 Row E），选择正弦版本主要看中其外推潜力。

最终输入为：$H = X \cdot \sqrt{d_{model}} + PE$（逐元素相加）

### 3.3 缩放点积注意力（Scaled Dot-Product Attention）

这是 Transformer 最核心的计算单元。论文将注意力描述为"将 query 和一组 key-value 对映射到输出"的函数，输出是对 values 的加权求和。

$$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right) V$$

其中 $Q \in \mathbb{R}^{n \times d_k}$，$K \in \mathbb{R}^{n \times d_k}$，$V \in \mathbb{R}^{n \times d_v}$。

**逐步拆解**：

1. **$QK^T$**：计算所有位置对之间的**相似度**（点积），得到 $n \times n$ 的注意力分数矩阵
2. **$\div \sqrt{d_k}$**：**缩放**，防止点积值过大导致 softmax 梯度饱和
3. **$\text{softmax}$**：将分数转换为**概率分布**（每行和为 1）
4. **$\times V$**：用注意力权重对值进行**加权求和**

**为什么用点积注意力而非加性注意力？** 点积注意力可以用高度优化的矩阵乘法实现，比加性注意力更快、更省内存。虽然两者理论复杂度相似，但在 GPU 上点积版本有明显优势。

**为什么要除以 $\sqrt{d_k}$？** 假设 $Q$ 和 $K$ 的元素是均值为 0、方差为 1 的独立随机变量，则 $QK^T$ 中每个元素的均值为 0、方差为 $d_k$。当 $d_k=64$ 时，点积值的数量级约为 $\sqrt{64}=8$，softmax 进入梯度极小的饱和区。除以 $\sqrt{d_k}$ 将方差控制在 1 左右，保证梯度健康。

对于较小的 $d_k$ 值，加性注意力 [2] 和点积注意力表现相似；但对于较大的 $d_k$，加性注意力优于**未缩放**的点积注意力 [3]。缩放因子正是为了解决这一问题。

### 3.4 多头注意力（Multi-Head Attention）

单头注意力只能捕获一种依赖关系，且平均化会抑制多角度理解能力。语言的依赖关系是多样的（语法、语义、指代、长距离修饰等）。多头注意力通过 $h=8$ 个并行的头来解决：

$$\text{MultiHead}(Q, K, V) = \text{Concat}(\text{head}_1, ..., \text{head}_h) W^O$$

$$\text{head}_i = \text{Attention}(Q W_i^Q, K W_i^K, V W_i^V)$$

其中投影矩阵：
- $W_i^Q \in \mathbb{R}^{d_{model} \times d_k}$，$W_i^K \in \mathbb{R}^{d_{model} \times d_k}$
- $W_i^V \in \mathbb{R}^{d_{model} \times d_v}$，$W^O \in \mathbb{R}^{h d_v \times d_{model}}$

论文取 $h=8$，$d_k = d_v = d_{model} / h = 64$。**关键设计**：由于每个头的维度减少，总计算量与单头全维注意力相当——多头并没有增加额外计算成本。

**多头注意力的三个应用场景**：

| 场景 | Query 来源 | Key/Value 来源 | 作用 |
|------|----------|---------------|------|
| 编码器自注意力 | 上一编码器层 | 同一来源 | 捕捉输入序列内部任意位置间的依赖 |
| 解码器自注意力 | 上一解码器层 | 同一来源（带掩码） | 保持自回归特性，防止看到未来位置 |
| 编码器-解码器注意力 | 解码器 | 编码器输出 | 让解码器的每个位置关注输入序列的全部位置 |

解码器的**掩码**（Masking）：将未来位置（$j > i$）的注意力分数设为 $-\infty$，经 softmax 后权重为 0。结合输出嵌入右移一位，确保位置 $i$ 的预测只依赖位置 $< i$ 的已知输出。

### 3.5 逐位置前馈网络（Position-wise Feed-Forward Networks）

每个编码器/解码器层都包含一个全连接前馈网络，**逐位置**（position-wise）独立应用——即同一层中不同位置共享 FFN 参数，但计算独立：

$$\text{FFN}(x) = \max(0, x W_1 + b_1) W_2 + b_2$$

等价于两个核大小为 1 的卷积。其中 $W_1 \in \mathbb{R}^{d_{model} \times d_{ff}}$，$W_2 \in \mathbb{R}^{d_{ff} \times d_{model}}$，$d_{ff}=2048$（约为 $d_{model}$ 的 4 倍）。

**为什么是 2048？** 内部维度给 FFN 足够的容量在"局部"处理注意力层输出的聚合信息。注意力负责"路由"全局信息，FFN 负责"变换"每个位置的表示。

### 3.6 嵌入层与 Softmax 输出

解码器输出通过线性变换和 softmax 生成下一个 token 的概率分布：

$$P(y_t | y_{<t}, x) = \text{softmax}(z_t W_{out} + b_{out})$$

其中 $W_{out} \in \mathbb{R}^{d_{model} \times V}$，$V$ 是词表大小。

论文采用了**权重共享**（Weight Tying）策略 [30]：**输入嵌入层、输出嵌入层和 pre-softmax 线性变换共享同一权重矩阵**。这既减少了参数量，也是一种有效的正则化手段（类似 Press & Wolf [30] 的做法）。

---

## 四、为什么是自注意力？（Why Self-Attention）

> 论文第 4 节专门对比了自注意力与循环层、卷积层的优劣。这是理解 Transformer 设计动机的关键。

### 三个评估维度

论文从三个维度（desiderata）对比不同层类型：

1. **每层总计算复杂度**（Total computational complexity per layer）
2. **可并行化的计算量**（以最少所需串行操作数衡量）
3. **长距离依赖的路径长度**（网络中前向和反向信号需要穿过的路径长度）——这是学习长距离依赖的核心挑战 [12]

**路径长度为何重要？** 前向和反向信号在网络中任意两个位置间穿过的路径越短，梯度消失/爆炸越弱，长距离依赖越容易学习。这是 Hochreiter 等人 [12] 的核心观点。

### 关键对比结论

| 维度 | 自注意力 | RNN | CNN |
|------|---------|-----|-----|
| 串行操作数 | $O(1)$ ✅ | $O(n)$ ❌ | $O(1)$ ✅ |
| 最大路径长度 | $O(1)$ ✅ | $O(n)$ ❌ | $O(\log_k n)$ ⚠️ |
| 每层复杂度 | $O(n^2 \cdot d)$ | $O(n \cdot d^2)$ | $O(k \cdot n \cdot d^2)$ |

**实际分析**：
- 当序列长度 $n < d$（表示维度）时，自注意力**每层更快**——这在机器翻译中很常见（word-piece [38] 和 BPE [31] 表示的句子长度通常 < 512）
- 卷积层需要堆叠 $O(n/k)$（连续卷积）或 $O(\log_k n)$（空洞卷积）层才能覆盖全序列，增加了路径长度
- **可分离卷积**[6] 将复杂度降为 $O(k \cdot n \cdot d + n \cdot d^2)$。当 $k=n$ 时，其复杂度恰好等于**自注意力 + FFN 的组合**——这正是 Transformer 的做法

### 自注意力的可解释性附加收益

论文指出自注意力能产生**更有可解释性的模型**。通过检查注意力分布，不仅可以发现不同头学习了不同的任务，许多头还展现出与句法和语义结构相关的行为（详见附录可视化分析）。

### 长序列扩展思路

对于超长序列（$n$ 很大，$n > d$），论文提出**受限自注意力**（restricted self-attention）：每个输出位置只关注其周围半径为 $r$ 的邻域，复杂度降为 $O(r \cdot n \cdot d)$，最大路径长度增加到 $O(n/r)$。论文表示将在未来工作中探索这一方向——后续的 Sparse Transformer、Longformer 等工作正是沿着这个思路展开。

---

## 五、可训练参数详解

### 5.1 参数总览（以 Base 模型为例）

| 模块 | 参数数量 | 计算公式 |
|------|---------|---------|
| **输入嵌入层** | $V \times d_{model}$ | 约 37k × 512 ≈ 19M |
| **编码器 ×6 层** | 6 × (注意力 + FFN + LayerNorm) | ≈ 6 × 2.1M ≈ 12.6M |
| **解码器 ×6 层** | 6 × (自注意力 + 交叉注意力 + FFN + LayerNorm) | ≈ 6 × 3.0M ≈ 18M |
| **输出层** | $d_{model} \times V$（与嵌入层共享） | 0（共享） |
| **总计** | | **约 65M 参数** |

### 5.2 逐层参数拆解

**（1）多头注意力层**（$d_{model}=512$，$h=8$，$d_k=d_v=64$）

每个头有 3 个投影矩阵，加上输出投影：

$$4 \times d_{model} \times d_{model} = 4 \times 512 \times 512 = 1,048,576$$

实际上 $W_i^Q, W_i^K, W_i^V$ 的参数量是 $h \times d_{model} \times d_k \times 3 = 8 \times 512 \times 64 \times 3 = 786,432$，$W^O$ 是 $512 \times 512 = 262,144$，合计 **1,048,576**。

**（2）前馈网络层**（$d_{ff}=2048$）

$$W_1: 512 \times 2048 = 1,048,576$$
$$b_1: 2048$$
$$W_2: 2048 \times 512 = 1,048,576$$
$$b_2: 512$$

合计约 **2.1M** 参数（含偏置）。

**（3）层归一化**（每子层后）

$$\gamma, \beta \in \mathbb{R}^{d_{model}} \Rightarrow 2 \times 512 = 1024 \text{ 参数}$$

每个编码器层有 2 个子层 → 2,048 个 LN 参数；每个解码器层有 3 个子层 → 3,072 个 LN 参数。

### 5.3 Base 模型 vs Big 模型

| 参数 | Base 模型 | Big 模型 |
|------|---------|---------|
| $d_{model}$ | 512 | 1024 |
| $d_{ff}$ | 2048 | 4096 |
| $h$（头数） | 8 | 16 |
| $d_k = d_v$ | 64 | 64 |
| $N$（层数） | 6 | 6 |
| Dropout $P_{drop}$ | 0.1 | 0.3 |
| 参数量 | 65M | 213M |
| 训练步数 | 100K | 300K |
| 每步时间 | 0.4 秒 | 1.0 秒 |
| 训练时间 | 12 小时 | 3.5 天 |

---

## 六、训练策略与优化

### 6.1 训练数据与批处理

| 数据集 | 规模 | 词表 | 编码方式 |
|--------|------|------|---------|
| WMT 2014 EN-DE | 约 450 万句对 | 约 37K tokens | Byte-Pair Encoding (BPE) [31] |
| WMT 2014 EN-FR | 约 3600 万句对 | 约 32K tokens | Word-Piece [38] |

**Batch 构建方式**：按近似序列长度配对，每个训练 batch 包含约 25,000 个源 token 和 25,000 个目标 token（动态批大小，长句少、短句多）。

**硬件**：单机 8 块 NVIDIA P100 GPU。

### 6.2 损失函数：交叉熵损失

对于自回归序列建模任务，使用**交叉熵损失**：

$$\mathcal{L} = -\frac{1}{N} \sum_{i=1}^{N} \sum_{t=1}^{T} \log P(y_t^{(i)} | y_{<t}^{(i)}, x^{(i)})$$

其中 $N$ 是 batch 中的样本数，$T$ 是目标序列长度。

### 6.3 反向传播：梯度如何流动

**前向传播路径**（以编码器层为例）：

$$X \xrightarrow{+PE} H \xrightarrow{\text{MultiHeadAttn}} A \xrightarrow{+H} \xrightarrow{\text{LayerNorm}} L_1 \xrightarrow{\text{FFN}} F \xrightarrow{+L_1} \xrightarrow{\text{LayerNorm}} L_2$$

**反向传播**通过链式法则逐层计算梯度：

$$\frac{\partial \mathcal{L}}{\partial W} = \frac{\partial \mathcal{L}}{\partial \hat{y}} \cdot \frac{\partial \hat{y}}{\partial \text{LayerNorm}} \cdot \frac{\partial \text{LayerNorm}}{\partial \text{FFN}} \cdot ... \cdot \frac{\partial \text{Attention}}{\partial W}$$

**残差连接的关键作用**：梯度可以从输出层**直接"短路"** 传播到浅层，$\frac{\partial \mathcal{L}}{\partial H} = \frac{\partial \mathcal{L}}{\partial A} + \frac{\partial \mathcal{L}}{\partial L_2}$，有效防止了深层网络的梯度消失。这也是 Transformer 能堆叠 6 层甚至后续百层的关键。

**多头注意力的梯度**：每个头独立计算梯度，$\frac{\partial \mathcal{L}}{\partial W_i^Q} = \frac{\partial \mathcal{L}}{\partial \text{head}_i} \cdot \frac{\partial \text{head}_i}{\partial W_i^Q}$，各头的梯度在 $W^O$ 处汇聚。

### 6.4 优化器：Adam + Warmup 学习率调度

#### Adam 优化器

论文使用 **Adam 优化器**[20]，超参数为 $\beta_1=0.9$，$\beta_2=0.98$，$\epsilon=10^{-9}$。

Adam 的更新规则（第 $t$ 步）：

$$m_t = \beta_1 m_{t-1} + (1-\beta_1) g_t \quad \text{(一阶矩估计)}$$
$$v_t = \beta_2 v_{t-1} + (1-\beta_2) g_t^2 \quad \text{(二阶矩估计)}$$
$$\hat{m}_t = \frac{m_t}{1-\beta_1^t}, \quad \hat{v}_t = \frac{v_t}{1-\beta_2^t} \quad \text{(偏差校正)}$$
$$\theta_t = \theta_{t-1} - \eta \cdot \frac{\hat{m}_t}{\sqrt{\hat{v}_t} + \epsilon}$$

**注意**：$\beta_2=0.98$ 而非默认的 0.999——由于 Transformer 梯度噪声较大，稍低的 $\beta_2$ 让二阶矩估计对近期梯度更敏感。

#### 学习率调度（Warmup + 反平方根衰减）

论文采用独特的学习率调度策略：

$$lrate = d_{model}^{-0.5} \cdot \min(step\_num^{-0.5}, step\_num \cdot warmup\_steps^{-1.5})$$

- **前 $warmup\_steps=4000$ 步**：学习率**线性增加**（$\propto step\_num$）
- **之后**：学习率**按步数平方根倒数衰减**（$\propto 1/\sqrt{step\_num}$）

其中 $d_{model}^{-0.5} = 1/\sqrt{512} \approx 0.044$ 是缩放因子——模型越大，初始学习率越小。

**为什么需要 Warmup？** 在训练初期，参数是随机初始化的，注意力分布几乎是均匀的，梯度可能带有较大噪声。Warmup 让模型用小学习率平稳度过这一阶段，等注意力模式初步建立后再加速。

### 6.5 正则化技术

**（1）Dropout**[33]

在所有三个位置应用，$P_{drop}=0.1$（base）：
- 每个子层的输出（加残差之前）
- 嵌入与位置编码的加法结果
- Big 模型英德使用 $P_{drop}=0.3$，英法使用 $P_{drop}=0.1$

**（2）标签平滑**（Label Smoothing）[36]

$\epsilon_{ls}=0.1$，将 one-hot 目标分布改为：

$$y_{smooth} = (1-\epsilon_{ls}) \cdot y_{onehot} + \epsilon_{ls} / V$$

虽然**增加了困惑度**（约 0.6 PPL，因模型学得更"不确定"），但**提升了 BLEU 分数**和泛化能力。这是典型的 perplexity 与生成质量的权衡。

### 6.6 推理策略

- **检查点平均**（Checkpoint Averaging）：Base 模型平均最后 5 个检查点（每 10 分钟保存一次）；Big 模型平均最后 20 个检查点。这是一种简单有效的模型集成近似
- **束搜索**（Beam Search）：beam size=4，长度惩罚 $\alpha=0.6$ [38]
- **最大输出长度**：输入长度 + 50（英德）/ + 300（解析任务），但尽可能提前终止
- 推理超参数在开发集上调优

---

## 七、实验结果

### 7.1 机器翻译（Table 2）

| 模型 | EN-DE BLEU | EN-FR BLEU | EN-DE FLOPs | EN-FR FLOPs |
|------|-----------|-----------|-------------|-------------|
| ByteNet [18] | 23.75 | — | — | — |
| Deep-Att + PosUnk [39] | — | 39.2 | — | 1.0×10²⁰ |
| GNMT + RL [38] | 24.6 | 39.92 | 2.3×10¹⁹ | 1.4×10²⁰ |
| ConvS2S [9] | 25.16 | 40.46 | 9.6×10¹⁸ | 1.5×10²⁰ |
| MoE [32] | 26.03 | 40.56 | 2.0×10¹⁹ | 1.2×10²⁰ |
| GNMT + RL Ensemble | 26.30 | 41.16 | 1.8×10²⁰ | 1.1×10²¹ |
| ConvS2S Ensemble | 26.36 | 41.29 | 7.7×10¹⁹ | 1.2×10²¹ |
| **Transformer (base)** | **27.3** | **38.1** | **3.3×10¹⁸** | — |
| **Transformer (big)** | **28.4** | **41.8** | **2.3×10¹⁹** | — |

**关键信息**：
- Big 模型 EN-DE 成绩（28.4 BLEU）超过所有集成模型 2+ BLEU
- Base 模型 EN-DE（27.3 BLEU）已超过此前所有已发表的单模型和集成模型
- **训练成本**：Base 模型训练 FLOPs 仅为 ConvS2S 的 1/3，Big 模型也远低于竞争对手

### 7.2 模型变体消融实验（Table 3）

论文通过控制变量实验验证了各组件的重要性（均基于 EN-DE 开发集 newstest2013）：

**A. 头数与 $d_k$/$d_v$ 的变化**（保持总计算量恒定）：
- 单头：PPL 5.29 / BLEU 24.9（比最佳差 0.9 BLEU）
- 8 头（论文设置）：PPL 4.91 / BLEU 25.8 ✅
- 32 头：PPL 5.01 / BLEU 25.4（头太多反而下降）

> 结论：单头削弱了模型从多个子空间理解语言的能力；但头过多导致每个头的维度太小，表示能力不足。

**B. 减少 $d_k$**（降低 key 维度）：
- $d_k=16$：BLEU 25.1 / 58M
- $d_k=32$：BLEU 25.4 / 60M

> 结论：降低 key 维度损害质量——说明"判定兼容性"（compatibility）不是一件简单的事，**可能需要比点积更复杂的兼容性函数**。

**C. 模型规模**：
- $d_{model}=256, d_{ff}=1024$：BLEU 25.3 / 50M
- $d_{model}=512, d_{ff}=2048$（默认）：BLEU 25.8 / 65M
- $d_{model}=1024, d_{ff}=4096$：BLEU 26.0 / 168M
- $d_{model}=1024, d_{ff}=4096, h=16$：BLEU 26.2 / 168M

> 结论：更大的模型更好，越多参数带来越强性能——这一规律在 GPT 时代被不断验证。

**D. Dropout**：
- $P_{drop}=0.0$：BLEU 24.6（明显过拟合）
- $P_{drop}=0.1$（默认）：BLEU 25.5 ✅
- $P_{drop}=0.2$：BLEU 25.5

> 结论：Dropout 对防止过拟合非常重要，但过多 Dropout 未必更好。

**E. 位置编码**：
- 可学习位置嵌入替代正弦编码：PPL 4.92 / BLEU 25.7（与 base 的 4.92 / 25.8 几乎一致）

> 结论：两种方法效果相当，但正弦版本可能支持外推更长序列。

### 7.3 英语句法成分解析（Table 4）

为验证 Transformer 的泛化能力，论文在 Penn Treebank WSJ 上进行句法解析实验。

| Parser | Setting | WSJ 23 F1 |
|--------|---------|-----------|
| Vinyals & Kaiser et al. (2014) [37] | WSJ only | 88.3 |
| Petrov et al. (2006) [29] | WSJ only | 90.4 |
| Zhu et al. (2013) [40] | WSJ only | 90.4 |
| Dyer et al. (2016) [8] (RNNG) | WSJ only | **91.7** |
| **Transformer (4 layers)** | WSJ only | **91.3** |
| Vinyals & Kaiser et al. (2014) [37] | semi-supervised | 92.1 |
| **Transformer (4 layers)** | semi-supervised | **92.7** |
| Luong et al. (2015) [23] | multi-task | 93.0 |
| Dyer et al. (2016) [8] (RNNG) | generative | **93.3** |

**关键发现**：
- 仅用 WSJ 训练集（4 万句），4 层 Transformer 就超越了 Berkeley Parser（尽管缺乏任务特定调优）
- 对比 RNN seq2seq 模型 [37] 在少数据场景下无法达到 SOTA，但 Transformer 做到了
- 半监督设置下达到 92.7 F1，超过此前所有方法
- 这强烈表明 Transformer 不仅是翻译专用架构，**其核心机制具有通用性**

---

## 八、注意力可视化——可解释性分析

> 论文附录展示了编码器自注意力的可视化，直观说明"模型学到了什么"。

### 8.1 长距离依赖（Figure 3）

在"making ... more difficult"这个短语中（中间插入了大量修饰成分），第 5 层编码器的多个注意力头准确地将 "making" 与远处的内容关联起来。不同颜色代表不同的注意力头。**这证明了自注意力 $O(1)$ 路径长度的优势——无论两个词相距多远，注意力都可以直接连接。**

### 8.2 指代消解（Figure 4 & 5）

在处理句子 "The Law will never be perfect, but its application should be just..." 时：
- 注意力头 5 和 6 在 "its" 处给出了非常锐利的注意力分布（attention is very sharp）
- Head 5 聚焦于整个句子的多个部分，Head 6 则精准指向 "The Law"
- 这展示了**不同头学会了执行不同的语言任务**——有的负责指代消解，有的负责句法结构

### 8.3 结构化行为（Figure 5）

许多注意力头表现出与句子结构（句法/语义）相关的行为模式。论文给出两个来自不同头的示例，证明不同头确实学会了执行不同类型的分析任务。这为 Transformer 的强泛化能力提供了一个直观的解释：**多头机制自然地解耦了不同的语言特征。**


## §流程

### encode

输入：$X = (x_1, x_2, ..., x_n), \quad x_i \in \mathbb{R}^{d_{model}}, \quad d_{model}=512$
位置参数：$PE$
最终输入：$H = X \cdot \sqrt{d_{model}} + PE$
多头注意力：
1. 构建 $Q,K,V$ ：$Q=HW^Q,K=HW^K,V=HW^V$
2. 拆分成多头：$Q=\{Q_1,\cdots,Q_h\},K=\{K_1,\cdots,K_h\},V=\{V_1,\cdots,V_h\}$
3. 每个头的缩放点积注意力：
    $\text{head}_i = \text{Attention}(Q_i, K_i, V_i) = \text{softmax}\left(\frac{Q_iK_i^T}{\sqrt{d_k}}\right) V_i$
4. 拼接与输出投影：$\text{AttnOut}=\text{MultiHead}(Q, K, V) = \text{Concat}(\text{head}_1, ..., \text{head}_h) W^O$
5. 残差连接和层归一化：$H_1​=\text{LayerNorm}(H+\text{Attn\_Out})$
	- $\text{LayerNorm}(x) = \gamma \odot \frac{x - \mu}{\sqrt{\sigma^2 + \epsilon}} + \beta$
	- $\mu = \frac{1}{d_{\text{model}}} \sum_{j=1}^{d_{\text{model}}} x_j$
	- $\sigma^2 = \frac{1}{d_{\text{model}}} \sum_{j=1}^{d_{\text{model}}} (x_j - \mu)^2$
	- $\gamma, \beta \in \mathbb{R}^{d_{\text{model}}}$ 是可学习的缩放和偏移参数
	- $\epsilon$ 是防止除零的小常数
6. 逐位置前馈网络（FFN）
	- 对 $H_1$中的每个 token 独立应用相同的两层全连接网络（含 ReLU 激活）：$\text{FFN}(x) = \max(0, x W_1 + b_1) W_2 + b_2, \quad \forall x \in H_1$
	-  $W_1 \in \mathbb{R}^{512 \times 2048} ， b_1 \in \mathbb{R}^{2048}$ 
	-  $W_2 \in \mathbb{R}^{2048 \times 512} ， b_2 \in \mathbb{R}^{512}$ 
	- 记 FFN 的输出为  $\text{FFN\_Out} \in \mathbb{R}^{n \times 512}$
7. 第二个残差连接 + 层归一化（Add & Norm）
	1. $H_{\text{out}} = \text{LayerNorm}\big(H_1 + \text{FFN\_Out}\big)$

### decode

#### 带掩码的多头自注意力

生成 Q、K、V
$$\begin{aligned}
Q_{\text{self}} &= Y W^Q_{\text{self}}, \quad W^Q_{\text{self}} \in \mathbb{R}^{512 \times 512} \\
K_{\text{self}} &= Y W^K_{\text{self}}, \quad W^K_{\text{self}} \in \mathbb{R}^{512 \times 512} \\
V_{\text{self}} &= Y W^V_{\text{self}}, \quad W^V_{\text{self}} \in \mathbb{R}^{512 \times 512}
\end{aligned}$$


拆分为多头
$Q_{\text{self}} \rightarrow \{Q_1, ..., Q_h\}, \quad Q_i \in \mathbb{R}^{m \times 64}, \quad \text{对 } K, V \text{同理}$


带因果掩码的缩放点积注意力

对第  $i$  个头，计算注意力：
$\text{head}_i^{\text{self}} = \text{Attention}(Q_i, K_i, V_i) = \text{softmax}\left(\frac{Q_i K_i^T}{\sqrt{d_k}} + M\right) V_i$
$M_{ij} = \begin{cases}0, & i \geq j \quad (\text{允许看到当前位置及之前}) \\-\infty, & i < j \quad (\text{屏蔽未来位置})\end{cases}$

拼接 + 输出投影
$\text{Attn\_Self\_Out} = \text{Concat}(\text{head}_1^{\text{self}}, ..., \text{head}_h^{\text{self}}) W^O_{\text{self}}$

第一个残差连接 + 层归一化
$Y_1 = \text{LayerNorm}\big(Y + \text{Attn\_Self\_Out}\big), \quad Y_1 \in \mathbb{R}^{m \times 512}$


#### 交叉注意力

这是编码器与解码器之间的**唯一桥梁**。**关键区别**： $Q$ 来自解码器 $Y_1$ ，$K, V$ 来自编码器 $H_{enc}$ 。

生成 Q（来自解码器）、K 和 V（来自编码器）
$$\begin{aligned}
Q_{\text{cross}} &= Y_1 W^Q_{\text{cross}}, \quad W^Q_{\text{cross}} \in \mathbb{R}^{512 \times 512} \\
K_{\text{cross}} &= H_{enc} W^K_{\text{cross}}, \quad W^K_{\text{cross}} \in \mathbb{R}^{512 \times 512} \\
V_{\text{cross}} &= H_{enc} W^V_{\text{cross}}, \quad W^V_{\text{cross}} \in \mathbb{R}^{512 \times 512}
\end{aligned}$$


> **注意**：这里的  $W^Q_{\text{cross}}, W^K_{\text{cross}}, W^V_{\text{cross}}$  与阶段一的  $W^Q_{\text{self}}$  等**不是同一个矩阵**，它们是独立可学习的参数。

拆分为多头，标准缩放点积注意力（不加掩码）
对第  i  个头：
$$\text{head}_i^{\text{cross}} = \text{Attention}(Q_i, K_i, V_i) = \text{softmax}\left(\frac{Q_i K_i^T}{\sqrt{d_k}}\right) V_i$$
这里的  $Q_i K_i^T \in \mathbb{R}^{m \times n}$ ，第  j  行第  k  列表示目标序列第  j  个位置对源序列第  k  个位置的关注权重。
拼接 + 输出投影
$$\text{Attn\_Cross\_Out} = \text{Concat}(\text{head}_1^{\text{cross}}, ..., \text{head}_h^{\text{cross}}) W^O_{\text{cross}}$$
第二个残差连接 + 层归一化
$Y_2 = \text{LayerNorm}\big(Y_1 + \text{Attn\_Cross\_Out}\big), \quad Y_2 \in \mathbb{R}^{m \times 512}$

#### 逐位置前馈网络（FFN）
与编码器的 FFN 完全相同，独立作用于每个 token：
$\text{FFN}(x) = \max(0, x W_1 + b_1) W_2 + b_2, \quad \forall x \in Y_2$
记输出为  $\text{FFN\_Out} \in \mathbb{R}^{m \times 512}$ 。

#### 第三个残差连接 + 层归一化
$Y_{\text{out}} = \text{LayerNorm}\big(Y_2 + \text{FFN\_Out}\big), \quad Y_{\text{out}} \in \mathbb{R}^{m \times 512}$
至此，完成一个完整的 Transformer 解码器层。 $Y_{\text{out}}$  将作为下一解码器层的输入，堆叠  $N = 6$  层后，送入最终的线性层 + Softmax 生成目标词汇的概率分布。
