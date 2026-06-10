## 4.1 非负可测函数的 L 积分

设$f$在\[a,b\]上有界，
(1) $[a,b]=\cup_{k=1}^\infty I_{k},I_{k}=[x_{k-1},x_{k}],a=x_{0}<x_{1}<\dots<x_{n}=b$ 
$M_{k}=\sup_{x\in I_{k}}f(x),m_{k}=\inf_{x\in I_{k}}f(x)$ 
(2) $\overline{S_{\triangle}^{(f)}}=\sum_{k=1}^nM_{k}\triangle x_{k},\underline{S_{\triangle}^{(f)}}=\sum_{k=1}^nm_{k}\triangle x_{k}$ 


**※【定义】** 非负简单函数的L积分、L可积
$E\in \mathcal{M},\varphi(x)\in s^+(E)$，即$\varphi(x)=\sum a_{k}\chi_{E_{k}}(x)$，其中$E=\cup_{k=1}^pE_{k},E_{i}\cap E_{j}=\emptyset$ 
即$\varphi(x)$在$E$上$L$积分定义为$\int_{E}\varphi(x)dx=\sum_{k=1}^pa_{k}m(E_{k})$ 
若$\int_{E}\varphi(x)dx<\infty$，称$\varphi(x)$为Lebesgue可积，可记作$L^{1}(E)/L^{1}$
注：约定$0\cdot(+\infty)=0$，故$0\leq \int_{E}\varphi(x)dx\leq+\infty$ 

例 $\int_E \chi_A(x) dx = m(A)$. 若 $m(A) < \infty$，则 $\chi_A(x)$ Lebesgue 可积.

例 $\int_{[0,1]} D(x) dx = \int_{[0,1]} (1 \cdot \chi_{\mathbb{Q}}(x) + 0 \cdot \chi_{\mathbb{R}\setminus\mathbb{Q}}(x)) dx = 1 \cdot m([0,1]\cap \mathbb{Q}) = 0$，故 Dirichlet 函数 Lebesgue 可积.

**※【命题】** 几乎处处相等的非负简单函数的L积分相等
若 $\varphi, \psi \in \mathcal{s}^+(E)$ 且 $\varphi(x) = \psi(x)$ a.e.，则 $\int_E \varphi(x) dx = \int_E \psi(x) dx$. 

**※【命题】** 非负简单函数L积分的性质
设 $\varphi, \psi \in \mathcal{s}^+(E)$，则
(1) $0 \le \int_E \varphi(x) dx \le +\infty$；
(2) $\int_E \lambda \varphi(x) dx = \lambda \int_E \varphi(x) dx$，$\lambda \ge 0$；
(3) $\int_E (\varphi(x)+\psi(x)) dx = \int_E \varphi(x) dx + \int_E \psi(x) dx$；
(4) 若 $E = A \cup B$, $A \cap B = \emptyset$, $A,B \in \mathcal{M}$，则 $\int_E \varphi(x) dx = \int_A \varphi(x) dx + \int_B \varphi(x) dx$；
(5) 若 $\varphi(x) \le \psi(x)$，则 $\int_E \varphi(x) dx \le \int_E \psi(x) dx$.

**※【定义】** 非负可测函数的L积分、L可积
设 $f \in \mathcal{M}^+(E)$，则 $\int_E f(x) dx = \sup\{ \int_E \varphi(x) dx : \varphi(x) \in \mathcal{s}^+(E), 0 \le \varphi(x) \le f(x) \}$，有 $0 \le \int_E f(x) dx \le +\infty$.若 $\int_E f(x) dx < +\infty$，则称 $f(x)$ 在 $E$ 上可积，记作 $L^1(E)$.

例 $\int_{[0,\infty)} e^{-x} dx$.构造 $\varphi_n(x) = \sum_{k=1}^{n} e^{-k/n} \chi_{((k-1)/n, k/n]}(x)$，则 $\varphi_n(x) \uparrow e^{-x}$，$\int_0^\infty \varphi_n(x) dx = \frac{1}{n} \sum_{k=1}^n e^{-k/n} \to 1$，故 $\int_0^\infty e^{-x} dx = 1$.

**※【命题】** 若存在非负简单函数列单增趋近于非负可测函数，则L积分相等
设 $f \in \mathcal{M}^+(E)$ 且存在 $\varphi_n(x) \in \mathcal{s}^+(E)$，$\varphi_n(x) \uparrow f(x)$，则 $\lim_{n \to \infty} \int_E \varphi_n(x) dx = \int_E f(x) dx$.

**※【命题】** （Beppo Levi非负渐升列积分定理）若存在非负可测函数列单增趋近于非负可测函数，则L积分相等
设 $f \in \mathcal{M}^+(E)$ 且存在 $\varphi_n(x) \in \mathcal{M}^+(E)$，$\varphi_n(x) \uparrow f(x)$，则 $\lim_{n \to \infty} \int_E \varphi_n(x) dx = \int_E f(x) dx$.

例 设$\{ f_{k}(x) \}$是$E$上的非负可积函数渐降列，且有$\lim_{ k \to \infty }f_{k}(x)=f(x),a.e.x\in E$，则$\lim_{ k \to \infty }\int_{E} f_{k}(x)dx=\int f(x)dx$.

**※【命题】** 非负可测函数L积分的性质
设 $f,g \in \mathcal{M}^+(E)$，$\lambda \ge 0$，则
(1) $\int_E \lambda f(x) dx = \lambda \int_E f(x) dx$；
(2) $\int_E (f(x)+g(x)) dx = \int_E f(x) dx + \int_E g(x) dx$；
(3) 若 $E = A \cup B$，$A \cap B = \emptyset$，则 $\int_E f = \int_A f + \int_B f$；
(4) 若 $f(x) \le g(x)$ a.e.，则 $\int_E f \le \int_E g$；
(5) 若 $m(E)=0$，则 $\int_E f(x) dx = 0$.

**※【定理】** 
若$f(x)$是$E$上的非负可积函数，则$f(x)$在$E$上是几乎处处有限的.

## 4.3 一般可测函数的 L 积分

**※【定义】** L 积分，L 可积
设 $f(x) = f^+(x) - f^-(x)$，定义 $\int_E f(x) dx = \int_E f^+(x) dx - \int_E f^-(x) dx$，要求右边至少有一个有限.若 $\int_E |f(x)| dx < \infty$，则称 $f$ 是 Lebesgue 可积的.

例 $f(x)\in \mathcal{M}(E)$且有界，$m(E)<\infty$，则$f\in L(E)$. 

**※【定理】** 一般可测函数L可积等价于其绝对值函数L可积
$f \in \mathcal{M}(E)$，则 $f \in L^1(E) \iff |f| \in L^1(E)$，且 $|\int_E f(x) dx| \le \int_E |f(x)| dx$.

例 $E=[0,1]$, $f(x) = (-1)^n$, $x \in [\frac{1}{n+1}, \frac{1}{n})$, $n \in \mathbb{N}$.解: $f^+(x)=1$ on $[1/(2m+1), 1/(2m))$? 实际上计算得 $\int_0^1 f^+(x) dx = \sum_{n \text{ even}} (1/n - 1/(n+1)) = 1 - \ln 2$，$\int_0^1 f^-(x) dx = \ln 2$，故 $\int_E f = 1 - 2\ln 2$.

**※【命题】** 几乎处处相等的一般可测函数的L积分相等
设 $f,g: E \to \mathbb{R}$，若 $f$ 有 L 积分且 $f(x)=g(x)$ a.e.，则 $g$ 也有 L 积分且 $\int_E f = \int_E g$.


**※【命题】** L 积分的性质
(1) $\int_E \lambda f = \lambda \int_E f$；
(2) 若 $f,g$ 的 L 积分不是异号的无穷大，则 $\int_E (f+g) = \int_E f + \int_E g$；
(3) 若 $E = A \cup B$，$A \cap B = \emptyset$，则 $\int_E f = \int_A f + \int_B f$.

L可积的性质
$L^1(E)$ 是线性空间.
(1) 若 $m(E)<\infty$，则 $E$ 上的有界可测函数必可积.
(2) 若 $f$ 可测，$|f(x)| \le g(x) \in L^1(E)$，则 $f \in L^1(E)$ 且 $|\int_E f| \le \int_E g$.

**※【定理】** Chebyshev 不等式
$f \in L^1(E)$，则 $\forall a \ge 0$，$m[E(|f| \ge a)] \le \frac{1}{a} \int_E |f(x)| dx$.

**※【定理】** 若 $f \in L^1(E)$，则 $|f(x)| < +\infty$ a.e..

**※【定理】** $f \in \mathcal{M}(E)$，则 $\int_E |f(x)| dx = 0 \iff f(x)=0$ a.e..

**※【定理】** 绝对连续性
$f \in L^1(E)$，则 $\forall \epsilon > 0$，$\exists \delta > 0$，使得对任意 $E_\delta \subset E$，当 $m(E_\delta) < \delta$ 时，有 $\int_{E_\delta} |f(x)| dx < \epsilon$.

**※【定理】Levi 单调收敛定理**
设 $\{f_k(x)\} \subset \mathcal{M}^+(E)$ 满足 (1) $f_k(x) \ge 0$，(2) $f_k(x) \le f_{k+1}(x)$，(3) $f_k(x) \to f(x)$ a.e.，则 $\lim_{k \to \infty} \int_E f_k(x) dx = \int_E f(x) dx$.

例 $\lim_{k \to \infty} \int_0^k \frac{k}{1+kx} dx$.令 $f_k(x) = \frac{k}{1+kx} \chi_{[0,k]}(x)$，单调非负收敛于 $f(x)=1/x$，但注意积分发散？实际上 $\int_0^k \frac{k}{1+kx} dx = \ln(1+k^2) \to \infty$，而 $\int_0^\infty 1/x dx$ 发散，定理仍成立.

例 $\lim_{t \to 0^+} \int_0^\infty \frac{e^{-tx}}{1+x^2} dx = \int_0^\infty \frac{1}{1+x^2} dx = \frac{\pi}{2}$.

**※【定理】逐项积分定理**
设 $f_k(x) \in \mathcal{M}^+(E)$，则 $\int_E \sum_{k=1}^\infty f_k(x) dx = \sum_{k=1}^\infty \int_E f_k(x) dx$.

**※【引理】Fatou 引理**
设 $f_k(x) \in \mathcal{M}^+(E)$，则 $\int_E \liminf_{k \to \infty} f_k(x) dx \le \liminf_{k \to \infty} \int_E f_k(x) dx$.
设 $f_k(x) \in \mathcal{M}^+(E)$，则 $\int_E \limsup_{k \to \infty} f_k(x) dx \ge \limsup_{k \to \infty} \int_E f_k(x) dx$.

证: 令 $g_k(x) = \inf_{j \ge k} f_j(x)$，则 $g_k \uparrow \liminf f_k$，由 Levi 定理得 $\int \liminf f_k = \lim \int g_k \le \liminf \int f_k$.

例 设$g\in L(E),f_{n}\in L(E)$. 若$f_{n}(x)\geq g(x),a.e.x\in E$，则$\int_{E}\liminf f_{n}(x)dx\leq\liminf\int_{E}f_{n}(x)dx$. 

**※【定理】Lebesgue 控制收敛定理 (LDCT)**
设 $f_k(x) \in \mathcal{M}(E)$，满足 (1) $f_k(x) \to f(x)$ a.e. $x \in E$，(2) 存在 $F(x) \in L^1(E)$，$F(x) \ge 0$，使得 $|f_k(x)| \le F(x)$ ，则 $f_k, f \in L^1(E)$ 且 $\lim_{k \to \infty} \int_E f_k(x) dx = \int_E f(x) dx$.

**※【推论】** 若 $m(E)<\infty$，且 $|f_k(x)| \le M$，$f_k \to f$ a.e.，则 $\lim \int_E f_k = \int_E f$.

**※【定理】** 逐项积分定理
设 $f_k(x) \in \mathcal{M}(E)$ 满足 $\sum_{k=1}^\infty \int_E |f_k(x)| dx < \infty$，则 $f(x)=\sum_{k=1}^\infty f_k(x)$ 在 $E$ 上几乎处处绝对收敛，且 $\int_E \sum_{k=1}^\infty f_k(x) dx = \sum_{k=1}^\infty \int_E f_k(x) dx$. 

**※【定理】** 积分区域极限定理
设 $f \in \mathcal{M}(E)$，$E_1 \subset E_2 \subset \cdots$，$E = \bigcup_{k=1}^\infty E_k$.若 $f \ge 0$ 或 $f \in L^1(E)$，则 $\int_E f(x) dx = \lim_{k \to \infty} \int_{E_k} f(x) dx$.

**※【定理】** $\sigma$-可加性
设 $f \in \mathcal{M}(E)$，$E = \bigcup_{k=1}^\infty E_k$，$E_i \cap E_j = \emptyset$. 若 $f \ge 0$ 或 $f \in L^1(E)$，则 $\int_E f(x) dx = \sum_{k=1}^\infty \int_{E_k} f(x) dx$.

**※【定理】** 积分的绝对连续性
若$f\in L(E)$，则对任给的$\varepsilon>0$，存在$\delta>0$，使得当$E$中子集$e$的测度$m(e)<\delta$时，有$|\int_{e}f(x)dx|\leq \int_{e}|f(x)|dx<\varepsilon$

## 4.4 L积分与R积分

**※【引理】** 
设$f(x)$是定义在$I=[a,b]$上的有界函数，记$\omega(x)$是$f(x)$在$[a,b]$上的振幅函数，则有$\int_{I}\omega(x)dx=\overline{\int}_{a}^bf(x)dx-\underline{\int}_{a}^bf(x)dx$ 其中左端是$\omega(x)$在$I$上的Lebesgue积分

**※【定义】** 
$M_{\triangle}(x)=\sum_{k=1}^N M_{k}\chi_{I_{k}}(x)$ $m_{\triangle}(x)=\sum_{k=1}^N m_{k}\chi_{I_{k}}(x)$ 则$M_{k},m_{k}$是$[a,b]$上的简单函数，满足 (1) $m_{\triangle}(x)\leq f(x)\leq M_{\triangle}(x),x\in I$ (2) $\int_{I}m_{\triangle}(x)=\underline{S}_{\triangle}(f)$ $\int_{I}M_{\triangle}(x)dx=\overline{S}_{\triangle}(f)$ 

**※【命题】** 函数有界，则R可积与几乎处处连续等价
设 $f$ 在 $[a,b]$ 上*有界*，$M(x) = \limsup_{y \to x} f(y)$，$m(x) = \liminf_{y \to x} f(y)$，则 $M(x), m(x)$ 可测，且 $m(x) \le f(x) \le M(x)$，$f$ 在 $[a,b]$ 上 *Riemann 可积* $\iff M(x)=m(x)$ a.e..

**※【定理】** 在闭区间上，R 可积 ⇒ L 可积
设 $f \in R[a,b]$，则 $f \in L^1[a,b]$ 且 $(L)\int_{[a,b]} f = (R)\int_a^b f$ 

**※【命题】** 若 $x_0 \in [a,b)$ 不是分割区间内节点，$f$ 在 $x_0$ 点连续 $\iff M(x_0)=m(x_0)$.

**※【定理】** $f$ 在 $[a,b]$ 上有界，$f \in R[a,b] \iff f$ 在 $[a,b]$ 上几乎处处连续.

**※【命题】** 
若 $(R)\int_0^{+\infty} f(x) dx$ 绝对收敛（即 $\int_0^{+\infty} |f(x)| dx < \infty$），且 $f$ 在任意有限区间上 Riemann 可积，则 $f \in L^1(0,+\infty)$ 且 $(L)\int_0^{+\infty} f = (R)\int_0^{+\infty} f$.
注: 仅条件收敛不能保证 Lebesgue 可积（如 $\int_0^\infty \frac{\sin x}{x} dx$ 条件收敛，但不是 Lebesgue 可积）.

$f(x,y)$定义在$x\in E,y\in[a,b]$上，$\forall\,y\in[a,b],\int_{E}f(x,y)dx<\infty$. 命 $\varphi(y)=\int_{E}f(x,y)dx$
**※【命题】** 
设$\forall\,y\in[a.b],f(\cdot,y)\in L(E)$ 以及对于$y_{0}\in[a,b]$有
(1) $\lim_{ y \to y_{0} }f(x,y)=g(y)\,a.e.x\in E$ 
(2) $|f(x,y)|\leq F(x)\in L(E),\forall\,x\in E,y\in[a,b]$

## 4.5 重积分与累次积分的关系
### 4.5.1 Fubini定理
**※【定理】** Tonelli定理 非负可测函数的情形
设$f(x,y)$是$\mathbb{R}^n=\mathbb{R}^p\times \mathbb{R}^q$上的非负可测函数，则有
1. 对于几乎处处的$x\in \mathbb{R}^p$，$f(x,y)$作为$y$的函数是$\mathbb{R}^p$上的非负可测函数
2. 记$F_{f}(x)=\int_{\mathbb{R}^q}f(x,y)dy$，则$F_{f}(x)$是$\mathbb{R}^p$上的非负可测函数
3. $\int_{\mathbb{R}^p}F_{f}(x)dx=\int dx\int f(x,y)dy=\int f(x,y)dxdy$ 

**※【定理】** Fubini定理 可积函数的情形
若$f\in L(\mathbb{R}^n)$，$(x,y)\in \mathbb{R}^n$，则
1. 对于几乎处处的$x\in \mathbb{R}^p$，$f(x,y)$作为$y$的函数是$\mathbb{R}^q$上的可积函数
2. 记$F_{f}(x)=\int_{\mathbb{R}^q}f(x,y)dy$，则$F_{f}(x)$是$\mathbb{R}^p$上的可积函数
3. $\int f(x,y)dxdy=\int dx\int f(x,y)dy=\int dy\int f(x,y)dx$ 

注：也就是只有非负可测函数和可积函数是可以交换积分顺序的

### 4.5.2 积分的几何意义

**※【定理】** 截段集
设$E$是$\mathbb{R}^n$中的点集，对任意的$x\in \mathbb{R}^p$，令$E(x)=\{ y\in \mathbb{R}^n:(x,y)\in E \}$，称它为点集E在x处的*截段集*. 若E是可测集，则对几乎处处的$x,E(x)$是$\mathbb{R}^q$中的可测集，$m(E(x))$是$\mathbb{R}^p$上的可测函数，且有$m(E)=\int_{\mathbb{R}^p}m(E(x))dx$.

**※【定理】** 可测集的乘积
若$E_{1},E_{2}$是$\mathbb{R}^p,\mathbb{R}^q$中的可测集，则$E_{1}\times E_{2}$是$\mathbb{R}^p\times \mathbb{R}^q$中的可测集且有$m(E_{1}\times E_{2})=m(E_{1})\cdot m(E_{2})$ 

**※【推论】** 可测函数图形的测度

**※【定理】** 积分的几何意义
设$f(x)$是$E\subset \mathbb{R}^n$上的非负实值函数，记$\underline{G}(f)=\{ (x,y)\in \mathbb{R}^{n+1}:x\in E,0\leq y\leq f(x) \}$，称它为$y=f(x)$在E上的下方图形. 我们有下述结论：
1. 若$f(x)$是可测函数，则$\underline{G}(f)$是$\mathbb{R}^{n+1}$中的可测集，且有$m(\underline{G}(f))=\int_{E}f(x)dx$ 
2. 若$E$是可测集，$\underline{G}(f)$是$\mathbb{R}^{n+1}$中的可测集，则$f(x)$是可测函数，且有$m(\underline{G}(f))=\int_{E}f(x)dx$ 

### 4.5.3 卷积函数、分布函数

**※【定义】** 卷积

**※【定理】** 
若$f,g\in L(\mathbb{R}^n)$，则$(f*g)(x)$对几乎处处的$x\in \mathbb{R}^n$存在，$(f*g)(x)$是$\mathbb{R}^n$上的可积函数，且有$\int_{\mathbb{R}^n}|(f*g)(x)|dx\leq\int_{\mathbb{R}^n}|f(x)|dx\int_{\mathbb{R}^n}|g(x)|dx$ 

例 卷积是连续函数
设$f\in L(\mathbb{R}^n),g(x)$在$\mathbb{R}^n$上有界可测，则$F(x)=(f*g)(x)$是$\mathbb{R}$上的一致连续函数


