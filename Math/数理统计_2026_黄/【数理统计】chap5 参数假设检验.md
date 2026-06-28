## 5.1 假设检验的若干基本概念

### （一）什么是假设检验

假设检验的做法：
1. 建立假设
2. 构造检验统计量
3. 确定拒绝域和接受域
4. 据给定的显著性水平来确定临界点
5. 做判断

假设检验的做法：
1. 建立假设
2. 构造检验统计量
3. 确定拒绝域和接受域
4. 据检验统计量给出p值大小
5. 做判断

### （二）假设

原假设，备择假设

### （三）检验

检验函数
$\varphi(\tilde{x})=\begin{cases}1, & \tilde{x}\in D \\ 0, & \tilde{x}\in\bar{D}\end{cases}$ 

### （四）两类错误与势函数

第一类错误（弃真） $\alpha(\theta)=P(\tilde{X}\in D|H_{0})$ 
第二类错误（存伪） $\beta(\theta)=P(\tilde{X}\in \bar{D}|H_{1})$ 

势函数 $g(\theta)=P_{\theta}(\tilde{X}\in D), \theta\in\Theta$ 

### （五）Neyman-Pearson原则与显著性水平为a的检验

显著性水平为$\alpha$的检验 $\sup_{\theta\in\Theta_{0}}\alpha(\theta)\leq\alpha$ 

## 5.2 正态分布总体参数的假设检验
### U检验
### t检验
### $\chi^{2}$检验
### F检验

## 5.3 假设检验与区间估计 

### 一、假设检验与区间估计的对偶关系

1. 基本思想
- 假设检验与区间估计在本质上互为“逆运算”。
- 一个显著性水平为 $\alpha$ 的检验的接受域，可以构造出置信水平为 $1-\alpha$ 的置信集；反之，一个置信水平为 $1-\alpha$ 的置信集，也可导出相应的检验接受域。
- 这为构造置信区间提供了一种系统方法，同时也让检验结论与区间估计结论保持一致（若 $\theta_0$ 落在置信区间内，则接受 $H_0:\theta=\theta_0$）。

2. 从检验到置信集
- 设对每个 $\theta_0\in\Theta$，$A(\theta_0)$ 为检验问题 $H_0:\theta=\theta_0 \leftrightarrow H_1:\theta\neq\theta_0$ 的显著性水平 $\alpha$ 的接受域。
- 对每个样本 $\tilde{x}$，定义$C(\tilde{x}) = \{\theta_0 : \tilde{x}\in A(\theta_0)\}.$
- 则 $C(\tilde{X})$ 是参数 $\theta$ 的置信水平为 $1-\alpha$ 的置信集。

1. 从置信集到检验
- 反之，设 $C(\tilde{X})$ 是 $\theta$ 的置信水平为 $1-\alpha$ 的置信集。
- 对任意 $\theta_0$，定义$A(\theta_0)=\{\tilde{x}:\theta_0\in C(\tilde{x})\}.$
- 则 $A(\theta_0)$ 即为检验问题 $H_0:\theta=\theta_0 \leftrightarrow H_1:\theta\neq\theta_0$ 的显著性水平 $\alpha$ 的接受域。

### 二、单参数指数型分布族的单调性性质

1. 指数型分布族的联合密度（或分布列）形式
设总体 $X$ 属于单参数指数型分布族，样本 $\tilde{X}=(X_1,\dots,X_n)$ 的联合密度（或分布列）可写为
$$
p(\tilde{x};\theta)=C^*(\theta)\exp\{Q(\theta)U(\tilde{x})\}h^*(\tilde{x}),
$$
其中 $Q(\theta)$ 是 $\theta$ 的**严格增函数**，$U(\tilde{x})$ 为充分统计量（通常取完备最小充分统计量）。

2. 核心定理（单调性）
- 若 $\psi(U)$ 是 $U$ 的非降函数，则 $E_\theta[\psi(U(\tilde{X}))]$ 也是 $\theta$ 的**非降函数**。

1. 推论（尾部概率的单调性）
- 对于任意常数 $C$，
  - $P_\theta\{U(\tilde{X})>C\}$ 是 $\theta$ 的**非降函数**；
  - $P_\theta\{U(\tilde{X})<C\}$ 是 $\theta$ 的**非增函数**。
- 这为单侧检验提供理论依据：若 $\theta$ 增大，则 $U$ 取大值的概率增大，取小值的概率减小。

### 三、单参数指数型分布族的假设检验

1. 检验问题类型
设 $\theta_0$ 为已知常数，检验：
- (1) 左侧检验：$H_0:\theta\ge\theta_0 \leftrightarrow H_1:\theta<\theta_0$
- (2) 右侧检验：$H_0:\theta\le\theta_0 \leftrightarrow H_1:\theta>\theta_0$
- (3) 双侧检验：$H_0:\theta=\theta_0 \leftrightarrow H_1:\theta\neq\theta_0$

2. 检验统计量与拒绝域形式
- 通常取 $U(\tilde{X})$（充分统计量）作为检验统计量。
- 由于 $E_\theta U$ 是 $\theta$ 的非降函数，拒绝域形式为：
  - 左侧检验：$\{U(\tilde{x})<C\}$
  - 右侧检验：$\{U(\tilde{x})>C\}$
  - 双侧检验：$\{U(\tilde{x})<C_1 \text{ 或 } U(\tilde{x})>C_2\}$

1. 临界值的确定
- 左侧检验：$\sup_{\theta\ge\theta_0}P_\theta(U<C)=P_{\theta_0}(U<C)$，令其等于 $\alpha$ 求 $C$。
- 右侧检验：$\sup_{\theta\le\theta_0}P_\theta(U>C)=P_{\theta_0}(U>C)$，令其等于 $\alpha$ 求 $C$。
- 双侧检验：分别令两侧尾部概率为 $\alpha/2$，即 $P_{\theta_0}(U<C_1)=\alpha/2$，$P_{\theta_0}(U>C_2)=\alpha/2$。
- 因此临界值仅需在 $\theta=\theta_0$ 下计算 $U$ 的分布（或分位数）。

### 四、具体分布下的假设检验（指数分布、二点分布、泊松分布）

1. 指数分布
- 样本联合密度：$p(\tilde{x};\theta)=\theta^{-n}\exp\{-\frac{1}{\theta}\sum x_i\}.$属于指数型，$U=\sum X_i$，$E(U)=n\theta$，单调增。
- 利用 $2n\bar{X}/\theta \sim \chi^2(2n)$（因为 $2\sum X_i/\theta \sim \chi^2(2n)$）。

- **左侧检验** $H_0:\theta\ge\theta_0 \leftrightarrow H_1:\theta<\theta_0$：
  - 拒绝域 $\{2n\bar{x}/\theta_0 < \chi_{1-\alpha}^2(2n)\}$。

- **右侧检验** $H_0:\theta\le\theta_0 \leftrightarrow H_1:\theta>\theta_0$：
  - 拒绝域 $\{2n\bar{x}/\theta_0 > \chi_{\alpha}^2(2n)\}$。

- **双侧检验** $H_0:\theta=\theta_0 \leftrightarrow H_1:\theta\neq\theta_0$：
  - 拒绝域 $\{2n\bar{x}/\theta_0 < \chi_{1-\alpha/2}^2(2n) \text{ 或 } > \chi_{\alpha/2}^2(2n)\}$。

2. 二点分布（$X\sim B(1,p)$，$0<p<1$）
- 联合分布列：$p(\tilde{x};p)=(1-p)^n \exp\{\ln\frac{p}{1-p}\sum x_i\}.$属于指数型，$U=\sum X_i\sim B(n,p)$，且 $\ln\frac{p}{1-p}$ 关于 $p$ 严格增。

- 检验统计量取 $U=\sum X_i$（或 $\bar X$），拒绝域由二项分布尾部概率确定。

- **左侧检验** $H_0:p\ge p_0 \leftrightarrow H_1:p<p_0$：
  - 拒绝域 $\{U\le C'\}$，其中$C'=\max\{C\in\mathbb{N}: \sum_{j=0}^{C}\binom{n}{j}p_0^j(1-p_0)^{n-j}\le \alpha\}.$

- **右侧检验** $H_0:p\le p_0 \leftrightarrow H_1:p>p_0$：
  - 拒绝域 $\{U\ge C'\}$，其中$C'=\min\{C\in\mathbb{N}: \sum_{j=C}^{n}\binom{n}{j}p_0^j(1-p_0)^{n-j}\le \alpha\}.$

- **双侧检验** $H_0:p=p_0 \leftrightarrow H_1:p\neq p_0$：
  - 拒绝域 $\{U\le C_1' \text{ 或 } U\ge C_2'\}$，其中 $C_1'$ 满足左侧累积概率 $\le\alpha/2$ 的最大整数，$C_2'$ 满足右侧累积概率 $\le\alpha/2$ 的最小整数。

- **大样本近似**：当 $n$ 充分大时，可利用中心极限定理，采用 U 检验，统计量$\frac{\sum X_i - np_0}{\sqrt{np_0(1-p_0)}} \overset{\text{近似}}{\sim} N(0,1).$

3. 泊松分布（$X\sim P(\lambda)$，$\lambda>0$）
- 联合分布列：$p(\tilde{x};\lambda)=\frac{e^{-n\lambda}\lambda^{\sum x_i}}{\prod x_i!}=e^{-n\lambda}\exp\{(\ln\lambda)\sum x_i\}\cdot \frac{1}{\prod x_i!}.$属于指数型，$U=\sum X_i\sim P(n\lambda)$，$E(U)=n\lambda$ 单调增。
- 检验统计量取 $T=\sum X_i$。
- **右侧检验** $H_0:\lambda\le\lambda_0 \leftrightarrow H_1:\lambda>\lambda_0$：
  - 拒绝域 $\{T\ge C'\}$，其中$C'=\min\{C\in\mathbb{N}: \sum_{j=C}^{\infty}\frac{(n\lambda_0)^j}{j!}e^{-n\lambda_0}\le \alpha\}.$
  - 也可利用 $\chi^2$ 近似：当 $T=C$ 时，$P_{\lambda_0}(T\ge C)=\chi^2(2n\lambda_0;2C)$，故 $C'$ 为满足 $2n\lambda_0 \le \chi_{1-\alpha}^2(2C)$ 的最小整数。

- **左侧检验** $H_0:\lambda\ge\lambda_0 \leftrightarrow H_1:\lambda<\lambda_0$：
  - 拒绝域 $\{T\le C'\}$，其中 $C'$ 为满足 $2n\lambda_0 \ge \chi_{\alpha}^2(2(C+1))$ 的最大整数。

- **双侧检验** $H_0:\lambda=\lambda_0 \leftrightarrow H_1:\lambda\neq\lambda_0$：
  - 拒绝域 $\{T\le C_1' \text{ 或 } T\ge C_2'\}$，其中 $C_1'$ 由左侧尾部 $\alpha/2$ 确定，$C_2'$ 由右侧尾部 $\alpha/2$ 确定。

- **大样本近似**：当 $n$ 充分大时，可用$\frac{T-n\lambda_0}{\sqrt{n\lambda_0}} \overset{\text{近似}}{\sim} N(0,1)$进行 U 检验。

## 5.4 广义似然比检验

### 1 广义似然比检验

**※【定义】** 广义似然比
$\lambda(\tilde{x})=\frac{\sup_{\theta\in\Theta}L(\theta;\tilde{x})}{\sup_{\theta\in\Theta_{0}}L(\theta;\tilde{x})}=\frac{L(\hat{\theta};\tilde{x})}{L(\hat{\theta_{0}};\tilde{x})}$ 

###  2 分布的似然比检验

$H_{0}:p(x;\theta)=p_{0}(x;\theta)\leftrightarrow H_{1}:p(x;\theta)=p_{1}(x;\theta)$ 

### 3 大样本似然比检验

**※【定理】** LRT的渐近分布，简单原假设情形
$H_{0}:\theta=\theta_{0}\leftrightarrow H_{1}:\theta\not=\theta_{0}$，当$p(x;\theta)$满足适当的正则条件，
则在$H_{0}$下，$2\log\lambda(\tilde{X}) \stackrel{ D}{ \longrightarrow}\chi^{2}(1).n\to \infty$ 

**※【定理】** LRT的渐近分布，复杂原假设情形
$H_{0}:\theta\in\Theta_{0}\leftrightarrow H_{1}:\theta\not\in\Theta_{0}$，当$p(x;\theta)$满足适当的正则条件，
则在$H_{0}$下，$2\log\lambda(\tilde{X}) \stackrel{ D}{ \longrightarrow}\chi^{2}(\upsilon).n\to \infty$ ，其中$\upsilon=\dim\Theta-\dim\Theta_{0}$ 

## 检验的p值

**※【定义】** p 值
p 值：在原假设 $H_0$ 为真的前提下，检验统计量取其观察值或更极端（更不利于 $H_0$）值的概率。

- p 值的计算原则
	1. 单侧检验
		- 右侧检验（$H_1: \theta > \theta_0$）：$p = P_{H_0}(T \ge t_{\text{obs}}).$ 
		- 左侧检验（$H_1: \theta < \theta_0$）：$p = P_{H_0}(T \le t_{\text{obs}}).$ 
	2. 双侧检验
		- $H_1: \theta \ne \theta_0$。
		- 拒绝域分散在两侧，p 值为两侧尾部概率之和。
		- 若统计量 $T$ 的分布关于某点对称，则$p = 2 \times \min\bigl\{P_{H_0}(T \ge t_{\text{obs}}),\, P_{H_0}(T \le t_{\text{obs}})\bigr\},$  
		- 或等价地，$p = 2 \times P_{H_0}(T \ge |t_{\text{obs}}|)$ 
		- 若分布不对称（如 $F$ 分布），则需根据实际观察值所在位置，取较小尾部概率的两倍，使得总概率不超过 1。


- 假设检验的一般步骤

- 传统步骤（固定 $\alpha$）
1. 提出 $H_0$ 和 $H_1$。
2. 选择检验统计量 $T$，确定拒绝域的形式（依赖于 $H_1$）。
3. 选取 $\alpha$，求临界值，使 $\sup_{H_0} P(\text{拒绝})\le \alpha$（连续型通常取等号）。
4. 由样本计算统计量值 $t_{\text{obs}}$，判断是否落入拒绝域，得出拒绝或不拒绝的结论。

- 基于 p 值的步骤（更现代）
1. 提出 $H_0$ 和 $H_1$。
2. 选择检验统计量 $T$，并确定拒绝域形式。
3. 由样本计算统计量值 $t_{\text{obs}}$，并计算 p 值（按对应分布求尾部概率）。
4. 将 p 值与预设的 $\alpha$ 比较，若 $p\le\alpha$ 则拒绝 $H_0$，否则不拒绝。

- 特殊情形：离散分布下的 p 值

- 当检验统计量服从离散分布（如二项分布）时，p 值定义为“至少与观测值一样极端”的概率。
- 例：$X\sim B(n,\theta)$，检验 $H_0:\theta\le\theta_0$ vs $H_1:\theta>\theta_0$，观测到 $X=t_0$，则
  $$
  p = P_{\theta_0}(X \ge t_0) = \sum_{k=t_0}^{n} \binom{n}{k}\theta_0^k(1-\theta_0)^{n-k}.
  $$
- 此时 p 值不一定是连续均匀分布，但决策规则同样适用。
