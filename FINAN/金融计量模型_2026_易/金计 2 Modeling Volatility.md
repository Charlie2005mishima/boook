> **要点**：波动率模型研究条件方差的动态变化。ARCH 利用过去冲击的平方解释波动率，GARCH 进一步引入过去的条件方差。建模过程包括模型识别、ARCH 效应检验、极大似然估计、残差诊断及波动率预测。
> 
> #时间序列 #Volatility #ARCH #GARCH #金融计量

## §1 波动率(Volatility)

### §1.1 均值方程与条件信息

考虑时间序列的均值方程(mean equation)：$y_{t+1}=\mu_{t+1}+\varepsilon_{t+1}$,
其中，$\mu_{t+1}$ 为时刻 $t$ 对 $y_{t+1}$ 的一步预测：$\mu_{t+1}=\phi_0+\sum_{j=1}^p\phi_jy_{t+1-j}+\sum_{j=1}^q\beta_j\varepsilon_{t+1-j}$.
$\varepsilon_{t+1}$ 为时刻 $t$ 无法预测的随机冲击。

记 $\mathcal F_t$ 为截至时刻 $t$ 的信息集。此前通常假设 ${\varepsilon_t}$ 为满足条件均值独立的白噪声(white noise)：
- $E(\varepsilon_t)=0$；
- $\operatorname{Var}(\varepsilon_t)=\sigma^2$，无条件方差为常数；
- $E(\varepsilon_t\mid\mathcal F_{t-1})=0$。

在实践中，随着信息集更新，条件统计量通常比无条件统计量更重要。

### §1.2 条件波动率

**※【定义】** 条件波动率(conditional volatility)
在给定过去信息集 $\mathcal F_{t-1}$ 时，$y_t$ 的条件标准差称为条件波动率，记为 $\sigma_t$：
$\sigma_t^2=\operatorname{Var}(y_t\mid\mathcal F_{t-1})=\operatorname{Var}(\varepsilon_t\mid\mathcal F_{t-1})$. (1)
其中 $\sigma_t^2$ 为条件方差(conditional variance)，(1) 称为波动率方程(volatility equation)。

条件波动率主要应用于：
1. 衍生品定价，例如 Black–Scholes 期权定价公式。
2. 风险管理，例如风险价值(Value at Risk, VaR) 和预期损失(Expected Shortfall, ES)。
3. 资产配置，例如最小方差投资组合。
4. 提高参数估计效率，例如可行广义最小二乘法(feasible generalized least squares, FGLS)。
5. 改进区间预测，提供随时间变化的预测标准误。

**※【性质】** 无条件方差恒定不意味着条件方差恒定。
即使 $\operatorname{Var}(\varepsilon_t)=\sigma^2$ 不随时间变化，$\operatorname{Var}(\varepsilon_t\mid\mathcal F_{t-1})$ 仍可能随过去的信息变化。

![[Pasted image 20261123192504.png|500]]
图：1950 年 1 月 3 日至 2008 年 4 月 11 日标普 500 指数的日变动。市场存在平静期与剧烈波动期，体现波动聚集(volatility clustering)现象。

## §2 单变量波动率模型(Univariate Volatility Models)

### §2.1 ARCH 模型的构造

由条件均值为零的假设：$E(\varepsilon_t\mid\mathcal F_{t-1})=0$, 可得：$\sigma_t^2=E(\varepsilon_t^2\mid\mathcal F_{t-1})$.

一种自然的建模方法是令平方冲击 $\varepsilon_t^2$ 满足自回归形式：$\varepsilon_t^2=\alpha_0+\alpha_1\varepsilon_{t-1}^2+\cdots+\alpha_q\varepsilon_{t-q}^2+\eta_t$,
其中：$\eta_t=\varepsilon_t^2-E(\varepsilon_t^2\mid\mathcal F_{t-1})$, 满足 $E(\eta_t\mid\mathcal F_{t-1})=0$。

对上述方程取条件期望：$\sigma_t^2=\alpha_0+\alpha_1\varepsilon_{t-1}^2+\cdots+\alpha_q\varepsilon_{t-q}^2$. (2)

**※【定义】** 自回归条件异方差模型(autoregressive conditional heteroskedasticity, ARCH)
若条件方差满足 (2)，则称为 ARCH(q) 模型.
其核心思想是：当前条件方差由过去 $q$ 期的平方冲击决定.

### §2.2 ARCH(1) 模型

**※【定义】** ARCH(1) 模型
$y_t=E(y_t\mid\mathcal F_{t-1})+\varepsilon_t$, $\varepsilon_t=\sigma_tv_t$, $\sigma_t^2=\alpha_0+\alpha_1\varepsilon_{t-1}^2$.
其中：
- $\alpha_0>0$；
- $0\leq\alpha_1<1$；
- ${v_t}$ 为独立同分布(independent and identically distributed, i.i.d.)随机变量序列；
	- $E(v_t)=0$，$\operatorname{Var}(v_t)=1$；
	- $v_t$ 与过去信息集 $\mathcal F_{t-1}$ 独立。
$v_t$ 可以服从标准正态分布、标准化 Student-$t$ 分布、广义误差分布(generalized error distribution, GED)及其偏斜形式。

**※【性质】** ARCH(1) 的参数约束
$\alpha_0>0$ 和 $\alpha_1\geq0$ 保证条件方差为正；$\alpha_1<1$ 保证存在具有有限二阶矩的平稳解。
当 $\alpha_1>0$ 时，较大的过去冲击平方会提高下一期条件方差，从而产生波动聚集。

### §2.3 ARCH(q) 模型

**※【定义】** ARCH(q) 模型
$y_t=E(y_t\mid\mathcal F_{t-1})+\varepsilon_t$, $\varepsilon_t=\sigma_tv_t$, $\sigma_t^2=\alpha_0+\sum_{i=1}^q\alpha_i\varepsilon_{t-i}^2$.
参数条件为：$\alpha_0>0,\quad \alpha_i\geq0,\quad i=1,\ldots,q$.
保证有限无条件方差的条件为：$\sum_{i=1}^q\alpha_i<1$.
这里讨论的是平稳解的存在性，并不意味着从任意初始条件出发，各期无条件方差立即相等。

### §2.4 ARCH 误差的性质

**※【性质】** ARCH 误差的条件均值为零。

**证明**
由 $\varepsilon_t=\sigma_tv_t$，且 $\sigma_t$ 关于 $\mathcal F_{t-1}$ 可测，得到：$E(\varepsilon_t\mid\mathcal F_{t-1})=\sigma_tE(v_t\mid\mathcal F_{t-1})=0$.
利用全期望公式：$E(\varepsilon_t)=E[E(\varepsilon_t\mid\mathcal F_{t-1})]=0$.

**※【性质】** ARCH 误差序列不具有线性自相关。
对任意 $j\geq1$：$E(\varepsilon_t\varepsilon_{t-j})=E[E(\varepsilon_t\varepsilon_{t-j}\mid\mathcal F_{t-1})]$, $=E[\varepsilon_{t-j}E(\varepsilon_t\mid\mathcal F_{t-1})]=0$.
因此，在二阶矩有限时：$\operatorname{Cov}(\varepsilon_t,\varepsilon_{t-j})=0,\quad j\geq1$.

**※【性质】** ARCH(q) 平稳解的无条件方差
若 $\sum_{i=1}^q\alpha_i<1$，且过程处于有限二阶矩平稳状态，则：$\operatorname{Var}(\varepsilon_t)=E(\sigma_t^2)=\frac{\alpha_0}{1-\sum_{i=1}^q\alpha_i}$.

**证明**
由于 $E(v_t^2)=1$ 且 $v_t$ 与过去信息独立：$E(\varepsilon_t^2)=E(\sigma_t^2v_t^2)=E(\sigma_t^2)$.
代入条件方差方程：$E(\varepsilon_t^2)=\alpha_0+\sum_{i=1}^q\alpha_iE(\varepsilon_{t-i}^2)$.
对于平稳解，各期二阶矩相等。令 $v=E(\varepsilon_t^2)$，则：$v=\alpha_0+v\sum_{i=1}^q\alpha_i$,
解得：$v=\frac{\alpha_0}{1-\sum_{i=1}^q\alpha_i}$.
以上计算确定了平稳解的方差，但不能单独证明平稳解存在。

### §2.5 ARCH 的非线性依赖与波动聚集

**※【性质】** ARCH 误差序列可以不相关，但并不独立。
虽然：$\operatorname{Cov}(\varepsilon_t,\varepsilon_{t-j})=0$,
但：$E(\varepsilon_t^2\mid\mathcal F_{t-1})=\alpha_0+\sum_{i=1}^q\alpha_i\varepsilon_{t-i}^2$,
通常依赖过去的平方冲击。

因此，收益率本身的自相关函数(autocorrelation function, ACF)可能接近零，而平方收益率的 ACF 仍表现出明显的相关性。
ARCH 模型通过这种非线性依赖刻画金融数据中的平静期与高波动期。

### §2.6 厚尾(Heavy Tails)

**※【定义】** 峰度(kurtosis)
对具有有限四阶矩的随机变量 $Y$，定义：$\operatorname{Kurt}(Y)=\frac{E[(Y-EY)^4]}{[\operatorname{Var}(Y)]^2}$.
例如：
- 正态分布的峰度为 $3$；
- 自由度 $\nu>4$ 的 Student-$t$ 分布峰度为 $3+\frac{6}{\nu-4}$。

**※【定义】** 超额峰度(excess kurtosis)
$\operatorname{ExKurt}(Y)=\operatorname{Kurt}(Y)-3$.
正的超额峰度表明相对于正态分布具有更高的峰度，通常用于描述金融收益率的尖峰厚尾特征。

**※【性质】** ARCH(1) 的无条件分布可以具有厚尾。
考虑：$\varepsilon_t=\sigma_tv_t,\,\sigma_t^2=\alpha_0+\alpha_1\varepsilon_{t-1}^2$, 其中 $v_t\overset{i.i.d.}{\sim}N(0,1)$。
若 $0\leq\alpha_1^2<1/3$，则：$\frac{E(\varepsilon_t^4)}{[\operatorname{Var}(\varepsilon_t)]^2}=3\frac{1-\alpha_1^2}{1-3\alpha_1^2}$.
当 $\alpha_1>0$ 时，上式严格大于 $3$；当 $\alpha_1=0$ 时等于 $3$。

**证明**
正态扰动满足 $E(v_t^4)=3$，因此：$E(\varepsilon_t^4)=3E(\sigma_t^4)$.
将条件方差平方：$\sigma_t^4=\alpha_0^2+2\alpha_0\alpha_1\varepsilon_{t-1}^2+\alpha_1^2\varepsilon_{t-1}^4$.
在平稳状态下取期望：$E(\varepsilon_t^4)=3\left[\alpha_0^2+2\alpha_0\alpha_1E(\varepsilon_t^2)+\alpha_1^2E(\varepsilon_t^4)\right]$.
代入：$E(\varepsilon_t^2)=\frac{\alpha_0}{1-\alpha_1}$,
解得：$E(\varepsilon_t^4)=\frac{3\alpha_0^2(1+\alpha_1)}{(1-\alpha_1)(1-3\alpha_1^2)}$.

再除以无条件方差的平方，即得结论。

因此，即使条件扰动 $v_t$ 服从正态分布，ARCH 误差的无条件分布也可以表现出高峰度。

### §2.7 ARCH 模型的优缺点

优点：
1. 模型结构简单
2. 能够描述波动聚集。
3. 可以递推预测条件方差。
4. 可以产生相对于正态分布的高峰厚尾特征。

局限：
1. 正负冲击以平方形式进入方差方程，对相同绝对值的正负冲击作对称处理，无法直接刻画杠杆效应。
2. 参数非负等约束限制模型的灵活性。
3. 为刻画持久的波动变化，可能需要较大的滞后阶数 $q$。

### §2.8 GARCH 模型的构造

ARCH(q) 可能需要很大的 $q$，导致参数过多和过拟合。
Bollerslev 于 1986 年提出广义自回归条件异方差模型(generalized autoregressive conditional heteroskedasticity, GARCH)，允许条件方差依赖自身的滞后值。

ARCH 类似于波动率的 AR 模型，而 GARCH 类似于波动率的 ARMA 模型。

**※【定义】** GARCH(p,q) 模型
$y_t=E(y_t\mid\mathcal F_{t-1})+\varepsilon_t$, $\varepsilon_t=\sigma_tv_t$, $\sigma_t^2=\alpha_0+\sum_{i=1}^q\alpha_i\varepsilon_{t-i}^2+\sum_{j=1}^p\beta_j\sigma_{t-j}^2$.
其中：
- $q$ 为平方冲击 $\varepsilon_t^2$ 的滞后阶数；
- $p$ 为条件方差 $\sigma_t^2$ 的滞后阶数；
- $\alpha_0>0$，$\alpha_i\geq0$，$\beta_j\geq0$；
- $v_t$ 为均值零、方差一且与过去信息独立的 i.i.d. 扰动。

**※【性质】** GARCH 有限二阶矩平稳解的充分条件 $\sum_{i=1}^q\alpha_i+\sum_{j=1}^p\beta_j<1$.

该条件在上述非负参数假设下保证有限无条件方差的平稳解存在。

### §2.9 GARCH 的 ARMA 表示

定义：$\eta_t=\varepsilon_t^2-\sigma_t^2$.
由于：$E(\varepsilon_t^2\mid\mathcal F_{t-1})=\sigma_t^2$,
所以：$E(\eta_t\mid\mathcal F_{t-1})=0$.
由 $\sigma_t^2=\varepsilon_t^2-\eta_t$，代入 GARCH 方差方程：$\varepsilon_t^2-\eta_t=\alpha_0+\sum_{i=1}^q\alpha_i\varepsilon_{t-i}^2+\sum_{j=1}^p\beta_j(\varepsilon_{t-j}^2-\eta_{t-j})$.
整理得到：$\varepsilon_t^2=\alpha_0+\sum_{i=1}^{\max(p,q)}(\alpha_i+\beta_i)\varepsilon_{t-i}^2+\eta_t-\sum_{j=1}^p\beta_j\eta_{t-j}$. (3)
其中，当 $i>q$ 时定义 $\alpha_i=0$，当 $i>p$ 时定义 $\beta_i=0$。

**※【性质】** GARCH 模型的平方误差具有 ARMA 型表示。
(3) 表明平方误差序列具有自回归移动平均结构，可用于分析矩、相关性和预测。

这里 $\eta_t$ 是鞅差分(martingale difference)；只有在其二阶矩有限等条件下，才能进一步将其视为通常意义下的白噪声。

### §2.10 GARCH 误差的性质

**※【性质】** GARCH 误差序列具有零条件均值和零非零阶自协方差。
由 $\varepsilon_t=\sigma_tv_t$：$E(\varepsilon_t\mid\mathcal F_{t-1})=0$, $E(\varepsilon_t)=0$, $E(\varepsilon_t\varepsilon_{t-j})=0,\quad j\geq1$.

证明与 ARCH 模型相同，均利用 $\sigma_t$ 的过去信息可测性以及 $v_t$ 与过去信息的独立性。

**※【性质】** GARCH 平稳解的无条件方差
在有限二阶矩平稳条件下：$\operatorname{Var}(\varepsilon_t)=\frac{\alpha_0}{1-\sum_{i=1}^q\alpha_i-\sum_{j=1}^p\beta_j}$.

**证明**
取条件方差方程的无条件期望：$E(\sigma_t^2)=\alpha_0+\sum_{i=1}^q\alpha_iE(\varepsilon_{t-i}^2)+\sum_{j=1}^p\beta_jE(\sigma_{t-j}^2)$.
对有限二阶矩平稳解，有：$E(\varepsilon_t^2)=E(\sigma_t^2)=v$,
所以：$v=\alpha_0+\left(\sum_{i=1}^q\alpha_i+\sum_{j=1}^p\beta_j\right)v$.
整理即得结论。

### §2.11 GARCH(1,1) 的厚尾性质

考虑：$\sigma_t^2=\alpha_0+\alpha_1\varepsilon_{t-1}^2+\beta_1\sigma_{t-1}^2$.

**※【性质】** GARCH(1,1) 的无条件峰度
假设 $v_t\overset{i.i.d.}{\sim}N(0,1)$，且：$1-(\alpha_1+\beta_1)^2-2\alpha_1^2>0$,
则：$\frac{E(\varepsilon_t^4)}{[E(\varepsilon_t^2)]^2}=\frac{3[1-(\alpha_1+\beta_1)^2]}{1-(\alpha_1+\beta_1)^2-2\alpha_1^2}$.
当 $\alpha_1>0$ 时，该峰度大于 $3$；当 $\alpha_1=0$ 时等于 $3$。

**证明**
由 $\varepsilon_{t-1}=\sigma_{t-1}v_{t-1}$：$\sigma_t^2=\alpha_0+(\alpha_1v_{t-1}^2+\beta_1)\sigma_{t-1}^2$.
记 $A_{t-1}=\alpha_1v_{t-1}^2+\beta_1$，则：$E(A_{t-1})=\alpha_1+\beta_1$,
$E(A_{t-1}^2)=3\alpha_1^2+2\alpha_1\beta_1+\beta_1^2=(\alpha_1+\beta_1)^2+2\alpha_1^2$.
由于 $A_{t-1}$ 与 $\sigma_{t-1}^2$ 独立，在平稳状态下：$E(\sigma_t^4)=\alpha_0^2+2\alpha_0(\alpha_1+\beta_1)E(\sigma_t^2)+E(A_{t-1}^2)E(\sigma_t^4)$.
结合：$E(\sigma_t^2)=\frac{\alpha_0}{1-\alpha_1-\beta_1}$, 以及 $E(\varepsilon_t^4)=3E(\sigma_t^4)$，整理得到峰度公式。

### §2.12 模型阶数与扩展

GARCH 阶数识别通常比 ARCH 更困难。实际应用中经常采用低阶模型，例如：
- GARCH(1,1)；
- GARCH(2,1)；
- GARCH(1,2)。

其中 GARCH(1,1) 在许多金融数据中已经能够较好地描述波动聚集，但并非对所有数据都充分。

**※【性质】** 条件波动率可以利用三类过去信息：
1. 过去的平方冲击：${\varepsilon_{t-1}^2,\varepsilon_{t-2}^2,\ldots}$。
2. 过去的条件方差：${\sigma_{t-1}^2,\sigma_{t-2}^2,\ldots}$。
3. 其他可能与波动率相关的历史信息。

ARCH 只使用第一类信息；GARCH 同时使用前两类信息；其他 GARCH 扩展模型还可以引入第三类信息。

主要扩展包括：
- 均值 GARCH 模型(GARCH-in-Mean, GARCH-M)；
- 积分 GARCH 模型(integrated GARCH, IGARCH)，应用于 RiskMetrics 等风险模型；
- 指数 GARCH 模型(exponential GARCH, EGARCH)，Nelson，1991；
- 门限 GARCH 模型(threshold GARCH, TGARCH)，Zakoian，1994；
- GJR-GARCH 模型，Glosten、Jagannathan 和 Runkle，1993；
- GARCH-MIDAS 模型，Engle、Ghysels 和 Sohn，2013。

## §3 模型设定、估计与诊断(Specification, Estimation and Diagnostic Checks)

### §3.1 建模动机：IBM 股票收益率

**※【例题】** IBM 股票日收益率的条件异方差
数据为 1970 年 1 月 2 日至 2008 年 12 月 31 日 IBM 股票的日简单收益率。
其均值方程近似满足 MA(2)：$y_t=0.000401-0.019697\hat\varepsilon_{t-1}-0.018922\hat\varepsilon_{t-2}+\hat\varepsilon_t$.
虽然均值方程已经描述了收益率的部分线性动态结构，但平方残差的 ACF 仍表现出显著相关性。
因此，需要检验是否存在 ARCH 效应，并进一步建立条件方差模型。

### §3.2 ARCH 与 GARCH 的识别

模型识别通常分为两个阶段。

**第一阶段：建立均值方程并检验 ARCH 效应。**
先拟合适当的 ARMA 均值模型，得到残差 ${\hat\varepsilon_t}$，再分析平方残差 ${\hat\varepsilon_t^2}$。
常用方法：
- 平方残差的 Ljung–Box Q 检验（见§3.3）；
- ARCH 拉格朗日乘子检验(Lagrange multiplier test, LM test, 见§3.4).


**第二阶段：确定条件方差模型的阶数。**
- ARCH(q)：平方残差的偏自相关函数(partial autocorrelation function, PACF)可为选择 $q$ 提供参考。
- GARCH(p,q)：通常结合信息准则、参数显著性及残差诊断确定阶数。

### §3.3 Ljung–Box Q 检验

**※【定义】** 平方残差的 Ljung–Box 检验
设 $\rho_k$ 为平方残差序列的第 $k$ 阶自相关系数。
检验假设为：$H_0:\rho_1=\rho_2=\cdots=\rho_m=0$, $H_1:$ 至少存在一个 $\rho_i\neq0$.
步骤：
1. 估计均值方程，得到 ${\hat\varepsilon_t}$。
2. 构造平方残差序列 ${\hat\varepsilon_t^2}$。
3. 对平方残差应用 Ljung–Box Q 检验。
4. 若 $p$ 值较小，则拒绝平方残不相关的原假设，表明存在可检测的条件异方差动态。

该检验直接针对平方残差的序列相关性，而不是检验所有可能形式的条件异方差。

### §3.4 ARCH-LM 检验

**※【定义】** ARCH-LM 检验

检验原假设：$H_0:\alpha_1=\alpha_2=\cdots=\alpha_q=0$,
备择假设：$H_1:\text{至少存在一个 }\alpha_i\neq0$.
**步骤一：** 估计均值方程，得到残差 $\hat\varepsilon_t$。
**步骤二：** 建立辅助回归(auxiliary regression)：$\hat\varepsilon_t^2=\alpha_0+\alpha_1\hat\varepsilon_{t-1}^2+\cdots+\alpha_q\hat\varepsilon_{t-q}^2+u_t$.
得到辅助回归的决定系数 $R_{\mathrm{AUX}}^2$。
**步骤三：** 构造 LM 统计量：${LM_{\mathrm{ARCH}}=T R_{\mathrm{AUX}}^2}$, 其中 $T$ 为辅助回归的有效样本量。

在原假设及标准正则条件下：${LM_{\mathrm{ARCH}}\xrightarrow{d}\chi^2(q)}$. 若统计量足够大，或 $p$ 值小于显著性水平，则拒绝无 ARCH 效应的原假设。该检验也对 GARCH(p,q) 等条件异方差备择模型具有检验能力。

### §3.5 ARCH/GARCH 模型的极大似然估计

ARCH/GARCH 的条件方差是过去残差及参数的非线性函数，通常不能通过普通最小二乘法直接联合估计全部参数。采用极大似然估计(maximum likelihood estimation, MLE)，通过最大化给定观测数据的对数似然函数估计参数。

**※【例题】** AR(1)-GARCH(1,1) 的联合估计
考虑：$y_t=\phi_0+\phi_1y_{t-1}+\varepsilon_t$, $\varepsilon_t=\sigma_tv_t,\, v_t\overset{i.i.d.}{\sim}N(0,1)$,  $\sigma_t^2=\alpha_0+\alpha_1\varepsilon_{t-1}^2+\beta_1\sigma_{t-1}^2$.
待估参数为：$\theta=(\phi_0,\phi_1,\alpha_0,\alpha_1,\beta_1)$.

**第一步：指定均值方程和条件方差方程。**
给定参数后：$\varepsilon_t(\theta)=y_t-\phi_0-\phi_1y_{t-1}$.
由残差递推计算 $\sigma_t^2(\theta)$，并对初始条件方差作适当处理。

**第二步：构造条件对数似然函数。**
在条件正态假设下，使用 $t=2,\ldots,T$ 的观测：
$L(\theta)=-\frac{T-1}{2}\log(2\pi)-\frac12\sum_{t=2}^T\log(\sigma_t^2)-\frac12\sum_{t=2}^T\frac{(y_t-\phi_0-\phi_1y_{t-1})^2}{\sigma_t^2}$.

**第三步：数值最大化。**
$\hat\theta=\arg\max_{\theta}L(\theta)$. 计算机同时给出参数估计值及其标准误。
这里需要联合估计均值方程和波动率方程，即同时估计 $\phi_0,\phi_1,\alpha_0,\alpha_1,\beta_1$。

### §3.6 拟极大似然估计

**※【定义】** 拟极大似然估计(quasi-maximum likelihood estimation, QMLE)
实际中，标准化扰动 $v_t$ 的真实分布可能未知或不服从正态分布。
即使正态性假设不成立，仍可使用高斯条件对数似然函数进行估计，这称为高斯 QMLE。

**※【性质】** QMLE 的一致性
在条件均值与条件方差模型正确设定，并满足适当的平稳性、遍历性、可识别性及矩条件等正则条件时，即使真实扰动分布不是正态分布，高斯 QMLE 仍可保持一致性(consistency)。

需要注意：
- QMLE 的假设似然函数不一定是真实似然函数。
- 正态性错误可能影响传统信息矩阵标准误的有效性。
- 实践中通常需要使用适当的稳健标准误。
- 若条件均值或条件方差设定错误，一致性不再自动得到保证。

### §3.7 模型诊断(Model Diagnostic Checking)

**※【定义】** 标准化残差(standardized residual)
对拟合后的 ARCH/GARCH 模型，定义：${\hat v_t=\frac{\hat\varepsilon_t}{\hat\sigma_t}}$. 若均值方程和条件方差方程均正确设定，标准化残差应近似表现为均值零、方差一且不存在序列相关的创新序列。

**※【性质】** 均值方程的充分性诊断
如果均值方程已经捕捉数据中的线性相关结构，则残差 $\hat\varepsilon_t$ 应近似不相关。
标准化残差 $\hat v_t$ 也应不具有显著序列相关性。
因此，可对 ${\hat v_t}$ 计算 Ljung–Box Q 统计量，检验均值方程是否充分。

**※【性质】** 波动率方程的充分性诊断
如果波动率方程已捕捉平方冲击的动态结构，则：$\hat v_t^2=\frac{\hat\varepsilon_t^2}{\hat\sigma_t^2}$, 应不具有显著的序列相关性。
因此，可对 ${\hat v_t^2}$ 计算 Ljung–Box Q 统计量，检验是否仍存在未解释的 ARCH 效应。

**※【性质】** 扰动分布假设的诊断
通过标准化残差的以下统计特征检查分布假设：
- 偏度(skewness)；
- 峰度(kurtosis)；
- 分位数图(quantile–quantile plot, Q–Q plot)；
- 直方图及正态性检验。

模型诊断的对应关系为：

| 诊断对象           | 检验工具           | 主要目的     |
| -------------- | -------------- | -------- |
| $\hat v_t$     | Ljung–Box Q 检验 | 检查均值方程   |
| $\hat v_t^2$   | Ljung–Box Q 检验 | 检查波动率方程  |
| $\hat v_t$ 的分布 | 偏度、峰度、Q–Q 图    | 检查扰动分布假设 |

## §4 波动率预测(Forecast)

### §4.1 ARCH(q) 的条件方差预测

ARCH(q) 的预测可以像 AR 模型一样递推。
记：$E_t(\cdot)=E(\cdot\mid\mathcal F_t)$, 表示基于时刻 $t$ 信息的条件期望。
对 ARCH(q)：$\sigma_{t+1}^2=\alpha_0+\alpha_1\varepsilon_t^2+\cdots+\alpha_q\varepsilon_{t+1-q}^2$.

**※【性质】** 一步条件方差预测
由于 $\sigma_{t+1}^2$ 在时刻 $t$ 已由过去信息确定：${E_t(\sigma_{t+1}^2)=\sigma_{t+1}^2}$.

**※【性质】** 两步条件方差预测
$E_t(\sigma_{t+2}^2)=E_t(\alpha_0+\alpha_1\varepsilon_{t+1}^2+\alpha_2\varepsilon_t^2+\cdots+\alpha_q\varepsilon_{t+2-q}^2)$.
由于：$E_t(\varepsilon_{t+1}^2)=E_t(\sigma_{t+1}^2)=\sigma_{t+1}^2$,
因此：${E_t(\sigma_{t+2}^2)=\alpha_0+\alpha_1\sigma_{t+1}^2+\alpha_2\varepsilon_t^2+\cdots+\alpha_q\varepsilon_{t+2-q}^2}$.

更一般地，多步预测通过递推获得：对未来尚未观测到的平方冲击，使用其条件期望替代；对时刻 $t$ 已观测到的平方冲击，使用实际值。

### §4.2 GARCH(1,1) 的条件方差预测

考虑：$\sigma_t^2=\alpha_0+\alpha_1\varepsilon_{t-1}^2+\beta_1\sigma_{t-1}^2$,
其中：$\alpha_0>0,\,\alpha_1,\beta_1\geq0$, $\alpha_1+\beta_1<1$.
无条件方差为：$\bar\sigma^2=\frac{\alpha_0}{1-\alpha_1-\beta_1}$.

**※【性质】** 一步预测
${E_t(\sigma_{t+1}^2)=\sigma_{t+1}^2=\alpha_0+\alpha_1\varepsilon_t^2+\beta_1\sigma_t^2}$.

**※【性质】** 多步预测递推公式
对于 $j\geq2$：$E_t(\sigma_{t+j}^2)=E_t(\alpha_0+\alpha_1\varepsilon_{t+j-1}^2+\beta_1\sigma_{t+j-1}^2)$.
利用 $\varepsilon_{t+j-1}^2=\sigma_{t+j-1}^2v_{t+j-1}^2$ 及 $E(v_{t+j-1}^2)=1$：
${E_t(\sigma_{t+j}^2)=\alpha_0+(\alpha_1+\beta_1)E_t(\sigma_{t+j-1}^2),\quad j\geq2}$. (4)
由 (4) 可进一步得到闭式预测公式：
${E_t(\sigma_{t+j}^2)=\bar\sigma^2+(\alpha_1+\beta_1)^{j-1}(\sigma_{t+1}^2-\bar\sigma^2)}$.

因此，多步条件方差预测逐渐回归长期无条件方差，$\alpha_1+\beta_1$ 决定波动率预测偏离长期水平后的衰减速度。

### §4.3 区间预测(Interval Forecasting)

考虑：$y_{t+1}=E(y_{t+1}\mid\mathcal F_t)+\varepsilon_{t+1}$, $\varepsilon_{t+1}=\sigma_{t+1}v_{t+1}$.
条件分布取决于 $v_{t+1}$ 的分布。
若：$v_{t+1}\overset{i.i.d.}{\sim}N(0,1)$,
则：$y_{t+1}\mid\mathcal F_t\sim N(E_t(y_{t+1}),\sigma_{t+1}^2)$.

**※【性质】** 正态条件下的 95% 预测区间 ${[E_t(y_{t+1})-1.96\sigma_{t+1},\ E_t(y_{t+1})+1.96\sigma_{t+1}]}$.

该区间描述未来观测值的条件预测范围，严格来说是预测区间(prediction interval)，而非未知总体参数的置信区间。

**※【例题】** AR(1)-GARCH(1,1) 的一步预测区间

设：$y_t=\phi_0+\phi_1y_{t-1}+\varepsilon_t$, $\varepsilon_t=\sigma_tv_t,\, v_t\overset{i.i.d.}{\sim}N(0,1)$, $\sigma_t^2=\alpha_0+\alpha_1\varepsilon_{t-1}^2+\beta_1\sigma_{t-1}^2$.
在时刻 $t$：$E_t(y_{t+1})=\phi_0+\phi_1y_t$, $\sigma_{t+1}^2=\alpha_0+\alpha_1\varepsilon_t^2+\beta_1\sigma_t^2$.

因此，95% 一步预测区间为：
${\left[\phi_0+\phi_1y_t-1.96\sqrt{\alpha_0+\alpha_1\varepsilon_t^2+\beta_1\sigma_t^2},\ \phi_0+\phi_1y_t+1.96\sqrt{\alpha_0+\alpha_1\varepsilon_t^2+\beta_1\sigma_t^2}\right]}$.

## 补充

### 1. QMLE

#### 1.1. 高斯 QMLE 的一般形式

考虑一般的时间序列模型： $y_t=\mu_t(\theta)+\sigma_t(\theta)v_t,$
其中 $\theta$ 为待估参数，假设：$\begin{cases} E(v_t\mid\mathcal F_{t-1})=0,\\ E(v_t^2\mid\mathcal F_{t-1})=1. \end{cases}$ 不要求 $v_t$ 服从正态分布。

为了构造似然函数，暂时将条件分布视为：$y_t\mid\mathcal F_{t-1} \sim N(\mu_t(\theta),\sigma_t^2(\theta))$.
由正态分布的概率密度函数，得到拟对数似然函数：
$\ell_T(\theta) =-\frac12\sum_{t=1}^T \left[ \log(2\pi)+\log\sigma_t^2(\theta) +\frac{(y_t-\mu_t(\theta))^2}{\sigma_t^2(\theta)} \right]$.

去掉常数项和系数，最大化似然函数等价于：
${ \hat\theta_{\mathrm{QMLE}} =\arg\min_{\theta\in\Theta} \sum_{t=1}^T \left[ \log\sigma_t^2(\theta) +\frac{(y_t-\mu_t(\theta))^2}{\sigma_t^2(\theta)} \right] }$

这就是高斯 QMLE 的一般目标函数。其中两项分别起到不同作用：
- $\log\sigma_t^2$：惩罚过大的预测方差。
- $\frac{(y_t-\mu_t)^2}{\sigma_t^2}$：惩罚相对于预测方差而言过大的预测误差。

如果只有第二项，模型可以通过无限增大预测方差来降低损失。第一项防止了这种情况。

#### 1.2. GARCH(1,1) 的一般估计算法

考虑 AR(1)-GARCH(1,1)：$\begin{cases} y_t=\phi_0+\phi_1y_{t-1}+\varepsilon_t,\\ \varepsilon_t=\sigma_tv_t,\\ \sigma_t^2=\alpha_0+\alpha_1\varepsilon_{t-1}^2 +\beta_1\sigma_{t-1}^2. \end{cases}$
待估参数为：$\theta=(\phi_0,\phi_1,\alpha_0,\alpha_1,\beta_1).$
初始化参数 $\theta^{(0)}$：指定初始条件方差及参数约束
计算均值方程残差：$\varepsilon_t=y_t-\phi_0-\phi_1y_{t-1}$.
递推计算条件方差：$\sigma_t^2=\alpha_0+\alpha_1\varepsilon_{t-1}^2+\beta_1\sigma_{t-1}^2$.
计算目标函数：$J(\theta)=\sum_{t=1}^T\left(\log\sigma_t^2+\frac{\varepsilon_t^2}{\sigma_t^2}\right)$.
数值优化，更新参数：BFGS、SLSQP 等优化算法
未收敛：返回并重新计算残差与方差
已收敛：输出参数估计值
若要求存在有限二阶矩的因果平稳解，优化过程中通常施加约束：
$\begin{cases} |\phi_1|<1,\\ \alpha_0>0,\\ \alpha_1,\beta_1\geq0,\\ \alpha_1+\beta_1<1. \end{cases}$

本质上，这就是一个带约束的非线性优化问题，并不存在专属于 QMLE 的优化算法。
