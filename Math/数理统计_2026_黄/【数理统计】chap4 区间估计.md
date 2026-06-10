
## 4.1 区间估计的基本概念

### 一、区间估计

**※【定义】** 区间估计
$\theta\in\Theta \subset \mathbb{R}$ 是待估参数，设 $\hat{\theta}_{L}=\hat{\theta}_{L}(\tilde{X})$ 和 $\hat{\theta}_{U}=\hat{\theta}_{U}(\tilde{X})$ 是参数空间 $\Theta$ 上取值的样本 $\tilde{X}$ 的两个统计量且满足$\hat{\theta}_{L}(\tilde{X})\leq\hat{\theta}_{U}(\tilde{X})$，则称$[\hat{\theta}_{L}(\tilde{X}),\hat{\theta}_{U}(\tilde{X})]$ 为参数的区间估计.
注：因为$\hat{\theta}_{L}(\tilde{X}),\hat{\theta}_{U}(\tilde{X})$是随机变量，所以应当说是估计区间涵盖了$\theta$，而不是$\theta$落在了区间

### 二、区间估计的评价准则

**※【定义】** 涵盖概率、置信度 $P_{\theta}\{ \theta\in [\hat{\theta}_{L}(\tilde{X}),\hat{\theta}_{U}(\tilde{X})]\}$ 

**※【定义】** 置信系数 $\inf_{\theta\in\Theta}P_{\theta}\{ \theta\in [\hat{\theta}_{L}(\tilde{X}),\hat{\theta}_{U}(\tilde{X})]\}$ 

**※【定义】** 精确度 $\mathbb{E}_{\theta}\{ \hat{\theta}_{U}(\tilde{X})-\hat{\theta}_{L}(\tilde{X})\}$ 

**※【定义】** 置信区间 给定$\alpha\in(0,1)$，参数$\theta$的置信系数不低于$1-\alpha$的区间估计，称其为$\theta$的置信水平为$1-\alpha$的置信区间. 即$P_{\theta}\{ \theta\in [\hat{\theta}_{L}(\tilde{X}),\hat{\theta}_{U}(\tilde{X})]\}\geq 1-\alpha,\forall\,\theta\in\Theta$ 

**※【定义】** 同等双侧置信区间 
$[\hat{\theta}_{L}(\tilde{X}),\hat{\theta}_{U}(\tilde{X})]$ 为参数$\theta$的一个区间估计. 假设给定$\alpha\in(0,1)$，有$\inf_{\theta\in\Theta}P_{\theta}\{ \theta\in [\hat{\theta}_{L}(\tilde{X}),\hat{\theta}_{U}(\tilde{X})]\}=1-\alpha$，则$[\hat{\theta}_{L}(\tilde{X}),\hat{\theta}_{U}(\tilde{X})]$是$\theta$的置信水平为$1-\alpha$的同等双侧置信区间 

**※【定义】** 单侧置信上限、下限
给定$\alpha$，$P_{\theta}\{ \theta\in [-\infty,\hat{\theta}_{U}(\tilde{X})]\}\geq 1-\alpha,\forall\,\theta\in\Theta$ 则称$\hat{\theta}_{U}(\tilde{X})$为$\theta$的置信水平$1-\alpha$的单侧置信上限
$P_{\theta}\{ \theta\in [\hat{\theta}_{L}(\tilde{X}),\infty]\}\geq 1-\alpha,\forall\,\theta\in\Theta$ 则称$\hat{\theta}_{U}(\tilde{X})$为$\theta$的置信水平$1-\alpha$的单侧置信下限

**※【引理】** 
$\hat{\theta}_{U}(\tilde{X})$分别是$1-\alpha_{1},1-\alpha_{2}$的单侧置信下上限且$\forall\,\tilde{X}$，均有$\hat{\theta}_{L}(\tilde{X})\leq\hat{\theta}_{U}(\tilde{X})$，则$[\hat{\theta}_{L}(\tilde{X}),\hat{\theta}_{U}(\tilde{X})]$为$1-\alpha_{1}-\alpha_{2}$

**※【定义】** 置信域
待估$\theta=(\theta_{1},\theta_2,\dots,\theta_{k})\in\Theta \subset \mathbb{R}^k$ 样本$\tilde{X}$

## 4.2 枢轴变量法——正态总体参数的置信分布
**※【定义】** 枢轴量
形式$G(\tilde{X},\theta)$：样本+待估；$G$的分布不依赖于未知参数

**※【定理】** 步骤
1. 构造枢轴量
2. 对给定$\alpha$选择$c,d$满足$P_{\theta}\{ c\leq G(\tilde{X},\theta)\leq d \}=1-\alpha$
3. 转化为$P_{\theta}\{ \hat{\theta}_{L}(\tilde{X})\leq\theta\leq\hat{\theta}_{U}(\tilde{X}) \}=1-\alpha$ 
	1. 最优置信区间：$\min \mathbb{E}_{\theta}[\hat{\theta}_{u}(\tilde{X})-\hat{\theta}_{L}(\tilde{X})]$ 
	2. 等尾置信区间：$P_{\theta}\{  G(\tilde{X},\theta)<c \}=P_{\theta}\{  G(\tilde{X},\theta)>d \}=\frac{\alpha}{2}$ 

**※【定理】** 正态总体参数的置信区间

| 已知信息                                      | 目标信息                                      | 枢轴量                                                                                                                                                                              |
| ----------------------------------------- | ----------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| $\sigma^2$                                | $\mu$                                     | $\frac{\bar{X}-\mu}{\sigma / \sqrt{ n }}\sim N(0,1)$                                                                                                                             |
|                                           | $\mu$                                     | $\sqrt{ n(n-1) } \frac{\bar{X}-\mu}{Q}\sim t(n-1)$                                                                                                                               |
| $\mu$                                     | $\sigma^2$                                | $\frac{Q^{2}}{\sigma^{2}}\sim \chi^{2}(n-1)$                                                                                                                                     |
|                                           | $\sigma^2$                                | $\frac{\sum_{i=1}^n(X_{i}-\mu)^{2}}{\sigma^{2}}\sim \chi^{2}(n)$                                                                                                                 |
|                                           | $(\mu,\sigma^{2})$                        | $(\frac{\bar{X}-\mu}{\sigma / \sqrt{ n }},\frac{Q^{2}}{\sigma^{2}})$                                                                                                             |
| $\sigma_{1},\sigma_{2}$                   | $\delta=\mu_{2}-\mu_1$                    | $(d-\delta) / \sqrt{ \sigma^{2}_{1} /n_{1}+ \sigma^{2}_{2} /n_{2}}\sim N(0,1)$                                                                                                   |
| $\sigma_{1}=\sigma_{2}$                   | $\delta=\mu_{2}-\mu_1$                    | $\sqrt{ \frac{n_{1}n_{2}(n_{1}+n_{2}-2)}{n_{1}+n_{2}} }(d-\delta) / \sqrt{ Q_{1}^{2}+Q_{2}^{2}}\sim t(n_{1}+n_{2}-2)$                                                            |
| $\sigma_{2}^{2} / \sigma_{1}^{2}=\lambda$ | $\delta=\mu_{2}-\mu_1$                    | $\sqrt{ \frac{n_{1}n_{2}(n_{1}+n_{2}-2)}{n_{1}\lambda+n_{2}} }(d-\delta) / \sqrt{ Q_{1}^{2}+Q_{2}^{2} /\lambda}\sim t(n_{1}+n_{2}-2)$                                            |
| $n_{1}=n_{2}$                             | $\delta=\mu_{2}-\mu_1$                    | $\sqrt{ n(n-1) } \frac{d-\delta}{Q}\sim t(n-1)$                                                                                                                                  |
| $m,n$充分大                                  | $\delta=\mu_{2}-\mu_1$                    | $(d-\delta) / \sqrt{ S^{2}_{1} /n_{1}+ S^{2}_{2} /n_{2}} \stackrel{ D}{ \longrightarrow}N(0,1)$                                                                                  |
| 一般情况                                      | 近似解                                       | $(d-\delta) / \sqrt{ S^{2}_{1} /n_{1}+ S^{2}_{2} /n_{2}}\sim t(l)$<br>$l=[S_{1}^{2} /n_{1}+S_{2}^{2} /n_{2}]^{2} /[S_{1}^{4} /n_{1}^{2}(n_{1}-1)+S_{2}^{4} /n_{2}^{2}(n_{2}-1)]$ |
| $\mu_{1},\mu_{2}$                         | $\sigma_{2}^{2} / \sigma_{1}^{2}=\lambda$ | $\frac{\sum_{i=1}^{n_{1}}(X_{i}-\mu_{1})^{2} /n_{1}\sigma^{2}_{1}}{\sum_{j=1}^{n_{2}}(Y_{j}-\mu_{2})^{2} /n_{2}\sigma^{2}}\sim F(n_{1},n_{2})$                                   |
|                                           | $\sigma_{2}^{2} / \sigma_{1}^{2}=\lambda$ | $F= \frac{Q_{1}^{2} /[\sigma_{1}^{2}(n_{1}-1)]}{Q_{2}^{2} /[\sigma^{2}_{2}(n_{2}-1)]}\sim F(n_{1}-1,n_{2}-1)$                                                                    |

## 4.3 枢轴变量法——非正态总体参数的置信分布

### 4.3.1 小样本方法
#### 1. 指数分布参数的置信区间
$2\lambda \sum_{i=1}^nX_{i}\sim \chi^2_{2n}$ 
#### 2. 均匀分布参数的置信区间
$\theta /X_{(n)}\sim g(z)=nz^{-(n+1)}I_{(1,\infty)}(z)$ 

### 4.3.2 大样本方法
#### 1. Cauchy 分布位置参数的置信区间
样本中位数：$m_{n}$
$\frac{2\sqrt{ n }(m_{n}-\theta)}{\pi} \stackrel{ \mathscr{L}}{ \longrightarrow}N(0,1)$ 
#### 2. 二项分布总体参数的置信区间
$S_{n}=\sum_{i=1}^nX_{i}\sim b(n,p)$
$\frac{S_{n}-np}{\sqrt{ np(1-p) }}= \frac{\sqrt{ n }(\bar{X}-p)}{\sqrt{ p(1-p) }} \stackrel{ \mathscr{L}}{ \longrightarrow}N(0,1),n\to \infty$ 
当n充分大时有 $P(|T|\leq u_{\alpha /2})=P\left( |\frac{\sqrt{ n }(\bar{X}-p)}{\sqrt{ p(1-p) }}|\leq u_{\alpha /2} \right)\approx 1-\alpha$ 
可以解得：$p^{2}(n+\gamma^{2})-p(2n\hat{p}+\gamma^{2})+n\hat{p}^{2}\leq{0}$ 
在实验中可以采用更简单的方法：$P\left( |\frac{\sqrt{ n }(\hat{p}-p)}{\sqrt{ \hat{p}(1-\hat{p}) }}|\leq u_{\alpha /2} \right)$ 
#### 3. Poisson分布参数的置信区间
$S_{n}=\sum_{i=1}^nX_{i}\sim P(n\lambda)$ 
当n充分大时，由中心极限定理：$\frac{S_{n}-n\lambda}{\sqrt{ n\lambda }}= \frac{\sqrt{ n }(\bar{X}-\lambda)}{\sqrt{ \lambda }}\stackrel{ \mathscr{L}}{ \longrightarrow}N(0,1),when\,n\to \infty$ 
故：$P(|\frac{\sqrt{ n }(\bar{X}-\lambda)}{\sqrt{ \lambda }}|\leq u_{\alpha /2})\approx1-\alpha$ 
同样可以求解：$(\hat{\lambda}-\lambda)^{2}=\frac{\lambda u^{2}_{\alpha /2}}{n}$ 
或者近似求解：$P(|\frac{\sqrt{ n }(\bar{X}-\lambda)}{\sqrt{ \hat{\lambda} }}|\leq u_{\alpha /2})\approx 1-\alpha$ 

### 4.3.3 寻找枢轴量的一般方法

$T$的分布$F(t;\theta)$，$F(T;\theta)\sim U(0,1)$为枢轴量

**※【定理】** 
统计量$T=T(\tilde{x})$的分布函数$G(t;\theta)\in C(t,\theta)$ 设$\alpha_{1}+\alpha_{2}=\alpha$ 
$1-G(t-0,\theta_{1}(t))=P_{\theta_{1}}\{ T\geq t \}=\alpha_{1}$ 
$G(t-0,\theta_{2}(t))=P_{\theta_{2}}\{ T\leq t \}=\alpha_{2}$ 
1. $G(t;\theta)$随$\theta$渐降，$[\theta_{1}(T),\theta_{2}(T)]$为同等置信区间
2. $G(t;\theta)$随$\theta$渐升，$[\theta_{2}(T),\theta_{1}(T)]$为同等置信区间

**※【引理】** 
设$F(x)$是$X$的分布函数，则$P\{ F(X)\leq y \}\leq y\leq P\{ F(X-0)<y \}\,\forall\,y\in(0,1)$ 
特别地，若$F(x)\in C$，则$F(x)\sim U(0,1)$ 

**※【定理】** 
统计量$T=T(\tilde{x})$的分布函数$G(t;\theta)\in C(t,\theta)$ 设$\alpha_{1}+\alpha_{2}=\alpha$ 
$1-G(t-0,\theta_{1}(t))=P_{\theta_{1}}\{ T\geq t \}=\alpha_{1}$ 
$G(t-0,\theta_{2}(t))=P_{\theta_{2}}\{ T\leq t \}=\alpha_{2}$ 
1. $G(t;\theta)$随$\theta$渐降，$[\theta_{1}(T),\theta_{2}(T)]$为置信区间
2. $G(t;\theta)$随$\theta$渐升，$[\theta_{2}(T),\theta_{1}(T)]$为置信区间
- 注：未必同等

## 4.4 Bayes Intervals

**※【定义】** 可信区间
参数$\theta$的后验分布$\pi(\theta|\tilde{x})$，对给定的$1-\alpha$，若存在$\hat{\theta}_{L}=\hat{\theta}_{L}(\tilde{x}),\hat{\theta}_{U}=\hat{\theta}_{U}(\tilde{x})$，使得$P(\hat{\theta}_{L}\leq\theta\leq\hat{\theta}_{U}|\tilde{x})\geq 1-\alpha$，则称区间为参数$\theta$的可信水平为$1-\alpha$的置信区间