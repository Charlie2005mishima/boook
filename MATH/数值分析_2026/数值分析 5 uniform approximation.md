
**※【定义】** 最佳逼近(Best Approximation)：
给定一个函数赋范向量空间 $Y$ 及其子空间 $X \subset Y$。
如果函数 $\hat{\varphi} \in X$ 满足 $\forall \varphi \in X, \|f - \hat{\varphi}\| \leq \|f - \varphi\|$ (5.1)，则称 $\hat{\varphi}$ 为 $f \in Y$ 在 $X$ 中关于该范数的最佳逼近。

**※【定义】** 线性逼近的基本问题：
给定 $n$ 个线性无关且先验给定的元素 $u_1, u_2, \ldots, u_n \in X \subset Y$，寻找 $f \in Y$ 的最佳逼近 $\hat{\varphi} = \sum_{i=1}^n a_i u_i$。

**※【例题】** 对于 $\mathcal{C}^\infty[-1,1]$ 中的 $f(x) = e^x$，在子空间 $X = \text{span}\{1, x, x^2, \ldots\}$ 中寻求形式为 $\hat{\varphi} = \sum_{i=1}^n a_i u_i$ 的最佳逼近，就是一个线性逼近问题。其中 $n$ 可以是任意正整数，范数可以是最大范数 $\|g\|_\infty := \max_{x\in[-1,1]}|g(x)|$、1-范数 $\|g\|_1 := \int_{-1}^1 |g(x)|\mathrm{d}x$ (5.2) 或 2-范数 $\|g\|_2 := (\int_{-1}^1 |g(x)|^2\mathrm{d}x)^{1/2}$ (5.3)。

**※【定义】** 最大范数(Max-norm)或一致范数(Uniform norm)：
对于 $f \in \mathcal{C}[a,b]$，定义 $\|f\|_\infty := \max_{x\in[a,b]}|f(x)|$ (5.4)。
此时 $(\mathcal{C}[a,b], \|\cdot\|_\infty)$ 是一个赋范向量空间。

**※【定义】** 上确界范数(Sup-norm)：
设 $B[a,b]$ 表示所有有界函数 $f: [a,b] \to \mathbb{R}$ 构成的向量空间。
对于 $f \in B[a,b]$，定义 $\|f\|_\infty := \sup_{x\in[a,b]}|f(x)|$ (5.5)。
若 $f$ 连续，上确界可以取到，此时与最大范数一致。

**※【定义】** 最佳一致逼近(Best uniform approximation)或极小极大逼近(Minimax approximation)：
指关于最大范数或上确界范数的最佳逼近。

**※【例题】** 设 $Y = \mathbb{P}_n$，$X = \mathbb{P}_{n-1}$。对于 $f(x) = -x^n$，其在 $Y$ 中从 $X$ 寻找的最佳一致逼近为 $\hat{\varphi} = \frac{T_n}{2^{n-1}} - x^n$，其中 $T_n$ 是 $n$ 次切比雪夫多项式（见定理 5.88）。

## 5.1 最佳逼近的存在性与唯一性

**※【定理】** （最佳逼近的存在性）
设 $X$ 是赋范向量空间 $(Y, \|\cdot\|)$ 的**有限**维子空间。
则 $\forall y \in Y, \exists \hat{\varphi} \in X \text{ s.t. } \forall \varphi \in X, \|\hat{\varphi} - y\| \leq \|\varphi - y\|$ (5.6)。

**证明**：对于给定 $y \in Y$，定义闭球 $B_y := \{x \in X : \|x\| \leq 2\|y\|\}$。显然 $0 \in B_y$ 且 $y$ 到 $B_y$ 的距离小于等于 $\|y\|$。若 $z \notin B_y$，则 $\|z - y\| \geq \|z\| - \|y\| > \|y\|$。因此若最佳逼近存在，必在 $B_y$ 内。$B_y$ 作为有限维空间 $X$ 的有界闭子集，是紧集。由广义极值定理，连续函数 $d(x) = \|x - y\|$ 必在 $B_y$ 上取得最小值. 

**证明**：令 $\varphi _{n}\in X,d\leq\|f-\varphi _{n}\|\leq d+\frac{1}{n}$. $\|\varphi _{n}\|\leq\|f-\varphi _{n}\|+\|f\|<d+1+\|f\|$，则称 $\{\varphi _{n}\}$ 在 $X$ 中是紧的，且 $X$ 为有限维空间，则 $\varphi_{n_{k}}\to\varphi_{\star}$ 在 $X$ 中. 则存在$\varphi_{\star}$使得$\|f-\varphi_{\star}\|=\min_{\varphi\in X}\|f-\varphi\|$. $\square$ 

**※【定理】** 对于赋范向量空间 $Y$ 的子空间 $X$，$f \in Y$ 在 $X$ 中的最佳逼近集合 $U$ 是凸集。

**证明**：若 $U$ 非空，设 $\varphi_1, \varphi_2 \in U$，令 $\rho := \min_{\varphi \in X} \|f - \varphi\|$。对于 $0 \le t \le 1$，有 $\rho \leq \|f - [t\varphi_1 + (1-t)\varphi_2]\| \leq t\|f - \varphi_1\| + (1-t)\|f - \varphi_2\| = \rho$。因此 $t\varphi_1 + (1-t)\varphi_2 \in U$。\(\square\)

**※【定义】** 严格凸集(Strictly convex set)：
集合 $U \subset Y$ 是严格凸的，如果它是凸集，且对于任意边界上的相异两点 $x, y \in \partial U$，开线段 $\{tx + (1-t)y : 0 < t < 1\}$ 不与 $\partial U$ 相交。

**※【定义】** 严格凸范数(Strictly convex norm) 严格凸空间：
若赋范空间的闭单位球 $B = \{v \in Y : \|v\| \leq 1\}$ 是严格凸集，则称该范数为严格凸范数。
对应空间为严格凸空间。

**※【引理】** 赋范向量空间 $Y$ 是严格凸的，当且仅当对任意满足 $\|v_1\| = \|v_2\| = 1$ 的 $v_1 \neq v_2 \in Y$，有 $\forall t \in (0,1), \|tv_1 + (1-t)v_2\| < 1$。

**※【定理】** （最佳逼近的唯一性）
对于严格凸空间 $Y$，任意 $f \in Y$ 在任意子空间 $X$ 中至多有一个最佳逼近。

**证明**：假设存在两个不同的最佳逼近 $\varphi_1, \varphi_2$，设 $\rho = \|f - \varphi_1\| = \|f - \varphi_2\| > 0$。定义 $\phi_1 = (f - \varphi_1)/\rho$，$\phi_2 = (f - \varphi_2)/\rho$，其范数均为 1。由严格凸性 $1 > \|\frac{\phi_1 + \phi_2}{2}\| = \frac{1}{\rho}\|f - \frac{\varphi_1 + \varphi_2}{2}\|$，即 $\|f - \frac{\varphi_1 + \varphi_2}{2}\| < \rho$，与假设矛盾。\(\square\)

**※【定义】** 连续函数的范数
在连续函数空间上的 $p$-范数：$\|f\|_{p}=(\int_{S} |f(x)|^p \, dx)^{1/p}$.

**※【定理】** 定义在连续函数空间上的 $p$-范数的凸性
对于 $1 < p < \infty$，定义在连续函数空间上的 $p$-范数是严格凸范数。
换言之，$p=1,\infty$，定义在连续函数空间上的 $p$-范数并不是是严格凸范数。

**※【例题】** 在 $(\mathcal{C}[-1,1], \|\cdot\|_1)$ 中，$f_1(x) = \frac{3}{2}x^2$ 和 $f_2(x) = \frac{3}{4}(1-x^2)$ 满足 $\|f_1\|_1 = \|f_2\|_1 = \|(f_1+f_2)/2\|_1 = 1$。因此 1-范数不是严格凸的。

## 5.2 最佳一致逼近

**※【定理】** 最佳一致逼近的存在性：
对于任意 $f \in \mathcal{C}[a,b]$（或 $f \in B[a,b]$），在 $\mathbb{P}_n$ 中至少存在一个最佳一致逼近。

**※【定义】** 交错(Equioscillates)：
函数 $E$ 在 $[a,b]$ 上的 $m$ 个点交错，如果存在点 $a \leq x_1 < x_2 < \cdots < x_m \leq b$ 使得 $|E(x_k)| = \|E\|_\infty$ ($1 \leq k \leq m$) 且 $E(x_{k+1}) = -E(x_k)$ ($1 \leq k \leq m-1$)。若 $E$ 在 $m+1$ 点不交错，则称其极大交错于 $m$ 个点。

**※【例题】** 函数 $E(x) = |x| - \frac{1}{8} - x^2$ 在 $[-1,1]$ 上的 $x = 0, \pm\frac{1}{2}, \pm1$ 处交错。

**※【例题】** 第一类 $n$ 次切比雪夫多项式 $T_n(x) = \cos(n \arccos x)$ 在 $n+1$ 个点 $x_k' = \cos\frac{k}{n}\pi$ ($k=0, 1, \ldots, n$) 处极大交错。

**※【引理】** 最佳一致逼近 $p_n^*$ 的误差函数 $f - p_n^*$ 至少在 2 点交错。

**※【定理】** 切比雪夫交错定理(Chebyshev alternating)：
多项式 $p_n$ 是 $f \in \mathcal{C}[a,b]$ 在 $\mathbb{P}_n$ 中的最佳一致逼近，当且仅当 $f - p_n$ 在 $[a,b]$ 中的 $n+2$ 个点交错。

**证明**：
充分性：假设 $E = f - p_n$ 在 $n+2$ 个点交错，但 $p_n$ 不是最佳逼近。设 $p_n^*$ 为最佳逼近，则 $\|f - p_n^*\|_\infty < \|E\|_\infty$。因此 $p_n - p_n^*$ 符号由 $E = f - p_n$ 决定，在这些点交替变号，根据介值定理至少有 $n+1$ 个不同的零点，但它是 $n$ 次多项式，只能恒为零，矛盾。
必要性：假设 $p_n$ 是最佳逼近。设 $E$ 极大交错于 $k+1$ 个点，假设 $k \leq n$。我们可以构造一个 $k$ 次多项式 $q(x)$，使得在这些极值点处 $q(x)$ 与 $E(x)$ 同号。考虑 $p_n^* = p_n + \varepsilon q$，对于充分小的 $\varepsilon$，可以证明 $\|f - p_n^*\|_\infty < \|E\|_\infty$，从而产生矛盾。所以必定 $k \geq n+1$，即至少在 $n+2$ 点交错。\(\square\)

**※【例题】** 在例题 5.22 中，$E(x) = |x| - 1/8 - x^2$ 在 5 点交错。根据定理 5.25，$p(x) = x^2 + 1/8$ 是 $f(x) = |x|$ 不仅在 $\mathbb{P}_2$ 中，而且在 $\mathbb{P}_3$ 中的最佳一致逼近。这说明最佳逼近的误差函数可能在超过 $n+2$ 个点交错。

**※【定理】** 如果 $f \in \mathcal{C}^{n+1}[a,b]$ 且 $f^{(n+1)}$ 在 $(a,b)$ 内没有零点，则 $f - p_n^*$ 极大交错于 $n+2$ 个点。

**※【定理】** 最佳一致逼近的唯一性：对于任意 $f \in \mathcal{C}[a,b]$，在 $\mathbb{P}_n$ 中的最佳一致逼近是唯一的。

**证明**：假设 $p_n$ 和 $p_n^*$ 是两个不同的最佳逼近。根据凸性，$q = (p_n + p_n^*)/2$ 也是最佳逼近。根据交错定理，$f - q$ 在 $x_1 < \cdots < x_{n+2}$ 处交错。这要求 $f - p_n$ 和 $f - p_n^*$ 在这些点也必须达到最大误差且符号相同。因此 $p_n$ 和 $p_n^*$ 在 $n+2$ 个点取值相同，从而它们必须是同一个多项式，矛盾。\(\square\)

**※【例题】** 对于 $B[a,b]$ 中的函数（如不连续函数），其最佳一致逼近存在但可能不唯一。

**※【定义】** 对于 $f \in \mathcal{C}[a,b]$，其在 $[a,b]$ 上的 $n$ 次极小极大多项式(Minimax polynomial) $p_n^*$ 即为最佳一致逼近。极小极大误差(Minimax error)定义为 $E_n(f) = \|f - p_n^*\|_\infty$ (5.10)。

## 5.3 伯恩斯坦多项式与魏尔斯特拉斯逼近定理

**※【定义】** 伯恩斯坦 **基** 多项式(Bernstein basis polynomials)：
在区间 $[0,1]$ 上的 $n$ 次伯恩斯坦基多项式定义为 $b_{n,k}(t) := \binom{n}{k} t^k (1-t)^{n-k}$ (5.11)，其中 $k = 0, 1, \dots, n$.

**※【例题】** $n=3$ 时的伯恩斯坦基多项式为 $t^3, 3t^2(1-t), 3t(1-t)^2, (1-t)^3$。

**※【定义】** 伯恩斯坦多项式(Bernstein polynomial)是伯恩斯坦基多项式的线性组合，其系数称为伯恩斯坦系数或贝塞尔系数。

**※【引理】** 伯恩斯坦基多项式的基本性质：
- 递推/降阶关系：$b_{n-1,k}(t) = \frac{n-k}{n}b_{n,k}(t) + \frac{k+1}{n}b_{n,k+1}(t)$ (5.12)
- 导数：$b_{n,k}'(t) = n b_{n-1,k-1}(t) - n b_{n-1,k}(t)$ (5.13)
- 积分：$\int_0^1 b_{n,k}(t) \mathrm{d}t = \frac{1}{n+1}$ (5.14)
- $n$ 次伯恩斯坦基构成多项式空间 $\mathbb{P}_n$ 的一组基。
- 非负性与单位分解等恒等式：$b_{n,k}(t) > 0$，$\sum_{k=0}^n b_{n,k}(t) = 1$，$\sum_{k=0}^n k b_{n,k}(t) = nt$，$\sum_{k=0}^n (k-nt)^2 b_{n,k}(t) = nt(1-t)$。

**※【定义】** 函数 $f \in \mathcal{C}[0,1]$ 的 $n$ 次伯恩斯坦多项式为 $(B_n f)(t) := \sum_{k=0}^n f(\frac{k}{n}) b_{n,k}(t)$ (5.17)。

**※【定义】** 单调线性算子：若 $f \ge g \implies Lf \ge Lg$，则算子 $L$ 是单调的。

**※【引理】** 伯恩斯坦算子 $B_n$ 是单调的。

**※【定理】** 魏尔斯特拉斯逼近定理(Weierstrass approximation)：
任何连续函数 $f: [a,b] \to \mathbb{R}$ 都可以被多项式序列一致逼近到任意精度。
即 $\forall f \in \mathcal{C}[a,b], \forall \epsilon > 0, \exists N \in \mathbb{N}^+ \text{ s.t. } \forall n > N, \exists p_n \in \mathbb{P}_n \text{ s.t. } \forall x \in [a,b], |p_n(x) - f(x)| < \epsilon$ (5.19)。

**证明**：不失一般性设 $[a,b] = [0,1]$，取 $p_n = B_n f$。
利用伯恩斯坦基的性质和 $f$ 的一致连续性，可以将 $|(B_n f)(t) - f(t)|$ 的误差分为 $|k/n - t| < \delta$ 和 $\ge \delta$ 两部分。前者由一致连续性控制，后者由方差恒等式 $\sum (k-nt)^2 b_{n,k}(t) = nt(1-t)$ 控制。最终可证得当 $n$ 充分大时，误差小于 $\epsilon$。\(\square\)

**※【推论】** 对于 $f \in \mathcal{C}[a,b]$，有极小极大误差 $E_n(f) \to 0$。

## 5.4 连续模

**※【定义】** 连续模(Modulus of continuity)：
函数 $f$ 在 $[a,b]$ 上的连续模定义为 $\omega_{f,[a,b]}(\delta) = \sup_{x_1,x_2\in[a,b], |x_1-x_2|\le\delta} |f(x_1) - f(x_2)|$ (5.20)。

**※【例题】** 连续模的例子：
- $f(x) = |x|$ 在 $[-1,1]$ 上：$\omega(\delta) = \delta$ (当 $\delta < 1$)。
- 符号函数在 $[-1,1]$ 上：$\omega(\delta) \equiv 2$。
- $f(x) = \sin(1/x)$ 在 $(0,1]$ 上：$\omega(\delta) \equiv 2$。
- $f(x) = 1/x$ 在 $(0,\infty)$ 上：$\omega(\delta) \equiv \infty$。

**※【引理】** 连续模的基本性质：
- $|f(x_1) - f(x_2)| \le \omega_f(|x_1 - x_2|)$。
- $\omega$ 是非减函数。
- $f$ 在 $[a,b]$ 上一致连续当且仅当 $\lim_{\delta \to 0^+} \omega_f(\delta) = 0$。
- 对于任意 $\lambda > 0$，有 $\omega(\lambda\delta) \le (1+\lambda)\omega(\delta)$。

**※【定理】** 若 $f \in B[0,1]$，则 $\|f - B_n f\|_\infty \le \frac{3}{2}\omega_f(\frac{1}{\sqrt{n}})$ (5.22)。

**※【推论】** 极小极大误差的弱估计：对于 $f \in B[0,1]$，$E_n(f) \le \frac{3}{2}\omega_f(\frac{1}{\sqrt{n}})$ (5.23)。

**※【定义】** 若存在常数 $L>0$ 使得 $|f(x_1) - f(x_2)| \le L|x_1 - x_2|^\alpha$ (5.24)，则称 $f$ 满足 $\alpha$-赫尔德连续($\alpha$-Hölder continuous)（$\alpha=1$ 时为利普希茨连续）。

**※【推论】** 若 $f$ 是 $\alpha$-赫尔德连续的，则 $E_n(f) = O(n^{-\alpha/2})$。

## 5.5 杰克逊定理(Jackson theorems)

**※【定义】** 三角系统(Trigonometric system)：由 $1, \cos x, \sin x, \cos 2x, \sin 2x, \dots$ 构成的正交函数系。

**※【定理】** 三角系统的正交性：在 $[-\pi,\pi]$ 上，不同频率的三角函数的内积为 0，同频率的余弦或正弦函数的内积为 $\pi$。

**※【定义】** 三角多项式(Trigonometric polynomial)：形式为 $T(\theta) = \frac{\alpha_0}{2} + \sum_{k=1}^n (\alpha_k \cos k\theta + \beta_k \sin k\theta)$ 的函数。若 $\beta_k=0$ 则为余弦多项式，$\alpha_k=0$ 则为正弦多项式。

**※【引理】** 三角多项式为偶函数当且仅当它是余弦多项式，奇函数当且仅当它是正弦多项式。

**※【定义】** 傅里叶系数(Fourier coefficients)：对于 $2\pi$ 周期函数 $g \in \mathcal{L}^1[-\pi,\pi]$，定义 $a_k = \frac{1}{\pi}\int_{-\pi}^\pi g(\phi)\cos k\phi \mathrm{d}\phi$，$b_k = \frac{1}{\pi}\int_{-\pi}^\pi g(\phi)\sin k\phi \mathrm{d}\phi$ (5.26)。

**※【定义】** 修改的部分和(Modified partial sum)：$q_n(g;\theta) = \frac{a_0}{2} + \sum_{k=1}^n \rho_{k,n} (a_k \cos k\theta + b_k \sin k\theta)$ (5.27)。

**※【引理】** 修改的部分和可以表示为积分形式 $q_n(g;\theta) = \frac{1}{\pi}\int_{-\pi}^\pi g(\phi+\theta)u_n(\phi)\mathrm{d}\phi$，其中 $u_n$ 为余弦多项式。

**※【定理】** 对于 $2\pi$ 周期函数 $g \in B[-\pi,\pi]$，其在三角多项式空间中的最佳逼近满足 $\|g - q_n^*\|_\infty \le 6\omega_{g,[-\pi,\pi]}(\frac{1}{n})$ (5.37)。

**※【推论】** 任何 $2\pi$ 周期连续函数都可以被三角多项式序列一致逼近（魏尔斯特拉斯定理的三角多项式版本）。

**※【定理】** 对于代数多项式逼近，若 $f \in B[-1,1]$，则 $E_n(f) \le 6\omega_f(\frac{1}{n})$ (5.39)。

**证明**：构造 $g(\theta) = f(\cos\theta)$，将其转化为三角多项式逼近，利用偶数最佳逼近 $q_n^*$，再将其转换为关于 $\cos\theta$ 的多项式 $p_n(\cos\theta)$，从而得到关于 $x$ 的多项式逼近误差界。\(\square\)

**※【推论】** 任意区间 $[a,b]$ 上的连续函数，其极小极大误差满足 $E_n(f; [a,b]) \le 6\omega_f(\frac{b-a}{2n})$。

**※【推论】** 若 $f$ 可导且导数有界，则 $E_n(f) \le 6\|f'\|_\infty n^{-1}$。

**※【定理】** 若 $f$ 是 $k$ 次可导且 $f^{(k)} \in B[-1,1]$，则对于 $n > k$，有 $E_n(f; [-1,1]) \le \frac{6c}{n^k} \omega_{f^{(k)}}(\frac{1}{n-k})$ (5.40)，其中 $c$ 是与 $k$ 相关的常数。

## 5.6 切比雪夫多项式(Chebyshev polynomials)

**※【定义】** 第一类切比雪夫多项式：$T_n(x) = \cos(n \arccos x)$ (5.42)，其中 $x \in [-1,1]$，$n \in \mathbb{N}$。

**※【定理】** 递推关系：$T_{n+1}(x) = 2x T_n(x) - T_{n-1}(x)$ (5.43)。

**※【推论】** $T_n(x)$ 的首项系数为 $2^{n-1}$ ($n>0$)。

**※【定理】** $T_n(x)$ 在 $n$ 个点 $x_k = \cos\frac{2k-1}{2n}\pi$ ($k=1,\dots,n$) 有单零点。在 $n+1$ 个点 $x_k' = \cos\frac{k}{n}\pi$ ($k=0,\dots,n$) 取交替极值 $(-1)^k$。

**Figure 5.1:** Zeros and extrema of Chebyshev polynomials with \(n=4\)

**※【定理】** 切比雪夫定理(Chebyshev)：在所有次数为 $n$ 且最高次项系数为 1 的多项式集合 $\tilde{\mathbb{P}}_n$ 中，$\frac{T_n(x)}{2^{n-1}}$ 在 $[-1,1]$ 上的最大绝对值最小。即对于任意 $p \in \tilde{\mathbb{P}}_n$，有 $\max_{x\in[-1,1]} |\frac{T_n(x)}{2^{n-1}}| \le \max_{x\in[-1,1]} |p(x)|$ (5.46)。

**证明**：假设存在首项系数为 1 的多项式 $p(x)$ 使得 $\max |p(x)| < 1/2^{n-1}$。构造 $Q(x) = \frac{T_n(x)}{2^{n-1}} - p(x)$，由于最高次项抵消，$Q(x)$ 的次数不超过 $n-1$。但在 $T_n(x)$ 的 $n+1$ 个极值点处，$Q(x)$ 的符号交替，这意味着 $Q(x)$ 至少有 $n$ 个零点。一个次数不超过 $n-1$ 的多项式有 $n$ 个零点，只能恒为零。但这与 $p(x)$ 的最大值假设矛盾。\(\square\)

**※【推论】** 对于任何首项系数为 1 的 $n$ 次多项式，其在 $[-1,1]$ 上的最大绝对值至少为 $\frac{1}{2^{n-1}}$。