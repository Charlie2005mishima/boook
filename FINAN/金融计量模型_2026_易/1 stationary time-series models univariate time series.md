

## 1 动机与引言

- 过去能帮助预测未来吗？
- 时间序列的特殊之处在于：
  1. **时间排序(Temporal ordering)**。
  2. 过去可能影响未来，即存在**自相关(autocorrelation)**。
  3. 我们能否利用现有数据来建模和估计这种自相关？
  4. 时间序列研究的一个重要目标：**预测(Forecast)**！

**※【定义】** **单变量时间序列(Univariate time series)**：对某个经济指标随时间的观测集合（如股票的对数收益率、GDP增长率等）。数据表示为 $\{y_1, y_2, \dots, y_T\}$。目的是提取信息，建立能够捕捉真实**数据生成过程(Data-Generating Process, DGP)** 本质的模型。

---

## 2 白噪声与基本模型

**※【定义】** **白噪声(White Noise)**：如果序列 $\{\epsilon_t\}$ 满足以下条件：
- 均值：$E(\epsilon_t) = 0$
- 方差：$E(\epsilon_t^2) = \operatorname{var}(\epsilon_t) = \sigma^2$
- 协方差：$E(\epsilon_t \epsilon_\tau) = 0$，对所有 $\tau \neq t$。
进一步，如果跨期独立，则为**独立白噪声(independent white noise)**；如果服从正态分布 $\epsilon_t \sim N(0, \sigma^2)$，则为**高斯白噪声(Gaussian white noise)**。

**※【定义】** **移动平均模型(Moving Average, MA)**：$q$ 阶移动平均，记为 $\text{MA}(q)$，定义为白噪声的线性组合：
$x_t = \sum_{j=0}^q \beta_j \epsilon_{t-j}$ (1)
通常设定 $\beta_0 = 1$。带常数项的 $\text{MA}(q)$ 形式为 $x_t = \mu + \sum_{j=0}^q \beta_j \epsilon_{t-j}$。

**※【例题】** 考虑 $\text{MA}(1)$ 过程 $x_t = \epsilon_t + 0.5\epsilon_{t-1}$：
- $E(x_t) = 0$
- $\operatorname{var}(x_t) = \sigma^2(1 + 0.5^2) = 1.25\sigma^2$
- $\operatorname{cov}(x_t, x_{t-1}) = 0.5\sigma^2$
- 不是白噪声过程（因为存在一阶自相关）。

**※【定义】** **后移算子(Back-Shift/Lag Operator)**：定义为 $Ly_t = y_{t-1}$ 或 $By_t = y_{t-1}$。高阶形式 $L^2 y_t = L(Ly_t) = y_{t-2}$。
$\text{MA}(q)$ 模型用滞后算子可写为：$x_t = (1 + \beta_1 L + \dots + \beta_q L^q)\epsilon_t$。

**※【定义】** **自回归模型(Autoregressive, AR)**：$p$ 阶自回归，记为 $\text{AR}(p)$，即：
$y_t = \phi_0 + \sum_{j=1}^p \phi_j y_{t-j} + \epsilon_t$ (2)
用滞后算子表示为：$(1 - \phi_1 L - \phi_2 L^2 - \dots - \phi_p L^p)y_t = \phi_0 + \epsilon_t$。

**※【定义】** **自回归移动平均模型(Autoregressive Moving Average, ARMA)**：记为 $\text{ARMA}(p, q)$：
$y_t = \phi_0 + \sum_{j=1}^p \phi_j y_{t-j} + \sum_{j=0}^q \beta_j \epsilon_{t-j}$ (3)
如果 $q=0$，则退化为 $\text{AR}(p)$ 过程；如果 $p=0$，则退化为 $\text{MA}(q)$ 过程。

---

## 3 平稳性 (Stationarity)

**※【定义】** **平稳性(Stationarity)**：
- **严平稳(Strict/Strong Stationarity)**：分布随时间不变（条件极强，难以验证）。
- **弱平稳(Weak Stationarity) / 协方差平稳(Covariance Stationarity)**：前二阶矩随时间不变。
满足以下条件：
1. $E(y_t) = \mu$，对所有 $t$。
2. $\operatorname{var}(y_t) = E[(y_t - \mu)^2] = \sigma_y^2$，对所有 $t$。
3. $\gamma_k = \operatorname{cov}(y_t, y_{t-k}) = E[(y_t - \mu)(y_{t-k} - \mu)]$，仅取决于 $k$。

**※【定义】** **自相关(Autocorrelation)**：滞后 $k$ 阶自相关定义为 $\rho_k \equiv \frac{\operatorname{cov}(y_t, y_{t-k})}{\operatorname{var}(y_t)} = \frac{\gamma_k}{\gamma_0}$。性质：$\rho_0 = 1$，$\rho_k = \rho_{-k}$。

**※【例题】** 判断弱平稳性：
1. 白噪声：均值、方差恒定，协方差为0，**是**弱平稳的。
2. $y_t = \alpha + \beta t$：均值随时间 $t$ 变化，**不是**弱平稳的。
3. $y_t = y_{t-1} + \epsilon_t$（随机游走）：方差随时间递增，**不是**弱平稳的。

**※【定理】** **MA(q) 过程的平稳性**：
对于所有 $t$ 和 $k>0$，$E(x_t) = 0$，$\operatorname{var}(x_t) = \sigma^2(1 + \sum_{j=1}^q \beta_j^2)$。自协方差仅与 $k$ 有关，因此 **MA(q) 过程总是平稳的**。

**※【定理】** **AR(1) 过程的平稳性**：
通过迭代 $y_t = \phi_0 \sum_{j=0}^{t-1} \phi_1^j + \phi_1^t y_0 + \sum_{j=0}^{t-1} \phi_1^j \epsilon_{t-j}$ (4)。
若 $|\phi_1| \geq 1$，冲击会累积而非衰减，不是协方差平稳的。**平稳条件：$|\phi_1| < 1$**。
用滞后算子表示为 $(1-\phi_1 L)y_t = \phi_0 + \epsilon_t$ (5)。平稳条件是特征方程 $(1-\phi_1 z)=0$ 的解（根）在单位圆外。

**※【定理】** **AR(p) 与 ARMA(p,q) 过程的平稳性**：
- **AR(p)** 的平稳条件：自回归多项式 $(1 - \phi_1 z - \phi_2 z^2 - \dots - \phi_p z^p) = 0$ 的根都在单位圆外。
- **ARMA(p,q)** 的平稳条件：完全由自回归部分决定，即 AR 多项式的根在单位圆外。

弱平稳在实践中的含义：过去的数据围绕固定水平波动；未来的前二阶矩与历史相同，可进行有意义的推断（预测）。在金融文献中，通常假设资产收益率序列是弱平稳的。

---

## 4 自相关函数 (ACF) 与偏自相关函数 (PACF)

**※【定义】** **自相关函数(Autocorrelation Function, ACF)**：$\gamma_k$ 或 $\rho_k$ 关于 $k$ 的图。

**※【定理】** **MA(1) 的 ACF**：
$\gamma_0 = (1+\beta_1^2)\sigma^2$，$\gamma_1 = \beta_1\sigma^2$，$\gamma_k = 0 (k>1)$。
$\rho_1 = \frac{\beta_1}{1+\beta_1^2}$，$\rho_k = 0 (k>1)$。
MA(1) 具有**有限记忆(Finite memory)**，ACF 在滞后 $q$ 处截尾。部分日度股票收益率有轻微序列相关，可用 MA 模型建模。

**※【定理】** **MA(q) 的 ACF**：
$\gamma_0 = \sigma^2(1 + \sum_{j=1}^q \beta_j^2)$，$\gamma_k = \sigma^2(\beta_k + \beta_{k+1}\beta_1 + \dots + \beta_q\beta_{q-k})$ 对于 $k \leq q$，$k>q$ 时为 0。ACF 在 $q$ 阶截尾。

**※【定义】** **Yule-Walker 方程(Yule-Walker Equations)**：对方程两边乘 $y_{t-k}$ 并取期望，用于分析 ACF。

**※【定理】** **AR(1) 的 ACF**：
Yule-Walker 方程：$\gamma_0 = \phi_1\gamma_1 + \sigma^2$，$\gamma_k = \phi_1\gamma_{k-1}$。
解得 $\gamma_0 = \frac{\sigma^2}{1-\phi_1^2}$，$\gamma_k = \frac{\sigma^2}{1-\phi_1^2}\phi_1^k$。
ACF：$\rho_k = \phi_1^k$。若平稳，呈几何衰减。

**※【定理】** **AR(2) 的 ACF**：
$\gamma_0 = \phi_1\gamma_1 + \phi_2\gamma_2 + \sigma^2$ (7)；$\gamma_1 = \phi_1\gamma_0 + \phi_2\gamma_1$ (8)；$\gamma_k = \phi_1\gamma_{k-1} + \phi_2\gamma_{k-2}$ (9)。
$\rho_1 = \frac{\phi_1}{1-\phi_2}$，$\rho_k = \phi_1\rho_{k-1} + \phi_2\rho_{k-2}$。若平稳，呈几何衰减。
$\gamma_0 = \frac{\sigma^2}{1-\phi_1\rho_1 - \phi_2\rho_2}$。

**※【定理】** **ARMA(1,1) 的 ACF**：
$\gamma_0 = \phi_1\gamma_1 + \sigma^2 + \beta_1(\phi_1+\beta_1)\sigma^2$ (10)；$\gamma_1 = \phi_1\gamma_0 + \beta_1\sigma^2$ (11)；$\gamma_k = \phi_1\gamma_{k-1}$ (12) $(k \ge 2)$。
$\rho_1 = \frac{(\phi_1+\beta_1)(1+\beta_1\phi_1)}{1+2\beta_1\phi_1+\beta_1^2} = \phi_1 + \frac{\beta_1\sigma^2}{\gamma_0} \neq \phi_1$。
$\rho_k = \phi_1\rho_{k-1}$ 对于 $k \ge 2$。若平稳，从滞后 1 期开始几何衰减。

**※【定理】** **ARMA(p,q) 的 ACF**：
$\rho_k = \sum_{j=1}^p \phi_j \rho_{k-j}$ 对于 $k \ge \max\{p, q+1\}$。若平稳，最终几何衰减。

**※【定义】** **偏自相关函数(Partial Autocorrelation Function, PACF)**：滞后 $k$ 阶偏自相关 $\phi_{k,k}$ 剔除了中间变量 $\{y_{t-1}, \dots, y_{t-k+1}\}$ 的影响。
$y_t = \phi_{1,0} + \phi_{1,1}y_{t-1} + e_{1t}$
$y_t = \phi_{2,0} + \phi_{2,1}y_{t-1} + \phi_{2,2}y_{t-2} + e_{2t}$
以此类推。可用 Yule-Walker 方程从 ACF 推导，如 $\phi_{2,2} = \frac{\rho_2 - \rho_1^2}{1 - \rho_1^2}$。

**※【定理】** **各类过程的 PACF 特征**：
- **AR(p)**：$k > p$ 时 $\phi_{k,k} = 0$。PACF 在 $p$ 阶**截尾**。
- **MA(1)**：PACF 不会突然跳到0，而是几何衰减（$\beta_1 < 0$ 时直接衰减；$\beta_1 > 0$ 时震荡衰减）。
- **ARMA(p,q)**：PACF 最终衰减趋于 0，衰减模式取决于 $\beta_j$。

**※【定理】** **理论 ACF 与 PACF 总结 (Properties of the ACF and PACF)**：

| 过程 (Process) | ACF | PACF |
| :--- | :--- | :--- |
| White noise | 所有 $\rho_s = 0 (s \neq 0)$ | 所有 $\phi_{ss} = 0$ |
| AR(1): $\phi_1 > 0$ | 直接几何衰减: $\rho_s = \phi_1^s$ | $\phi_{11} = \rho_1$; $s \ge 2$ 时 $\phi_{ss} = 0$ |
| AR(1): $\phi_1 < 0$ | 震荡几何衰减: $\rho_s = \phi_1^s$ | $\phi_{11} = \rho_1$; $s \ge 2$ 时 $\phi_{ss} = 0$ |
| AR(p) | 衰减至 0，可能震荡 | 在 lag p 处截尾，所有 $s > p$ 时 $\phi_{ss} = 0$ |
| MA(1): $\beta > 0$ | lag 1 处正尖峰，$s \ge 2$ 时 $\rho_s = 0$ | 震荡衰减: $\phi_{11} > 0$ |
| MA(1): $\beta < 0$ | lag 1 处负尖峰，$s \ge 2$ 时 $\rho_s = 0$ | 几何衰减: $\phi_{11} < 0$ |
| ARMA(1,1): $\phi_1 > 0$ | 从 lag 1 后开始几何衰减 | 从 lag 1 后开始震荡衰减 |
| ARMA(1,1): $\phi_1 < 0$ | 从 lag 1 后开始震荡衰减 | 从 lag 1 后开始几何衰减 |
| ARMA(p,q) | 从 lag q 后开始衰减 | 从 lag p 后开始衰减 |

---

## 5 样本 ACF 与 PACF

**※【定义】** **样本自相关 (Sample ACF)**：
样本均值 $\bar{y} = \frac{\sum_{t=1}^T y_t}{T}$
样本方差 $\hat{\sigma}_y^2 = \frac{\sum_{t=1}^T (y_t - \bar{y})^2}{T}$
滞后 $k$ 样本自相关 $\hat{\rho}_k = \frac{\sum_{t=k+1}^T (y_t - \bar{y})(y_{t-k} - \bar{y})}{\sum_{t=1}^T (y_t - \bar{y})^2}$

**※【定理】** **显著性检验 (t-test)**：检验 $H_0: \rho_k = 0$。
若序列是 i.i.d.（如 MA(0)），则 $\hat{\rho}_k$ 渐近服从均值为 0，方差为 $1/T$ 的正态分布。t 统计量 $= \frac{\hat{\rho}_k}{\sqrt{1/T}} \xrightarrow{D} \mathcal{N}(0,1)$。
若序列满足 $\rho_j = 0 (j \ge k)$（如 MA(k-1)），方差变为 $\frac{1 + 2\sum_{j=1}^{k-1} \rho_j^2}{T}$。

**※【定义】** **Ljung-Box Q 统计量**：检验一组自相关是否联合为 0，$H_0: \rho_1 = \dots = \rho_m = 0$。
$Q(m) = T(T+2) \sum_{k=1}^m \frac{\hat{\rho}_k^2}{T-k} \xrightarrow{D} \chi_m^2$。
若 $Q(m) > \chi_m^2(\alpha)$，拒绝原假设。在用于检验 ARMA 模型残差时，自由度为 $m - g$（$g$ 为估计的参数个数）。

**※【定义】** **样本偏自相关 (Sample PACF)**：用 OLS 估计每个滞后回归方程，第 $k$ 个方程的 $\hat{\phi}_{k,k}$ 即为样本 PACF。
对于平稳高斯 AR(p) 模型，$\hat{\phi}_{k,k}$ 依概率收敛于 $\phi_{k,k}$。对于 $k > p$，$\hat{\phi}_{k,k}$ 渐近服从均值为 0，方差为 $1/T$ 的正态分布。样本 PACF 有助于识别 AR 模型的阶数 $p$。

---

## 6 模型选择

识别模型不应仅依靠 ACF 或 PACF。我们需要简约模型（估计量方差与自由度成反比）。
**※【定义】** **信息准则 (Information Criteria, IC)**：
- **AIC (Akaike Information Criterion)**：$AIC(I) = \log(\tilde{\sigma}_I^2) + \frac{2I}{T}$
- **BIC (Bayesian Information Criterion)**：$BIC(I) = \log(\tilde{\sigma}_I^2) + \frac{I\log(T)}{T}$
其中 $I$ 为参数个数，$\tilde{\sigma}_I^2 = SSR/T$。选择使 IC 最小的模型。

**※【定理】** **AIC vs BIC**：
- BIC 具有更好的大样本性质，渐近选择正确模型（惩罚项更重）；AIC 偏向于选择过度参数化的模型。
- 小样本中，AIC 可能优于 BIC。
- 如果两者建议冲突：BIC 选出的模型应检查残差是否为白噪声；AIC 选出的模型应检查系数 t 统计量是否显著。

**※【例题】** **模拟 AR(2) 序列**：$y_t = 0.7y_{t-1} - 0.49y_{t-2} + \epsilon_t$。
逆根为复数：$\alpha_1, \alpha_2 = 0.7[\cos(1.047) \pm i\sin(1.047)]$。丢弃前500个观测以保证平稳性。

样本 ACF 衰减，PACF 在 lag 1 和 lag 2 有尖峰，表明 AR(2) 模型。用 OLS 估计 `y c ar(1) ar(2)`。

---

## 7 Box-Jenkins 方法

Box and Jenkins (1970, 1976) 提出了系统的三阶段方法：
1. **识别 (Identification)**
2. **估计 (Estimation)**
3. **模型诊断检验 (Model diagnostic checking)**

### 1. 识别
观察时间图、样本 ACF、样本 PACF，与理论比较提出候选模型。模型设定使用 AIC, BIC。注意**简约性(Parsimony)**，避免共同因子问题。

### 2. 估计
- **AR 模型**：最小二乘法或极大似然法。
- **MA 和 ARMA 模型**：极大似然法。

**※【定义】** **可逆性(Invertibility)**：
对 MA 模型 $x_t = (1 + \beta_1 L + \dots + \beta_q L^q)\epsilon_t$。
可逆条件：MA 多项式 $1 + \beta_1 z + \dots + \beta_q z^q = 0$ 的所有根均在单位圆外。
MA 模型的可逆性是 AR 模型平稳性的对偶属性。
ARMA(p,q) 的可逆条件完全由其 MA 部分决定。
t 统计量、ACF、Q 统计量等均假设过程平稳且可逆（暗示无单位根）。

### 3. 模型诊断检验
- 绘制残差图，寻找异常值或拟合较差的时间段。
- 检查残差的 ACF, PACF, Q 统计量，确保序列不相关（白噪声）。
- 将样本分为子区间检验，或进行样本外预测检验。

---

## 8 预测 (Forecasting)

利用**条件期望(Conditional expectations)**。在时刻 $t$，信息集 $\mathcal{F}_t \equiv \{y_t, y_{t-1}, \dots\}$。
最优 1 步向前预测：$E_t y_{t+1} \equiv E(y_{t+1} | \mathcal{F}_t) = \arg\min_g E[(y_{t+1} - g)^2 | \mathcal{F}_t]$（均方误差最小化）。
假设 $E(\epsilon_{t+1} | \mathcal{F}_t) = 0$。

**※【定理】** **MA(1) 的预测**：$x_t = \mu + \beta_1\epsilon_{t-1} + \epsilon_t$
- 1 步预测：$E_t x_{t+1} = \mu + \beta_1\epsilon_t$。预测误差：$e_t(1) = \epsilon_{t+1}$，方差 $\sigma^2$。
- $j \ge 2$ 步预测：$E_t x_{t+2} = \mu = E(x_t)$。预测误差：$e_t(j) = \beta_1\epsilon_{t+j-1} + \epsilon_{t+j}$，方差 $(1+\beta_1^2)\sigma^2$。
- 多步预测就是序列均值（因为只有 1 期记忆）。95% 置信区间：1步为 $[\mu+\beta_1\epsilon_t \pm 1.96\sigma]$。

**※【定理】** **AR(1) 的预测**：$y_t = \phi_0 + \phi_1 y_{t-1} + \epsilon_t$
- 1 步预测：$E_t y_{t+1} = \phi_0 + \phi_1 y_t$。
- 2 步预测：$E_t y_{t+2} = \phi_0 + \phi_1 E_t y_{t+1} = \phi_0(1+\phi_1) + \phi_1^2 y_t$。
- $j$ 步预测：$E_t y_{t+j} = \phi_0 \sum_{i=0}^{j-1} \phi_1^i + \phi_1^j y_t$。当 $j \to \infty$，收敛于无条件均值 $\frac{\phi_0}{1-\phi_1}$。
- 预测误差：$e_t(j) = \sum_{i=0}^{j-1} \phi_1^i \epsilon_{t+j-i}$，方差 $\sigma^2 \sum_{i=0}^{j-1} \phi_1^{2i}$。当 $j \to \infty$，趋于 $\frac{\sigma^2}{1-\phi_1^2} = \operatorname{var}(y_t)$。

### 样本外预测评估
留出部分观测值不参与估计。比较预测误差。
**※【定义】** 误差度量指标：
- **MSPE** (Mean Squared Prediction Error)
- **MAE** (Mean Absolute Error)
- **MAPE** (Mean Absolute Percentage Error)

**※【定义】** **Diebold-Mariano 检验**：比较两模型预测精度。
设损失函数 $g(e_j)$，差异损失 $d_j = g(e_{1j}) - g(e_{2j})$。
原假设 $H_0: E(d_j) = 0$（精度相同）。
统计量：$\frac{\bar{d}}{\sqrt{\hat{\gamma}/H}} \xrightarrow{D} \mathcal{N}(0,1)$。若序列相关，需使用 Newey-West 估计方差。

---

## 附录：AR 多项式复根与平稳性

**※【定理】** **AR 多项式的复根**：若 $(1 - \phi_1 z - \phi_2 z^2 - \dots - \phi_p z^p) = 0$ 的根为复数。
例如 $p=2$ 且 $\phi_1^2 + 4\phi_2 < 0$，逆根是一对共轭复数：
$\alpha_1 = \frac{\phi_1 + i\sqrt{-\phi_1^2 - 4\phi_2}}{2} = r(\cos\theta + i\sin\theta)$
$\alpha_2 = \frac{\phi_1 - i\sqrt{-\phi_1^2 - 4\phi_2}}{2} = r(\cos\theta - i\sin\theta)$
其中模长 $r = \sqrt{-\phi_2}$，辐角 $\theta = \arccos\left(\frac{\phi_1}{2\sqrt{-\phi_2}}\right)$。
**平稳条件**：$|\alpha_1| = |\alpha_2| = r < 1$。