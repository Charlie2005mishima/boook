
## chap 3 点估计
### 3.1 评价标准

 **【定义】**  点估计
设$\mathbf{X}=(X_{1},\dots X_{n})$是样本，统计量 $\hat{\theta} = \hat{\theta}(X_1,\dots,X_n)$ 称为参数 $\theta$ 的点估计. 一组具体的样本值$\mathbf{x}=(x_{1},\dots x_{n})$下估计量的值$\hat{g}(\mathbf{x})$称为$g(\theta)$的估计值

 **【定义】**  无偏性
若 $\mathbb{E}(\hat{\theta}) = \theta$ 对所有 $\theta \in \Theta$ 成立，则称 $\hat{\theta}$ 为无偏估计。  
偏差：$\text{Bias}_{\theta}(\hat{\theta}) = \mathbb{E}_{\theta}(\hat{\theta}) - \theta$。

**例**：$\bar{X}$ 是 $\mu$ 的无偏估计；$S^2 = \frac{1}{n-1}\sum (X_i-\bar{X})^2$ 是 $\sigma^2$ 的无偏估计。

 **【定义】**  渐近无偏
若 $\lim_{n\to\infty} \mathbb{E}(\hat{\theta}_n) = \theta$，则称 $\hat{\theta}_n$ 是渐近无偏估计。

 **【定义】**  有效性
对于无偏估计，方差越小越有效。若 $\text{Var}(\hat{\theta}_1) \le \text{Var}(\hat{\theta}_2)$ 且严格不等式对某些 $\theta$ 成立，则 $\hat{\theta}_1$ 比 $\hat{\theta}_2$ 有效。

 **【定义】**  均方误差
$\text{MSE}_{\theta}(\hat{\theta}) = \mathbb{E}[(\hat{\theta}-\theta)^2] = \text{Var}(\hat{\theta}) + \text{Bias}^2_{\theta}(\hat{\theta})$
 
 **【定义】**  相合性
- **弱相合**：$\hat{\theta}_n \xrightarrow{P} \theta$，即 $\forall \varepsilon>0,\ \lim_{n\to \infty} P(|\hat{\theta}_n-\theta|>\varepsilon)=0$。
- **强相合**：$\hat{\theta}_n \xrightarrow{\text{a.s.}} \theta$，即 $P(\lim_{n\to\infty}\hat{\theta}_n = \theta)=1$。

 **【定义】**  渐近正态性
若 $\sqrt{n}(\hat{\theta}_n - \theta) \xrightarrow{D} N(0,\sigma^2(\theta))$，则称 $\hat{\theta}_n$ 是渐近正态估计，$\sigma^2(\theta)$ 称为渐近方差。

### 3.2 矩法估计

 **【定义】**  矩法原理
用样本矩代替总体矩，解出参数估计。

**步骤**：
1. 计算总体前 $k$ 阶矩 $\mu_i = \mathbb{E}(X^i) = g_i(\theta_1,\dots,\theta_k)$。
2. 反解 $\theta_j = h_j(\mu_1,\dots,\mu_k)$。
3. 用样本矩 $\hat{\mu}_i = \frac{1}{n}\sum_{j=1}^n X_j^i$ 代入得 $\hat{\theta}_j = h_j(\hat{\mu}_1,\dots,\hat{\mu}_k)$。

**例**：$X \sim \text{Exp}(\lambda)$，$\lambda>0$。  $\mu_1 = 1/\lambda \Rightarrow \lambda = 1/\mu_1$，故矩估计 $\hat{\lambda} = 1/\bar{X}$。  其期望 $\mathbb{E}(1/\bar{X}) = \frac{n}{n-1}\lambda$，有偏，但渐近无偏。

**例**：$X \sim U(a,b)$，$a<b$。  $\mu_1 = \frac{a+b}{2},\quad \mu_2 = \mathbb{E}(X^2) = \frac{a^2+ab+b^2}{3}$。  也可用方差：$\sigma^2 = \frac{(b-a)^2}{12}$。解得 $a = \mu_1 - \sqrt{3\sigma^2},\quad b = \mu_1 + \sqrt{3\sigma^2}$  用样本均值和样本方差 $S_n^2 = \frac{1}{n}\sum (X_i-\bar{X})^2$ 代入得矩估计：$\hat{a} = \bar{X} - \sqrt{3S_n^2},\quad \hat{b} = \bar{X} + \sqrt{3S_n^2}$
 
 **※【定义】** 相合渐近正态性
 $\mathbf{X}$是总体$\{ f(x,\theta),\theta\in\Theta \}$中的简单样本，$\hat{g}_{n}(\mathbf{X})$是$g(\theta)$的矩估计量. 若存在$A_{n}(\theta),B_{n}(\theta)$，其中$B_{n}(\theta)>0$. 当$n\to \infty$时，$\frac{\hat{g}_{n}(\mathbf{X})-A_{n}(\theta)}{B_{n}(\theta)} \stackrel{ \mathscr{L}}{ \longrightarrow}N(0,1)$  且 $\hat{g}_{n}(\mathbf{X})$为$g(\theta)$的弱相合估计，则称$\hat{g}_{n}(\mathbf{X})$为$g(\theta)$的相合渐近正态估计
 
 **【定理】** 矩估计的相合性
若总体 $k$ 阶矩存在，则样本 $k$ 阶矩是总体 $k$ 阶矩的强相合估计；若 $g$ 连续，则矩估计量是参数的相合估计。

 **【定理】** 矩估计的渐近正态性（Delta方法）
设 $\hat{\mu}_n = (\hat{\mu}_{1n},\dots,\hat{\mu}_{kn})$ 是总体前 $k$ 阶矩的样本矩向量，则 $\sqrt{n}(\hat{\mu}_n - \mu) \xrightarrow{D} N(0, \Sigma),\quad \Sigma_{ij} = \mu_{i+j} - \mu_i\mu_j$  若 $\theta = h(\mu)$ 可微，则 $\sqrt{n}(\hat{\theta}_n - \theta) \xrightarrow{D} N(0, \nabla h^\top \Sigma \nabla h)$
 
### 3.3 极大似然估计 (MLE)

 **【定义】**  似然函数
对于离散总体：$L(\theta;\tilde{x}) = \prod_{i=1}^n P_\theta(X_i = x_i)$。  
对于连续总体：$L(\theta;\tilde{x}) = \prod_{i=1}^n f(x_i;\theta)$。

 **【定义】**  MLE
$\hat{\theta}_{\text{MLE}}$ 满足 $L(\hat{\theta};\tilde{x}) = \sup_{\theta\in\Theta} L(\theta;\tilde{x})$。


**【步骤】** 通常取对数，求导，解似然方程；若支撑集依赖于参数，则用定义法直接最大化。

**例**：$X_1,\dots,X_n \stackrel{\text{i.i.d.}}{\sim} N(\mu,\sigma^2)$。  对数似然：$\ln L = -\frac{n}{2}\ln(2\pi) - \frac{n}{2}\ln\sigma^2 - \frac{1}{2\sigma^2}\sum (X_i-\mu)^2$ 解得 $\hat{\mu} = \bar{X}$，$\hat{\sigma}^2 = \frac{1}{n}\sum (X_i-\bar{X})^2$ 

**例**：$X_1,\dots,X_n \stackrel{\text{i.i.d.}}{\sim} U(0,\theta)$。  似然函数 $L(\theta) = \theta^{-n} I\{X_{(n)} \le \theta\}$，关于 $\theta$ 递减，故取 $\hat{\theta} = X_{(n)}$。

**例**：指数分布定数截尾试验：$n$ 个产品，观察前 $r$ 个失效时间 $X_{(1)},\dots,X_{(r)}$，密度 $f(x)=\lambda e^{-\lambda x}$。  似然函数：$L = \frac{n!}{(n-r)!} \lambda^r \exp\left\{ -\lambda\left( \sum_{i=1}^r X_{(i)} + (n-r)X_{(r)} \right) \right\}$ 解得 $\hat{\lambda} = \dfrac{r}{\sum_{i=1}^r X_{(i)} + (n-r)X_{(r)}}$。

**例**：双参数指数分布 $f(x;\mu,\theta) = \frac{1}{\theta}e^{-(x-\mu)/\theta},\ x\ge\mu$。  似然 $L = \theta^{-n} e^{-\frac{1}{\theta}\sum (X_i-\mu)}$，$\mu$ 越大似然越大，故 $\hat{\mu}=X_{(1)}$，然后 $\hat{\theta} = \bar{X} - X_{(1)}$。

 **【定理】** 不变性
若 $\eta = q(\theta)$ 是一一变换，则 $\hat{\eta} = q(\hat{\theta})$ 是 $\eta$ 的 MLE。

### 3.4 一致最小方差无偏估计

**※【定义】** 一致最小均方误差估计
若存在 $\hat{g}^*(x)$，使得对 $g(\theta)$ 的任一估计量 $\hat{g}(x)$，有 $\mathbb{E}_\theta(\hat{g}^*(x) - g(\theta))^2 \le \mathbb{E}_\theta(\hat{g}(x) - g(\theta))^2,\quad \forall\theta\in\Theta$ 则称 $\hat{g}^*(x)$ 为一致最小均方误差估计。

但这样的估计通常不存在（因为允许有偏）。

**※【定义】** 一致最小方差无偏估计 UMVUE
若 $\hat{g}^*(x)$ 是 $g(\theta)$ 的无偏估计，且对任一无偏估计 $\hat{g}(x)$ 有 $\operatorname{Var}_\theta(\hat{g}^*(x)) \le \operatorname{Var}_\theta(\hat{g}(x)),\quad \forall\theta\in\Theta$ 则称 $\hat{g}^*(x)$ 为一致最小方差无偏估计。

**※【定理】** Rao-Blackwell定理：对于充分统计量的条件期望一定是更好的改进
设 $T = T(X)$ 是充分统计量，$\hat{g}(X)$ 是 $g(\theta)$ 的一个无偏估计。令 $h(T) = \mathbb{E}[\hat{g}(X) \mid T]$，则：
- $h(T)$ 是 $g(\theta)$ 的无偏估计，
- $\operatorname{Var}_\theta(h(T)) \le \operatorname{Var}_\theta(\hat{g}(X))$，等号成立当且仅当 $P_\theta(\hat{g}(X) = h(T)) = 1$。

**例**：$X_i \sim B(1,p)$，$T = \sum X_i$ 是充分统计量，$\hat{g}(X) = X_1$ 是无偏估计，则 $h(T) = \mathbb{E}[X_1 \mid T] = \frac{T}{n} = \bar{X}$ 是更优的无偏估计。

**※【定理】** 从零无偏估计出发的UMVUE的充要条件
设 $\hat{g}(X)$ 是 $g(\theta)$ 的一个无偏估计，且 $\operatorname{Var}_\theta(\hat{g}(X)) < \infty$。令 $\mathcal{U} = \{ U(X) : \mathbb{E}_\theta[U(X)] = 0,\ \forall\theta\in\Theta \}$ 为零无偏估计的集合。则 $\hat{g}(X)$ 是 UMVUE 当且仅当 $\operatorname{Cov}_\theta(\hat{g}(X), U(X)) = 0,\quad \forall\theta\in\Theta,\ \forall U\in\mathcal{U}.$ 

**例**：$X_i \sim B(1,p)$，证明 $\bar{X}$ 是 $p$ 的 UMVUE。

**※【推论】** 因为充分统计量的改进一定更好，所以UMVUE一定是充分统计量的函数
设 $T=T(X)$ 是充分统计量，$h(T)$ 是 $g(\theta)$ 的一个方差有限的无偏估计，$\mathcal{U}_T$ 是基于 $T$ 的零无偏估计的集合。则 $h(T)$ 是 UMVUE 当且仅当 $\mathbb{E}_\theta[h(T) \cdot \delta(T)] = 0,\quad \forall\theta\in\Theta,\ \forall\delta(T)\in\mathcal{U}_T.$ 

**※【定义】** 完全分布族 完全统计量
设 $T = T(X)$ 是一个统计量，其分布族为 $\{q(t;\theta):\theta\in\Theta\}$。若对任一函数 $\varphi(t)$，由 $\mathbb{E}_\theta[\varphi(T)] = 0,\quad \forall\theta\in\Theta$ 可推出 $P_\theta(\varphi(T)=0)=1,\ \forall\theta\in\Theta$，则称 $T$ 为**完全统计量**（或分布族是完全的）。
**注**：完全性依赖于分布族，且不一定一致。

**例**：二项分布族 $B(n,p),\ 0<p<1$ 是完全的。

**例**：正态分布族 $N(0,\sigma^2),\ \sigma>0$ 是不完全的（例如 $\varphi(x)=x^2-\sigma^2$ 期望为0但不恒为0）。

**例**：Gamma 分布族 $\Gamma(\omega,\lambda),\ \lambda>0$ 是完全的（利用 Laplace 变换唯一性）。

**例**：$X_1,\dots,X_n \sim N(\mu,\sigma^2)$，$\mu,\sigma$ 均未知，则 $(\bar{X}, S^2)$ 是完全统计量（利用 Laplace 变换唯一性）。

**例**：$X_1,\dots,X_n \sim U(\theta-\frac12,\theta+\frac12)$，则 $T = (X_{(1)},X_{(n)})$ 是充分统计量，但不完全。因为 $\psi(T) = X_{(n)} - X_{(1)} - c_n,\quad c_n = \mathbb{E}(X_{(n)}-X_{(1)})$ 满足 $\mathbb{E}_\theta[\psi(T)] = 0$ 但 $\psi(T)$ 不恒为零。

**※【定理】** Basu定理
设总体分布族为 $\{p(x;\theta):\theta\in\Theta\}$，$\tilde{X}=(X_1,\dots,X_n)$ 是样本。若 $T=T(\tilde{X})$ 是完备充分统计量，且随机变量 $V=V(\tilde{X})$ 的分布与 $\theta$ 无关，则 $T$ 与 $V$ 独立。

| 分布                            | 概率密度                                                                                  | 参数                     | 完备充分统计量                                               |
| ----------------------------- | ------------------------------------------------------------------------------------- | ---------------------- | ----------------------------------------------------- |
| **伯努利分布**$Bernoulli(p)$       | $p^x(1-p)^{1-x}$                                                                      | $p\in(0,1)$            | $T=\sum_{i=1}^n X_i$                                  |
| **二项分布**$Binomial(m,p)$       | $\binom{m}{x}p^x(1-p)^{m-x}$                                                          | $p\in(0,1)$            | $T=\sum_{i=1}^n X_i$                                  |
| **泊松分布**$Poisson(\lambda)$    | $\frac{e^{-\lambda}\lambda^x}{x!}$                                                    | $\lambda>0$            | $T=\sum_{i=1}^n X_i$                                  |
| **几何分布**$Geometric(p)$        | $(1-p)^{x-1}p$                                                                        | $p\in(0,1)$            | $T=\sum_{i=1}^n X_i$                                  |
| **负二项分布**$NB(r,p)$            | $\binom{x-1}{r-1}p^r(1-p)^{x-r}$                                                      | $p\in(0,1)$            | $T=\sum_{i=1}^n X_i$                                  |
| **正态分布**$N(\mu,\sigma^2)$     | $\frac{1}{\sqrt{2\pi\sigma^2}}e^{-\frac{(x-\mu)^2}{2\sigma^2}}$                       | $\mu\in\mathbb{R}$     | $T=\bar{X}=\frac{1}{n}\sum X_i$                       |
| **正态分布**$N(\mu,\sigma^2)$     | 同上                                                                                    | $\sigma^2>0$           | $T=\sum(X_i-\mu)^2$                                   |
| **正态分布**$N(\mu,\sigma^2)$     | 同上                                                                                    | $(\mu,\sigma^2)$       | $(T_1,T_2)=\left(\sum X_i,\sum X_i^2\right)$          |
| **指数分布**$Exp(\lambda)$        | $\lambda e^{-\lambda x},\ x>0$                                                        | $\lambda>0$            | $T=\sum_{i=1}^n X_i$                                  |
| **指数分布**$Exp(\theta)$         | $\frac{1}{\theta}e^{-x/\theta},\ x>0$                                                 | $\theta>0$             | $T=\sum_{i=1}^n X_i$                                  |
| **均匀分布**$U(0,\theta)$         | $\frac{1}{\theta},\ 0<x<\theta$                                                       | $\theta>0$             | $X_{(n)}$                                             |
| **均匀分布**$U(\theta,\theta+1)$  | $1,\theta<x<\theta+1$                                                                 | $\theta\in \mathbb{R}$ | $(X_{(1)},X_{(n)})$ 不完备捏                              |
| **伽马分布**$Gamma(\alpha,\beta)$ | $\frac{\beta^\alpha}{\Gamma(\alpha)}x^{\alpha-1}e^{-\beta x}$                         | $\beta>0$              | $T=\sum_{i=1}^n X_i$                                  |
| **伽马分布**$Gamma(\alpha,\beta)$ | 同上                                                                                    | $\alpha>0$             | $T=\sum_{i=1}^n \ln X_i$                              |
| **伽马分布**$Gamma(\alpha,\beta)$ | 同上                                                                                    | $(\alpha,\beta)$       | $(T_1,T_2)=\left(\sum X_i,\sum \ln X_i\right)$        |
| **贝塔分布**$Beta(\alpha,\beta)$  | $\frac{\Gamma(\alpha+\beta)}{\Gamma(\alpha)\Gamma(\beta)}x^{\alpha-1}(1-x)^{\beta-1}$ | $\alpha>0$             | $T=\sum \ln X_i$                                      |
| **贝塔分布**$Beta(\alpha,\beta)$  | 同上                                                                                    | $\beta>0$              | $T=\sum \ln(1-X_i)$                                   |
| **贝塔分布**$Beta(\alpha,\beta)$  | 同上                                                                                    | $(\alpha,\beta)$       | $(T_1,T_2)=\left(\sum \ln X_i,\sum \ln(1-X_i)\right)$ |


### 3.5 充分完全统计量

**※【定理】** Lehmann-Scheffé 定理  ：充分完备的无偏统计量一定是UMVUE的
设 $X\sim\{f(x;\theta):\theta\in\Theta\}$, $g(\theta)$ 定义于 $\Theta$, $T(X)$ 为充分完备统计量, $\hat{g}(T(X))$ 为 $g(\theta)$ 的一个无偏估计, 则 $\hat{g}(T(\tilde{X}))$ 是 $g(\theta)$ 的唯一 UMVUE; 或 $\mathbb{E}(\psi(X)\mid T)$ 也是 UMVUE, 其中 $\psi(X)$ 为无偏估计.

**例** $X\sim B(1,p)$, $T=\sum_{i=1}^n X_i$ 是 $p$ 的充分完备统计量, 求 $p$ 的 UMVUE.  

**例** 若是 $p^2$ 呢? 
$\begin{align*}\mathbb{E}T^2 &= \sum_{i=1}^n \mathbb{E}X_i^2 + \sum_{i\neq j}\mathbb{E}X_i\mathbb{E}X_j = \mathbb{E}T + n(n-1)p^2\\\Rightarrow p^2 &= \frac{\mathbb{E}T^2 - T^2}{n(n-1)}.\end{align*}$ 


**例** $X\sim \text{Poisson}(\lambda)$, $\lambda>0$ 未知, 求 (1) $\lambda$ 的 UMVUE; (2) $P(X=k)=\frac{\lambda^k}{k!}e^{-\lambda}$ 的 UMVUE.  
解: $p(\tilde{X};\lambda)=e^{-n\lambda}\lambda^{\sum x_i}(x_1!\cdots x_n!)^{-1}I\{X\in\mathbb{N}\}$, 故 $T_n=\sum_{i=1}^n X_i$ 为充分统计量.  
$\mathbb{E}_\lambda f(T_n)=e^{-n\lambda}\sum_{t=0}^\infty f(t)\frac{(n\lambda)^t}{t!}=0\ \forall\lambda>0$, 此为 $\lambda$ 的多项式, 故 $f(t)=0\ \forall t$, 从而 $P_\lambda\{f(T_n)=0\}=1$, 完备.  
又 $\mathbb{E}[T]=n\lambda$, 故 $\bar{X}$ 为 $\lambda$ 的 UMVUE.  
(3) $P(X=k)=\mathbb{E}_\lambda[I\{X_1=k\}]$, 故 $\psi(\tilde{X})=I\{X_1=k\}$, 则  
$\mathbb{E}[I\{X_1=k\}\mid T_n=t]=\frac{P(X_1=k,T_n=t)}{P(T_n=t)}=\binom{t}{k}\left(\frac1n\right)^k\left(1-\frac1n\right)^{t-k}.$ 

**例** $X_i\sim U(0,\theta)$, $\theta>0$ 未知, $n\ge2$, 求 $\theta$ 的 UMVUE.  
解: $X_{(n)}$ 是充分统计量, $X_{(n)}\sim p(t;\theta)=\frac{n}{\theta^n}t^{n-1}I_{(0,\theta)}(t)$.  
$\mathbb{E}_\theta f(T)=\int_0^\theta f(t)\frac{n}{\theta^n}t^{n-1}dt=0\ \forall\theta$, 对 $\theta$ 求导得 $f(\theta)\frac{n}{\theta^n}\theta^{n-1}=0\Rightarrow f(\theta)=0\ \forall\theta$, 故完备.  
$\mathbb{E}X_{(n)}=\frac{n}{n+1}\theta$, 故 $\frac{n+1}{n}X_{(n)}$ 是 $\theta$ 的 UMVUE.

**※【定理】**  
若 $p(\tilde{X};\theta)=c(\theta)\exp\left\{\sum_{j=1}^k Q_j(\theta)T_j(\tilde{X})\right\}h(\tilde{X})$, 且 $(Q_1(\theta),\dots,Q_k(\theta))$ 的值域有非空内点, 则 $T=(T_1,\dots,T_k)$ 为充分完备统计量.

**例** $\Gamma(\alpha,\lambda)$, $\lambda$ 未知, $\alpha$ 已知, 求 $\lambda$ 的 UMVUE.  
解: $p(\tilde{X};\lambda)=\frac{\lambda^{n\alpha}}{(\Gamma(\alpha))^n}\exp\{-\lambda\sum x_i\}\prod x_i^{\alpha-1}$, 由上述定理知 $\sum X_i$ 是充分完备统计量. 又 $\mathbb{E}[\sum X_i]=n\alpha/\lambda$, 故 $\frac{n\alpha-1}{\sum X_i}$ 是 $1/\lambda$ 的 UMVUE? 实际常用 $\frac{n\alpha-1}{\sum X_i}$ 估计 $\lambda$ 有偏, 需调整. (此处笔记不完整, 仅记录.)

### 3.6 Cramer-Rao 不等式  

**※【定义】** C-R正则分布族 C-R正则条件 Fisher信息量
若 $\mathcal{F}=\{ f(x,\theta) \}$ 满足下列条件
(1) 参数空间 $\Theta$ 是直线上的某个开区间
(2) $\forall\,x\in \mathscr{X},\theta\in\Theta,f(x,\theta)>0$，即分布族具有共同支撑
(3) $\forall\,x\in \mathscr{X},\theta\in\Theta,\frac{ \partial f(x,\theta) }{ \partial \theta }$存在
(4) $\frac{ \partial  }{ \partial \theta }\int f(x,\theta)dx=\int \frac{ \partial  }{ \partial \theta }f(x,\theta)$ 
(5) $0<I(\theta)=\mathbb{E}_{\theta}\left[ \frac{ \partial \ln f(x,\theta) }{ \partial \theta } \right]^{2}<\infty$ 
则称该分布族为C-R正则分布族，$I(\theta)$为该分布的Fisher信息量
注：若$\frac{d^{2}}{d\theta^{2}}\int_{-\infty}^{+\infty} p(x;\theta) \, dx=\int_{-\infty}^{+\infty} \frac{ \partial  }{ \partial \theta }[p(x;\theta)\frac{ \partial \ln p(x;\theta)}{ \partial \theta }] \, dx$ 故


**※【定理】** Cramer-Rao 不等式  
设总体分布族 $\mathcal{F}=\{ f(x,\theta) \}$ 是 C-R 正则分布族， $g(\theta)$ 是定义于 $\Theta$ 上的可微函数， $\tilde{X}$ 是来自 $f(x,\theta)$ 的简单随机样本， $\hat{g}(\tilde{X})$ 是 $g(\theta)$ 的任一无偏估计，且满足可交换积分与导数, 则 $\operatorname{Var}_\theta[\hat{g}(\tilde{X})] \ge \frac{[g'(\theta)]^2}{n I(\theta)},\quad \forall\theta\in\Theta,$  其中 $I(\theta)=\mathbb{E}_\theta\left[\left(\frac{\partial\log f(X;\theta)}{\partial\theta}\right)^2\right]$.  

注：尽管验证正则条件(1)-(5)和条件(6)非常麻烦，但是幸运的是对指数族来说，上述条件(1)-(6)均成立. 但要注意，$\operatorname{Var}_\theta[\hat{g}(\tilde{X})]$达不到C-R下界不能得出$g(\theta)$的UMVUE就不存在, 

**※【推论】** C-R不等式等号成立的条件
(1) 若样本分布组非指数族，任何$g(\theta)$的任何无偏估计，其方差不能处处达到C-R不等式中的下界
(2) 及时样本分布族为指数族$f(\tilde{x},\theta)=C(\theta)\exp \{ Q(\theta)T(\tilde{x}) \}h(\tilde{x})$，也不是对每一$g(\theta)$均能找到无偏估计$\hat{g}(X)$，其方差处处达到C-R不等式中的下界. 唯有在$g(\theta)=E_{\theta}(aT(\tilde{X})+b)$时才有. 

**※【定义】** 有效估计和估计的效率
设 $\hat{g}(\tilde{X})$ 为 $g(\theta)$ 的无偏估计，比值 $e_{\hat{g}}(\theta)=\frac{[g'(\theta)]^{2}/(nI(\theta))}{\operatorname{Var}_\theta[\hat{g}(\tilde{X})]}$ 成为无偏估计 $\hat{g}(\tilde{X})$ 的效率，当$e_{\hat{g}}(\theta)=1$ 时，称 $\hat{g}(\tilde{X})$ 为 $g(\theta)$ 的有效估计. 若 $\hat{g}(\tilde{X})$ 不是 $g(\theta)$ 的有效估计，但 $\lim_{ n \to \infty }e_{\hat{g}}(\theta)=1$ ，则称 $\hat{g}(\tilde{X})$ 为 $g(\theta)$ 的渐进有效估计.
### 3.7 Bayes 点估计法

**※【定义】** 先验分布
参数空间$\Theta$上$\theta$的任意概率分布称为先验分布，记作$\pi(\theta)$

**※【定义】** 后验分布
获得样本$\tilde{X}$后，$\theta$的后验分布记作$\pi(\theta|\tilde{x})=\frac{f(\tilde{x},\theta)}{m(\tilde{x})}=\frac{f(\tilde{x}|\theta)\pi(\theta)}{\int_{\Theta}f(\tilde{x}|\theta)\pi(\theta)d\theta}$ ，条件密度$f(\tilde{x}|\theta)$，先验密度$\pi(\theta)$，边缘密度$m(\tilde{x})$

**※【定义】** Bayes点估计
众数型：将基于$\pi(\theta|\tilde{x})$的众数作为$\theta$的估计
中位数型：将基于$\pi(\theta|\tilde{x})$的中位数作为$\theta$的估计
期望：将基于$\pi(\theta|\tilde{x})$的期望作为$\theta$的估计

**※【定义】** 共轭先验分布
后验分布与先验分布属于同一分布族

**※【定义】** Bayes预测
$z$的后验预测密度函数$\hat{p}_{Z|T}(z|t)=\int_{\Theta}g(z|\theta)\pi(\theta|t)d\theta$

**※【定义】** 广义先验密度
样本$\tilde{X}\sim p(\tilde{x}|\theta)$ 若 $\pi(\theta)$ 满足
(1) $\pi(\theta)\geq0$ 且 $\int_{\Theta}\pi(\theta)d\theta=\infty$
(2) 后验$\pi(\theta|\tilde{x})=\frac{f(\tilde{x}|\theta)\pi(\theta)}{\int_{\Theta}f(\tilde{x}|\theta)\pi(\theta)d\theta}$ 是正常密度函数

**※【推论】** 位置参数的无信息先验
设总体$X\sim f(x-\theta),-\infty<\theta<\infty$，$\theta$为位置参数.
即对$X$做平移变换$Y=X+c$，同时对$\theta$也作同样的平移变换$\eta=\theta+c$，显然$Y\sim f(y-\eta)$，$\eta$位置参数.
所以$(X,\theta)$与$(Y,\eta)$的统计结构相同. 因此主张它们有相同的无信息先验是合理的. 
这样$\theta,\eta$有相同的分布，从而$\theta$的先验分布不依赖于原点的选择，它在等长度区间内的先验概率应当一样.
所以可取 $\pi(\theta)=1$ 它是一个广义先验密度

**※【推论】** 尺度参数的无信息先验
设总体$X\sim \frac{g(x/\sigma)}{\sigma},\sigma>0$为尺度单数. 
即对$X$左尺度变换$Y=cX(c>0)$，同时对$\sigma$也作相同的尺度变换$\eta=c\sigma$，容易知道$Y\sim \frac{g(x /\eta)}{\eta}$.
所以$(X,\theta)$与$(Y,\eta)$的统计结构相同. 因此主张它们有相同的无信息先验是合理的.
故可取$\pi(\sigma)=\frac{1}{\sigma},\sigma>0$，它是一个广义先验密度. 