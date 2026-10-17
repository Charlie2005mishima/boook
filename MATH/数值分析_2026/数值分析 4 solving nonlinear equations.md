
## 4.1 the bisection method

**※【定义】** 标量非线性方程(Scalar nonlinear equation)：
形如 $f(x) = 0$ (4.1) 的方程，其中 $f : \mathbb{R} \to \mathbb{R}$ 是非线性函数。

数值求解的目标是找到 $\alpha \in \mathbb{R}$，称为根或解，使得在给定容差内近似满足 $f(\alpha) = 0$。

**※【算法】** 二分法(Bisection method)：
输入：$f : [a,b] \to \mathbb{R}$，$a \in \mathbb{R}$，$b \in \mathbb{R}$，$M \in \mathbb{N}$，$\delta \in \mathbb{R}^+$，$\epsilon \in \mathbb{R}^+$。
前置条件：$f \in C[a,b]$，$\mathrm{sgn}(f(a)) \neq \mathrm{sgn}(f(b))$。
输出：$c, h, k$。
后置条件：$|f(c)| < \epsilon$ 或 $|h| < \delta$ 或 $k = M$。
步骤：
1. $u \leftarrow f(a)$
2. $v \leftarrow f(b)$
3. for $k = 0 : M$ do
4. $\quad h \leftarrow b - a$
5. $\quad c \leftarrow a + h/2$
6. $\quad$ if $|h| < \delta$ or $k = M$ then break
7. $\quad w \leftarrow f(c)$
8. $\quad$ if $|w| < \epsilon$ then
9. $\quad\quad$ break
10. $\quad$ else if $\mathrm{sgn}(w) \neq \mathrm{sgn}(u)$ then
11. $\quad\quad b \leftarrow c$
12. $\quad\quad v \leftarrow w$
13. $\quad$ else
14. $\quad\quad a \leftarrow c$
15. $\quad\quad u \leftarrow w$
16. $\quad$ end
17. end

数值零点的精度：$|c_{n}-\alpha|\leq \frac{1}{2^{n+1}}|b-a|$.
特点：简单；但是收敛速度不够快

## 4.2 proof of correctness and simplification of algorithms

**※【定义】** 不变量(Invariant)：在算法执行期间始终成立的条件。

**※【定义】** 临时变量与持久变量(Temporary/derived variable, Persistent/primary variable)：在循环内部初始化的变量称为循环的临时变量或导出变量；
在循环之前初始化且其值在不同迭代中变化的变量称为循环的持久变量或主变量。

**※【算法】** 简化的二分法：
输入：$f : [a,b] \to \mathbb{R}$，$a \in \mathbb{R}$，$b \in \mathbb{R}$，$M \in \mathbb{N}$，$\delta \in \mathbb{R}^+$，$\epsilon \in \mathbb{R}^+$。
前置条件：$f \in C[a,b]$，$\mathrm{sgn}(f(a)) \neq \mathrm{sgn}(f(b))$。
输出：$c, h, k$。
后置条件：$|f(c)| < \epsilon$ 或 $|h| < \delta$ 或 $k = M$。
步骤：
1. $h \leftarrow b - a$
2. $u \leftarrow f(a)$
3. for $k = 0 : M$ do
4. $\quad h \leftarrow h/2$
5. $\quad c \leftarrow a + h$
6. $\quad$ if $|h| < \delta$ or $k = M$ then break
7. $\quad w \leftarrow f(c)$
8. $\quad$ if $|w| < \epsilon$ then
9. $\quad\quad$ break
10. $\quad$ else if $\mathrm{sgn}(w) = \mathrm{sgn}(u)$ then
11. $\quad\quad a \leftarrow c$
12. $\quad$ end
13. end

## 4.3 Q-order convergence

**※【定义】** Q 阶收敛(Q-order convergence)：
收敛序列 $\{x_n\}$ 以 $Q$ 阶 $p$ ($p \ge 1$) 收敛到 $L$，
若极限 $\lim_{n \to \infty} \frac{|x_{n+1}-L|}{|x_n-L|^p}$ 存在且 $\lim_{n \to \infty} \frac{|x_{n+1}-L|}{|x_n-L|^p} = c > 0$ (4.2)
常数 $c$ 称为渐近因子。特别地，$p=1$ 时为 Q 线性收敛，$p=2$ 时为 Q 二次收敛。

**※【例题】** 考虑求 $f(x) = \sin(x) = 0$ 在 $x \approx 3$ 附近的根。
若 $c = \frac{1}{10}$，则线性收敛方法每次迭代增加一位正确十进制数字。
若 $c = \frac{1}{2}$，则每次迭代增加一位正确二进制位。对于二次收敛方法，每次迭代正确数字/位数大约翻倍。

**※【定义】** 线性收敛：
迭代序列 $\{x_n\}$ **线性收敛**到 $L$，
若 $\exists c \in (0,1), \exists d > 0, \exists N \in \mathbb{N} \text{ s.t. } \forall n > N, |x_n - L| \leq c^n d$ (4.3)

**※【定义】** 收敛阶：
对于收敛到 $L$ 的序列 $\{x_n\}$，其**收敛阶**是满足下式的最大 $p \in \mathbb{R}^+$：
$\exists c > 0, \exists N \in \mathbb{N} \text{ s.t. } \forall n > N, |x_{n+1} - L| \leq c |x_n - L|^p$ (4.4)
特别地，若 $p=2$，则二次收敛。

**※【例题】** (4.3) 保证收敛，而 (4.4) 不一定：序列 $\{1, -1, 1, -1, \dots\}$ 对 $L=0, p=1, c=1$ 满足 (4.4)，但不收敛。

**※【定理】** 单调序列定理(Monotonic sequence theorem)：
每个有界单调序列都收敛。

**※【定理】** 二分法的收敛性(Convergence of bisection)：
对于连续函数 $f : [a_0,b_0] \to \mathbb{R}$ 满足 $\mathrm{sgn}(f(a_0)) \neq \mathrm{sgn}(f(b_0))$，
二分法中的迭代序列**线性收敛**，渐近因子为 $\frac{1}{2}$：
$\lim_{n \to \infty} a_n = \lim_{n \to \infty} b_n = \lim_{n \to \infty} c_n = \alpha$ (4.5)
$f(\alpha) = 0$ (4.6)
$|c_n - \alpha| \leq 2^{-(n+1)}(b_0 - a_0)$ (4.7)
其中 $[a_n,b_n]$ 是第 $n$ 次迭代的区间，$c_n = \frac{1}{2}(a_n+b_n)$ 是二分法返回的解。

**证明**：由二分法可知 $a_0 \leq a_1 \leq \dots \leq b_0$，$b_0 \geq b_1 \geq \dots \geq a_0$，$b_{n+1} - a_{n+1} = \frac{1}{2}(b_n - a_n)$。由定理 4.16，$\{a_n\}$ 和 $\{b_n\}$ 均收敛。且 $\lim(b_n - a_n) = \lim \frac{1}{2^n}(b_0 - a_0) = 0$，故 $\lim b_n = \lim a_n = \alpha$。由条件及算法，不变量 $f(a_n)f(b_n) \leq 0$ 始终成立。因 $f$ 连续，$\lim f(a_n)f(b_n) = f(\lim a_n)f(\lim b_n)$，故 $f^2(\alpha) \leq 0$，即 $f(\alpha) = 0$。(4.7) 是另一个重要不变量，可由归纳法证明。比较 (4.7) 与 (4.3) 可得二分法收敛，且为线性收敛，渐近因子 $c = \frac{1}{2}$。$\square$
## 4.4 Newton's method

**※【算法】** 牛顿法(Newton's method)：
输入：$f : \mathbb{R} \to \mathbb{R}$，$f'$，$x_0 \in \mathbb{R}$，$M \in \mathbb{N}^+$，$\epsilon \in \mathbb{R}^+$。
前置条件：$f \in C^2$ 且 $x_0$ 充分接近 $f$ 的根。
输出：$x, k$。
后置条件：$|f(x)| < \epsilon$ 或 $k = M$。
步骤：
1. $x \leftarrow x_0$
2. for $k = 0 : M$ do
3. $\quad u \leftarrow f(x)$
4. $\quad$ if $|u| < \epsilon$ then break
5. $\quad x \leftarrow x - u / f'(x)$
6. end

**※【算法】** 牛顿法在初始猜测 $x_0$ 附近通过迭代公式求 $f : \mathbb{R} \to \mathbb{R}$ 的根：
$x_{n+1} = x_n - \frac{f(x_n)}{f'(x_n)}, \quad n \in \mathbb{N}$ (4.8)

**※【定理】** 牛顿法的收敛性(Convergence of Newton's method)：
对于 $C^2$ 函数 $f : B \to \mathbb{R}$，$B := [\alpha - \delta, \alpha + \delta]$，满足 $f(\alpha) = 0$ 且 $f'(\alpha) \neq 0$，有
$\exists \delta_1 \in (0, \delta) \text{ s.t. } \forall x \in B_1 := [\alpha - \delta_1, \alpha + \delta_1], f'(x) \neq 0$。
定义 $M := \frac{\max_{x \in B_1} |f''(x)|}{2 \min_{x \in B_1} |f'(x)|}$，选取 $x_0$ 充分接近 $\alpha$ 使得 (i) $|x_0 - \alpha| = \delta_0 < \delta_1$ 且 (ii) $M\delta_0 < 1$ 成立。则牛顿法中的迭代序列 $\{x_n\}$ 至少二次收敛到根 $\alpha$，即
$\lim_{n \to \infty} \frac{\alpha - x_{n+1}}{(\alpha - x_n)^2} = -\frac{f''(\alpha)}{2f'(\alpha)}$ (4.9)

**证明**：
由泰勒定理和 $f \in C^2$ 可得 $f(\alpha) = f(x_n) + (\alpha - x_n)f'(x_n) + \frac{(\alpha - x_n)^2}{2}f''(\xi)$，其中 $\xi$ 在 $\alpha,x_n$。
由 $f(\alpha)=0$ 得 $-\alpha = -x_n + \frac{f(x_n)}{f'(x_n)} + \frac{(\alpha - x_n)^2}{2}\frac{f''(\xi)}{f'(x_n)}$。
由 (4.8) 得 $x_{n+1} - \alpha = (x_n - \alpha)^2 \frac{f''(\xi)}{2f'(x_n)}$。
由 $M$ 的定义得 $|x_{n+1} - \alpha| \leq M|x_n - \alpha|^2$。
比较 (4.4) 可知若 $\{x_n\}$ 收敛，则收敛阶为 2。还需证明 (a) 收敛且 (b) 收敛到 $\alpha$。
由 (i) 和 (ii) 得 $M|x_0 - \alpha| < 1$，归纳可得 $|x_n - \alpha| \leq \frac{1}{M}(M|x_0 - \alpha|)^{2^n}$，这证明了 (a) 和 (b)。最后，若 $f''(\alpha)=0$，收敛速度可能高于 2。$\square$

**※【定理】** 连续函数 $f : [a,b] \to [c,d]$ 是双射当且仅当它严格单调。

**※【定理】** 若 $C^2$ 函数 $f : \mathbb{R} \to \mathbb{R}$ 满足 $f(\alpha)=0$，$f'>0$ 且 $f''>0$，则 $\alpha$ 是 $f$ 的唯一根，且 $\forall x_0 \in \mathbb{R}$，牛顿法中的迭代序列 $\{x_n\}$ 二次收敛到 $\alpha$。

**证明**：由定理 4.20，$f$ 连续且严格单调，故为双射。因 0 在其值域内，$f$ 必有唯一根。在证明定理 4.19 时，有 $x_{n+1} - \alpha = (x_n - \alpha)^2 \frac{f''(\xi)}{2f'(x_n)}$。由 $f'>0$ 和 $f''>0$ 进一步可知对所有 $n>0$ 有 $x_n > \alpha$。$f$ 严格递增意味着对所有 $n>0$ 有 $f(x_n) > f(\alpha) = 0$。由牛顿法定义，$x_{n+1} - \alpha = x_n - \alpha - \frac{f(x_n)}{f'(x_n)}$，故序列 $\{x_n - \alpha : n>0\}$ 严格单调递减且有下界 0。由定理 4.16 收敛。设 $\lim_{n \to \infty} x_n = a$，对 (4.8) 两边取极限得 $a = a - \frac{f(a)}{f'(a)}$，即 $f(a)=0$。由根的唯一性得 $a = \alpha$。二次收敛率可由 (4.10) 归纳证明，如同定理 4.19。$\square$

**※【定义】** 凸集(Convex set)：
向量空间 $\mathcal{V}$ 的子集 $\mathcal{U}$ 是凸集，若
$\forall x,y \in \mathcal{U}, \forall t \in (0,1), \quad tx + (1-t)y \in \mathcal{U}$ (4.11)
子集 $W \subset \mathcal{V}$ 的凸包是包含 $W$ 的最小凸集。

**※【定义】** 凸函数(Convex function)：
对于实向量空间的子集，函数 $f : \mathcal{U} \to \mathbb{R}$ 是凸的，若
$\forall t \in (0,1), \forall x,y \in \mathcal{U}, \quad x \neq y \implies f(tx + (1-t)y) \leq tf(x) + (1-t)f(y)$ (4.12)
特别地，若将 (4.12) 中的 $\leq$ 替换为 $<$，则 $f$ 是严格凸的。

## 4.5 the secant method

**※【定义】** 割线法(Secant method)：
在初始猜测 $x_0, x_1$ 附近通过迭代求 $f : \mathbb{R} \to \mathbb{R}$ 的根：
$x_{n+1} = x_n - f(x_n)\frac{x_n - x_{n-1}}{f(x_n) - f(x_{n-1})}, \quad n \in \mathbb{N}^+$ (4.13)

**※【算法】** 割线法算法：
输入：$f : \mathbb{R} \to \mathbb{R}$，$x_0 \in \mathbb{R}$，$x_1 \in \mathbb{R}$，$M \in \mathbb{N}^+$，$\delta \in \mathbb{R}^+$，$\epsilon \in \mathbb{R}^+$。
前置条件：$f \in C^2$；$x_0, x_1$ 充分接近 $f$ 的根。
输出：$x_n, x_{n-1}, k$。
后置条件：$|f(x_n)| < \epsilon$ 或 $|x_n - x_{n-1}| < \delta$ 或 $k = M$。
步骤：
1. $x_n \leftarrow x_1$
2. $x_{n-1} \leftarrow x_0$
3. $u \leftarrow f(x_n)$
4. $v \leftarrow f(x_{n-1})$
5. for $k = 2 : M$ do
6. $\quad s \leftarrow \frac{x_n - x_{n-1}}{u - v}$
7. $\quad x_{n-1} \leftarrow x_n$
8. $\quad v \leftarrow u$
9. $\quad x_n \leftarrow x_n - u \times s$
10. $\quad$ if $|x_n - x_{n-1}| < \delta$ then break
11. $\quad u \leftarrow f(x_n)$
12. $\quad$ if $|u| < \epsilon$ then break
13. end

**※【定义】** 斐波那契数列(Fibonacci numbers)：
序列 $\{F_n\}$ 定义为
$F_0 = 0, F_1 = 1, \quad F_{n+1} = F_n + F_{n-1}$ (4.14)

**※【定理】** 比内公式(Binet's formula)：
记黄金比例为 $r_0 = \frac{1+\sqrt{5}}{2}$，$r_1 = 1 - r_0 = \frac{1-\sqrt{5}}{2}$，则
$F_n = \frac{r_0^n - r_1^n}{\sqrt{5}}$ (4.15)

**证明**：
由定义 4.25，有 $\mathbf{u}_k := \begin{bmatrix} F_{k+1} \\ F_k \end{bmatrix} = A \begin{bmatrix} F_k \\ F_{k-1} \end{bmatrix}$，其中 $A := \begin{bmatrix} 1 & 1 \\ 1 & 0 \end{bmatrix}$。故 $\mathbf{u}_n = A^n \mathbf{u}_0$。
由 $\det(A - \lambda I) = \lambda^2 - \lambda - 1 = 0$ 得 $A$ 的两个特征值为 $\lambda = r_0, r_1$，对应特征向量为 $\mathbf{x}_0 = (r_0, 1)^\top$ 和 $\mathbf{x}_1 = (r_1, 1)^\top$。
最后，将 $\mathbf{u}_0$ 表示为 $\mathbf{x}_0$ 和 $\mathbf{x}_1$ 的线性组合：$\mathbf{u}_0 = \frac{1}{r_0 - r_1}(\mathbf{x}_0 - \mathbf{x}_1)$，结合 $\mathbf{u}_n = A^n \mathbf{u}_0$ 得 $\mathbf{u}_n = \frac{1}{r_0 - r_1}(r_0^n \mathbf{x}_0 - r_1^n \mathbf{x}_1)$，其第二个分量即为 (4.15)。$\square$

**※【推论】** 定理 4.26 中的比率 $r_0, r_1$ 满足
$F_{n+1} = r_0 F_n + r_1^n$ (4.16)

**证明**：由 (4.15) 及 $r_0, r_1$ 的值可得。$\square$

**※【定义】** 差商(divided difference)
一阶差商：$f[x_{n},x_{n-1}]= \frac{f(x_{n})-f(x_{n-1})}{x_{n}-x_{n-1}}$
二阶差商：$f[a,b,c]=\frac{f[b,c]-f[a,b]}{c-a}$

**※【引理】** 
$a\neq b,a\neq c,b\neq c,f\in \mathbb{C}^{2}$, then $f[a,b,c]=\frac{1}{2}f''(\xi)$, $\xi$ between $a,b,c$.
证明：
令 $p(x)=f(a)+f[a,b](x-a)+f[a,b,c](x-a)(x-b)$. 拉格朗日插值多项式 (quadratic interpolation polynomid)
有 $p(a)=f(a),p(b)=f(b),p(c)=f(c)$.
则存在 $\xi_{1},\xi_{2}$ $p'(\xi_{1})=f'(\xi_{1}),p'(\xi_{2})=f'(\xi_{2})$.
则存在 $\xi$ $p''(\xi)=f''(\xi)$.

**※【引理】** 割线法的误差关系(Error relation of the secant method)：
对于割线法 (4.13)，存在 $\xi_n$ 在 $x_{n-1}$ 和 $x_n$ 之间，以及 $\zeta_n$ 在 $\min(x_{n-1}, x_n, \alpha)$ 和 $\max(x_{n-1}, x_n, \alpha)$ 之间，使得
$x_{n+1} - \alpha = (x_n - \alpha)(x_{n-1} - \alpha)\frac{f''(\zeta_n)}{2f'(\xi_n)}$ (4.17)

**证明**：
差商 $f[a,b] = \frac{f(a)-f(b)}{a-b}$ (4.18)。
经过代数运算可得 (4.13) 等价于 $x_{n+1} - \alpha = (x_n - \alpha)(x_{n-1} - \alpha)\frac{\frac{f[x_{n-1},x_n] - f[x_n,\alpha]}{x_{n-1}-\alpha}}{f[x_{n-1},x_n]}$ (4.19)。
由 (4.18) 和中值定理，存在 $\xi_n$ 在 $x_{n-1}$ 和 $x_n$ 之间使得 $f[x_{n-1},x_n] = f'(\xi_n)$ (4.20)。
定义函数 $g(x) := f[x,x_n]$，对 $g(x)$ 应用中值定理得 $\frac{f[x_{n-1},x_n] - f[x_n,\alpha]}{x_{n-1}-\alpha} = g'(\beta)$ (4.21)， $\beta\in [x_{n-1},\alpha]$ 之间。
由 (4.18) 计算 $g'(\beta)$，利用拉格朗日余项定理得 $\frac{f[x_{n-1},x_n] - f[x_n,\alpha]}{x_{n-1}-\alpha} = \frac{f''(\zeta_n)}{2}$ (4.22)，其中 $\zeta_n$ 在 $\min(x_{n-1},x_n,\alpha)$ 和 $\max(x_{n-1},x_n,\alpha)$ 之间。
将 (4.20) 和 (4.22) 代入 (4.19) 即证。$\square$

**※【定理】** 割线法的收敛性(Convergence of the secant method)：
考虑 $C^2$ 函数 $f : B \to \mathbb{R}$，$B = [\alpha - \delta, \alpha + \delta]$，满足 $f(\alpha)=0$ 且 $f'(\alpha) \neq 0$。若 $x_0, x_1$ 均充分接近 $\alpha$ 且 $f''(\alpha) \neq 0$，则割线法中的迭代序列 $\{x_n\}$ 以阶 $p = \frac{1}{2}(1+\sqrt{5}) \approx 1.618$ 收敛到根 $\alpha$。

**证明**：
由 $f'$ 的连续性和 $f'(\alpha) \neq 0$ 得 $\exists \delta_1 \in (0,\delta) \text{ s.t. } \forall x \in B_1, f'(x) \neq 0$，其中 $B_1 = [\alpha - \delta_1, \alpha + \delta_1]$。
定义 $E_i = |x_i - \alpha|$，$M = \frac{\max_{x \in B_1}|f''(x)|}{2\min_{x \in B_1}|f'(x)|}$.
由引理 4.28 得 $M E_{n+1} \leq M E_n M E_{n-1}$。

选取 $x_0, x_1$ 使得 (i) $E_0 < \delta, E_1 < \delta$；(ii) $\max(M E_1, M E_0) = \eta < 1$。归纳可得 $E_n < \delta$，$M E_n < \eta$。
为证明收敛性，写 $M E_0 \leq \eta, M E_1 \leq \eta, M E_2 \leq M E_1 M E_0 \leq \eta^2, \dots, M E_{n+1} \leq \eta^{q_{n+1}}$，
即 $E_n \leq B_n := \frac{1}{M}\eta^{q_n}$。$\{q_n\}$ 是从 $q_0=1, q_1=1$ 开始的斐波那契数列。
由定理 4.26，当 $n \to \infty$ 时 $q_n \to \frac{1.618^{n+1}}{\sqrt{5}}$，因 $|r_1| \approx 0.618 < 1$，故 $\lim_{n \to \infty} E_n = 0$。

为估计收敛速度，考察上界 $\{B_n\}$ 的下降速率：
$\frac{B_{n+1}}{B_n^{r_0}} = M^{r_0-1}\eta^{q_{n+1}-r_0 q_n} \leq M^{r_0-1}\eta^{-1}$，其中 $q_{n+1} - r_0 q_n = r_1^{n+1} > -1$。
为证明收敛阶，定义 $m_n := \left|\frac{f''(\zeta_n)}{2f'(\xi_n)}\right|$，$m_\alpha := \left|\frac{f''(\alpha)}{2f'(\alpha)}\right|$ (4.23)，其中 $\zeta_n, \xi_n$ 同引理 4.28。
归纳得 $E_n = E_1^{F_n} E_0^{F_{n-1}} m_1^{F_{n-1}} \dots m_{n-1}^{F_1}$，$E_{n+1} = E_1^{F_{n+1}} E_0^{F_n} m_1^{F_n} \dots m_n^{F_1}$，其中 $F_n$ 为定义 4.25 中的斐波那契数。
则 $\frac{E_{n+1}}{E_n^{r_0}} = E_1^{r_1^n} E_0^{r_1^{n-1}} m_1^{r_1^{n-1}} \dots m_n^1$ (4.24)。
由 (4.23) 及已证收敛性得 $\lim_{n \to \infty} m_n = m_\alpha$ (4.25)，即 $\exists N \in \mathbb{N} \text{ s.t. } \forall n > N, m_n \in (\frac{1}{2}m_\alpha, 2m_\alpha)$ (4.26)。

定义 $A, B$ 使得 $\frac{E_{n+1}}{E_n^{r_0}} = AB$。
因 $|r_1|<1$，$\lim_{n \to \infty} A = 1$。由 (4.26) 得 $B \leq 2^{\sum_{i=0}^{n-N-1}(-r_1)^i} \cdot m_\alpha^{\sum_{i=0}^{n-N-1}r_1^i}$，故 $\lim_{n \to \infty} \frac{E_{n+1}}{E_n^{r_0}} \leq 2^{\frac{1}{1+r_1}} m_\alpha^{\frac{1}{r_0}}$。
由定义 4.12 即证。$\square$

**※【推论】** 考虑在根 $\alpha$ 附近求解 $f(x)=0$。设 $m$ 和 $sm$ 分别为计算 $f(x)$ 和 $f'(x)$ 的时间。牛顿法和割线法达到所需绝对精度 $\epsilon$ 的最短时间分别为
$T_N = (1+s)m \lceil \log_2 K \rceil$ (4.27)
$T_S = m \lceil \log_{r_0} K \rceil + m$ (4.28)
其中 $r_0 = \frac{1+\sqrt{5}}{2}$，$c = \left|\frac{f''(\alpha)}{2f'(\alpha)}\right|$，$K = \frac{\log c\epsilon}{\log c|x_0-\alpha|}$ (4.29)，$\lceil \cdot \rceil$ 表示向上取整函数。

**证明**：在证明定理 4.19 时已证 $|x_n - \alpha| \leq \frac{1}{M}(M|x_0-\alpha|)^{2^n}$。记 $E_n = |x_n - \alpha|$，有 $M E_n \leq (M E_0)^{2^n}$。设 $i \in \mathbb{N}^+$ 为满足所需精度 $\epsilon$ 的最小迭代次数，即 $(M E_0)^{2^i} \leq M\epsilon$。当 $\epsilon$ 充分小时，$M \to c$。故 $i = \lceil \log_2 K \rceil$。每次牛顿迭代计算一次函数值和一次导数值，分别耗时 $m$ 和 $sm$，故 (4.27) 成立。对于割线法，假设 $f E_0 \geq M E_1$。由定理 4.29 的证明，有 $M E_n \approx (M E_0)^{\frac{\sqrt{5}}{5}r_0^{n+1}}$，其中忽略了指数中的 $-\frac{\sqrt{5}}{5}r_1^{n+1}$ 项。设 $j$ 为满足所需精度 $\epsilon$ 的最小迭代次数，即 $r_0^j \leq \frac{\sqrt{5}}{r_0}K$，故 $j = \lceil \log_{r_0}K + \log_{r_0}\frac{\sqrt{5}}{r_0} \rceil \leq \lceil \log_{r_0}K \rceil + 1$。由于割线法中前两个值 $x_0, x_1$ 已给定，最少迭代次数为 $\lceil \log_{r_0}K \rceil$（与牛顿法比较！）。此外，每次迭代只需计算函数值 $f(x_n)$，因为 $f(x_{n-1})$ 已在上一次迭代中计算。最后的 $m$ 是由于第一步需要计算 $f(x_0)$。$\square$

## 4.6 不动点迭代

**※【定义】** 函数 $g$ 的不动点(Fixed point)是满足 $g(\alpha) = \alpha$ 的独立参数。

**※【例题】** $f(x) = x^2 - 3x + 4$ 的不动点是 $x=2$。

**※【引理】** 若 $g : [a,b] \to [a,b]$ 连续，则 $g$ 在 $[a,b]$ 中至少有一个不动点。

**证明**：函数 $f(x) = g(x) - x$ 满足 $f(a) \geq 0$ 且 $f(b) \leq 0$。由介值定理即证。$\square$

**※【定义】** 不动点迭代(Fixed-point iteration)：
用形如 $x_{n+1} = g(x_n)$ (4.30)，$n \in \mathbb{N}$ 的公式求 $g$ 的不动点的方法。

**※【例题】** 牛顿法是一种不动点迭代。

**※【定义】** 压缩映射(Contraction)：
函数 $f : [a,b] \to [a,b]$ 是 $[a,b]$ 上的压缩或压缩映射，
若 $\exists \lambda \in [0,1) \text{ s.t. } \forall x,y \in [a,b], |f(x) - f(y)| \leq \lambda |x-y|$ (4.31)

**※【例题】** 任何线性函数 $f(x) = \lambda x + c$，$0 \leq \lambda < 1$ 都是压缩映射。

**※【引理】** 
若 $g(x)$ 是 $[a,b]$ 上的压缩映射且在 $[a,b]$ 中有不动点 $\alpha$，
则对任意 $x_0 \in [a,b]$，由不动点迭代 (4.30) 生成的序列 $\{x_n\}$ 
满足 $\frac{1}{1+\lambda}|x_{n+1}-x_n| \leq |x_n-\alpha| \leq \frac{1}{1-\lambda}|x_{n+1}-x_n|$ (4.32)

**证明**：
由定义 4.38，$x_{n+1} = g(x_n)$ 意味着所有 $x_n$ 都留在 $[a,b]$ 中，且 $|x_{n+1}-\alpha| = |g(x_n)-g(\alpha)| \leq \lambda|x_n-\alpha|$。
故 $|x_{n+1}-x_n| \leq |x_{n+1}-\alpha| + |x_n-\alpha| \leq (1+\lambda)|x_n-\alpha|$，这证明了 (4.32) 的第一个不等式。
对于第二个不等式，有 $|x_n-\alpha| \leq |x_{n+1}-x_n| + |x_{n+1}-\alpha| \leq |x_{n+1}-x_n| + \lambda|x_n-\alpha|$，
即 $|x_n-\alpha| \leq \frac{1}{1-\lambda}|x_{n+1}-x_n|$。$\square$

**※【定理】** 压缩映射的收敛性(Convergence of contractions)：
若 $g(x)$ 是 $[a,b]$ 上的连续压缩映射，则它在 $[a,b]$ 中有唯一不动点 $\alpha$。
此外，对任意 $x_0 \in [a,b]$，不动点迭代 (4.30) 收敛到 $\alpha$，且 $|x_n - \alpha| \leq \frac{\lambda^n}{1-\lambda}|x_1 - x_0|$ (4.33)

**证明**：
由引理 4.33，$g$ 在 $[a,b]$ 中至少有一个不动点。
假设有两个不同不动点 $\alpha$ 和 $\beta$，则 $|\alpha-\beta| = |g(\alpha)-g(\beta)| \leq \lambda|\alpha-\beta|$，即 $|\alpha-\beta| \leq 0$，故两不动点相同。
为证明收敛性，$|x_{n+1}-x_n| = |g(x_n)-g(x_{n-1})| \leq \lambda|x_n-x_{n-1}|$。
归纳得 $|x_{n+1}-x_n| \leq \lambda^n|x_1-x_0|$，故 $|x_n-\alpha| \leq \frac{1}{1-\lambda}|x_{n+1}-x_n| \leq \frac{\lambda^n}{1-\lambda}|x_1-x_0|$，其中第一个不等式由 (4.32) 得到。$\square$

**※【定理】** 
考虑 $g : [a,b] \to [a,b]$。
若 $g \in C^1[a,b]$ 且 $\lambda = \max_{x \in [a,b]}|g'(x)| < 1$，则 $g$ 在 $[a,b]$ 中有唯一不动点 $\alpha$。
此外，对任意 $x_0 \in [a,b]$，不动点迭代 (4.30) 收敛到 $\alpha$，误差界 (4.33) 成立，且 $\lim_{n \to \infty} \frac{x_{n+1}-\alpha}{x_n-\alpha} = g'(\alpha)$ (4.34)

**证明**：
由中值定理，对所有 $x,y \in [a,b]$，$|g(x)-g(y)| \leq \lambda|x-y|$。
定理 4.41 给出除 (4.34) 外的所有结果，
(4.34) 由 $x_{n+1}-\alpha = g(x_n)-g(\alpha) = g'(\xi)(x_n-\alpha)$，$\lim_{n \to \infty} x_n = \alpha$，以及 $\xi$ 在 $x_n$ 和 $\alpha$ 之间得到。$\square$

**※【推论】**
设 $\alpha$ 是 $g : \mathbb{R} \to \mathbb{R}$ 的不动点，$|g'(\alpha)| < 1$，且 $g \in C^1(B)$，$B = [\alpha-\delta, \alpha+\delta]$，$\delta > 0$。
若 $x_0$ 充分接近 $\alpha$，则定理 4.41 的结果成立。

**证明**：
选取 $\lambda$ 使得 $|g'(\alpha)| < \lambda < 1$。
选取 $\delta_0 \leq \delta$ 使得 $\max_{x \in B_0}|g'(x)| \leq \lambda < 1$，其中 $B_0 = [\alpha-\delta_0, \alpha+\delta_0]$。
则 $g(B_0) \subset B_0$，应用定理 4.42 即证。$\square$

**※【推论】** 
考虑 $g : [a,b] \to [a,b]$，有不动点 $g(\alpha) = \alpha \in [a,b]$。
若 $\begin{cases} g \in C^p[a,b], \\ \forall k = 1,2,\dots,p-1, g^{(k)}(\alpha) = 0, \\ g^{(p)}(\alpha) \neq 0 \end{cases}$ (4.35) 且 $x_0$ 充分接近 $\alpha$，
则不动点迭代 (4.30) 以 $p$ 阶精度收敛到 $\alpha$ ($p>1, p \in \mathbb{N}$)。

**证明**：
由推论 4.43，$g'(\alpha)=0$，由 $g'$ 的连续性，存在 $\delta>0$ 使得不动点迭代在 $[\alpha-\delta, \alpha+\delta]$ 内唯一收敛到 $\alpha$。
由 $g$ 在 $\alpha$ 处的泰勒展开，
$E_{\mathrm{abs}}(x_{n+1}) := |x_{n+1}-\alpha| = |g(x_n)-g(\alpha)| = \left|\sum_{i=1}^{p-1}\frac{(x_n-\alpha)^i}{i!}g^{(i)}(\alpha) + \frac{(x_n-\alpha)^p}{p!}g^{(p)}(\xi)\right|$，其中 $\xi \in [a,b]$。
因 $g^{(p)}$ 在 $[a,b]$ 上连续，故有界。因此存在常数 $M$ 使得 $E_{\mathrm{abs}}(x_{n+1}) \leq M E_{\mathrm{abs}}^p(x_n)$。$\square$

**※【例题】** 
以下方法计算 $\sqrt{R}$ 具有三阶收敛性：$x_{n+1} = \frac{x_n(x_n^2+3R)}{3x_n^2+R}$。
首先，$\sqrt{R}$ 是 $F(x) = \frac{x(x^2+3R)}{3x^2+R}$ 的不动点：$F(\sqrt{R}) = \frac{\sqrt{R}(R+3R)}{3R+R} = \sqrt{R}$。
其次，$F(x)$ 的各阶导数为：
$n=1: F'(x) = \frac{3(x^2-R)^2}{(3x^2+R)^2}, F'(\sqrt{R}) = 0$.
$n=2: F''(x) = \frac{48Rx(x^2-R)}{(3x^2+R)^3}, F''(\sqrt{R}) = 0$.
$n=3: F'''(x) = \frac{-48R(9x^4-18Rx^2+R^2)}{(3x^2+R)^4}, F'''(\sqrt{R}) = \frac{3}{2R} \neq 0$.
其余由推论 4.44 可得。

## 4.7 非线性方程组

**※【定义】** 非线性方程组(System of nonlinear equations)：
形如 $\mathbf{f}(\mathbf{x}) = \mathbf{0}$ (4.36) 的方程组，其中 $\mathbf{f} : \mathbb{R}^n \to \mathbb{R}^n$ 是多变量向量值函数：$\mathbf{f}(\mathbf{x}) = \begin{bmatrix} f_1(\mathbf{x}) \\ f_2(\mathbf{x}) \\ \vdots \\ f_n(\mathbf{x}) \end{bmatrix}$ (4.37)
每个 $f_i : \mathbb{R}^n \to \mathbb{R}$ 是标量函数，至少有一个 $f_i$ 是非线性的，$\mathbf{x} = [x_1,\dots,x_n]^\top$。
数值目标是找到 $\boldsymbol{\alpha} \in \mathbb{R}^n$，称为根或解，使得在给定范数下近似满足 $\mathbf{f}(\boldsymbol{\alpha}) = \mathbf{0}$。

**※【定义】** 线性收敛，收敛阶

迭代序列 $\{\mathbf{x}^{(k)}\} \subset \mathbb{R}^n$ 线性收敛到 $\mathbf{L} \in \mathbb{R}^n$，
若 $\exists c \in (0,1), \exists d > 0, \exists N \in \mathbb{N} \text{ s.t. } \forall k > N, \|\mathbf{x}^{(k)} - \mathbf{L}\|_\infty \leq c^k d$ (4.38)

对于收敛到 $\mathbf{L}$ 的序列 $\{\mathbf{x}^{(k)}\}$，其收敛阶是满足下式的最大 $p \in \mathbb{R}^+$：
$\exists c > 0, \exists N \in \mathbb{N} \text{ s.t. } \forall k > N, \|\mathbf{x}^{(k+1)} - \mathbf{L}\|_\infty \leq c \|\mathbf{x}^{(k)} - \mathbf{L}\|_\infty^p$ (4.39) 特别地，若 $p=2$，则二次收敛。

### 4.7.1 不动点迭代

**※【定义】** 不动点(Fixed point)
函数 $\mathbf{g} : \mathbb{R}^n \to \mathbb{R}^n$ 的不动点(Fixed point)是满足 $\mathbf{g}(\boldsymbol{\alpha}) = \boldsymbol{\alpha}$ 的独立参数。

**※【例题】** 在定义 3.76 中，线性系统 $A\mathbf{x} = \mathbf{b}$ 的解是函数 $g(\mathbf{x}) = T\mathbf{x} + \mathbf{c}$ 的不动点。

**※【定义】** 不动点迭代(Fixed-point iteration)：
用形如 $\mathbf{x}^{(k+1)} = \mathbf{g}(\mathbf{x}^{(k)})$，$k \in \mathbb{N}$ (4.40) 的公式求 $\mathbf{g}$ 的不动点的方法。

**※【定理】** 布劳威尔不动点定理(Brouwer's fixed point theorem)：
记 $\mathbb{D}^n := \{\mathbf{x} \in \mathbb{R}^n : \|\mathbf{x}\|_2 \leq 1\}$，任何连续函数 $\mathbf{f} : \mathbb{D}^n \to \mathbb{D}^n$ 都有不动点。

**※【例题】** 取一张你国家的地图 $C$ 放在房间地面上。设 $f$ 是将国家中每一点映射到地图上对应点的函数。则 $f$ 可视为连续函数 $C \to C$。若 $C$ 同胚于 $\mathbb{D}^2$，则地图上必存在一点恰好对应其正下方的地面点。

**※【定理】** 
设 $E$ 是 $\mathbb{R}^n$ 中的闭、有界凸集。
假设 $\mathbf{g} : E \to E$ 是 $C^1$ 函数，且 $\lambda = \max_{\mathbf{x} \in E} \|D\mathbf{g}(\mathbf{x})\|_\infty < 1$ (4.41)
其中 $D\mathbf{g}$ 表示 $\mathbf{g}$ 的雅可比矩阵。
则 $\mathbf{g}$ 在 $E$ 中有唯一不动点 $\boldsymbol{\alpha}$，对任意 $\mathbf{x}^{(0)} \in E$，不动点迭代 (4.40) 收敛到 $\boldsymbol{\alpha}$，
且$\lim_{k \to \infty} \frac{\|\mathbf{x}^{(k+1)} - \boldsymbol{\alpha}\|_\infty}{\|\mathbf{x}^{(k)} - \boldsymbol{\alpha}\|_\infty} \leq \|D\mathbf{g}(\boldsymbol{\alpha})\|_\infty$ (4.42)
此外，第 $k$ 次迭代的误差范数满足
$\|\mathbf{e}^{(k)}\|_\infty \leq \frac{\lambda^k}{1-\lambda}\|\mathbf{x}^{(1)} - \mathbf{x}^{(0)}\|_\infty$ (4.43)
$\|\mathbf{e}^{(k)}\|_\infty \leq \frac{1}{1-\lambda}\|\mathbf{x}^{(k+1)} - \mathbf{x}^{(k)}\|_\infty$ (4.44)
其中 $\mathbf{e}^{(k)} := \boldsymbol{\alpha} - \mathbf{x}^{(k)}$。

**证明**：
对每个 $k \in \mathbb{N}^+$，定义 $\boldsymbol{\varphi}^{(k)} : [0,1] \to E$，$\boldsymbol{\varphi}^{(k)}(t) = (1-t)\mathbf{x}^{(k-1)} + t\mathbf{x}^{(k)}$。
因 $E$ 凸，定义良好。
则 $\mathbf{x}^{(k+1)} - \mathbf{x}^{(k)} = \mathbf{g}(\mathbf{x}^{(k)}) - \mathbf{g}(\mathbf{x}^{(k-1)}) = \mathbf{g}(\boldsymbol{\varphi}^{(k)}(1)) - \mathbf{g}(\boldsymbol{\varphi}^{(k)}(0)) = D(\mathbf{g} \circ \boldsymbol{\varphi}^{(k)})(t_k) = D\mathbf{g}(\boldsymbol{\varphi}^{(k)}(t_k))(\mathbf{x}^{(k)} - \mathbf{x}^{(k-1)})$，其中 $t_k \in (0,1)$，第三步由向量值中值定理，最后一步由链式法则。
故 $\boldsymbol{\varphi}^{(k)}(t_k) \in E$，由 (4.41) 得 $\|\mathbf{x}^{(k+1)} - \mathbf{x}^{(k)}\|_\infty \leq \lambda \|\mathbf{x}^{(k)} - \mathbf{x}^{(k-1)}\|_\infty$。归纳得 $\|\mathbf{x}^{(k+1)} - \mathbf{x}^{(k)}\|_\infty \leq \lambda^k \|\mathbf{x}^{(1)} - \mathbf{x}^{(0)}\|_\infty$。这意味着对任意 $m \in \mathbb{N}^+$，$\|\mathbf{x}^{(k)} - \mathbf{x}^{(k+m)}\|_\infty \leq \sum_{i=1}^m \|\mathbf{x}^{(k+i-1)} - \mathbf{x}^{(k+i)}\|_\infty \leq \sum_{i=1}^m \lambda^{k+i-1} \|\mathbf{x}^{(1)} - \mathbf{x}^{(0)}\|_\infty < \frac{\lambda^k}{1-\lambda}\|\mathbf{x}^{(1)} - \mathbf{x}^{(0)}\|_\infty$。故序列 $\{\mathbf{x}^{(k)}\}$ 是柯西序列，收敛到某个 $\boldsymbol{\alpha} \in E$。对 (4.40) 取极限，由 $\mathbf{g}$ 的连续性得 $\mathbf{g}(\boldsymbol{\alpha}) = \boldsymbol{\alpha}$。至于唯一性，假设有两个不同不动点 $\boldsymbol{\alpha}$ 和 $\boldsymbol{\beta}$，则存在 $\boldsymbol{\xi} \in E$ 在连接 $\boldsymbol{\alpha}$ 和 $\boldsymbol{\beta}$ 的线段上使得 $\boldsymbol{\alpha} - \boldsymbol{\beta} = \mathbf{g}(\boldsymbol{\alpha}) - \mathbf{g}(\boldsymbol{\beta}) = D\mathbf{g}(\boldsymbol{\xi})(\boldsymbol{\alpha} - \boldsymbol{\beta})$。故 $\|\boldsymbol{\alpha} - \boldsymbol{\beta}\|_\infty \leq \lambda \|\boldsymbol{\alpha} - \boldsymbol{\beta}\|_\infty$，即 $\|\boldsymbol{\alpha} - \boldsymbol{\beta}\|_\infty \leq 0$，两不动点相同。为证明收敛性，对每个 $k$，存在 $\boldsymbol{\xi}^{(k)} \in E$ 在连接 $\mathbf{x}^{(k)}$ 和 $\boldsymbol{\alpha}$ 的线段上使得 $\mathbf{x}^{(k+1)} - \boldsymbol{\alpha} = \mathbf{g}(\mathbf{x}^{(k)}) - \mathbf{g}(\boldsymbol{\alpha}) = D\mathbf{g}(\boldsymbol{\xi}^{(k)})(\mathbf{x}^{(k)} - \boldsymbol{\alpha})$。由此得 $\|\mathbf{x}^{(k+1)} - \boldsymbol{\alpha}\|_\infty \leq \|D\mathbf{g}(\boldsymbol{\xi}^{(k)})\|_\infty \|\mathbf{x}^{(k)} - \boldsymbol{\alpha}\|_\infty$，由 (4.41) 得 $\|\mathbf{x}^{(k+1)} - \boldsymbol{\alpha}\|_\infty \leq \lambda \|\mathbf{x}^{(k)} - \boldsymbol{\alpha}\|_\infty$。归纳得 $\|\mathbf{x}^{(k)} - \boldsymbol{\alpha}\|_\infty \leq \lambda^k \|\mathbf{x}^{(0)} - \boldsymbol{\alpha}\|_\infty$。因 $\lambda < 1$，这证明 $\mathbf{x}^{(k)} \to \boldsymbol{\alpha}$。然后 (4.42) 由上述不等式、$\|\cdot\|_\infty$ 的连续性、$\lim_{k \to \infty} \mathbf{x}^{(k)} = \boldsymbol{\alpha}$ 以及 $\boldsymbol{\xi}^{(k)}$ 在连接 $\mathbf{x}^{(k)}$ 和 $\boldsymbol{\alpha}$ 的线段上得到。最后，三角不等式和上述不等式给出 $\|\mathbf{x}^{(k)} - \boldsymbol{\alpha}\|_\infty \leq \|\mathbf{x}^{(k)} - \mathbf{x}^{(k+1)}\|_\infty + \|\mathbf{x}^{(k+1)} - \boldsymbol{\alpha}\|_\infty \leq \|\mathbf{x}^{(k)} - \mathbf{x}^{(k+1)}\|_\infty + \lambda \|\mathbf{x}^{(k)} - \boldsymbol{\alpha}\|_\infty$。故 (4.44) 成立，结合上述不等式即得 (4.43)。$\square$

**※【推论】** 设 $\boldsymbol{\alpha}$ 是 $\mathbf{g} : \mathbb{R}^n \to \mathbb{R}^n$ 的不动点，$\|D\mathbf{g}(\boldsymbol{\alpha})\|_\infty < 1$，且
$\mathcal{B} = B(\boldsymbol{\alpha}, \delta) := \{\mathbf{x} \in \mathbb{R}^n : \|\mathbf{x} - \boldsymbol{\alpha}\|_\infty \leq \delta\}$ (4.45)
$\delta > 0$。若 $\mathbf{g} \in C^1(\mathcal{B})$ 且 $\mathbf{x}^{(0)}$ 充分接近 $\boldsymbol{\alpha}$，则定理 4.54 的结果成立。

**证明**：选取 $\lambda$ 使得 $\|D\mathbf{g}(\boldsymbol{\alpha})\|_\infty < \lambda < 1$。选取 $\delta_0 \leq \delta$ 使得 $\max_{\mathbf{x} \in \mathcal{B}_0} \|D\mathbf{g}(\mathbf{x})\|_\infty \leq \lambda < 1$，其中 $\mathcal{B}_0 = B(\boldsymbol{\alpha}, \delta_0)$。则 $\mathbf{g}(\mathcal{B}_0) \subset \mathcal{B}_0$，应用定理 4.54 即证。$\square$

**※【例题】** 考虑 $\mathbf{f} : \mathbb{R}^2 \to \mathbb{R}^2$，$f_1([x_1,x_2]^\top) = 3x_1^2 + 4x_2^2 - 1$，$f_2([x_1,x_2]^\top) = x_2^3 - 8x_1^3 - 1$。用迭代法求解 $\mathbf{f}(\mathbf{x}) = \mathbf{0}$：
$\mathbf{x}^{(k+1)} = \mathbf{g}(\mathbf{x}^{(k)}) = \mathbf{x}^{(k)} - A\mathbf{f}(\mathbf{x}^{(k)})$ (4.46)
其中 $A = \begin{bmatrix} 0.016 & -0.17 \\ 0.52 & -0.26 \end{bmatrix}$ (4.47)。给定初始值 $\mathbf{x}^{(0)} = [-0.5, 0.25]^\top$，数值结果如下：
$k=0: x_1=-0.5, x_2=0.25$
$k=1: x_1=-0.497343750, x_2=0.254062500$
$k=2: x_1=-0.497254794, x_2=0.254077922$
$k=3: x_1=-0.497251343, x_2=0.254078566$
$k=4: x_1=-0.497251208, x_2=0.254078592$
解为 $\boldsymbol{\alpha} \approx [-0.49725, 0.25408]^\top$。计算得 $D\mathbf{g}(\boldsymbol{\alpha}) \approx \begin{bmatrix} 0.0389 & 0.0004 \\ 0.0085 & -0.0066 \end{bmatrix}$，故 $\|D\mathbf{g}(\boldsymbol{\alpha})\|_\infty \approx 0.0393 < 1$。由推论 4.55 和 (4.42)，对所有充分大的 $k$，近似有 $\frac{\|\mathbf{x}^{(k+1)} - \boldsymbol{\alpha}\|_\infty}{\|\mathbf{x}^{(k)} - \boldsymbol{\alpha}\|_\infty} \leq \|D\mathbf{g}(\boldsymbol{\alpha})\|_\infty \approx 0.0393$。

### 4.7.2 牛顿法

**※【算法】** 牛顿法(Newton's method)：
在初始猜测 $\mathbf{x}^{(0)}$ 附近通过迭代公式求 $\mathbf{f} : \mathbb{R}^n \to \mathbb{R}^n$ 的根：$\mathbf{x}^{(k+1)} = \mathbf{x}^{(k)} - [D\mathbf{f}(\mathbf{x}^{(k)})]^{-1}\mathbf{f}(\mathbf{x}^{(k)}), \quad k \in \mathbb{N}$ (4.48)
其中 $D\mathbf{f}$ 是 $\mathbf{f}$ 的雅可比矩阵。

**※【定理】** 牛顿法的收敛性(Convergence of Newton's method)：
对于 $C^2$ 函数 $\mathbf{f} : B \to \mathbb{R}^n$，其中 $B$ 如 (4.45) 定义.
满足 $\mathbf{f}(\boldsymbol{\alpha}) = \mathbf{0}$ 且 $D\mathbf{f}(\boldsymbol{\alpha})$ 非奇异，存在 $\delta_1 \in (0,\delta)$ 使得对所有 $\mathbf{x} \in B_1 = B(\boldsymbol{\alpha}, \delta_1)$，雅可比矩阵 $D\mathbf{f}(\mathbf{x})$ 非奇异。
定义 $M_i$ 为 $f_i$ 在 $B_1$ 上所有二阶偏导数的最大绝对值，即 $M_i := \max_{1 \leq j,\ell \leq n} \max_{\mathbf{x} \in B_1} \left|\frac{\partial^2 f_i}{\partial x_j \partial x_\ell}(\mathbf{x})\right|$。
假设 $\mathbf{x}^{(0)}$ 充分接近 $\boldsymbol{\alpha}$ 使得 (i) $\|\mathbf{x}^{(0)} - \boldsymbol{\alpha}\|_\infty = \delta_0 < \delta_1$ 且 (ii) $M\delta_0 < 1$，
其中 $M := \frac{1}{2}n^2 \max_{\mathbf{x} \in B_1}\|[D\mathbf{f}(\mathbf{x})]^{-1}\|_\infty \cdot \max_{1 \leq i \leq n}M_i$。
则 (4.48) 中的迭代序列 $\{\mathbf{x}^{(k)}\}$ 至少二次收敛到根 $\boldsymbol{\alpha}$，即$\|\boldsymbol{\alpha} - \mathbf{x}^{(k+1)}\|_\infty \leq M \|\boldsymbol{\alpha} - \mathbf{x}^{(k)}\|_\infty^2$ (4.49)

**证明**：
由泰勒定理和 $f_i \in C^2$ 得 $f_i(\boldsymbol{\alpha}) = f_i(\mathbf{x}^{(k)}) + \sum_{j=1}^n (\alpha_j - x_j^{(k)})\frac{\partial f_i(\mathbf{x}^{(k)})}{\partial x_j} + \frac{1}{2}c_i^{(k)}$，
其中 $c_i^{(k)}$ 是 $\mathbf{c}^{(k)}$ 的第 $i$ 个元素，$c_i^{(k)} = \left[\sum_{j=1}^n (\alpha_j - x_j^{(k)})\frac{\partial}{\partial x_j}\right]^2 f_i(\boldsymbol{\xi}^{(k,i)})$，$\boldsymbol{\xi}^{(k,i)}$ 在连接 $\mathbf{x}^{(k)}$ 和 $\boldsymbol{\alpha}$ 的线段上。
则 $\mathbf{0} = \mathbf{f}(\boldsymbol{\alpha}) = \mathbf{f}(\mathbf{x}^{(k)}) + D\mathbf{f}(\mathbf{x}^{(k)})(\boldsymbol{\alpha} - \mathbf{x}^{(k)}) + \frac{1}{2}\mathbf{c}^{(k)}$，结合 (4.48) 得 $\mathbf{x}^{(k+1)} - \boldsymbol{\alpha} = \frac{1}{2}[D\mathbf{f}(\mathbf{x}^{(k)})]^{-1}\mathbf{c}^{(k)}$。
由 $M_i$ 的定义得 $|c_i^{(k)}| \leq n^2 M_i \|\boldsymbol{\alpha} - \mathbf{x}^{(k)}\|_\infty^2$，故 $\|\mathbf{c}^{(k)}\|_\infty \leq n^2 \max_i M_i \|\boldsymbol{\alpha} - \mathbf{x}^{(k)}\|_\infty^2$。
因此 $\|\mathbf{x}^{(k+1)} - \boldsymbol{\alpha}\|_\infty \leq M \|\boldsymbol{\alpha} - \mathbf{x}^{(k)}\|_\infty^2$。
比较 (4.39) 可知若 $\{\mathbf{x}^{(k)}\}$ 收敛，则收敛阶为 2。还需证明 (a) 收敛且 (b) 收敛到 $\boldsymbol{\alpha}$。
由 (i) 和 (ii) 得 $M\|\mathbf{x}^{(0)} - \boldsymbol{\alpha}\|_\infty < 1$，归纳得 $\|\mathbf{x}^{(k)} - \boldsymbol{\alpha}\|_\infty \leq \frac{1}{M}(M\|\mathbf{x}^{(0)} - \boldsymbol{\alpha}\|_\infty)^{2^k}$，这证明了 (a) 和 (b)。最后，若 $D^2\mathbf{f}(\boldsymbol{\alpha}) = \mathbf{0}$，收敛速度可能高于 2。$\square$

### 4.7.3 拟牛顿法

**※【算法】** 对于无约束优化问题 $\min_{\mathbf{x} \in \mathbb{R}^n} f(\mathbf{x})$，可利用一阶必要条件 $\nabla f(\mathbf{x}) = \mathbf{0}$ 应用牛顿法。迭代为
$\mathbf{x}^{(k+1)} = \mathbf{x}^{(k)} - H_k^{-1}\mathbf{g}^{(k)}$ (4.50)
其中 $\mathbf{g}^{(k)} = \mathbf{g}(\mathbf{x}^{(k)}) = \nabla f(\mathbf{x}^{(k)})$ 是 $f$ 在 $\mathbf{x}^{(k)}$ 处的梯度向量，$H_k = H(\mathbf{x}^{(k)})$ 是 $f$ 在 $\mathbf{x}^{(k)}$ 处的海森矩阵，定义为 $H = (h_{ij})_{n \times n}$，$h_{ij}(\mathbf{x}) = \frac{\partial^2 f}{\partial x_i \partial x_j}(\mathbf{x})$。

**※【算法】** 拟牛顿法(Quasi-Newton methods)旨在通过使用对称正定矩阵近似海森矩阵 $H_k$ 或其逆矩阵来避免计算二阶导数，从而在此近似下优化目标函数 $f$。不同的近似方案产生不同的拟牛顿法变体。

**※【定义】** 拟牛顿条件(Quasi-Newton condition)：对于牛顿迭代 (4.50)，拟牛顿条件由以下两个等价关系之一组成：
$\mathbf{y}^{(k)} = H_k \boldsymbol{\delta}^{(k)}$ (4.51)
$H_k^{-1}\mathbf{y}^{(k)} = \boldsymbol{\delta}^{(k)}$ (4.52)
其中 $\mathbf{y}^{(k)} = \mathbf{g}^{(k+1)} - \mathbf{g}^{(k)}$，$\boldsymbol{\delta}^{(k)} = \mathbf{x}^{(k+1)} - \mathbf{x}^{(k)}$。
在拟牛顿法中，构造对称正定矩阵 $G_k$（近似 $H_k^{-1}$）或 $B_k$（近似 $H_k$），设计满足 (4.52) 或 (4.51)。

**※【算法】** DFP (Davidon–Fletcher–Powell) 算法是一种拟牛顿法，构造并更新矩阵 $G_k$ 以近似目标函数 $f : \mathbb{R}^n \to \mathbb{R}$ 的逆海森矩阵 $H_k^{-1}$。其迭代更新公式为
$G_{k+1} = G_k + \frac{\boldsymbol{\delta}^{(k)}(\boldsymbol{\delta}^{(k)})^\top}{(\boldsymbol{\delta}^{(k)})^\top\mathbf{y}^{(k)}} - \frac{G_k\mathbf{y}^{(k)}(\mathbf{y}^{(k)})^\top G_k}{(\mathbf{y}^{(k)})^\top G_k\mathbf{y}^{(k)}}$ (4.53)
其中 $\boldsymbol{\delta}^{(k)}$ 和 $\mathbf{y}^{(k)}$ 如定义 4.62 所定义。若初始矩阵 $G_0$ 选为正定对称矩阵（通常为单位矩阵 $I_n$），则迭代过程中生成的每个矩阵 $G_k$ 都保持正定和对称。

**※【算法】** BFGS (Broyden–Fletcher–Goldfarb–Shanno) 算法是一种拟牛顿法，构造并更新矩阵 $B_k$ 以近似目标函数 $f : \mathbb{R}^n \to \mathbb{R}$ 的海森矩阵 $H_k$。其迭代更新公式为
$B_{k+1} = B_k + \frac{\mathbf{y}^{(k)}(\mathbf{y}^{(k)})^\top}{(\mathbf{y}^{(k)})^\top\boldsymbol{\delta}^{(k)}} - \frac{B_k\boldsymbol{\delta}^{(k)}(\boldsymbol{\delta}^{(k)})^\top B_k}{(\boldsymbol{\delta}^{(k)})^\top B_k\boldsymbol{\delta}^{(k)}}$ (4.54)
其中 $\boldsymbol{\delta}^{(k)}$ 和 $\mathbf{y}^{(k)}$ 如定义 4.62 所定义。若初始矩阵 $B_0$ 选为正定对称矩阵（通常为单位矩阵 $I_n$），则迭代过程中生成的每个矩阵 $B_k$ 都保持正定和对称。

**※【总结】** 本章系统发展了非线性方程及方程组的数值方法，包括二分法、牛顿法、割线法、不动点迭代及其多维推广（不动点迭代、牛顿法和拟牛顿法）。理论核心围绕收敛性分析（线性/二次/超线性阶、全局/局部准则）和误差估计，并辅以定量效率比较（如牛顿法与割线法）。本笔记的特色包括：通过误差递推和斐波那契数列严格证明割线法的收敛阶，确定黄金比例阶；以及将非线性不动点迭代与线性系统求解器进行跨域类比，统一在压缩映射原理下。理论与实践紧密结合：每种方法都附有完整的算法签名，并通过编程作业加强基于契约的设计。