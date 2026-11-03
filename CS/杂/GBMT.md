
XGBoost 和 LightGBM 都属于梯度提升决策树(Gradient Boosting Decision Trees, GBDT)家族。

## 一、GBDT：梯度提升决策树

### 1.1 回归树基础

**※【定义】** 回归树(Regression Tree)：
将特征空间划分为若干区域 $R_1,R_2,\ldots,R_J$，每个区域对应一个叶节点，预测结果为该叶节点的常数值 $f(\mathbf x)=\sum_{j=1}^Jc_jI(\mathbf x\in R_j)$，其中 $c_j$ 为叶节点预测值，$I(\cdot)$ 为指示函数。

**※【定理】** 平方误差损失下的叶节点最优预测值：
对于平方误差损失 $L=\frac12\sum_{i=1}^N(y_i-f(\mathbf x_i))^2$，在树的划分确定时，叶节点的最优预测值为该节点内所有样本真实值的均值，即 $c_j^*=\frac{1}{|R_j|}\sum_{i:\mathbf x_i\in R_j}y_i$。

单棵树存在欠拟合或过拟合的风险，GBDT 通过不断加入新的、相对简单的树，逐步修正已有模型的预测误差。

### 1.2 Boosting 与加法模型

**※【定义】** 加法模型(Additive Model)：
$F_M(\mathbf x)=F_0(\mathbf x)+\eta\sum_{m=1}^Mf_m(\mathbf x)$，其中 $F_0$ 为初始预测，$M$ 为迭代轮数，$f_m$ 为第 $m$ 轮训练出的回归树，$\eta\in(0,1]$ 为学习率(Learning Rate)。

第 $m$ 棵树的训练目标取决于前 $m-1$ 棵树的预测结果，第 $m$ 轮的更新为 $F_m(\mathbf x) = F_{m-1}(\mathbf x)+\eta f_m(\mathbf x)$。这是 Boosting 与 Random Forest（主要并行、独立训练、平均或投票）的根本区别。

### 1.3 拟合负梯度（伪残差）

**※【定义】** 伪残差(Pseudo-residual)：
在第 $m$ 轮，第 $i$ 个样本的负梯度 $r_{im}= -\left. \frac{\partial L(y_i,z)}{\partial z} \right|_{z=F_{m-1}(\mathbf x_i)}$。
然后训练一棵回归树 $f_m = \arg\min_{f\in\mathcal T} \sum_{i=1}^N(r_{im}-f(\mathbf x_i))^2$ 去拟合伪残差。

**※【例题】** 平方误差下的负梯度：
设 $L(y_i,\hat y_i)=\frac12(y_i-\hat y_i)^2$，一阶导数为 $\hat y_i-y_i$，负梯度为 $y_i-\hat y_i$。
此时负梯度恰好等于真实残差。若采用 Logistic Loss，负梯度通常是 $y_i-p_i$。

### 1.4 原始 GBDT 训练算法

1. 初始化模型 $F_0(\mathbf x)= \arg\min_c\sum_{i=1}^NL(y_i,c)$（平方误差下为均值 $\bar y$）。
2. 计算负梯度 $r_{im}$。
3. 训练回归树拟合 $(\mathbf x_i,r_{im})$ 得到叶节点区域 $R_{jm}$。
4. 计算叶节点更新值 $c_{jm} = \arg\min_c \sum_{\mathbf x_i\in R_{jm}} L(y_i,F_{m-1}(\mathbf x_i)+c)$（平方误差下等于平均残差）。
5. 更新模型 $F_m(\mathbf x) = F_{m-1}(\mathbf x) + \eta\sum_{j=1}^{J_m} c_{jm}I(\mathbf x\in R_{jm})$。
6. 重复迭代直至达到设定数量或验证集性能不再改善。

---

## 二、XGBoost：通过二阶优化构造更好的树

XGBoost (Extreme Gradient Boosting) 引入了系统的正则化、二阶优化和高效分裂点搜索机制。

### 2.1 目标函数与正则化

假设已训练 $t-1$ 棵树，加入新树 $f_t$ 后的目标函数为：$\mathcal L^{(t)} = \sum_{i=1}^N L(y_i,F_{t-1}(\mathbf x_i)+f_t(\mathbf x_i)) + \Omega(f_t)$.

**※【定义】** XGBoost 正则化项：对于具有 $T$ 个叶节点的树，$\Omega(f_t)=\gamma T+\frac{\lambda}{2}\sum_{j=1}^Tw_j^2$，其中 $w_j$ 为叶节点输出值，$\gamma$ 惩罚叶节点数量，$\lambda$ 惩罚叶节点权重（L2 正则化）。

### 2.2 二阶泰勒展开

在当前预测值 $\hat y_i^{(t-1)}=F_{t-1}(\mathbf x_i)$ 处，定义一阶梯度 $g_i= \left. \frac{\partial L(y_i,z)}{\partial z} \right|_{z=\hat y_i^{(t-1)}}$ 和二阶 Hessian $h_i= \left. \frac{\partial^2 L(y_i,z)}{\partial z^2} \right|_{z=\hat y_i^{(t-1)}}$。

**※【定理】** XGBoost 目标函数近似：忽略常数项后，目标函数近似为 $\widetilde{\mathcal L}^{(t)} = \sum_{i=1}^N \left[ g_if_t(\mathbf x_i)+\frac12h_if_t^2(\mathbf x_i) \right] +\Omega(f_t)$。

设第 $j$ 个叶节点样本集为 $I_j$，定义 $G_j=\sum_{i\in I_j}g_i,\ H_j=\sum_{i\in I_j}h_i$，代入得 $\widetilde{\mathcal L}^{(t)} = \sum_{j=1}^T \left[ G_jw_j+\frac12(H_j+\lambda)w_j^2 \right]+\gamma T$。

### 2.3 最优叶权重与分裂增益

**※【定理】** XGBoost 叶节点最优权重：对目标函数求导并令为 0，得 $w_j^*=-\frac{G_j}{H_j+\lambda}$。此解析解可直接求出叶节点最优权重。

**※【定理】** XGBoost 分裂增益(Gain)：将最优权重代回，计算分裂前后的目标函数下降量：
$\operatorname{Gain} = \frac12\left[ \frac{G_L^2}{H_L+\lambda} + \frac{G_R^2}{H_R+\lambda} - \frac{G_P^2}{H_P+\lambda} \right]-\gamma$
Gain 越大说明分裂越有价值，若最大 Gain 不大于 0 则停止分裂。

### 2.4 XGBoost 计算例题

**※【例题】** XGBoost 树构建。
数据集：A(1,0), B(2,0), C(3,1), D(4,1)。平方误差损失，$F_0(x)=0.5$，$\lambda=1, \gamma=0, \eta=1$。
1. **计算梯度**：$g_i=\hat y_i-y_i$，$h_i=1$。得 A(0.5,1), B(0.5,1), C(-0.5,1), D(-0.5,1)。$G_P=0, H_P=4$。
2. **枚举分裂点**（如 $x\leq2.5$）：左节点 A、B，$G_L=1, H_L=2$；右节点 C、D，$G_R=-1, H_R=2$。计算 Gain $=\frac12[\frac{1^2}{3}+\frac{(-1)^2}{3}-0]\approx0.333$。其他分裂点 Gain 更小，故选 $x\leq2.5$。
3. **计算叶节点权重**：$w_L^*=-\frac{1}{3}$，$w_R^*=\frac{1}{3}$。
4. **更新预测**：A、B 预测更新为 $0.5-1/3=0.1667$；C、D 更新为 $0.5+1/3=0.8333$。MSE 从 0.25 降至 0.0278。

XGBoost 还支持 `subsample`（样本抽样）、`colsample_bytree`（特征抽样）、`tree_method`（建树算法，如 `hist`）等优化。

---

## 三、LightGBM：更快的构造决策树

LightGBM (Light Gradient Boosting Machine) 的核心优化在于寻找分裂点、使用样本以及生长策略。

### 3.1 直方图算法 (Histogram)

**※【定义】** 直方图算法(Histogram-based Algorithm)：将连续特征离散化为有限数量的箱(Bins)。对第 $b$ 个 Bin 记录 $G_b=\sum_{i:x_i\in B_b}g_i$ 和 $H_b=\sum_{i:x_i\in B_b}h_i$。寻找最佳分裂点只需扫描 $O(B)$ 个箱，而非 $O(N)$ 个样本。此外可借助父节点直方图减去子节点直方图进行加速（直方图相减）。

### 3.2 叶子优先生长 (Leaf-wise)

**※【定义】** 叶子优先生长(Leaf-wise Growth)：与传统按层生长(Level-wise)不同，LightGBM 在所有当前叶节点中比较最佳分裂 Gain，优先分裂 Gain 最大的叶节点。这能更快降低训练损失，但更容易过拟合，需限制深度或叶节点数。

### 3.3 GOSS 与 EFB

**※【定义】** 基于梯度的单边采样(Gradient-based One-Side Sampling, GOSS)：保留梯度绝对值最大的 $aN$ 个样本，在剩余小梯度样本中随机抽取 $bN$ 个，并乘以补偿权重 $\frac{1-a}{b}$，从而在不损失过多精度的情况下加速计算。

**※【定义】** 互斥特征捆绑(Exclusive Feature Bundling, EFB)：将从未同时非零的互斥特征捆绑成一列，降低有效特征维度，特别适用于稀疏特征。

### 3.4 LightGBM 计算例题

**※【例题】** LightGBM 树构建与 Leaf-wise 策略。
数据：A(1,0), B(2,0), C(3,0), D(4,1), E(5,1), F(6,3)。$\lambda=0, \gamma=0$。
1. 初始预测均值 $F_0 = 5/6$。梯度 $g_i = 5/6 - y_i$，$h_i=1$。$G_P=0, H_P=6$。
2. 假设分为 4 个 Bin。计算各 Bin 的 $G, H$。
3. 尝试在 Bin 间分裂。例如在 Bin 3 后分裂 ($x\leq5$)，左节点 $G_L=13/6, H_L=5$；右节点 $G_R=-13/6, H_R=1$。Gain 达 2.8167，为最大，确定根节点分裂。
4. 此时产生左节点（5个样本）和右节点（仅1个样本）。Leaf-wise 策略选择继续对左节点进行分裂。将左节点分为 $x\leq3$ 和 $3<x\leq5$，继续计算最大增益并分裂。
5. 最终完全拟合，预测值分别更新为 0, 1, 3。

---

## 四、核心比较与补充

### 4.1 核心比较

| 维度 | XGBoost | LightGBM |
| :--- | :--- | :--- |
| 优化信息 | 一阶梯度、二阶 Hessian | 一阶梯度、二阶 Hessian |
| 分裂点搜索 | exact, approx, hist 等 | 以 Histogram 为核心 |
| 典型生长策略 | Depth-wise（按层） | Leaf-wise（按叶） |
| 正则化 | L1、L2、分裂惩罚等 | L1、L2、分裂约束等 |
| 样本采样 | 随机采样等 | Bagging 或 GOSS |
| 特征处理 | 特征采样等 | Histogram、EFB等 |

### 4.2 三大核心数学机制

1. 加法累加：$F_t(\mathbf x)=F_{t-1}(\mathbf x)+\eta f_t(\mathbf x)$
2. 叶节点最优权重（无 L1、无额外约束）：$w_j^*=-\frac{G_j}{H_j+\lambda}$
3. 分裂增益：$\operatorname{Gain} = \frac12 \left[ \frac{G_L^2}{H_L+\lambda} + \frac{G_R^2}{H_R+\lambda} - \frac{G_P^2}{H_P+\lambda} \right]-\gamma$

### 4.3 二分类场景下的应用

**※【定义】** 二分类中的交叉熵与梯度：设真实标签 $y_i\in\{0,1\}$，模型输出 Logit $z_i$，概率 $p_i=\sigma(z_i)=\frac{1}{1+e^{-z_i}}$。单样本交叉熵损失 $L(y_i,z_i) = -y_i\log p_i-(1-y_i)\log(1-p_i)$。
对其求一阶导得 $g_i=p_i-y_i$，二阶导得 $h_i=p_i(1-p_i)$。

每棵树实际上输出对 Logit 的修正，最终预测概率为 $P(y_i=1\mid\mathbf x_i)=\sigma(z_i)$。