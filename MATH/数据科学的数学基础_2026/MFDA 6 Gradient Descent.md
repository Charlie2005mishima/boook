
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

若函数在闭有界子集 $S$ 上连续，则其必定在 $S$ 上存在全局最小值和全局最大值（可能在边界上）。
对于 $d>1$，**鞍点(Saddle point)** $\alpha$ 是指在任意邻域 $B_r(\alpha)$ 内，既存在使 $f(p) < f(\alpha)$ 的点，也存在使 $f(p') > f(\alpha)$ 的点，且上下区域不连通。如果上下区域各自连通且不是极值点，则称为**正则点(Regular point)**。

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

梯度下降是一类迭代技术，用于寻找可微函数 $f: \mathbb{R}^d \to \mathbb{R}$ 的最小值或最优参数 $\alpha^\star$。这在 $f$ 是凸函数且没有闭式解时非常有效。

**※【算法】** 梯度下降(Gradient Descent)：
初始化 $\alpha^{(0)} = \alpha_{start} \in \mathbb{R}^d$；$k=0$。
重复：$\alpha^{(k+1)} := \alpha^{(k)} - \gamma_k \nabla f(\alpha^{(k)})$
$k = k + 1$
直到 $\|\nabla f(\alpha^{(k)})\| \leq \tau$。
返回 $\alpha^{(k)}$。

**停止条件(Stopping Condition)**：参数 $\tau$ 是容差。在极小值点 $\alpha^\star$ 处梯度为 0，所以当 $\alpha$ 接近极小值时，梯度范数应当很小。常用 $\|\nabla f\|$ 作为代理指标。有时也使用固定步数 $T$。

### 6.3.1 学习率(Learning Rate)

最关键参数 $\gamma$ 控制算法移动的步长。若 $\gamma$ 太大，接近极小值时可能越过极小值而不收敛；若太小，收敛速度太慢。
**Lipschitz 界**：函数 $g$ 是 $L$-Lipschitz 的，如果对所有 $p,q$，有 $\|g(p) - g(q)\| \leq L\|p - q\|$。如果梯度 $\nabla f$ 是 $L$-Lipschitz 的，且设置 $\gamma \leq 1/L$，梯度下降将收敛到驻点。对于凸函数，经过最多 $k = C/\varepsilon$ 步可保证 $f(\alpha^{(k)}) - f(\alpha^\star) \leq \varepsilon$。
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

这里统一一下符号避免混淆：在优化中$,\eta$ 通常指学习率，所以你题目里的 $\eta_*$ 其实指代的是**最优解参数**（通常记为 $\theta^\star$），而 $\eta_t$ 指代的是第 $t$ 步的参数 $\theta_t$。学习率我们用 $\gamma$ 表示。

以下是最小二乘法梯度下降收敛性的完整证明笔记。

### 最小二乘法梯度下降收敛性证明

考虑线性最小二乘问题(Linear Least Squares)：
$F(\theta) = \frac{1}{2}\|X\theta - y\|_2^2$
其中 $X \in \mathbb{R}^{n \times d}$ 为数据矩阵$,y \in \mathbb{R}^n$ 为标签向量$,\theta \in \mathbb{R}^d$ 为待求参数。

**※【定义】** 海森矩阵(Hessian Matrix)：
对于目标函数 $F(\theta)$，其海森矩阵为二阶导数矩阵，记为 $H = \nabla^2 F(\theta) = X^T X$。由于 $X^T X$ 是半正定矩阵，故 $F(\theta)$ 为凸函数。

**※【定义】** 梯度下降(Gradient Descent)更新规则：
$\theta_{t+1} = \theta_t - \gamma \nabla F(\theta_t)$，其中 $\gamma > 0$ 为学习率$,\nabla F(\theta) = X^T(X\theta - y) = H\theta - X^T y$。

**※【定理】** 最优解条件：设最优解为 $\theta^\star$，由于 $F(\theta)$ 是凸函数，最优解满足 $\nabla F(\theta^\star) = 0$，即 $H\theta^\star = X^T y$。

**※【引理】** 目标函数在任意点 $\theta$ 与最优解 $\theta^\star$ 的差值，可以表示为：
$F(\theta) - F(\theta^\star) = \frac{1}{2}(\theta - \theta^\star)^T H (\theta - \theta^\star)$

**证明：**
展开 $F(\theta)$：
$\begin{align}F(\theta)  & = \frac{1}{2}(X\theta - y)^T(X\theta - y)   \\  & = \frac{1}{2}\theta^T X^T X \theta - y^T X \theta + \frac{1}{2}y^T y  \\  &= \frac{1}{2}\theta^T H \theta - y^T X \theta + \frac{1}{2}y^T y\end{align}$.

代入最优解条件 $X^T y = H\theta^\star$：
$F(\theta) = \frac{1}{2}\theta^T H \theta - (H\theta^\star)^T \theta + \frac{1}{2}y^T y = \frac{1}{2}\theta^T H \theta - (\theta^\star)^T H \theta + \frac{1}{2}y^T y$.
同时计算最优点的函数值：
$F(\theta^\star) = \frac{1}{2}(\theta^\star)^T H \theta^\star - (\theta^\star)^T H \theta^\star + \frac{1}{2}y^T y = -\frac{1}{2}(\theta^\star)^T H \theta^\star + \frac{1}{2}y^T y$.
将两式相减：$F(\theta) - F(\theta^\star) = \frac{1}{2}\theta^T H \theta - (\theta^\star)^T H \theta + \frac{1}{2}(\theta^\star)^T H \theta^\star$.
配方并利用 $H$ 的对称性：$F(\theta) - F(\theta^\star) = \frac{1}{2}(\theta - \theta^\star)^T H (\theta - \theta^\star)$.
得证。

**※【引理】** 第 $t$ 步迭代的参数 $\theta_t$ 与最优解 $\theta^\star$ 的误差，满足：
$\theta_t - \theta^\star = (I - \gamma H)^t (\theta_0 - \theta^\star)$

**证明：**
根据梯度下降更新规则：
$\theta_{t+1} = \theta_t - \gamma \nabla F(\theta_t) = \theta_t - \gamma(H\theta_t - X^T y)$
代入 $X^T y = H\theta^\star$：
$\theta_{t+1} = \theta_t - \gamma H\theta_t + \gamma H\theta^\star$
两边同时减去 $\theta^\star$：
$\theta_{t+1} - \theta^\star = \theta_t - \theta^\star - \gamma H(\theta_t - \theta^\star) = (I - \gamma H)(\theta_t - \theta^\star)$
这是一个递推关系，展开 $t$ 步即可得到：
$\theta_t - \theta^\star = (I - \gamma H)^t (\theta_0 - \theta^\star)$
得证。

**※【定理】** 对于最小二乘问题，若学习率 $\gamma$ 满足 $0 < \gamma < \frac{2}{\lambda_{\max}(H)}$ （其中 $\lambda_{\max}(H)$ 为矩阵 $H$ 的最大特征值），则梯度下降算法线性收敛，即当 $t \to \infty$ 时$,\theta_t \to \theta^\star$，且 $F(\theta_t) \to F(\theta^\star)$。

**证明：**
由于 $H = X^T X$ 是实对称矩阵，可正交对角化：$H = Q \Lambda Q^T$，其中 $Q$ 是正交矩阵（$Q^T Q = I$）$,\Lambda = \text{diag}(\lambda_1, \lambda_2, \dots, \lambda_d)$ 是特征值构成的对角矩阵，且 $\lambda_i \ge 0$。

根据引理二：
$\theta_t - \theta^\star = (I - \gamma H)^t (\theta_0 - \theta^\star) = (Q I Q^T - \gamma Q \Lambda Q^T)^t (\theta_0 - \theta^\star) = Q (I - \gamma \Lambda)^t Q^T (\theta_0 - \theta^\star)$

定义对角矩阵 $D = I - \gamma \Lambda$，其对角元素为 $d_i = 1 - \gamma \lambda_i$。
为了让误差收敛到零，即 $\lim_{t \to \infty} (I - \gamma H)^t = 0$，矩阵 $D^t$ 的所有对角元素必须趋于零。这要求对所有的 $i$，有 $|1 - \gamma \lambda_i| < 1$。
即：
$-1 < 1 - \gamma \lambda_i < 1$
$0 < \gamma \lambda_i < 2$
对于所有特征值 $\lambda_i > 0$，必须满足 $\gamma < \frac{2}{\lambda_i}$。即 $\gamma < \frac{2}{\lambda_{\max}(H)}$。
在此条件下，当 $t \to \infty$ 时$,\theta_t \to \theta^\star$。

接下来证明函数值也收敛。
根据引理一，函数值误差：
$F(\theta_t) - F(\theta^\star) = \frac{1}{2} (\theta_t - \theta^\star)^T H (\theta_t - \theta^\star)$
代入引理二的误差表达式：
$F(\theta_t) - F(\theta^\star) = \frac{1}{2} (Q (I - \gamma \Lambda)^t Q^T (\theta_0 - \theta^\star))^T (Q \Lambda Q^T) (Q (I - \gamma \Lambda)^t Q^T (\theta_0 - \theta^\star))$
由于 $Q^T Q = I$，化简为：
$F(\theta_t) - F(\theta^\star) = \frac{1}{2} (\theta_0 - \theta^\star)^T Q (I - \gamma \Lambda)^{2t} \Lambda Q^T (\theta_0 - \theta^\star)$
这是一个关于 $t$ 的指数衰减项。
设最大特征值为 $\lambda_{\max}$，则有：
$F(\theta_t) - F(\theta^\star) \le (1 - \gamma \lambda_{\min})^{2t} (F(\theta_0) - F(\theta^\star))$ （当 $H$ 正定时）
或者只要满足 $0 < \gamma < \frac{2}{\lambda_{\max}(H)}$，随着 $t \to \infty$$,(I - \gamma \Lambda)^{2t} \to 0$，因此 $F(\theta_t) \to F(\theta^\star)$。
得证。

**※【例题】** 假设一维二次函数 $F(\theta) = \frac{1}{2} \cdot 2 \cdot \theta^2$，即 $H = 2$（最大特征值为2），最优点 $\theta^\star = 0$。若初始点 $\theta_0 = 1$。
根据定理，学习率应满足 $0 < \gamma < \frac{2}{2} = 1$。
若取 $\gamma = 0.5$，则迭代变为：
$\theta_{t+1} - 0 = (1 - 0.5 \cdot 2)(\theta_t - 0) = 0 \cdot \theta_t = 0$。一步直接收敛。
若取 $\gamma = 0.9$，则 $\theta_{t+1} = (1 - 1.8)\theta_t = -0.8\theta_t$。虽然符号震荡，但绝对值以 $0.8$ 的比例缩小，最终收敛。
若取 $\gamma = 1.1$（超出范围），则 $\theta_{t+1} = (1 - 2.2)\theta_t = -1.2\theta_t$。此时 $|1 - \gamma \lambda| = 1.2 > 1$，误差会逐渐发散，无法收敛。

**※【定义】** 条件数(Condition Number)：对于半正定矩阵 $H$，若其最小特征值 $\mu > 0$，则定义条件数为 $\kappa = \frac{L}{\mu}$。当 $\mu=0$ 时，条件数趋于无穷。

**※【定理】** **收敛率界（Convergence Rate Bound）**
$F(\theta _{t})-F(\eta_{*})\leq \begin{cases}\left( 1-\frac{1}{\kappa} \right)^{2t}(F(\theta_{0})-F(\eta_{*})) & \mu>0 \\  \frac{1}{4t\gamma}\|\theta_{0}-\eta_{*}\|_{2}^2 & \mu=0\end{cases}$

**情况一：当 $\mu > 0$ 时（强凸情况）**
当 $H$ 满秩（即数据矩阵 $X$ 列满秩$,\mu > 0$）时，我们可以使用步长 $\gamma = \frac{1}{L}$。

**证明：**
根据特征值分解 $H = Q \Lambda Q^T$，有：
$F(\theta_t) - F(\theta^\star) = \frac{1}{2}(\theta_0 - \theta^\star)^T (I - \gamma H)^{2t} H (\theta_0 - \theta^\star)$
将其展开为特征值 $\lambda_i$ 的形式：
$F(\theta_t) - F(\theta^\star) = \frac{1}{2} \sum_{i=1}^d (1 - \gamma \lambda_i)^{2t} \lambda_i |q_i^T(\theta_0 - \theta^\star)|^2$
我们希望把 $(1 - \gamma \lambda_i)^{2t} \lambda_i$ 放大成一个与 $i$ 无关的常数倍。
因为 $\lambda_i \in [\mu, L]$，且 $\gamma = \frac{1}{L}$，所以 $1 - \gamma \lambda_i = 1 - \frac{\lambda_i}{L}$。
当 $\lambda_i = \mu$ 时，因子取得最大值 $1 - \frac{\mu}{L} = 1 - \frac{1}{\kappa}$。
因此，对于所有的 $i$，都有：
$(1 - \gamma \lambda_i)^{2t} \lambda_i \leq \left(1 - \frac{1}{\kappa}\right)^{2t} \lambda_i$
将其代回求和式中：
$F(\theta_t) - F(\theta^\star) \leq \left(1 - \frac{1}{\kappa}\right)^{2t} \frac{1}{2} \sum_{i=1}^d \lambda_i |q_i^T(\theta_0 - \theta^\star)|^2$
而后面那一坨恰好就是初始的误差：
$\frac{1}{2} \sum_{i=1}^d \lambda_i |q_i^T(\theta_0 - \theta^\star)|^2 = \frac{1}{2}(\theta_0 - \theta^\star)^T H (\theta_0 - \theta^\star) = F(\theta_0) - F(\theta^\star)$
得证：
$F(\theta_t) - F(\theta^\star) \leq \left(1 - \frac{1}{\kappa}\right)^{2t} (F(\theta_0) - F(\theta^\star))$

**情况二：当 $\mu = 0$ 时（弱凸情况，即仅凸非强凸）**

当 $H$ 不满秩（存在特征值为 $0$）时$,1 - \gamma \lambda_i$ 的最小可能值是 $1$（当 $\lambda_i=0$ 时）。此时不能用指数衰减（线性收敛），只能退化为 **$O(1/t)$ 的次线性收敛**。

**证明：**
令步长 $\gamma = \frac{1}{L}$，我们要证明对于任意 $\lambda \in [0, L]$，都有：$(1 - \gamma \lambda)^{2t} \lambda \leq \frac{1}{4\gamma t}$ 

考虑函数 $g(\lambda) = (1 - \gamma \lambda)^{2t} \lambda$，对其求导：
$g'(\lambda) = -2t\gamma (1 - \gamma \lambda)^{2t-1} \lambda + (1 - \gamma \lambda)^{2t}$.
令 $g'(\lambda) = 0$：$(1 - \gamma \lambda)^{2t-1} [ -2t\gamma \lambda + 1 - \gamma \lambda ] = 0$.
解得驻点：$\lambda = \frac{1}{\gamma(2t + 1)}$.
将驻点代回 $g(\lambda)$：$g_{\text{max}} = \left(1 - \frac{1}{2t+1}\right)^{2t} \frac{1}{\gamma(2t+1)} = \left(\frac{2t}{2t+1}\right)^{2t} \frac{1}{\gamma(2t+1)}$.
我们知道 $\lim_{t \to \infty} (1 + \frac{1}{x})^x = e$，所以 $\left(\frac{2t}{2t+1}\right)^{2t} < 1$。
因此：
$g_{\text{max}} < \frac{1}{\gamma(2t+1)} < \frac{1}{2\gamma t}$.

将其代回求和式：
$F(\theta_t) - F(\theta^\star) = \frac{1}{2} \sum_{i=1}^d g(\lambda_i) |q_i^T(\theta_0 - \theta^\star)|^2 \leq \frac{1}{2} \cdot \frac{1}{2\gamma t} \sum_{i=1}^d |q_i^T(\theta_0 - \theta^\star)|^2$
因为 $Q$ 是正交矩阵$,\sum_{i=1}^d |q_i^T(\theta_0 - \theta^\star)|^2 = \|\theta_0 - \theta^\star\|_2^2$。
所以：$F(\theta_t) - F(\theta^\star) \leq \frac{1}{4\gamma t} \|\theta_0 - \theta^\star\|_2^2$.

**※【例题】** 为什么 $\mu=0$ 时只能达到 $O(1/t)$？
考虑一维函数 $F(\theta) = \theta^2$，但这是受限数据下的特例。假设 $H=0$，即目标函数是常数 $F(\theta)=0$，那么梯度为0，不下降。
我们看一个真正的奇异矩阵例子：假设 $H = \begin{bmatrix} 0 & 0 \\ 0 & 1 \end{bmatrix}$，即 $F(\theta) = \frac{1}{2}\theta_2^2$。最优解集是 $\theta^\star = (c, 0)$，其中 $c$ 为任意常数。
设初始点 $\theta_0 = (1, 1)$，步长 $\gamma = 1$（即 $L=1$）。
梯度更新：$\theta_{t+1} = \theta_t - H\theta_t = (I-H)\theta_t$
其中 $I-H = \begin{bmatrix} 1 & 0 \\ 0 & 0 \end{bmatrix}$。
迭代轨迹：
$\theta_1 = (1, 0)$
$\theta_2 = (1, 0)$
可以看到，第一维的参数 $\theta_1$ 永远停留在 $1$，无法收敛到最优点 $(0,0)$。**但是，函数值收敛了**：因为 $F(\theta_1) = 0$，这等于最优点时的函数值。
因为参数 $\theta$ 没有完全收敛（存在零空间误差），我们不能再用范数 $\|\theta_0 - \theta^\star\|$ 的指数衰减来约束，而必须用 $O(1/t)$ 这种更宽松的界来描述函数值的收敛。

## 凸优化（Convex Optimization）

### 凸函数（Convex Function）

**※【定义】** 凸函数(Convex Function)
设 $F:\mathbb R^d\to\mathbb R$。若对任意 $\theta,\eta\in\mathbb R^d$ 及 $\lambda\in[0,1]$，均有
$F(\lambda\eta+(1-\lambda)\theta)\leq\lambda F(\eta)+(1-\lambda)F(\theta)$，
则称 $F$ 为凸函数。几何上，函数图像上任意两点间的弦位于图像上方。

**※【定理】** 凸函数的一阶判别法
设 $F:\mathbb R^d\to\mathbb R$ 可微，则 $F$ 为凸函数，当且仅当对任意 $\theta,\eta\in\mathbb R^d$，有
$F(\eta)\geq F(\theta)+\nabla F(\theta)^T(\eta-\theta)$。
即函数图像始终位于任意一点的切平面(Tangent Hyperplane)之上。

**※【定理】** 凸函数的二阶判别法
设 $F:\mathbb R^d\to\mathbb R$ 二阶可微，则 $F$ 为凸函数，当且仅当对任意 $\theta\in\mathbb R^d$，有 $\nabla^2F(\theta)\succeq0$。
其中 $\nabla^2F(\theta)$ 为海森矩阵(Hessian Matrix)$,\succeq0$ 表示半正定(Positive Semidefinite, PSD)。

**※【定理】** 詹森不等式(Jensen's Inequality)
设随机向量 $X$ 与 $F(X)$ 的期望存在，且 $F$ 为凸函数，则 $F(\mathbb E[X])\leq\mathbb E[F(X)]$。

**※【推论】** 凸函数的非负线性组合
设 $F_1,\ldots,F_n$ 均为凸函数$,a_1,\ldots,a_n\geq0$，则 $F(\theta)=\sum_{i=1}^n a_iF_i(\theta)$ 仍为凸函数。

**※【推论】** 范数的凸性
任意范数 $\|\cdot\|$ 均为凸函数。对任意 $\theta,\eta\in\mathbb R^d$ 及 $\lambda\in[0,1]$，有
$\|\lambda\theta+(1-\lambda)\eta\|\leq\lambda\|\theta\|+(1-\lambda)\|\eta\|$。

**证明**
由三角不等式及范数的齐次性$,\|\lambda\theta+(1-\lambda)\eta\|\leq\|\lambda\theta\|+\|(1-\lambda)\eta\|=\lambda\|\theta\|+(1-\lambda)\|\eta\|$。

**※【推论】** 凸函数与线性映射的复合
设 $F:\mathbb R^m\to\mathbb R$ 为凸函数$,A:\mathbb R^d\to\mathbb R^m$ 为线性映射，则 $G(\theta)=F(A\theta)$ 为凸函数。

**证明**
由 $A$ 的线性性和 $F$ 的凸性，对任意 $\theta,\eta\in\mathbb R^d$ 及 $\lambda\in[0,1]$，有
$G(\lambda\theta+(1-\lambda)\eta)=F(\lambda A\theta+(1-\lambda)A\eta)\leq\lambda G(\theta)+(1-\lambda)G(\eta)$。

**※【命题】** 可微凸函数的一阶最优性条件(First-order Optimality Condition)
设 $F:\mathbb R^d\to\mathbb R$ 为可微凸函数，则 $\theta^\star\in\arg\min_{\theta\in\mathbb R^d}F(\theta)\iff\nabla F(\theta^\star)=0$。

**证明**
必要性：若 $\theta^\star$ 为无约束全局最优点，由费马定理(Fermat's theorem)得 $\nabla F(\theta^\star)=0$。
充分性：若 $\nabla F(\theta^\star)=0$，由一阶判别法，对任意 $\theta$ 有 $F(\theta)\geq F(\theta^\star)+\nabla F(\theta^\star)^T(\theta-\theta^\star)=F(\theta^\star)$，故 $\theta^\star$ 为全局最优点。

## 强凸与光滑收敛（Strong Convexity and Smooth Convergence）

### 强凸函数（Strongly Convex Function）

**※【定义】** $\mu$-强凸函数（$\mu$-Strongly Convex）
设 $\mu>0$。若对任意 $\theta,\eta\in\mathbb R^d$ 及 $\lambda\in[0,1]$，均有
$\begin{aligned} F(\lambda\eta+(1-\lambda)\theta) \leq {}&\lambda F(\eta)+(1-\lambda)F(\theta)-\frac{\mu}{2}\lambda(1-\lambda) \|\eta-\theta\|_2^2, \end{aligned}$
则称 $F$ 为 $\mu$-强凸函数。

**※【定理】** 强凸函数的一阶判别法
设 $F:\mathbb R^d\to\mathbb R$ 可微，则 $F$ 为 $\mu$-强凸函数，当且仅当对任意 $\theta,\eta\in\mathbb R^d$：
$F(\eta)\geq F(\theta) +\nabla F(\theta)^T(\eta-\theta) +\frac{\mu}{2}|\eta-\theta|_2^2$.

**※【定理】** 强凸函数的二阶判别法
设 $F:\mathbb R^d\to\mathbb R$ 二阶可微，则 $F$ 为 $\mu$-强凸函数，当且仅当
$\nabla^2F(\theta)\succeq\mu I, \quad \forall\theta\in\mathbb R^d$.

**※【推论】** 凸函数加二次项得到强凸函数
若 $F$ 为凸函数，则对任意 $\mu>0$，$G(\theta)=F(\theta)+\frac{\mu}{2}|\theta|_2^2$为 $\mu$-强凸函数。

**证明**
由凸函数定义：$F(\lambda\eta+(1-\lambda)\theta) \leq \lambda F(\eta)+(1-\lambda)F(\theta)$.
另一方面，利用二次函数的恒等式：$\begin{aligned} &\|\lambda\eta+(1-\lambda)\theta\|_2^2=\lambda\|\eta\|_2^2+(1-\lambda)\|\theta\|_2^2 -\lambda(1-\lambda)\|\eta-\theta\|_2^2. \end{aligned}$
两式相加，并乘以相应系数，即得 $G$ 满足 $\mu$-强凸定义。

**※【推论】** 强凸函数的最优解唯一性
若 $F$ 为 $\mu$-强凸函数，且存在最优解 $\theta^\star\in\arg\min F$，则该最优解唯一。

**证明**
反设存在两个不同的最优解 $\theta_1^\star,\theta_2^\star$，满足 $F(\theta_1^\star)=F(\theta_2^\star)=F^\star$.
由强凸函数定义，取 $\lambda=\frac12$：
$\begin{aligned} F\left(\frac{\theta_1^\star+\theta_2^\star}{2}\right) \leq {}&\frac12F(\theta_1^\star)+\frac12F(\theta_2^\star)-\frac{\mu}{8}|\theta_1^\star-\theta_2^\star|_2^2. \end{aligned}$
由于 $\theta_1^\star\neq\theta_2^\star$，故 $F\left(\frac{\theta_1^\star+\theta_2^\star}{2}\right)<F^\star$,
与 $F^\star$ 为全局最小值矛盾。因此最优解唯一。

### Łojasiewicz 不等式

**※【引理】** Łojasiewicz 不等式（Łojasiewicz Inequality）
设 $F:\mathbb R^d\to\mathbb R$ 可微且为 $\mu$-强凸函数$,\mu>0$，
并设 $\theta^\star\in\arg\min F$，则对任意 $\theta\in\mathbb R^d$：$|\nabla F(\theta)|_2^2 \geq 2\mu\big(F(\theta)-F(\theta^\star)\big)$.

**证明**
由强凸函数的一阶判别法：$\begin{aligned} F(\theta^\star)\geq {}&F(\theta) +\nabla F(\theta)^T(\theta^\star-\theta)+\frac{\mu}{2}|\theta^\star-\theta|_2^2. \end{aligned}$
固定 $\theta$，考虑关于 $y$ 的函数：$G(y)=\nabla F(\theta)^T(y-\theta) +\frac{\mu}{2}|y-\theta|_2^2+F(\theta)$.
由上式，对任意 $y$ 均有 $F(y)\geq G(y)$，特别地：$F(\theta^\star)\geq G(\theta^\star)\geq\min_yG(y)$.
对 $G(y)$ 求导：$\nabla_yG(y)=\nabla F(\theta)+\mu(y-\theta)$.
令其为零，得到 $\hat y-\theta=-\frac{1}{\mu}\nabla F(\theta)$.
代入 $G(y)$：$\begin{aligned} \min_yG(y) &=F(\theta)-\frac{1}{\mu}|\nabla F(\theta)|_2^2+\frac{1}{2\mu}|\nabla F(\theta)|_2^2=F(\theta)-\frac{1}{2\mu}|\nabla F(\theta)|_2^2. \end{aligned}$
因此 $F(\theta^\star)\geq F(\theta)-\frac{1}{2\mu}|\nabla F(\theta)|_2^2$.
整理即得 $|\nabla F(\theta)|_2^2 \geq 2\mu(F(\theta)-F(\theta^\star))$.

### 光滑函数（Smooth Function）

**※【定义】** $L$-光滑函数（$L$-Smooth Function）
设 $F:\mathbb R^d\to\mathbb R$ 可微。若存在常数 $L>0$，使得对任意 $\theta,\eta\in\mathbb R^d$：
$| F(\eta)-F(\theta) -\nabla F(\theta)^T(\eta-\theta)| \leq \frac{L}{2}\|\eta-\theta\|_2^2$, 则称 $F$ 为 $L$-光滑函数。

**※【定理】** 光滑函数的一阶等价刻画
$F$ 为 $L$-光滑函数，当且仅当其梯度满足 Lipschitz 连续性（Lipschitz Continuity），
即 $\|\nabla F(\theta)-\nabla F(\eta)\|_2 \leq L\|\theta-\eta\|_2, \quad\forall\theta,\eta\in\mathbb R^d$.

**※【定理】** 光滑函数的二阶判别法
设 $F$ 二阶可微，则 $F$ 为 $L$-光滑函数，
当且仅当 $-LI\preceq\nabla^2F(\theta)\preceq LI, \quad\forall\theta\in\mathbb R^d$. 
若 $F$ 同时为凸函数，则进一步有 $0\preceq\nabla^2F(\theta)\preceq LI$.

**※【性质】** 实对称矩阵的半正定判别
设 $A\in\mathbb R^{d\times d}$ 为实对称矩阵，则 $A\succeq0 \iff x^TAx\geq0,\quad\forall x\in\mathbb R^d$.
进一步，$A\succeq\mu I \iff x^TAx\geq\mu|x|_2^2, \quad\forall x\in\mathbb R^d$.
其中 $\mu>0$ 时$,A$ 的最小特征值不小于 $\mu$。

**※【引理】** 下降引理（Descent Lemma）
若 $F$ 为 $L$-光滑函数，则对任意 $\theta,\eta\in\mathbb R^d$：$F(\eta)\leq F(\theta) +\nabla F(\theta)^T(\eta-\theta) +\frac{L}{2}\|\eta-\theta\|_2^2$.
特别地，取 $\eta=\theta-\frac1L\nabla F(\theta)$, 得到 $F\left(\theta-\frac1L\nabla F(\theta)\right) \leq F(\theta)-\frac{1}{2L}\|\nabla F(\theta)\|_2^2$.

**证明**
直接由 $L$-光滑函数定义：
$F(\eta)\leq F(\theta) +\nabla F(\theta)^T(\eta-\theta) +\frac{L}{2}|\eta-\theta|_2^2$.
代入 $\eta=\theta-\frac1L\nabla F(\theta)$：
$\begin{aligned} F(\eta) &\leq F(\theta)-\frac1L|\nabla F(\theta)|_2^2+\frac{L}{2}\frac1{L^2}|\nabla F(\theta)|_2^2=F(\theta)-\frac{1}{2L}|\nabla F(\theta)|_2^2\end{aligned}$.

**※【推论】** 光滑函数的梯度范数与函数值差距
若 $F$ 为 $L$-光滑函数，且存在全局最优解 $\theta^\star$，则$|\nabla F(\theta)|_2^2 \leq 2L(F(\theta)-F(\theta^\star))$.

**证明**
由下降引理：$F\left(\theta-\frac1L\nabla F(\theta)\right) \leq F(\theta)-\frac{1}{2L}|\nabla F(\theta)|_2^2$.
又因为 $\theta^\star$ 为全局最优点：$F(\theta^\star)\leq F\left(\theta-\frac1L\nabla F(\theta)\right)$.
因此 $F(\theta^\star)\leq F(\theta) -\frac{1}{2L}|\nabla F(\theta)|_2^2$. 整理即得结论。

## 梯度下降收敛性（Gradient Descent Convergence）

### 强凸且光滑情形

梯度下降法（Gradient Descent, GD）的迭代公式为 $\theta_t=\theta_{t-1}-\gamma\nabla F(\theta_{t-1})$,
其中 $\gamma>0$ 为步长（Step Size）。

**※【定理】** 强凸光滑函数的梯度下降收敛性
设 $F:\mathbb R^d\to\mathbb R$ 为 $L$-光滑且 $\mu$-强凸函数，存在唯一全局最优解 $\theta^\star$。
取步长 $\gamma=\frac1L$, 并采用梯度下降迭代 $\theta_t=\theta_{t-1}-\frac1L\nabla F(\theta_{t-1})$,
则对任意 $t\geq0$：
$\begin{aligned} F(\theta_t)-F(\theta^\star) &\leq \left(1-\frac{\mu}{L}\right)^t \frac L2\|\theta_0-\theta^\star\|_2^2\leq \exp\left(-\frac{\mu}{L}t\right) \frac L2\|\theta_0-\theta^\star\|_2^2\end{aligned}$.

因此，若希望函数值误差满足 $F(\theta_t)-F(\theta^\star)\leq\varepsilon$, 只需 $t\geq\frac L\mu \ln\left(\frac{L|\theta_0-\theta^\star|_2^2}{2\varepsilon}\right)$.
即迭代复杂度为 $O\left(\frac L\mu\log\frac1\varepsilon\right)$.

**证明**
首先，由梯度下降迭代公式：$\theta_t=\theta_{t-1}-\gamma\nabla F(\theta_{t-1})$,
展开距离平方：$\begin{aligned} \|\theta_t-\theta^\star\|_2^2 =\|\theta_{t-1}-\theta^\star\|_2^2-2\gamma\nabla F(\theta_{t-1})^T (\theta_{t-1}-\theta^\star)+\gamma^2\|\nabla F(\theta_{t-1})\|_2^2\end{aligned}$.

由强凸函数的一阶判别法：
$\begin{aligned} F(\theta^\star)\geq{}& F(\theta_{t-1})+\nabla F(\theta_{t-1})^T (\theta^\star-\theta_{t-1})+\frac{\mu}{2}|\theta_{t-1}-\theta^\star|_2^2. \end{aligned}$

整理得到
$\begin{aligned} \nabla F(\theta_{t-1})^T (\theta_{t-1}-\theta^\star) \geq{}&F(\theta_{t-1})-F(\theta^\star)+\frac{\mu}{2}|\theta_{t-1}-\theta^\star|_2^2. \end{aligned}$

又由前述光滑性推论：
$|\nabla F(\theta_{t-1})|_2^2 \leq 2L(F(\theta_{t-1})-F(\theta^\star))$.

代入距离平方展开式：
$\begin{aligned} |\theta_t-\theta^\star|_2^2 \leq{}&(1-\mu\gamma) |\theta_{t-1}-\theta^\star|_2^2-2\gamma(1-\gamma L) (F(\theta_{t-1})-F(\theta^\star)). \end{aligned}$

取 $\gamma=\frac1L$，第二项消失，得到
$|\theta_t-\theta^\star|_2^2 \leq \left(1-\frac{\mu}{L}\right) |\theta_{t-1}-\theta^\star|_2^2$.

递推可得 $|\theta_t-\theta^\star|_2^2 \leq \left(1-\frac{\mu}{L}\right)^t |\theta_0-\theta^\star|_2^2$.

另一方面，由下降引理，以 $\theta^\star$ 为展开点，且 $\nabla F(\theta^\star)=0$：
$F(\theta_t)-F(\theta^\star) \leq\frac L2|\theta_t-\theta^\star|_2^2$.

因此 $F(\theta_t)-F(\theta^\star) \leq \left(1-\frac{\mu}{L}\right)^t \frac L2|\theta_0-\theta^\star|_2^2$.

利用不等式 $1-x\leq e^{-x}$，得到 $F(\theta_t)-F(\theta^\star) \leq e^{-\frac{\mu}{L}t} \frac L2|\theta_0-\theta^\star|_2^2$.

证毕。

### 凸且光滑情形

**※【定理】** 凸光滑函数的梯度下降收敛性

设 $F:\mathbb R^d\to\mathbb R$ 为凸且 $L$-光滑函数，存在全局最优解 $\theta^\star$。
取步长 $\gamma=\frac1L$, 并采用梯度下降迭代 $\theta_t=\theta_{t-1}-\gamma\nabla F(\theta_{t-1})$,
则对任意整数 $T\geq1$：$F(\theta_T)-F(\theta^\star) \leq\frac{L|\theta_0-\theta^\star|_2^2}{T}$.
因此，若希望函数值误差满足 $F(\theta_T)-F(\theta^\star)\leq\varepsilon$,
只需 $T\geq\frac{L|\theta_0-\theta^\star|_2^2}{\varepsilon}$. 即迭代复杂度为 $O(1/\varepsilon)$。

**证明**
首先，由梯度下降迭代公式：$\theta_t=\theta_{t-1}-\gamma\nabla F(\theta_{t-1})$,
展开距离平方：
$\begin{aligned} |\theta_t-\theta^\star|_2^2 ={}&|\theta_{t-1}-\theta^\star|_2^2-2\gamma\nabla F(\theta_{t-1})^T (\theta_{t-1}-\theta^\star)+\gamma^2|\nabla F(\theta_{t-1})|_2^2. \end{aligned}$
由凸函数的一阶判别法：$F(\theta^\star)\geq F(\theta_{t-1}) +\nabla F(\theta_{t-1})^T (\theta^\star-\theta_{t-1})$.
因此$\nabla F(\theta_{t-1})^T (\theta_{t-1}-\theta^\star) \geq F(\theta_{t-1})-F(\theta^\star)$.
代入距离平方展开式：$\begin{aligned} 2\gamma(F(\theta_{t-1})-F(\theta^\star)) \leq{}& |\theta_{t-1}-\theta^\star|_2^2-|\theta_t-\theta^\star|_2^2+\gamma^2|\nabla F(\theta_{t-1})|_2^2 \end{aligned}$.
由下降引理，取 $\gamma=1/L$：$F(\theta_t)\leq F(\theta_{t-1}) -\frac{\gamma}{2}|\nabla F(\theta_{t-1})|_2^2$.
故 $\gamma^2|\nabla F(\theta_{t-1})|_2^2 \leq 2\gamma(F(\theta_{t-1})-F(\theta_t))$.
代回并整理：$\begin{aligned} F(\theta_{t-1})-F(\theta^\star) \leq{}& \frac{1}{2\gamma} \left( |\theta_{t-1}-\theta^\star|_2^2 -|\theta_t-\theta^\star|_2^2 \right)+F(\theta_{t-1})-F(\theta_t)\end{aligned}$.
对 $t=1,\ldots,T$ 求和，利用望远镜求和（Telescoping Sum）：
$\begin{aligned} &\sum_{t=0}^{T-1}(F(\theta_t)-F(\theta^\star))\leq \frac{1}{2\gamma} \left(|\theta_0-\theta^\star|_2^2 -|\theta_T-\theta^\star|_2^2\right)+F(\theta_0)-F(\theta_T)\end{aligned}$.
舍去非负项：$\begin{aligned} \sum_{t=0}^{T-1}(F(\theta_t)-F(\theta^\star)) \leq{}& \frac{1}{2\gamma}|\theta_0-\theta^\star|_2^2+F(\theta_0)-F(\theta_T)\end{aligned}$.

由于梯度下降过程中函数值单调不增：$F(\theta_T)\leq F(\theta_t), \quad 0\leq t<T$.
因此$T(F(\theta_T)-F(\theta^\star)) \leq \sum_{t=0}^{T-1}(F(\theta_t)-F(\theta^\star))$.
又由下降引理，以 $\theta^\star$ 为展开点：$F(\theta_0)-F(\theta^\star) \leq\frac L2|\theta_0-\theta^\star|_2^2$.
取 $\gamma=1/L$，最终得到
$\begin{aligned} T(F(\theta_T)-F(\theta^\star)) &\leq \frac L2|\theta_0-\theta^\star|_2^2+\frac L2|\theta_0-\theta^\star|_2^2=L|\theta_0-\theta^\star|_2^2\end{aligned}$.
即 $F(\theta_T)-F(\theta^\star) \leq\frac{L|\theta_0-\theta^\star|_2^2}{T}$. $\square$.

## §6.4 拟合模型到数据（Fitting a Model to Data）

给定数据集 $Z={(x_i,y_i)}_{i=1}^n$，其中 $x_i\in\mathbb R^p$ 为输入，$y_i\in\mathbb R$ 为输出。
设 $M_\alpha:\mathbb R^p\to\mathbb R$ 为由参数 $\alpha\in\mathbb R^k$ 确定的模型。
通过损失函数（Loss Function）$L(Z,M_\alpha)$ 衡量模型预测与真实观测之间的差异，
定义优化目标：$f(\alpha)=L(Z,M_\alpha)$
模型拟合即求解：$\min_{\alpha\in\mathbb R^k}f(\alpha)$

**※【定义】** 误差平方和（Sum of Squared Errors, SSE）
对于回归问题，可以采用误差平方和作为损失函数：$f(\alpha)=\mathrm{SSE}=\sum_{i=1}^n(y_i-M_\alpha(x_i))^2$ (6.1)
其中 $y_i-M_\alpha(x_i)$ 称为残差（Residual）。
### §6.4.1 回归的最小均方更新（Least Mean Squares Updates）

考虑简单的二次多项式回归模型，参数维数 $k=3$：
$\alpha=(\alpha_0,\alpha_1,\alpha_2)^T$
$M_\alpha(x)=\alpha_0+\alpha_1x+\alpha_2x^2$
定义特征映射（Feature Map）：$\phi(x)=(1,x,x^2)^T$
则模型可以写为内积形式：$M_\alpha(x)=\langle\alpha,\phi(x)\rangle=\alpha^T\phi(x)$

**※【例题】** 单个数据点的最小均方更新
给定单个数据点 $(x_1,y_1)$，定义目标函数：$f_1(\alpha)=(M_\alpha(x_1)-y_1)^2$
对参数 $\alpha_j$ 求偏导，其中 $j=0,1,2$：$\frac{\partial f_1(\alpha)}{\partial\alpha_j}=2(M_\alpha(x_1)-y_1)x_1^j$
因此梯度为：$\nabla f_1(\alpha)=2(M_\alpha(x_1)-y_1)\phi(x_1)$
展开可得：$\nabla f_1(\alpha)=2(M_\alpha(x_1)-y_1)(1,x_1,x_1^2)^T$
采用梯度下降（Gradient Descent, GD）更新：$\alpha_{t+1}=\alpha_t-\gamma_t\nabla f_1(\alpha_t)$
即：$\alpha_{t+1}=\alpha_t-2\gamma_t(M_{\alpha_t}(x_1)-y_1)\phi(x_1)$
这一更新称为最小均方更新规则（Least Mean Squares, LMS），也称 Widrow–Hoff 学习规则（Widrow–Hoff Learning Rule）。
更新幅度与预测残差成正比：残差较大时，梯度较大；残差接近零时，更新幅度也趋近于零。

**※【性质】** 最小均方更新的几何意义
设残差为 $r_i=M_\alpha(x_i)-y_i$，则单点梯度为：$\nabla f_i(\alpha)=2r_i\phi(x_i)$
因此负梯度方向为 $-2r_i\phi(x_i)$：
- 当 $r_i>0$，即预测值偏高时，更新沿 $-\phi(x_i)$ 方向进行。
- 当 $r_i<0$，即预测值偏低时，更新沿 $\phi(x_i)$ 方向进行。
- 当 $r_i=0$，该数据点对应的梯度为零。

对于固定特征向量，模型在负梯度方向上的预测值会向真实值移动。

**※【例题】** 多个数据点的矩阵表示
设有 $n$ 个观测，定义设计矩阵（Design Matrix）：$\Phi=\begin{pmatrix}\phi(x_1)^T\\\phi(x_2)^T\\\vdots\\\phi(x_n)^T\end{pmatrix}\in\mathbb R^{n\times k}$.
以及观测向量：$y=(y_1,\ldots,y_n)^T\in\mathbb R^n$.
则误差平方和可以写为：$f(\alpha)=|\Phi\alpha-y|_2^2$.
其梯度为：$\nabla f(\alpha)=2\Phi^T(\Phi\alpha-y)$.
Hessian 矩阵为：$\nabla^2f(\alpha)=2\Phi^T\Phi$.
由于对任意 $v\in\mathbb R^k$：$v^T\nabla^2f(\alpha)v=2|\Phi v|_2^2\geq0$. 因此 $f$ 为凸函数。

若 $\Phi$ 列满秩，则 $\Phi^T\Phi$ 正定，从而 $f$ 为强凸函数，其强凸参数可取：$\mu=2\lambda_{\min}(\Phi^T\Phi)>0$
此时最小二乘解唯一，满足正规方程（Normal Equations）：$\Phi^T\Phi\alpha^*=\Phi^Ty$.
因此：$\alpha^*=(\Phi^T\Phi)^{-1}\Phi^Ty$. 若 $\Phi$ 不列满秩，则目标函数仍为凸函数，但不具有全局强凸性，最优解不一定唯一。

### §6.4.2 可分解函数（Decomposable Functions）

**※【定义】** 可分解目标函数
若目标函数可以表示为各个样本损失之和：$f(\alpha)=\sum_{i=1}^nf_i(\alpha)$.则称其具有可分解结构（Decomposable Structure）。

对于平方误差损失：$f_i(\alpha)=(M_\alpha(x_i)-y_i)^2$. 由于每个 $f_i$ 均为凸函数，因此其和 $f$ 也是凸函数。
在大规模数据集上，通常采用平均经验损失（Empirical Risk）：$F(\alpha)=\frac1n\sum_{i=1}^nf_i(\alpha)$.
最小化 $F$ 与最小化 $f$ 具有相同的最优解，但梯度相差常数因子 $n$。
为了使更新公式更加统一，以下采用平均经验损失 $F$。

**※【定义】** 批量梯度下降（Batch Gradient Descent, BGD）
批量梯度下降在每次迭代中使用整个数据集计算梯度：$\nabla F(\alpha)=\frac1n\sum_{i=1}^n\nabla f_i(\alpha)$
更新规则为：$\alpha_{t+1}=\alpha_t-\gamma_t\frac1n\sum_{i=1}^n\nabla f_i(\alpha_t)$
对于二次回归：$\nabla f_i(\alpha)=2(M_\alpha(x_i)-y_i)\phi(x_i)$
因此：$\alpha_{t+1}=\alpha_t-\frac{2\gamma_t}{n}\sum_{i=1}^n(M_{\alpha_t}(x_i)-y_i)\phi(x_i)$
其矩阵形式为：$\alpha_{t+1}=\alpha_t-\frac{2\gamma_t}{n}\Phi^T(\Phi\alpha_t-y)$
每次迭代均需要遍历所有样本。对于固定参数维数 $k$，一次梯度计算的时间复杂度为 $O(n)$；一般情况下为 $O(nk)$。

**※【定义】** 增量梯度下降（Incremental Gradient Descent, IGD）
增量梯度下降在每次迭代中仅选择一个样本，
利用其损失函数的梯度更新参数：$\alpha_{t+1}=\alpha_t-\gamma_t\nabla f_{i_t}(\alpha_t)$
其中 $i_t$ 按预先确定的次序选取，例如循环遍历全部数据点。

对于平方误差：$\alpha_{t+1}=\alpha_t-2\gamma_t(M_{\alpha_t}(x_{i_t})-y_{i_t})\phi(x_{i_t})$

**※【定义】** 随机梯度下降（Stochastic Gradient Descent, SGD）
随机梯度下降每次随机抽取一个样本索引 $i_t$，计算单样本梯度并更新：$\alpha_{t+1}=\alpha_t-\gamma_t\nabla f_{i_t}(\alpha_t)$
若 $i_t$ 在 ${1,\ldots,n}$ 上独立均匀抽样，则有：$\mathbb E_{i_t}[\nabla f_{i_t}(\alpha)]=\frac1n\sum_{i=1}^n\nabla f_i(\alpha)=\nabla F(\alpha)$
因此，**SGD 的单样本梯度是完整平均梯度的无偏估计**。

对于单点平方误差采用标准化损失：$\ell_i(\alpha)=\frac12(\alpha^T\phi(x_i)-y_i)^2$
则：$\nabla\ell_i(\alpha)=(\alpha^T\phi(x_i)-y_i)\phi(x_i)$
对应的 SGD 更新为：$\alpha_{t+1}=\alpha_t-\gamma_t(\alpha_t^T\phi(x_{i_t})-y_{i_t})\phi(x_{i_t})$
其中因子 $1/2$ 用于消去求导产生的系数 $2$，不改变最优解。

**※【性质】** 批量、增量与随机梯度下降的区别

|方法|每次使用的样本|梯度性质|单步计算量|
|---|---|---|---|
|BGD|全部 $n$ 个样本|精确全梯度|$O(nk)$|
|IGD|一个按固定次序选取的样本|单样本梯度|$O(k)$|
|SGD|一个随机样本|对平均梯度无偏|$O(k)$|

其中 $k$ 为模型参数维数。

SGD 的单次迭代成本较低，但由于使用随机梯度，迭代轨迹会产生随机波动。固定学习率下通常不能保证精确收敛到最优解；采用适当递减学习率和平均迭代点，可以建立期望意义下的收敛保证。

### §6.4.3 随机梯度下降的收敛性（SGD Convergence）

**※【定理】** 凸目标函数的随机梯度下降收敛性
设 $F:\mathbb R^d\to\mathbb R$ 为凸函数，存在最优解 $\theta^*$。随机梯度下降迭代为：$\theta_{t+1}=\theta_t-\gamma_tg_t$
其中 $g_t=\nabla f_{i_t}(\theta_t)$，并假设：
1. 随机梯度无偏，即 $\mathbb E[g_t\mid\theta_t]=\nabla F(\theta_t)$。
2. 随机梯度有界，即 $|g_t|_2\leq B$.
3. 初始点满足 $|\theta_1-\theta^*|_2\leq D$。
4. 学习率选为 $\gamma_t=\frac{D}{B\sqrt t}$，其中 $D,B>0$。

定义加权平均迭代点：$\bar\theta_T=\frac{\sum_{t=1}^T\gamma_t\theta_t}{\sum_{t=1}^T\gamma_t}$.
则有：$\mathbb E[F(\bar\theta_T)-F(\theta^*)]\leq\frac{D^2+B^2\sum_{t=1}^T\gamma_t^2}{2\sum_{t=1}^T\gamma_t}$.
进一步：$\mathbb E[F(\bar\theta_T)-F(\theta^*)]=O\left(\frac{DB\log T}{\sqrt T}\right)$.
即 SGD 在上述条件下具有期望意义下的次线性收敛保证。

**证明**：
由更新公式：$\theta_{t+1}=\theta_t-\gamma_tg_t$
展开距离平方：$|\theta_{t+1}-\theta^\star |_2^2=|\theta_t-\theta^\star|_2^2-2\gamma_t\langle g_t,\theta_t-\theta^*\rangle+\gamma_t^2|g_t|_2^2$
对随机索引 $i_t$ 取条件期望，利用无偏性：$\mathbb E[\langle g_t,\theta_t-\theta^\star\rangle\mid\theta_t]=\langle\nabla F(\theta_t),\theta_t-\theta^\star\rangle$
由凸函数的一阶判别法：$\langle\nabla F(\theta_t),\theta_t-\theta^\star\rangle\geq F(\theta_t)-F(\theta^\star)$

结合 $|g_t|_2^2\leq B^2$，得到：$\mathbb E[|\theta_{t+1}-\theta^\star|_2^2]\leq\mathbb E[|\theta_t-\theta^\star|_2^2]-2\gamma_t\mathbb E[F(\theta_t)-F(\theta^*)]+\gamma_t^2B^2$.
整理：$2\gamma_t\mathbb E[F(\theta_t)-F(\theta^\star)]\leq\mathbb E[|\theta_t-\theta^\star|_2^2]-\mathbb E[|\theta_{t+1}-\theta^*|_2^2]+\gamma_t^2B^2$.
对 $t=1,\ldots,T$ 求和，利用望远镜求和：$2\sum_{t=1}^T\gamma_t\mathbb E[F(\theta_t)-F(\theta^*)]\leq D^2+B^2\sum_{t=1}^T\gamma_t^2$.
另一方面，由 Jensen 不等式及 $F$ 的凸性：$F(\bar\theta_T)\leq\frac{\sum_{t=1}^T\gamma_tF(\theta_t)}{\sum_{t=1}^T\gamma_t}$.
因此 $\mathbb E[F(\bar\theta_T)-F(\theta^*)]\leq\frac{D^2+B^2\sum_{t=1}^T\gamma_t^2}{2\sum_{t=1}^T\gamma_t}$.
取 $\gamma_t=D/(B\sqrt t)$，则：$\sum_{t=1}^T\gamma_t^2=\frac{D^2}{B^2}\sum_{t=1}^T\frac1t$.
利用调和级数上界：$\sum_{t=1}^T\frac1t\leq1+\ln T$.
以及：$\sum_{t=1}^T\frac1{\sqrt t}\geq\sqrt T$.
可得：$\mathbb E[F(\bar\theta_T)-F(\theta^*)]\leq\frac{DB(2+\ln T)}{2\sqrt T}$.
因此：$\mathbb E[F(\bar\theta_T)-F(\theta^*)]=O\left(\frac{DB\log T}{\sqrt T}\right)$.
证毕。

需要注意，该定理保证的是**加权平均迭代点的期望函数值误差**，而非每一个随机迭代点都单调下降或几乎必然具有同样的收敛速度。
