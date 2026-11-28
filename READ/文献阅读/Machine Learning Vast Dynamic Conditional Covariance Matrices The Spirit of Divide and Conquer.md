
论文作者： Zhoufan Zhu、Ke Zhu  
核心模型： GRAND（Graph-based Quantiled Conditional Covariance Matrix Estimator）  
核心网络： SGA（Self-grouping Autoencoder）  
核心统计方法： QCM（Quantiled Conditional Moments）

这篇文章值得认真研究的地方，不只是它使用了深度学习，而是它试图解决一个非常典型的高维金融统计问题：

> 当资产数量 $N$ 很大、时间序列长度 $T$ 相对有限时，如何利用资产特征和资产之间的动态关系，估计一个随时间变化的高维条件协方差矩阵？

我会按照长期学习笔记的方式组织内容：先建立统计问题，再解释模型结构，然后逐层拆解需要估计的参数、损失函数、梯度下降过程，最后讨论模型的统计意义、计算复杂度和潜在局限。

有一个概念需要先区分：

- GRAND 是完整的协方差矩阵估计框架。
- SGA 是其中负责学习动态关系和条件分位数的神经网络。
- QCM 是将条件分位数转化为条件方差、条件协方差的统计方法。

因此，不能简单地把这篇文章理解成一个输入股票特征、输出协方差矩阵的神经网络。它实际上将神经网络、分位数回归、矩估计和半正定矩阵投影组合在了一起。

# 一、研究问题：究竟要估计什么？

## 1.1 条件协方差矩阵

假设我们有 $N$ 个资产，在时刻 $t$ 的收益率向量为：

$$ \mathbf r_t= \begin{pmatrix} r_{1,t}\\ r_{2,t}\\ \vdots\\ r_{N,t} \end{pmatrix} \in \mathbb R^N. $$

令 $\mathcal F_{t-1}$ 表示截至 $t-1$ 时刻所有可获得的信息。

本文的核心预测目标是：

$$ \boxed{ \Sigma_t = \operatorname{Cov} (\mathbf r_t\mid\mathcal F_{t-1}) } $$

其中：

$$ \Sigma_t= \begin{pmatrix} \sigma_{1,t}^2 & \sigma_{12,t} & \cdots & \sigma_{1N,t}\\ \sigma_{21,t} & \sigma_{2,t}^2 & \cdots & \sigma_{2N,t}\\ \vdots & \vdots & \ddots & \vdots\\ \sigma_{N1,t} & \sigma_{N2,t} & \cdots & \sigma_{N,t}^2 \end{pmatrix}. $$

对角线元素：

$$ \sigma_{i,t}^2 = \operatorname{Var}(r_{i,t}\mid\mathcal F_{t-1}) $$

表示资产 $i$ 的条件方差。

非对角线元素：

$$ \sigma_{ij,t} = \operatorname{Cov}(r_{i,t},r_{j,t}\mid\mathcal F_{t-1}) $$

表示资产 $i,j$ 之间的条件协方差。

这里的关键词是 Conditional 和 Dynamic。

- Conditional：我们不是计算无条件历史协方差，而是利用过去的信息预测未来的风险结构。
- Dynamic：协方差矩阵不是固定的，市场环境变化时，资产间的风险关系也会变化。

例如，在正常市场中，两个行业的收益率可能相关性较低；在系统性风险事件中，它们可能同时下跌，条件协方差明显上升。

## 1.2 为什么这个问题具有维度灾难？

一个 $N\times N$ 的对称协方差矩阵有：

$$ \boxed{ \frac{N(N+1)}{2} } $$

个独立元素。

假设 $N=1950$，那么需要估计：

$$ \frac{1950\times1951}{2} = 1,903,725 $$

个独立元素。

而本文的实证数据只有 $T=645$ 周。

这意味着：如果对每个时点都直接估计整个协方差矩阵，参数数量与可用时间序列长度严重不匹配。

本文的基本策略是：

不直接学习整个矩阵，而是先学习每一个资产的条件方差，再学习资产两两之间的条件协方差，最后将这些元素组合起来。

这就是标题中 Divide and Conquer 的含义。

## 1.3 与传统方法的区别

|方法|核心假设或处理方式|主要局限|
|---|---|---|
|样本协方差|使用历史样本估计|难以刻画动态关系|
|Factor Model|假设收益率由少数公共因子驱动|依赖因子结构是否合理|
|Shrinkage|将样本协方差向结构化目标收缩|对动态结构的刻画有限|
|DCC-NL|使用简约动态相关结构|动态更新参数较少|
|GRAND|分位数学习 + 图结构 + 逐元素矩估计|训练和计算流程更加复杂|

这里尤其需要注意：GRAND 并不是简单地用神经网络替换 GARCH。它改变的是整个协方差矩阵的估计思路。


# 二、GRAND 的整体结构：把一个大问题拆成六个任务



## GRAND 模型计算流程

输入数据  
$\mathbf X_{t-1}\in\mathbb R^{N\times P\times S}$

N 个资产 × P 个特征 × S 个历史时点

任务一：学习个体条件分位数

SGA（GRU Encoder + Graph + Decoder） $\widehat Q_{i,t}(\tau)$

任务二：QCM 估计对角线

对不同分位数做 OLS 回归 $\widehat\sigma_{i,t}^{\,2}$

任务三：学习资产两两组合的条件分位数

Node Fusion + 预训练 SGA $\widehat Q_{i,j,t}(\tau)$

任务四：QCM 估计两两组合方差

$\widehat\delta_{i,j,t}^{\,2}$

进而得到：

$$ \widehat\sigma_{ij,t} = \frac{\widehat\delta_{i,j,t}^{\,2} -\widehat\sigma_{i,t}^{\,2} -\widehat\sigma_{j,t}^{\,2}}{2} $$

任务五：学习条件方差的因果图

另一个 SGA + MSE

判断哪些资产对需要计算协方差

任务六：组装与半正定投影

$$ \widehat\Sigma_t \longrightarrow \widetilde\Sigma_t\succeq0 $$

得到最终的动态条件协方差矩阵

这里有一个容易混淆的地方：

任务一和任务三的 SGA 使用分位数损失；任务五的 SGA 使用均方误差损失。QCM 使用 OLS；最后的矩阵投影使用数值优化算法。

它们不是同一个训练目标，也不是一次梯度下降就完成全部模型。

下面逐层展开。

# 三、核心神经网络：Self-grouping Autoencoder（SGA）

这是整篇论文最重要的机器学习部分。

## 3.1 输入数据的数学表达

对于资产 $i$，假设每个时点有 $P$ 个特征：

$$ \mathbf x_{i,t}\in\mathbb R^P. $$

例如：
- 收益率、成交量、成交额；
- 开盘价、最高价、最低价、收盘价；
- 市值、ROE、营业收入增长率；
- 宏观经济指标；
- 行业虚拟变量。

模型不只使用 $t-1$ 时刻的特征，而是使用过去连续 $S$ 个时点的特征：

$$ \mathbf X_{i,t-1} = \left[ \mathbf x_{i,t-S}, \mathbf x_{i,t-S+1}, \ldots, \mathbf x_{i,t-1} \right] \in\mathbb R^{P\times S}. $$

将所有资产的数据合在一起：

$$ \boxed{ \mathbf X_{t-1} = [\mathbf X_{1,t-1},\ldots,\mathbf X_{N,t-1}] \in\mathbb R^{N\times P\times S} } $$

这是整个 SGA 的输入。

例如，假设：

- $N=1000$：1000 只股票；
- $P=31$：每只股票有31个特征；
- $S=48$：每个特征使用过去48周的数据。

那么模型输入就是：

$$ 1000\times31\times48. $$

注意，输入特征是多维时间序列，不是简单地把过去48周的收益率拼接成一个向量。

## 3.2 Encoder：首先使用 GRU 提取时间信息

SGA 的 Encoder 第一部分是一个两层 GRU 网络。

它的作用是：

将每个资产过去 $S$ 个时点的多维特征，压缩成一个 $D$ 维的时间特征向量。

即：

$$ \boxed{ \mathbf h_{i,t} = \operatorname{GRU}(\mathbf X_{i,t-1};\phi) \in\mathbb R^D } $$

这里：

- $\mathbf X_{i,t-1}$：输入特征；
- $\mathbf h_{i,t}$：输出的隐藏状态；
- $D$：GRU 隐藏层维度；
- $\phi$：GRU 中所有可训练参数。

先理解 GRU 本身。

### （1）GRU 的基本机制

GRU 的核心是两个门：

1. Update Gate：更新门，决定保留多少历史信息。
2. Reset Gate：重置门，决定在生成新的候选状态时使用多少历史信息。

为了便于理解，下面使用常见的 GRU 记号。论文中分别使用 $z,i,o$ 表示更新门、重置门和候选状态。

对于第 $s$ 个时点：

更新门：

$$ \mathbf z^s = \operatorname{sigmoid} \left( W_z\mathbf x^s+ U_z\mathbf h^{s-1}+ \mathbf b_z \right) $$

重置门：

$$ \mathbf r^s = \operatorname{sigmoid} \left( W_r\mathbf x^s+ U_r\mathbf h^{s-1}+ \mathbf b_r \right) $$

候选隐藏状态：

$$ \widetilde{\mathbf h}^{s} = \tanh \left( W_h\mathbf x^s+ U_h(\mathbf r^s\odot\mathbf h^{s-1}) +\mathbf b_h \right) $$

最终隐藏状态：

$$ \boxed{ \mathbf h^s = \mathbf z^s\odot\mathbf h^{s-1} + (1-\mathbf z^s)\odot \widetilde{\mathbf h}^{s} } $$

其中：

- $\operatorname{sigmoid}$ 将数值映射到 $(0,1)$；
- $\tanh$ 将数值映射到 $(-1,1)$；
- $\odot$ 表示逐元素乘法；
- $W,U,b$ 都是需要训练的参数。

### （2）为什么要有两个门？

可以从极端情况理解。

当：

$$ \mathbf z^s\approx 1 $$

那么：

$$ \mathbf h^s\approx\mathbf h^{s-1}. $$

模型倾向于保留过去的信息。

当：

$$ \mathbf z^s\approx 0 $$

那么：

$$ \mathbf h^s\approx\widetilde{\mathbf h}^s. $$

模型倾向于使用当前输入产生的新信息。

而重置门 $\mathbf r^s$ 决定生成候选状态时，历史隐藏状态有多少参与计算。

所以 GRU 的优势在于：它能够根据输入数据自动学习不同特征的时间依赖结构，而不是人为规定每个历史时点的权重。

这也是它相对于简单线性滞后模型的一个重要优势。GRU 的门控机制来自 Cho et al.（2014）的 RNN Encoder–Decoder 工作。

ACL Anthology+1

### （3）论文中的两层 GRU

本文并非只使用一层 GRU，而是采用两层结构。

第一层：

$$ \mathbf h_{i,t,1}^{s} = \operatorname{GRU}_1 \left( \mathbf x_{i,t-S+s}, \mathbf h_{i,t,1}^{s-1} \right) $$

第二层：

$$ \mathbf h_{i,t,2}^{s} = \operatorname{GRU}_2 \left( \mathbf h_{i,t,1}^{s}, \mathbf h_{i,t,2}^{s-1} \right) $$

最终取：

$$ \boxed{ \mathbf h_{i,t} = \mathbf h_{i,t,2}^{S} } $$

因此，GRU Encoder 需要估计的参数包括：

|参数|含义|维度|
|---|---|---|
|$W_{1z},W_{1r},W_{1h}$|第一层输入权重|$D\times P$|
|$U_{1z},U_{1r},U_{1h}$|第一层循环权重|$D\times D$|
|$W_{2z},W_{2r},W_{2h}$|第二层输入权重|$D\times D$|
|$U_{2z},U_{2r},U_{2h}$|第二层循环权重|$D\times D$|
|$b$|各门的偏置项|$D$|

每一层的三组权重分别对应更新门、重置门和候选状态。

论文将这些参数统一记作：

$$ \phi. $$

特别注意： 这部分参数是所有资产共享的，不是每只股票单独训练一套 GRU。这样才能在横截面上共享信息。

## 3.3 Encoder 的第二部分：Self-grouping 图结构学习

现在我们已经得到了每只股票的时间特征：

$$ \mathbf h_{1,t},\ldots,\mathbf h_{N,t}. $$

接下来，模型需要解决一个问题：

> 哪些股票在当前时刻具有相似的预测结构？哪些股票应该被归入同一个关系组？

传统方法可能直接使用行业分类、产业链关系、概念板块等人工构造的关系。

本文不采用这种固定分组，而是让网络根据 GRU 提取的特征自动分组。

### （1）计算余弦相似度

将每只股票都作为一个中心节点。

对于股票 $i$ 和中心节点 $k$，计算：

$$ \boxed{ \gamma_{i,k,t} = \frac{ \mathbf h_{i,t}^{\top}\mathbf h_{k,t} }{ \|\mathbf h_{i,t}\|_2 \|\mathbf h_{k,t}\|_2 } } $$

这就是 Cosine Similarity。

它衡量的是两个隐藏特征向量的方向相似程度。

- 接近 $1$：特征方向高度相似；
- 接近 $0$：特征方向相对不相关；
- 接近 $-1$：特征方向相反。

这里需要注意：相似的是网络提取的潜在特征，不是股票收益率本身。

### （2）Top-K 筛选

如果直接使用所有相似度，很多微弱的相似关系可能只是噪声。

因此，作者引入超参数 $K^*$。

对每个股票 $i$，只保留其最相似的 $K^*$ 个中心节点。

论文的处理为：

$$ \gamma_{i,k,t}^{*} = \begin{cases} 1,&k=i,\\ \gamma_{i,k,t}, &|\gamma_{i,k,t}|> \operatorname{Top}(A_i,K^*),\\ 0,&\text{otherwise}. \end{cases} $$

其中：

$$ A_i= \left\{ |\gamma_{i,k,t}|:k\ne i \right\}. $$

$\operatorname{Top}(A_i,K^*)$ 表示其中第 $K^*$ 大的值。

这里把自身的连接设置为1，是为了保证每个中心节点属于自己的组。

### （3）形成关系组

对于中心节点 $k$，定义：

$$ \boxed{ M_{k,t} = \{i:\gamma_{i,k,t}^{*}\ne0\} } $$

这就是关系组。

举一个简化例子：

假设我们有五只股票：

$$ A,B,C,D,E. $$

模型经过相似度计算后，可能得到：

$$ M_{1,t}=\{A,B,C\} $$

$$ M_{2,t}=\{B,C,D\} $$

$$ M_{3,t}=\{D,E\}. $$

这里有一个很重要的性质：

不同的组可以重叠。

例如 B、C 同时属于第一组和第二组。

这意味着模型允许一只股票同时受到多种关系结构的影响。

### （4）形成邻接矩阵

每一个组对应一个邻接矩阵：

$$ A_{k,t}\in\mathbb R^{N\times N} $$

其中：

$$ A_{k,t}^{(i,j)} = \begin{cases} 1,&i,j\in M_{k,t},\ i\ne j,\\ 0,&\text{otherwise}. \end{cases} $$

最终得到：

$$ G_t=(V,A_t), $$

其中 $V$ 是所有资产组成的节点集合，$A_t$ 是不同关系组对应的邻接矩阵集合。

这就是 SGA 的 Encoder 输出。

上图仅为概念示意。论文原始 SGA 网络结构见第47页 Fig. A1：从 GRU、相似度计算、阈值筛选到关系组，再进入 Decoder。

论文第47页的 Fig. A1 非常值得反复看。它把整个网络分成了两个部分：

- Encoder：GRU → Similarities → Thresholding → Groups → Graph。
- Decoder：Group Feature → Attention → Residual Learning → Prediction。

MachineLearningVastDynamicConditionalCovarianceMatrices.pdf

## 3.4 Decoder：如何利用图结构预测目标变量？

现在我们有了：

1. 每只股票的时间特征 $\mathbf h_{i,t}$；
2. 不同关系组 $M_{k,t}$；
3. 关系图 $G_t$。

Decoder 的任务是利用这些信息，预测每只股票的目标变量 $y_{i,t}$。

本文的 Decoder 由两个模块构成：

- Group Module：提取群体共同影响；
- Individual Module：提取股票自身的个体特征。

这是 SGA 结构设计中另一个很重要的地方。

### （1）Group Module：构建组特征

首先，对每一个组 $M_{k,t}$，聚合其中所有股票的时间特征：

$$ \boxed{ \mathbf H_{k,t} = \operatorname{LeakyReLU} \left[ W_1 \left( \sum_{i\in M_{k,t}} \gamma_{i,k,t}^{*}\mathbf h_{i,t} \right) +\mathbf b_1 \right] } $$

其中：

- $\mathbf H_{k,t}\in\mathbb R^D$：第 $k$ 个关系组的特征；
- $W_1\in\mathbb R^{D\times D}$：权重矩阵；
- $\mathbf b_1\in\mathbb R^D$：偏置项。

直观理解：

一只股票的行为不仅由自身历史决定，也可能由其所在的关系组共同决定。

例如，某只股票可能同时属于新能源组和高成长组。

两个组都可能对它的未来风险产生影响。

### （2）Attention：不同关系组的重要程度不同

对于股票 $i$，找到它所属于的所有关系组：

$$ \mathcal N_{i,t} = \{k:i\in M_{k,t}\}. $$

然后计算股票与组之间的相似度：

$$ \alpha_{i,k,t} = \operatorname{Cosine} (\mathbf h_{i,t},\mathbf H_{k,t}). $$

通过 Softmax 得到 Attention 权重：

$$ \boxed{ \beta_{i,k,t} = \frac{ \exp(\alpha_{i,k,t}) }{ \sum_{k'\in\mathcal N_{i,t}} \exp(\alpha_{i,k',t}) } } $$

于是，股票 $i$ 的整体群体影响为：

$$ \boxed{ \mathbf g_{i,t} = \sum_{k\in\mathcal N_{i,t}} \beta_{i,k,t}\mathbf H_{k,t} } $$

这里的 Attention 有明确的经济含义：

不是每个关系组都同等重要，模型需要根据当前股票的特征，自动学习不同群体影响的相对权重。

例如：

$$ \beta_{i,1,t}=0.8,\qquad \beta_{i,2,t}=0.2. $$

说明当前时刻第一个关系组对股票 $i$ 的预测更重要。

### （3）Group Module 的双头输出

先进行一次特征变换：

$$ \widetilde{\mathbf g}_{i,t} = \operatorname{LeakyReLU} (W_2\mathbf g_{i,t}+\mathbf b_2). $$

然后分成两个输出：

$$ \boxed{ \mathbf h_{i,t}^{G} = \operatorname{LeakyReLU} (W_3\widetilde{\mathbf g}_{i,t}+\mathbf b_3) } $$

$$ \boxed{ \mathbf o_{i,t}^{G} = \operatorname{LeakyReLU} (W_4\widetilde{\mathbf g}_{i,t}+\mathbf b_4) } $$

两个输出分别承担不同任务：

- $\mathbf o_{i,t}^{G}$：群体信息对最终预测的贡献；
- $\mathbf h_{i,t}^{G}$：用于识别原始个体特征中已经被群体信息解释的部分。

### （4）Individual Module：提取个体剩余信息

这部分的设计很有意思。

作者先从原始个体特征中减去群体解释的部分：

$$ \boxed{ \widetilde{\mathbf h}_{i,t} = \mathbf h_{i,t} - \mathbf h_{i,t}^{G} } $$

再通过一个全连接网络：

$$ \boxed{ \mathbf o_{i,t}^{I} = \operatorname{LeakyReLU} (W_5\widetilde{\mathbf h}_{i,t}+\mathbf b_5) } $$

其中：

$$ \mathbf o_{i,t}^{I} $$

代表个体自身无法由群体关系解释的异质性信息。

因此，模型在预测时同时考虑：

- 群体共同影响；    
- 个体特有影响。

这其实是一个非常重要的建模思想：将共同结构和个体异质性分开学习。

### （5）最终预测

最后将两个模块的输出相加：

$$ \boxed{ \widehat y_{i,t} = W_6 \left( \mathbf o_{i,t}^{G} + \mathbf o_{i,t}^{I} \right)+b_6 } $$

因此，完整的 SGA 可以表示为：

$$ \boxed{ \widehat{\mathbf y}_t = f(\mathbf X_{t-1};G_t,\theta), \qquad G_t=q(\mathbf X_{t-1};\phi) } $$

其中：

$$ \theta=\{W_1,\ldots,W_6,\mathbf b_1,\ldots,\mathbf b_6\}. $$

注意 $W_6$ 是一个 $D$ 维向量，$b_6$ 是标量。

因此，SGA 实际上要学习两大类神经网络参数：

|参数|所属模块|作用|
|---|---|---|
|$\phi$|GRU Encoder|学习时间特征|
|$\theta$|Decoder|学习群体影响、Attention、个体异质性及最终预测|

这里的 Decoder 并不是传统意义上将输入重新还原的 Autoencoder Decoder。它的任务是利用 Encoder 学到的结构进行监督预测。

MachineLearningVastDynamicConditionalCovarianceMatrices.pdf

# 四、如何训练 SGA？——损失函数与梯度下降

这一部分是理解模型的核心。

前面我们介绍了网络结构，现在需要回答：

网络中的参数究竟如何确定？

答案是：对于条件分位数模型，使用 Quantile Loss，并通过 Adam 算法优化。

## 4.1 为什么不用 MSE？

假设我们要预测：

$$ Y_{i,t}=r_{i,t}. $$

传统神经网络通常使用：

$$ L_{\mathrm{MSE}} = \sum_{t,i} (r_{i,t}-\widehat r_{i,t})^2. $$

这相当于主要学习条件均值：

$$ E(r_{i,t}\mid\mathcal F_{t-1}). $$

但是，本文并不首先关心条件均值，而是希望学习整个条件分布的不同位置。

例如：

- 5% 分位数：极端下行情形；
- 50% 分位数：条件中位数；
- 95% 分位数：极端上行情形。

因此，需要使用分位数回归。

## 4.2 Quantile Loss 的数学表达

对于给定分位数水平：
$$ \tau\in(0,1), $$
定义预测值：

$$ \widehat Q_{i,t}(\tau) = f_i(\mathbf X_{t-1}; q(\mathbf X_{t-1};\phi_\tau), \theta_\tau). $$

这里需要特别注意：

$$ \phi_\tau,\theta_\tau $$

都是与分位数水平 $\tau$ 相关的参数。

也就是说，5% 分位数模型和95%分位数模型不是同一个参数组。

定义残差：

$$ u_{i,t} = r_{i,t}-\widehat Q_{i,t}(\tau). $$

分位数损失函数：

$$ \boxed{ \rho_\tau(u) = u\left[\tau-I(u<0)\right] } $$

展开：

$$ \rho_\tau(u) = \begin{cases} \tau u,&u\geq0,\\ (\tau-1)u,&u<0. \end{cases} $$

这个函数也称为 Pinball Loss 或 Check Loss。

### 用一个简单例子理解

假设我们要预测5%分位数：

$$ \tau=0.05. $$

此时：

$$ \rho_{0.05}(u) = \begin{cases} 0.05u,&u\geq0,\\ -0.95u,&u<0. \end{cases} $$

如果：

$$ r_{i,t}>\widehat Q_{i,t}(0.05), $$

说明实际收益率高于预测分位数。

损失增加的速度是：

$$ 0.05. $$

如果：

$$ r_{i,t}<\widehat Q_{i,t}(0.05), $$

说明实际收益率落在预测的5%分位数以下。

损失增加的速度是：

$$ 0.95. $$

因此，模型会对预测分位数以下的观测给予更大的惩罚，推动预测结果达到5%分位数的统计要求。

更严格地说，条件分位数是期望 Pinball Loss 的最优解：

$$ Q_Y(\tau\mid X) = \arg\min_q E[\rho_\tau(Y-q)\mid X]. $$

这也是分位数回归的统计基础。

American Economic Association+1

## 4.3 本文的优化目标函数

对于给定的 $\tau$，本文的目标函数是：

$$ \boxed{ \begin{aligned} (\widehat\phi_\tau,\widehat\theta_\tau) = \arg\min_{\phi_\tau,\theta_\tau} \sum_{t\in\mathcal T} \sum_{i=1}^{N} \rho_\tau \left[ r_{i,t} - f_i( \mathbf X_{t-1}; q(\mathbf X_{t-1};\phi_\tau), \theta_\tau ) \right]. \end{aligned} } $$

其中：

- $\mathcal T$：训练时间集合；
- $N$：资产数量；
- $\phi_\tau$：Encoder 参数；    
- $\theta_\tau$：Decoder 参数。

这意味着我们要找到一组参数，使得整个训练样本上的预测分位数尽可能准确。

需要强调的是：

$$ \boxed{ \text{Encoder 和 Decoder 是联合训练的。} } $$

也就是说，不能先把 GRU 训练好就完全固定，然后再单独训练 Decoder。

在第一个分位数学习任务中，两部分参数共同参与优化。

## 4.4 梯度究竟怎么计算？

假设某一次迭代得到：

$$ \widehat Q_{i,t}(\tau). $$

首先计算：

$$ u_{i,t}=r_{i,t}-\widehat Q_{i,t}(\tau). $$

对于预测分位数的梯度：

$$ \frac{\partial\rho_\tau(u)} {\partial\widehat Q} = \begin{cases} -\tau,&u>0,\\ 1-\tau,&u<0. \end{cases} $$

在 $u=0$ 处，使用次梯度。

然后通过链式法则：

$$ \frac{\partial L}{\partial\theta_\tau} = \sum_{t,i} \frac{\partial L}{\partial\widehat Q_{i,t}} \frac{\partial\widehat Q_{i,t}} {\partial\theta_\tau}. $$

同理：

$$ \frac{\partial L}{\partial\phi_\tau} = \sum_{t,i} \frac{\partial L}{\partial\widehat Q_{i,t}} \frac{\partial\widehat Q_{i,t}} {\partial\phi_\tau}. $$

这就是反向传播。

你可以把它理解为：

1. 输入历史特征；
2. GRU 得到隐藏状态；
3. 计算资产相似度；
4. 形成关系组；
5. Decoder 输出预测分位数；
6. 计算预测误差；
7. 将误差反向传播到网络参数；
8. 更新参数。

有一个技术细节值得注意： SGA 的 Top-$K$ 筛选和组成员判定涉及离散操作。反向传播可以通过当前被选中的相似度数值和隐藏状态进行，但组成员的离散变化本身不是普通的光滑梯度。因此，这个网络实际上是具有分段可微结构的模型；组成员关系在排序发生变化时可能跳变。论文没有另外介绍连续松弛或 Straight-Through Estimator，不能假定它使用了这些技巧。

## 4.5 为什么使用 Adam，而不是普通梯度下降？

论文采用的是 Adam（Adaptive Moment Estimation）。

Adam 是一种基于梯度一阶矩和二阶原始矩的自适应随机优化算法。

papers.baulab.info

假设当前网络所有参数合并成：

$$ \vartheta=(\phi_\tau,\theta_\tau). $$

第 $l$ 次迭代得到的梯度为：

$$ g_l=\nabla_\vartheta L_l(\vartheta_l). $$

Adam 分为以下几个步骤。

第一步：计算梯度的一阶矩。

$$ \boxed{ m_l = \beta_1m_{l-1} + (1-\beta_1)g_l } $$

它是梯度的指数加权移动平均，可以理解为带有历史信息的梯度方向。

第二步：计算梯度的二阶原始矩。

$$ \boxed{ v_l = \beta_2v_{l-1} + (1-\beta_2)g_l^2 } $$

这里的平方是逐元素平方。

它反映了每个参数对应的梯度变化尺度。

第三步：偏差修正。

因为通常初始化：

$$ m_0=v_0=0, $$

前期估计会受到零初始化的影响。

因此需要：

$$ \widehat m_l = \frac{m_l}{1-\beta_1^l} $$

$$ \widehat v_l = \frac{v_l}{1-\beta_2^l}. $$

第四步：更新参数。

$$ \boxed{ \vartheta_{l+1} = \vartheta_l - \gamma \frac{\widehat m_l} {\sqrt{\widehat v_l}+\epsilon} } $$

其中：

- $\gamma$：学习率；
- $\beta_1$：一阶矩衰减系数；
- $\beta_2$：二阶矩衰减系数；
- $\epsilon$：防止除零的小常数。

这个更新过程与普通梯度下降相比，最重要的区别是：
每个参数拥有自适应的有效学习步长。
梯度变化大的参数会受到相对更强的尺度调整，梯度变化小的参数则可以保留适当的更新幅度。
原始 Adam 论文给出的常见默认值是：

$$ \beta_1=0.9,\quad \beta_2=0.999,\quad \epsilon=10^{-8}. $$

但需要注意，本文明确给出了学习率候选值，并没有在正文中明确列出实际采用的 $\beta_1,\beta_2,\epsilon$。不能把原始 Adam 的默认值直接当成本文已经确认的实验设置。

## 4.6 本文实际如何执行梯度下降？

根据论文 Appendix E 的 Algorithm E1：
1. 初始化 $\phi_\tau,\theta_\tau$；
2. 从训练样本中随机抽取一个时间点对应的横截面数据；
3. 计算该批次的 Quantile Loss；
4. 对 $\phi_\tau,\theta_\tau$ 计算梯度；
5. 使用 Adam 更新参数；
6. 重复，直到参数收敛。

这里的一个细节是：论文将其描述为 cross-sectional minibatch。也就是说，抽取一个时间点后，利用该横截面上的资产数据构造损失，并非简单地把每只股票当作完全独立的单条训练样本。

这使得网络能够在每次训练时观察不同股票之间的关系。


# 五、QCM：如何把分位数转化为条件方差？

这是整篇文章最重要的统计学创新之一。

前面我们已经知道，神经网络输出的是：
$$ \widehat Q_{i,t}(\tau). $$
但我们最终想要的是：
$$ \widehat\sigma_{i,t}^{\,2}. $$
怎么从一组分位数估计方差？
作者使用 Quantiled Conditional Moments（QCM）方法。

## 5.1 背景：Cornish-Fisher Expansion

先引入几个统计量。假设资产 $i$ 在时刻 $t$ 的条件分布具有：
- 条件均值：$\mu_{i,t}$；
- 条件标准差：$\sigma_{i,t}$；
- 条件偏度：$s_{i,t}$；
- 条件峰度：$k_{i,t}$。

这里注意：
$$ \sigma_{i,t}^2 = \operatorname{Var}(r_{i,t}\mid\mathcal F_{t-1}). $$
偏度定义：
$$ s_{i,t} = \frac{ E[(r_{i,t}-\mu_{i,t})^3\mid\mathcal F_{t-1}] }{ \sigma_{i,t}^3 }. $$
峰度定义：
$$ k_{i,t} = \frac{ E[(r_{i,t}-\mu_{i,t})^4\mid\mathcal F_{t-1}] }{ \sigma_{i,t}^4 }. $$

正态分布的峰度为3。

Cornish-Fisher Expansion 的基本思想是：利用标准正态分位数，加上偏度和峰度修正，近似得到非正态分布的分位数。设：
$$ z(\tau)=\Phi^{-1}(\tau), $$

其中 $\Phi$ 是标准正态分布的 CDF。

本文使用的展开形式为：
$$ Q_{i,t}(\tau) = \mu_{i,t} +z(\tau)\sigma_{i,t}+ \frac{z(\tau)^2-1}{6} \sigma_{i,t}s_{i,t} + \frac{z(\tau)^3-3z(\tau)}{24} \sigma_{i,t}(k_{i,t}-3)+ \sigma_{i,t}\omega_{i,t}(\tau). $$

其中：
$$ \omega_{i,t}(\tau) $$
是展开的余项，用来容纳未被显式列出的高阶影响。

这里不应该把这个展开理解为收益率服从正态分布。它是一个利用正态分位数作为基准、再利用偏度和峰度修正的近似表达。标准 Cornish-Fisher 展开还包含更高阶的修正项，本文将未显式保留的部分放入余项。

## 5.2 将公式转化为线性回归

这是 QCM 最关键的一步。

定义：
$$ z_l=z(\tau_l). $$

构造解释变量：
$$ \boxed{ Z_l= \begin{pmatrix} z_l\\ z_l^2-1\\ z_l^3-3z_l \end{pmatrix} } $$

再定义参数：
$$ \boxed{ \beta_{i,t} = \begin{pmatrix} \beta_{i,t,1}\\ \beta_{i,t,2}\\ \beta_{i,t,3} \end{pmatrix} = \begin{pmatrix} \sigma_{i,t}\\ \dfrac{\sigma_{i,t}s_{i,t}}{6}\\ \dfrac{\sigma_{i,t}(k_{i,t}-3)}{24} \end{pmatrix} } $$

那么原来的公式就变成：
$$ \boxed{ Q_{i,t}(\tau_l) = \mu_{i,t} + Z_l^\top\beta_{i,t} + \epsilon^\bullet_{i,t,l} } $$

其中：
$$ \epsilon^\bullet_{i,t,l} = \sigma_{i,t}\omega_{i,t}(\tau_l) + \left[ \widehat Q_{i,t}(\tau_l)-Q_{i,t}(\tau_l) \right]. $$

你会发现一个很有意思的事情：
原本复杂的条件分布估计问题，被转化成了一个线性回归问题。
这里的因变量是不同分位数水平下的预测分位数。
解释变量则是由标准正态分位数构造的三个多项式项。

## 5.3 为什么需要额外增加一个截距？

作者指出：

$$ \epsilon^\bullet_{i,t,l} $$

不一定满足零均值条件。

因此，直接对上面的表达做 OLS 可能存在识别问题。

于是引入一个干扰参数：

$$ \gamma_{i,t}. $$

构造：

$$ \boxed{ \widehat Q_{i,t}(\tau_l) = (\mu_{i,t}+\gamma_{i,t}) + Z_l^\top\beta_{i,t} + \epsilon_{i,t,l} } $$

令：

$$ \overline Z_l = \begin{pmatrix} 1\\ Z_l \end{pmatrix} $$

$$ \theta_{i,t} = \begin{pmatrix} \beta_{i,t,0}\\ \beta_{i,t,1}\\ \beta_{i,t,2}\\ \beta_{i,t,3} \end{pmatrix} $$

其中：

$$ \beta_{i,t,0}=\mu_{i,t}+\gamma_{i,t}. $$

那么：

$$ \boxed{ \widehat Q_{i,t}(\tau_l) = \overline Z_l^\top\theta_{i,t} + \epsilon_{i,t,l} } $$

现在已经完全变成标准的线性回归形式。

## 5.4 OLS 的具体计算

假设我们训练了 $L$ 个不同的条件分位数模型：

$$ \tau_1,\tau_2,\ldots,\tau_L. $$

对于固定的资产 $i$ 和时间 $t$，我们拥有：

$$ \widehat Q_{i,t}(\tau_1), \ldots, \widehat Q_{i,t}(\tau_L). $$

构造：

$$ \mathbf Y_{i,t} = \begin{pmatrix} \widehat Q_{i,t}(\tau_1)\\ \widehat Q_{i,t}(\tau_2)\\ \vdots\\ \widehat Q_{i,t}(\tau_L) \end{pmatrix} \in\mathbb R^L. $$

以及：

$$ \overline Z = \begin{pmatrix} 1&z_1&z_1^2-1&z_1^3-3z_1\\ 1&z_2&z_2^2-1&z_2^3-3z_2\\ \vdots&\vdots&\vdots&\vdots\\ 1&z_L&z_L^2-1&z_L^3-3z_L \end{pmatrix}. $$

于是：

$$ \mathbf Y_{i,t} = \overline Z\theta_{i,t} + \boldsymbol\epsilon_{i,t}. $$

OLS 解为：

$$ \boxed{ \widehat\theta_{i,t} = (\overline Z^\top\overline Z)^{-1} \overline Z^\top\mathbf Y_{i,t} } $$

这就是 QCM 的核心计算。

注意，这里不需要神经网络再次进行梯度下降。只需要进行一次四参数的 OLS 回归。

## 5.5 如何得到条件方差、偏度和峰度？

通过上面的回归得到：

$$ \widehat\theta_{i,t} = \begin{pmatrix} \widehat\beta_{i,t,0}\\ \widehat\beta_{i,t,1}\\ \widehat\beta_{i,t,2}\\ \widehat\beta_{i,t,3} \end{pmatrix}. $$

根据参数定义：

$$ \beta_{i,t,1}=\sigma_{i,t}. $$

因此：

$$ \boxed{ \widehat\sigma_{i,t}^{\,2} = \widehat\beta_{i,t,1}^{\,2} } $$

偏度：

$$ \boxed{ \widehat s_{i,t} = \frac{6\widehat\beta_{i,t,2}} {\widehat\beta_{i,t,1}} } $$

峰度：

$$ \boxed{ \widehat k_{i,t} = \frac{24\widehat\beta_{i,t,3}} {\widehat\beta_{i,t,1}} +3 } $$

所以最终我们就从多个分位数预测中，得到了：

- 条件方差；
- 条件偏度；
- 条件峰度。

这就是 QCM。

## 5.6 QCM 最有价值的两个性质

### 性质一：不需要事先估计条件均值

这是一个非常重要的性质。传统的条件方差估计通常需要：

$$ \varepsilon_{i,t} = r_{i,t}-\widehat\mu_{i,t} $$

然后对残差进行建模。

但是这里：

$$ \beta_{i,t,0} = \mu_{i,t}+\gamma_{i,t}. $$

由于干扰参数 $\gamma_{i,t}$ 吸收了截距中的偏差，所以：

$$ \widehat\beta_{i,t,1} $$

仍然可以用于估计标准差。

因此，不需要先训练一个高维条件均值模型。

这对于高维资产来说，能够减少很多额外工作。

### 性质二：允许条件分位数存在一定估计偏差

QCM 并不要求每个分位数模型都完全正确。

因为条件分位数的估计误差可以在一定程度上被截距中的干扰参数吸收。

不过，这并不意味着任意严重的分位数误差都不会影响方差估计。

论文给出了相应的一致性条件：

$$ \overline Z^\top\overline Z $$

需要正定，并且误差项满足：

$$ \frac{1}{L} \overline Z^\top\boldsymbol\epsilon \xrightarrow{p}0. $$

在这些条件下，当分位数水平数量 $L$ 增大时，QCM 参数估计具有一致性。

还要注意：QCM 的截距无法准确恢复条件均值，因为其中混入了 $\gamma_{i,t}$。如果需要条件均值，必须单独估计。

# 六、如何估计两只股票之间的条件协方差？

现在我们已经能够估计：

$$ \sigma_{i,t}^2,\quad \sigma_{j,t}^2. $$

接下来要估计：

$$ \sigma_{ij,t}. $$

作者没有直接建立一个双变量分布模型，而是使用了一个非常巧妙的恒等式。

## 6.1 一个简单的方差恒等式

令：

$$ r_{ij,t}=r_{i,t}+r_{j,t}. $$

那么：

$$ \begin{aligned} \operatorname{Var}(r_{ij,t}\mid\mathcal F_{t-1}) ={}& \operatorname{Var}(r_{i,t}+r_{j,t}\mid\mathcal F_{t-1})\\ ={}& \operatorname{Var}(r_{i,t}\mid\mathcal F_{t-1})+ \operatorname{Var}(r_{j,t}\mid\mathcal F_{t-1})+2\operatorname{Cov}(r_{i,t},r_{j,t}\mid\mathcal F_{t-1}). \end{aligned} $$

定义：

$$ \delta_{ij,t}^2 = \operatorname{Var}(r_{i,t}+r_{j,t} \mid\mathcal F_{t-1}). $$

于是：

$$ \delta_{ij,t}^2 = \sigma_{i,t}^2+ \sigma_{j,t}^2+ 2\sigma_{ij,t}. $$

移项：

$$ \boxed{ \sigma_{ij,t} = \frac12 \left( \delta_{ij,t}^2 -\sigma_{i,t}^2 -\sigma_{j,t}^2 \right) } $$

这就是本文式（1）。

非常重要的一点：

估计两只股票的条件协方差，只需要额外估计它们收益率之和的条件方差。

而条件方差已经可以通过前面的 QCM 方法估计。

所以整个模型又回到了熟悉的单变量条件分布估计问题。

这就是作者所说的 Divide and Conquer。

## 6.2 难点：两只股票相加之后，没有对应的特征

假设我们要估计：

$$ Q_{ij,t}(\tau) = Q_{r_i+r_j\mid\mathcal F_{t-1}}(\tau). $$

我们知道股票 $i$ 和股票 $j$ 的特征：

$$ X_{i,t-1},\quad X_{j,t-1}. $$

但并没有天然存在的：

$$ X_{ij,t-1}. $$

毕竟，两只股票组合并不是市场中真实存在的一只股票。

如果简单把两只股票的特征取平均，可能会损失大量非线性信息。

因此作者提出：**Node Fusion（节点融合）** 技术。

## 6.3 Node Fusion 的数学表达

首先，构造一个人工的融合节点：

$$ \boxed{ X_{ij,t-1} = m(X_{i,t-1},X_{j,t-1};\psi) } $$

其中：

$$ m: \mathbb R^{P\times S} \times \mathbb R^{P\times S} \longrightarrow \mathbb R^{P\times S}. $$

作者采用两层全连接网络。

第一层：

$$ H = \operatorname{LeakyReLU} \left[ W_7 \begin{pmatrix} X_{i,t-1}\\ X_{j,t-1} \end{pmatrix} +B_7 \right]. $$

第二层：

$$ \boxed{ X_{ij,t-1} = \operatorname{LeakyReLU}(W_8H+B_8) } $$

其中参数：

$$ W_7\in\mathbb R^{D\times2P} $$

$$ W_8\in\mathbb R^{P\times D} $$

$$ B_7\in\mathbb R^{D\times S} $$

$$ B_8\in\mathbb R^{P\times S}. $$

所以：

$$ \boxed{ \psi=\{W_7,W_8,B_7,B_8\} } $$

就是 Node Fusion 需要学习的参数。

它的作用是将两只股票的历史特征进行非线性融合，生成一组人工特征。

## 6.4 将融合节点重新放入 SGA

现在我们把原来的两只股票：

$$ i,j $$

替换成一只人工的融合股票：

$$ (i,j). $$

此时整个输入资产集合变成：

$$ \{(i,j),1,\ldots,i-1,i+1,\ldots,j-1,j+1,\ldots,N\}. $$

也就是说：

- 原本的 $N$ 个节点；
- 删除 $i,j$；
- 新增一个融合节点 $(i,j)$。

因此节点总数变为：

$$ N-1. $$

然后重新使用 SGA：

$$ \boxed{ \mathbf Q_t^{(i,j)}(\tau) = f\left( \mathbf X_{t-1}^{(i,j)}(\psi_\tau); G_{t,\tau}^{(i,j)}, \theta_\tau^{(i,j)} \right) } $$

其中第一个输出就是：

$$ \widehat Q_{ij,t}(\tau). $$

这一步很有意思：

作者不仅学习融合节点自身的特征，还允许其他股票通过重新构建的图结构影响这个融合节点的条件分位数。

因此，它并不是一个简单的两股票特征拼接网络。

## 6.5 Node Fusion 的训练参数与梯度下降

理论上，作者可以联合优化：

$$ (\psi_\tau,\phi_\tau,\theta_\tau). $$

目标函数为：

$$ \boxed{ \begin{aligned} \min_{\psi_\tau,\phi_\tau,\theta_\tau} \sum_{t\in\mathcal T} \sum_{i<j} \sum_{k=1}^{N-1} \rho_\tau \left( r_{k,t}^{(i,j)} - \widehat Q_{k,t}^{(i,j)}(\tau) \right). \end{aligned} } $$

这里：

$$ \mathbf r_t^{(i,j)} $$

是融合后的真实收益率向量，第一个元素为：

$$ r_{i,t}+r_{j,t}. $$

其他元素为剩余股票的收益率。

但这样做有一个问题：
每次训练都要同时优化大量 SGA 参数，计算成本很高。
于是作者采用了类似 Transfer Learning 的思路。
先完成原始 SGA 的训练，再冻结原始网络参数，只训练 Node Fusion。

第一阶段：

$$ (\widehat\phi_\tau,\widehat\theta_\tau) $$

通过普通分位数模型得到。

第二阶段：

固定：

$$ \phi_\tau=\widehat\phi_\tau, \qquad \theta_\tau=\widehat\theta_\tau. $$

仅优化：

$$ \boxed{ \widehat\psi_\tau = \arg\min_{\psi_\tau} \sum_{t\in\mathcal T} \sum_{i<j} \ell^{(i,j)} (r_t,X_{t-1}; \psi_\tau,\widehat\phi_\tau,\widehat\theta_\tau,\tau) } $$

这样，第二阶段真正需要梯度下降的参数就只有：

$$ \boxed{ \psi_\tau=\{W_7,W_8,B_7,B_8\}. } $$

论文的 Algorithm E2 进一步采用：

- 随机抽取一个时间点；
- 从所有可能的股票对中随机抽取 $M$ 个；
- 计算这批股票对的分位数损失；
- 对 $\psi_\tau$ 求梯度；
- 使用 Adam 更新。

也就是说，作者不是每次都对全部股票对进行完整优化，而是采用 Pair Minibatch。

论文比较了联合训练与冻结参数两种实现。联合训练的计算时间增加约12%，但没有带来主要投资表现指标的改善，因此最终采用冻结参数的基准版本。


## 6.6 最终通过 QCM 得到条件协方差

现在我们已经有：
$$ \widehat Q_{ij,t}(\tau_1),\ldots, \widehat Q_{ij,t}(\tau_L). $$
完全重复第五部分的 QCM 过程，得到：
$$ \widehat\delta_{ij,t}^{\,2}. $$
最终：

$$ \boxed{ \widehat\sigma_{ij,t} = \frac12 \left( \widehat\delta_{ij,t}^{\,2} - \widehat\sigma_{i,t}^{\,2} - \widehat\sigma_{j,t}^{\,2} \right) } $$

至此，协方差矩阵的所有元素在理论上都可以逐个估计。

但是，如果有1950只股票，需要处理：

$$ \frac{1950\times1949}{2} = 1,901,775 $$

个不同的股票对。

计算成本仍然很高。

因此，作者又引入了下一项设计。

# 七、Variance-based Causal Graph：如何降低计算复杂度？

现在面临一个问题：

$$ N=1950 $$

意味着将近190万组不同的股票对。

如果每个时间点都计算全部股票对的条件协方差，计算成本仍然非常高。

作者的解决办法是：先学习一个条件方差图，再利用这个图筛选需要估计协方差的股票对。

## 7.1 为什么还要训练一个新的 SGA？

前面的 SGA 主要用于预测条件分位数。

这里作者另行训练一个 SGA，直接以条件方差作为预测目标。

定义：

$$ \boldsymbol\sigma_t^2 = \begin{pmatrix} \sigma_{1,t}^2\\ \vdots\\ \sigma_{N,t}^2 \end{pmatrix}. $$

模型：

$$ \boxed{ \boldsymbol\sigma_t^2 = f(\mathbf X_{t-1};G_{t,\sigma},\theta_\sigma) + \boldsymbol\epsilon_t } $$

其中：

$$ G_{t,\sigma} = q(\mathbf X_{t-1};\phi_\sigma). $$

这里有一套新的参数：

$$ \phi_\sigma,\quad\theta_\sigma. $$

它们与之前条件分位数模型的参数不是同一套。

## 7.2 使用什么损失函数？

这里不再使用 Quantile Loss，而是使用 MSE。
注意，目标变量不是原始收益率，而是前面 QCM 得到的条件方差估计：

$$ \widehat\sigma_{i,t}^{\,2,QCM}. $$

因此：

$$ \boxed{ \begin{aligned} L_\sigma(\phi_\sigma,\theta_\sigma) = \sum_{t\in\mathcal T}\sum_{i=1}^N \left[ \widehat\sigma_{i,t}^{\,2,QCM} - f_i(\mathbf X_{t-1}; q(\mathbf X_{t-1};\phi_\sigma), \theta_\sigma) \right]^2. \end{aligned} } $$

然后：

$$ (\widehat\phi_\sigma,\widehat\theta_\sigma) = \arg\min_{\phi_\sigma,\theta_\sigma} L_\sigma. $$

优化方法仍然是 Adam。

## 7.3 这里的输入特征有什么变化？

作者为了提高条件方差图的学习效率，在原来的股票特征基础上增加了四个特征：

1. 已经估计的条件均值；
2. 已经估计的条件方差；
3. 已经估计的条件偏度；
4. 已经估计的条件峰度。

因此，条件方差模型的输入包含：

$$ \mathbf x_{i,t}^{*} = \begin{pmatrix} \mathbf x_{i,t}\\ \widehat\mu_{i,t}\\ \widehat\sigma_{i,t}^{\,2}\\ \widehat s_{i,t}\\ \widehat k_{i,t} \end{pmatrix}. $$

这里的矩估计使用截至 $t-1$ 时刻可获得的信息，避免引入未来数据。

## 7.4 如何得到最终的方差图？

利用训练好的 Encoder：

$$ \widehat G_{t,\sigma} = q(\mathbf X_{t-1};\widehat\phi_\sigma). $$

得到整体邻接矩阵：

$$ A_{t,\sigma}. $$

其元素定义为：

$$ \boxed{ A_{t,\sigma}^{(i,j)} = \begin{cases} 1,&\exists k:\ i,j\in M_{k,t,\sigma},\\ 0,&\text{otherwise}. \end{cases} } $$

因此，如果两只股票在方差预测的关系图中存在连接，就认为它们之间存在值得进一步考察的非线性 Granger 关系。

如果没有连接，则按照本文的设定：

$$ \boxed{ \sigma_{ij,t}=0. } $$

于是最终协方差矩阵的初步估计为：

$$ \boxed{ \widehat\Sigma_t^{(i,j)} = \begin{cases} \widehat\sigma_{i,t}^{\,2},&i=j,\\[4pt] \dfrac{ \widehat\delta_{ij,t}^{\,2} -\widehat\sigma_{i,t}^{\,2} -\widehat\sigma_{j,t}^{\,2} }{2}, &i\ne j,\ A_{t,\sigma}^{(i,j)}=1,\\[8pt] 0,&i\ne j,\ A_{t,\sigma}^{(i,j)}=0. \end{cases} } $$

这就是论文公式（15）。

### 一个值得注意的统计问题

这里存在一个需要认真理解的假设：

$$ \text{不存在非线性 Granger 因果关系} \Longrightarrow \text{条件协方差为零}. $$

这不是普遍成立的数学定理，而是作者为了实现稀疏化而采用的建模准则。

例如，两只股票可能存在同时受到市场冲击的现象，使得它们具有较高的同期条件协方差，但彼此的历史特征并不能帮助预测对方的条件方差。

这时，单纯使用 Granger 图筛选可能会遗漏一些真实的同期协方差。

因此，这一步实际上引入了一个重要的结构假设。

作者的实证结果表明，这种做法能够有效降低计算负担。论文报告其计算复杂度大致为：

$$ O(N^\kappa) $$

其中 $\kappa$ 是一个较小的正数。这里是作者在稀疏关系图条件下给出的量级描述，不应理解为对所有数据结构都成立的普遍复杂度保证。


# 八、最后一步：Positive Semidefinite Projection

现在我们已经得到了：

$$ \widehat\Sigma_t. $$

但是，直接逐元素估计出来的矩阵不一定是一个合法的协方差矩阵。

这涉及一个重要的线性代数知识点。

## 8.1 什么是半正定矩阵？

对于任意实对称矩阵 $A$，如果：

$$ \boxed{ \mathbf x^\top A\mathbf x\geq0, \quad\forall\mathbf x\in\mathbb R^N, } $$

那么称 $A$ 为半正定矩阵：

$$ A\succeq0. $$

一个合法的协方差矩阵必须满足：

$$ \Sigma_t\succeq0. $$

等价地，其所有特征值都必须非负：

$$ \lambda_i(\Sigma_t)\geq0. $$

为什么？

因为对于任意组合投资组合权重 $\mathbf w$，组合收益率的方差为：

$$ \operatorname{Var}(\mathbf w^\top\mathbf r_t \mid\mathcal F_{t-1}) = \mathbf w^\top\Sigma_t\mathbf w. $$

方差不可能小于零。

## 8.2 为什么逐元素估计可能不满足半正定？

我们在前面分别估计：

$$ \sigma_{i,t}^2,\quad \sigma_{ij,t}. $$

这些估计分别来自不同的神经网络输出和 QCM 回归。

因此，可能出现：

$$ |\widehat\sigma_{ij,t}| > \sqrt{ \widehat\sigma_{i,t}^{\,2} \widehat\sigma_{j,t}^{\,2} }. $$

这违反了协方差的基本约束：

$$ |\sigma_{ij,t}| \leq \sigma_{i,t}\sigma_{j,t}. $$

更进一步，即使所有两两协方差都满足这个约束，也不能保证整个矩阵一定半正定。

因此必须进行全局矩阵调整。

## 8.3 优化问题

作者采用 Higham（2002）的 Positive Semidefinite Projection 方法。

数学表达为：

$$ \boxed{ \begin{aligned} \widetilde\Sigma_t = \arg\min_{\Sigma_t} &\quad \|\Sigma_t-\widehat\Sigma_t\|_F\\ \text{s.t.} &\quad \Sigma_t\succeq0,\\ &\quad \operatorname{diag}(\Sigma_t) = \operatorname{diag}(\widehat\Sigma_t). \end{aligned} } $$

其中：

$$ \|A\|_F = \sqrt{\sum_{i,j}a_{ij}^2} $$

是 Frobenius Norm。

这个优化问题的含义是：

> 在保持所有资产条件方差不变的前提下，寻找一个距离原始估计矩阵最近的半正定矩阵。

如果原始矩阵已经半正定：

$$ \widetilde\Sigma_t=\widehat\Sigma_t. $$

否则就需要进行调整。

Higham 的经典工作研究了最近相关矩阵问题，提出了修正的交替投影算法。其核心是处理半正定约束与固定对角线约束之间的交集。

### 为什么不能简单地把负特征值改成零？

如果没有固定对角线约束，可以对矩阵进行特征值分解：

$$ \widehat\Sigma=U\Lambda U^\top. $$

然后：

$$ \Lambda^+ = \operatorname{diag} \left( \max(\lambda_1,0),\ldots,\max(\lambda_N,0) \right). $$

再构造：

$$ \Sigma^+=U\Lambda^+U^\top. $$

这可以得到一个半正定矩阵。

但它通常会改变对角线元素，也就是改变已经估计出来的各资产方差。

而本文要求：

$$ \operatorname{diag}(\widetilde\Sigma_t) = \operatorname{diag}(\widehat\Sigma_t). $$

因此需要处理两个约束的交集，不能仅仅进行一次特征值截断。

注意：PSD Projection 不是本文神经网络的一部分，也不通过前面的 Adam 进行训练。它是最后独立执行的数值矩阵优化步骤。

# 九、所有需要拟合的参数：完整汇总

现在把全文的参数体系整理一下。这是以后阅读类似论文时非常值得保留的一张表。

## 9.1 神经网络参数

|参数|所属模型|具体作用|优化方法|
|---|---|---|---|
|$\phi_\tau$|条件分位数 SGA Encoder|GRU 时间特征提取|Adam|
|$\theta_\tau$|条件分位数 SGA Decoder|群体与个体信息整合|Adam|
|$\psi_\tau$|Node Fusion|两只股票的非线性特征融合|Adam|
|$\phi_\sigma$|方差 SGA Encoder|学习方差预测关系图|Adam|
|$\theta_\sigma$|方差 SGA Decoder|预测各资产条件方差|Adam|
|$\phi_\mu$|均值 SGA Encoder|学习条件均值|Adam|
|$\theta_\mu$|均值 SGA Decoder|预测各资产条件均值|Adam|

其中，均值模型是附录 D 中的扩展模型，并不是估计条件协方差矩阵所必需的部分。

## 9.2 QCM 参数

对于每一个资产 $i$ 和时间 $t$，需要估计：

$$ \theta_{i,t} = (\beta_{i,t,0},\beta_{i,t,1}, \beta_{i,t,2},\beta_{i,t,3})^\top. $$

对于每一个资产组合 $(i,j)$ 和时间 $t$，需要估计：

$$ \theta_{i,j,t} = (\beta_{i,j,t,0},\beta_{i,j,t,1}, \beta_{i,j,t,2},\beta_{i,j,t,3})^\top. $$

这些参数通过 OLS 得到。

它们不是神经网络中的可训练参数，而是每个时间点、每个资产或资产对对应的统计量估计。

## 9.3 超参数

超参数不由梯度下降直接决定，而是通过候选集合和验证集选择。

论文 Table 1 给出的候选集合为：

|超参数|含义|候选值|
|---|---|---|
|$S$|输入历史窗口长度|24、48、96|
|$\gamma$|Adam 学习率|$10^{-4},10^{-3},10^{-2}$|
|$K^*$|每只股票保留的相似中心节点数量|1–20|
|$D$|GRU 隐藏单元数量|64、128、256|
|$M$|Node Fusion 股票对 minibatch 大小|32、64、128|
|$L$|条件分位数水平数量|99|
|$\alpha$|分位数有效性检验显著性水平|0.01|

这里尤其需要区分：

- $D$ 是神经网络结构超参数；
- $S$ 是时间窗口超参数；
- $K^*$ 是图结构超参数；
- $\gamma$ 是优化超参数；
- $M$ 是计算批次超参数；
- $L$ 是 QCM 的分位数网格数量。

它们不是需要通过反向传播拟合的模型权重。


# 十、完整训练过程：把论文转换成可以执行的算法

如果你以后要自己复现这篇文章，我建议按照下面的顺序理解。

## Step 1：数据预处理

输入：

$$ \{r_{i,t},x_{i,t}\}_{i=1,\ldots,N;\ t=1,\ldots,T}. $$

进行：

- 缺失值处理；
- 特征标准化；
- 处理财务数据公布延迟；
- 构建历史窗口；
- 生成三维特征张量。

## Step 2：训练条件分位数模型

对于每一个：

$$ \tau_l=\frac{l}{L+1}, \quad l=1,\ldots,L, $$

训练一套独立的 SGA：

$$ (\widehat\phi_{\tau_l}, \widehat\theta_{\tau_l}). $$

损失函数：

$$ L_{\tau_l} = \sum_{t,i} \rho_{\tau_l} (r_{i,t}-\widehat Q_{i,t}(\tau_l)). $$

优化方法：Adam。

## Step 3：条件分位数筛选

不同分位数模型分别训练，可能出现分位数预测不合理的情况。

例如：

$$ \widehat Q_{i,t}(0.05) > \widehat Q_{i,t}(0.10). $$

这违反了分位数单调性。

因此作者使用：

- Kupiec（1995）的 LRuc 检验；
    
- Christoffersen（1998）的 LRcc 检验。
    

对无条件覆盖率和条件覆盖率进行检验，筛选有效分位数。

## Step 4：使用 QCM 估计对角线

对每只股票、每个时间点：

$$ \widehat Q_{i,t}(\tau_1),\ldots, \widehat Q_{i,t}(\tau_L) $$

进行 OLS：

$$ \widehat\sigma_{i,t}^{\,2} = \widehat\beta_{i,t,1}^{\,2}. $$

## Step 5：训练 Node Fusion

对于每一个分位数水平：

1. 加载预训练的 SGA；
    
2. 固定 $\widehat\phi_\tau,\widehat\theta_\tau$；
    
3. 初始化融合网络参数 $\psi_\tau$；
    
4. 随机抽取时间点和 $M$ 组股票对；
    
5. 计算融合特征；
    
6. 重新构造 SGA 图；
    
7. 计算融合后的分位数损失；
    
8. 使用 Adam 更新 $\psi_\tau$。
    

## Step 6：估计组合方差与条件协方差

通过融合节点得到：

$$ \widehat Q_{ij,t}(\tau_1),\ldots, \widehat Q_{ij,t}(\tau_L). $$

进行 QCM：

$$ \widehat\delta_{ij,t}^{\,2}. $$

再计算：

$$ \widehat\sigma_{ij,t} = \frac12 \left( \widehat\delta_{ij,t}^{\,2} -\widehat\sigma_{i,t}^{\,2} -\widehat\sigma_{j,t}^{\,2} \right). $$

## Step 7：训练方差关系图

以 QCM 方差估计为训练标签：

$$ \widehat{\boldsymbol\sigma}_t^{\,2,QCM}. $$

训练另一个 SGA：

$$ \min_{\phi_\sigma,\theta_\sigma} L_{\mathrm{MSE}}. $$

得到：

$$ \widehat A_{t,\sigma}. $$

根据图结构筛选需要计算的协方差元素。

## Step 8：组装并投影

形成：

$$ \widehat\Sigma_t. $$

然后：

$$ \boxed{ \widetilde\Sigma_t = \operatorname{PSDProjection}(\widehat\Sigma_t). } $$

最终得到每个时点的条件协方差矩阵。

# 十一、论文的实证设计及其结果

为了帮助理解模型的实际应用，也整理一下作者的实验设置。

## 11.1 数据

|项目|设置|
|---|---|
|市场|中国 A 股|
|时间|2010年1月—2022年7月|
|观测频率|周频|
|时间序列长度|645周|
|股票数量|1950|
|每只股票的初始特征数|31|

特征包括市场指数、宏观经济指标、股票价格与成交数据、财务指标和行业分类等。

作者将2010—2018年作为样本内时期，在样本内进一步划分训练集和验证集，比例分别为80%和20%。

2019—2022年作为真正的样本外测试集。

## 11.2 为什么使用99个分位数？

作者采用：

$$ L=99 $$

对应：

$$ \tau_l=\frac{l}{100}, \quad l=1,\ldots,99. $$

也就是：

$$ 0.01,0.02,\ldots,0.99. $$

这使得 QCM 能够利用比较丰富的分位数信息来估计条件方差、偏度和峰度。

不过，分位数模型分别训练，因此不保证天然满足：

$$ Q(\tau_1)\leq Q(\tau_2), \qquad \tau_1<\tau_2. $$

论文确实观察到样本外存在局部分位数交叉。

经过筛选后，对于间隔至少0.1的有效分位数水平，没有观察到严重交叉，但局部交叉仍然存在。这是值得关注的模型问题。

MachineLearningVastDynamicConditionalCovarianceMatrices.pdf

## 11.3 投资组合表现

论文使用 GRAND 预测的条件均值和协方差矩阵构建 Markowitz 切点组合。

其主要结果：

GRAND 样本外年化 Sharpe Ratio

# 1.48

相比 DCC-NL 方法高出 26.5%。

相比论文中的其他基准、因子模型和因子-DCC-NL 模型，优势更加明显。

这里的投资组合设置包括：

- 不允许卖空；
    
- 单只股票最大权重为5%；
    
- 每周重新平衡；
    
- 交易成本为20个基点。
    

这说明作者不只是验证了协方差矩阵的统计估计，还进一步验证了其在资产配置中的实际价值。

MachineLearningVastDynamicConditionalCovarianceMatrices.pdf

# 十二、我认为最值得深入理解的几个问题

到这里，模型已经拆解完毕。但如果把这篇文章当成长期学习材料，我认为还有几个问题值得专门记下来。

## 12.1 SGA 的图真的代表因果关系吗？

作者使用的是 Granger Causality。

其基本定义是：

如果在预测资产 $i$ 的目标变量时，加入资产 $j$ 的历史信息能够改变预测结果，那么资产 $j$ 对资产 $i$ 存在 Granger 因果关系。

形式上：

$$ y_{i,t} = f_i(X_{1,\leq t-1},\ldots,X_{N,\leq t-1}). $$

如果改变：

$$ X_{j,\leq t-1} $$

会改变：

$$ f_i(\cdot), $$

那么就认为 $j$ Granger-causes $i$。

但有两个需要注意的地方。

第一，Granger 因果是预测意义上的因果，不等于经济学中的结构因果，也不意味着干预某只股票的价格就会造成另一只股票发生变化。

第二，本文的图结构是无向的。

因为一个关系组中的节点被定义为彼此连接，所以：

$$ A_{t,\sigma}^{(i,j)} = A_{t,\sigma}^{(j,i)}. $$

传统 Granger 因果关系可以是有方向的，而这里的图主要表示共同的预测关系组。

因此，解释结果时最好称为动态预测关联或 Granger 型关系结构，而不要直接将其解释成严格的有向因果网络。

## 12.2 为什么要使用两套不同的 SGA？

这篇论文实际上有两种图：

|图|预测目标|用途|
|---|---|---|
|分位数图 $G_{t,\tau}$|条件分位数|学习不同风险分位数的动态关系|
|方差图 $G_{t,\sigma}$|条件方差|筛选需要估计的条件协方差|

这是一个很容易忽略的地方。

尤其要注意，作者不是先学习一个统一的图，然后所有任务都使用这个图。

不同的预测目标可以产生不同的动态关系。

例如：

- 在预测收益率中存在联系的两只股票；
    
- 在预测条件方差中存在联系的两只股票；
    
- 在预测极端下跌分位数中存在联系的两只股票。
    

这三种关系不一定相同。

这也是本文比简单地按照行业分组更灵活的地方。

## 12.3 QCM 是否意味着完全不需要分布假设？

不能这样理解。

分位数回归本身不需要假定收益率服从正态分布。

但是 QCM 通过 Cornish-Fisher Expansion 建立分位数与矩之间的近似关系。

所以它并不是完全没有统计假设。

需要理解的是：

1. 分位数模型不需要指定完整的参数化收益率分布；
    
2. QCM 使用 Cornish-Fisher 结构建立条件分位数与条件矩的联系；
    
3. QCM 的一致性依赖论文提出的设计矩阵和误差条件；
    
4. 实际使用中，分位数估计误差仍然可能影响矩估计。
    

这几个层次不能混为一谈。

## 12.4 为什么不直接训练一个神经网络输出协方差矩阵？

这实际上是本文最值得借鉴的设计思想。

假设直接训练：

$$ f_\theta: \mathbb R^{N\times P\times S} \longrightarrow \mathbb R^{N\times N}. $$

那么输出层就有 $N^2$ 个元素。

当 $N=1950$ 时，单个时间点就需要预测接近380万项矩阵元素。

而且，即使输出维度可以接受，还必须额外保证：

- 对称性；
    
- 半正定性；
    
- 方差非负；
    
- 协方差与方差之间的约束。
    

GRAND 将这些问题拆开：

$$ \text{单资产分布} \rightarrow \text{单资产方差} $$

$$ \text{双资产组合分布} \rightarrow \text{双资产协方差} $$

$$ \text{关系图} \rightarrow \text{稀疏化} $$

$$ \text{矩阵投影} \rightarrow \text{半正定约束}. $$

这是一种统计结构与机器学习结构相结合的设计方式。

# 十三、建议长期掌握的知识清单

如果希望真正掌握这篇文章，而不是只记住它的网络结构，我建议按照以下顺序学习。

第一层：统计学基础

1. Conditional Distribution / Quantile Regression
    
    条件分布、条件分位数、Pinball Loss。
    
2. Conditional Moments
    
    条件均值、方差、偏度、峰度。
    
3. Cornish-Fisher Expansion
    
    理解分位数与高阶矩之间的近似关系。
    
4. OLS 与渐近一致性
    
    理解 QCM 为什么可以通过线性回归估计条件方差。
    

第二层：机器学习基础

1. GRU / RNN
    
    门控机制、时间序列隐藏状态、BPTT。
    
2. Attention Mechanism
    
    Softmax 权重和信息聚合。
    
3. Graph Neural Networks
    
    节点、边、邻接矩阵、动态关系图。
    
4. Adam / Backpropagation
    
    链式法则、随机梯度、矩估计和偏差修正。
    

第三层：数值分析与金融统计

1. Positive Semidefinite Matrices
    
    特征值、半正定矩阵、协方差矩阵的合法性。
    
2. Matrix Projection
    
    Frobenius Norm、交替投影、最近半正定矩阵。
    
3. Granger Causality
    
    预测意义上的因果关系及其局限。
    
4. High-dimensional Covariance Estimation
    
    因子模型、收缩估计、DCC、稀疏协方差估计。
    

## 推荐阅读的背景文献

以下是与这篇文章的模型结构直接相关的原始或基础文献：

1. Cho et al. (2014), Learning Phrase Representations using RNN Encoder–Decoder for Statistical Machine Translation. GRU 的基础文献。[ACL Anthology](https://aclanthology.org/D14-1179/?utm_source=chatgpt.com)
    
2. Koenker & Bassett (1978), Regression Quantiles. 分位数回归的基础文献。[Econometrica](https://doi.org/10.2307/1913643?utm_source=chatgpt.com)
    
3. Kingma & Ba (2015), Adam: A Method for Stochastic Optimization. Adam 算法原始文献。[arXiv](https://arxiv.org/abs/1412.6980?utm_source=chatgpt.com)
    
4. Higham (2002), Computing the Nearest Correlation Matrix—A Problem from Finance. 最近半正定相关矩阵问题。[University of Manchester](https://eprints.maths.manchester.ac.uk/232/?utm_source=chatgpt.com)
    
5. Zhang & Zhu (2026), Quantiled Conditional Variance, Skewness, and Kurtosis by Cornish–Fisher Expansion. 与本文 QCM 部分直接相关的后续统计理论工作。[The Annals of Applied Statistics](https://doi.org/10.1214/25-AOAS2122?utm_source=chatgpt.com)
    

# 十四、最后总结：这篇论文真正值得记住的是什么？

我认为可以把它归纳成三个层次。

第一，统计思想。

作者没有直接估计高维协方差矩阵，而是利用：

$$ \operatorname{Var}(X+Y) = \operatorname{Var}(X) + \operatorname{Var}(Y) + 2\operatorname{Cov}(X,Y) $$

把协方差估计转换成单变量条件方差估计。

然后通过 QCM，把条件分位数转换成条件方差。

第二，机器学习思想。

作者通过 GRU 提取时间特征，通过 Self-grouping 发现资产之间的动态关系，再通过 Group Module 和 Individual Module 分离共同影响与个体异质性。

Node Fusion 则解决了组合资产没有天然特征输入的问题。

第三，数值计算思想。

通过方差图进行稀疏化，最后通过半正定投影确保协方差矩阵符合数学约束。

## 整篇论文最核心的数学链条

$$ \boxed{ \mathbf X_{t-1} \xrightarrow{\mathrm{SGA}} \widehat Q_{i,t}(\tau) } $$

$$ \boxed{ \widehat Q_{i,t}(\tau) \xrightarrow{\mathrm{QCM}} \widehat\sigma_{i,t}^{\,2} } $$

$$ \boxed{ (\widehat Q_{i,j,t}(\tau), \widehat\sigma_{i,t}^{\,2}, \widehat\sigma_{j,t}^{\,2}) \xrightarrow{\mathrm{QCM}} \widehat\sigma_{ij,t} } $$

$$ \boxed{ \widehat\Sigma_t \xrightarrow{\mathrm{PSD\ Projection}} \widetilde\Sigma_t\succeq0 } $$

我的评价： 这篇文章最有价值的部分并不是 GRU 或 Attention 本身，而是它把高维金融统计问题拆解成多个具有明确统计意义、又能通过机器学习实现的子问题。神经网络负责学习复杂的条件分布和动态关系，QCM 负责把分布信息转化为统计矩，线性代数负责保证最终估计的可行性。

如果要真正吃透这篇文章，我建议下一步重点推导 QCM 的一致性命题，以及为什么截距中的干扰参数能够吸收分位数估计偏差。这部分比单纯复现 SGA 网络更能帮助你理解作者的统计创新。