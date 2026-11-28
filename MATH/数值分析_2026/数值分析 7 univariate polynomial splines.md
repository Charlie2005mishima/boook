
**※【定义】** 网格（mesh/grid）、样条函数（spline function）、节点（knots）与网格大小

给定 $a=x_1<x_2<\cdots<x_N=b$，记分割 $\Delta=\{x_1,\ldots,x_N\}$，区间长度
$$
h_i:=x_{i+1}-x_i>0,\qquad 1\le i\le N-1,
$$
全局**网格大小**（mesh size）为 $h:=\max_{1\le i<N}h_i$。
给定多项式次数 $n$ 与光滑阶数 $k$，定义
$$
\mathbb S_n^k(\Delta)
=\bigl\{s\in C^k[a,b]:\ s|_{[x_i,x_{i+1}]}\in\mathbb P_n,\quad i=1,\ldots,N-1\bigr\}.\tag{7.1–7.2}
$$

其中 $\mathbb P_n$ 为次数**不超过** $n$ 的多项式集合；内部节点与两端节点共同构成分割。若无歧义，简写为 $\mathbb S_n^k$。

**※【例题】** 分段插值与光滑度

1. **$k=0$，分段 Lagrange 插值。** 
2. **$k=1,n=3$，分段三次 Hermite 插值。** 
3. **$k=2,n=3$，三次样条。** 若试图在**单个区间**同时规定两端的函数值、一阶导数和二阶导数，共有 $6$ 个条件，而 $\dim\mathbb P_3=4$，一般超定。因此不能依靠逐段独立地施加这 $6$ 个 Hermite 条件来直接构造 $C^2$ 三次样条，需要通过**全局连续性条件与边界条件**确定节点导数。

**※【推论】** 若 $k\ge n$，则
$$
\mathbb S_n^k(\Delta)=\mathbb P_n.
$$
例如 $\mathbb S_3^3=\mathbb P_3$、$\mathbb S_1^1=\mathbb P_1$；这是因为相邻两段次数不超过 $n$ 的多项式在交点处的 $0,1,\ldots,n$ 阶导数完全相同，故两段必为同一多项式。

**※【定义】** 最高光滑度样条
$\mathbb S_n^{n-1}(\Delta)$ 称为最高光滑度样条(maximally smooth spline)，即在保持“分段次数为 $n$”且一般不退化为同一个整体多项式的前提下，达到的最高连续阶数。
最常用的情形是 $\mathbb S_3^2$（三次样条）。

## 7.1 PP 形式样条（PP-form splines）

### 7.1.1 构造 PP 形式三次样条

#### （一）节点一阶导数 $m_i$ 路线

**※【引理】** 设 $m_i:=s'(x_i)$，记
$$
\mu_i=\frac{x_i-x_{i-1}}{x_{i+1}-x_{i-1}},\qquad
\lambda_i=\frac{x_{i+1}-x_i}{x_{i+1}-x_{i-1}},\qquad
\mu_i+\lambda_i=1.
\tag{7.4}
$$

则 $i=2,\ldots,N-1$ 时有
$$
\lambda_i m_{i-1}+2m_i+\mu_i m_{i+1}
=3\mu_i f[x_i,x_{i+1}]+3\lambda_i f[x_{i-1},x_i].\tag{7.3}
$$

**证明** 
对第 $i$ 段令 $K_i=f[x_i,x_{i+1}]$。
满足端点函数值及一阶导数条件的三次 Hermite 多项式为
$$
\begin{aligned}
p_i(x)={}&f_i+(x-x_i)m_i+\frac{K_i-m_i}{h_i}(x-x_i)^2\\
&+\frac{m_i+m_{i+1}-2K_i}{h_i^2}(x-x_i)^2(x-x_{i+1}).
\end{aligned}\tag{7.5}
$$

将其写为 $p_i(x)=\sum_{j=0}^3c_{i,j}(x-x_i)^j$，则内部节点二阶导数连续意味着
$$
3c_{i-1,3}h_{i-1}=c_{i,2}-c_{i-1,2}.\tag{7.6}
$$

代入系数并整理，得到 (7.3)。$\square$

#### （二）以 $M_i=s''(x_i)$ 构造分段三次多项式

**※【推导】** 
$s''$ 在每个小区间上为线性函数。
令 $s_i=s|_{[x_i,x_{i+1}]}$，则
$$
s_i''(x)=M_i\frac{x_{i+1}-x}{h_i}+M_{i+1}\frac{x-x_i}{h_i},
\qquad x\in[x_i,x_{i+1}].
$$

积分两次，可写成
$$
s_i(x)=M_i\frac{(x_{i+1}-x)^3}{6h_i}
+M_{i+1}\frac{(x-x_i)^3}{6h_i}+p_i(x),
\quad p_i\in\mathbb P_1.
$$

由 $s_i(x_i)=f_i$ 和 $s_i(x_{i+1})=f_{i+1}$，有
$$
p_i(x_i)=f_i-\frac{M_i h_i^2}{6},\qquad
p_i(x_{i+1})=f_{i+1}-\frac{M_{i+1}h_i^2}{6}.
$$

$\,p_i$ 为连接上述两个端点值的**线性插值多项式**，
$$
\begin{aligned}
s_i(x)={}&\frac{M_i(x_{i+1}-x)^3}{6h_i}
+\frac{M_{i+1}(x-x_i)^3}{6h_i}\\
&+\left(f_i-\frac{M_i h_i^2}{6}\right)\frac{x_{i+1}-x}{h_i}
+\left(f_{i+1}-\frac{M_{i+1}h_i^2}{6}\right)\frac{x-x_i}{h_i}.
\end{aligned}
$$

这个公式保证 $s\in C^0[a,b]$ 且 $s''\in C^0[a,b]$（因为所有分段在节点处具有统一的 $M_i$）；剩下只需满足**一阶导数连续**。

**※【推导 2】** 导出各段端点的一阶导数（手写第 5–6 页）。令
$$
d_i:=\frac{f_{i+1}-f_i}{h_i}=f[x_i,x_{i+1}].
$$

对 (PP-M) 求导并在两端取值，得到
$$
s_i'(x_i^+)=d_i-\frac{h_i}{6}(2M_i+M_{i+1}),
$$
$$
s_i'(x_{i+1}^-)=d_i+\frac{h_i}{6}(M_i+2M_{i+1}).
$$

**※【推导 3】** 由 $s_{i-1}'(x_i^-)=s_i'(x_i^+)$，对 $i=2,\ldots,N-1$ 有

$$
d_{i-1}+\frac{h_{i-1}}{6}(M_{i-1}+2M_i)
=d_i-\frac{h_i}{6}(2M_i+M_{i+1}).
$$

移项得**未归一化的内部节点方程**：
$$
h_{i-1}M_{i-1}+2(h_{i-1}+h_i)M_i+h_iM_{i+1}
=6(d_i-d_{i-1}).
\tag{A}
$$
令
$$
\mu_i:=\frac{h_{i-1}}{h_{i-1}+h_i},\qquad
\nu_i:=\frac{h_i}{h_{i-1}+h_i},\qquad \mu_i+\nu_i=1.
$$

则 (A) 等价于
$$
\mu_iM_{i-1}+2M_i+\nu_iM_{i+1}=r_i,\,
r_i:=\frac{6(d_i-d_{i-1})}{h_{i-1}+h_i}
=6f[x_{i-1},x_i,x_{i+1}].
\tag{7.7}
$$

#### （三）边界条件：使方程组闭合

仅由 $N-2$ 个内部节点方程，无法确定全部 $N$ 个未知量 $M_1,\ldots,M_N$，还需要 **2 条边界条件**。

**※【定义】** 老师板书着重讨论的两类（手写第 7、9 页）：

- **夹持边界条件（clamped boundary condition）：** 给定 $s'(a)=v_a$、$s'(b)=v_b$。若 $v_a=f'(a)$、$v_b=f'(b)$，即为**完备三次样条（complete cubic spline）**。
- **自然边界条件（natural boundary condition）：** $s''(a)=M_1=0$，$s''(b)=M_N=0$。

**※【推导 4：夹持边界方程】** 用前述端点一阶导数公式，有
$$
\begin{aligned}
s_1'(x_1^+)=d_1-\frac{h_1}{6}(2M_1+M_2)&=v_a,\\
s_{N-1}'(x_N^-)=d_{N-1}+\frac{h_{N-1}}{6}(M_{N-1}+2M_N)&=v_b.
\end{aligned}
$$

整理得
$$
2M_1+M_2=\frac{6(d_1-v_a)}{h_1}=:r_1,
$$
$$
M_{N-1}+2M_N=\frac{6(v_b-d_{N-1})}{h_{N-1}}=:r_N.
$$

若恰好 $v_a=f'(x_1)$、$v_b=f'(x_N)$，等价于原笔记中的重复节点差商公式
$$
2M_1+M_2=6f[x_1,x_1,x_2],\tag{7.12}
$$
$$
M_{N-1}+2M_N=6f[x_{N-1},x_N,x_N].\tag{7.13}
$$

**※【推导 5：自然边界方程】** 已知 $M_1=M_N=0$，仅需用 (7.7) 求解 $M_2,\ldots,M_{N-1}$，解出后再将两个零端点加入即可。

其他常见边界条件还有：
- 端点指定二阶导数 $s''(a)=f''(a)$、$s''(b)=f''(b)$；
- 非扭结（not-a-knot，要求 $s'''$ 在 $x_2$ 和 $x_{N-1}$ 连接）；
- 周期样条（要求端点的值、一阶导、二阶导相容）。

#### （四）线性方程组与唯一性

**※【定理】** 对于夹持边界条件，未知数 $\mathbf M=(M_1,\ldots,M_N)^\mathsf T$ 满足
$$
\underbrace{\begin{pmatrix}
2&1&0&\cdots&0\\
\mu_2&2&\nu_2&\ddots&\vdots\\
0&\mu_3&2&\ddots&0\\
\vdots&\ddots&\ddots&\ddots&\nu_{N-1}\\
0&\cdots&0&1&2
\end{pmatrix}}_{A\in\mathbb R^{N\times N}}
\underbrace{\begin{pmatrix}M_1\\M_2\\\vdots\\M_{N-1}\\M_N\end{pmatrix}}_{\mathbf M}
=
\underbrace{\begin{pmatrix}r_1\\r_2\\\vdots\\r_{N-1}\\r_N\end{pmatrix}}_{\mathbf r}.
\tag{7.15}
$$

其中上式的省略号表示**三对角矩阵**，即内部第 $i$ 行只有 $\mu_i,2,\nu_i$ 三个可能非零的元素。对于自然边界条件，消去 $M_1=M_N=0$ 后，得到关于 $M_2,\ldots,M_{N-1}$ 的 $(N-2)\times(N-2)$ 三对角方程组。

**※【定义】** 严格对角占优
矩阵 $A$ 称为（行）严格对角占优，若 $|a_{ii}|>\sum_{j\ne i}|a_{ij}|$ 对每一行均成立.

**※【定理】** 严格对角占优矩阵非奇异。

**证明** 
假设 $A\mathbf z=0$，存在非零 $\mathbf z$。取 $|z_k|=\|\mathbf z\|_\infty>0$，则

$$
|a_{kk}|\,|z_k|
=\left|\sum_{j\ne k}a_{kj}z_j\right|
\le\sum_{j\ne k}|a_{kj}|\,|z_j|
\le\left(\sum_{j\ne k}|a_{kj}|\right)|z_k|,
$$

与严格对角占优矛盾，故 $A$ 可逆。$\square$

**※【结论】** 由于 $\mu_i,\nu_i>0$ 且 $\mu_i+\nu_i=1$，内部各行 $2>\mu_i+\nu_i=1$，而夹持边界两行均满足 $2>1$，故 $A$ 严格对角占优，解 $\mathbf M$ 唯一。由 (PP-M) 各段的构造也唯一，因而**给定节点值及两端一阶导数后，存在唯一的夹持三次样条**。自然三次样条的缩减系统同样严格对角占优，因而也存在唯一解。

**※【定理：条件数】** 对上述规范化三对角系统，可得
$$
\kappa_\infty(A)=\|A\|_\infty\|A^{-1}\|_\infty\le 3.
$$

**证明** 
夹持系统每一行绝对值之和均为 $3$，故 $\|A\|_\infty=3$。此外，对任意 $A\mathbf z=\mathbf y$，取 $|z_k|=\|\mathbf z\|_\infty$，有
$$
\|\mathbf y\|_\infty\ge |y_k|
\ge\left(|a_{kk}|-\sum_{j\ne k}|a_{kj}|\right)\|\mathbf z\|_\infty
\ge\|\mathbf z\|_\infty.
$$

故 $\|A^{-1}\|_\infty\le1$，结论成立。对于自然边界的缩减系统，$\|A\|_\infty\le3$ 且对角占优裕量不小于 $1$，同样有上述界。$\square$


**※【例题】** 已知 $f(x)=\ln x$ 在 $x_1=1,x_2=2,x_3=3,x_4=4,x_5=6$ 的值及端点一阶导数，构造完备三次样条并估算 $\ln5$。求出 $M_i$ 后，代入 $[4,6]$ 上的 (PP-M)，有 $s(5)\approx1.60977$（$\ln 5\approx1.60944$）。

### 7.1.2 极小性质

**※【定理】** 极小弯曲能量(Minimum bending energy)：
对于任意满足 $g'(a)=f'(a), g'(b)=f'(b)$ 且 $g(x_i)=f(x_i)$ 的函数 $g \in \mathcal{C}^2[a, b]$，
完备三次样条 $s = s(f; x)$ 满足：$\int_a^b [s''(x)]^2 \mathrm{d}x \le \int_a^b [g''(x)]^2 \mathrm{d}x$ (7.16)
当且仅当 $g(x) = s(f; x)$ 时等号成立。

**证明**：令 $\eta(x) = g(x) - s(x)$，则 $\eta \in \mathcal{C}^2[a,b]$，且 $\eta'(a)=\eta'(b)=0$，$\eta(x_i)=0$。将 $g'' = s'' + \eta''$ 代入积分，展开为三项。通过分部积分，交叉项 $\int s''\eta''$ 可以化为边界项与内部节点的求和，结合 $s^{(4)} \equiv 0$（由于是三次样条）及边界/节点条件，该交叉项为零，从而不等式成立。$\square$

**※【定理】** 极小弯曲能量（自然三次样条）：对于任意满足 $g(x_i)=f(x_i)$ 的函数 $g \in \mathcal{C}^2[a, b]$，自然三次样条 $s = s(f; x)$ 满足同样形式的极小弯曲能量不等式，因为此时 $s''(a) = s''(b) = 0$ 依然使得交叉项为零。

**※【引理】** 若 $\mathcal{C}^2$ 函数被完备三次样条或端点指定二阶导数的三次样条插值，则：
$\forall x \in [a, b], \quad |s''(x)| \le 3 \max_{x \in [a, b]} |f''(x)|$ (7.18)

### 7.1.3 误差分析

**※【定理】** 设 $f\in C^2([a,b])\cap C^4([a,b]\setminus X_b)$，且 $f$ 被完备三次样条或端点指定二阶导数的三次样条插值，则
$$
|f^{(j)}(x)-s^{(j)}(x)|
\le c_jh^{4-j}\max_{\xi\in[a,b]\setminus X_b}|f^{(4)}(\xi)|,
\qquad j=0,1,2,\tag{7.20}
$$

其中 $c_0=1/16$，$c_1=c_2=1/2$，$h=\max_i h_i$。若网格均匀且 $h=(b-a)/(N-1)$，则 $j=0$ 时呈现 $O(N^{-4})$ 的逼近阶。
特别地，当 $j=0$ 时，$|f(x)-s(x)|\le c_jh^{4}\max_{\xi\in[a,b]\setminus X_b}|f^{(4)}(\xi)|$.

#### （一）误差估计起步

先取任意 $g\in C^2[x_i,x_{i+1}]$，以两端节点作**线性插值** $I_i g\in\mathbb P_1$。插值余项为
$$
g(x)-I_ig(x)
=\frac{g''(\xi_x)}{2}(x-x_i)(x-x_{i+1}),\qquad \xi_x\in(x_i,x_{i+1}).
$$
因为
$$
\max_{x\in[x_i,x_{i+1}]}|(x-x_i)(x-x_{i+1})|
=\frac{h_i^2}{4},
$$
所以
$$
\|g-I_ig\|_{\infty,[x_i,x_{i+1}]}
\le\frac{h_i^2}{8}\,\|g''\|_{\infty,[x_i,x_{i+1}]}.
\tag{B}
$$

令 $g=f-s$。由节点插值条件 $(f-s)(x_i)=(f-s)(x_{i+1})=0$，$I_i(f-s)\equiv0$。于是

$$
\|f-s\|_\infty\le\frac{h^2}{8}\|f''-s''\|_\infty.
\tag{C}
$$


#### （二）引入 $f''$ 的分段线性插值

令 $w\in\mathbb S_1^0(\Delta)$ 为 $f''$ 的分段 Lagrange 插值函数，即

$$
w(x_i)=f''(x_i),\qquad i=1,\ldots,N.
$$

于是由三角不等式

$$
\|f''-s''\|_\infty
\le\underbrace{\|f''-w\|_\infty}_{\text{分段线性插值误差}}
+\underbrace{\|w-s''\|_\infty}_{\text{还需估计的部分}}.
\tag{D}
$$

对第一项，若 $f\in C^4[a,b]$，可再次使用 (B)（取 $g=f''$），得

$$
\|f''-w\|_\infty\le\frac{h^2}{8}\|f^{(4)}\|_\infty.
\tag{E}
$$

但要从 (C) 推出 $O(h^4)$，还必须进一步估计 $\|w-s''\|_\infty$。

**※【引理】** $\|(S_{\triangle}f)''\|_{\infty}\leq 3\|f''\|_{\infty}$.
**证明**：
令 $s=S_{\triangle}f,\,s''(x_{k})=M_{k}$，且符合自然边界条件.
$\|(S_{\triangle}f)''\|_{\infty}=\|s''\|_{\infty}=\max_{0\leq j\leq N}|M_{j}|=\|\vec{M}\|_{\infty}=|M_{i}|$. 不妨设.
又 $AM=r$，有 $\mu _{i}M_{i-1}+2M_{i}+\upsilon _{i}M_{i+1}=r_{i}$，
故 $|r_{i}|\geq 2|M_{i}|-\mu_{i}|M_{i-1}|-\upsilon _{i}|M_{i+1}|\leq|M_{i}|$.
故 $\|(S_{\triangle}f)''\|_{\infty}=|M_{i}|\leq|r_{i}|=6|f[x_{i-1},x_{i},x_{i+1}]|=3|f''(\xi)|\leq 3\|f''\|_{\infty}$.

继续估计 $\|w-s''\|_\infty$。取 $W''=w$。由$w\in S_{1}^0(\triangle)\implies W\in S_{3}^2(\triangle)\implies W=S_{\triangle}W$.
$\|w-s''\|_\infty=\|W''-s''\|_{\infty}=\\\|(S_{\triangle}(W-f))''\|\leq 3\|(W-f)''\|=3\|w-f''\|_{\infty}\leq \frac{3}{8}\|f^{(4)}\|_{\infty}h^{2}$.

综上整理可以得到 $\|f-s\|_{\infty}\leq \frac{h^4}{16}\|f^{(4)}\|_{\infty}$.

---
拓展内容
## 7.2 B 形式样条(B-form splines)

**※【记号】** $\mathbb{S}_n^{n-1}(t_1, t_2, \dots, t_N)$ 中 $t_i$ 代表节点。无歧义时简写为 $\mathbb{S}_{n,N}^{n-1}$ 或 $\mathbb{S}_n^{n-1}$。

**※【定理】** 样条空间 $\mathbb{S}_n^{n-1}(t_1, \dots, t_N)$ 是一个线性空间，维数为 $n + N - 1$。
**证明**：$N-1$ 个区间，每个区间次数为 $n$ 的多项式有 $n+1$ 个系数，共 $(N-1)(n+1)$。在 $N-2$ 个内部节点处，需满足 $0$ 到 $n-1$ 阶导数连续（$n$ 个条件）。总维度减去约束即得。$\square$

**※【例题】** 三次样条 $\mathbb{S}_3^2$ 的 $n=3$，维度为 $N+2$。除去 $N$ 个节点插值条件，还需 2 个边界条件才能唯一确定，由此衍生出定义 7.5 中的多种三次样条。

### 7.2.1 截断幂函数(Truncated power functions)

**※【定义】** 截断幂函数为 $x_+^n = \begin{cases} x^n & \text{if } x \ge 0, \\ 0 & \text{if } x < 0. \end{cases}$ (7.22)

**※【引理】** $\mathbb{S}_n^{n-1}(t_1, \dots, t_N)$ 的一组基为：
$1, x, x^2, \dots, x^n, (x-t_2)_+^n, (x-t_3)_+^n, \dots, (x-t_{N-1})_+^n$ (7.24)
**证明**：证明线性无关性（从最左侧区间逐步向右可证所有系数为 0）。共 $n+N-1$ 个函数，维数吻合，故构成基。$\square$

**※【推论】** 任意 $s \in \mathbb{S}_{n,N}^{n-1}$ 可表示为：
$s(x) = \sum_{i=0}^n a_i(x-t_1)^i + \sum_{j=1}^{N-2} a_{n+j}(x-t_{j+1})_+^n$ (7.26)

**※【例题】** 截断幂函数基可能因节点不均匀导致高度病态（矩阵接近奇异）。

### 7.2.2 B 样条的局部支撑

**※【定义】** $t_i$ 处的帽函数(hat function)：
$\hat{B}_i(x) = \begin{cases} \frac{x-t_{i-1}}{t_i-t_{i-1}} & x \in (t_{i-1}, t_i], \\ \frac{t_{i+1}-x}{t_{i+1}-t_i} & x \in (t_i, t_{i+1}], \\ 0 & \text{otherwise.} \end{cases}$ (7.28)

**※【定理】** 帽函数构成 $\mathbb{S}_1^0$ 的一组基。
**证明**：根据定义，$\hat{B}_i(t_j) = \delta_{ij}$（克罗内克 $\delta$）。设线性组合为零，代入节点即得系数全零。$\square$

**※【例题】** 使用帽函数基进行分段线性插值是完美条件数的（单位矩阵）。系数直接由函数值给出。

**※【定义】** B样条(B-splines)由如下递归定义（Cox-de Boor 递推）：
$B_i^{n+1}(x) = \frac{x-t_{i-1}}{t_{i+n}-t_{i-1}}B_i^n(x) + \frac{t_{i+n+1}-x}{t_{i+n+1}-t_i}B_{i+1}^n(x)$ (7.31)
基础情况（0次B样条）：
$B_i^0(x) = \begin{cases} 1 & \text{if } x \in (t_{i-1}, t_i], \\ 0 & \text{otherwise.} \end{cases}$ (7.32)

**※【定义】** 支撑集(support)：$\text{supp}(f) = \text{closure}\{x \in \mathbb{R}^n : f(x) \neq 0\}$ (7.35)。

**※【引理】** $B_i^n$ 的支撑区间为 $[t_{i-1}, t_{i+n}]$，且在内部严格大于 $0$。

**※【记号】** 为了分析 B 样条，将差商视为线性泛函，记为 $[x_0, \dots, x_k]f$。

**※【定理】** 莱布尼茨公式(Leibniz formula)：$[x_0, \dots, x_k]fg = \sum_{i=0}^k [x_0, \dots, x_i]f \cdot [x_i, \dots, x_k]g$ (7.38)。
**证明**：对 $k$ 进行归纳，利用差商的递推公式展开化简。$\square$

**※【定理】** B样条可表示为截断幂函数的差商：
$B_i^n(x) = (t_{i+n}-t_{i-1})[t_{i-1}, \dots, t_{i+n}](t-x)_+^n$ (7.39)
**证明**：通过对 $n$ 进行归纳。基础情况 $n=0$ 显然成立。利用莱布尼茨公式作用于 $(t-x)(t-x)_+^n$ 可推得递推关系。$\square$

### 7.2.3 积分与导数

**※【推论】** B样条在其支撑集上的平均值仅取决于其次数：$\frac{1}{t_{i+n}-t_{i-1}}\int_{t_{i-1}}^{t_{i+n}} B_i^n(x)\mathrm{d}x = \frac{1}{n+1}$ (7.41)。

**※【定理】** B样条的导数公式：
$\frac{\mathrm{d}}{\mathrm{d}x}B_i^n(x) = \frac{n B_i^{n-1}(x)}{t_{i+n-1}-t_{i-1}} - \frac{n B_{i+1}^{n-1}(x)}{t_{i+n}-t_i}$ (7.42)
对于 $n=1$，在节点处导数不定义。

**※【推论】** $B_i^n \in \mathbb{S}_n^{n-1}$（B样条属于对应次数和光滑度的样条空间）。

### 7.2.4 马斯登恒等式(Marsden's identity)

**※【定理】** 马斯登恒等式：
$(t-x)^n = \sum_{i=-\infty}^{+\infty} (t-t_i)\dots(t-t_{i+n-1})B_i^n(x)$ (7.44)

**※【推论】** 截断幂函数作为 B 样条的线性组合：
$(t_j-x)_+^n = \sum_{i=-\infty}^{j-n} (t_j-t_i)\dots(t_j-t_{i+n-1})B_i^n(x)$ (7.46)

### 7.2.5 对称多项式(Symmetric polynomials)

**※【定义】** $n$ 元 $k$ 次初等对称多项式：从 $n$ 个变量中选 $k$ 个变量乘积之和。
$ \sigma_k(x_1, \dots, x_n) = \sum_{1 \le i_1 < \dots < i_k \le n} x_{i_1}\dots x_{i_k}$ (7.47)
如果不要求互异（即允许重复选同一个变量），则为完全对称多项式 $\tau_k$。

**※【定义】** 生成函数：$g_{\sigma, n}(z) = \prod_{i=1}^n (1+x_i z)$；$g_{\tau, n}(z) = \prod_{i=1}^n \frac{1}{1-x_i z}$。

**※【定理】** 完全对称多项式可以表示为单项式的差商：
$\tau_{m-n}(x_i, \dots, x_{i+n}) = [x_i, \dots, x_{i+n}]x^m$ (7.57)

### 7.2.6 B样条确实构成一组基

**※【定理】** 单项式 $x^k$ 可表示为 B 样条的线性组合：
$\binom{n}{k}x^k = \sum_{i=-\infty}^{+\infty} \sigma_k(t_i, \dots, t_{i+n-1})B_i^n(x)$ (7.59)
其中 $\sigma_k$ 是初等对称多项式。

**※【推论】** 单位分解(Partition of Unity)：$\sum_{i=-\infty}^{+\infty} B_i^n = 1$ (7.60)。

**※【定理】** 列表 $\{B_{2-n}^n, B_{3-n}^n, \dots, B_N^n\}$ 构成 $\mathbb{S}_n^{n-1}(t_1, \dots, t_N)$ 的一组基。

### 7.2.7 基数 B 样条(Cardinal B-splines)

**※【定义】** 在整数节点集 $\mathbb{Z}$ 上定义的 B 样条称为基数 B 样条，记为 $B_{i, \mathbb{Z}}^n$。

**※【推论】** 同次数的基数 B 样条互为平移：$B_{i, \mathbb{Z}}^n(x) = B_{i+1, \mathbb{Z}}^n(x+1)$。且关于支撑中心对称。

**※【定理】** $n$ 次基数 B 样条可显式表达为求和形式：
$B_{i, \mathbb{Z}}^n(x) = \frac{1}{n!}\sum_{k=-1}^n (-1)^{n-k}\binom{n+1}{k+1}(k+i-x)_+^n$ (7.70)

**※【定理】** 完备三次/二次基数 B 样条的唯一插值问题：可转化为求解一个三对角严格对角占优的线性方程组。具体方程及边界条件见教材公式。

## 7.3 曲线拟合(Curve fitting)

**※【定义】** 累计弦长(cumulative chordal length)：点序列 $(x_i)_{i=1}^N$ 的累计弦长序列定义为 $t_1=0$，且 $t_i = t_{i-1} + \|x_i - x_{i-1}\|_2$ (7.80)。

**※【算法】** 曲线拟合算法：对于曲线 $\gamma$ 上的特征点，计算累计弦长，并将 $t$ 作为独立参数，对 $\gamma$ 的每个坐标分别拟合样条函数。

**※【定理】** 上述曲线拟合算法是良态的。特征点位置 $O(\epsilon)$ 的扰动只会导致拟合曲线上各点产生 $O(\epsilon)$ 的误差。