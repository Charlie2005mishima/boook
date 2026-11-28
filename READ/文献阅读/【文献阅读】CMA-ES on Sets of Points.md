

> **论文**：Uchida, K., Saito, S., Shirakawa, S., Hamano, R., Nomura, M. (2024). *CMA-ES on Sets of Points*.


## 1. 这篇论文要解决什么问题？

现实中有这样一类优化问题：你需要从几个预先给定的离散点里选一个最好的，但有时候这些离散点又和连续变量混在一起。

举例：风力发电机选址，只能从100个预先勘测过的候选位置里挑，但风机的叶片角度是连续变量。这就成了混合优化问题——部分变量从点集里选，部分变量是连续的。

现有的CMA-ES（一种很强的连续优化算法）处理不了这种问题，因为它本质上是为连续空间设计的。硬套到离散点集上，它会在非最优的点附近过早收敛。

这篇论文做的：把CMA-ES改造成能处理从点集中选点的版本，取名CMA-ES-SoP（SoP = Sets of Points）。


## 2. 核心思想

让CMA-ES在连续空间里照常采样，采样完后把每个样本投影到离它最近的离散点上，用这个离散点去计算目标函数。

问题在于：CMA-ES按连续空间的方式更新采样分布。不加干预的话，它会逐渐把采样集中到某一个离散点附近（哪怕不是最优的），然后卡死。

解决方案：加两个机制——

1. 边际修正：强制要求每个离散点被选中的概率不低于某个下限，防止过早锁定
2. 边际自适应：动态调整这个下限值，避免矫枉过正


## 3. 整体流程

每轮迭代的流程如下：

1. 从高斯分布采样连续样本
2. 将连续样本投影到最近的离散点（编码）
3. 用离散点计算目标函数值
4. 用原始连续样本更新CMA-ES的分布参数
5. 检查邻近离散点的生成概率，若低于边际值则修正协方差矩阵
6. 根据当前概率平均值调整边际值
7. 回到步骤1，直到收敛或达到最大评估次数


## 4. 关键数学公式与模块说明

### 4.1 采样（CMA-ES标准过程）

第t代生成λ个候选解：
$$
\boldsymbol{z}^{(i)} \sim \mathcal{N}(\boldsymbol{0}, \mathbf{I}),\quad
\boldsymbol{y}^{(i)} = \sqrt{\boldsymbol{C}^{(t)}}\boldsymbol{z}^{(i)},\quad
\boldsymbol{x}^{(i)} = \sigma^{(t)}\boldsymbol{y}^{(i)} + \boldsymbol{m}^{(t)}
$$

其中m为均值向量（分布中心），σ为步长，C为协方差矩阵（决定分布形状）。这三个参数是CMA-ES的核心状态。

### 4.2 样本编码

将连续样本x投影到离散点集S中最近的点：
$$
\mathrm{ENC}(\boldsymbol{x}) = \arg\min_{\boldsymbol{s} \in \mathcal{S}} \|\boldsymbol{x} - \boldsymbol{s}\|
$$

编码后的离散点用于计算目标函数。均值向量m本身也会被投影，用于确定当前分布中心落在哪个离散点的Voronoi区域内。

### 4.3 分布参数更新（CMA-ES标准过程）

用原始连续样本更新分布参数：

**均值更新**（向好的区域移动）：
$$
\boldsymbol{m}^{(t+1)} = \boldsymbol{m}^{(t)} + c_m \sum_{i=1}^{\mu} w_i (\boldsymbol{x}^{(i:\lambda)} - \boldsymbol{m}^{(t)})
$$

**步长更新**（控制全局探索范围）：
$$
\sigma^{(t+1)} = \sigma^{(t)} \exp\left(\frac{c_\sigma}{d_\sigma}\left(\frac{\|\boldsymbol{p}_{\sigma}^{(t+1)}\|}{\chi_N} - 1\right)\right)
$$

**协方差更新**（调整分布形状）：
$$
\boldsymbol{C}^{(t+1)} = (1 + \delta)\boldsymbol{C}^{(t)} + c_1\left(\boldsymbol{p}_c\boldsymbol{p}_c^{\mathrm{T}} - \boldsymbol{C}^{(t)}\right) + c_\mu\sum_{i=1}^{\mu}w_i\left(\boldsymbol{y}^{(i:\lambda)}(\boldsymbol{y}^{(i:\lambda)})^{\mathrm{T}} - \boldsymbol{C}^{(t)}\right)
$$

这三个更新公式是CMA-ES的核心。均值决定搜索中心，步长决定探索半径，协方差决定搜索方向和各维度上的分散程度。

### 4.4 边际概率计算

对于均值向量m附近的每个邻近离散点s_neighbor，计算中点：
$$
\boldsymbol{s}_{\mathrm{mid}} = \frac{\boldsymbol{m} + \boldsymbol{s}_{\mathrm{neighbor}}}{2}
$$

计算该方向上的马氏距离：
$$
d = \left\| \sqrt{\boldsymbol{C}^{-1}} \cdot \frac{\boldsymbol{s}_{\mathrm{mid}} - \boldsymbol{m}}{\sigma} \right\|
$$

边际概率（高斯分布在超过中点之外的尾部概率）：
$$
p = \Phi_{\mathrm{cdf}}(-d)
$$

如果p小于边际值α，说明分布抽中这个邻近离散点的概率太低，需要修正。中点作为阈值点是有道理的：若均值偏向点A，那么点B要能被抽中，采样点必须越过A和B的中点才行。

### 4.5 协方差修正

当p < α时，在协方差矩阵上沿该方向增加方差：
$$
\boldsymbol{C} \leftarrow \boldsymbol{C} + \frac{d^2 - \gamma_\alpha^2}{d^2 \cdot \gamma_\alpha^2} \cdot \boldsymbol{\xi}\boldsymbol{\xi}^{\mathrm{T}}, \quad \gamma_\alpha = \Phi_{\mathrm{pdf}}(1-\alpha)
$$

其中ξ是沿该方向的单位向量（只在当前子空间的分量上非零）。这行代码的直观含义是：沿着从均值指向邻近离散点的方向，把高斯分布的方差撑大，从而增加抽中那个离散点的概率。

修正后的马氏距离会等于γ_α，保证p = α。

### 4.6 边际自适应

计算所有邻近点概率的平均值，动态调整边际值：
$$
\alpha^{(t+1)} = \begin{cases}
\alpha^{(t)} / \beta, & \text{若平均概率} \geq \alpha_{\mathrm{target}} \\
\beta \cdot \alpha^{(t)}, & \text{否则}
\end{cases}
$$

其中α_target = 1/(N×λ)，β = 1 + 1/N。平均概率已达标时减小边际值（放松约束），概率偏低时增大边际值（加强约束），保持修正力度适中。


## 5. 完整算法伪代码

```
Algorithm: CMA-ES-SoP

输入: 目标函数 f, 各子空间的点集 S_1,...,S_K, 种群大小 λ
输出: 最优解

初始化:
    m(0) ← 随机初始均值向量
    σ(0) ← 初始步长
    C(0) ← I (单位矩阵)
    α_k(0) ← α_target (k = 1,...,K)
    t ← 0

while 终止条件未满足 do:

    /* 步骤1: 采样与编码 */
    for i = 1 to λ do:
        从 N(m(t), σ(t)^2 · C(t)) 采样 x(i)
        对每个子空间 k:
            x̃_k(i) ← argmin_{s ∈ S_k} ||x_k(i) - s||
        x̃(i) ← 拼接所有 x̃_k(i)
        计算 f(x̃(i))

    /* 步骤2: 更新分布参数 (用原始连续样本) */
    按式(5)(6)(7)更新 m(t+1), σ(t+1), C(t+1)

    /* 步骤3: 边际修正与自适应 (每个子空间独立) */
    for k = 1 to K do:
        找到 m_k(t+1) 的最近点 s_close
        找到 s_close 的所有Voronoi邻近点 S_neighbor

        for each s_neighbor in S_neighbor:
            计算中点 s_mid = (m_k + s_neighbor) / 2
            计算马氏距离 d (式13)
            计算边际概率 p = Φ_cdf(-d)   (式12)

            if p < α_k(t) then:
                沿该方向修正 C(t+1)        (式15)

        计算所有邻近点的平均边际概率 p_avg
        按式(16)更新 α_k(t+1)

    t ← t + 1

end while

返回: 历史上最优的编码后解
```


## 6. 实验结果简述

**离散优化**（全是点集）：
- 朴素CMA-ES在高维时成功率接近0
- CMA-ES-SoP成功率接近100%

**混合变量**（部分点集、部分连续）：
- CMA-ES-SoP在高维问题上明显更优
- 低维时朴素CMA-ES有时也能赢（点太少，容易蒙对）

**边际自适应的价值**：
- 固定边际值在高维性能下降
- 自适应版本保持优势


## 7. 一句话总结

CMA-ES-SoP在CMA-ES的采样-评估-更新循环中插入了三个操作：采样后投影到离散点（编码）、更新后检查离散点被抽中的概率是否太低（边际修正）、动态调整修正力度（边际自适应）。三个机制配合，使CMA-ES能处理从点集中选点的离散和混合变量优化。