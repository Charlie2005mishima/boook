
**※【定义】** 插值(Interpolation)：
插值是在已知数据点之间估计未知值的过程。与预测已知范围之外值的外推(extrapolation)不同，插值构造一个函数（称为插值函数(interpolant)），该函数穿过一组离散的已知点以确定中间值。

**※【例题】** 插值函数可以是一个多项式，也可以是非多项式函数，例如有理函数和分段多项式样条。无论插值函数的形式如何，我们都需要通过确定原函数与插值函数之间的最大差异来控制插值的不确定性。

## 6.1 The Vandermonde determinant

**※【定义】** 范德蒙矩阵(Vandermonde matrix)：
对于给定的 $n$ 个参数 $t_1, t_2, \ldots, t_n$，相关的 $n \times n$ 范德蒙矩阵 $V$ 的第 $(i,j)$ 个元素为 $v_{i,j} = t_j^{i-1}$；矩阵形式为
$V(t_1, t_2, \ldots, t_n) = \begin{bmatrix} 1 & 1 & \dots & 1 \\ t_1 & t_2 & \dots & t_n \\ \vdots & \vdots & \ddots & \vdots \\ t_1^{n-1} & t_2^{n-1} & \dots & t_n^{n-1} \end{bmatrix}$ (6.1)

**※【引理】** $(n+1) \times (n+1)$ 范德蒙矩阵的行列式可表示为
$\det V(x_0, x_1, \dots, x_n) = \prod_{i > j} (x_i - x_j)$ (6.2)

**证明**：
考虑函数 $U(x) = \det V(x_0, x_1, \ldots, x_{n-1}, x)$ (6.3)。
显然 $U(x) \in \mathbb{P}_n$，且由于将 $x_0, \ldots, x_{n-1}$ 代入 $x$ 会导致行列式有两列相同，故 $U(x)$ 在这些点处为零。
因此 $U(x) = A \prod_{i=0}^{n-1}(x - x_i)$，其中 $A$ 仅依赖于 $x_0, \ldots, x_{n-1}$。
同时，按最后一列展开 $U(x)$ 可知 $x^n$ 的系数为 $\det V(x_0, x_1, \dots, x_{n-1})$。
于是得到递推式，结合 $U(x_0, x_1) = x_1 - x_0$ 归纳即得 (6.2)。$\square$ 

**※【定理】** 多项式插值的唯一性(Uniqueness of polynomial interpolation)：
给定互不相同的点 $x_0, x_1, \ldots, x_n \in \mathbb{C}$ 及对应的值 $f_0, f_1, \dots, f_n \in \mathbb{C}$。
记 $\mathbb{P}_n$ 为次数不超过 $n$ 的多项式类。
则存在唯一的多项式 $p_n(x) \in \mathbb{P}_n$ 使得 $\forall i = 0, 1, \dots, n, \quad p_n(x_i) = f_i$ (6.4)

**证明**：设多项式为 $\sum_{i=0}^n a_i x^i$，代入插值条件得到一个关于系数 $a_i$ 的 $n+1$ 阶线性方程组，其系数矩阵的行列式即为范德蒙行列式。由于节点互不相同，行列式非零，由克莱姆法则可知解存在且唯一。$\square$

## 6.2 The Cauchy remainder

**※【定理】** 广义罗尔定理(Generalized Rolle)：
设 $n \ge 2$，$f \in \mathcal{C}^{n-1}[a,b]$ 且在 $(a,b)$ 上 $f^{(n)}(x)$ 存在。
若 $f(x_0) = f(x_1) = \dots = f(x_n) = 0$ 其中 $a \le x_0 < x_1 < \dots < x_n \le b$，则存在 $\xi \in (x_0, x_n)$ 使得 $f^{(n)}(\xi) = 0$。

**证明**：在 $n$ 个区间 $(x_i, x_{i+1})$ 上应用罗尔定理得到 $n$ 个一阶导数为零的点，反复应用即得。
$g(t)=p(t)+\frac{c}{(n+1)!}\Pi_{n+1}(t)$.

**※【定理】** 多项式插值的柯西余项(Cauchy remainder)：
设 $f \in \mathcal{C}^n[a,b]$，且在 $(a,b)$ 上 $f^{(n+1)}(x)$ 存在。
设 $p_n(f; x)$ 是在 $x_0, \ldots, x_n$ 处与 $f$ 重合的 $\mathbb{P}_n$ 中的唯一多项式。
定义 $R_n(f; x) := f(x) - p_n(f; x)$ (6.5) 为插值的柯西余项。
若 $a \le x_0 < x_1 < \dots < x_n \le b$，则存在 $\xi \in (a,b)$ 使得 $R_n(f; x) = \frac{f^{(n+1)}(\xi)}{(n+1)!} \prod_{i=0}^n (x - x_i)$ (6.6)
其中 $\xi$ 的值依赖于 $x, x_0, \ldots, x_n$ 和 $f$。

**证明**：
构造辅助函数 $W(t) = f(t) - p_n(f; t) - K(x) \prod_{i=0}^n (t - x_i)$，
其中 $K(x)$ 为常数使得 $W(x) = 0$。
则 $W(t)$ 在 $x_0, \ldots, x_n$ 及 $x$ 处共有 $n+2$ 个零点。
由广义罗尔定理，存在 $\xi$ 使得 $W^{(n+1)}(\xi) = 0$。
注意 $p_n^{(n+1)} \equiv 0$ 且 $\prod(t-x_i)$ 的 $(n+1)$ 阶导数为 $(n+1)!$.

$f(x)-p(x)=c(x-x_{0})\dots(x-x_{n})$,
$f^{(n+1)}(x)-p^{(n+1)}(x)=f^{(n+1)}(x)=c(n+1)!$,
故 $f(x)-p(x)= \frac{f^{(n+1)}(\xi)}{(n+1)!}(x-x_{0})\dots(x-x_{n})$.

**※【推论】** 
若 $f(x) \in \mathcal{C}^{n+1}[a,b]$，则 $|R_n(f; x)| \le \frac{M_{n+1}}{(n+1)!} \prod_{i=0}^n |x - x_i| \le \frac{M_{n+1}}{(n+1)!} (b-a)^{n+1}$ (6.7) 其中 $M_{n+1} = \max_{x \in [a,b]} |f^{(n+1)}(x)|$。

**※【例题】** 
已知 $x=0.5330$ 和 $x=0.5340$ 处的值，通过线性插值求 $\arcsin(0.5335)$，估计误差。
设 $f(x) = \arcsin(x)$。
计算知 $f''(x) = x(1-x^2)^{-3/2}$ 在 $[0.5330, 0.5340]$ 上递增，故 $M_2 = f''(0.5340)$。
由推论6.8，$|R_1| \le \frac{M_2}{2!} \max |(x-x_0)(x-x_1)| \approx 4.42 \times 10^{-7}$。真实误差约为 $1.10 \times 10^{-7}$。

## 6.3 The Lagrange formula

**※【定义】** 拉格朗日公式(Lagrange formula)：
为了在互不相同的点 $x_0, \ldots, x_n$ 处插值给定值 $f_0, \ldots, f_n$，拉格朗日公式为 $p_n(x) = \sum_{k=0}^n f_k(x) \ell_k(x)$ (6.8)
其中点态插值的基本多项式（或拉格朗日基函数）$\ell_k(x)$ 为 $\ell_k(x) = \prod_{i \neq k; i=0}^n \frac{x - x_i}{x_k - x_i}$ (6.9) 特别地，当 $n=0$ 时，$\ell_0 = 1$。

**※【例题】** 给定点 $x_i = 1, 2, 4$ 及 $f(x_i) = 8, 1, 5$。
拉格朗日公式生成的插值多项式为 $p_2(x) = 3x^2 - 16x + 21$。

**※【引理】** 
对于 $n$ 个不同的坐标，定义对称多项式 $\pi_n(x) = \prod_{i=0}^{n-1} (x - x_i)$ (6.10)（当 $n=0$ 时为1）。
则对于 $n > 0$，基本多项式可表示为 $\forall x \neq x_k, \quad \ell_k(x) = \frac{\pi_{n+1}(x)}{(x - x_k)\pi_{n+1}'(x_k)}$ (6.11)

**※【引理】** 柯西关系(Cauchy relations)：
基本多项式满足 $\sum_{k=0}^n \ell_k(x) \equiv 1$ (6.12) $\forall j = 1, \dots, n, \quad \sum_{k=0}^n (x_k - x)^j \ell_k(x) \equiv 0$ (6.13)

**证明**：常数函数 $f(x) \equiv 1$ 的插值多项式显然就是 1 本身，代入拉格朗日公式即得 (6.12)。对于 $j \ge 1$，考虑函数 $q_j(u) = (u-x)^j$ 的插值，因为次数 $j \le n$，所以插值多项式就是 $q_j(u)$ 本身，取 $u=x$ 即得 (6.13)。

## 6.4 The Newton formula

**※【定义】** 差商(divided difference)与牛顿公式(Newton formula)：
在点 $x_0, \ldots, x_n$ 处插值的牛顿公式为 $p_n(x) = \sum_{k=0}^n a_k \pi_k(x)$ (6.14)
其中 $\pi_k$ 见 (6.10)，第 $k$ 阶差商 $a_k$ 定义为 $p_k(f; x)$ 中 $x^k$ 的系数，记作 $f[x_0, x_1, \dots, x_k]$。特别地，$f[x_0] = f(x_0)$。

**※【推论】** 差商具有对称性：
若 $(i_0, i_1, \ldots, i_k)$ 是 $(0, 1, \ldots, k)$ 的任意排列，则 $f[x_0, x_1, \ldots, x_k] = f[x_{i_0}, x_{i_1}, \ldots, x_{i_k}]$ (6.15)

**※【推论】** 第 $k$ 阶差商可显式表示为
$f[x_0, x_1, \dots, x_k] = \sum_{i=0}^k \frac{f_i}{\prod_{j \neq i; j=0}^k (x_i - x_j)} = \sum_{i=0}^k \frac{f_i}{\pi_{k+1}'(x_i)}$ (6.16)

**※【定理】** 差商满足递推关系：
$f[x_0, x_1, \dots, x_k] = \frac{f[x_1, x_2, \dots, x_k] - f[x_0, x_1, \dots, x_{k-1}]}{x_k - x_0}$ (6.17)

**证明**：通过构造多项式 $P(x) = P_1(x) + \frac{x-x_0}{x_k-x_0}(P_2(x) - P_1(x))$，验证其满足所有插值条件，且最高次项系数由 $P_2$ 和 $P_1$ 的差提供，从而得出递推关系.

**※【定义】** 差商表(divided difference table)：
第 $k$ 阶差商通过计算左下方相邻两项之差除以上方与下方 $x$ 值之差得出。通常以表格形式呈现。

**※【例题】** 
已知表格：
$x$: 0, 1, 2, 3
$f(x)$: 6, -3, -6, 9
构建差商表并写出牛顿插值多项式。
解：由递推公式构建差商表，对角线元素为 $f[x_0]=6$, $f[x_0,x_1] = -9$, $f[x_0,x_1,x_2] = 3$, $f[x_0,x_1,x_2,x_3] = 2$。
插值多项式为：$p_3(x) = 6 - 9x + 3x(x-1) + 2x(x-1)(x-2)$。估算 $f(1.5) \approx p_3(1.5) = -6$。

**※【定理】** 
对于不同点，有
$f(x) = f[x_0] + f[x_0, x_1](x-x_0) + \dots + f[x_0, \dots, x_n]\prod_{i=0}^{n-1}(x-x_i) + f[x_0, \dots, x_n, x]\prod_{i=0}^n (x-x_i)$ (6.20)

**※【命题】**
$\sum_{k=0}^nf[x_{0},\dots,x_{k}]\pi_{k}(x)=\sum_{k=0}^nf(x_{k})\mathscr{l}_{k}(x)$.

**※【推论】** 
$f[x_{0},\dots,x_{k}]=\sum_{k=1}^n \frac{f(x_{k})}{(x_{k}-x_{1})\dots(x_{k}-x_{k-1})(x_{k}-x_{k+1})\dots(x_{k}-x_{n})}$.

**※【推论】** 
若 $f \in \mathcal{C}^n[a,b]$ 且 $f^{(n+1)}$ 存在，对于 $a=x_0 < x_1 < \dots < x_n=b$，
存在 $\xi(x) \in (a,b)$ 使得 $f[x_0, x_1, \dots, x_n, x] = \frac{1}{(n+1)!} f^{(n+1)}(\xi(x))$ (6.21)

**※【推论】** 
若 $x_0 < x_1 < \dots < x_n$ 且 $f \in \mathcal{C}^n[x_0, x_n]$，有 $\lim_{x_n \to x_0} f[x_0, x_1, \dots, x_n] = \frac{1}{n!} f^{(n)}(x_0)$ (6.22)

**※【推论】** 
$f(x)-p_{n}(x)=f[x,x_{0},\dots,x_{n}]\pi_{n+1}(x)$.

**※【定义】** 双序列(bisequence) 前后向位移 前后向差分
双序列(bisequence)是一个函数 $f: \mathbb{Z} \to \mathbb{R}$。
前向移位 $E$ 和后向移位 $B$ 定义为 $(Ef)(i) = f(i+1), (Bf)(i) = f(i-1)$ (6.23)。
前向差分 $\Delta$ 和后向差分 $\nabla$ 为 $\Delta = E - I, \nabla = I - B$ (6.24)。

*※【例题】* 
$(\Delta f)_{i}=f_{i+1}-f_{i},(Ef)_{i}=f_{i+1}$.
$\Delta^nf=(E-I)^nf=\sum_{k=0}^n{n\choose k}E^k(-I)^{n-k}f$.

**※【定理】** 
前向差分可显式表示为 $\Delta^n f_i = \sum_{k=0}^n (-1)^{n-k} \binom{n}{k} f_{i+k}$ (6.28)

**※【定理】** 
在等距网格 $x_i = x_0 + ih$ 上，有 $\forall n \in \mathbb{N}^+, \quad f[x_0, x_1, \dots, x_n] = \frac{\Delta^n f_0}{n! h^n}$ (6.30)
证明：
$f[x_{0},\dots,x_{n}]=\sum_{k=0}^n \frac{f(x_{k})}{\pi _{n+1}'(x_{k})}$,
$\pi _{n+1}'(x_{k})=k!(n-k)!(-1)^{n-k}h^n$,
$f[x_{0},\dots,x_{n}]=\sum_{k=0}^n \frac{f(x_{k})}{k!(n-k)!(-1)^{n-k}h^n}=\sum_{k=0}^n(-1)^{n-k}{n\choose k}\frac{f(x_{k})}{n!h^n}$,
$f[x_{0},\dots,x_{n}]=\frac{(\Delta^nf)_{0}}{n!h^n}$.

**※【定理】** 牛顿前向差分公式(Newton's forward difference formula)：
设 $p_n$ 在等距网格上插值，则
$\forall s \in \mathbb{R}, \quad p_n(f; x_0 + sh) = \sum_{k=0}^n \binom{s}{k} \Delta^k f_0$ (6.32)
其中 $\binom{s}{k} = \frac{s(s-1)\cdots(s-k+1)}{k!}$ (6.33)。

**※【例题】** 设 $f(x)=x^2$，步长 $h=1$，求 $\Delta^2 f_0$ 以验证前向差分和差商的关系。
计算：$f_0=0, f_1=1, f_2=4$。$\Delta f_0 = 1, \Delta f_1 = 3$。$\Delta^2 f_0 = \Delta f_1 - \Delta f_0 = 2$。
差商 $f[x_0, x_1, x_2] = \frac{1-0}{1} = 1$ 的差商为 $(3-1)/2 = 1$。由公式 (6.30)，$f[x_0,x_1,x_2] = \Delta^2 f_0 / (2! 1^2) = 2/2 = 1$，验证相符。

## 6.5 The Neville-Aitken algorithm

**※【定理】** 记 $p_0^{[i]} = f(x_i)$。对于 $k=0, \dots, n-1$ 和 $i=0, \dots, n-k-1$，定义
$p_{k+1}^{[i]}(x) = \frac{(x - x_i)p_k^{[i+1]}(x) - (x - x_{i+k+1})p_k^{[i]}(x)}{x_{i+k+1} - x_i}$ (6.34)
则 $p_k^{[i]}$ 是在点 $x_i, \dots, x_{i+k}$ 处的插值多项式。特别地，$p_n^{[0]}$ 是这 $n+1$ 个点的插值多项式。

**※【例题】** 直接通过 Neville 算法计算 $x=1.5$ 处的插值，只需代入节点值进行递推计算，无需得出多项式的代数表达式。该算法常用于只需要某一点函数值的情况，但数值运算过程中可能会损失部分精度。

## 6.6 The Runge phenomenon

$\parallel f(x)-p_{n}(x)\parallel\leq \frac{1}{(n+1)!}\parallel f^{(n+1)}(\xi)\parallel \parallel \pi_{n+1}(x)\parallel$.
$\min_{a\leq x_{0},\dots,x_{n}\leq b}\parallel \pi _{n+1}\parallel_{C[a,b]}=\min\max_{a\leq x\leq b}|(x-x_{0})\dots(x-x_{n})|=\max_{-1\leq x\leq 1} | \frac{T_{n+1}(x)}{2^n} |$.
这里用了Chebyshev多项式的放缩。
零点的确定可以直接用Chebyshev多项式三角函数的定义，$x_{i}=\cos \frac{(2i+1)\pi}{2(n+1)}$。

**※【例题】** 定理 6.5 中的节点 $x_i$ 通常是先验给定的，例如在区间上均匀分布。
随着 $n$ 增大，插值多项式次数增加。理想情况下我们希望 $\lim_{n \to \infty} p_n(f; x) = f(x)$ 对所有连续函数成立。
然而，对于等距节点，这是不成立的。著名的龙格现象(Runge's example)展示了在区间端点附近的剧烈振荡。
例如对 $f(x) = \frac{1}{1+x^2}$ 在 $[-5,5]$ 上等距插值。$|f^{(n)}(0)|=n!,n\%2=0$

**※【例题】** 某些函数无法用多项式很好地逼近。
例如原点处有尖点的圆弧函数 $y = H(x)$ 在原点的高阶导数呈阶乘级别增长，导致插值误差随阶数升高而不收敛。

**※【推论】** 如果在切比雪夫多项式的零点处进行多项式插值，
柯西余项满足 $|R_n(f; x)| \le \frac{1}{2^n(n+1)!} \max_{x \in [-1,1]} |f^{(n+1)}(x)|$ (6.38)

## 6.7 The Lebesgue constant

**※【定义】** 多项式插值算子 $\mathcal{L}: \mathcal{C}[a,b] \to \mathbb{P}_n$ 定义为 $\mathcal{L}f := p_n(f, x)$ (6.39)。

**※【引理】** 多项式插值算子 $\mathcal{L}$ 是线性的，且关于无穷范数有界。证明依赖于拉格朗日基函数绝对值之和的最大值。

**※【定义】** 勒贝格常数(Lebesgue constant)：给定插值节点，插值算子 $\mathcal{L}$ 的勒贝格常数定义为
$\Lambda_n := \|\mathcal{L}\|_\infty = \sup_{f \neq 0, f \in \mathcal{C}[a,b]} \frac{\|\mathcal{L}f\|_\infty}{\|f\|_\infty}$ (6.41)

**※【定义】** 勒贝格函数(Lebesgue function)：给定节点，插值算子的勒贝格函数为
$\lambda(x) = \lambda_n(x; x_0, \dots, x_n) := \sum_{k=0}^n |\ell_k(x)|$ (6.42)

**※【引理】** 勒贝格常数与勒贝格函数的关系为 $\Lambda_n = \max_{x \in [a,b]} \lambda(x)$ (6.43)。

**※【定理】** 设 $p^*$ 为 $f$ 的最佳逼近多项式。设 $p = \mathcal{L}f$ 为在给定节点上的插值多项式。则
$\|f - p\|_\infty \le (\Lambda_n + 1) \|f - p^*\|_\infty$ (6.45)

**※【定理】** 插值误差受勒贝格常数控制。若节点等距，$\Lambda_n$ 随 $n$ 指数增长（如 $\Lambda_n \sim \frac{2^{n+1}}{en \log n}$）；若节点为切比雪夫零点，$\Lambda_n$ 仅对数增长（$\Lambda_n \sim \frac{2}{\pi} \log n$）。这就解释了为什么切比雪夫节点能有效避免龙格现象。

## 6.8 The Hermite interpolation

**※【定义】** 埃尔米特插值问题(Hermite interpolation problem)：
给定相异点 $x_i$、非负整数 $m_i$ 以及函数 $f \in \mathcal{C}^M$，
寻找一个最低次数的多项式 $p$ 使得 $\forall i = 0,\dots,k, \forall \mu = 0,\dots,m_i, p^{(\mu)}(x_i) = f_i^{(\mu)}$ (6.49)
即在节点处不仅匹配函数值，还匹配直到 $m_i$ 阶的导数值。

**※【定理】** 埃尔米特插值问题存在唯一解。
**证明**：设总条件数为 $N+1$，若该齐次方程组有非零解，则该多项式有根 $x_i$ 且重数至少为 $m_i+1$。
因此 $\prod (x-x_i)^{m_i+1}$ 是它的因子，但该因子次数为 $N+1$，大于假设多项式次数 $N$，矛盾，故解唯一。

**※【定义】** 广义差商(Generalized divided difference)：
在带有重节点的埃尔米特插值多项式中，最高次项 $x^N$ 的系数即为广义差商。

**※【推论】** 
在 $n+1$ 个相同的重合点处的差商为 $f[x_0, \dots, x_0] = \frac{1}{n!} f^{(n)}(x_0)$ (6.50)

**※【定理】** 埃尔米特插值的余项：
设 $N = k + \sum m_i$，若 $f^{(N+1)}$ 存在，存在 $\xi \in (a,b)$ 使得
$f(x) - p_N(f; x) = \frac{f^{(N+1)}(\xi)}{(N+1)!} \prod_{i=0}^k (x - x_i)^{m_i+1}$ (6.51)

证明：可以推出$f(x) - p_N(f; x) = C\prod_{i=0}^k (x - x_i)^{m_i+1}$，然后两边求 $N+1$ 次导数来确定 $C$.

**※【例题】** 若只给出 $p(x_0)=f_0, p'(x_0)=f'_0, p''(x_0)=f''_0$，牛顿公式给出的插值多项式就是泰勒多项式 $p(x) = f_0 + f'_0(x-x_0) + \frac{1}{2} f''_0(x-x_0)^2$。可见泰勒展开是埃尔米特插值的特例，其余项公式也退化为泰勒余项。

**※【例题】** 当给出不同点的函数值和导数值时（如 $x_0$ 给值，$x_1$ 给值及导数），同样可以构造差商表，只是在同一节点重复写出函数值并在差商时替换为该阶导数值（如 $f[x_1, x_1] = f'_1$）。

## 6.9 Hermite splines

**※【定义】** 三次埃尔米特样条(cubic Hermite spline)：
是由分段三次多项式组成且属于 $\mathcal{C}^1$ 的函数，用于求解每个节点同时给出函数值和一阶导数值的埃尔米特插值问题。

**※【引理】** 给定节点 $x_i$ 及函数值 $f_i$、导数值 $f'_i$，三次埃尔米特样条 $p_f$ 在区间 $[x_j, x_{j+1}]$ 上的表达式可使用 Bernstein 基底 $b_{3,k}$ 给出：
$p_j(x) = \sum_{k=0}^3 \beta_k b_{3,k}(\frac{x - x_j}{x_{j+1} - x_j})$ (6.52a)
其中系数为 $\beta_0 = f_j$, $\beta_1 = f_j + \frac{1}{3} f'_j(x_{j+1} - x_j)$, $\beta_2 = f_{j+1} - \frac{1}{3} f'_{j+1}(x_{j+1} - x_j)$, $\beta_3 = f_{j+1}$。

## 6.10 Bezier curves

**※【定义】** 贝塞尔曲线(Bézier curve)：
由 $n+1$ 个不同点 $\mathbf{q}_0, \dots, \mathbf{q}_n \in \mathbb{R}^D$ 定义的贝塞尔曲线为
$\mathbf{B}(t) := \sum_{k=0}^n \mathbf{q}_k b_{n,k}(t)$ (6.53)
其中 $b_{n,k}$ 为 $n$ 次 Bernstein 基多项式，点 $\mathbf{q}_k$ 称为控制点(control points)。

**※【引理】** 贝塞尔曲线完全包含在这些控制点的凸包(convex hull)内。

**※【引理】** 次数为 $n$ 的贝塞尔曲线满足端点性质：
$\mathbf{B}(0) = \mathbf{q}_0, \quad \mathbf{B}'(0) = n(\mathbf{q}_1 - \mathbf{q}_0)$ (6.54a)
$\mathbf{B}(1) = \mathbf{q}_n, \quad \mathbf{B}'(1) = n(\mathbf{q}_n - \mathbf{q}_{n-1})$ (6.54b)

**※【算法】** 曲线拟合算法(Curve fitting via Cubic Bézier curves)：给定曲线 $\gamma$ 及切线向量 $\gamma'$，可以使用分段三次贝塞尔曲线进行逼近。选取标记点 $\mathbf{p}_j$ 后，构造每个区间的控制点为：
$\mathbf{q}_0 = \mathbf{p}_j$, $\mathbf{q}_1 = \frac{1}{3}\gamma'(\mathbf{p}_j) + \mathbf{p}_j$, $\mathbf{q}_2 = \mathbf{p}_{j+1} - \frac{1}{3}\gamma'(\mathbf{p}_{j+1})$, $\mathbf{q}_3 = \mathbf{p}_{j+1}$。
连接所有分段即得光滑的贝塞尔曲线拟合。

## 6.11 chebyshev interpolation nodes

**※【定义】** 切比雪夫多项式 $T_n(x)$ 
- **定义一（三角函数形式）：**
	- 在区间 $x \in [-1, 1]$ 上，令 $x = \cos\theta$（即 $\theta = \arccos x$）
	- 则 $n$ 阶切比雪夫多项式定义为：$T_n(x) = \cos(n \arccos x), \quad n = 0, 1, 2, \dots$
- **定义二（递推形式）：**
	- 利用三角恒等式 $\cos((n+1)\theta) + \cos((n-1)\theta) = 2\cos\theta \cos(n\theta)$，
	- 可以推导出递推关系：$T_0(x) = 1$, $T_1(x) = x$, $T_{n+1}(x) = 2x T_n(x) - T_{n-1}(x), \quad n \ge 1$

*※【例题】* 根据递推公式，可以写出前几个切比雪夫多项式：
*   $T_0(x) = 1$
*   $T_1(x) = x$
*   $T_2(x) = 2x^2 - 1$
*   $T_3(x) = 4x^3 - 3x$
*   $T_4(x) = 8x^4 - 8x^2 + 1$

**※【命题】** 在 $[ −1,1]$ 上有界且等幅振荡
因为 $T_n(x) = \cos(n \arccos x)$，而余弦函数的值域是 $[-1, 1]$，
所以：$|T_n(x)| \le 1, \quad \forall x \in [-1, 1]$. 它在 $[-1, 1]$ 之间不断在 $-1$ 和 $1$ 之间来回振荡。

**※【命题】** 极值点（交错点）
当 $n\theta = k\pi$ 时，$\cos(n\theta) = \pm 1$。
因此，$T_n(x)$ 在区间 $[-1, 1]$ 内有 $n+1$ 个极值点：$x_k = \cos\left(\frac{k\pi}{n}\right), \quad k = 0, 1, \dots, n$
并且在这些点上，$T_n(x_k) = (-1)^k$。**交替取到最大值 1 和最小值 -1**。

**※【命题】** 零点（用于插值节点）
当 $n\theta = \frac{2k-1}{2}\pi$ 时，$\cos(n\theta) = 0$。
因此，$T_n(x)$ 在 $(-1, 1)$ 内有 $n$ 个单根：$x_k = \cos\left(\frac{2k-1}{2n}\pi\right), \quad k = 1, 2, \dots, n$
这被称为**切比雪夫节点（Chebyshev nodes）**。

**※【命题】** 最高次项系数
从 $T_1(x)=x$ 开始，每一次递推，最高次项都会乘上一个 $2x$。
因此，对于 $n \ge 1$，$T_n(x)$ 的首项（$x^n$）系数是：$a_n = 2^{n-1}$

**※【命题】** 正交性
切比雪夫多项式在区间 $[-1, 1]$ 上带权 $\frac{1}{\sqrt{1-x^2}}$ 正交：$\int_{-1}^1 \frac{T_m(x) T_n(x)}{\sqrt{1-x^2}} dx = \begin{cases} 0, & m \neq n \\ \pi, & m = n = 0 \\ \frac{\pi}{2}, & m = n \neq 0 \end{cases}$

**※【定理】**  极小极大性质（Minimax Property）
对于任何首一多项式 $P_n(x) = x^n + c_{n-1}x^{n-1} + \dots + c_0$，其在 $[-1, 1]$ 上的最大绝对值满足：$\max_{x \in [-1, 1]} |P_n(x)| \ge \frac{1}{2^{n-1}}$ 且等号成立当且仅当 $P_n(x) = \frac{1}{2^{n-1}} T_n(x)$。

**证明思路：**
利用反证法。假设存在另一个首一多项式 $Q_n(x)$，使得 $\max|Q_n(x)| < \frac{1}{2^{n-1}}$。
构造 $R(x) = \frac{1}{2^{n-1}}T_n(x) - Q_n(x)$。由于两者都是首一的，最高次项抵消，$R(x)$ 的次数最高为 $n-1$。
但在 $T_n(x)$ 的 $n+1$ 个极值点 $x_k = \cos(k\pi/n)$ 处，$\frac{1}{2^{n-1}}T_n(x_k) = \pm \frac{1}{2^{n-1}}$。而 $|Q_n(x_k)| < \frac{1}{2^{n-1}}$。
所以 $R(x_k)$ 的符号一定与 $\frac{1}{2^{n-1}}T_n(x_k)$ 相同，意味着 $R(x)$ 在这 $n+1$ 个点上正负交替。由介值定理，$R(x)$ 在 $(-1, 1)$ 内至少有 $n$ 个零点。但一个 $n-1$ 次多项式最多只能有 $n-1$ 个零点，矛盾！


插值不可能永远只在 $[-1, 1]$ 上做，对于任意区间 $[a, b]$，只需要做一个简单的线性映射：
$x = \frac{a+b}{2} + \frac{b-a}{2}t, \quad t \in [-1, 1]$ 在这个区间上，最小的最大绝对误差（即你刚才第2题的解）就是：
$\max_{x \in [a, b]} \left| \prod_{i=0}^n (x - x_i) \right| = \frac{(b-a)^{n+1}}{2^{2n+1}}$ 而对应的**切比雪夫节点**就是：$x_k = \frac{a+b}{2} + \frac{b-a}{2} \cos\left(\frac{2k+1}{2(n+1)}\pi\right)$

