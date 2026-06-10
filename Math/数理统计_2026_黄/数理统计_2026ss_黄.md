## 绪言
1. 数理统计最难的部分在第二章和第三章。而从第四章往后的内容很多都是比较公式化的内容，只要记住解题步骤，往上套就可以了；并且，后面的内容也是以二、三两章为基础。
2. 因此，想要学好数理统计，前半个学期肯定是要花时间下去的。 第二章其实并非数理统计的内容，而是有点像概率论的内容，主要讲述几大分布，和他们之间的关系。
3. 最后是考试的一点经验，数理统计考试可能会体量比较大，计算量也比较大（因为每道判断题都可以看成是大题），所以时间可能是来不及的，除了一些必要的计算外，应用题按计算器（往往是最后计算统计量为多少）可以放到后面，先写别的题目。
4. 而在考试中判断是否接受假设检验（可能会有一分判断分），可以不用计算，直接去做判断，因为数理统计考试出出来的表格不会刚好在假设检验的临界值（那样其实是无法判断），所以你可以肉眼看出来要不要接受假设检验。比如我们[今年最后一题](https://www.cc98.org/topic/6220669)，分布是 37 40 16 4 0 2 1 ，问你是否符合泊松分布，其实这一眼就能看出来肯定是泊松分布。
## chap1 绪论
### 1.2 基本概念
#### （一）总体 分布族
**※【定义】** 总体 样本 样本大小 抽样 有限总体 无限总体
> 总体由与研究问题有关的所有个体组成
> 样本按照一定方法抽样的一部分个体

**※【定义】** 总体的随机变量
> 总体可以用一个随机变量及其概率分布来描述

**※【定义】** 分布族：分布的集合 $F\subset \mathscr{F}$
例 $\varepsilon\sim N(0,\sigma^2),\,\mathscr{F}_{1}=\{ N(0,\sigma^2):\sigma>0 \}$
**※【定义】** 参数分布族 非参数分布族
**※【定义】** 参数空间
例 $N(a,\sigma^2),\,\Theta=\{ (a,\sigma):-\infty<a<\infty,\sigma>0 \}$
#### （二）样本
**※【定义】** 样本容量：抽样中抽取的个体的数目
例 样本$\tilde{X}=\{ X_{1},X_{2},\dots,X_{n} \}\implies$样本观察值$\tilde{x}=\{ x_{1},x_{2},\dots,x_{n} \}$
**※【定义】** 样本空间：样本的所有可能取值
#### （三）简单随机抽样
**※【定义】** 简单随机样本
> 有一总体F，$X_{1},X_{2},\dots,X_{n}$为从F中抽取的容量为n的样本. 若
> (i) $X_{1},\dots,X_{n}$同分布
> (ii) $X_{1},\dots,X_{n}$相互独立
> 则为简单随机样本

#### （四）从样本认识总体
**※【定义】** 经验分布函数
> 样本$\{ x_{1},x_{2},\dots,x_{n} \}$的经验分布函数$F_{n}(x)=\frac{1}{n}\#\{X_{i}:x_{i}\leq x,\,i=1,2,\dots,n\}$

**※【定理】** Glivenko-Cantelli定理
> 设样本$X_{1},X_{2},\dots,X_{n}$ iid F，$F_{n}(x)$为经验分布函数，则$P(\lim_{ n \to \infty }\sup_{x\in(-\infty,\infty)}|F_{n}(x)-F(x)|=0)=1$ 
### 1.3 date reduction - statistics 统计量
**※【定义】** 统计量
> 分布族$\mathcal{F}=\{F\},\,\tilde{X}=(X_{1},\dots,X_{n})$是一个样本. 若样本的实值函数$T=T(\tilde{X})=T(X_{1},\dots,X_{n})$不依赖于分布族，则称T为统计量. 对参数分布族而言，不依赖于未知参数$\theta$

**※【定义】** 抽样分布
> 统计量$T(\tilde{X})$的分布
> $\begin{align}F_{T}(t) & =P\{ T(\tilde{X})\leq t \} \\  & =P\{ (X_{1},X_{2},\dots,X_{n})\in B \}(B=\{(x_{1},\dots,x_{n}):T(x_{1},\dots,x_{n})\leq t  \}) \\  & =\int\dots \int_{B}dF(x_{1})dF(x_{2})\dots dF(x_{n})\end{align}$

**※【定义】** 常用统计量
> 样本均值 $\overline{X}=\frac{1}{n}\sum_{i=1}^nX_{i}$
> 样本方差 $S^{2}=\frac{1}{n-1}\sum_{i=1}^n(X_{i}-\overline{X})^{2}$
> 样本k阶原点矩 $a_{n,k}=\frac{1}{n}\sum_{i=1}^nX_{i}^k$
> 样本k阶中心矩 $m_{n,k}=\frac{1}{n}\sum_{i=1}^n(X_{i}-\overline{X})^k$
> 样本变异系数 $\frac{S_{n}}{\overline{X}}$
> 样本偏度 $\frac{\sqrt{ n }\sum_{i=1}^n(X_{i}-\overline{X})^{3}}{\left( \sum_{i=1}^n(X_{i}-\overline{X}^{2})^{3/2} \right)}$
> 样本峰度 $\frac{n\sum_{i=1}^n(X_{i}-\overline{X})^{4}}{\left( \sum_{i=1}^n(X_{i}-\overline{X})^{2} \right)^{2}}-3$ 

**※【定义】** 独立性和可积性
> 设$T_{1}$和$T_{2}$是分布族$\mathcal{F}=\{ F \}$上的两个统计量. 若对任意分布F，统计量$T_{1},T_{2}$相互独立称为两个统计量相互独立；若对任意分布F，统计量T是可积的，则统计量可积.

## chap2 抽样分布

### 2.2 正态总体样本均值和样本方差的分布

**※【引理】** 
> 随机变量$\tilde{X}=(X_{1},\dots,X_{n})',\,\tilde{Y}=(Y_{1},\dots,Y_{n})'$间有线性变换$\tilde{Y}=A\tilde{X}$，其中$A=(a_{ij})_{n\times n}$为方阵，则$E\tilde{Y}=A(E\tilde{X}),\,Var\tilde{Y}=A(Var\tilde{X})A'$

**※【定理】** 
> $X_{1},\dots ,X_{n}$取自$N(\mu,\sigma^{2})$. $\overline{X}$和$S^{2}$为样本均值和样本方差
> (1) $\overline{X}\sim N(\mu,\sigma^{2}/n)$
> (2) $\frac{(n-1)S^{2}}{\sigma^{2}}=\frac{nS_{n}^{2}}{\sigma^{2}}=\frac{1}{\sigma^{2}}\sum_{i=1}^n(X_{i}-\overline{X})^{2}\sim \chi^{2}(n-1)$
> (2)' $\frac{1}{\sigma^{2}}\sum_{i=1}^n(X_{i}-\mu)^{2}\sim \chi^{2}(n)$
> (3) $\overline{X}$与$S^{2}$独立

### 2.3 次序统计量及其分布
#### 一、次序统计量

**※【定义】** 次序统计量
> 样本$X_{1},\dots,X_{n}$按从小到大重排$X_{(1)}\leq\dots\leq X_{(n)}$，则$(X_{(1)},\dots,X_{(n)})$为次序统计量
#### 二、次序统计量的分布

> 1. $(X_{(1)},\dots,X_{(n)})$的联合密度函数$g(y_{1},\dots,y_{n})=n!p(y_{1})\dots p(y_{n})$ 
> 2. $X_{(n)},F^{n}(x)$ 
> 3. $X_{(1)},1-(1-F(x))^{n}$
> 4. $X_{(k)}, \frac{n!}{(k-1)!(n-k)!}p(y)F^{k-1}(y)(1-F(y))^{n-k}$
> 5. $(X_{(i)},X_{(j)}), \frac{n!}{(i-1)!(j-i-1)!(n-j)!}p(y_{i})p(y_{j})F^{i-1}(y_{i})(F(y_{j})-F(y_{i}))^{j-i-1}(1-F(y_{j}))^{n-j}$

#### 三、极差、样本中位数与分位数

样本极差 $R_{n}=X_{(n)}-X_{(1)}$ $p_{R_{n}}(r)=\int_{-\infty}^{+\infty } n(n-1)(F(r+z)-F(z))^{n-2} p(z)p(r+z)\, dz$
样本中位数 $m_{e}\begin{cases}X_{\left( \frac{n+1}{2} \right)} & \frac{n}{2}\not\in \mathbb{Z} \\ \left( X_{\left( \frac{n}{2} \right)}+X_{\left( \frac{n}{2}+1 \right)} \right)/2 & \frac{n}{2}\in\mathbb{Z}\end{cases}$
总体分位数 $F(\xi_{p})=p$
样本分位数 $F_{n}(a-0)=\frac{\#\{X_{i}<a  \}}{n}\leq p\leq F_{n}(a)=\frac{\#\{X_{i}\leq a  \}}{n}$

**※【定义】** 样本下侧p分位数
> $X_{1},\dots,X_{n}$是取自总体$F(x)$的一个样本 称$m_{p}=X_{[np]}+(n+1)\left( p-\frac{[np]}{n+1} \right)$

**※【定理】** 样本下侧p分位数的近似分布

###  2.4 常用分布与分布族

**【定义】** Gamma分布
> 设 $\omega > 0, \lambda > 0$，称随机变量 $X$服从参数为 $\omega, \lambda$的 Gamma 分布，若其概率密度函数为：$p(x; \omega, \lambda) = \frac{\lambda^\omega}{\Gamma(\omega)} x^{\omega - 1} e^{-\lambda x}, \quad x > 0$ 其中 $\Gamma(\omega) = \int_0^{+\infty} t^{\omega - 1} e^{-t} dt$为 Gamma 函数

**【定理】** Gamma分布的性质
> 1. Gamma 分布的 $k$阶矩为：$\mathbb{E} X^k = \frac{\Gamma(\omega + k)}{\lambda^k \Gamma(\omega)} = \frac{\omega(\omega+1)\cdots(\omega+k-1)}{\lambda^k}$
> 2. Gamma分布的矩母函数：$\mathbb{E} e^{tX} = \left(1 - \frac{t}{\lambda}\right)^{-\omega}, \quad t < \lambda$
> 3. Gamma分布的可加性：设 $X_1 \sim \Gamma(\omega_1, \lambda)$，$X_2 \sim \Gamma(\omega_2, \lambda)$相互独立，则：$X_1 + X_2 \sim \Gamma(\omega_1 + \omega_2, \lambda)$
> 4. Gamma分布的尺度变换：设 $X \sim \Gamma(\omega, \lambda)$，则对任意 $k > 0$，有：$Y = \frac{X}{k} \sim \Gamma(\omega, k\lambda)$
>> 注：$\Gamma(1, \lambda)$即指数分布 $\text{Exp}(\lambda)$。$2\lambda X\sim \chi^{2}(2\omega)$  
>> $\Gamma(n, \lambda)$可视为 $n$个独立同分布指数变量之和
>> $\Gamma\left(\frac{1}{2}, \frac{1}{2}\right)$即为 $\chi^2(1)$分布

**【定义】** chi^2分布
> 设 $X_1, X_2, \dots, X_n \overset{\text{i.i.d.}}{\sim} N(0, 1)$，定义：$\chi^2(n) = \sum_{i=1}^n X_i^2$则称其服从自由度为 $n$的 $\chi^2$分布，记为 $\chi^2(n)$。

**【定理】** chi^2分布的性质
> q. 若 $\chi^2(m)$与 $\chi^2(n)$独立，则：$\chi^2(m) + \chi^2(n) \sim \chi^2(m + n)$

**【定义】** 非中心chi^2分布
> 设 $X_i \sim N(\mu_i, 1)$相互独立，定义：$\chi^2(n, \lambda) = \sum_{i=1}^n X_i^2, \quad \lambda = \sum_{i=1}^n \mu_i^2$其中 $\lambda$为非中心参数。

**【定理】** 非中心chi^2分布的性质
> 1. 矩母函数：$\mathbb{E} e^{t\chi^2(n, \lambda)} = (1 - 2t)^{-n/2} \exp\left( \frac{\lambda t}{1 - 2t} \right), \quad t < \frac{1}{2}$
> 2. 可加性：若 $\chi^2(n_1, \lambda_1)$与 $\chi^2(n_2, \lambda_2)$独立，则：$\chi^2(n_1, \lambda_1) + \chi^2(n_2, \lambda_2) \sim \chi^2(n_1 + n_2, \lambda_1 + \lambda_2)$
> 3. 期望与方差：$\mathbb{E} \chi^2(n, \lambda) = n + \lambda, \quad \text{Var}(\chi^2(n, \lambda)) = 2(n + 2\lambda)$

**【定义】** t 分布
> 设 $Z \sim N(0, 1)$，$U \sim \chi^2(n)$相互独立，定义：$T = \frac{Z}{\sqrt{U / n}} \sim t(n)$

**【定理】** t 分布的性质
> 1. 当 $n \to \infty$时，$t(n) \overset{d}{\to} N(0, 1)$。


**【定义】** 非中心 t 分布
> 1. 设 $Z \sim N(\delta, 1)$，$U \sim \chi^2(n)$相互独立，定义：$T = \frac{Z}{\sqrt{U / n}} \sim t(n, \delta)$

**【定义】** F 分布
> 设 $U \sim \chi^2(m)$，$V \sim \chi^2(n)$相互独立，定义：$F = \frac{U / m}{V / n} \sim F(m, n)$

**【定理】** F 分布的性质
> 若 $F \sim F(m, n)$，则 $1/F \sim F(n, m)$。
> 若 $T \sim t(n)$，则 $T^2 \sim F(1, n)$。

**【定义】** 非中心 F 分布
> 设 $U \sim \chi^2(m, \lambda)$，$V \sim \chi^2(n)$相互独立，定义：$F = \frac{U / m}{V / n} \sim F(m, n, \lambda)$


**【定义】** 上 $\alpha$分位点
> 设 $X$的分布函数为 $F(x)$，若：$P(X \ge \chi_\alpha) = \int_{\chi_\alpha}^\infty f(x) dx = \alpha$则称 $\chi_\alpha$为 $X$的 上 $\alpha$分位点。常用关系：$t_{1-\alpha}(n) = -t_\alpha(n), \quad F_{1-\alpha}(m, n) = \frac{1}{F_\alpha(m, n)}$

**【定义】** Beta 分布
> 设 $a > 0, b > 0$，称随机变量 $X$服从 Beta 分布 $\text{Be}(a, b)$，若其概率密度函数为：$p(x; a, b) = \frac{\Gamma(a+b)}{\Gamma(a)\Gamma(b)} x^{a-1} (1-x)^{b-1}, \quad x \in (0, 1)$

**【定理】** Beta 分布的性质
> 1. 矩：$\mathbb{E} X^k = \frac{B(a+k, b)}{B(a, b)} = \frac{a(a+1)\cdots(a+k-1)}{(a+b)(a+b+1)\cdots(a+b+k-1)}$
> 2. 当 $a = b = 1$时，$\text{Be}(1, 1) \sim U(0, 1)$。
> 3. 次序统计量与 Beta 分布：设 $X_1, \dots, X_n \overset{\text{i.i.d.}}{\sim} U(0, 1)$，则第 $k$个次序统计量 $X_{(k)} \sim \text{Be}(k, n - k + 1)$


**【定义】** Fisher Z 分布（Beta Ⅱ型）
设 $a > 0, b > 0$，称随机变量 $X$服从 Fisher Z 分布 $\mathcal{Z}(a, b)$，若其概率密度函数为：$p(x; a, b) = \frac{\Gamma(a+b)}{\Gamma(a)\Gamma(b)} \frac{x^{a-1}}{(1+x)^{a+b}}, \quad x > 0$

**【定理】** Fisher Z 分布的性质
1. 矩：$\mathbb{E} X^k = \frac{B(a+k, b-k)}{B(a, b)}, \quad k < b$
2. 若 $Y \sim \text{Be}(a, b)$，则：$X = \frac{Y}{1-Y} \sim \mathcal{Z}(a, b)$
3. 若 $X_1 \sim \Gamma(\alpha_1, \lambda)$，$X_2 \sim \Gamma(\alpha_2, \lambda)$独立，则：$\frac{X_1}{X_2} \sim \mathcal{Z}(\alpha_1, \alpha_2), \quad \frac{X_1}{X_1 + X_2} \sim \text{Be}(\alpha_1, \alpha_2)$
4. 若 $F \sim F(m, n)$，则：$\frac{m}{n} F \sim \mathcal{Z}\left(\frac{m}{2}, \frac{n}{2}\right), \quad \frac{mF}{n + mF} \sim \text{Be}\left(\frac{m}{2}, \frac{n}{2}\right)$

### 2.5 指数型分布族

**【定义】** 指数型分布族
分布族：$\mathcal{F} = \left\{ p(x; \theta) = c(\theta) \exp\left( \sum_{j=1}^k \theta_j T_j(x) \right) h(x) : \theta \in \Theta \subset \mathbb{R}^k \right\}$称为 **指数型分布族**。当 $k = 1$时称为 **单参数指数族**。

**【定理】** 指数型分布族的性质
1. 支撑集与参数 $\theta$无关。
2. 具有较好的解析性质（如矩母函数存在、可微性等）。

### 2.6 充分统计量

**【定义】** 充分统计量
统计量 $T(X)$称为充分统计量，若在给定 $T = t$的条件下，样本 $X$的条件分布与总体分布中的参数 $\theta$无关。

**【定理】** 充分统计量的变换： 若 $T$ 是充分统计量，则其一一变换 $S = f(T)$ 也是充分统计量。

**【定理】** 因子分解定理
统计量 $T(X)$是充分统计量当且仅当样本的联合概率函数可分解为：$p(x; \theta) = g(T(x); \theta) \cdot h(x)$其中 $g(t; \theta)$仅依赖于 $t$和 $\theta$，$h(x)$与 $\theta$无关。

**例**：$X_1,\dots,X_n \stackrel{\text{i.i.d.}}{\sim} U(0,\theta)$，$\theta>0$。  联合密度为$p(\tilde{x};\theta) = \theta^{-n} I\{0 \le X_{(1)} \le X_{(n)} \le \theta\}$ 取 $g(t;\theta) = \theta^{-n} I\{t \le \theta\}$，$t = X_{(n)}$，$h(\tilde{x}) = I\{0 \le X_{(1)} \le X_{(n)}\}$，故 $X_{(n)}$ 是充分统计量。

**例**：$X_1,\dots,X_n \stackrel{\text{i.i.d.}}{\sim} N(\mu,\sigma^2)$，$\mu,\sigma^2$ 未知。  联合密度为 $p(\tilde{x};\mu,\sigma^2) = (2\pi\sigma^2)^{-n/2} \exp\left\{ -\frac{1}{2\sigma^2}\left( \sum x_i^2 - 2\mu\sum x_i + n\mu^2 \right) \right\}$  由因子分解定理，$\left( \sum X_i, \sum X_i^2 \right)$ 是充分统计量。又 $(\bar{X}, S^2)$ 是其一一变换，故也是充分统计量。

**【定义】**  极小充分统计量
充分统计量 $T$ 称为极小充分统计量，如果对任意其他充分统计量 $S$，存在函数 $f$ 使得 $T = f(S)$ 几乎必然成立。


**※【定理】** 
假定存在统计量$T(\mathbf{x})$，使得会每两个样本点$\mathbf{x},\mathbf{y}$，比值$f(\mathbf{x},\boldsymbol{\theta})/f(\mathbf{y},\boldsymbol{\theta})$ 为与$\theta$ 无关的常数的充要条件为$T(\mathbf{x})=T(y)$且$T(\mathbf{x})$是充分统计量，则$T(\mathbf{x})$为$\theta$极小充分统计量，其中$\theta$为一维或多维参数向量

**【定理】** 
总体为指数型分布族. 若自然参数空间 $\Theta \subset \mathbb{R}^k$ 具有非空内部，则 $T = (T_1,\dots,T_k)$ 是极小充分统计量。
