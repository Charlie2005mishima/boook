

## § 1.1 时间序列的分解

### A. 时间序列

按时间次序排列的随机变量序列 ：$X _ {1}, X _ {2}, \dots$
随机变量 $X_{1}, X_{2}, \cdots, X_{N}$ 的观测值：$x _ {1}, x _ {2}, \dots , x _ {N}$ 

我们用 $\{X_{t}\}$ 表示时间序列(1.1),用 $\{x_{t}\}$ 表示观测样本(1.2). 
为了表达简单,有时还用 $X(t)$ 表示 $X_{t}$ ,用 $x(t)$ 表示 $x_{t}$ . 

### B. 时间序列的分解

大量时间序列的观测样本都表现出趋势性、季节性和随机性. 
这样, 可以认为每个时间序列, 或经过适当的函数变换的时间序列, 都可以分解成三个部分的叠加 
$X _ {t} = T _ {t} + S _ {t} + R _ {t}, \quad t = 1, 2, \dots ,\tag{1.4}$
其中 $\{T_{t}\}$ 是趋势项， $\{S_{t}\}$ 是季节项， $\{R_{t}\}$ 是随机项。时间序列 $\{X_{t}\}$ 是这三项的叠加。 

方法1 分段趋势. 

方法 2 回归直线趋势. 

方法3 二次曲线趋势， 

方法4 逐步平均法. 

### C. 时间序列和随机过程

设 $\mathcal{S}$ 是实数集合 $\mathbb{R} = (-\infty, \infty)$ 的子集. 通常称 $\mathcal{S}$ 为指标集. 如果对每个 $t$ 属于 $\mathcal{S}$ , 都有一个随机变量 $X_t$ 与之对应, 就称随机变量的集合 $\{X _ {t} \} = \{X _ {t}: t \in \mathcal {T} \}\tag{1.12}$ 是一个随机过程. 

当 $\mathcal{S}$ 是全体整数或全体非负整数时, 称相应的随机过程为随机序列. 
把随机序列的指标集合 $\mathcal{S}$ 看成时间指标时, 这个随机序列就是时间序列. 

当 $\mathcal{S}$ 是全体实数或全体非负实数时，相应的随机过程被称为连续时随机过程。
如果把 $\mathcal{S}$ 认做时间指标，连续时随机过程就是连续时的时间序列。 

在应用上, 对连续时的时间序列的处理大多是通过离散化完成的. 这种离散化被称为离散采样. 
因此, 我们以后把重点放在离散时的时间序列上. 

时间序列在适当的去掉趋势项和季节项后,剩下的随机部分通常会有某种平稳性.带有平稳性的时间序列是时间序列分析的研究重点. 

## § 1.2 平稳序列

### A. 平稳序列及其自协方差函数

**※【定义】**  平稳时间序列

如果时间序列 $\{X_{t}\} = \{X_{t}: t \in \mathbb{N}\}$ 满足 
(1) 对任何 $t \in N, EX_{t}^{2} < \infty;$ 
(2) 对任何 $t \in N, EX_{t} = \mu;$ 
(3) 对任何 $t, s \in \mathbb{N}$ , $\operatorname{E}[(X_t - \mu)(X_s - \mu)] = \gamma_{t - s}$ , 
就称 $\{X_{t}\}$ 是平稳时间序列，简称为平稳序列. 称实数列 $\{\gamma_{t}\}$ 为 $\{X_{t}\}$ 的自协方差函数. 
又称平稳序列是二阶矩平稳序列. 



**※【命题】** 自协方差函数的性质

自协方差函数满足以下三条基本性质： 
(1) 对称性: $\gamma_{k} = \gamma_{-k}$ 对所有 $k \in \mathbb{Z}$ 成立; 
(2) 非负定性：对任何 $n \in \mathbb{N}_1$ ， $n$ 阶自协方差矩阵 
$\Gamma_ {n} = (\gamma_ {k - j}) _ {k, j = 1} ^ {n} = \left[ \begin{array}{c c c c} \gamma_ {0} & \gamma_ {1} & \dots & \gamma_ {n - 1} \\ \gamma_ {1} & \gamma_ {0} & \dots & \gamma_ {n - 2} \\ \vdots & \vdots & & \vdots \\ \gamma_ {n - 1} & \gamma_ {n - 2} & \dots & \gamma_ {0} \end{array} \right]\tag{2.1}$ 是非负定矩阵； 
(3) 有界性： $|\gamma_k| \leqslant \gamma_0$ 对所有 $k \in \mathbb{Z}$ 成立. 

任何满足上述性质(1)，(2)，(3)的实数列都被称为非负定序列。所以平稳序列的自协方差函数是非负定序列。

例2.1 平稳序列 $\{X_{t}\}$ 经过线性变换后仍然是平稳序列. 

例 2.2 (调和平稳序列) 设 a, b 是常数, 随机变量 $U'$ 在 $(-π, π)$ 内均匀分布. 
则 $X _ {t} = b \cos (a t + U), \quad t \in \mathbb {Z}$是平稳序列.

### B. 白噪声


**※【定义】** 白噪声

设 $\{\varepsilon_{t}\}$ 是一个平稳序列. 如果对任何 $s, t \in N$ , $\mathrm{E} \varepsilon_ {t} = \mu , \quad \operatorname{cov} (\varepsilon_ {t}, \varepsilon_ {s}) - \left\{ \begin{array}{l l} \sigma^ {2}, & t = s, \\ 0, & t \neq s, \end{array} \right.\tag{2.5}$
就称 $\{\varepsilon_t\}$ 是一个白噪声，记做 $\mathrm{WN}(\mu, \sigma^2)$ . 设 $\{\varepsilon_t\}$ 是 $\mathrm{WN}(\mu, \sigma^2)$ , 

当 $\{\epsilon_{t}\}$ 是独立序列时，称 $\{\epsilon_{t}\}$ 是独立白噪声； 
当 $\mu=0$ 时，称 $\{\varepsilon_{i}\}$ 为零均值白噪声； 
当 $\mu = 0, \sigma^2 = 1$ 时，称 $\{\varepsilon_t\}$ 为标准白噪声. 
对独立白噪声，当 $\varepsilon_{t}$ 服从正态分布时，称 $\{\varepsilon_t\}$ 是正态白噪声. 

引入 Kronecker 函数 $\delta_ {t} = \left\{ \begin{array}{l l} 1, & t = 0, \\ 0, & t \neq 0, \end{array} \right.\tag{2.6}$ 可以将(2.5)写成更简单的形式： 
$\mathrm{E} \varepsilon_ {t} = \mu , \quad \operatorname{cov} (\varepsilon_ {t}, \varepsilon_ {s}) = \sigma^ {2} \delta_ {t - s}.$


**※【定义】**  Poisson 过程和 Poisson 白噪声

如果连续时的随机过程 $\{N(t): t \in [0, \infty)\}$ 满足 
(1) $N(0) = 0$ 
(2) 对任何 $t > s \geqslant 0$ 和非负整数 $k$  $P (N (t) - N (s) = k) = \frac {(\lambda (t - s)) ^ {k}}{k !} \exp [ - \lambda (t - s) ],$ 其中 $\lambda$ 是正数； 
(3) $\{N(t)\}$ 有独立增量性：对任何 n>1 和 $0\leqslant t_{0}<t_{1}<\cdots<t_{n}$ ，
随机变量 $N (t _ {j}) - N (t _ {j - 1}), \quad j = 1, 2, \dots , n$相互独立， 
则称 $\{N(t)\}$ 是一个强度 $\lambda$ 的 Poisson 过程. 

### C. 正交平稳序列

**※【定义】** 对于平稳序列 $\{X_{t}\}$ 和 $\{Y_{t}\}$ ， 

(1) 如果对任何 $s, t \in \mathbb{Z}$ , $\operatorname{E}(X_t Y_t) = 0$ , 则称 $\{X_t\}$ 和 $\{Y_t\}$ 是正交的; 

(2) 如果对任何 $s, t \in \mathbb{Z}$ , $\operatorname{cov}(X_t, Y_s) = 0$ , 则称 $\{X_t\}$ 和 $\{Y_t\}$ 是不相关的. 

可以看出，对于零均值的平稳序列，正交性和不相关性等价. 

**※【定理】** 设 $\gamma_{X}(k)$ 和 $\gamma_{Y}(k)$ 分别是平稳序列 $\{X_{t}\}$ 和 $\{Y_{t}\}$ 的自协方差函数. 记 $\mu_{X} = \mathrm{E}X_{t}$ 和 $\mu_{Y} = \mathrm{E}Y_{t}$ . 定义 $Z _ {t} - X _ {t} + Y _ {t}, \quad t \in \mathbb {Z}.\tag{2.7}$
(1) 如果 $\{X_{t}\}$ 和 $\{Y_{t}\}$ 正交，则 $\{Z_{t}\}$ 是平稳序列，$\gamma_ {z} (k) = \gamma_ {x} (k) + \gamma_ {y} (k) - 2 \mu_ {x} \mu_ {y}, \quad k = 0, 1, 2, \dots ;\tag{2.8}$
(2) 如果 $\{X_{t}\}$ 和 $\{Y_{t}\}$ 不相关，则 $\{Z_{t}\}$ 是平稳序列，有自协方差函数$\gamma_ {z} (k) = \gamma_ {x} (k) + \gamma_ {y} (k), \quad k = 0, 1, 2, \dots .\tag{2.9}$

## § 1.3 线性平稳序列和线性滤波

### A. 有限运动平均

设 $\{\varepsilon_{t}\}=\{\varepsilon_{t}:t\in\mathbb{Z}\}$ 是 $\mathrm{WN}(0,\sigma^{2})$ . 
对于非负整数 q 和常数 $a_{0}, a_{1}, \cdots, a_{q}$ ，我们称 $X _ {t} = a _ {0} \varepsilon_ {t} + a _ {1} \varepsilon_ {t - 1} + \dots + a _ {q} \varepsilon_ {t - q}, \quad t \in \mathbb {Z}$
是白噪声 $\{\varepsilon_t\}$ 的(有限)运动平均, 简称为 MA (Moving Average). 

$\mathrm{E} X _ {t} = \sum_ {j = 0} ^ {q} a _ {j} \mathrm{E} \varepsilon_ {t - j} = 0$ 
$\begin{array}{r l} \mathrm{E} (X _ {n} X _ {m}) & = \sum_ {j = 0} ^ {q} \sum_ {k = 0} ^ {q} a _ {j} a _ {k} \mathrm{E} (\varepsilon_ {n - j} \varepsilon_ {m - k}) \\ & = \left\{ \begin{array}{l l} \sigma^ {2} \sum_ {j = 0} ^ {q - (n - m)} a _ {j} a _ {n - m + j}, & 0 \leqslant n - m \leqslant q, \\ 0, & n - m > q. \end{array} \right. \end{array}$ 
于是， $\{X_{i}\}$ 是平稳序列，有自协方差函数 

$\gamma_ {k} = \mathrm{E} (X _ {k} X _ {0}) = \left\{ \begin{array}{l l} \sigma^ {n} \sum_ {j = 0} ^ {q - k} a _ {j} a _ {j + k}, & 0 \leqslant k \leqslant q, \\ 0, & k > q. \end{array} \right.\tag{3.2}$

如果 $\{X_{i}\}$ 是用来描述随机干扰的，上述运动平均序列说明现在的随机干扰 $X_{n}$ 是现时刻及前面的 $q$ 个时间的白噪声（简单随机干扰）的线性组合.

由于当 $k > q$ 时， $\gamma_{k} = 0$ ，所以，时间间隔大于 $q$ 步的随机干扰是不相关的.

**※【定理】** 单调收敛定理
如果非负随机变量序列单调不减：$0\leq \xi_{1}\leq \xi_{2}\leq\dots$，则当 $\xi _{n}\to \xi$ a.s. 时，有 $E\xi=\lim_{ n \to \infty }E\xi _{n}$  

**※【定理】** 控制收敛定理
如果随机变量序列 $\{\xi_n\}$ 满足 $|\xi_n| \leqslant \xi_0$ a.s.和 $\mathrm{E}|\xi_0| < \infty$ ，则当 $\xi_n \to \xi$ a.s.时， $\mathrm{E}|\xi| < \infty$ 且 $\mathrm{E}\xi_n \to \mathrm{E}\xi.$ 

### B. 线性平稳序列

如果实数列 $\{a_{j}\}$ 满足 $\sum_ {j = - \infty} ^ {\infty} | a _ {j} | <   \infty ,$ 就称 $\{a_{j}\}$ 是绝对可和的. 对于绝对可和的实数列 $\{a_{j}\}$ , 

**※【定义】** 零均值白噪声 $\{\varepsilon_{i}\}$ 的无穷滑动和
$X _ {t} = \sum_ {j = - \infty} ^ {\infty} a _ {j} \varepsilon_ {t - j}, \quad t \in \mathbb {Z}.\tag{3.4}$

**※【命题】** $\{X_{i}\}$ 是平稳序列，有零均值

**※【定理】** 设 $\{\varepsilon_{i}\}$ 是 $\mathrm{WN}(0, \sigma^2)$ ，实数列 $\{a_{j}\}$ 平方可和，线性平稳序列 $\{X_{i}\}$ 由(3.4)定义，则自协方差函数 $\lim_{k \to \infty} \gamma_{k} = 0$ 。 

### C. 时间序列的线性滤波

在数字信号分析和处理中, 时间序列 $\{X_{r}\}$ 被称为信号过程. 
按通常的定义, 信号过程的频率是单位时间内该信号过程的振动次数. 振动次数越大, 频率就越高. 
在一些通讯工程问题里, 常常需要设计一个线性滤波器来滤掉信号过程中的高频噪声. 这种滤波器被称为线性低通滤波器. 

在线性滤波问题中, 绝对可和的实数列 $H = \{h_j\}$ 被称为一个保时线性滤波器. 

信号 $\{X_t\}$ 通过滤波器 $H$ 后得到输出 $Y _ {t} = \sum_ {j = - \infty} ^ {\infty} h _ {j} X _ {t - j}, \quad t \in \mathbb {Z}.\tag{3.9}$
如果 $\{X_{t}\}$ 是平稳信号，有数学期望 $EX_{t} = \mu$ 和自协方差函数 $\{\gamma_k\}$ 
数学期望 $\mu_ {Y} = \mathrm{E} Y _ {t} = \sum_ {j = - \infty} ^ {\infty} h _ {j} \mathrm{E} X _ {t - j} = \mu \sum_ {j = - \infty} ^ {\infty} h _ {j}\tag{3.10}$
自协方差函数 $\gamma_ {Y} (n)  = \sum_ {j, k = - \infty} ^ {\infty} h _ {j} h _ {k} \gamma_ {n + k - j}.$ 

如果取保时线性滤波器 H 满足 $h _ {j} = \left\{ \begin{array}{l l} \frac {1}{2 M + 1}, & | j | \leqslant M, \\ 0, & | j | > M, \end{array} \right. \quad M \in \mathbb {N}.$ 
则 $Y _ {t} = \frac {1}{2 M + 1} (X _ {t - M} + X _ {t - 1 - M} + \dots + X _ {t + M}), t \in \mathbb {Z}$ 是 $\{X_{t}\}$ 的逐步平均. 
它可以对平稳信号 $\{X_{t}\}$ 起平滑的作用, 同时对抑制噪声, 特别是抑制高频噪声是有效的. 

但是要得到更有效的滤波效果, 必须针对具体的信号序列设计不同的滤波器 $H$ . 


## § 1.4 正态时间序列和随机变量的收敛性

### A. 随机向量的数学期望和方差

对于以随机变量 $X_{i,j}$ 构成的 $m \times n$ 的矩阵 $\underline{X} = (X_{i,j})$ ，

如果每个 随机变量的数学期望 $EX_{i,j} = \mu_{i,j}$ 存在
就定义 $X$ 的数学期望 $\mathrm{E} \underline {{X}} = (\mathrm{E} X _ {i, j}) = (\mu_ {i, j}).\tag{4.1}$

如果 n 维随机向量 $X=(X_{1},X_{2},\cdots,X_{n})^{\mathrm{T}}$ 中每个 $X_{j}$ 的方差有限
就定义 X 的协方差矩阵 $\Sigma_ {X} = \operatorname{cov} (X, X) = \mathrm{E} [ (X - \mu) (X - \mu) ^ {\mathrm{T}} ] = (\sigma_ {i, j})$ 
这里 $\mu = EX, \sigma_{i,j} = \operatorname{cov}(X_i, X_j)$ 是 $\Sigma_X$ 的第 $(i,j)$ 元素. 

协方差矩阵 $\Sigma_X$ 总是对称非负定的. 

和一维的情况一样, 协方差矩阵有如下的计算公式 
$\begin{array}{r l} \Sigma_ {X} & = \operatorname{cov} (X, X) = \mathrm{E} [ (X - \mu) (X - \mu) ^ {\mathrm{T}} ] \\ & = \mathrm{E} (X X ^ {\mathrm{T}}) - \mu \mathrm{E} (X) ^ {\mathrm{T}} - \mathrm{E} (X) \mu^ {\mathrm{T}} + \mu \mu^ {\mathrm{T}} \\ & = \mathrm{E} (X X ^ {\mathrm{T}}) - \mathrm{E} (X) \mathrm{E} (X) ^ {\mathrm{T}}. \end{array}\tag{4.3}$

由概率论的知识知道,对于 m 维常数列向量 a 和 $m \times n$ 常数矩阵 B,
由线性变换 $\boldsymbol {Y} = \boldsymbol {a} + B \boldsymbol {X}\tag{4.4}$
定义的 $m$ 维随机向量 $\pmb{Y}$ 分别有数学期望和协方差矩阵 
$\mathbf {E} \pmb {Y} = \pmb {a} + B \mathbf {E} \pmb {X} \quad \text {和} \quad \operatorname{cov} (\pmb {Y}, \pmb {Y}) = B \Sigma_ {\pmb {X}} B ^ {'}.\tag{4.5}$

**※【定义】**  如果存在 m 维常数列向量 $\mu, m \times n$ 常数矩阵 B 和相互独立的标准正态随机变量 $X_{1}, X_{2}, \cdots, X_{n}$ 使得 $Y = \mu + BX$ ，则称随机向量 $Y = (Y_{1}, Y_{2}, \cdots, Y_{m})^{\mathrm{T}}$ 服从 m 维正态分布. 

 $EY=\mu,\Sigma=\operatorname{cov}(Y,Y)=BB^{T}$ 

在定义 4.1 中, 由于 $X_{i}$ 有特征函数 $\mathrm{E} \left[ \exp (\mathrm{i} t X _ {i}) \right] = \exp (- t ^ {2} / 2), \quad t \in \mathbb {R}.$
所以随机向量 $X=(X_{1},X_{2},\cdots,X_{n})^{\mathrm{T}}$ 
有特征函数 $\varphi_ {X} (t) = \mathrm{Eexp} (\mathrm{i} t ^ {\mathrm{T}} X) = \prod_ {j = 1} ^ {n} \exp (- t _ {j} ^ {2} / 2) = \exp (- t ^ {\mathrm{T}} t / 2),$
正态随机向量 Y 有特征函数 $\varphi_ {Y} (t) = \exp \left[ \mathrm{i} t ^ {\mathrm{T}} \mu - \frac {1}{2} t ^ {\mathrm{T}} \Sigma t \right].$


**※【定理】** 
$\boldsymbol{\xi}=(\boldsymbol{\xi}_{1},\boldsymbol{\xi}_{2},\cdots,\boldsymbol{\xi}_{n})^{\mathrm{T}}\sim N(\boldsymbol{\mu},\boldsymbol{\Sigma})$ 的充分必要条件是对任何 $\boldsymbol{a}=(a_{1},a_{2},\cdots,a_{n})^{\mathrm{T}}\in\mathbb{R}^{n}$ ，
有$Y = \boldsymbol {a} ^ {\mathrm{T}} \boldsymbol {\xi} \sim N (\boldsymbol {a} ^ {\mathrm{T}} \mu , \boldsymbol {a} ^ {\mathrm{T}} \Sigma \boldsymbol {a}).\tag{4.7}$

### B. 正态平稳序列

**※【定义】** 正态时间序列
对于时间序列 $\{X_{t}\}$ ，如果对任何 $n \geqslant 1$ 和 $t_{1}, t_{2}, \cdots, t_{n} \in N, (X(t_{1}), X(t_{2}), \cdots, X(t_{n}))$ 服从多元正态分布，就称 $\{X_{t}\}$ 是正态时间序列。特别当 $\{X_{t}\}$ 还是平稳序列时，又称为正态平稳序列。 

收敛性的基本知识. 

**※【定义】** 
设随机变量 $\xi_{n}$ 和 $\xi$ 分别有分布函数 $F_{n}(x)=P(\xi_{n}\leqslant x)$ 和 $F(x)=P(\xi\leqslant x)$ . 当 $n\to\infty$ 时， 
(1) 如果在 F 的每个连续点 x 有 $F_{n}(x) \rightarrow F(x)$ ，就称 $\xi_{n}$ 依分布收敛到 $\xi$ ; 
(2) 如果对任取 $\varepsilon > 0$ 有 $P(|\xi_{n} - \xi| \geqslant \varepsilon) \to 0$ ，就称 $\xi_{n}$ 依概率收敛到 $\xi;$ 
(3) 如果 $E|\xi_{n}-\xi|\rightarrow0$ ，就称 $\xi_{n}$ 在 $L^{1}$ 下收敛到 $\xi$ ; 
(4) 如果 $E(\xi_{n}-\xi)^{2}\rightarrow0$ ，就称 $\xi_{n}$ 均方收敛到 $\xi$ . 


**※【定理】** 在定义 4.3 的四种收敛中， $(4)\Rightarrow(3)\Rightarrow(2)\Rightarrow(1)$ . 

**※【定理】**  如果正态时间序列  依分布收敛到随机变量  ，则  ，并且 
$\mathrm{E} \xi_ {n} \rightarrow \mathrm{E} \xi , \quad \operatorname{var} (\xi_ {n}) \rightarrow \operatorname{var} (\xi).$

**※【定理】** 如果 $\{\varepsilon_{i}\}$ 是正态 $\mathrm{WN}(0,\sigma^{2})$ ，实数列 $\{a_{j}\}$ 绝对可和，
则由$X _ {t} = \sum_ {j = - \infty} ^ {\infty} a _ {j} \varepsilon_ {t - j}, t \in \mathbb {Z}$
定义的平稳序列是零均值正态序列, 自协方差函数由(3.5)给出. 

### § 1.5 严平稳序列及其遍历性

在概率论中，随机向量 $(Y_{1},Y_{2},\cdots,Y_{n})^{\mathrm{T}}$ 的联合分布由 
$F (y _ {1}, y _ {2}, \dots , y _ {n}) = P (Y _ {1} \leqslant y _ {1}, Y _ {2} \leqslant y _ {2}, \dots , Y _ {n} \leqslant y _ {n})$
定义. 具有相同的联合分布函数的随机向量被称为是同分布的. 

在时间序列分析中，称时间序列 $\{X_{t}:t\in N\}$ 和 $\{Y_{t}:t\in N\}$ 是同分布的，如果对任何正整数n和 $t_{1},t_{2},\cdots,t_{n}\in N$ ，随机向量 
$(X (t _ {1}), X (t _ {2}), \dots , X (t _ {n})) ^ {\mathrm{T}} \quad \text {和} \quad (Y (t _ {1}), Y (t _ {2}), \dots , Y (t _ {n})) ^ {\mathrm{T}}$
是同分布的. 

定义5.1 设 $\{X_{t}: t \in \mathbb{N}\}$ 是时间序列. 如果对任何正整数 $n$ 和 $k \in \mathbb{N}$ , 随机向量 
$(X _ {1}, X _ {2}, \dots , X _ {n}) ^ {\mathrm{T}} \quad \text {和} \quad (X _ {1, k}, X _ {2, k}, \dots , X _ {n + k}) ^ {\mathrm{T}}$
同分布，就称 $\{X_{t}: t \in \mathbb{N}\}$ 是严平稳序列. 

严平稳序列的特征是它的分布平移不变性：对任何固定的 $k \geqslant 1$ ，时间序列 $\{X_t: t \in \mathbb{Z}\}$ 和 $\{X_{t+k}: t \in \mathbb{Z}\}$ 同分布。于是，对任何多元函数 $\varphi(x_1, x_2, \cdots, x_m)$ ， 
$Y _ {t} = \varphi (X _ {t + 1}, X _ {t + 2}, \dots , X _ {t + m}), \quad t \in \mathbb {Z}\tag{5.1}$
仍然是严平稳序列(见习题5.2). 

严平稳序列和平稳序列有密切的关系. 由于 $\{X_{t}\}$ 的均值 $EX_{t}$ 和协方差 $\operatorname{cov}(X_{t}, X_{t})$ 都由联合分布决定, 所以如果严平稳序列 $\{X_{t}\}$ 的方差 $\operatorname{var}(X_{t})$ 有限, 则它一定是平稳序列. 但是, 平稳序列一般不必是严平稳序列. 鉴于这点, 平稳序列又被称为弱平稳序列或宽平稳序列. 严平稳序列又被称为强平稳序列. 但是对于正态时间序列 $\{X_{t}\}$ 来讲, 由于 $X = (X(t_{1}), X(t_{2}), \dots, X(t_{n}))^{\mathrm{T}}$ 的数学期望 $EX$ , 协方差矩阵 $\operatorname{cov}(X, X)$ 和 $X$ 的联合分布相互惟一决定, 所以严平稳性和平稳性是等价的. 

在很多的应用科学中, 时间序列的观测是不能重复的. 于是, 人们总是遇到要用时间序列 $\{X_{t}\}$ 的一次实现 $x_{1}, x_{2}, \cdots$ 推断 $\{X_{t}\}$ 的统计性质这个问题. 要做到这点, 必须对时间序列有所要求. 遍历性的要求就是其中一种. 我们不去关心遍历性的数学定义, 但是需要知道如果严平稳序列是遍历的, 那么从它的一次实现 $x_{1}, x_{2}, \cdots$ 就可以推断出这个严平稳序列的所有有限维分布: 
$F (x _ {1}, x _ {2}, \dots , x _ {m}) = P (X _ {1} \leqslant x _ {1}, X _ {2} \leqslant x _ {2}, \dots , X _ {m} \leqslant x _ {m}), \quad m \in \mathbb {N} _ {+}.$
有遍历性的严平稳序列被称为严平稳遍历序列. 在应用工作中以下定理是有用的. 它的证明超出了本书关心的范围. 

定理5.1（遍历定理）如果 $\{X_{t}\}$ 是严平稳遍历序列，则有如下的结果： 

(1) 强大数律：如果 $E|X_{1}|<\infty$ ，则 
$\lim _ {n \rightarrow \infty} \frac {1}{n} \sum_ {t = 1} ^ {n} X _ {t} = E X _ {1}, \quad a. s..$
(2) 对任何多元函数 $\varphi(x_{1}, x_{2}, \cdots, x_{m})$ , 
$Y _ {t} = \varphi (X _ {t + 1}, X _ {t + 2}, \dots , X _ {t - m})$
是严平稳遍历序列. 

下面的定理在判定线性平稳序列的遍历性时是十分有用的. 

定理 5.2 如果 $\{\varepsilon_{i}\}$ 是独立同分布的 $\mathrm{WN}(0,\sigma^{2})$ ，实数列 $\{a_{j}\}$ 平方可和，则线性平稳序列 
$X _ {t} = \sum_ {j = - \infty} ^ {\infty} a _ {j} \varepsilon_ {t - j}, \quad t \in \mathbb {Z}\tag{5.2}$
是严平稳遍历的. 

引入事件 $A$ 的示性函数 
$I [ A ] = \left\{ \begin{array}{l l} 1, & \text {当} A \text {发生}, \\ 0, & \text {当} A \text {不发生}. \end{array} \right.$
例 5.1 对严平稳序列 $\{X_{i}\}$ ，定义严平稳序列 
$Y _ {t} = I \left[ X (t + t _ {1}) \leqslant y _ {1}, X (t + t _ {2}) \leqslant y _ {2}, \dots , X (t + t _ {m}) \leqslant y _ {m} \right], \quad t \in \mathbb {Z}.$
如果 $\{X_{i}\}$ 是遍历的，由定理5.1的(2)知道 $\{Y_{i}\}$ 也是遍历的，并且有界.利用强数大数律(1)得到 
$\begin{array}{r l} & {\underset {n \to \infty} {\lim} \frac {1}{n} \sum_ {i = 1} ^ {n} Y _ {i} = \mathrm{E} Y _ {0}} \\ & {\qquad = P (X (t _ {1}) \leqslant y _ {1}, X (t _ {2}) \leqslant y _ {2}, \dots , X (t _ {m}) \leqslant y _ {m}), \quad \mathrm{a.s.}.} \end{array}$
这个例子说明,在几乎必然的意义下, $\{X_{i}\}$ 的每一次观测都可以决定 $\{X_{i}\}$ 的有限维分布. 

严平稳遍历序列是时间序列的应用和理论分析中常用的时间序列. 如果要使得一个估计量能(几乎必然)收敛到所要估计的参数, 通常都需要这个条件. 


## § 1.6 Hilbert 空间中的平稳序列

早在 1941 年柯尔莫哥洛夫(Kolmogorov)就用泛函分析的方法在 Hilbert 空间中研究了平稳序列的预测问题,为平稳序列的研究开辟了具有实践意义的领域.他在这个领域中创立的概念和方法已经成为研究平稳序列的有力工具(见文献[7]). 

### A. Hilbert 空间

在研究时间序列的预测问题时,常常考虑用历史资料 $X_{1}, X_{2}, \cdots, X_{n}$ 的线性组合 
$a _ {1} X _ {1} + a _ {2} X _ {2} + \dots + a _ {k} X _ {n}\tag{6.1}$
对未来 $X_{n + k}$ 进行预测.也是为了回答 §1.4 中线性序列的均方收敛问题,我们有必要对形如(6.1)的线性组合进行详细考察. 

用 $L^2 (X)$ 表示平稳序列 $\{X_{i}\}$ 中随机变量有限线性组合的全体： 
$L ^ {2} (X) = \left\{\sum_ {j = 1} ^ {k} a _ {j} X (t _ {j}) \mid a _ {j} \in \mathbb {R}, t _ {j} \in \mathbb {Z}, 1 \leqslant j \leqslant k, k \in \mathbb {N} _ {1} \right\},$
则对任何 $X, Y, Z \in L^2(X), a, b \in \mathbb{R}$ 不难看出以下的结论成立： 

(1) $X + Y = Y + X \in L^2(X), (X + Y) + Z = X + (Y + Z)$ ; 

(2) $0 \in L^{2}(X), X + 0 = X, X + (-X) = 0 \in L^{2}(X)$ ; 

(3) $a(X + Y) = aX + aY \in L^2(X), (a + b)X = aX + bX, a(bX) = (ab)X.$ 

所以， $L^2 (X)$ 是一个线性空间.在 $L^2 (X)$ 上定义内积 $\langle X,Y\rangle =$ $\operatorname {E(XY)}$ ，则有 
$\langle X, Y \rangle = \langle Y, X \rangle , \langle a X + b Y, Z \rangle = a \langle X, Z \rangle + b \langle Y, Z \rangle ; \tag {4}$
(5) $\langle X, X \rangle \geqslant 0$ ，并且 $\langle X, X \rangle = 0$ 当且仅当 $X = 0$ ，a.s.. 

所以 $L^2 (X)$ 又是内积空间.在任何内积空间上有Schwarz不等式(习题6.2): 
$| \langle X, Y \rangle | \leqslant [ \langle X, X \rangle \langle Y, Y \rangle ] ^ {1 / 2}.\tag{6.2}$
在 $L^2 (X)$ 上引入距离 $\| X - Y\| = (\langle X - Y,X - Y\rangle)^{1 / 2}$ ，则有 

(6) $\| X - Y \| = \| Y - X \| \geqslant 0$ ，并且 $\| X - Y \| = 0$ ，当且仅当 $X = Y, a, s$ . 

利用 Schwarz 不等式可以证明如下的 

(7) 三角不等式： $\| X - Y \| \leqslant \| X - Z \| + \| Z - Y \|$ . 

这样， $L^2 (X)$ 又成为距离空间.不难看出，在任何内积空间上都可以定义距离，使它自然成为距离空间. 

如果用 $L^2$ 表示二阶矩有限的随机变量的全体： 
$L ^ {2} = \{X: \mathrm{E} X ^ {2} <   \infty \},\tag{6.3}$
则 $L^2$ 也是内积空间和距离空间，并且 $L^2(X)$ 是 $L^2$ 的子空间. 

定义6.1 对 $\xi_{n} \in L^{2}, \xi_{0} \in L^{2}$ , 

(1) 如果 $\lim_{n\to\infty}\|\xi_n-\xi_0\|=0$ ，就称 $\xi_n$ 在 $L^{2}$ 中（或均方）收敛到 $\xi_{0}$ ，记做 $\xi_{n}\xrightarrow{m.s.}\xi_{0}$ ; 

(2) 如果当 $n, m \to \infty$ 时， $\| \xi_n - \xi_m \| \to 0$ ，就称 $\{\xi_n\}$ 是 $L^2$ 中的基本列或 Cauchy 列. 

由于 $\| \xi_n - \xi_0\|^2 = E(\xi_n - \xi_0)^2$ ，所以定义6.1中的(1)和定义4.3中的(4)是一致的. 

定理6.1 如果 $\{\xi_{n}\}$ 是 $L^2$ 中的基本列，则(在a.s.的意义下)有惟一的 $\xi \in L^2$ ，使得 $\xi_{n} \xrightarrow{\mathrm{m.s.}} \xi$ . 

证明 由基本列的定义知道, 存在 $\{n\}$ 的子序列 $\{n_k\}$ , 使得当 $n, m \geqslant n_k$ 时, $\| \xi_n - \xi_m \|^2 \leqslant 2^{-3k}$ . 利用切比雪夫不等式得到 
$P (| \xi_ {n} - \xi_ {m} | \geqslant 2 ^ {- k}) \leqslant 2 ^ {2 k} \mathrm{E} (\xi_ {n} - \xi_ {m}) ^ {2} \leqslant 2 ^ {- k}.$
于是由单调收敛定理得到 
$E \sum_ {k = 1} ^ {\infty} I [ | \xi (n _ {k + 1}) - \xi (n _ {k}) | \geqslant 2 ^ {- k} ]$$\begin{array}{l} = \sum_ {k = 1} ^ {\infty} E I [ | \xi (n _ {k + 1}) - \xi (n _ {k}) | \geqslant 2 ^ {- k} ] \\ = \sum_ {k = 1} ^ {\infty} P (| \xi (n _ {k + 1}) - \xi (n _ {k}) | \geqslant 2 ^ {- k}) \\ \leqslant \sum_ {k = 1} ^ {\infty} 2 ^ {- k} <   \infty . \end{array}$
从而 
$\sum_ {k = 1} ^ {\infty} I [ | \xi (n _ {k + 1}) - \xi (n _ {k}) | \geqslant 2 ^ {- k} ] <   \infty , \quad \mathrm{a.s.}.$
于是对充分大的 $k$ ，有 $\left|\xi (n_{k + 1}) - \xi (n_k)\right| \leqslant 2^{-k}$ ，这样 
$\left| \xi (n _ {k + m}) - \xi (n _ {k}) \right| \leqslant \sum_ {j = 1} ^ {m} \left| \xi (n _ {k + j}) - \xi (n _ {k + j - 1}) \right| \leqslant 2 ^ {- k + 1}.$
说明(在 a. s. 意义下) $\{\xi(n_{k})\}$ 是实数基本列. 由实数的完备性知道有 $\xi$ , 使得 $\lim_{n\to\infty}\xi(n_{k})=\xi$ , a. s., 利用 Fatou 引理(习题 6.3)得到 
$\begin{array}{r l}\mathrm{E} (\xi_ {n} - \xi) ^ {2}&= \mathrm{E} [ \lim _ {k \to \infty} [ \xi_ {n} - \xi (n _ {k}) ] ^ {2} ]\\&\leqslant \varliminf_ {k \to \infty} \mathrm{E} [ \xi_ {n} - \xi (n _ {k}) ] ^ {2}\\&\rightarrow 0, \quad \text {当} n \rightarrow \infty .\end{array}$
利用三角不等式知道， $\sqrt{\mathrm{E}\xi^2} = \| \xi \| \leqslant \| \xi_n - \xi \| + \| \xi_n \| < \infty.$ 于是 $\xi \in L^2$ . 

下面证明惟一性. 如果又有 $\xi_{n} \xrightarrow{\mathrm{m.s.}} \eta$ 成立, 再利用三角不等式得到: 
$\| \eta - \xi \| \leqslant \| \xi - \xi_ {n} \| + \| \eta - \xi_ {n} \| \rightarrow 0, \quad \text {当} n \rightarrow \infty ,$
所以 $\eta = \xi$ ，a.s.. 

如果内积空间中的每个基本列都有极限,而且极限也在这个内积空间中,就称这个内积空间是完备的.定理6.1说明内积空间 $L^{2}$ 是完备的.完备的内积空间又称为Hilbert空间.所以 $L^{2}$ 是Hilbert空间.正因为如此,才使得平稳序列的研究和Hilbert空间的理论发生密切的联系.其实,著名数学家柯尔莫哥洛夫最早就是在一般的Hilbert空间中定义平稳序列的:设 $\{\xi_{n}\}$ 是Hilbert空间H中的序列,如果对任何 $n,m,\langle\xi_{n+m},\xi_{m}\rangle=B(n)$ 不依赖m,就称 $\{\xi_{n}\}$ 是Hilbert空间H中的平稳序列. 

如果用 $\bar{L}^{2}(X)$ 表示 $L^{2}$ 中含 $L^{2}(X)$ 的最小闭子空间，则 $L^{2}(X)$ 是 Hilbert 空间，称为由平稳序列 $\{X_{t}\}$ 生成的 Hilbert 空间. 

### B. 内积的连续性

内积的连续性是内积空间的基本性质. 它方便我们研究时间序列的收敛性. 

定理 6.2（内积的连续性） 在内积空间中，如果 $\|\xi_{n}-\xi\|\to0,\quad\|\eta_{n}-\eta\|\to0$ ，则有 

(1) $\| \xi_n \| \to \| \xi \|$ ; 

(2) $\langle \xi_n, \eta_n \rangle \to \langle \xi, \eta \rangle$ . 

证明 (1) 由三角不等式 

$\| \xi_{n} \| \leqslant \| \xi_{n} - \xi \| + \| \xi \|$ 和 $\| \xi \| \leqslant \| \xi - \xi_{n} \| + \| \xi_{n} \|$ 得到. 

(2) 由 Schwarz 不等式得到 
$\begin{array}{r l}&{| \langle \xi_ {n}, \eta_ {n} \rangle - \langle \xi , \eta \rangle | = | \langle \xi_ {n}, \eta_ {n} - \eta \rangle + \langle \xi_ {n} - \xi , \eta \rangle |}\\&{\quad \leqslant | \langle \xi_ {n}, \eta_ {n} - \eta \rangle | + | \langle \xi_ {n} - \xi , \eta \rangle |}\\&{\quad \leqslant \|   \xi_ {n}   \|   \|   \eta_ {n} - \eta   \| + \|   \xi_ {n} - \xi   \|   \|   \eta   \|}\\&{\quad \rightarrow 0, \quad \text {当} n \rightarrow \infty   \text {时}.}\end{array}$
例6.1 用 $\mathbb{R}^n$ 表示 $n$ 维实向量空间 
$\mathbb {R} ^ {n} = \left\{\boldsymbol {a}: \boldsymbol {a} = (a _ {1}, a _ {2}, \dots , a _ {n}) ^ {\mathrm{T}}, a _ {i} \in \mathbb {R} \right\},$
则 $\mathbb{R}^n$ 是线性空间. 在 $\mathbb{R}^n$ 上定义内积 $\langle a, b \rangle = a^{\mathrm{T}}b$ , 则 $\mathbb{R}^n$ 成为内积空间. 利用实数的完备性, 不难证明 $\mathbb{R}^n$ 是完备的, 从而是 Hilbert 空间. 以后用 $|a| = \sqrt{a^{\mathrm{T}}a}$ 表示 $\mathbb{R}^n$ 中的欧氏模. 

例 6.2 如果随机向量 $X=(X_{1},X_{2},\cdots,X_{n})^{\mathrm{T}}$ 是零均值平稳序列 $\{X_{i}\}$ 一段，则它的线性组合全体构成的内积空间 
$L _ {n} \stackrel {\text {记为}} {=} \operatorname{sp} (X _ {1}, X _ {2}, \dots , X _ {n}) \stackrel {\mathrm{def}} {=} \{\pmb {a} ^ {\mathrm{T}} \pmb {X}: \pmb {a} \in \mathbb {R} ^ {n} \}\tag{6.4}$
是 Hilbert 空间, 称为由 X 生成的 Hilbert 空间. 

实际上， $L_{n}$ 首先是线性空间和内积空间.我们来证明 $L_{n}$ 的完备性.先设 $\{X_{t}\}$ 是标准白噪声WN(0,1)，对任何线性组合 $\xi_{n} = a_{n}^{\mathrm{T}}X$ ，只要 
$\| \xi_ {n} - \xi_ {m} \| ^ {2} = \| a _ {n} ^ {\mathrm{T}} X - a _ {m} ^ {\mathrm{T}} X \| ^ {2} = (a _ {n} - a _ {m}) ^ {\mathrm{T}} (a _ {n} - a _ {m}) \rightarrow 0,$
由例6.1知道有 $a \in \mathbb{R}^n$ ，使得 $|a_n - a| \to 0$ ，当 $n \to \infty$ 。取 $\xi = a^{\mathrm{T}}X$ 时， 
$\left| \xi_ {n} - \xi \right| ^ {2} = (a _ {n} - a) ^ {\mathrm{T}} (a _ {n} - a) \rightarrow 0.$
于是， $L_{n}$ 是完备的. 

对一般的零均值平稳序列,可以设协方差阵 $\Gamma=\mathrm{E}(\mathbf{X}\mathbf{X}^{\mathrm{T}})$ 的秩是 $m,m\leqslant n$ . 就有正交矩阵 P 和对角阵 $\Lambda=\mathrm{diag}(\lambda_{1},\cdots,\lambda_{m},0,\cdots,0)$ , 使得 $\Lambda=P\Gamma P^{T}$ . 取 Y=APX, 其中 
$A = \operatorname{diag} \left(\frac {1}{\sqrt {\lambda_ {1}}}, \dots , \frac {1}{\sqrt {\lambda_ {m}}}, 1, \dots , 1\right),$
于是 $Y=(Y_{1},Y_{2},\cdots,Y_{n})^{\mathrm{T}}$ ，有协方差阵 
$\mathbf {E} (\mathbf {Y Y} ^ {\mathrm{T}}) = A P E (\mathbf {X X} ^ {\mathrm{T}}) P ^ {\mathrm{T}} A ^ {\mathrm{T}} = A \Lambda A ^ {\mathrm{T}} = \operatorname{diag} (1, \dots , 1, 0, \dots , 0).$
说明 $Y_{1}, Y_{2}, \cdots, Y_{m}$ 是某个 WN(0,1) 的一段. 而且从 
$\pmb {a} ^ {\mathrm{T}} \pmb {X} = \pmb {a} ^ {\mathrm{T}} (A P) ^ {- 1} \pmb {Y} \quad \text {和} \quad \pmb {Y} = A P \pmb {X}$
看出， $L_{n}$ 是 $Y_{1}, Y_{2}, \cdots, Y_{m}$ 的线性组合的全体。于是 $L_{n}$ 是完备的。 

例6.3 设 $\{\varepsilon_t\}$ 是 $\mathbf{WN}(0, \sigma^2), \{a_j\}$ 平方可和. 定义 
$\xi_ {n} = \sum_ {j = - n} ^ {n} a _ {j} \varepsilon_ {i - j}, \quad t \in \mathbb {Z}.$
则对 $m < n$ ，当 $m \to \infty$ 时， 
$\begin{array}{r l}\| \xi_ {n} - \xi_ {m} \| ^ {2}&= \left\| \sum_ {j = m + 1} ^ {n} a _ {j} \varepsilon_ {t - j} + \sum_ {j = - n} ^ {- m - 1} a _ {j} \varepsilon_ {t - j} \right\| ^ {2}\\&= \sigma^ {2} \left[ \sum_ {j = m + 1} ^ {n} a _ {j} ^ {2} + \sum_ {j = - n} ^ {- m - 1} a _ {j} ^ {2} \right]\rightarrow 0.\end{array}$
于是，有 $X_{t}\in L^{2}$ ，使得 $\xi_{n}\xrightarrow{\mathrm{III.S.}} X_{t}$ 为了表示 $X_{t}$ 是 $\xi_{n}$ 的均方极限，定义 
$X _ {t} = \sum_ {j = - \infty} ^ {\infty} a _ {j} \varepsilon_ {t - j}, \quad t \in \mathbb {Z}.\tag{6.5}$
利用内积的连续性得到 
$\mathrm{E} X _ {t} = \lim _ {n \rightarrow \infty} \langle \xi_ {n}, 1 \rangle = \lim _ {n \rightarrow \infty} \mathrm{E} \xi_ {n} = 0,$$\begin{array}{l}\mathrm{E} X _ {t} = \lim _ {n \rightarrow \infty} \langle \xi_ {n}, 1 \rangle = \lim _ {n \rightarrow \infty} \mathrm{E} \xi_ {n} = 0,\\\gamma_ {k} = \mathrm{E} (X _ {t} X _ {t + k}) = \lim _ {n \rightarrow \infty} \left\langle \sum_ {j = - n} ^ {n} a _ {j} \varepsilon_ {t - j}, \sum_ {j = - n} ^ {n} a _ {j} \varepsilon_ {t + k - j} \right\rangle\\= \lim _ {n \rightarrow \infty} \mathrm{E} \left[ \sum_ {j = - n} ^ {n} a _ {j} \varepsilon_ {t - j} \sum_ {j = - n} ^ {n} a _ {j} \varepsilon_ {t + k - j} \right] = \sigma^ {2} \sum_ {j = - \infty} ^ {\infty} a _ {j} a _ {j + k}.\end{array}\tag{6.6}$
这说明 $\{X_{i}\}$ 是平稳序列，以(6.6)为自协方差函数. 

### C. 复值时间序列

在时间序列分析中, 引入复值的随机变量往往会带来很多的方便. 如果 $X$ 和 $Y$ 是随机变量, 就称 $Z = X + \mathrm{i}Y$ 是复随机变量. 如果 $EX$ 和 $EY$ 都存在, 就称 $Z = X + \mathrm{i}Y$ 的数学期望存在, 并且定义 
$\mathrm{EZ} = \mathrm{EX} + \mathrm{iEY}.$
如果 
$\mathrm{E} | Z | ^ {2} = \mathrm{E} X ^ {2} + \mathrm{E} Y ^ {2} <   \infty ,$
就称 Z 是二阶矩有限的复随机变量. 

用 $H$ 表示二阶矩有限的复随机变量的全体，用 $\overline{Y}$ 表示 $Y$ 的共轭。在 $H$ 上定义内积 $\langle X, Y \rangle = E(X\overline{Y})$ ，可以证明 $H$ 是复（复数域上的）Hilbert 空间。这是因为 $\{Z_n\}$ 是基本列，当且仅当 $\{Z_n\}$ 的实部和虚部都是基本列。定理 6.2 在复 Hilbert 空间上也成立。 

按时间次序排列的复随机变量 $\{Z_{n}\}$ 的序列称为复时间序列. 如果复时间序列 $\{Z_{n}\}$ 满足 
$\mathrm{E} Z _ {n} = \mu , \quad \gamma_ {n - m} = \mathrm{E} [ (Z _ {n} - \mu) (\overline {{Z _ {m} - \mu}}) ], \quad n, m \in \mathbb {Z},$
就称 $\{Z_{n}\}$ 是一个复值平稳序列，称 $\{\gamma_k\}$ 是 $\{Z_n\}$ 的自协方差函数. 

特别当 
$\mathrm{E} Z _ {n} = 0, \quad \mathrm{E} (Z _ {n} \bar {Z} _ {m}) = \sigma^ {2} \delta_ {n - m}, \quad n, m \in \mathbb {Z}$
时，称 $\{Z_{n}\}$ 是一个复值零均值白噪声.这里 $\delta_{n - m}$ 是Kronecker函数 

例6.4 设随机变量 $Y$ 在 $[- \pi, \pi]$ 上均匀分布，于是有概率密度 $p(x) = 1 / 2\pi, x \in [-\pi, \pi]$ . 定义 $\varepsilon_{n} = \mathrm{e}^{\mathrm{i}nY}, n \in \mathbb{Z}$ ，则 
$\mathrm{E} \varepsilon_ {n} = \delta_ {n}, \quad \mathrm{E} (\varepsilon_ {n} \dot {\varepsilon} _ {m}) = \frac {1}{2 \pi} \int_ {- \pi} ^ {\pi} \mathrm{e} ^ {\mathrm{i} (n - m) y} \mathrm{d} y = \delta_ {n - m}.$
对于平方可和的实数列 $\{a_{j}\}$ ，定义 
$Z _ {n} = \sum_ {j = - \infty} ^ {\infty} a _ {j} \varepsilon_ {n - j} = \sum_ {j = - \infty} ^ {\infty} a _ {j} \mathrm{e} ^ {\mathrm{i} (n - j) Y}, \quad n \in \mathbb {Z}.$
由内积的连续性得到 
$\mathrm{E} Z _ {n} = a _ {n}, \quad \mathrm{E} (Z _ {n} \overline {{{Z}}} _ {m}) = \sum_ {j = - \infty} ^ {\infty} a _ {j} a _ {j + n - m}.$
另一方面又有 
$\begin{array}{r l} \mathrm{E} (Z _ {n} \bar {Z} _ {m}) & = \mathrm{E} \left[ \sum_ {i = - \infty} ^ {\infty} a _ {j} \mathrm{e} ^ {i (n - j) Y} \sum_ {k = - \infty} ^ {\infty} a _ {k} \mathrm{e} ^ {- i (m - k) Y} \right] \\ & = \frac {1}{2 \pi} \int_ {- \pi} ^ {\pi} \left[ \sum_ {j = - \infty} ^ {\infty} a _ {j} \mathrm{e} ^ {i (n - j) y} \right] \left[ \sum_ {k = - \infty} ^ {\infty} a _ {k} \mathrm{e} ^ {- i (m - k) y} \right] \mathrm{d} y \\ & = \frac {1}{2 \pi} \int_ {- \pi} ^ {\pi} \left| \sum_ {j = - \infty} ^ {\infty} a _ {j} \mathrm{e} ^ {- i j y} \right| ^ {2} \mathrm{e} ^ {i (n - m) y} \mathrm{d} y. \end{array}$
最后得到 
$\sum_ {j = - \infty} ^ {\infty} a _ {j} a _ {j + k} = \frac {1}{2 \pi} \int_ {- \pi} ^ {\pi} \left| \sum_ {j = - \infty} ^ {\infty} a _ {j} e ^ {- i y} \right| ^ {2} e ^ {- i k y} d y.$
定义 
$f (\lambda) = \frac {\sigma^ {2}}{2 \pi} \left| \sum_ {j = - \infty} ^ {\infty} a _ {j} e ^ {- i j \lambda} \right| ^ {2} = \frac {\sigma^ {2}}{2 \pi} \left| \sum_ {j = - \infty} ^ {\infty} a _ {j} e ^ {i j \lambda} \right| ^ {2}.$
注意 $f(\lambda)$ 是偶函数，就得到公式 
$\sigma^ {2} \sum_ {j = - \infty} ^ {\infty} a _ {j} a _ {j + k} = \int_ {- \pi} ^ {\pi} f (\lambda) \mathrm{e} ^ {i k y} \mathrm{d} y, \quad k \in \mathbb {Z}.\tag{6.7}$
注 以后在使用复值随机变量时将有说明. 没有说明的随机变量都是实值的. 


## § 1.7 平稳序列的谱函数

随机变量的统计性质可以由它的分布函数或概率密度刻画. 分布函数是单调不减的右连续函数. 概率密度是在整个直线上积分值等于 1 的非负函数. 任何随机变量的分布函数都惟一存在. 只有连续型的随机变量才有概率密度. 完全类似地, 平稳序列的二阶统计性质可以由它的谱分布函数或谱密度函数刻画. 下面会看到平稳序列的谱分布函数是惟一存在的, 但是并不是所有的平稳序列都存在谱密度. 

定义7.1 设平稳序列 $\{X_{i}\}$ 有自协方差函数 $\{\gamma_k\}$ ， 

(1) 如果有 $[-π, π]$ 上的单调不减右连续的函数 $F(\lambda)$ ，使得 
$\gamma_ {k} = \int_ {- \pi} ^ {\pi} \mathrm{e} ^ {\mathrm{i} k \lambda} \mathrm{d} F (\lambda), \quad F (- \pi) = 0, \quad k \in \mathbb {Z},\tag{7.1}$
就称 $F(\lambda)$ 是 $\{X_{t}\}$ 或 $\{\gamma_k\}$ 的谱分布函数，简称为谱函数. 

(2) 如果有 $[- \pi, \pi]$ 上的非负函数 $f(\lambda)$ , 使得 
$\gamma_ {k} = \int_ {- \pi} ^ {\pi} f (\lambda) \mathrm{e} ^ {i k \lambda} \mathrm{d} \lambda , \quad k \in \mathbb {Z},\tag{7.2}$
就称 $f(\lambda)$ 是 $\{X_{i}\}$ 或 $\{\gamma_k\}$ 的谱密度函数或功率谱密度，简称为谱密度或功率谱. 

如果 $\{X_{i}\}$ 有谱密度 $f(\lambda)$ ，则变上限的积分 
$F (\lambda) = \int_ {- \pi} ^ {\lambda} f (s) d s\tag{7.3}$
就是 $\{X_{i}\}$ 的谱函数. 当谱函数 $F(\lambda)$ 绝对连续, 它的几乎处处导函数 $F'(\lambda)$ 就是谱密度. 特别, 当 $F(\lambda)$ 是连续函数, 除去有限点外导函数存在且连续, 则 
$f (\lambda) = \left\{ \begin{array}{l l} F ^ {\prime} (\lambda), & \text {当} F ^ {\prime} (\lambda) \text {存在,} \\ 0, & \text {当} F ^ {\prime} (\lambda) \text {不存在} \end{array} \right.$
是谱密度. 

平稳序列的谱函数是否总存在呢? Herglotz 定理给出了肯定的回答. 证明可以参见文献[11]. 

定理 7.1 (Herglotz 定理) 平稳序列的谱函数是惟一存在的. 

从Herglotz定理知道，平稳序列的功率谱如果存在，则在几乎处处的意义下是惟一的. 

定理 7.2 如果 $\{\varepsilon_{t}\}$ 是 $\mathrm{WN}(0,\sigma^{2})$ ，实数列 $\{a_{j}\}$ 平方可和，则线性平稳序列 
$X _ {t} = \sum_ {j = - \infty} ^ {\infty} a _ {j} \varepsilon_ {t - j}, \quad t \in \mathbb {Z}$
有谱密度 
$f (\lambda) = \frac {\sigma^ {2}}{2 \pi} \left| \sum_ {j = - \infty} ^ {\infty} a _ {j} e ^ {i j \lambda} \right| ^ {2}.\tag{7.4}$


定理7.3 设 $\{X_{i}\}$ 和 $\{Y_{i}\}$ 是相互正交的零均值平稳序列， $c$ 是常数，定义 
$Z _ {t} = X _ {t} + Y _ {t} + c, \quad t \in \mathbb {Z}.$
(1) 如果 $\{X_{t}\}$ 和 $\{Y_{t}\}$ 分别有谱函数 $F_{X}(\lambda)$ 和 $F_{Y}(\lambda)$ ，则平稳序列 $\{Z_{t}\}$ 有谱函数 $F_{Z}(\lambda)=F_{X}(\lambda)+F_{Y}(\lambda)$ ; 

(2) 如果 $\{X_{t}\}$ 和 $\{Y_{t}\}$ 分别有谱密度 $f_{X}(\lambda)$ 和 $f_{Y}(\lambda)$ , 则 $\{Z_{t}\}$ 有谱密度 $f_{Z}(\lambda) = f_{X}(\lambda) + f_{Y}(\lambda)$ . 

证明 由定理2.2知道 $\{Z_{t}\}$ 是平稳序列，且有自协方差函数 
$\gamma_ {z} (k) = \gamma_ {x} (k) + \gamma_ {y} (k), \quad k \in \mathbb {Z}.$
于是，在(1)的条件下，由 
$\begin{array}{r l} & \int_ {- \pi} ^ {\pi} \mathrm{e} ^ {\mathrm{i} k \lambda} \mathrm{d} [ F _ {X} (\lambda) + F _ {Y} (\lambda) ] \\ & = \int_ {- \pi} ^ {\pi} \mathrm{e} ^ {\mathrm{i} k \lambda} \mathrm{d} F _ {X} (\lambda) + \int_ {- \pi} ^ {\pi} \mathrm{e} ^ {\mathrm{i} k \lambda} \mathrm{d} F _ {Y} (\lambda) \\ & = \gamma_ {X} (k) + \gamma_ {Y} (k) = \gamma_ {Z} (k) \end{array}$
和谐函数的定义，得到 $\{Z_t\}$ 的谱函数 $F_Z(\lambda) = F_X(\lambda) + F_Y(\lambda)$ . 

在(2)的条件下，由 
$\begin{array}{r l} \int_ {- \pi} ^ {\pi} \mathrm{e} ^ {\mathrm{i} k \lambda} [ f _ {X} (\lambda) + f _ {Y} (\lambda) ] \mathrm{d} \lambda & = \int_ {- \pi} ^ {\pi} \mathrm{e} ^ {\mathrm{i} k \lambda} f _ {X} (\lambda) \mathrm{d} \lambda + \int_ {- \pi} ^ {\pi} \mathrm{e} ^ {\mathrm{i} k \lambda} f _ {Y} (\lambda) \mathrm{d} \lambda \\ & = \gamma_ {X} (k) + \gamma_ {Y} (k) = \gamma_ {Z} (k) \end{array}$
和谐密度的定义, 得到 $\{Z_{t}\}$ 的谱密度 $f_{Z}(\lambda)=f_{X}(\lambda)+f_{Y}(\lambda)$ . 

下面考察从线性滤波器输出的平稳序列的谱函数和谱密度的变化. 设平稳序列 $\{X_{i}\}$ 有谱函数 $F_{X}(\lambda)$ 和自协方差函数 $\{\gamma_{k}\}$ , $H = \{h_{j}\}$ 是一个绝对可和的保时线性滤波器(参考 §1.3). 当输入过程是 $\{X_{i}\}$ 时, 输出过程是 
$Y _ {t} = \sum_ {j = - \infty} ^ {\infty} h _ {j} X _ {t - j}, \quad t \in \mathbb {Z}.\tag{7.5}$
由(3.11)知道， $\{Y_{i}\}$ 有自协方差函数 
$\gamma_ {Y} (k) = \sum_ {l, j = - \infty} ^ {\infty} h _ {l} h _ {j} \gamma (k + l - j).\tag{7.6}$
利用等式 
$\sum_ {i, j = - \infty} ^ {\infty} | h _ {i} h _ {j} | = \left[ \sum_ {j = - \infty} ^ {\infty} | h _ {j} | \right] ^ {2} <   \infty$
和控制收敛定理,得到 
$\begin{array}{r l} \gamma_ {Y} (k) & = \sum_ {l, j = - \infty} ^ {\infty} h _ {l} h _ {j} \int_ {- \pi} ^ {\pi} \exp (\mathrm{i} (k + l - j) \lambda) \mathrm{d} F _ {X} (\lambda) \\ & = \int_ {- \pi} ^ {\pi} \sum_ {l, j = - \infty} ^ {\infty} h _ {l} h _ {j} \exp (\mathrm{i} (l - j) \lambda) \mathrm{e} ^ {\mathrm{i} k \lambda} \mathrm{d} F _ {X} (\lambda) \\ & = \int_ {- \pi} ^ {\pi} \left| \sum_ {j = - \infty} ^ {\infty} h _ {j} \exp (- i j \lambda) \right| ^ {2} \mathrm{e} ^ {\mathrm{i} k \lambda} \mathrm{d} F _ {X} (\lambda) \\ & = \int_ {- \pi} ^ {\pi} | H (\mathrm{e} ^ {- \mathrm{i} \lambda}) | ^ {2} \mathrm{e} ^ {\mathrm{i} k \lambda} \mathrm{d} F _ {X} (\lambda), \end{array}\tag{7.7}$
其中 
$H (z) = \sum_ {j = - \infty} ^ {\infty} h _ {j} z ^ {j}, \quad | z | \leqslant 1.\tag{7.8}$
于是得到 $\{Y_t\}$ 的谱函数 
$F _ {Y} (\lambda) = \int_ {- \pi} ^ {\lambda} | H (e ^ {- u}) | ^ {2} d F _ {X} (s).\tag{7.9}$
特别当 $\{X_{t}\}$ 有谱密度 $f_{X}(\lambda)$ 时，可以将(7.9)写成 
$F _ {Y} (\lambda) = \int_ {- \pi} ^ {\lambda} | H (e ^ {- s}) | ^ {n} f _ {X} (s) d s.\tag{7.10}$
于是，滤波后的平稳序列 $\{Y_i\}$ 有谱密度 
$f _ {Y} (\lambda) = \left| I I \left(\mathrm{e} ^ {- \mathrm{i} \lambda}\right) \right| ^ {\gamma} f _ {X} (\lambda).\tag{7.11}$
我们把上述结果总结成定理如下. 

定理 7.4 设 $\{X_{i}\}$ 是平稳序列， $II=\{h_{j}\}$ 是绝对可和的保时线性滤波器， $\{Y_{i}\}$ 和 $II(z)$ 分别由 (7.5) 和 (7.8) 定义. 

(1) 如果 $\{X_{t}\}$ 有谱函数 $F_{X}(\lambda)$ ，则 $\{Y_{t}\}$ 有谱函数 (7.9); 

(2) 如果 $\{X_{t}\}$ 有谱密度 $f_{X}(\lambda)$ ，则 $\{Y_{t}\}$ 有谱密度 (7.11). 

在本节的最后指出：实值平稳序列的谱密度是偶函数(习题7.1). 


## § 1.8 离散谱序列及其周期性

### A. 简单的离散谱序列

设随机变量 $\xi, \eta$ 满足 $E\xi = E\eta = 0, E(\xi\eta) = 0, E\xi^2 = E\eta^2 = \sigma^2$ . 对于常数 $\lambda_0 \in (0, \pi]$ , 定义时间序列 
$Z _ {t} = \xi \cos (t \lambda_ {0}) + \eta \sin (t \lambda_ {0}), t \in \mathbb {N} _ {r}.\tag{8.1}$
如果取 $A$ 和 $\theta$ 满足 
$A = \sqrt {\xi^ {2}} + \eta^ {2}, \quad \cos \theta = \frac {\xi}{A}, \quad \sin \theta = \frac {\eta}{A},\tag{8.2}$
可以把 $Z_{i}$ 改写成 
$Z _ {t} = A \cos (t \lambda_ {0} - \theta), \quad t \in \mathbb {N} _ {+}.\tag{8.3}$
这是一个以 $\lambda_0$ 为角频率，以 $T = 2\pi /\lambda_0$ 为周期的具有随机振幅 $A$ 和随机初始相位 $\theta$ 的三角函数.容易计算出， $EZ_{t} = 0$ ，及 
$\begin{array}{r l} \gamma_ {t - s} & = \mathrm{E} (Z _ {t} Z _ {s}) \\ & = \sigma^ {2} [ \cos (t \lambda_ {0}) \cos (s \lambda_ {0}) + \sin (t \lambda_ {0}) \sin (s \lambda_ {0}) ] \\ & = \sigma^ {2} \cos ((t - s) \lambda_ {0}). \end{array}\tag{8.4}$
于是 $\{Z_{t}\}$ 平稳，有自协方差函数 $\{\gamma_k\}$ . (8.3)也称为调和平稳序列. 完全不同于线性平稳序列的自协方差函数收敛到零， $\{Z_{t}\}$ 的自协方差函数 $\gamma_{k}$ 是周期函数. 这里我们再次看到：平稳序列的观测也可以是纯周期函数. 

为了得到(8.3)的谱函数,用 $I_{[a,b]}(\lambda)$ 表示集合 $[a,b]$ 上以 $\lambda$ 为自变量的示性函数: 
$I _ {[ a, b ]} (\lambda) = \left\{ \begin{array}{l l} 1, & \lambda \in [ a, b ], \\ 0, & \text {其他}. \end{array} \right.$
当 $\lambda_0\neq \pi$ 时，定义阶梯函数 
$F (\lambda) = \frac {\sigma^ {2}}{2} \left[ I _ {[ - \lambda_ {0}, \pi ]} (\lambda) + I _ {[ \lambda_ {0}, \pi ]} (\lambda) \right].$
则有 $F(-\pi) = 0$ ，并且对任何整数 $\pmb{k}$ 
$\int_ {- \pi} ^ {\pi} \mathrm{e} ^ {i k \lambda} \mathrm{d} F (\lambda) = \frac {\sigma^ {2}}{2} [ \mathrm{e} ^ {- i k \lambda_ {0}} + \mathrm{e} ^ {i k \lambda_ {0}} ] = \gamma_ {k}.$
所以 $F(\lambda)$ 是 $\{Z_t\}$ 的谱函数. 当 $\lambda_0 = \pi, \{Z_t\}$ 的谱函数是 
$F (\lambda) = \sigma^ {2} I _ {\{\pi \}} (\lambda).$
当平稳序列的谱函数 $F(\lambda)$ 是阶梯函数时，习惯上就把它称为离散谱函数。把相应的平稳序列称为离散谱序列。（8.1）是最简单的离散谱序列。 

设 $\{Z_{i}\}$ 由(8.1)定义，从数学上讲， $\{Z_{i}\}$ 的谱密度是不存在的。但是对 $\lambda_0 < \pi$ ，定义集合 
$A _ {n} - \left[ \lambda_ {0} - 1 / 2 n, \lambda_ {0} \right], \quad B _ {n} = \left[ - \lambda_ {0}, - \lambda_ {0} + 1 / 2 n \right]$
和非负函数 
$f _ {n} (\lambda) = \left\{ \begin{array}{l l} n \sigma^ {2}, & \text {当} \lambda \in A _ {n} \bigcup B _ {n}, \\ 0, & \text {其他}. \end{array} \right.$
相应地按公式(7.3)定义 
$F _ {n} (\lambda) = \int_ {- \pi} ^ {\lambda} f _ {n} (\lambda) \mathrm{d} \lambda .$
这时 $F_{n}(\lambda)$ 是一连续的折线函数. 当 $n \to \infty$ 时， 
$F _ {n} (\lambda) \rightarrow F (\lambda), \quad \lambda \neq \pm \lambda_ {0}.$
利用中值定理，还知道对某列 $a_{n} \in (-1 / 2n, 0)$ ，当 $n \to \infty$ 时， 
$\begin{array}{r l}g _ {k} (n)&= \int_ {- \pi} ^ {\pi} \mathrm{e} ^ {\mathrm{i} k \lambda} f _ {n} (\lambda) \mathrm{d} \lambda = \sigma^ {2} \int_ {A _ {n}} n \mathrm{e} ^ {\mathrm{i} k \lambda} \mathrm{d} \lambda + \sigma^ {2} \int_ {B _ {n}} n \mathrm{e} ^ {\mathrm{i} k \lambda} \mathrm{d} \lambda\\&= 2 n \sigma^ {2} \int_ {\lambda_ {0} - 1 / 2 n} ^ {\lambda_ {0}} \cos (k \lambda) \mathrm{d} \lambda\\&= \sigma^ {2} \cos (k \lambda_ {0} + k a _ {n})\\&\rightarrow \sigma^ {2} \cos (k \lambda_ {0}) = \gamma_ {k}.\end{array}\tag{8.5}$
不难看出，对任何固定的 $m$ ，当 $n\to \infty$ 时， 
$\max _ {0 \leqslant k \leqslant m} | g _ {k} (n) - \gamma_ {k} | \rightarrow 0,$
从图1.8.1看到，当 $n = 20$ ，对于 $0 \leqslant k \leqslant 50, \gamma_k$ 和 $g_k(n)$ 已经很接近了。如果取 $n = 80$ ，对 $0 \leqslant k \leqslant 50, \gamma_k$ 和 $g_k(n)$ 就基本相同了。由于自协方差函数反映平稳序列的统计规律，所以对较大的 $n$ ，以 $f_n(\lambda)$ 为谱密度的平稳序列有与调和平稳序列 $\{Z_t\}$ 相近的统计规律。由于 $\{Z_t\}$ 是余弦函数，角频率是 $\lambda_0$ ，所以可以想象对较大的 $n$ ，以 $f_n(\lambda)$ 为谱密度的平稳序列也近似地有角频率 $\lambda_0$ 。 $n$ 越大，频率特性越强。 

![](images/f82744925fd9df193ecd185e161d41b4045b148eec98697971e8e3ac45a16c2a.jpg)



图1.8.1 $n = 20, \lambda = 0.9$ 时， $\gamma_{k}$ 和 $g_{k}(n)$ 的50个数据


从上面的解释可以联想到下面的现象：如果平稳序列的谱密度存在，并且在某一点 $\lambda_0$ 有一个显著的峰值，则该平稳序列应该有一个以 $\lambda_0$ 为角频率的频率成分。这个峰值越陡峭，相应的频率成分就越重要，或说在这个频率点集中的能量越大。实际情况确实是这样的。我们将在 §8.1 中对此做出进一步解释。 

在上面的举例中， $F_{n}(\lambda)$ 向 $F(\lambda)$ 的收敛是一个从量变到质变的过程。每个 $F_{n}$ 有密度 $f_{n}$ ，随着谱密度 $f_{n}(\lambda)$ 的能量逐步向角频率 $\lambda_{0}$ 集中，时间序列的周期性越来越明显，最终通过质变得到一个以 $F(\lambda)$ 为谱函数的离散谱序列。 

### B. 多个频率成分的离散谱序列

上面定义了有单个频率的平稳序列,实际问题中常遇到的是多个频率成分叠加构成的离散谱序列.设随机变量 $\xi_{j},\eta_{k}(j,k=1,2,\cdots,p)$ 两两正交,满足 
$\mathrm{E} \xi_ {j} = \mathrm{E} \eta_ {j} = 0, \quad \mathrm{E} \xi_ {j} ^ {2} = \mathrm{E} \eta_ {j} ^ {2} = \sigma_ {j} ^ {2}.\tag{8.6}$
对于正整数 $p$ 和 $\lambda_{j} \in (0, \pi]$ , 定义时间序列 
$Z _ {i} = \sum_ {j = 1} ^ {p} \left[ \xi_ {j} \cos (t \lambda_ {j}) + \eta_ {j} \sin (t \lambda_ {j}) \right]$$= \sum_ {j = 1} ^ {p} A _ {j} \cos (t \lambda_ {j} - \theta_ {j}), t \in \mathbb {N} _ {1},\tag{8.7}$
其中 
$A _ {j} = \sqrt {\xi_ {j} ^ {2} + \eta_ {j} ^ {2}}, \quad \cos (\theta_ {j}) = \frac {\xi_ {j}}{A _ {j}}, \quad \sin (\theta_ {j}) = \frac {\eta_ {j}}{A _ {j}}.\tag{8.8}$
这是 $p$ 个以 $\lambda_{j}$ 为角频率，以 $T = 2\pi /\lambda_{j}$ 为周期具有随机振幅 $A_{j}$ 和随机初始相位 $\theta_{j}$ 的三角函数的叠加.不难看到在(8.7)中，尽管 $A_{j}$ 和 $\theta_{j}$ 是随机变量，但是在观测数据中它们不随时间 $t$ 变化，所以只起到常数的作用.人们把这种随机序列称为伪随机序列. 

由于 $\{Z_{i}\}$ 是 $p$ 个相互正交的零均值平稳序列的和，利用定理2.2知道， $\{Z_{i}\}$ 是平稳序列，具有零均值和自协方差函数 
$\gamma_ {k} = \sum_ {j = 1} ^ {p} \sigma_ {j} ^ {2} \cos (k \lambda_ {j}), \quad k \in \mathbb {Z}.\tag{8.9}$
自协方差函数 $\{\gamma_k\}$ 也是 $p$ 个三角函数的叠加. 利用定理7.3知道当 $\lambda_j \neq \pi, 1 \leqslant j \leqslant p$ 时, 阶梯函数 
$F (\lambda) = \sum_ {j = 1} ^ {p} \frac {\sigma_ {j} ^ {2}}{2} [ I _ {[ - \lambda_ {j}, \pi ]} (\lambda) + I _ {[ \lambda_ {j}, \pi ]} (\lambda) ]$
是 $\{Z_{i}\}$ 的谱函数. 这是一个纯跳跃函数, 在每个 $\pm\lambda_{j}$ 处有跳跃 $\sigma_{j}^{2}/2$ , $j=1,2,\cdots,p$ . 

实际问题中，用这样的离散谱序列描述具有多个频率成分叠加的观测数据时，经常取得较满意的结果。下页图1.8.2是离散谱序列(8.7)的80个观测值。 $p=5,\xi_{j},\eta_{j}$ 服从正态分布 $N(0,j^{2}),\lambda_{1}=0.12,\lambda_{2}=0.18,\lambda_{3}=0.7,\lambda_{4}=1.45,\lambda_{5}=1.97$ 。只从数据本身来看，周期性并不明显。 

最一般的离散谱序列由可列个简单的离散谱序列叠加而成. 设 $\xi_{j}, \eta_{k}(j, k=1,2,\cdots)$ 两两正交，满足(8.6). 对于角频率 $\lambda_{j} \in (0, \pi]$ ，当 
$\sum_ {j = 1} ^ {\infty} \sigma_ {j} ^ {2} <   \infty\tag{8.10}$
时，定义 
$Z _ {t} = \sum_ {j = 1} ^ {\infty} \left[ \xi_ {j} \cos (t \lambda_ {j}) + \eta_ {j} \sin (t \lambda_ {j}) \right], \quad t \in \mathbb {N} _ {+}.\tag{8.11}$
由于 $Z_{i}$ 可以改写成 




图1.8.2 ave=0.034, std=6.953

$Z _ {t} = \sum_ {j = 1} ^ {\infty} \left[ \sigma_ {j} \cos (t \lambda_ {j}) (\xi_ {j} / \sigma_ {j}) + \sigma_ {j} \sin (t \lambda_ {j}) / (\eta_ {j} / \sigma_ {j}) \right], \quad t \in \mathbb {N} _ {1},\tag{8.12}$
其中 $\{\xi_j / \sigma_j\}$ 和 $\{\eta_j / \sigma_j\}$ 是WN(0,1)，组合系数平方可和： 
$\sum_ {j = 1} ^ {\infty} \left[ (\sigma_ {j} \cos (t \lambda_ {j})) ^ {2} + (\sigma_ {j} \sin (t \lambda_ {j})) ^ {2} \right] = \sum_ {j = 1} ^ {\infty} \sigma_ {j} ^ {2} <   \infty .$
因而，级数(8.12)的右端均方收敛. 这样对任何 $t, s \leqslant 1$ ，由内积的连续性得到 
$\begin{array}{l}\mathrm{E} Z _ {t} = \mathrm{E} (Z _ {t} \cdot 1) = \lim _ {n \rightarrow \infty} \mathrm{E} \left[ \sum_ {j = 1} ^ {n} \xi_ {j} \cos (t \lambda_ {j}) + \eta_ {j} \sin (t \lambda_ {j}) \right] = 0,\\\mathrm{E} (Z _ {t} Z _ {s}) = \lim _ {n \rightarrow \infty} \mathrm{E} \left\{\sum_ {j = 1} ^ {n} [ \xi_ {j} \cos (t \lambda_ {j}) + \eta_ {j} \sin (t \lambda_ {j}) ] \right.\\\left. \cdot \sum_ {j = 1} ^ {n} [ \xi_ {j} \cos (s \lambda_ {j}) + \eta_ {j} \sin (s \lambda_ {j}) ] \right\}\\= \lim _ {n \rightarrow \infty} \sum_ {j = 1} ^ {n} \sigma_ {j} ^ {2} \cos [ (t - s) \lambda_ {j} ]\\= \sum_ {j = 1} ^ {\infty} \sigma_ {j} ^ {2} \cos [ (t - s) \lambda_ {j} ].\end{array}$
于是，由(8.11)定义的 $\{Z_{i}\}$ 是零均值平稳序列，由可列个正弦波和余弦波叠加构成，有自协方差函数 
$\gamma_ {k} = \sum_ {j = 1} ^ {\infty} \sigma_ {j} ^ {2} \cos (k \lambda_ {j}), \quad k \in \mathbb {Z}\tag{8.13}$
和谱函数 
$F (\lambda) = \sum_ {j = 1} ^ {\infty} \frac {\sigma_ {j} ^ {2}}{2} (I _ {[ - \lambda_ {j}, \pi ]} (\lambda) + I _ {[ \lambda_ {j}, \pi ]} (\lambda)).\tag{8.14}$
这是一个有可列个跳跃点的纯阶梯函数. 它在土 $\lambda$ , 有跳跃 $\sigma_{j}^{2}/2$ . 

注 (8.14)中如果有 $\lambda_{j} = \pi$ ，则相应的 
$(\sigma_ {j} ^ {2} / 2) [ I _ {[ - \lambda_ {j}, \pi ]} (\lambda) + I _ {[ \lambda_ {j}, \pi ]} (\lambda) ]$
应当改为 $\sigma_{j}^{2}I_{\{\pi\}}(\lambda)$ ，以保持右连续性. 

尽管离散谱序列是平稳序列,但是它的每一次观测是确定的三角函数的简单叠加.所以在实际工作中常常被人们视做非随机的处理.这时的模型称为调和模型(Harmonic Model). 

