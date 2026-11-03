## 5.1 Simple Linear Regression

给定输入数据集 $(X,y) = \{(x_1,y_1),(x_2,y_2),\ldots,(x_n,y_n)\}$，
目标是仅使用 $x$ 的值来预测 $y$ 的值。

$x$ 是解释变量(explanatory variable)，$y$ 是因变量(dependent variable)。

我们将一条直线“拟合”到数据上，模型形式为 $y = \ell(x) = ax + b$，其中 $a$（斜率）和 $b$（截距）是参数。目标不仅是插值，还要做预测。
对于每个 $x_i$，预测值为 $\hat{y}_i = \ell(x_i)$。

**※【定义】** 残差(residual)
为 $r_i = y_i - \hat{y}_i = y_i - \ell(x_i)$。残差不是点到直线的距离，而是到直线上同 $x$ 值的点的有符号距离。
整体误差用误差平方和(Sum of Squared Errors, SSE)表示：
$\mathrm{SSE}((X,y),\ell) = \sum_{i=1}^n r_i^2 = \sum_{i=1}^n (y_i - \ell(x_i))^2$。

1. SSE是凸的
2. SSE是可微的
3. 有显式解

要求 $X^TX$ 是可逆的。则有$X^TX\,\begin{bmatrix}a \\  b\end{bmatrix}=X^TY$.

求解最优直线 $\ell$：
计算均值 $\bar{x} = \frac{1}{n}\sum_{i=1}^n x_i$ 和 $\bar{y} = \frac{1}{n}\sum_{i=1}^n y_i$，
中心化向量 $\bar{X} = (x_1 - \bar{x}, \ldots, x_n - \bar{x})$，$\bar{Y} = (y_1 - \bar{y}, \ldots, y_n - \bar{y})$。
1. 设置 $a = \langle \bar{Y}, \bar{X} \rangle / \|\bar{X}\|^2$
2. 设置 $b = \bar{y} - a\bar{x}$

**几何直观**：$b$ 确保直线通过数据中心 $(\bar{x}, \bar{y})$。斜率 $a = \frac{\|\bar{Y}\|}{\|\bar{X}\|}\cos\theta$，其中 $\theta$ 是 $\bar{X}$ 和 $\bar{Y}$ 的夹角。$\frac{\|\bar{Y}\|}{\|\bar{X}\|}$ 捕捉了变化率，$\cos\theta$ 捕捉了归一化后的相关性。

## 5.2 Linear Regression with Multiple Explanatory Variables

数据点包含 $d$ 个解释变量 $x_i = (x_{i,1},\ldots,x_{i,d}) \in \mathbb{R}^d$ 和一个因变量 $y_i \in \mathbb{R}$。
模型为：$\hat{y}_i = M_{\alpha}(x_i) = \alpha_0 + \sum_{j=1}^d \alpha_j x_{i,j} = \langle \alpha, (1, x_i) \rangle$
其中 $\alpha = (\alpha_0, \alpha_1, \ldots, \alpha_d)$，$\alpha_0$ 替换了截距 $b$。

为了最小化 $\mathrm{SSE}(X,y,M) = \sum_{i=1}^n (y_i - M(x_i))^2$，
构造 $n \times (d+1)$ 的数据矩阵 $\tilde{X} = [1, X_1, X_2, \ldots, X_d]$。
系数计算如下：$\alpha = (\tilde{X}^T \tilde{X})^{-1} \tilde{X}^T y$

**法向方程的几何(Geometry of the Normal Equations)**：
$S(\alpha) = \mathrm{SSE}$ 是凸二次函数。
求导置零得：
$\sum_{i=1}^n X_{i,j} \langle x_i, \alpha \rangle = \sum_{i=1}^n X_{i,j} y_i \implies (X^T X)\alpha = X^T y$。
几何上，这意味着残差向量 $r = y - X\alpha$ 与预测向量 $\hat{y} = X\alpha$ 正交：$\langle r, \hat{y} \rangle = 0$。由于 $\hat{y}$ 限制在 $X$ 的列空间内，最优解使得 $r$ 垂直于该子空间。

**※【定义】** 逻辑回归
$(x_{i},y_{i}),x_{i}\in \mathbb{R},y_{i}\in\{0,1\}$. 
$\mathscr{L}(x,y,\alpha)=-\sum_{i=1}^n[y_{i}\ln p(x_{i})+(1-y_{i})\ln(1-p(x_{i}))]$. 
损失函数基于KL散度$d_{KL}=\sum_{i=1}^np_{i}\ln \frac{p _{i}}{q_{i}}$导出. 对于特定$y_{i}$，概率是固定的.

## 5.3 Polynomial Regression

当线性关系不足以捕捉数据模式时，建立多项式模型：
$\hat{y} = M_p(x) = \alpha_0 + \alpha_1 x + \alpha_2 x^2 + \ldots + \alpha_p x^p$

对于数据点，构建特征向量 $v = (1, x, x^2, \ldots, x^p)$，从而生成 $n \times (p+1)$ 的数据矩阵 $\tilde{X}_P$。求解方法与多元线性回归相同：$\alpha = (\tilde{X}_P^T \tilde{X}_P)^{-1} \tilde{X}_P^T y$.

随着 $p$ 增大，拟合会越来越好，当 $p \ge n-1$ 时，曲线完美穿过所有点（插值），但这不利于预测新数据（过拟合）。

## 5.4 Cross-Validation

回归的最终目标是泛化(generalization)，即对未知新数据的预测能力。
交叉验证通过将数据随机划分为训练集(training data)和测试集(test data)来评估泛化能力。
构建模型：$\alpha = (X_{\mathrm{train}}^T X_{\mathrm{train}})^{-1} X_{\mathrm{train}}^T y_{\mathrm{train}}$。
评估模型：$\mathrm{SSE}(X_{\mathrm{test}}, y_{\mathrm{test}}, M_{\alpha}) = \sum_{(x_i,y_i) \in \text{test}} (y_i - \langle (1;x_i), \alpha \rangle)^2$。

注意，不能将同一测试集同时用于选择模型参数和评估泛化误差。选择最优 $p$ 时，在训练集上计算不同 $p$ 的模型，在测试集上计算 SSE，选择 SSE 最小的 $p$。

**※【例题】** 带交叉验证的简单多项式示例
将数据分为训练集和测试集，拟合 $p = \{1,2,3,4,5,8\}$ 的多项式。计算测试集上的 SSE，发现 $p=2$ 的模型 SSE 最低（0.916），且模型最简洁，故选择 $p=2$。

**留一交叉验证(Leave-one-out Cross-Validation, LOOCV)**：
当数据量很小，不想浪费训练数据时。创建 $n$ 个训练集，每个包含 $n-1$ 个点，留出 1 个点作为测试集。计算平均误差 $E = \frac{1}{n}\sum_{i=1}^n E_i$。计算成本高。

**其他评估指标**：决定系数 $R^2 = 1 - \frac{SS_{\mathrm{tot}}}{SS_{\mathrm{reg}}}$（注：原文公式似有颠倒，通常 $R^2 = 1 - \frac{SS_{\mathrm{res}}}{SS_{\mathrm{tot}}}$）。平均绝对误差 $\mathrm{MAE} = \sum |r_i|$。

## 5.5 正则化回归(Regularized Regression)

当维度 $d$ 大于数据点 $n$ 时，普通最小二乘法（OLS）无解或极不稳定。
**高斯-马尔可夫定理(Gauss-Markov Theorem)**：在残差方差最小、无预期误差且残差不相关的条件下，OLS 模型 $\alpha = (X^T X)^{-1} X^T y$ 是最佳的。正则化通过引入偏差(bias)打破“无预期误差”假设，来提升泛化能力。

### 5.5.1 岭回归的吉洪诺夫正则化(Tikhonov Regularization for Ridge Regression)

引入正则化参数 $s > 0$，新的成本函数为：$W_{\circ}(X,y,\alpha,s) = \frac{1}{n}\|X\alpha - y\|_2^2 + s\|\alpha\|_2^2$.
加入 $L_2$ 正则项 $s\|\alpha\|_2^2$ 会偏向于较小的 $\alpha$。小斜率模型更稳定，对 $x$ 的微小变化不敏感。（换言之，原本的线性回归对异常值的敏感度是比较高的）
解为：$\alpha_s^{\circ} = (X^T X + snI)^{-1} X^T y$
项 $(s/n)I$ 增加了矩阵的所有特征值，使得当 $X^T X$ 不满秩或特征值极小时，求逆变得数值稳定。

*※【例题】* 对 $X=\begin{bmatrix}1 &  &   & \\ & \frac{1}{2} &  &  \\  &  & \dots &  \\  &  &  & \frac{1}{2^{10}}  \end{bmatrix}$ 不做修正 $X^TX$会非常之大。但是$s=0.01,n=100$，$(X^TX+nsI)$ 就能被改善很多。

**※【例题】** 岭回归改进泛化
对比 OLS ($s=0$) 和岭回归 ($s=0.01$)，在测试集上，带正则化的模型 SSE (361.30) 低于无正则化模型 (524.05)。

*※【例题】* $W_{\circ}(X,y,\alpha,s) = \frac{1}{n}\|X\alpha - y\|_2^2 + s\|\alpha\|_2^2$
$=\frac{1}{n}\|U^TX\alpha  - U^Ty\|_2^2 + s\|V^T\alpha\|_2^2=\frac{1}{n}\|\Sigma V^T\alpha  - U^Ty\|_2^2 + s\|V^T\alpha\|_2^2=\frac{1}{n}\|\Sigma \tilde{\alpha}  - \tilde{y}\|_2^2 + s\|\tilde{\alpha}\|_2^2$.
又可以转化到对角阵的上。
### 5.5.2 套索(Lasso)

将 $L_2$ 惩罚项替换为 $L_1$ 惩罚项：$W_{\circ}(X,y,\alpha,s) = \frac{1}{n}\|X\alpha - y\|_2^2 + s\|\alpha\|_1$
这种形式不仅具有岭回归的稳健性，还能**产生稀疏解(sparse solutions)**。
当 $s$ 足够大时，许多系数 $\alpha_j$ 会恰好变为 0。
这对于高维数据的变量选择(variable selection)和模型解释非常有用。
但是 Lasso 没有显式解。

### 5.5.3 对偶约束形式(Dual Constrained Formulation)

等价形式为在约束下最小化 OLS：
岭回归：$\alpha_t^{\circ} = \arg\min_{\alpha}\|X\alpha - y\|_2^2 \quad \text{such that} \quad \|\alpha\|_2^2 \leq t$
套索：$\alpha_t^{\circ} = \arg\min_{\alpha}\|X\alpha - y\|_2^2 \quad \text{such that} \quad \|\alpha\|_1 \leq t$

**套索稀疏模型的几何解释**：
在 2D 空间 ($d=2$) 中，约束条件形成原点周围的等值面。OLS 解的等值线是围绕最优点的椭圆。$L_2$ 约束是圆形，$L_1$ 约束是菱形（带有尖角）。由于菱形的尖角恰好位于坐标轴上，椭圆往往首先与尖角相切，使得某个系数恰好为 0。在高维空间中，菱形更加尖锐，更容易产生稀疏解。

### 5.5.4 匹配追踪(Matching Pursuit, MP)

针对套索等无闭式解的问题，MP 提供了一种贪心算法。它迭代地揭示数据矩阵中能捕捉因变量趋势的特征子集。
初始化残差 $r = y$，系数 $\alpha_j = 0$。每次迭代：
1. 找到与残差最相关的列：$j^* = \arg\max_j |\langle r, X_j \rangle|$.
2. 求解该维度上的最优系数：$\alpha_{j^*} = \arg\min_{\gamma} \|r - X_j\gamma\|^2 + s|\gamma|$.
3. 解得$\alpha _{j}=\frac{1}{X_{1}^TX_{1}}\left( \langle r, X_j \rangle-\text{sign}(\langle r, X_j \rangle) \frac{s}{2} \right)$.
4. 更新残差：$r = r - X_j\alpha_{j^*}$.
重复直到选择 $k$ 个特征或残差足够小。正交匹配追踪(Orthogonal Matching Pursuit)在每次更新后会重新拟合所有已选特征的系数，更不易陷入局部最优。

**※【例题】** 信号恢复
$n=5$ 个数据点，$d=7$ 个维度，真实模型含有少量非零值。运行 OLS 会失败。使用岭回归会得到稠密解。使用 MP 算法，第一步识别出相关性最大的 $j=4$，并解出 $\alpha_4 \approx 5.47$，更新残差；第二步选择 $j=6$，解出 $\alpha_6 \approx -1.93$；第三步再次选择 $j=4$ 修正。最终有效系数接近真实模型 $[0,0,0,5,0,-2,0]$。

**压缩感知(Compressed Sensing)**：
从少量观测数据 $n \ll d$ 中恢复稀疏信号 $s$。模型为 $y = Xs$。使用匹配追踪，每次选择与残差 $r$ 内积最大的列 $X_j$，假设该位置信号为 1，并从 $y$ 中减去该列的影响。重复直到残差为 0 或达到迭代次数。

**非可比列与归一化**：正则化算法直接比较列向量的范数。如果列代表不同单位的特征（如身高和体重），结果会依赖单位的选择。因此，必须对输入矩阵的列进行标准化或中心化处理，确保各项具有可比性。