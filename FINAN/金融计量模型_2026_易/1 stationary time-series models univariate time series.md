
## 1. 动机与时间序列的特殊性

**图片展示：中国GDP年化增长率，1978-2018**
问题：过去能帮助预测未来吗？

时间序列的特殊之处在于：
*   **时间排序(Temporal ordering)**：过去可能影响未来，即存在**自相关(autocorrelation)**。
*   能否利用现有数据来建模和估计这种自相关？
*   时间序列研究的一个重要目标：**预测(Forecast)**。

**※【定义】** **单变量时间序列(Univariate time series)**：
对某个经济指标随时间的观测集合。例如股票的对数收益率、GDP增长率等。
数据表示为一个观测到的时间序列 $\{y_1, y_2, \dots, y_T\}$（共 $T$ 个数据点）。
目的：提取 $\{y_t\}_{t=1}^T$ 中包含的信息，尝试建立能够捕捉真实**数据生成过程(Data-Generating Process, DGP)** 本质的模型。

---

## 2. 白噪声模型

**※【定义】** **白噪声(White Noise)**：
对于序列 $\{\epsilon_t\}$，如果对所有 $t$ 满足以下条件，则称为白噪声过程：
*   均值为零：$E(\epsilon_t) = 0$
*   方差恒定：$E(\epsilon_t^2) = \operatorname{var}(\epsilon_t) = \sigma^2$
*   跨期不相关：$E(\epsilon_t \epsilon_\tau) = \operatorname{cov}(\epsilon_t, \epsilon_\tau) = 0$，对所有 $\tau \neq t$
如果进一步满足 $\{\epsilon_t\}$ 跨期独立，则称为**独立白噪声(independent white noise)**；如果进一步满足 $\epsilon_t \sim N(0, \sigma^2)$，则为**高斯白噪声(Gaussian white noise)**。

白噪声没有“持续性”，即如果 $\epsilon_t$ 异常高，并不倾向于让 $\epsilon_{t+1}$ 也异常高或低。因此它无法捕捉时间序列中有趣的持久性特征，我们需要用它来构建更真实的模型。

---

## 3. 移动平均模型 (MA)

**※【定义】** **移动平均模型(Moving Average, MA)**：
序列 $q$ 阶移动平均，记为 $\text{MA}(q)$，定义为白噪声的线性组合：
$x_t = \sum_{j=0}^q \beta_j \epsilon_{t-j}$ (1)
通常设定 $\beta_0 = 1$ 进行标准化。
带常数项的 $\text{MA}(q)$ 形式为：$x_t = \mu + \sum_{j=0}^q \beta_j \epsilon_{t-j}$。

**※【例题】** 考虑 $\text{MA}(1)$ 过程 $x_t = \epsilon_t + 0.5\epsilon_{t-1}$：
*   均值：$E(x_t) = 0$
*   方差：$\operatorname{var}(x_t) = \sigma^2(1 + 0.5^2) = 1.25\sigma^2$
*   一阶协方差：$\operatorname{cov}(x_t, x_{t-1}) = 0.5\sigma^2$
*   判断：因为存在非零协方差，所以**不是**白噪声过程。

**※【定义】** **后移算子(Back-Shift/Lag Operator)**：
定义为 $Ly_t = y_{t-1}$ 或 $By_t = y_{t-1}$。
高阶形式：$L^2 y_t = L(Ly_t) = y_{t-2}$。
$B$ 或 $L$ 代表时间平移。
利用滞后算子，$\text{MA}(q)$ 模型可写为：
$x_t = (1 + \beta_1 L + \dots + \beta_q L^q)\epsilon_t = (\sum_{j=0}^q \beta_j L^j)\epsilon_t$。

---

## 4. 自回归模型 (AR)

**※【定义】** **自回归模型(Autoregressive, AR)**：
序列 $p$ 阶自回归，记为 $\text{AR}(p)$，定义为：
$y_t = \phi_0 + \sum_{j=1}^p \phi_j y_{t-j} + \epsilon_t$ (2)
用滞后算子表示为：
$(1 - \phi_1 L - \phi_2 L^2 - \dots - \phi_p L^p)y_t = \phi_0 + \epsilon_t$
其中，$y_t$ 与其自身的滞后值相关，$\{\epsilon_t\}$ 是序列不相关的“冲击”。

---

## 5. 自回归移动平均模型 (ARMA)

**※【定义】** **自回归移动平均模型(Autoregressive Moving Average, ARMA)**：
记为 $\text{ARMA}(p, q)$，包含自回归部分和移动平均部分：
$y_t = \phi_0 + \sum_{j=1}^p \phi_j y_{t-j} + \sum_{j=0}^q \beta_j \epsilon_{t-j}$ (3)
用滞后算子表示为：
$(1 - \phi_1 L - \dots - \phi_p L^p)y_t = \phi_0 + (1 + \beta_1 L + \dots + \beta_q L^q)\epsilon_t$
*注意：如果 $q=0$，则退化为 $\text{AR}(p)$ 过程；如果 $p=0$，则退化为 $\text{MA}(q)$ 过程。*

---

## 6. 平稳性 (Stationarity)

**图片展示：中国实际GDP年化增长率，1979-2018**
经验问题：GDP增长率的均值水平是多少？
一个自然的想法是用历史均值来估计。但只有当每期的均值相同时，这种估计才是合理的。进一步地，只有当序列是**平稳序列(stationary series)** 时，其均值、方差和自相关才能用足够长的时间平均来很好地近似，这些统计量才能用于预测未来的行为。

**※【定义】** **平稳性(Stationarity)**：
*   **严平稳(Strict/Strong Stationarity)**：分布随时间不变。这是一个非常强的条件，在经验上很难验证。
*   **弱平稳(Weak Stationarity) / 协方差平稳(Covariance Stationarity)**：前二阶矩随时间不变。本课程主要关注弱平稳序列。

**※【定义】** **弱平稳(Weak Stationarity)**的数学条件：
如果一个随机过程 $\{y_t\}$ 具有有限均值和方差，且满足：
1.  期望恒定：$E(y_t) = \mu$，对所有 $t$
2.  方差恒定：$\operatorname{var}(y_t) = E[(y_t - \mu)^2] = \sigma_y^2$，对所有 $t$
3.  滞后 $k$ 阶自协方差仅与 $k$ 有关：$\gamma_k = \operatorname{cov}(y_t, y_{t-k}) = E[(y_t - \mu)(y_{t-k} - \mu)]$，对所有 $t$ 和任意 $k$。

则称其为弱平稳序列。

**※【定义】** 自相关 (Autocorrelation)
对于弱平稳时间序列 $\{y_t\}$，**滞后 $k$ 阶自相关(Lag-$k$ autocorrelation)** 或序列相关定义为：
$\rho_k \equiv \frac{\operatorname{cov}(y_t, y_{t-k})}{\operatorname{var}(y_t)} = \frac{\gamma_k}{\gamma_0}$
性质：$\rho_0 = 1$ 且 $\rho_k = \rho_{-k}$（$k \neq 0$）。
*注：序列相关的存在意味着收益率是可预测的，这指示了市场无效率。*

**※【例题】** 判断以下过程的弱平稳性：
1.  **白噪声**：均值为 0，方差 $\sigma^2$ 恒定，协方差为 0，**是**弱平稳的。
2.  **线性趋势 $y_t = \alpha + \beta t$**：$E(y_t)$ 随时间 $t$ 变化，均值不恒定，**不是**弱平稳的。
3.  **随机游走 $y_t = y_{t-1} + \epsilon_t$**：方差随时间 $t$ 线性递增，**不是**弱平稳的。

**※【定理】** MA(q) 过程的平稳性
对于所有 $t$ 和任意 $k > 0$：
$E(x_t) = 0$
$\operatorname{var}(x_t) = \sigma^2 (1 + \beta_1^2 + \dots + \beta_q^2)$
$\operatorname{cov}(x_t, x_{t-k}) = \begin{cases} \sigma^2(\beta_k + \beta_{k+1}\beta_1 + \dots + \beta_q\beta_{q-k}), & k \le q \\ 0, & k > q \end{cases}$
由于有限阶 MA 过程的均值和所有自协方差都不依赖于时间原点 $t$，因此**任何有限阶 $\text{MA}(q)$ 过程都是弱平稳的**。

**※【定理】** AR(1) 过程的平稳性
给定初始值 $y_0$，通过不断迭代：
$y_t = \phi_0 \sum_{j=0}^{t-1} \phi_1^j + \phi_1^t y_0 + \sum_{j=0}^{t-1} \phi_1^j \epsilon_{t-j}$ (4)
如果 $|\phi_1| \ge 1$，$\epsilon$ 的影响会随时间累积而不是衰减，因此公式(4)不可能是协方差平稳的。
**平稳性条件**：$|\phi_1| < 1$。
使用滞后算子表示为：$(1 - \phi_1 L)y_t = \phi_0 + \epsilon_t$ (5)
平稳性条件等价于：**AR 多项式 $(1 - \phi_1 z) = 0$ 的根位于单位圆外。**

**※【定理】** AR(p) 与 ARMA(p,q) 过程的平稳性
对于 $\text{AR}(p)$ 和 $\text{ARMA}(p,q)$：
**平稳性条件**：自回归多项式 $(1 - \phi_1 z - \phi_2 z^2 - \dots - \phi_p z^p) = 0$ 的所有根都必须位于单位圆外。
$\text{ARMA}(p,q)$ 过程的平稳性完全由其自回归部分决定。

**图片展示：不同 $\phi_1$ 取值下的 AR(1) 模拟数据图**

---

## 7. 自相关函数 (ACF) 与偏自相关函数 (PACF)

### 自相关函数 (ACF)

**※【定义】** **自相关函数(Autocorrelation Function, ACF)**：
$\gamma_k$ 关于 $k$ 的图称为自协方差函数；$\rho_k$ 关于 $k$ 的图称为自相关函数(ACF)或相关图(correlogram)。
用于识别和估计时间序列模型。

*※【例题】* MA(1) 模型
$\gamma_0 = (1 + \beta_1^2)\sigma^2$
$\gamma_1 = \beta_1\sigma^2$
$\gamma_k = 0$ ($k > 1$)
ACF: $\rho_1 = \frac{\beta_1}{1 + \beta_1^2}$，$\rho_k = 0$ ($k > 1$)。
*特点*：有限记忆(Finite memory)。$\text{MA}(1)$ 不记得两个时期之前发生的事。

*※【例题】* MA(q) 模型
$\gamma_0 = \sigma^2(1 + \sum_{j=1}^q \beta_j^2)$
$\gamma_k = \sigma^2(\beta_k + \beta_{k+1}\beta_1 + \dots + \beta_q\beta_{q-k})$ ($k \le q$)；$\gamma_k=0$ ($k>q$)。
ACF: $\rho_k = \frac{\beta_k + \beta_{k+1}\beta_1 + \dots + \beta_q\beta_{q-k}}{1 + \sum_{j=1}^q \beta_j^2}$ ($k \le q$)；$\rho_k=0$ ($k>q$)。
*特点*：$\text{MA}(q)$ 只与其前 $q$ 阶滞后值线性相关，是有限记忆模型。**ACF 在滞后 $q$ 阶处截尾(cuts off)。**

**※【定理】** 尤尔-沃克方程 (Yule-Walker Equations)
将差分方程两边乘以 $y_{t-k}$ ($k=0,1,2,\dots$) 并取期望，即可得到 Yule-Walker 方程，它是分析 ACF 的有效工具。

*※【例题】* AR(1) 模型
设定 $\phi_0=0$，Yule-Walker 方程给出：
$\gamma_0 = \phi_1\gamma_1 + \sigma^2$
$\gamma_k = \phi_1\gamma_{k-1}$ ($k \ge 1$)
因此，$\gamma_0 = \frac{\sigma^2}{1 - \phi_1^2}$，$\gamma_k = \frac{\sigma^2}{1 - \phi_1^2}\phi_1^k$。
ACF: $\rho_0 = 1$，$\rho_k = \phi_1^k$ ($k \ge 1$)。
*特点*：如果序列平稳，ACF 呈几何衰减收敛至 0。

*※【例题】* AR(2) 模型
$y_t = \phi_1 y_{t-1} + \phi_2 y_{t-2} + \epsilon_t$
Yule-Walker 方程：
$\gamma_0 = \phi_1\gamma_1 + \phi_2\gamma_2 + \sigma^2$ (7)
$\gamma_1 = \phi_1\gamma_0 + \phi_2\gamma_1$ (8)
$\gamma_k = \phi_1\gamma_{k-1} + \phi_2\gamma_{k-2}$ ($k \ge 2$) (9)
ACF: $\rho_0=1$，$\rho_1 = \frac{\phi_1}{1-\phi_2}$，$\rho_k = \phi_1\rho_{k-1} + \phi_2\rho_{k-2}$ ($k \ge 2$)。
*特点*：几何衰减（可能是震荡的）。$\gamma_0 = \frac{\sigma^2}{1 - \phi_1\rho_1 - \phi_2\rho_2}$，且 $\gamma_k = \gamma_0\rho_k$。

*※【例题】* AR(p) 模型
由 Yule-Walker 方程可得 $\rho_k = \sum_{j=1}^p \phi_j \rho_{k-j}$ ($k \ge p$)。若平稳，ACF 几何衰减。

*※【例题】* ARMA(1,1) 模型
Yule-Walker 方程推导出：
$\gamma_0 = \frac{1 + 2\beta_1\phi_1 + \beta_1^2}{1 - \phi_1^2}\sigma^2$
$\gamma_1 = \frac{(\phi_1 + \beta_1)(1 + \beta_1\phi_1)}{1 - \phi_1^2}\sigma^2$
因此，$\rho_1 = \phi_1 + \frac{\beta_1\sigma^2}{\gamma_0} \neq \phi_1$。
对于 $k \ge 2$，$\rho_k = \phi_1 \rho_{k-1}$。
*特点*：若平稳，从滞后 1 期后呈几何衰减收敛至 0。

*※【例题】* ARMA(p,q) 模型
$\rho_k = \sum_{j=1}^p \phi_j \rho_{k-j}$，对于 $k \ge \max\{p, q+1\}$。若平稳，最终几何衰减至 0。

---

### 偏自相关函数 (PACF)
**※【定义】** **偏自相关函数(Partial Autocorrelation Function, PACF)**：
滞后 $k$ 阶偏自相关衡量了在排除了中间值 $\{y_{t-1}, y_{t-2}, \dots, y_{t-k+1}\}$ 的影响后，$y_t$ 与 $y_{t-k}$ 之间的相关性。
可通过一组回归方程定义：
$y_t = \phi_{1,0} + \phi_{1,1} y_{t-1} + e_{1t}$
$y_t = \phi_{2,0} + \phi_{2,1} y_{t-1} + \phi_{2,2} y_{t-2} + e_{2t}$
$y_t = \phi_{3,0} + \phi_{3,1} y_{t-1} + \phi_{3,2} y_{t-2} + \phi_{3,3} y_{t-3} + e_{3t}$
... (13)
$\{\phi_{k,k}, k \ge 1\}$ 即为偏自相关函数。

**※【例题】** 利用 Yule-Walker 方程从 ACF 推导 PACF。
为了得到 $\phi_{2,2}$：
$E(y_t y_{t-1}) = \phi_{2,1}E(y_{t-1}^2) + \phi_{2,2}E(y_{t-2}y_{t-1}) \Rightarrow \rho_1 = \phi_{2,1} + \phi_{2,2}\rho_1$
$E(y_t y_{t-2}) = \phi_{2,1}E(y_{t-1}y_{t-2}) + \phi_{2,2}E(y_{t-2}^2) \Rightarrow \rho_2 = \phi_{2,1}\rho_1 + \phi_{2,2}$
解得：$\phi_{2,2} = \frac{\rho_2 - \rho_1^2}{1 - \rho_1^2}$。

*   **AR(p) 的 PACF**：因为 $k>p$ 时 $y_t$ 与 $y_{t-k}$ 无直接关联，所以 $\phi_{k,k}=0$ ($k>p$)。**AR(p) 的 PACF 在滞后 $p$ 阶处截尾(cut to zero)。**
*   **MA(1) 的 PACF**：不会跳变为 0，而是呈几何衰减（震荡或直接衰减取决于 $\beta_1$ 的符号）。MA(q) 的 PACF 表现类似。
*   **ARMA(p,q) 的 PACF**：最终衰减趋向于 0。衰减模式取决于 $\{\beta_j\}$ 系数。

### 理论 ACF 与 PACF 总结表
| 过程 | ACF | PACF |
| :--- | :--- | :--- |
| 白噪声 | 所有 $\rho_s = 0$ ($s \neq 0$) | 所有 $\phi_{ss} = 0$ |
| AR(1) | 几何/震荡衰减 | lag 1 后截尾 |
| AR(p) | 趋向 0（可能震荡） | lag p 后截尾 ($\phi_{ss}=0, s>p$) |
| MA(1) | lag 1 处有尖峰，之后截尾 | 几何/震荡衰减 |
| MA(q) | lag q 后截尾 | 几何/震荡衰减 |
| ARMA(p,q) | lag q 后开始衰减 | lag p 后开始衰减 |

---

## 8. 样本 ACF 与 PACF

在实践中，理论矩是未知的。我们可以用样本均值、方差、ACF 和 PACF 来估计真实的 DGP 参数。
### 样本ACF

**※【定义】** **样本自相关(Sample ACF)**：
样本均值：$\bar{y} = \frac{\sum_{t=1}^T y_t}{T}$
样本方差：$\hat{\sigma}_y^2 = \frac{\sum_{t=1}^T (y_t - \bar{y})^2}{T}$
滞后 $k$ 阶样本自相关：
$\hat{\rho}_k = \frac{\sum_{t=k+1}^T (y_t - \bar{y})(y_{t-k} - \bar{y})}{\sum_{t=1}^T (y_t - \bar{y})^2}$，$k \ge 1$。

**※【定理】** 显著性检验 (t 检验)
检验 $H_0: \rho_k = 0$ vs $H_1: \rho_k \neq 0$。
如果 $\{y_t\}$ 是平稳高斯序列，且对于 $j \ge k$ 有 $\rho_j = 0$（即 $\text{MA}(k-1)$ 且正态分布），则 $\hat{\rho}_k$ 渐近服从均值为 0，方差为 $\frac{1 + 2\sum_{j=1}^{k-1}\rho_j^2}{T}$ 的正态分布。
检验统计量：
$t\text{ ratio} = \frac{\hat{\rho}_k}{\sqrt{(1 + 2\sum_{j=1}^{k-1}\hat{\rho}_j^2)/T}} \xrightarrow{D} \mathcal{N}(0,1)$。

如果 $\{y_t\}$ 是独立同分布(i.i.d.)序列，则 $\hat{\rho}_k$ 渐近服从均值为 0，方差为 $\frac{1}{T}$ 的正态分布。检验统计量简化：$t\text{ ratio} = \frac{\hat{\rho}_k}{\sqrt{1/T}} \xrightarrow{D} \mathcal{N}(0,1)$。

**※【定理】** 联合检验 (Ljung-Box Q 统计量)
检验一组自相关系数是否联合为 0：$H_0: \rho_1 = \dots = \rho_m = 0$。
在 i.i.d. 假设下：
$Q(m) = T(T+2)\sum_{k=1}^m \frac{\hat{\rho}_k^2}{T-k} \xrightarrow{D} \chi_m^2$
决策规则：如果 $Q(m) > \chi_m^2(\alpha)$，则拒绝 $H_0$。
**※【注意】** Q 统计量也可用于检验估计的 $\text{ARMA}(p,q)$ 模型的残差是否表现为白噪声。此时自由度需要减去估计的参数个数 $g$：
$Q(m) = T(T+2)\sum_{k=1}^m \frac{\tilde{\rho}_k^2}{T-k} \xrightarrow{D} \chi_{m-g}^2$

### 样本 PACF
通过逐阶 OLS 回归进行估计，第 $k$ 个方程得到的 $\hat{\phi}_{k,k}$ 即为滞后 $k$ 阶样本 PACF。
对于平稳高斯 $\text{AR}(p)$ 模型，$\hat{\phi}_{k,k}$ 是 $\phi_{k,k}$ 的一致估计。$k>p$ 时，$\hat{\phi}_{k,k}$ 渐近服从 $\mathcal{N}(0, 1/T)$。样本 PACF 有助于识别 AR 模型的阶数 $p$。

---

## 9. 模型选择 (Model Selection)

模型识别通常不能仅依赖样本 ACF 或 PACF。我们追求**简约模型(parsimonious model)**，因为：
1. 估计量方差与自由度成反比。
2. 过于复杂的模型容易拟合数据中的特定噪声。

因此引入信息准则(Information Criteria)，包含拟合优度和参数惩罚两部分：
**※【定义】** **赤池信息准则(Akaike Information Criterion, AIC)**：
$AIC(I) = \log(\tilde{\sigma}_I^2) + \frac{2I}{T}$
其中 $I$ 是估计参数的个数，$\tilde{\sigma}_I^2 = \frac{SSR}{T}$。

**※【定义】** **贝叶斯信息准则(Bayesian Information Criterion, BIC)** 或施瓦茨准则(SIC)：
$BIC(I) = \log(\tilde{\sigma}_I^2) + \frac{I\log(T)}{T}$
选择使 AIC 或 BIC 最小的模型。

*   **比较**：BIC 具有更优的大样本性质，能渐近选出正确模型，而 AIC 倾向于过度拟合。但在小样本中，AIC 可能表现更好。
*   **冲突处理**：若两者选择不同，BIC 倾向于更简约模型，应检查其残差是否为白噪声；AIC 可能选大模型，应检查所有系数的 t 统计量是否显著。

---

## 10. Box-Jenkins 方法

Box 和 Jenkins (1970, 1976) 提出了一个系统化的三阶段 ARMA 建模方法：

### 第一步：识别 (Identification)
目视检查时间序列图、样本 ACF 和 PACF。将样本函数与理论 ARMA 过程的特征进行比较以提出候选模型。利用 AIC/BIC 辅助定阶。
*注意共性因子问题(Common factor problem)：相似的过程可能由截然不同的模型近似。每个系数应在常规水平下显著异于零。*

### 第二步：参数估计 (Estimation)
根据模型选择最小二乘法或极大似然估计(Maximum Likelihood)。
*   AR 模型：最小二乘法或 MLE。
*   MA 与 ARMA 模型：MLE。

**※【定义】** **可逆性(Invertibility)**：
MA 模型的可逆性是 AR 模型平稳性的对偶性质。
以 $\text{MA}(1)$：$x_t = \epsilon_t + \beta_1 \epsilon_{t-1}$ 为例，可逆条件为 $|\beta_1| < 1$。这使得我们可以将 $\epsilon_t$ 表示为观测值 $\{x_t, x_{t-1}, \dots\}$ 的收敛线性组合（即拥有 AR 表示）。
对于 $\text{MA}(q)$，可逆条件为：MA 多项式 $1 + \beta_1 z + \beta_2 z^2 + \dots + \beta_q z^q = 0$ 的所有根位于单位圆外。
$\text{ARMA}(p,q)$ 的可逆性完全由 MA 部分决定。

### 第三步：模型诊断检验 (Model Diagnostic Checking)
残差诊断：
*   绘制残差图，寻找异常值和拟合不佳的时期。
*   残差应为序列不相关：检查残差的 ACF、PACF 和 Q 统计量。
*   划分子样本进行检验。
*   进行样本外预测。

---

## 11. 预测 (Forecasting)

预测时间序列 $\{y_t\}$ 的未来值。
利用**条件期望(Conditional Expectations)** 构建预测。当前时点 $t$ 的信息集为 $\mathcal{F}_t \equiv \{y_t, y_{t-1}, y_{t-2}, \dots\}$。
最优 1 步向前预测为：
$E_t y_{t+1} \equiv E(y_{t+1}|\mathcal{F}_t) = \arg\min_g E[(y_{t+1} - g)^2|\mathcal{F}_t]$（均方误差最小化）。
假设 $E(\epsilon_{t+1}|\mathcal{F}_t) = E(\epsilon_{t+1}) = 0$。

*※【例题】* MA(1) 模型的预测
$x_t = \mu + \beta_1 \epsilon_{t-1} + \epsilon_t$
*   1 步预测：$E_t x_{t+1} = \mu + \beta_1 \epsilon_t$
*   2 步预测：$E_t x_{t+2} = E_t[\mu + \beta_1 \epsilon_{t+1} + \epsilon_{t+2}] = \mu$
*   $j \ge 2$ 步预测均为 $\mu$。因为模型只有 1 期记忆。
*   **预测误差**：
    1 步误差 $e_t(1) = \epsilon_{t+1}$，方差为 $\sigma^2$。
    $j \ge 2$ 步误差 $e_t(j) = \beta_1 \epsilon_{t+j-1} + \epsilon_{t+j}$，方差为 $(1+\beta_1^2)\sigma^2 = \operatorname{var}(x_t)$。
*   **95% 置信区间**：假设正态，
    1 步：$[\mu + \beta_1 \epsilon_t - 1.96\sigma, \mu + \beta_1 \epsilon_t + 1.96\sigma]$
    $j \ge 2$：$[\mu - 1.96\sqrt{1+\beta_1^2}\sigma, \mu + 1.96\sqrt{1+\beta_1^2}\sigma]$

*※【例题】* AR(1) 模型的预测
$y_t = \phi_0 + \phi_1 y_{t-1} + \epsilon_t$
*   1 步预测：$E_t y_{t+1} = \phi_0 + \phi_1 y_t$
*   2 步预测：$E_t y_{t+2} = \phi_0 + \phi_1 E_t y_{t+1} = \phi_0(1+\phi_1) + \phi_1^2 y_t$
*   一般地，$E_t y_{t+j} = \phi_0 \sum_{i=0}^{j-1} \phi_1^i + \phi_1^j y_t$。
*   当 $j \to \infty$ 时，$E_t y_{t+j} \to \frac{\phi_0}{1-\phi_1} = E(y_t)$。**对于任何平稳 ARMA 模型，条件预测最终都会收敛到无条件均值。** 长期来看，序列相依性不重要，预测值就是样本均值。
*   **预测误差**：
    1 步：$e_t(1) = \epsilon_{t+1}$
    $j$ 步：$e_t(j) = \sum_{i=0}^{j-1} \phi_1^i \epsilon_{t+j-i}$
    误差方差：$\operatorname{var}(e_t(j)) = \sigma^2 \sum_{i=0}^{j-1} \phi_1^{2i}$。误差方差随 $j$ 增大而增大，当 $j \to \infty$ 时收敛到 $\frac{\sigma^2}{1-\phi_1^2} = \operatorname{var}(y_t)$。

### 样本外预测 (Out-of-sample Forecasts)
将一部分观测值留出，用缩短的数据集估计备选模型，用于预测留出的观测值，并比较预测误差的性质。
*   注意：例如对于 AR(1)，实际预测误差 $e_{AR,1} = y_{101} - (\hat{\phi}_0 + \hat{\phi}_1 y_{100})$。它包含了未来值的不确定性和**参数估计的不确定性**。模型越复杂，参数不确定性越大。

### 预测评估指标
如果留出期有 $H$ 个观测值，预测误差为 $\{e_j\}_{j=1}^H$：
*   **均方预测误差(Mean Squared Prediction Error, MSPE)** = $\frac{1}{H}\sum_{j=1}^H e_j^2$ (也称为 MSE)
*   **平均绝对误差(Mean Absolute Error, MAE)** = $\frac{1}{H}\sum_{j=1}^H |e_j|$
*   **平均绝对百分比误差(Mean Absolute Percentage Error, MAPE)** = $\frac{1}{H}\sum_{j=1}^H |\frac{e_j}{y_{T+j}}| \times 100$

### 预测比较：Diebold-Mariano 检验
损失函数 $g(e_j)$（如均方误差 $e_j^2$）。对比模型 1 和 2 的差异损失：$d_j = g(e_{1j}) - g(e_{2j})$。
原假设：预测精度相等，$E(d_j) = 0$。
样本平均损失 $\bar{d} = \frac{1}{H}\sum_{j=1}^H [g(e_{1j}) - g(e_{2j})]$。
在弱条件下，中心极限定理表明 $\bar{d} \xrightarrow{D} \mathcal{N}(0, \operatorname{var}(\bar{d}))$。
若 $\{d_j\}$ 序列不相关，方差估计量为 $\hat{\gamma}/H$，检验统计量为 $\frac{\bar{d}}{\sqrt{\hat{\gamma}/H}} \xrightarrow{D} \mathcal{N}(0,1)$。若存在序列相关，可采用 Newey-West 估计。

---

## 12. 真实数据应用

**例 1：剔除趋势**
数据：成品价格。存在明显上升趋势，样本 ACF 不收敛至 0。
处理：取对数差分(Log difference)，即通货膨胀率 $dly_t = \log(finished_t) - \log(finished_{t-1})$。
经过差分后，样本 ACF 衰减，PACF 在 lag 5 之后截断。可估计 $\text{AR}(5)$ (模型 1)，通过 Wald 检验和重新估计得到更简约的模型 2 $\text{AR}(1,3,5)$ 和模型 3 $\text{AR}(1,3)$。AIC 偏向模型 2，BIC 偏向模型 3。最后通过样本外动态预测和静态预测进行评估。

**例 2：季节性 ARMA**
数据：某金融资产日简单收益率。
特征：ACF 和 PACF 在 lag 5, 10, 15, 20... 呈现周期性峰值，且整体呈衰减趋势。
处理：建立考虑“周期性”或“季节性”的 ARMA 模型。
**※【定义】** **季节 AR 与 MA 模型**：
*   加法季节 AR：$(1 - \phi_1 L - \phi_5 L^5) y_t = \epsilon_t$
*   乘法季节 AR：$(1 - \phi_1 L)(1 - \phi_5 L^5) y_t = \epsilon_t$
*   加法季节 MA：$y_t = (1 + \beta_1 L + \beta_5 L^5)\epsilon_t$
*   乘法季节 MA：$y_t = (1 + \beta_1 L)(1 + \beta_5 L^5)\epsilon_t$
同时包含季节项和非季节项：例如同时提取季节 AR 和 MA 项，再逐步添加非季节 AR/MA 项，依据 AIC/BIC 和残差诊断选出最优模型。

---

## 13. 补充：模型假设与 AR 特征根

**假设层级**：
从高斯白噪声 $\epsilon_t \sim i.i.d. \mathcal{N}(0, \sigma^2)$ 到 独立白噪声，再到弱化的白噪声：
$E[\epsilon_t] = 0$
$\operatorname{Var}(\epsilon_t) = \sigma^2$
$\operatorname{Cov}(\epsilon_t, \epsilon_\tau) = 0$ (对所有 $\tau \neq t$)。
同方差假设：$\operatorname{Var}(\epsilon_t|\mathcal{F}_{t-1}) = \sigma^2$；异方差(Heteroskedasticity)：$\operatorname{Var}(\epsilon_t|\mathcal{F}_{t-1}) = \sigma_t^2$。

**复杂特征根 (Complex Roots)**：
AR 多项式 $(1 - \phi_1 z - \phi_2 z^2 - \dots - \phi_p z^p) = 0$ 的根可能是复数。
例如：$p=2$ 且 $\phi_1^2 + 4\phi_2 < 0$ 时，AR 多项式的逆根是一对共轭复数：
$\alpha_1 = \frac{\phi_1 + i\sqrt{-\phi_1^2 - 4\phi_2}}{2} = r(\cos\theta + i\sin\theta)$
$\alpha_2 = \frac{\phi_1 - i\sqrt{-\phi_1^2 - 4\phi_2}}{2} = r(\cos\theta - i\sin\theta)$
其中 $r = \sqrt{-\phi_2}$ 是模长，$\theta = \arccos(\frac{\phi_1}{2\sqrt{-\phi_2}})$ 是幅角。
**平稳性条件**：$|\alpha_1| = |\alpha_2| = r < 1$（即特征根在单位圆内，逆根在单位圆外，这在时间序列图上往往表现为周期的震荡）。