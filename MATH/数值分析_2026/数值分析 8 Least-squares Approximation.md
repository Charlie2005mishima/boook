
> 本章在内积空间的框架下研究最小二乘逼近问题。首先通过正交多项式、Gram–Schmidt 正交化和正交展开建立最佳逼近理论；随后利用 Gram 矩阵推导正规方程，并将连续最小二乘推广至离散最小二乘，介绍基于正规方程和 QR 分解的求解方法；最后针对无解、多解及病态线性方程组，讨论 Moore–Penrose 广义逆、谱截断与 Tikhonov 正则化。
>
> #LeastSquares #OrthogonalPolynomials #GramSchmidt #QR #SVD #Regularization

**※【定理 8.1】** 连续函数空间上的内积与范数：
设 $\mathcal C[a,b]$ 为区间 $[a,b]$ 上的复值连续函数空间，
权函数(weight function) $\rho\in\mathcal L[a,b]$ 可积，且对任意 $x\in(a,b)$ 有 $\rho(x)>0$。
定义内积：$\langle u,v\rangle:=\int_a^b\rho(t)u(t)\overline{v(t)}\,\mathrm dt$ (8.1)
其中 $\overline{v(t)}$ 表示复共轭。则 $\mathcal C[a,b]$ 构成复数域上的内积空间(inner product space)。
由该内积诱导的范数为 $\|u\|_2:=\sqrt{\langle u,u\rangle}=\left(\int_a^b\rho(t)|u(t)|^2\,\mathrm dt\right)^{1/2}$ (8.2)
因此 $\mathcal C[a,b]$ 也是赋范线性空间(normed vector space)。

**※【定义 8.2】** 最小二乘逼近(least-squares approximation)：
在 $\mathcal C[a,b]$ 中，采用 (8.2) 定义的范数求解最佳逼近问题，
即给定函数 $f$ 和逼近空间 $V$，求 $\hat\varphi=\arg\min_{\varphi\in V}\|f-\varphi\|_2$
等价于 $\hat\varphi=\arg\min_{\varphi\in V}\int_a^b\rho(t)|f(t)-\varphi(t)|^2\,\mathrm dt$.

## §8.1 正交系统(Orthogonal Systems)

**※【定义 8.3】** 正交系统(orthogonal system)：
设 $S$ 为内积空间 $X$ 的子集。
如果对任意 $u,v\in S$，有 $\langle u,v\rangle=\begin{cases}0,&u\neq v,\\ >0,&u=v,\end{cases}$ (8.3)
则称 $S$ 为正交系统。

正交系统中的元素均非零，且任意两个不同元素相互正交。

**※【定理 8.4】** 有限个非零正交元素 $u_1,u_2,\ldots,u_n$ 必线性无关。
**证明**：
设 $\sum_{i=1}^n a_i u_i=0$。等式两端与 $u_j$ 作内积，由正交性可得 $0=\left\langle\sum_{i=1}^n a_i u_i,u_j\right\rangle=a_j\langle u_j,u_j\rangle$.
由于 $\langle u_j,u_j\rangle>0$，故 $a_j=0$。对所有 $j=1,\ldots,n$ 均成立，因此这些元素线性无关。

**※【定义 8.5】** 标准正交系统(orthonormal system)：
若 $S$ 为正交系统，且对任意 $u\in S$ 有 $\langle u,u\rangle=1$，则称 $S$ 为标准正交系统。

**※【例题 8.6】** 欧氏空间 $\mathbb R^n$ 的标准基向量：
$e_1=(1,0,\ldots,0)^\top,\quad\ldots,\quad e_n=(0,\ldots,0,1)^\top$.
在欧氏内积下满足 $\langle e_i,e_j\rangle=\delta_{ij}$，故构成标准正交系统。

**※【例题 8.7】** 第一类 Chebyshev 多项式(Chebyshev polynomials of the first kind)：
$T_n(x)=\cos(n\arccos x)$. 在区间 $[-1,1]$ 上，取权函数 $\rho(x)=\frac{1}{\sqrt{1-x^2}}$,
则第一类 Chebyshev 多项式关于 (8.1) 定义的内积相互正交，但并未标准化，因此不是标准正交系统。

### §8.1.1 正交多项式(Orthogonal Polynomials)

**※【定义 8.8】** 正交多项式：
设 $P=\{p_0,p_1,\ldots\}$ 为多项式集合，其中 $\deg p_i=i$。
若 $\forall p_i,p_j\in P,\quad i\neq j\Longrightarrow\langle p_i,p_j\rangle=0$ (8.4) 则称 $P$ 为正交多项式系统。

**※【例题 8.9】** 经典正交多项式：
对于实多项式，常采用加权内积 $\langle p_i,p_j\rangle:=\int_a^b\rho(x)p_i(x)p_j(x)\,\mathrm dx$ (8.5)
不同积分区间和权函数对应不同的经典正交多项式系统。

| 正交多项式 | 积分区间 | 权函数 $\rho(x)$ |
|---|---|---|
| 第一类 Chebyshev 多项式 | $[-1,1]$ | $\frac{1}{\sqrt{1-x^2}}$ |
| 第二类 Chebyshev 多项式 | $[-1,1]$ | $\sqrt{1-x^2}$ |
| Legendre 多项式 | $[-1,1]$ | $1$ |
| Jacobi 多项式 | $[-1,1]$ | $(1-x)^\alpha(1+x)^\beta$ |
| Laguerre 多项式 | $[0,+\infty)$ | $x^\alpha e^{-x}$ |
| Hermite 多项式 | $(-\infty,+\infty)$ | $e^{-x^2}$ |

其中 Jacobi 多项式要求 $\alpha,\beta>-1$，Laguerre 多项式要求 $\alpha>-1$，以保证相应积分存在。
注意：上述权函数确定的是正交关系。若要得到标准正交多项式，还需将各多项式除以其范数。

**※【定义 8.10】** 首一移位 Legendre 多项式(monic shifted Legendre polynomials)：
定义 $\phi_s(t)=t^s(t-1)^s$, 则首一移位 Legendre 多项式为 $\widetilde P_s(t)=\frac{s!}{(2s)!}\frac{\mathrm d^s}{\mathrm dt^s}\phi_s(t)$ (8.6)
其中 $s=0,1,2,\ldots$，且 $\widetilde P_0(t)\equiv1$。

这里的“首一”(monic) 指最高次项系数为 $1$，“移位”(shifted) 指将通常定义于 $[-1,1]$ 上的 Legendre 多项式移至 $[0,1]$。

**※【引理 8.11】** 首一移位 Legendre 多项式在 $[0,1]$ 上关于权函数 $\rho\equiv1$ 相互正交。
**证明**
只需证明对任意 $Q\in\mathbb P_{s-1}$，有 $\int_0^1\widetilde P_s(t)Q(t)\,\mathrm dt=0$ (8.7)
由 $\phi_s(t)=t^s(t-1)^s$ 可知，$0$ 和 $1$ 均为 $\phi_s$ 的 $s$ 重零点，因此
$\phi_s^{(k)}(0)=\phi_s^{(k)}(1)=0,\quad k=0,1,\ldots,s-1$.
利用分部积分，并注意所有边界项均为零，有
$\begin{aligned}\int_0^1\widetilde P_s(t)Q(t)\,\mathrm dt&=\frac{s!}{(2s)!}\int_0^1\phi_s^{(s)}(t)Q(t)\,\mathrm dt\\&=-\frac{s!}{(2s)!}\int_0^1\phi_s^{(s-1)}(t)Q'(t)\,\mathrm dt\\&=\cdots\\&=(-1)^s\frac{s!}{(2s)!}\int_0^1\phi_s(t)Q^{(s)}(t)\,\mathrm dt\\&=0.\end{aligned}$
最后一步利用了 $\deg Q\leq s-1$，故 $Q^{(s)}(t)\equiv0$。

**※【定理 8.12】** 正交多项式零点的性质：
设 $\{p_n\}_{n=0}^{\infty}$ 为区间 $[a,b]$ 上关于内积 (8.5) 的实正交多项式系统，其中 $\deg p_n=n$，且 $\rho(x)>0$。
则对任意 $n\geq1$，$p_n$ 的全部 $n$ 个零点均为实数、单根，且位于开区间 $(a,b)$ 内。

**证明**

首先证明 $p_n$ 在 $(a,b)$ 内至少存在一个变号零点。

若 $p_n$ 在 $[a,b]$ 上不变号，由于 $p_0$ 为非零常数，有

$\int_a^b\rho(x)p_n(x)\,\mathrm dx\neq0$,

从而 $\langle p_n,p_0\rangle\neq0$，与正交性矛盾。

其次，若 $x_1\in(a,b)$ 为 $p_n$ 的重根，则

$q(x)=\frac{p_n(x)}{(x-x_1)^2}\in\mathbb P_{n-2}$.

由于 $p_n$ 与所有次数低于 $n$ 的多项式正交，应有

$\langle p_n,q\rangle=0$.

但

$\langle p_n,q\rangle=\int_a^b\rho(x)\frac{p_n^2(x)}{(x-x_1)^2}\,\mathrm dx>0$,

矛盾。因此区间内部的零点均为单根。

最后，假设 $p_n$ 仅有 $j<n$ 个零点 $x_1,\ldots,x_j$ 位于 $(a,b)$ 内，构造

$v_j(x)=\prod_{i=1}^j(x-x_i)\in\mathbb P_j$.

则存在多项式 $P_{n-j}$，使得

$p_n(x)=P_{n-j}(x)v_j(x)$.

由于 $P_{n-j}$ 在 $(a,b)$ 内无零点，因此不变号。于是

$\langle p_n,v_j\rangle=\int_a^b\rho(x)P_{n-j}(x)v_j^2(x)\,\mathrm dx\neq0$.

然而 $\deg v_j=j<n$，根据正交性应有 $\langle p_n,v_j\rangle=0$，再次产生矛盾。

故 $p_n$ 的全部 $n$ 个零点均位于 $(a,b)$ 内，且均为单根。

### §8.1.2 Gram–Schmidt 正交化(Gram–Schmidt Process)

**※【定义 8.13】** Gram–Schmidt 正交化：

给定有限或无限的线性无关序列 $(u_1,u_2,\ldots)$，通过递推构造正交序列 $(v_1,v_2,\ldots)$ 和标准正交序列 $(u_1^*,u_2^*,\ldots)$。

初始条件为

$v_1=u_1,\qquad u_1^*=\frac{v_1}{\|v_1\|}$.

递推公式为

$v_{n+1}=u_{n+1}-\sum_{k=1}^n\langle u_{n+1},u_k^*\rangle u_k^*$ (8.8a)

$u_{n+1}^*=\frac{v_{n+1}}{\|v_{n+1}\|}$ (8.8b)

其中 $v_{n+1}$ 是从 $u_{n+1}$ 中减去其在已有标准正交基上的投影后得到的残差，$u_{n+1}^*$ 则是该残差的标准化结果。

图8.1：Gram–Schmidt 正交化的几何意义。将新向量分解为已有子空间中的投影分量与垂直于该子空间的残差分量。

**※【定理 8.14】** Gram–Schmidt 正交化的基本性质：

对线性无关序列 $(u_1,u_2,\ldots)$，Gram–Schmidt 正交化产生标准正交序列 $(u_1^*,u_2^*,\ldots)$，且存在系数 $a_{ki}$，使得

$\begin{cases}u_1^*=a_{11}u_1,\\u_2^*=a_{21}u_1+a_{22}u_2,\\u_3^*=a_{31}u_1+a_{32}u_2+a_{33}u_3,\\\qquad\vdots\end{cases}$ (8.9)

一般地，

$u_k^*=\sum_{i=1}^k a_{ki}u_i$,

其中

$a_{kk}=\frac{1}{\|v_k\|}>0$.

即变换系数矩阵为对角元素为正的下三角矩阵。

**证明**

由递推公式，

$u_n^*=\frac{1}{\|v_n\|}\left(u_n-\sum_{k=1}^{n-1}\langle u_n,u_k^*\rangle u_k^*\right)$.

利用数学归纳法，每个 $u_k^*$ 均可表示为 $u_1,\ldots,u_k$ 的线性组合，因此得到 (8.9)，且 $u_n$ 的系数为 $1/\|v_n\|$。

由于原序列线性无关，$u_n$ 不属于前 $n-1$ 个向量张成的空间，故 $v_n\neq0$，标准化总是有定义。

下面证明正交性。假设前 $n$ 个向量已经标准正交，即

$\langle u_k^*,u_j^*\rangle=\delta_{kj}$ (8.11)

则对任意 $j=1,\ldots,n$，有

$\begin{aligned}\langle v_{n+1},u_j^*\rangle&=\langle u_{n+1},u_j^*\rangle-\sum_{k=1}^n\langle u_{n+1},u_k^*\rangle\langle u_k^*,u_j^*\rangle\\&=\langle u_{n+1},u_j^*\rangle-\langle u_{n+1},u_j^*\rangle\\&=0.\end{aligned}$ (8.10)

因此 $u_{n+1}^*$ 与此前所有向量正交，且由 (8.8b) 可知 $\|u_{n+1}^*\|=1$。

由数学归纳法，整个序列标准正交。

**※【推论 8.16】** 正交化的逆变换：

对于线性无关序列 $(u_1,u_2,\ldots)$ 及其标准正交序列 $(u_1^*,u_2^*,\ldots)$，存在系数 $b_{ki}$，使得

$\begin{cases}u_1=b_{11}u_1^*,\\u_2=b_{21}u_1^*+b_{22}u_2^*,\\u_3=b_{31}u_1^*+b_{32}u_2^*+b_{33}u_3^*,\\\qquad\vdots\end{cases}$ (8.12)

其中 $b_{kk}>0$。

**证明**

由定理 8.14，变换矩阵 $A=(a_{ki})$ 为对角元素为正的下三角矩阵，因此可逆，且其逆矩阵仍为对角元素为正的下三角矩阵。

特别地，对任意 $n$，有

$\operatorname{span}\{u_1,\ldots,u_n\}=\operatorname{span}\{u_1^*,\ldots,u_n^*\}$.

即 Gram–Schmidt 正交化不改变各阶段向量张成的子空间。

**※【推论 8.17】** 正交化后的第 $n$ 个向量与原序列中前 $n-1$ 个向量正交：

$\langle u_n^*,u_i\rangle=0,\quad i=1,\ldots,n-1$.

**证明**

由推论 8.16，

$u_i=\sum_{k=1}^i b_{ik}u_k^*$.

因此

$\langle u_n^*,u_i\rangle=\sum_{k=1}^i\overline{b_{ik}}\langle u_n^*,u_k^*\rangle=0$.

**※【例题 8.18】** 利用 Gram–Schmidt 正交化构造前三个标准正交 Legendre 多项式：

取原始多项式序列

$u_1=1,\qquad u_2=x,\qquad u_3=x^2$,

并定义内积

$\langle u,v\rangle=\int_{-1}^1u(x)v(x)\,\mathrm dx$.

第一步：

$v_1=1$,

$\|v_1\|^2=\int_{-1}^1 1\,\mathrm dx=2$,

$u_1^*=\frac{1}{\sqrt2}$.

第二步：

$v_2=x-\left\langle x,\frac1{\sqrt2}\right\rangle\frac1{\sqrt2}=x$.

由于 $x$ 为奇函数，其在对称区间上的积分为零。

$\|v_2\|^2=\int_{-1}^1x^2\,\mathrm dx=\frac23$,

$u_2^*=\sqrt{\frac32}x$.

第三步：

$\begin{aligned}v_3&=x^2-\left\langle x^2,\sqrt{\frac32}x\right\rangle\sqrt{\frac32}x-\left\langle x^2,\frac1{\sqrt2}\right\rangle\frac1{\sqrt2}\\&=x^2-\frac13.\end{aligned}$

计算范数：

$\|v_3\|^2=\int_{-1}^1\left(x^2-\frac13\right)^2\,\mathrm dx=\frac8{45}$.

因此

$u_3^*=\frac{3\sqrt{10}}4\left(x^2-\frac13\right)=\frac{\sqrt{10}}4(3x^2-1)$.

最终得到标准正交多项式：

$u_1^*=\frac1{\sqrt2},\qquad u_2^*=\sqrt{\frac32}x,\qquad u_3^*=\frac{\sqrt{10}}4(3x^2-1)$.

### §8.1.3 正交展开(Orthogonal Expansions)

**※【定义 8.19】** 正交展开或 Fourier 展开(Fourier expansion)：

设 $(u_1^*,u_2^*,\ldots)$ 为有限或无限标准正交序列。对任意元素 $w$，其关于前 $m$ 个标准正交元素的展开为

$\sum_{n=1}^m\langle w,u_n^*\rangle u_n^*$ (8.13)

其中

- $\langle w,u_n^*\rangle$ 称为 Fourier 系数(Fourier coefficient)；
- $\langle w,u_n^*\rangle u_n^*$ 称为 $w$ 在 $u_n^*$ 上的投影(projection)；
- $\sum_{n=1}^m\langle w,u_n^*\rangle u_n^*-w$ 为展开误差。

对于无限正交序列，可以进一步研究相应级数的收敛性，但正交性本身不保证该级数收敛至 $w$。

**※【例题 8.20】** 欧氏空间中的正交展开：

在 $\mathbb R^3$ 中取标准正交基

$u_1^*=(1,0,0)^\top,\quad u_2^*=(0,1,0)^\top,\quad u_3^*=(0,0,1)^\top$.

对于 $w=(a,b,c)^\top$，有

$\langle w,u_1^*\rangle=a,\qquad\langle w,u_2^*\rangle=b,\qquad\langle w,u_3^*\rangle=c$.

因此

$w=\langle w,u_1^*\rangle u_1^*+\langle w,u_2^*\rangle u_2^*+\langle w,u_3^*\rangle u_3^*$.

**※【定理 8.22】** 子空间内元素的精确正交展开：

设 $u_1,\ldots,u_n$ 线性无关，$u_1^*,\ldots,u_n^*$ 为其经过 Gram–Schmidt 正交化得到的标准正交基。

若

$w=\sum_{i=1}^n a_i u_i$,

则

$w=\sum_{i=1}^n\langle w,u_i^*\rangle u_i^*$ (8.16)

即子空间内的任意元素均等于其关于该子空间标准正交基的展开。

**证明**

由推论 8.16，存在系数 $c_i$，使得

$w=\sum_{i=1}^n c_i u_i^*$.

两端与 $u_k^*$ 作内积，由标准正交性，

$\langle w,u_k^*\rangle=\sum_{i=1}^n c_i\langle u_i^*,u_k^*\rangle=c_k$.

代回即得结论。

**※【定理 8.23】** 正交展开的最小性质(minimum property of orthogonal expansions)：

设 $(u_1^*,u_2^*,\ldots)$ 为标准正交系统，则对于任意元素 $w$ 和任意系数 $a_1,\ldots,a_N$，有

$\left\|w-\sum_{i=1}^N\langle w,u_i^*\rangle u_i^*\right\|\leq\left\|w-\sum_{i=1}^N a_i u_i^*\right\|$ (8.17)

即正交展开给出了 $w$ 在子空间 $\operatorname{span}\{u_1^*,\ldots,u_N^*\}$ 中的最佳逼近。

**证明**

记 $c_i=\langle w,u_i^*\rangle$，以下求和均从 $1$ 到 $N$。

由内积的性质及标准正交性，

$\begin{aligned}\left\|w-\sum_i a_i u_i^*\right\|^2&=\left\langle w-\sum_i a_i u_i^*,w-\sum_j a_j u_j^*\right\rangle\\&=\|w\|^2-\sum_i\overline{a_i}c_i-\sum_i a_i\overline{c_i}+\sum_i|a_i|^2\\&=\|w\|^2-\sum_i|c_i|^2+\sum_i|a_i-c_i|^2.\end{aligned}$ (8.18)

前两项与待定系数 $a_i$ 无关，第三项非负，且当且仅当 $a_i=c_i$ 时取零。

因此唯一的最优系数为

$a_i=\langle w,u_i^*\rangle,\quad i=1,\ldots,N$.

**※【推论 8.24】** 最佳线性逼近及其误差：

设 $(u_1,\ldots,u_n)$ 为线性无关序列，$u_1^*,\ldots,u_n^*$ 为其标准正交化结果，则 $w$ 的最佳线性逼近为

$\hat\varphi=\sum_{k=1}^n\langle w,u_k^*\rangle u_k^*$.

其误差满足

$\begin{aligned}\|w-\hat\varphi\|^2&=\min_{a_1,\ldots,a_n}\left\|w-\sum_{k=1}^n a_k u_k\right\|^2\\&=\|w\|^2-\sum_{k=1}^n|\langle w,u_k^*\rangle|^2.\end{aligned}$ (8.19)

**证明**

Gram–Schmidt 正交化不改变逼近空间，由 (8.18) 直接得到结论。

**※【推论 8.25】** Bessel 不等式(Bessel's inequality)：

若 $u_1^*,\ldots,u_N^*$ 为标准正交序列，则对任意 $w$，有

$\sum_{i=1}^N|\langle w,u_i^*\rangle|^2\leq\|w\|^2$ (8.20)

**证明**

由 (8.19) 及范数平方的非负性，

$0\leq\|w-\hat\varphi\|^2=\|w\|^2-\sum_{i=1}^N|\langle w,u_i^*\rangle|^2$.

移项即得结论。

**※【推论 8.26】** Gram–Schmidt 正交化中的残差范数：

对任意 $n\geq1$，有

$\|v_{n+1}\|^2=\|u_{n+1}\|^2-\sum_{k=1}^n|\langle u_{n+1},u_k^*\rangle|^2$ (8.21)

**证明**

由 (8.8a)，$v_{n+1}$ 恰为 $u_{n+1}$ 关于前 $n$ 个标准正交向量的展开残差。令推论 8.24 中的 $w=u_{n+1}$ 即得结论。

**※【例题 8.27】** 利用正交展开求 $e^x$ 的最佳多项式逼近：

在 $[-1,1]$ 上取权函数 $\rho\equiv1$，求解

$\min_{a_0,\ldots,a_n}\int_{-1}^1\left(e^x-\sum_{i=0}^n a_i x^i\right)^2\,\mathrm dx$ (8.22)

考虑 $n=1,2$ 的情形。

利用例题 8.18 中构造的标准正交 Legendre 多项式：

$u_1^*=\frac1{\sqrt2},\qquad u_2^*=\sqrt{\frac32}x,\qquad u_3^*=\frac{\sqrt{10}}4(3x^2-1)$.

计算 Fourier 系数：

$\begin{aligned}b_1&=\langle e^x,u_1^*\rangle\\&=\frac1{\sqrt2}\int_{-1}^1e^x\,\mathrm dx\\&=\frac1{\sqrt2}\left(e-\frac1e\right).\end{aligned}$

$\begin{aligned}b_2&=\langle e^x,u_2^*\rangle\\&=\sqrt{\frac32}\int_{-1}^1xe^x\,\mathrm dx\\&=\sqrt{\frac32}\left[(x-1)e^x\right]_{-1}^1\\&=\frac{\sqrt6}{e}.\end{aligned}$

$\begin{aligned}b_3&=\langle e^x,u_3^*\rangle\\&=\frac{\sqrt{10}}4\int_{-1}^1(3x^2-1)e^x\,\mathrm dx\\&=\frac{\sqrt{10}}2\left(e-\frac7e\right).\end{aligned}$

因此一次最佳逼近多项式为

$\hat\varphi_1(x)=b_1u_1^*+b_2u_2^*=\frac{e^2-1}{2e}+\frac3e x$.

二次最佳逼近多项式为

$\hat\varphi_2(x)=\hat\varphi_1(x)+\frac5{4e}(e^2-7)(3x^2-1)$.

综上，

$\hat\varphi_n(x)=\begin{cases}\frac{e^2-1}{2e}+\frac3e x,&n=1,\\\hat\varphi_1(x)+\frac5{4e}(e^2-7)(3x^2-1),&n=2.\end{cases}$ (8.23)

采用标准正交基的一个优势是：增加逼近多项式的次数时，原有 Fourier 系数无需重新计算，只需计算新增基函数对应的系数。

## §8.2 正规方程(The Normal Equations)

**※【定理 8.28】** 正交展开残差的正交性：

设 $u_1,\ldots,u_n$ 为内积空间中的线性无关元素，$u_1^*,\ldots,u_n^*$ 为其标准正交化结果。

则对任意元素 $w$，有

$\forall j=1,\ldots,n,\quad\left(w-\sum_{k=1}^n\langle w,u_k^*\rangle u_k^*\right)\perp u_j^*$ (8.24)

即正交展开的残差与整个逼近子空间正交。

**证明**

若 $w$ 属于该逼近子空间，则由定理 8.22 可知残差为零。

否则，可将 $w$ 作为原线性无关序列的第 $n+1$ 个元素。根据 Gram–Schmidt 正交化公式，

$v_{n+1}=w-\sum_{k=1}^n\langle w,u_k^*\rangle u_k^*$.

由 (8.10)，$v_{n+1}$ 与此前所有标准正交元素正交。

**※【推论 8.29】** 最佳线性逼近的正交条件：

设 $u_1,\ldots,u_n$ 线性无关，若

$\hat\varphi=\sum_{k=1}^n a_k u_k$

为 $w$ 的最佳线性逼近，则

$\forall j=1,\ldots,n,\quad(w-\hat\varphi)\perp u_j$ (8.25)

即

$\langle w-\hat\varphi,u_j\rangle=0,\quad j=1,\ldots,n$.

**证明**

由定理 8.23，最佳逼近可表示为

$\hat\varphi=\sum_{k=1}^n\langle w,u_k^*\rangle u_k^*$.

根据定理 8.28，残差与所有 $u_k^*$ 正交。又由推论 8.16，每个 $u_j$ 都可表示为标准正交基的线性组合，故残差也与所有 $u_j$ 正交。

**※【定义 8.30】** Gram 矩阵(Gram matrix)与 Gram 行列式(Gram determinant)：

设 $u_1,\ldots,u_n$ 为内积空间中的元素，定义

$G(u_1,\ldots,u_n)=(\langle u_i,u_j\rangle)_{i,j=1}^n$

$=\begin{pmatrix}\langle u_1,u_1\rangle&\langle u_1,u_2\rangle&\cdots&\langle u_1,u_n\rangle\\\langle u_2,u_1\rangle&\langle u_2,u_2\rangle&\cdots&\langle u_2,u_n\rangle\\\vdots&\vdots&\ddots&\vdots\\\langle u_n,u_1\rangle&\langle u_n,u_2\rangle&\cdots&\langle u_n,u_n\rangle\end{pmatrix}$ (8.26)

称为 Gram 矩阵。

其行列式

$g(u_1,\ldots,u_n)=\det G(u_1,\ldots,u_n)$ (8.27)

称为 Gram 行列式。

由于内积满足共轭对称性，

$G_{ij}=\overline{G_{ji}}$,

因此 Gram 矩阵为 Hermitian 矩阵。在实数域上，Gram 矩阵为实对称矩阵。

**※【引理 8.31】** Gram 矩阵在线性变换下的性质：

设

$w_i=\sum_{j=1}^n a_{ij}u_j,\quad i=1,\ldots,n$,

记 $A=(a_{ij})$，$A^H=(\overline{a_{ji}})$ 为其共轭转置，则

$G(w_1,\ldots,w_n)=AG(u_1,\ldots,u_n)A^H$ (8.28)

相应地，

$g(w_1,\ldots,w_n)=|\det A|^2g(u_1,\ldots,u_n)$ (8.29)

**证明**

利用内积对第一个变量的线性性及对第二个变量的共轭线性性，

$\begin{aligned}\langle w_i,w_j\rangle&=\left\langle\sum_{k=1}^n a_{ik}u_k,\sum_{\ell=1}^n a_{j\ell}u_\ell\right\rangle\\&=\sum_{k=1}^n\sum_{\ell=1}^n a_{ik}\langle u_k,u_\ell\rangle\overline{a_{j\ell}}.\end{aligned}$

写成矩阵形式即为 (8.28)。

又因为

$\det A^H=\overline{\det A}$,

所以

$\begin{aligned}\det G(w_1,\ldots,w_n)&=\det A\cdot\det G(u_1,\ldots,u_n)\cdot\det A^H\\&=|\det A|^2g(u_1,\ldots,u_n).\end{aligned}$

**※【定理 8.32】** Gram 行列式的上下界：

设 $u_1,\ldots,u_n$ 为非零元素，则

$0\leq g(u_1,\ldots,u_n)\leq\prod_{k=1}^n\|u_k\|^2$ (8.30)

其中：

- 左侧等号成立当且仅当 $u_1,\ldots,u_n$ 线性相关；
- 右侧等号成立当且仅当 $u_1,\ldots,u_n$ 相互正交。

**证明**

第一步：证明线性相关时 Gram 行列式为零。

若 $u_1,\ldots,u_n$ 线性相关，则存在不全为零的系数 $c_1,\ldots,c_n$，使得

$\sum_{i=1}^n c_i u_i=0$.

取满足 $c_j\neq0$ 的下标 $j$，构造

$w_k=\begin{cases}\sum_{i=1}^n c_i u_i=0,&k=j,\\u_k,&k\neq j.\end{cases}$

由于 $w_j=0$，Gram 矩阵的第 $j$ 行和第 $j$ 列均为零，故

$g(w_1,\ldots,w_n)=0$.

设对应的线性变换矩阵为 $C$，即将单位矩阵的第 $j$ 行替换为 $(c_1,\ldots,c_n)$。

按第 $j$ 行展开行列式可得

$\det C=c_j\neq0$.

由引理 8.31，

$0=|\det C|^2g(u_1,\ldots,u_n)$.

因此 $g(u_1,\ldots,u_n)=0$。

第二步：证明线性无关时 Gram 行列式严格为正。

若 $u_1,\ldots,u_n$ 线性无关，则通过 Gram–Schmidt 正交化可得

$u_k^*=\sum_{i=1}^k a_{ki}u_i,\qquad a_{kk}=\frac1{\|v_k\|}>0$.

记 $A=(a_{ki})$，则

$\det A=\prod_{k=1}^n a_{kk}$.

由于 $u_1^*,\ldots,u_n^*$ 标准正交，

$G(u_1^*,\ldots,u_n^*)=I$.

由引理 8.31，

$1=|\det A|^2g(u_1,\ldots,u_n)$.

因此

$g(u_1,\ldots,u_n)=\prod_{k=1}^n\frac1{a_{kk}^2}=\prod_{k=1}^n\|v_k\|^2>0$ (8.31)

第三步：证明上界及等号成立条件。

由推论 8.26，

$\|v_k\|^2=\|u_k\|^2-\sum_{j=1}^{k-1}|\langle u_k,u_j^*\rangle|^2\leq\|u_k\|^2$.

因此

$g(u_1,\ldots,u_n)=\prod_{k=1}^n\|v_k\|^2\leq\prod_{k=1}^n\|u_k\|^2$.

若 $u_1,\ldots,u_n$ 相互正交，则 Gram 矩阵为对角矩阵，直接得到

$g(u_1,\ldots,u_n)=\prod_{k=1}^n\|u_k\|^2$ (8.32)

反之，若上界等号成立，由于所有 $\|u_k\|>0$，可知原序列线性无关，且每一项均须满足

$\|v_k\|^2=\|u_k\|^2$ (8.33)

因此

$\sum_{j=1}^{k-1}|\langle u_k,u_j^*\rangle|^2=0$,

从而

$\langle u_k,u_j^*\rangle=0,\quad j<k$.

根据推论 8.16，$u_k$ 与所有 $u_1,\ldots,u_{k-1}$ 正交，故原序列相互正交。

**※【定理 8.33】** 正规方程：

设 $u_1,\ldots,u_n$ 线性无关，且

$\hat\varphi=\sum_{i=1}^n a_i u_i$

为 $w$ 的最佳线性逼近。则系数向量

$\mathbf a=(a_1,\ldots,a_n)^\top$

由以下正规方程(normal equations) 唯一确定：

$G(u_1,\ldots,u_n)^\top\mathbf a=\mathbf c$ (8.34)

其中

$\mathbf c=(\langle w,u_1\rangle,\ldots,\langle w,u_n\rangle)^\top$.

**证明**

由推论 8.29，

$\langle w-\hat\varphi,u_j\rangle=0$.

因此

$\langle w,u_j\rangle=\sum_{k=1}^n a_k\langle u_k,u_j\rangle,\quad j=1,\ldots,n$.

将上述方程写成矩阵形式，即得 (8.34)。

由定理 8.32，线性无关性保证 $\det G>0$，故 $G$ 可逆，正规方程具有唯一解。

注意：本章采用内积对第一个变量线性的约定，并定义 $G_{ij}=\langle u_i,u_j\rangle$，因此正规方程中出现 $G^\top$。在实数域上，由于 $G^\top=G$，可直接写为

$G\mathbf a=\mathbf c$.

**※【例题 8.34】** 利用正规方程求 $e^x$ 的最佳二次多项式逼近：

求

$\hat\varphi(x)=a_0+a_1x+a_2x^2$

使得

$\int_{-1}^1(e^x-\hat\varphi(x))^2\,\mathrm dx$

最小。

取基函数 $(1,x,x^2)$，权函数 $\rho=1$。

首先构造 Gram 矩阵：

$G=\begin{pmatrix}\langle1,1\rangle&\langle1,x\rangle&\langle1,x^2\rangle\\\langle x,1\rangle&\langle x,x\rangle&\langle x,x^2\rangle\\\langle x^2,1\rangle&\langle x^2,x\rangle&\langle x^2,x^2\rangle\end{pmatrix}$

$=\begin{pmatrix}2&0&\frac23\\0&\frac23&0\\\frac23&0&\frac25\end{pmatrix}$.

然后计算右端向量：

$\mathbf c=\begin{pmatrix}\langle e^x,1\rangle\\\langle e^x,x\rangle\\\langle e^x,x^2\rangle\end{pmatrix}=\begin{pmatrix}e-\frac1e\\\frac2e\\e-\frac5e\end{pmatrix}$.

原文右端向量第二项写为 $2/3$，但直接积分可得 $\int_{-1}^1xe^x\,\mathrm dx=2/e$，且原文最终系数与 $2/e$ 一致，此处已修正。

求解正规方程

$\begin{pmatrix}2&0&\frac23\\0&\frac23&0\\\frac23&0&\frac25\end{pmatrix}\begin{pmatrix}a_0\\a_1\\a_2\end{pmatrix}=\begin{pmatrix}e-\frac1e\\\frac2e\\e-\frac5e\end{pmatrix}$.

得到

$a_0=\frac{3(11-e^2)}{4e}$,

$a_1=\frac3e$,

$a_2=\frac{15(e^2-7)}{4e}$.

因此

$\hat\varphi(x)=\frac{3(11-e^2)}{4e}+\frac3e x+\frac{15(e^2-7)}{4e}x^2$.

该结果与例题 8.27 中利用正交展开得到的二次最佳逼近多项式完全一致。

**※【例题 8.35】** Hilbert 矩阵(Hilbert matrix)作为 Gram 矩阵：

设 $f\in\mathcal C[0,1]$，利用多项式

$p(x)=\sum_{k=0}^n\alpha_kx^k$

进行最小二乘逼近，取权函数 $\rho=1$。

以单项式 $(1,x,\ldots,x^n)$ 为基函数，有

$\langle x^j,x^k\rangle=\int_0^1x^{j+k}\,\mathrm dx=\frac1{j+k+1}$.

因此 Gram 矩阵恰为 Hilbert 矩阵：

$G=\begin{pmatrix}1&\frac12&\frac13&\cdots&\frac1{n+1}\\\frac12&\frac13&\frac14&\cdots&\frac1{n+2}\\\frac13&\frac14&\frac15&\cdots&\frac1{n+3}\\\vdots&\vdots&\vdots&\ddots&\vdots\\\frac1{n+1}&\frac1{n+2}&\frac1{n+3}&\cdots&\frac1{2n+1}\end{pmatrix}$.

正规方程为

$\forall j=0,\ldots,n,\quad\sum_{k=0}^n\frac{\alpha_k}{j+k+1}=\int_0^1f(x)x^j\,\mathrm dx$ (8.35)

特别地，当

$f(x)=\frac1{1+x}$

时，定义右端项

$r_j=\int_0^1\frac{x^j}{1+x}\,\mathrm dx$.

原文给出的计算公式为

$r_j=(-1)^j\ln2+\sum_{i=1}^j\frac{(-1)^{i+j}}i,\quad j=0,\ldots,n$ (8.36)

Hilbert 矩阵虽然非奇异，但随着阶数增加会变得高度病态，因此正规方程的唯一可解性并不意味着其数值求解具有良好的稳定性。

## §8.3 离散最小二乘(Discrete Least Squares, DLS)

**※【例题 8.36】** 利用离散最小二乘处理牛顿第二定律的实验数据：

考虑质量为 $M$ 的小车，在水平轨道上通过滑轮与质量为 $m_j$ 的悬挂物相连。

忽略轨道和滑轮的摩擦，假设牛顿第二定律成立，则系统满足

$m_jg=(m_j+M)a_j=(m_j+M)\frac{\mathrm d^2x}{\mathrm dt^2}$.

小车所受拉力为

$F_j=m_j(g-a_j)$.

实验步骤如下：

1. 固定 $M$ 和 $m_j$，利用高速摄像机记录小车的位置，得到离散数据 $(t_i,x_i)$。
2. 利用二次多项式 $p(t)=c_0+c_1t+c_2t^2$ 拟合运动轨迹，即求解

   $\min_{c_0,c_1,c_2}\sum_i(x_i-p(t_i))^2$.

3. 根据二次多项式的二阶导数，得到实验加速度

   $a_j=p''(t)=2c_2$,

   并计算拉力 $F_j=m_j(g-a_j)$。

4. 改变悬挂物质量 $m_j$，重复上述实验，得到一系列数据 $(a_j,F_j)$。
5. 利用一次多项式 $f(a)=c_0+c_1a$ 拟合力与加速度的关系：

   $\min_{c_0,c_1}\sum_j(F_j-f(a_j))^2$.

若拟合结果满足 $c_1\approx M$，则实验结果与牛顿第二定律 $F=Ma$ 一致。

注意：这一过程并未从逻辑上推导或证明牛顿第二定律，而是表明实验结果与该定律不存在明显矛盾。

### §8.3.1 Gaussian 函数与 Dirac Delta 函数

**※【定义 8.37】** Gaussian 函数(Gaussian function)：

形如

$f(x)=a\exp\left(-\frac{(x-b)^2}{2c^2}\right)$ (8.37)

的函数称为 Gaussian 函数，其中

- $a>0$ 为峰值高度；
- $b\in\mathbb R$ 为峰值中心位置；
- $c>0$ 为标准差或均方根宽度(root mean square width, RMS width)。

图8.2：Gaussian 函数具有对称的钟形曲线，参数 $c$ 控制曲线宽度。

**※【引理 8.38】** Gaussian 积分：

$\int_{-\infty}^{+\infty}a\exp\left(-\frac{(x-b)^2}{2c^2}\right)\,\mathrm dx=ac\sqrt{2\pi}$ (8.38)

**证明**

首先计算

$I=\int_{-\infty}^{+\infty}e^{-x^2}\,\mathrm dx$.

利用两个一维积分的乘积构造二维积分：

$I^2=\int_{\mathbb R^2}e^{-(x^2+y^2)}\,\mathrm dx\,\mathrm dy$.

采用极坐标变换 $x=r\cos\theta$，$y=r\sin\theta$，有

$\begin{aligned}I^2&=\int_0^{2\pi}\int_0^{+\infty}e^{-r^2}r\,\mathrm dr\,\mathrm d\theta\\&=2\pi\left[-\frac12e^{-r^2}\right]_0^{+\infty}\\&=\pi.\end{aligned}$

由于 $I>0$，故 $I=\sqrt\pi$。

令 $x=b+\sqrt2cy$，则

$\begin{aligned}\int_{-\infty}^{+\infty}a\exp\left(-\frac{(x-b)^2}{2c^2}\right)\,\mathrm dx&=\sqrt2ac\int_{-\infty}^{+\infty}e^{-y^2}\,\mathrm dy\\&=ac\sqrt{2\pi}.\end{aligned}$

**※【定义 8.39】** 正态分布(normal distribution)或 Gaussian 分布(Gaussian distribution)：

其概率密度函数为

$f_{\mu,\sigma}(x)=\frac1{\sigma\sqrt{2\pi}}\exp\left(-\frac{(x-\mu)^2}{2\sigma^2}\right)$ (8.39)

其中 $\mu\in\mathbb R$ 为均值，$\sigma>0$ 为标准差。

**※【定义 8.40】** Dirac Delta 函数(Dirac delta function)：

以 $\bar x$ 为中心的 Dirac Delta 函数可通过标准差趋于零的 Gaussian 函数定义。

令

$\phi_\epsilon(x-\bar x)=\frac1{\epsilon\sqrt{2\pi}}\exp\left(-\frac{(x-\bar x)^2}{2\epsilon^2}\right)$,

则

$\delta(x-\bar x)=\lim_{\epsilon\to0^+}\phi_\epsilon(x-\bar x)$ (8.40)

这里的极限应理解为广义函数(distribution)意义下的极限，而非通常的逐点函数极限。

**※【引理 8.41】** Dirac Delta 函数的基本性质：

形式上可写为

$\delta(x-\bar x)=\begin{cases}+\infty,&x=\bar x,\\0,&x\neq\bar x.\end{cases}$ (8.41a)

并满足

$\int_{-\infty}^{+\infty}\delta(x-\bar x)\,\mathrm dx=1$ (8.41b)

**证明**

当 $x=\bar x$ 时，

$\phi_\epsilon(0)=\frac1{\epsilon\sqrt{2\pi}}\to+\infty$.

当 $x\neq\bar x$ 时，指数衰减快于 $1/\epsilon$ 的增长，故

$\phi_\epsilon(x-\bar x)\to0$.

另一方面，由 Gaussian 积分，

$\int_{\mathbb R}\phi_\epsilon(x-\bar x)\,\mathrm dx=1$.

在广义函数或测度意义下取极限，即得到 (8.41b)。

注意：Dirac Delta 不是普通意义下的可积函数。不能仅根据 (8.41a) 进行普通积分并推出 (8.41b)。

**※【引理 8.42】** Dirac Delta 函数的筛选性质(sifting property)：

若 $f:\mathbb R\to\mathbb R$ 连续，则

$\int_{-\infty}^{+\infty}\delta(x-\bar x)f(x)\,\mathrm dx=f(\bar x)$ (8.42)

**证明**

取包含 $\bar x$ 的紧区间

$I_\epsilon=[\bar x-\epsilon,\bar x+\epsilon]$.

由连续性，$f$ 在该区间上有界，记

$m_\epsilon=\min_{x\in I_\epsilon}f(x),\qquad M_\epsilon=\max_{x\in I_\epsilon}f(x)$.

由于 Dirac 测度非负且集中于 $\bar x$，有

$\int_{\mathbb R}\delta(x-\bar x)f(x)\,\mathrm dx=\int_{I_\epsilon}\delta(x-\bar x)f(x)\,\mathrm dx$.

结合单位质量性质，得到

$m_\epsilon\leq\int_{\mathbb R}\delta(x-\bar x)f(x)\,\mathrm dx\leq M_\epsilon$.

当 $\epsilon\to0^+$ 时，由 $f$ 在 $\bar x$ 处连续，

$m_\epsilon\to f(\bar x),\qquad M_\epsilon\to f(\bar x)$.

由夹逼定理即得结论。

**※【定义 8.43】** Heaviside 函数(Heaviside function)或阶跃函数(step function)：

$H(x)=\begin{cases}0,&x<0,\\1,&x\geq0.\end{cases}$ (8.43)

**※【引理 8.44】** Dirac Delta 函数与 Heaviside 函数的关系：

$\int_{-\infty}^x\delta(t)\,\mathrm dt=H(x)$ (8.44)

其中积分按 Dirac 测度在 $(-\infty,x]$ 上的质量理解。

相应地，在广义函数意义下有

$H'(x)=\delta(x)$.

注意：若直接对称 Gaussian 正则化后再取极限，则在 $x=0$ 处得到 $1/2$。这与本章选取 $H(0)=1$ 的测度约定不同，但不影响广义导数关系。

### §8.3.2 连续与离散最小二乘的统一形式(Reusing the Formalism)

**※【定义 8.45】** 连续测度(continuous measure)：

设 $\rho$ 为区间 $[a,b]$ 上的非负可积权函数，定义

$\lambda(t)=\begin{cases}0,&t<a,\\\int_a^t\rho(\tau)\,\mathrm d\tau,&a\leq t\leq b,\\\int_a^b\rho(\tau)\,\mathrm d\tau,&t>b.\end{cases}$ (8.45)

相应的连续测度为

$\mathrm d\lambda=\begin{cases}\rho(t)\,\mathrm dt,&t\in[a,b],\\0,&\text{otherwise}.\end{cases}$ (8.46)

在本章权函数于区间内部严格为正的条件下，其支撑集(support) 为 $[a,b]$。

**※【定义 8.46】** 离散测度(discrete measure)或 Dirac 测度(Dirac measure)：

给定点集 $\{t_1,\ldots,t_N\}$ 及正权重 $\rho_1,\ldots,\rho_N$，定义集中于这些点的离散测度 $\mathrm d\lambda$，其中每个点 $t_i$ 的质量为 $\rho_i$。

其支撑集为 $\{t_1,\ldots,t_N\}$。

**※【引理 8.47】** 离散测度的积分公式：

定义

$\lambda(t)=\sum_{i=1}^N\rho_iH(t-t_i)$ (8.47)

则对于在这些点上有定义的函数 $u$，有

$\int_{\mathbb R}u(t)\,\mathrm d\lambda=\sum_{i=1}^N\rho_i u(t_i)$ (8.48)

**证明**

由 Heaviside 函数与 Dirac Delta 函数的关系，

$\mathrm d\lambda=\sum_{i=1}^N\rho_i\delta(t-t_i)\,\mathrm dt$.

再利用 Dirac Delta 函数的筛选性质，

$\begin{aligned}\int_{\mathbb R}u(t)\,\mathrm d\lambda&=\int_{\mathbb R}\sum_{i=1}^N\rho_i\delta(t-t_i)u(t)\,\mathrm dt\\&=\sum_{i=1}^N\rho_i u(t_i).\end{aligned}$

因此连续内积和离散内积可统一表示为

$\langle u,v\rangle=\int_{\mathbb R}u(t)\overline{v(t)}\,\mathrm d\lambda$.

对于连续测度，该式为

$\langle u,v\rangle=\int_a^b\rho(t)u(t)\overline{v(t)}\,\mathrm dt$.

对于离散测度，该式为

$\langle u,v\rangle=\sum_{i=1}^N\rho_i u(t_i)\overline{v(t_i)}$.

相应的离散最小二乘问题为

$\min_{\varphi\in V}\sum_{i=1}^N\rho_i|y_i-\varphi(t_i)|^2$.

注意：离散内积在离散数据向量空间上满足正定性；若直接定义在全部连续函数上，则非零函数可能在所有采样点处取零值，此时只得到半内积。应用前述最佳逼近理论时，需要保证基函数在采样点上形成的向量线性无关。

### §8.3.3 利用正规方程求解离散最小二乘(DLS via Normal Equations)

**※【例题 8.48】** 销售数据的二次多项式拟合：

给定以下销售记录：

| $x_i$ | $y_i$ |
|---:|---:|
| 1 | 256 |
| 2 | 201 |
| 3 | 159 |
| 4 | 61 |
| 5 | 77 |
| 6 | 40 |
| 7 | 17 |
| 8 | 25 |
| 9 | 103 |
| 10 | 156 |
| 11 | 222 |
| 12 | 345 |

图8.3：销售记录及其二次多项式拟合曲线。数据整体呈先下降后上升的趋势，适合采用二次多项式逼近。

设

$p(x)=a_0+a_1x+a_2x^2$,

求解

$\min_{a_0,a_1,a_2}\sum_{i=1}^{12}(y_i-a_0-a_1x_i-a_2x_i^2)^2$.

定义离散内积

$\langle u,v\rangle=\sum_{i=1}^{12}u(x_i)v(x_i)$.

首先构造 Gram 矩阵：

$G=\begin{pmatrix}\langle1,1\rangle&\langle1,x\rangle&\langle1,x^2\rangle\\\langle x,1\rangle&\langle x,x\rangle&\langle x,x^2\rangle\\\langle x^2,1\rangle&\langle x^2,x\rangle&\langle x^2,x^2\rangle\end{pmatrix}$

$=\begin{pmatrix}12&78&650\\78&650&6084\\650&6084&60710\end{pmatrix}$.

右端向量为

$\mathbf c=\begin{pmatrix}\langle y,1\rangle\\\langle y,x\rangle\\\langle y,x^2\rangle\end{pmatrix}=\begin{pmatrix}\sum_{i=1}^{12}y_i\\\sum_{i=1}^{12}y_ix_i\\\sum_{i=1}^{12}y_ix_i^2\end{pmatrix}=\begin{pmatrix}1662\\11392\\109750\end{pmatrix}$.

求解正规方程

$G\mathbf a=\mathbf c$,

得到

$\mathbf a=G^{-1}\mathbf c\approx\begin{pmatrix}386.00\\-113.43\\9.04\end{pmatrix}$.

因此最佳二次拟合多项式为

$p(x)\approx386.00-113.43x+9.04x^2$.

对于一般离散最小二乘问题，设基函数为 $\varphi_1,\ldots,\varphi_n$，定义设计矩阵(design matrix)

$A=\begin{pmatrix}\varphi_1(x_1)&\cdots&\varphi_n(x_1)\\\varphi_1(x_2)&\cdots&\varphi_n(x_2)\\\vdots&\ddots&\vdots\\\varphi_1(x_m)&\cdots&\varphi_n(x_m)\end{pmatrix}$.

在等权情形下，离散最小二乘问题可写为

$\min_{\mathbf a}\|A\mathbf a-\mathbf y\|_2^2$,

相应正规方程为

$A^\top A\mathbf a=A^\top\mathbf y$.

若存在正权重 $\rho_i$，令 $W=\operatorname{diag}(\rho_1,\ldots,\rho_m)$，则相应正规方程为

$A^\top WA\mathbf a=A^\top W\mathbf y$.

### §8.3.4 利用 QR 分解求解离散最小二乘(DLS via QR Decomposition)

**※【定义 8.49】** 正交矩阵(orthogonal matrix)：

若实方阵 $Q\in\mathbb R^{n\times n}$ 满足

$Q^\top Q=I$,

则称 $Q$ 为正交矩阵。

因此 $Q^{-1}=Q^\top$，且其列向量构成标准正交基。

**※【定理 8.50】** QR 分解(QR factorization)：

对于任意矩阵 $A\in\mathbb R^{m\times n}$，存在正交矩阵 $Q\in\mathbb R^{m\times m}$ 和上梯形矩阵 $R\in\mathbb R^{m\times n}$，使得

$A=QR$.

当 $m=n$ 时，$R$ 为上三角矩阵。

**证明**

将 $A$ 按列表示为

$A=[\xi_1,\ldots,\xi_n]$,

记 $r=\operatorname{rank}(A)$。

若 $r=0$，取 $Q=I$，$R=0$ 即可。以下考虑 $r>0$。

第一步：按原有列顺序选取线性无关列。

选取第一个非零列作为 $u_1=\xi_{k_1}$。

对于 $j=2,\ldots,r$，选取第一个不属于

$\operatorname{span}\{u_1,\ldots,u_{j-1}\}$

的后续列作为 $u_j=\xi_{k_j}$。

由此得到

$A_r=[u_1,\ldots,u_r]\in\mathbb R^{m\times r}$,

其中各列线性无关。

第二步：对 $A_r$ 的列进行 Gram–Schmidt 正交化。

令

$A_r^*=[u_1^*,\ldots,u_r^*]$.

由推论 8.16，

$A_r=A_r^*\begin{pmatrix}b_{11}&b_{21}&\cdots&b_{r1}\\0&b_{22}&\cdots&b_{r2}\\\vdots&\ddots&\ddots&\vdots\\0&\cdots&0&b_{rr}\end{pmatrix}$ (8.49)

其中 $b_{kk}>0$，且 $(A_r^*)^\top A_r^*=I_r$。

第三步：将未被选中的列插回原位置。

对于位于首个非零列之前的零列，有

$[\xi_1,\ldots,\xi_{k_1-1},u_1,\ldots,u_r]=A_r^*[0,\ldots,0,R_r]$ (8.50)

其中 $R_r$ 为 (8.49) 中的上三角矩阵。

对于位于第 $j-1$ 个和第 $j$ 个线性无关列之间的列 $\xi_\ell$，根据选取规则，有

$\xi_\ell\in\operatorname{span}\{u_1,\ldots,u_{j-1}\}$.

因此可表示为

$\xi_\ell=\sum_{i=1}^{j-1}c_{\ell i}u_i^*$.

相应的分解为

$[u_1,\ldots,u_{j-1},\xi_\ell]=[u_1^*,\ldots,u_{j-1}^*]\begin{pmatrix}b_{11}&\cdots&b_{j-1,1}&c_{\ell1}\\&\ddots&\vdots&\vdots\\&&b_{j-1,j-1}&c_{\ell,j-1}\end{pmatrix}$ (8.51)

最后一个线性无关列之后的列也可表示为全部 $r$ 个标准正交向量的线性组合。

因此，将所有列按原顺序插回后，仍可保持上梯形结构：

$A=A_r^*\widetilde R$,

其中 $\widetilde R\in\mathbb R^{r\times n}$。

第四步：补全标准正交基。

若 $r=m$，则直接取 $Q=A_r^*$。

若 $r<m$，将 $u_1^*,\ldots,u_r^*$ 扩充为 $\mathbb R^m$ 的标准正交基，构造

$Q=[u_1^*,\ldots,u_m^*]$,

并令

$R=\begin{pmatrix}\widetilde R\\0\end{pmatrix}$.

于是 $Q^\top Q=I$，且 $A=QR$。

**※【引理 8.51】** 正交矩阵保持向量的二范数：

若 $Q$ 为正交矩阵，则对任意适当维数的向量 $\mathbf x$，有

$\|Q\mathbf x\|_2=\|\mathbf x\|_2$.

**证明**

$\|Q\mathbf x\|_2^2=\mathbf x^\top Q^\top Q\mathbf x=\mathbf x^\top\mathbf x=\|\mathbf x\|_2^2$.

**※【定理 8.52】** 利用 QR 分解求解超定线性方程组的最小二乘问题：

考虑超定线性方程组(overdetermined linear system)

$A\mathbf x=\mathbf b$,

其中 $A\in\mathbb R^{m\times n}$，$m\geq n$。

当 $A$ 列满秩时，考虑最小二乘问题

$\min_{\mathbf x\in\mathbb R^n}\|A\mathbf x-\mathbf b\|_2^2$.

对 $A$ 作 QR 分解，并将 $R$ 和 $Q^\top\mathbf b$ 分块：

$Q^\top A=R=\begin{pmatrix}R_1\\0\end{pmatrix},\qquad Q^\top\mathbf b=\begin{pmatrix}\mathbf c\\\mathbf r\end{pmatrix}$ (8.53)

其中 $R_1\in\mathbb R^{n\times n}$ 为非奇异上三角矩阵，$\mathbf c\in\mathbb R^n$，$\mathbf r\in\mathbb R^{m-n}$。

则唯一最小二乘解 $\mathbf x^*$ 由

$R_1\mathbf x^*=\mathbf c$ (8.52)

确定，且最小残差平方为

$\min_{\mathbf x}\|A\mathbf x-\mathbf b\|_2^2=\|\mathbf r\|_2^2$.

**证明**

由正交矩阵保持二范数的性质，

$\begin{aligned}\|A\mathbf x-\mathbf b\|_2^2&=\|Q^\top(A\mathbf x-\mathbf b)\|_2^2\\&=\left\|\begin{pmatrix}R_1\\0\end{pmatrix}\mathbf x-\begin{pmatrix}\mathbf c\\\mathbf r\end{pmatrix}\right\|_2^2\\&=\|R_1\mathbf x-\mathbf c\|_2^2+\|\mathbf r\|_2^2.\end{aligned}$

第二项与 $\mathbf x$ 无关。由于 $A$ 列满秩，$R_1$ 非奇异，因此存在唯一的 $\mathbf x^*$ 使第一项为零，即

$R_1\mathbf x^*=\mathbf c$.

此时最小残差平方为 $\|\mathbf r\|_2^2$。

注意：原文定理未明确给出列满秩条件。若 $A$ 秩亏，则 $R_1$ 可能奇异，$R_1\mathbf x=\mathbf c$ 未必有解，不能直接断言最小残差平方为 $\|\mathbf r\|_2^2$。此处补充列满秩条件，以保证原结论成立。

**※【性质】** 正规方程与 QR 分解的数值稳定性比较（补充说明）：

对于列满秩矩阵 $A$，正规方程的系数矩阵为 $G=A^\top A$，满足

$\operatorname{cond}_2(G)=\operatorname{cond}_2(A)^2$.

而在 QR 分解中，

$A^\top A=R_1^\top R_1$,

因此 $A$ 与 $R_1$ 具有相同的奇异值，从而

$\operatorname{cond}_2(R_1)=\operatorname{cond}_2(A)$.

于是

$\operatorname{cond}_2(G)=\operatorname{cond}_2(R_1)^2$.

正规方程会将原矩阵的二范数条件数平方，因此当 $A$ 接近秩亏时，直接构造 $A^\top A$ 可能显著放大数值误差。QR 分解避免了这一问题。

## §8.4 病态线性方程组的求解(Solving Ill-posed Linear Systems)

**※【定义 8.53】** 适定问题(well-posed problem)与不适定问题(ill-posed problem)：

按照本章的数值分析表述，若一个数学物理问题满足以下三个条件，则称其为适定问题：

1. 解存在(existence)；
2. 解唯一(uniqueness)；
3. 问题具有较小的条件数，即解对输入扰动具有良好的稳定性(stability)。

若上述条件不能同时满足，则称其为不适定问题。

需要区分：严格的 Hadamard 适定性要求解连续依赖于输入数据，并不要求条件数一定很小。条件数很大但有限的问题可以是适定的，同时也是病态的(ill-conditioned)。本章将这类数值稳定性较差的问题一并纳入讨论。

### §8.4.1 无解或多解的线性方程组(No Solutions or Multiple Solutions)

以下采用原讲义的奇异系统(singular system)记号。

设 $A\in\mathbb C^{m\times n}$，$\operatorname{rank}(A)=r$，其非零奇异值满足

$\sigma_1\geq\sigma_2\geq\cdots\geq\sigma_r>0$.

记 $\mathbf u_j\in\mathbb C^n$ 为右奇异向量，$\mathbf v_j\in\mathbb C^m$ 为左奇异向量，则

$A\mathbf u_j=\sigma_j\mathbf v_j,\qquad A^*\mathbf v_j=\sigma_j\mathbf u_j$,

其中 $A^*$ 表示共轭转置。

相应地，

$\operatorname{range}(A)=\operatorname{span}\{\mathbf v_1,\ldots,\mathbf v_r\}$,

$\operatorname{null}(A)=\operatorname{span}\{\mathbf u_{r+1},\ldots,\mathbf u_n\}$.

原讲义公式 (C.93) 中的特解为

$\mathbf x^\oplus=\sum_{j=1}^r\frac1{\sigma_j}\langle\mathbf b,\mathbf v_j\rangle\mathbf u_j$.

注意：这里 $\mathbf u_j$ 为右奇异向量，$\mathbf v_j$ 为左奇异向量，与常见的 $A=U\Sigma V^*$ 记号中的字母对应关系相反。

**※【定理 8.54】** 无解线性方程组的最小残差解：

若线性方程组

$A\mathbf x=\mathbf b$

无解，则上述 $\mathbf x^\oplus$ 是残差二范数最小化问题的一个解：

$\mathbf x^\oplus\in\arg\min_{\mathbf x\in\mathbb C^n}\|A\mathbf x-\mathbf b\|_2$ (8.54)

**证明**

将 $\mathbf b$ 关于左奇异向量展开：

$\mathbf b=\sum_{j=1}^m\langle\mathbf b,\mathbf v_j\rangle\mathbf v_j$.

由 $\mathbf x^\oplus$ 的定义，

$A\mathbf x^\oplus=\sum_{j=1}^r\langle\mathbf b,\mathbf v_j\rangle\mathbf v_j$.

因此

$A\mathbf x^\oplus-\mathbf b=-\sum_{j=r+1}^m\langle\mathbf b,\mathbf v_j\rangle\mathbf v_j$,

即

$A\mathbf x^\oplus-\mathbf b\in\operatorname{span}\{\mathbf v_{r+1},\ldots,\mathbf v_m\}$.

另一方面，对任意 $\mathbf x\in\mathbb C^n$，

$A\mathbf x-A\mathbf x^\oplus\in\operatorname{range}(A)=\operatorname{span}\{\mathbf v_1,\ldots,\mathbf v_r\}$.

上述两个向量相互正交。由勾股定理，

$\|A\mathbf x-\mathbf b\|_2^2=\|A\mathbf x-A\mathbf x^\oplus\|_2^2+\|A\mathbf x^\oplus-\mathbf b\|_2^2$.

因此

$\|A\mathbf x-\mathbf b\|_2^2\geq\|A\mathbf x^\oplus-\mathbf b\|_2^2$.

故 $\mathbf x^\oplus$ 为最小残差解。

**※【定理 8.55】** 多解线性方程组的最小范数解：

若线性方程组 $A\mathbf x=\mathbf b$ 有多个解，则其通解为

$\mathbf x=\mathbf x^\oplus+\mathbf y,\qquad\mathbf y\in\operatorname{null}(A)$.

其中 $\mathbf x^\oplus$ 是所有解中唯一的最小二范数解(minimum-norm solution)。

**证明**

由奇异系统的性质，

$\mathbf x^\oplus\in\operatorname{span}\{\mathbf u_1,\ldots,\mathbf u_r\}$,

$\operatorname{null}(A)=\operatorname{span}\{\mathbf u_{r+1},\ldots,\mathbf u_n\}$.

因此对任意 $\mathbf y\in\operatorname{null}(A)$，有

$\langle\mathbf x^\oplus,\mathbf y\rangle=0$.

由勾股定理，

$\|\mathbf x^\oplus+\mathbf y\|_2^2=\|\mathbf x^\oplus\|_2^2+\|\mathbf y\|_2^2$.

该式当且仅当 $\mathbf y=0$ 时取得最小值。

因此 $\mathbf x^\oplus$ 是唯一的最小范数解。

### §8.4.2 Moore–Penrose 广义逆(The Moore–Penrose Inverse)

**※【定义 8.56】** Moore–Penrose 广义逆(Moore–Penrose inverse)或伪逆(pseudoinverse)：

设 $A\in\mathbb F^{m\times n}$，其中 $\mathbb F=\mathbb R$ 或 $\mathbb C$，且 $\operatorname{rank}(A)=r$。

定义广义逆 $A^+\in\mathbb F^{n\times m}$，使得对任意 $\mathbf y\in\mathbb F^m$，有

$A^+\mathbf y=\sum_{j=1}^r\frac1{\sigma_j}\langle\mathbf y,\mathbf v_j\rangle\mathbf u_j$ (8.55)

其中 $(\sigma_j,\mathbf u_j,\mathbf v_j)$ 为 $A$ 的奇异系统。

写成矩阵形式：

$A^+=\sum_{j=1}^r\frac1{\sigma_j}\mathbf u_j\mathbf v_j^*$.

因此，前述特解可以统一表示为

$\mathbf x^\oplus=A^+\mathbf b$.

当方程组无解时，它给出最小残差解；当方程组有多个解时，它给出最小范数解。

更一般地，$A^+\mathbf b$ 是所有最小二乘解中唯一的最小二范数解。

**※【定理 8.57】** Moore–Penrose 广义逆的基本性质：

广义逆满足以下四个条件，亦称 Penrose 条件(Penrose conditions)：

(PDI-1) $AA^+A=A$。

(PDI-2) $A^+AA^+=A^+$。

(PDI-3) $(AA^+)^*=AA^+$。

(PDI-4) $(A^+A)^*=A^+A$。

其中原文将后两个 Hermitian 条件合并为同一项。

**※【引理 8.58】** 满秩矩阵的广义逆公式：

若 $A$ 的列向量线性无关，即 $\operatorname{rank}(A)=n$，则 $A^*A$ 可逆，且

$A^+=(A^*A)^{-1}A^*$ (8.56)

此时 $A^+$ 为 $A$ 的左逆：

$A^+A=I_n$.

若 $A$ 的行向量线性无关，即 $\operatorname{rank}(A)=m$，则 $AA^*$ 可逆，且

$A^+=A^*(AA^*)^{-1}$ (8.57)

此时 $A^+$ 为 $A$ 的右逆：

$AA^+=I_m$.

特别地，若 $A$ 为非奇异方阵，则 $A^+=A^{-1}$。

### §8.4.3 谱截断法(Spectral Cutoff)

**※【例题 8.59】** 小奇异值对线性方程组稳定性的影响：

考虑线性方程组

$A\mathbf x=\mathbf b$.

若右端项沿第 $j$ 个左奇异向量方向受到扰动：

$\hat{\mathbf b}=\mathbf b+\epsilon\mathbf v_j$,

其中 $\epsilon\in\mathbb C$，$1\leq j\leq r$，则由奇异系统展开可得

$\hat{\mathbf x}=\mathbf x+\frac{\epsilon}{\sigma_j}\mathbf u_j$.

这里比较的是采用相同广义逆求解规则得到的解。

由于奇异向量的二范数均为 $1$，有

$\frac{\|\hat{\mathbf x}-\mathbf x\|_2}{\|\hat{\mathbf b}-\mathbf b\|_2}=\frac1{\sigma_j}$.

因此，奇异值越小，相应方向上的数据扰动被放大的程度越大。

线性方程组的病态性主要来源于小奇异值。对于非奇异方阵，其绝对条件数为

$\|A^{-1}\|_2=\frac1{\sigma_{\min}}$,

相对二范数条件数为

$\operatorname{cond}_2(A)=\frac{\sigma_{\max}}{\sigma_{\min}}$.

**※【定义 8.60】** 谱截断法：

在奇异系统展开

$\mathbf x^\oplus=\sum_{j=1}^r\frac1{\sigma_j}\langle\mathbf b,\mathbf v_j\rangle\mathbf u_j$

中，忽略与较小奇异值对应的项，以降低输入扰动对解的影响。

该方法也称为截断奇异值分解(truncated singular value decomposition, TSVD)。

**※【定义 8.61】** 偏差原则(discrepancy principle)：

考虑线性方程组 $A\mathbf x=\mathbf b$，实际可观测的右端项为受到扰动的 $\hat{\mathbf b}$。

偏差原则要求构造近似解 $\hat{\mathbf x}$，使其残差满足

$\|A\hat{\mathbf x}-\hat{\mathbf b}\|_2=C\|\mathbf b-\hat{\mathbf b}\|_2$ (8.58)

其中 $C\geq1$ 为适当选取的常数。

其基本思想是：当数据本身含有噪声时，不应要求模型残差远小于数据噪声水平，否则可能将噪声也拟合进解中。

**※【例题 8.62】** 基于偏差原则的谱截断：

设奇异值按降序排列，定义截断解

$\mathbf x_p=\begin{cases}0,&p=0,\\\sum_{j=1}^p\frac1{\sigma_j}\langle\hat{\mathbf b},\mathbf v_j\rangle\mathbf u_j,&p=1,\ldots,r.\end{cases}$ (8.59)

根据数据扰动大小 $\|\hat{\mathbf b}-\mathbf b\|_2$ 选择截断阶数 $p$，使残差达到与噪声水平相当的大小。

原文给出的常用选择为 $C=1$。

由于 $p$ 为整数，残差只能取离散的数值，通常无法严格满足 (8.58) 的等式，因此实际采用满足残差阈值的最小截断阶数。

**※【定理 8.63】** 谱截断偏差原则的存在性与收敛性：

设线性方程组 $A\mathbf x=\mathbf b$ 可解，其中 $A\in\mathbb C^{m\times n}$，且右端扰动满足

$\|\hat{\mathbf b}-\mathbf b\|_2\leq\epsilon\leq\|\hat{\mathbf b}\|_2$ (8.60)

则存在最小整数 $p=p(\epsilon)\in\{0,\ldots,r\}$，使得

$\|A\mathbf x_p-\hat{\mathbf b}\|_2\leq\epsilon$ (8.61)

其中 $\mathbf x_p$ 由 (8.59) 定义。

此外，当 $\epsilon\to0$ 时，有

$\lim_{\epsilon\to0}\mathbf x_{p(\epsilon)}=A^+\mathbf b$ (8.62)

**证明**

第一步：证明满足偏差原则的截断阶数存在。

定义

$f(p)=\|A\mathbf x_p-\hat{\mathbf b}\|_2^2-\epsilon^2$.

由奇异系统展开，

$A\mathbf x_p=\sum_{j=1}^p\langle\hat{\mathbf b},\mathbf v_j\rangle\mathbf v_j$.

因此

$f(p)=\sum_{j=p+1}^m|\langle\hat{\mathbf b},\mathbf v_j\rangle|^2-\epsilon^2$ (8.63)

该函数关于整数 $p$ 单调不增。

当 $p=0$ 时，

$f(0)=\|\hat{\mathbf b}\|_2^2-\epsilon^2\geq0$.

当 $p=r$ 时，若 $r=m$，则 $f(r)=-\epsilon^2\leq0$。

若 $r<m$，由于原方程组可解，$\mathbf b\in\operatorname{range}(A)$，故

$\langle\mathbf b,\mathbf v_j\rangle=0,\quad j=r+1,\ldots,m$.

于是

$\begin{aligned}f(r)&=\sum_{j=r+1}^m|\langle\hat{\mathbf b}-\mathbf b,\mathbf v_j\rangle|^2-\epsilon^2\\&\leq\|\hat{\mathbf b}-\mathbf b\|_2^2-\epsilon^2\\&\leq0.\end{aligned}$

因此必存在满足 $f(p)\leq0$ 的最小整数 $p$。

第二步：证明收敛性。

由三角不等式，

$\begin{aligned}\|A\mathbf x_p-\mathbf b\|_2&\leq\|A\mathbf x_p-\hat{\mathbf b}\|_2+\|\hat{\mathbf b}-\mathbf b\|_2\\&\leq2\epsilon.\end{aligned}$

由于

$\mathbf x_p\in\operatorname{span}\{\mathbf u_1,\ldots,\mathbf u_r\}$,

且 $A^+A$ 在该子空间上为恒等映射，故

$\mathbf x_p-A^+\mathbf b=A^+(A\mathbf x_p-\mathbf b)$.

因此

$\|\mathbf x_p-A^+\mathbf b\|_2\leq2\|A^+\|_2\epsilon\to0$.

故 $\mathbf x_p\to A^+\mathbf b$。

### §8.4.4 Tikhonov 正则化(Tikhonov Regularization)

**※【定义 8.64】** Tikhonov 正则化：

对于病态线性方程组

$A\mathbf x=\mathbf b$,

通过求解以下正则化方程组来近似原问题的解：

$\alpha\mathbf x+A^*A\mathbf x=A^*\mathbf b$ (8.64)

即

$(\alpha I+A^*A)\mathbf x=A^*\mathbf b$,

其中 $\alpha>0$ 称为正则化参数(regularization parameter)。

**※【定理 8.65】** Tikhonov 正则化解的奇异系统表达式及收敛性：

设 $A\in\mathbb C^{m\times n}$，$\operatorname{rank}(A)=r$。对于任意 $\mathbf b\in\mathbb C^m$ 和 $\alpha>0$，正则化方程 (8.64) 存在唯一解

$\mathbf x_\alpha=\sum_{j=1}^r\frac{\sigma_j}{\alpha+\sigma_j^2}\langle\mathbf b,\mathbf v_j\rangle\mathbf u_j$ (8.65)

其中 $(\sigma_j,\mathbf u_j,\mathbf v_j)$ 为 $A$ 的奇异系统。

此外，

$\lim_{\alpha\to0^+}(\alpha I+A^*A)^{-1}A^*\mathbf b=A^+\mathbf b$ (8.66)

**证明**

记

$B=\alpha I+A^*A$.

对于任意非零向量 $\mathbf z$，有

$\mathbf z^*B\mathbf z=\alpha\|\mathbf z\|_2^2+\|A\mathbf z\|_2^2>0$.

因此 $B$ 为 Hermitian 正定矩阵，从而非奇异，正则化方程存在唯一解。

根据奇异系统的性质，

$A^*A\mathbf u_j=\sigma_j^2\mathbf u_j$.

于是

$B\mathbf u_j=(\alpha+\sigma_j^2)\mathbf u_j,\quad j=1,\ldots,r$.

对于 $j>r$，有 $A\mathbf u_j=0$，因此

$B\mathbf u_j=\alpha\mathbf u_j$.

又因为

$\langle A^*\mathbf b,\mathbf u_j\rangle=\langle\mathbf b,A\mathbf u_j\rangle$,

当 $j>r$ 时该内积为零。

因此

$\begin{aligned}\mathbf x_\alpha&=B^{-1}A^*\mathbf b\\&=\sum_{j=1}^r\frac1{\alpha+\sigma_j^2}\langle A^*\mathbf b,\mathbf u_j\rangle\mathbf u_j\\&=\sum_{j=1}^r\frac1{\alpha+\sigma_j^2}\langle\mathbf b,A\mathbf u_j\rangle\mathbf u_j\\&=\sum_{j=1}^r\frac{\sigma_j}{\alpha+\sigma_j^2}\langle\mathbf b,\mathbf v_j\rangle\mathbf u_j.\end{aligned}$

当 $\alpha\to0^+$ 时，

$\frac{\sigma_j}{\alpha+\sigma_j^2}\to\frac1{\sigma_j}$.

因此

$\lim_{\alpha\to0^+}\mathbf x_\alpha=\sum_{j=1}^r\frac1{\sigma_j}\langle\mathbf b,\mathbf v_j\rangle\mathbf u_j=A^+\mathbf b$.

**※【性质】** Tikhonov 正则化的谱滤波机制（补充说明）：

广义逆解为

$\mathbf x^\oplus=\sum_{j=1}^r\frac1{\sigma_j}\langle\mathbf b,\mathbf v_j\rangle\mathbf u_j$.

Tikhonov 正则化解可以改写为

$\mathbf x_\alpha=\sum_{j=1}^r\frac{\sigma_j^2}{\alpha+\sigma_j^2}\frac1{\sigma_j}\langle\mathbf b,\mathbf v_j\rangle\mathbf u_j$.

其中

$g_\alpha(\sigma_j)=\frac{\sigma_j^2}{\alpha+\sigma_j^2}$

称为滤波因子(filter factor)。

当 $\sigma_j^2\gg\alpha$ 时，$g_\alpha(\sigma_j)\approx1$，相应分量基本保留。

当 $\sigma_j^2\ll\alpha$ 时，$g_\alpha(\sigma_j)\approx0$，相应分量受到抑制。

因此谱截断法直接删除小奇异值对应的分量，而 Tikhonov 正则化连续地衰减这些分量。

**※【定理 8.66】** Tikhonov 正则化的最优化解释：

对于任意 $\mathbf b\in\mathbb C^m$，Tikhonov 正则化方程

$(\alpha I+A^*A)\mathbf x_\alpha=A^*\mathbf b$

的唯一解，等价于以下优化问题的唯一极小点：

$\mathbf x_\alpha=\arg\min_{\mathbf x\in\mathbb C^n}\left\{\|A\mathbf x-\mathbf b\|_2^2+\alpha\|\mathbf x\|_2^2\right\}$ (8.67)

其中第一项为残差平方，第二项为正则化惩罚项。

**证明**

定义目标函数

$J(\mathbf x)=\|A\mathbf x-\mathbf b\|_2^2+\alpha\|\mathbf x\|_2^2$,

并令

$\mathbf z_\alpha=\alpha\mathbf x_\alpha+A^*A\mathbf x_\alpha-A^*\mathbf b$.

利用范数平方的展开公式，可得

$\begin{aligned}J(\mathbf x)&=J(\mathbf x_\alpha)+2\operatorname{Re}\langle\mathbf x-\mathbf x_\alpha,\mathbf z_\alpha\rangle\\&\quad+\|A(\mathbf x-\mathbf x_\alpha)\|_2^2+\alpha\|\mathbf x-\mathbf x_\alpha\|_2^2.\end{aligned}$ (8.68)

首先，若 $\mathbf x_\alpha$ 为正则化方程的解，则 $\mathbf z_\alpha=0$，因此

$J(\mathbf x)-J(\mathbf x_\alpha)=\|A(\mathbf x-\mathbf x_\alpha)\|_2^2+\alpha\|\mathbf x-\mathbf x_\alpha\|_2^2\geq0$.

由于 $\alpha>0$，等号当且仅当 $\mathbf x=\mathbf x_\alpha$ 时成立。

故 $\mathbf x_\alpha$ 为唯一极小点。

反之，假设 $\mathbf x_\alpha$ 为极小点，但 $\mathbf z_\alpha\neq0$。

构造

$\mathbf x=\mathbf x_\alpha-\epsilon\mathbf z_\alpha,\qquad\epsilon\in\mathbb R$.

代入 (8.68)，得到

$J(\mathbf x)=J(\mathbf x_\alpha)-2\epsilon a+\epsilon^2b$,

其中

$a=\|\mathbf z_\alpha\|_2^2>0$,

$b=\|A\mathbf z_\alpha\|_2^2+\alpha\|\mathbf z_\alpha\|_2^2>0$.

取 $\epsilon=a/b$，则

$J(\mathbf x)=J(\mathbf x_\alpha)-\frac{a^2}{b}<J(\mathbf x_\alpha)$,

与 $\mathbf x_\alpha$ 为极小点矛盾。

因此 $\mathbf z_\alpha=0$，即

$(\alpha I+A^*A)\mathbf x_\alpha=A^*\mathbf b$.

**※【例题 8.67】** Tikhonov 正则化中的误差分解与参数选择：

设原线性方程组的广义逆解为

$\mathbf x=A^+\mathbf b$.

实际观测到的右端项为 $\hat{\mathbf b}$，满足

$\|\hat{\mathbf b}-\mathbf b\|_2<\epsilon$.

令

$B=\alpha I+A^*A$,

则基于扰动数据得到的正则化解满足

$B\mathbf x_\alpha=A^*\hat{\mathbf b}$.

总误差可以分解为

$\begin{aligned}\mathbf x_\alpha-\mathbf x&=B^{-1}A^*\hat{\mathbf b}-A^+\mathbf b\\&=B^{-1}A^*(\hat{\mathbf b}-\mathbf b)+(B^{-1}A^*\mathbf b-A^+\mathbf b).\end{aligned}$

其中：

- 第一项为数据误差(data error)，来源于右端项的扰动；
- 第二项为正则化误差(regularization error)，来源于正则化对原问题的改变。

一方面，由定理 8.65，减小 $\alpha$ 可以降低正则化误差。

另一方面，$B$ 的二范数条件数为

$\operatorname{cond}_2(B)=\frac{\alpha+\sigma_1^2}{\alpha+\sigma_n^2}$,

其中若 $A$ 列秩不足，则将 $A^*A$ 的零特征值对应的奇异值补记为 $\sigma_j=0$。

增大 $\alpha$ 可以改善 $B$ 的条件数，降低数据扰动的影响。

进一步，由奇异系统表达式，

$\|B^{-1}A^*\|_2=\max_{1\leq j\leq r}\frac{\sigma_j}{\alpha+\sigma_j^2}$,

该量随 $\alpha$ 增大而减小。

因此，正则化参数的选取需要在逼近精度与数值稳定性之间进行权衡：

- $\alpha$ 过小：正则化误差较小，但数据噪声可能被显著放大；
- $\alpha$ 过大：数据扰动受到抑制，但正则化误差增大。

**※【定理 8.68】** Tikhonov 正则化的偏差原则及其收敛性：

设线性方程组 $A\mathbf x=\mathbf b$ 可解，其中 $A\in\mathbb C^{m\times n}$，右端扰动满足

$\|\hat{\mathbf b}-\mathbf b\|_2\leq\epsilon<\|\hat{\mathbf b}\|_2$ (8.69)

考虑正则化方程

$(\alpha I+A^*A)\mathbf x_\alpha=A^*\hat{\mathbf b}$ (8.71)

若进一步满足

$\|(I-AA^+)\hat{\mathbf b}\|_2<\epsilon$,

则存在唯一正则化参数 $\alpha=\alpha(\epsilon)>0$，使得

$\|A\mathbf x_\alpha-\hat{\mathbf b}\|_2=\epsilon$ (8.70)

此外，当 $\epsilon\to0$ 时，

$\lim_{\epsilon\to0}\mathbf x_{\alpha(\epsilon)}=A^+\mathbf b$ (8.72)

注意：原文仅给出 (8.69)，但其证明只能保证残差函数在 $\alpha\to0^+$ 时的极限不大于零。若该极限恰为零，则未必存在正的正则化参数满足 (8.70)。因此此处补充严格不等式条件，以保证存在性结论成立。

**证明**

第一步：证明满足偏差原则的正则化参数存在且唯一。

定义

$f(\alpha)=\|A\mathbf x_\alpha-\hat{\mathbf b}\|_2^2-\epsilon^2$.

由 (8.65)，

$\mathbf x_\alpha=\sum_{j=1}^r\frac{\sigma_j}{\alpha+\sigma_j^2}\langle\hat{\mathbf b},\mathbf v_j\rangle\mathbf u_j$.

因此

$A\mathbf x_\alpha=\sum_{j=1}^r\frac{\sigma_j^2}{\alpha+\sigma_j^2}\langle\hat{\mathbf b},\mathbf v_j\rangle\mathbf v_j$.

将 $\hat{\mathbf b}$ 关于左奇异向量展开，有

$A\mathbf x_\alpha-\hat{\mathbf b}=-\sum_{j=1}^r\frac{\alpha}{\alpha+\sigma_j^2}\langle\hat{\mathbf b},\mathbf v_j\rangle\mathbf v_j-\sum_{j=r+1}^m\langle\hat{\mathbf b},\mathbf v_j\rangle\mathbf v_j$.

由标准正交性，

$f(\alpha)=\sum_{j=1}^r\left|\frac{\alpha}{\alpha+\sigma_j^2}\langle\hat{\mathbf b},\mathbf v_j\rangle\right|^2+\sum_{j=r+1}^m|\langle\hat{\mathbf b},\mathbf v_j\rangle|^2-\epsilon^2$ (8.73)

由于

$\frac{\mathrm d}{\mathrm d\alpha}\frac{\alpha}{\alpha+\sigma_j^2}=\frac{\sigma_j^2}{(\alpha+\sigma_j^2)^2}>0$,

且假设保证 $\hat{\mathbf b}$ 在 $\operatorname{range}(A)$ 上存在非零分量，故 $f(\alpha)$ 连续且严格单调递增。

当 $\alpha\to0^+$ 时，

$\lim_{\alpha\to0^+}f(\alpha)=\sum_{j=r+1}^m|\langle\hat{\mathbf b},\mathbf v_j\rangle|^2-\epsilon^2$.

由于原方程组可解，

$\langle\mathbf b,\mathbf v_j\rangle=0,\quad j=r+1,\ldots,m$.

故

$\begin{aligned}\lim_{\alpha\to0^+}f(\alpha)&=\sum_{j=r+1}^m|\langle\hat{\mathbf b}-\mathbf b,\mathbf v_j\rangle|^2-\epsilon^2\\&\leq\|\hat{\mathbf b}-\mathbf b\|_2^2-\epsilon^2\\&\leq0.\end{aligned}$

结合补充的严格不等式条件，可得

$\lim_{\alpha\to0^+}f(\alpha)<0$.

另一方面，

$\lim_{\alpha\to+\infty}f(\alpha)=\|\hat{\mathbf b}\|_2^2-\epsilon^2>0$.

因此由连续性与严格单调性，存在唯一 $\alpha>0$ 使得 $f(\alpha)=0$。

第二步：证明正则化解的收敛性。

由 (8.70) 和三角不等式，

$\|\hat{\mathbf b}\|_2-\epsilon\leq\|A\mathbf x_\alpha\|_2$.

由正则化方程，

$\alpha\mathbf x_\alpha=A^*(\hat{\mathbf b}-A\mathbf x_\alpha)$.

两端左乘 $A$，并取二范数：

$\begin{aligned}\alpha\|A\mathbf x_\alpha\|_2&=\|AA^*(\hat{\mathbf b}-A\mathbf x_\alpha)\|_2\\&\leq\|AA^*\|_2\epsilon.\end{aligned}$

又由 (8.69)，

$\|\hat{\mathbf b}\|_2\geq\|\mathbf b\|_2-\epsilon$.

当 $\mathbf b\neq0$ 且 $\epsilon$ 充分小时，综合以上不等式可得

$0<\alpha\leq\frac{\|AA^*\|_2\epsilon}{\|\mathbf b\|_2-2\epsilon}$.

因此 $\alpha\to0$。

结合 $\hat{\mathbf b}\to\mathbf b$ 和 (8.65)，有

$\begin{aligned}\lim_{\epsilon\to0}\mathbf x_{\alpha(\epsilon)}&=\lim_{\epsilon\to0}\sum_{j=1}^r\frac{\sigma_j}{\alpha(\epsilon)+\sigma_j^2}\langle\hat{\mathbf b},\mathbf v_j\rangle\mathbf u_j\\&=\sum_{j=1}^r\frac1{\sigma_j}\langle\mathbf b,\mathbf v_j\rangle\mathbf u_j\\&=A^+\mathbf b.\end{aligned}$

补充：原文通过证明 $\alpha\to0$ 得到收敛性，这一步依赖于 $\mathbf b\neq0$。在固定有限维矩阵的情形下，也可以直接利用残差界证明收敛，而不必要求 $\alpha\to0$：

$\|A\mathbf x_\alpha-\mathbf b\|_2\leq\|A\mathbf x_\alpha-\hat{\mathbf b}\|_2+\|\hat{\mathbf b}-\mathbf b\|_2\leq2\epsilon$.

由于 $\mathbf x_\alpha\in\operatorname{span}\{\mathbf u_1,\ldots,\mathbf u_r\}$，有 $A^+A\mathbf x_\alpha=\mathbf x_\alpha$，因此

$\|\mathbf x_\alpha-A^+\mathbf b\|_2\leq2\|A^+\|_2\epsilon\to0$.

该论证同样覆盖 $\mathbf b=0$ 的情形。
