
本章讨论优化一般函数 $f: \mathbb{R}^d \to \mathbb{R}$。这种函数通常用于描述具有 $d$ 个参数的模型的“代价”（如多项式回归），目标是找到使代价最小的参数 $\alpha = (\alpha_1, \alpha_2, \ldots, \alpha_d)$。虽然某些特殊情况可以求精确解，但在很多情况下不行，此时我们通常使用梯度下降法：试图沿着使函数值下降最快的方向“走”，直到无法继续下降为止。

## 6.1 函数(Functions)

令 $B_r(\alpha)$ 定义以点 $\alpha \in \mathbb{R}^d$ 为中心、半径为 $r$ 的欧几里得球(Euclidean ball)，即包含所有满足 $\|\alpha - p\| \leq r$ 的点 $p$。

**※【定义】** 局部最大值(Local Maximum)与局部最小值(Local Minimum)：
- **局部最大值**：存在邻域 $B_r(\alpha)$，使得对所有 $p \in B_r(\alpha)$，有 $f(p) \leq f(\alpha)$。
- **局部最小值**：存在邻域 $B_r(\alpha)$，使得对所有 $p \in B_r(\alpha)$，有 $f(p) \geq f(\alpha)$。
- 如果对 $p \in B_r(\alpha), p \neq \alpha$ 去掉“等于”的条件，则称为**严格局部最大值/最小值**。

**※【定义】** 全局最大值(Global Maximum)与全局最小值(Global Minimum)：
- **全局最大值**：对于所有 $p \in \mathbb{R}^d$，有 $f(p) \leq f(\alpha)$。
- **全局最小值**：对于所有 $p \in \mathbb{R}^d$，有 $f(p) \geq f(\alpha)$。
- 如果全局最小值或最大值只有一个点，则称为**严格的**。
（注：仅使用“最小值”或“最大值”时，通常指代局部。）

若函数在闭有界子集 $S$ 上连续，则其必定在 $S$ 上存在全局最小值和全局最大值（可能在边界上）。对于 $d>1$，**鞍点(Saddle point)** $\alpha$ 是指在任意邻域 $B_r(\alpha)$ 内，既存在使 $f(p) < f(\alpha)$ 的点，也存在使 $f(p') > f(\alpha)$ 的点，且上下区域不连通。如果上下区域各自连通且不是极值点，则称为**正则点(Regular point)**。

**※【例题】** 连续函数
- 第一个函数有局部最小值和最大值。
- 第二个函数是常数，每个点既是全局最小值也是全局最大值。
- 第三个函数是凸函数(Convex function)，两点之间的线段始终位于函数图像上方，满足 $f(\lambda p + (1-\lambda)q) \leq \lambda f(p) + (1-\lambda)f(q)$ 对所有 $\lambda \in [0, 1]$。

**※【定义】** 凸函数(Convex Function)与严格凸函数(Strictly Convex Function)：
对于任意两点 $p, q \in \mathbb{R}^d$ 和 $\lambda \in [0,1]$，如果满足 $f(\lambda p + (1-\lambda)q) \leq \lambda f(p) + (1-\lambda)f(q)$，则函数是凸的。去掉“等于”号则为严格凸。
（性质：凸函数的和、最大值函数均为凸函数。）

**※【定理】** 凸性与全局最小值：
凸函数的任何局部最小值也是全局最小值。严格凸函数至多有一个最小值（即唯一的全局最小值）。这意味着如果我们找到了一个最小值，我们就找到了全局最小值。

## 6.2 梯度(Gradients)

**※【定义】** 方向导数(Directional Derivative)：对于单位向量 $u$，方向导数定义为 $\nabla_u f(\alpha) = \lim_{h \to 0} \frac{f(\alpha + hu) - f(\alpha)}{h}$。

**※【定义】** 梯度(Gradient)：对于可微函数 $f$，其梯度定义为各坐标偏导数构成的向量：
$\nabla f = (\frac{\mathrm{d}f}{\mathrm{d}\alpha_1}, \frac{\mathrm{d}f}{\mathrm{d}\alpha_2}, \dots, \frac{\mathrm{d}f}{\mathrm{d}\alpha_d})$。
$\nabla f$ 是一个从 $\mathbb{R}^d \to \mathbb{R}^d$ 的函数。

**※【例题】** 计算梯度
对于 $\alpha = (x,y,z) \in \mathbb{R}^3$，函数 $f(x,y,z) = 3x^2 - 2y^3 - 2xe^z$。
则 $\nabla f = (6x - 2e^z, -6y^2, -2xe^z)$。在点 $(3, -2, 1)$ 处的梯度为 $(18 - 2e, -24, -6e)$。

**线性近似(Linear Approximation)**：
方向导数可由梯度求得：$\nabla_u f(\alpha) = \langle \nabla f(\alpha), u \rangle$。
这表示梯度描述了函数在点 $\alpha$ 处的线性近似。$\nabla_u f(\alpha)$ 给出了切平面在任何方向 $u$ 上的斜率。
因此，函数在点 $\alpha$ 处增加最快的方向是使 $\langle \nabla f(\alpha), u \rangle$ 最大的单位向量 $u$，即归一化梯度 $\overline{\nabla f(\alpha)}$。要寻找函数的最小值，我们应向着 $-\overline{\nabla f(\alpha)}$ 方向移动（最速下降方向）。

## 6.3 梯度下降(Gradient Descent)

梯度下降是一类迭代技术，用于寻找可微函数 $f: \mathbb{R}^d \to \mathbb{R}$ 的最小值或最优参数 $\alpha^*$。这在 $f$ 是凸函数且没有闭式解时非常有效。

**※【算法】** 梯度下降(Gradient Descent)：
初始化 $\alpha^{(0)} = \alpha_{start} \in \mathbb{R}^d$；$k=0$。
重复：
$\alpha^{(k+1)} := \alpha^{(k)} - \gamma_k \nabla f(\alpha^{(k)})$
$k = k + 1$
直到 $\|\nabla f(\alpha^{(k)})\| \leq \tau$。
返回 $\alpha^{(k)}$。

**停止条件(Stopping Condition)**：参数 $\tau$ 是容差。在极小值点 $\alpha^*$ 处梯度为 0，所以当 $\alpha$ 接近极小值时，梯度范数应当很小。常用 $\|\nabla f\|$ 作为代理指标。有时也使用固定步数 $T$。

**6.3.1 学习率(Learning Rate)**
最关键参数 $\gamma$ 控制算法移动的步长。若 $\gamma$ 太大，接近极小值时可能越过极小值而不收敛；若太小，收敛速度太慢。
**Lipschitz 界**：函数 $g$ 是 $L$-Lipschitz 的，如果对所有 $p,q$，有 $\|g(p) - g(q)\| \leq L\|p - q\|$。如果梯度 $\nabla f$ 是 $L$-Lipschitz 的，且设置 $\gamma \leq 1/L$，梯度下降将收敛到驻点。对于凸函数，经过最多 $k = C/\varepsilon$ 步可保证 $f(\alpha^{(k)}) - f(\alpha^*) \leq \varepsilon$。
**强凸函数(Strongly Convex Functions)**：函数 $f$ 是 $\eta$-强凸的，如果 $f(p) \geq f(\alpha) + \langle \nabla f(\alpha), p - \alpha \rangle + \frac{\eta}{2}\|p - \alpha\|^2$。这意味着 $f$ 被一个二次函数下界约束。
如果 $f$ 是 $\eta$-强凸且梯度是 $L$-Lipschitz 的，设置 $\gamma \leq 2/(\eta+L)$，只需 $k = C\log(1/\varepsilon)$ 步即可达到误差 $\varepsilon$。这被称为**线性收敛(Linear Convergence)**（因为对数误差随步数线性下降），每步可以解析出常数个精度位数。常数 $C$ 取决于条件数 $L/\eta$。

**线搜索(Line Search)**：
如果不知道 Lipschitz 界，可以动态寻找最优 $\gamma_k$。每次计算梯度后，问题降维到一维。可以使用**黄金分割搜索(Golden Section Search)**：将区间 $[b,t]$ 分为三份，根据函数值比较排除不可能包含最小值的区间，不断递归缩小范围。
**可调学习率(Adjustable Rate)**：在实践中，**回溯线搜索(Backtracking Line Search)** 自动调整 $\gamma$。使用固定参数 $\beta \in (0,1)$（如 0.75），初始 $\gamma$ 设得较大。在每步 $\alpha$ 处，若 $f(\alpha - \gamma \nabla f(\alpha)) > f(\alpha) - \frac{\gamma}{2}\|\nabla f(\alpha)\|^2$，则更新 $\gamma = \beta \gamma$。这会缩小 $\gamma$ 直到满足条件，确保学习率不会太大。

**※【例题】** 固定学习率的梯度下降
考虑 $f(x,y) = (\frac{3}{4}x - \frac{3}{2})^2 + (y - 2)^2 + \frac{1}{4}xy$，梯度 $\nabla f(x,y) = (\frac{9}{8}x - \frac{9}{4} + \frac{1}{4}y, 2y - 4 + \frac{1}{4}x)$。
从初始点 $(5,4)$ 开始，运行 10 次迭代：
- 若 $\gamma$ 极小（0.01），算法无法接近极小值。
- 若 $\gamma$ 太大（0.5），几乎在第一步就会越过极小值并在周围跳跃，不收敛。
- 若 $\gamma = 0.3$，曲线平滑且较好地收敛到梯度范数极小的点。

## 6.4 拟合模型到数据(Fitting a Model to Data)

在数据分析中，梯度下降常用于将模型拟合到数据。给定数据集 $(X,y) = \{(x_1,y_1), \dots, (x_n,y_n)\}$ 和由参数 $\alpha$ 定义的模型族 $M_\alpha$，定义损失函数 $L((X,y), M_\alpha)$ 衡量预测值与真实值的差异。目标函数为 $f(\alpha) = L((X,y), M_\alpha)$。例如，可以使用误差平方和 $f(\alpha) = \mathrm{SSE} = \sum_{i=1}^n (y_i - M_\alpha(x_i))^2$ (6.1)。

**6.4.1 回归的最小均方更新(Least Mean Squares Updates)**
以简单二次回归为例，参数 $k=3$，$\alpha = (\alpha_0, \alpha_1, \alpha_2)$，模型为 $M_\alpha(x_i) = \alpha_0 + \alpha_1 x_i + \alpha_2 x_i^2 = \langle \alpha, (1, x_i, x_i^2) \rangle$。
对于单个数据点 $(x_1, y_1)$，代价 $f_1(\alpha) = (M_\alpha(x_1) - y_1)^2$。
对 $\alpha_j$ 求偏导：$\frac{\mathrm{d}}{\mathrm{d}\alpha_j} f_1(\alpha) = 2(M_\alpha(x_1) - y_1) x_1^j$。
梯度为：$\nabla f(\alpha) = 2(M_\alpha(x_1) - y_1)(1, x_1, x_1^2)$。
按照 $\alpha := \alpha - \gamma \nabla f(\alpha)$ 进行更新，这被称为**最小均方(Least Mean Squares, LMS)** 更新规则或 **Widrow-Hoff 学习规则**。更新的幅度与残差 $(M_\alpha(x_1) - y_1)$ 成正比：误差大步伐大，误差小步伐小。

**6.4.2 可分解函数(Decomposable Functions)**
当有多个数据点 $n > 1$ 时，利用代价函数的可分解性：$f(\alpha) = \sum_{i=1}^n f_i(\alpha)$。由于每个 $f_i$ 是凸的，它们的和 $f$ 也是凸的（如果特征向量满秩，通常是强凸的）。
**批量梯度下降(Batch Gradient Descent)**：
直接扩展单点的情况，利用导数的线性性质：
$\nabla f(\alpha) = 2 \sum_{i=1}^n (M_\alpha(x_i) - y_i) (1, x_i, x_i^2)$。
此方法每步需计算所有 $n$ 个数据点的梯度，时间复杂度为 $O(n)$，对于大数据集较慢。

**随机与增量梯度下降(Stochastic and Incremental Gradient Descent)**：
为了避免每步计算全量梯度，**增量梯度下降**每次只计算一个数据点 $i$ 的梯度 $\nabla f_i(\alpha) = 2(M_\alpha(x_i) - y_i)(1, x_i, x_i^2)$。
更常见的变体是**随机梯度下降(Stochastic Gradient Descent, SGD)**：每次随机选择一个数据点计算梯度并更新。
在超大数据集上，这比批量方法快得多，因为每次迭代只需常数时间。虽然它不能完美继承凸函数的收敛保证，但在远离最优解时收敛很快；在接近最优解时可能不会精确收敛，但能通过 PAC 风格的随机保证达到足够接近的最优点。对于极大数据集，往往在算法使用完所有数据点之前就已经收敛。