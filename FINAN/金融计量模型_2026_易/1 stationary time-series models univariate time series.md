
## 1. 白噪声

**※【定义】白噪声(white noise,WN)**  
$\{\epsilon_t\}$ 称为白噪声过程，如果对所有 $t$：
- $E(\epsilon_t)=0$，均值零。
- $E(\epsilon_t^2)=var(\epsilon_t)=\sigma^2$，方差为 $\sigma^2$。
- $E(\epsilon_t\epsilon_\tau)=cov(\epsilon_t,\epsilon_\tau)=0,\quad \tau\ne t$，跨时间不相关。

若进一步要求 $\{\epsilon_t\}$ 跨时间独立，则称为独立白噪声(independent white noise)。  
若进一步有 $\epsilon_t\sim N(0,\sigma^2)$，则称为高斯白噪声(Gaussian white noise)。

时间序列特点：
时间顺序；
过去可能影响未来，即自相关(autocorrelation)；
可用数据建模并预测(forecast)。

## 2. 滞后算子

**※【定义】滞后算子(lag operator)**  
$Ly_t=y_{t-1}$ 或 $By_t=y_{t-1}$。  
$L^2y_t=L(Ly_t)=Ly_{t-1}=y_{t-2}$。  
$B$ 或 $L$ 表示时间平移，$Ly_t$ 是序列在 $t-1$ 时刻的值。

MA(q) 模型可写为：
$x_t=(1+\beta_1L+\cdots+\beta_qL^q)\epsilon_t=\left(\sum_{j=0}^{q}\beta_jL^j\right)\epsilon_t$。



## 3. MA 模型

**※【定义】MA(q) 模型(moving average,MA)**  
$x_t=\sum_{j=0}^{q}\beta_j\epsilon_{t-j}$，即白噪声的线性组合。  
由该式形成的序列称为 $q$ 阶移动平均，记为 MA(q)。  
通常令 $\beta_0=1$ 作为归一化。  
带均值的 MA(q)：$x_t=\mu+\sum_{j=0}^{q}\beta_j\epsilon_{t-j}$。

**※【性质】MA(q) 的矩**  
$E(x_t)=0$。  
$var(x_t)=\sigma^2(1+\beta_1^2+\cdots+\beta_q^2)=\sigma^2(1+\sum_{j=1}^{q}\beta_j^2)$。  
$cov(x_t,x_{t-k})=cov(\epsilon_t+\sum_{j=1}^{q}\beta_j\epsilon_{t-j},\epsilon_{t-k}+\sum_{j=1}^{q}\beta_j\epsilon_{t-k-j})$。  
当 $k\le q$ 时：
$cov(x_t,x_{t-k})=\sigma^2(\beta_k+\beta_{k+1}\beta_1+\beta_{k+2}\beta_2+\cdots+\beta_q\beta_{q-k})$。  
当 $k>q$ 时：
$cov(x_t,x_{t-k})=0$。  
其均值与所有自协方差均不依赖于时间原点 $t$，因此 MA(q) 是平稳的有限记忆(finite memory)模型。

**※【例子】MA(1)**  
$x_t=\epsilon_t+0.5\epsilon_{t-1}$。  
$E(x_t)=0$。  
$var(x_t)=(1+0.5^2)\sigma^2=1.25\sigma^2$。  
$cov(x_t,x_{t-1})=0.5\sigma^2$。  
$cov(x_t,x_{t-k})=0,\ k>1$。  
它不是白噪声，因为存在 lag-1 自相关。

**※【例子】带均值 MA(q)**  
$x_t=0.2+\epsilon_t+0.5\epsilon_{t-1}+0.3\epsilon_{t-2}$。  
$E(x_t)=0.2$。  
$var(x_t)=(1+0.5^2+0.3^2)\sigma^2=1.34\sigma^2$。  
$cov(x_t,x_{t-1})=(0.5+0.3\times0.5)\sigma^2=0.65\sigma^2$。  
$cov(x_t,x_{t-2})=0.3\sigma^2$。  
$cov(x_t,x_{t-3})=0$。

**※【性质】MA(1) 的 ACF**  
$\rho_1=\frac{\beta_1}{1+\beta_1^2}$，$\rho_k=0,\ k>1$。  
MA(1) 模型不记得两个时间周期之前发生的事情。  
一般 MA(q) 的 ACF 在 lag $q$ 处截尾(cut off)。

## 4. AR 模型

**※【定义】AR(p) 模型(autoregressive,AR)**  
$y_t=\phi_0+\sum_{j=1}^{p}\phi_jy_{t-j}+\epsilon_t$，  
或 $(1-\phi_1L-\phi_2L^2-\cdots-\phi_pL^p)y_t=\phi_0+\epsilon_t$。  
由该式形成的序列称为 $p$ 阶自回归过程，记为 AR(p)。  
$y_t$ 与自身滞后值有关，$\{\epsilon_t\}$ 是序列不相关的“冲击”。

**※【性质】AR(1) 迭代**  
$y_t=\phi_0+\phi_1y_{t-1}+\epsilon_t$。  
给定初值 $y_0$，反复迭代可得：
$y_t=\phi_0\sum_{j=0}^{t-1}\phi_1^j+\phi_1^t y_0+\sum_{j=0}^{t-1}\phi_1^j\epsilon_{t-j}$ (4)。  
若 $|\phi_1|\ge1$，冲击影响累积而不消散，因此不能协方差平稳。  
平稳条件：$|\phi_1|<1$。

**※【定义】AR 多项式与平稳条件**  
AR(p) 多项式：
$(1-\phi_1L-\phi_2L^2-\cdots-\phi_pL^p)y_t=\phi_0+\epsilon_t$。  
平稳条件：AR 多项式 $(1-\phi_1z-\phi_2z^2-\cdots-\phi_pz^p)=0$ 的根均位于单位圆外。  
ARMA(p,q) 过程的平稳条件完全由自回归部分决定。

## 5. ARMA 模型

**※【定义】ARMA(p,q) 模型(autoregressive moving average,ARMA)**  
$y_t=\phi_0+\sum_{j=1}^{p}\phi_jy_{t-j}+\sum_{j=0}^{q}\beta_j\epsilon_{t-j}$ (3)。  
或：
$(1-\phi_1L-\cdots-\phi_pL^p)y_t=\phi_0+(1+\beta_1L+\cdots+\beta_qL^q)\epsilon_t$。  
由该式形成的序列称为自回归移动平均模型，记为 ARMA(p,q)。

**※【性质】ARMA(p,q) 平稳条件**  
AR 多项式 $(1-\phi_1z-\phi_2z^2-\cdots-\phi_pz^p)=0$ 的根在单位圆外。  
平稳性条件完全由 AR 部分决定。

## 6. 平稳性

**※【定义】严平稳(strict stationarity)**  
分布随时间不变。该条件很强，实际中难以验证。

**※【定义】弱平稳/协方差平稳(weak stationarity/covariance stationarity)**  
有限均值与方差，且：
- $E(y_t)=\mu$ 对所有 $t$。
- $var(y_t)=E[(y_t-\mu)^2]=\sigma_y^2$ 对所有 $t$。
- $\gamma_k=cov(y_t,y_{t-k})=E[(y_t-\mu)(y_{t-k}-\mu)]$ 对所有 $t$ 和任意 $k$。

**※【定义】自相关(autocorrelation)**  
$\rho_k=\frac{cov(y_t,y_{t-k})}{var(y_t)}=\frac{\gamma_k}{\gamma_0}$。  
$\rho_0=1$，$\rho_k=\rho_{-k}$。

**※【性质】MA(q) 平稳性**  
MA(q) 的均值、方差、自协方差均与时间原点无关。  
带均值 MA(q) 同样平稳。  
MA(q) 是有限记忆模型。

**※【性质】AR(1) 平稳性**  
$y_t=\phi_0+\phi_1y_{t-1}+\epsilon_t$。  
平稳条件：$|\phi_1|<1$。

**※【性质】AR(p) 平稳性**  
$(1-\phi_1L-\phi_2L^2-\cdots-\phi_pL^p)y_t=\phi_0+\epsilon_t$。  
平稳条件：AR 多项式 $(1-\phi_1z-\phi_2z^2-\cdots-\phi_pz^p)=0$ 的根均在单位圆外。
证明：
充分性：定义；必要性：$\det(\lambda I- A)$.

**※【性质】ARMA(p,q) 平稳性**  
平稳条件完全由 AR 部分决定。  
AR 多项式根在单位圆外。

**※【定义】复根情形**  
若 $p=2$ 且 $\phi_1^2+4\phi_2<0$，逆根为共轭复根：
$\alpha_1=r(\cos\theta+i\sin\theta)$，  
$\alpha_2=r(\cos\theta-i\sin\theta)$。  
其中 $r=\sqrt{-\phi_2}$，$\theta=\arccos\left(\frac{\phi_1}{2\sqrt{-\phi_2}}\right)$。  
平稳条件：$|\alpha_1|=|\alpha_2|=r<1$。

---

## 7. ACF 与 PACF

**※【定义】自相关函数(autocorrelation function,ACF)**  
$\rho_k$ 对 $k$ 的图称为自相关函数或相关图(correlogram)。

**※【定义】偏自相关函数(partial autocorrelation function,PACF)**  
lag-$k$ 偏自相关是 $y_t$ 与 $y_{t-k}$ 在消除中间值 $\{y_{t-1},y_{t-2},\dots,y_{t-k+1}\}$ 影响后的相关性。  
通过回归：
$y_t=\phi_{1,0}+\phi_{1,1}y_{t-1}+e_{1t}$，  
$y_t=\phi_{2,0}+\phi_{2,1}y_{t-1}+\phi_{2,2}y_{t-2}+e_{2t}$，  
$y_t=\phi_{3,0}+\phi_{3,1}y_{t-1}+\phi_{3,2}y_{t-2}+\phi_{3,3}y_{t-3}+e_{3t}$，  
$\vdots$  
$\{\phi_{k,k},k\ge1\}$ 为偏自相关函数。

**※【性质】从 ACF 求 PACF**  
利用 Yule-Walker 方程，例如：
$\phi_{2,2}=\frac{\rho_2-\rho_1^2}{1-\rho_1^2}$。这里使用PACF带进去算。

**※【性质】AR(1) 的 ACF**  
$\gamma_0=\frac{\sigma^2}{1-\phi_1^2}$，  
$\gamma_k=\frac{\sigma^2}{1-\phi_1^2}\phi_1^k$。  
$\rho_0=1$，$\rho_k=\phi_1^k,\ k\ge1$。  
若平稳，ACF 几何衰减；若 $\phi_1<0$，振荡衰减。

**※【性质】AR(2) 的 ACF**  
$y_t=\phi_1y_{t-1}+\phi_2y_{t-2}+\epsilon_t$。  
$\rho_0=1$，  
$\rho_1=\frac{\phi_1}{1-\phi_2}$，  
$\rho_k=\phi_1\rho_{k-1}+\phi_2\rho_{k-2},\ k\ge2$。  
$\gamma_0=\frac{\sigma^2}{1-\phi_1\rho_1-\phi_2\rho_2}$，$\gamma_k=\gamma_0\rho_k$。

**※【性质】AR(p) 的 ACF**  
$\rho_k=\sum_{j=1}^{p}\phi_j\rho_{k-j},\ k\ge p$。  
平稳时 ACF 几何衰减或振荡衰减。

**※【性质】ARMA(1,1) 的 ACF**  
$y_t=\phi_1y_{t-1}+\epsilon_t+\beta_1\epsilon_{t-1}$。  
$\gamma_0=\frac{1+2\beta_1\phi_1+\beta_1^2}{1-\phi_1^2}\sigma^2$，  
$\gamma_1=\frac{(\phi_1+\beta_1)(1+\beta_1\phi_1)}{1-\phi_1^2}\sigma^2$。  
$\rho_1=\frac{(\phi_1+\beta_1)(1+\beta_1\phi_1)}{1+2\beta_1\phi_1+\beta_1^2}=\phi_1+\frac{\beta_1\sigma^2}{\gamma_0}\ne\phi_1$。  
$\rho_k=\phi_1\rho_{k-1},\ k\ge2$。

**※【性质】ARMA(p,q) 的 ACF**  
$\rho_k=\sum_{j=1}^{p}\phi_j\rho_{k-j},\ k\ge\max\{p,q+1\}$。  
若平稳，ACF 最终几何衰减或振荡衰减。

**※【性质】理论 ACF 与 PACF 总结**

| 过程                 | ACF                   | PACF              |
| ------------------ | --------------------- | ----------------- |
| 白噪声                | 所有 $\rho_s=0,\ s\ne0$ | 所有 $\phi_{s,s}=0$ |
| AR(1): $\phi_1>0$  | 直接几何衰减                | lag 1 后截断         |
| AR(1): $\phi_1<0$  | 振荡衰减                  | lag 1 后截断         |
| AR(p)              | 衰减趋于零，可振荡             | lag p 后截断         |
| MA(1): $\beta_1>0$ | lag 1 正尖峰，之后为 0       | 振荡衰减              |
| MA(1): $\beta_1<0$ | lag 1 负尖峰，之后为 0       | 几何衰减              |
| ARMA(1,1)          | lag 1 后几何或振荡衰减        | lag 1 后几何或振荡衰减    |
| ARMA(p,q)          | lag q 后衰减             | lag p 后衰减         |

有几个事实：当模型是充分的，就会有回归残差是时间序列独立的；当模型是不充分的，回归残差是序列相关的；当回归中加入了冗余变量，理论上的相关系数应当为零；PACF可以直接挑选出AR模型。
这里在计算上，带着方差总是行动不变的。计算 $\rho$ 族来计算会更加方便。


---

## 8. 样本 ACF、PACF 与检验

**※【定义】样本 ACF(sample ACF)**  
样本均值：$\bar y=\frac{\sum_{t=1}^{T}y_t}{T}$。  
样本方差：$\hat\sigma_y^2=\frac{\sum_{t=1}^{T}(y_t-\bar y)^2}{T}$。  
lag-$k$ 样本自相关：
$\hat\rho_k=\frac{\sum_{t=k+1}^{T}(y_t-\bar y)(y_{t-k}-\bar y)}{\sum_{t=1}^{T}(y_t-\bar y)^2},\ k\ge1$。  
$\{\hat\rho_1,\hat\rho_2,\dots\}$ 称为样本 ACF。

**※【检验】单个自相关检验**  
$H_0:\rho_k=0$，$H_1:\rho_k\ne0$。  
若 $\{y_t\}$ 是平稳高斯序列且 $\rho_j=0,\ j\ge k$，
则 $\hat\rho_k$ 渐近正态，均值 0，方差：$\frac{1+2\sum_{j=1}^{k-1}\rho_j^2}{T}$。  
统计量：$t=\frac{\hat\rho_k}{\sqrt{(1+2\sum_{j=1}^{k-1}\hat\rho_j^2)/T}}\xrightarrow{D}N(0,1)$。  
若 $\{y_t\}$ 为 i.i.d.，则方差为 $1/T$。

注意：这里检验的时候应该选取较大的m一直做检验且中间不应该停下来。

*※【例题】* 如何检验一个白噪声
白噪声有性质 $\hat{\rho}_{k} \stackrel{ D}{ \longrightarrow}N(0,1/T)$.
$H_0:\rho_k=0$，$H_1:\rho_k\ne0$。  
统计量：$t=\frac{\hat\rho_k}{\sqrt{1/T}}\xrightarrow{D}N(0,1)$。  

**※【检验】Ljung-Box Q 统计量**  
$H_0:\rho_1=\cdots=\rho_m=0$，$H_1:\rho_i\ne0$ 对某个 $1\le i\le m$。  
在 i.i.d. 假设下：$Q(m)=T(T+2)\sum_{k=1}^{m}\frac{\hat\rho_k^2}{T-k}\xrightarrow{D}\chi_m^2$。  
拒绝域：$Q(m)>\chi_m^2(\alpha)$。  
残差诊断时：$Q(m)=T(T+2)\sum_{k=1}^{m}\frac{\tilde\rho_k^2}{T-k}\xrightarrow{D}\chi_{m-g}^2$，  
其中 $\tilde\rho_k$ 是残差样本 ACF，$g$ 是估计参数个数。可以用来判断我们选取的模型是否是充分的。

**※【定义】样本 PACF(sample PACF)**  
逐阶用 OLS 估计：
$y_t=\phi_{k,0}+\phi_{k,1}y_{t-1}+\cdots+\phi_{k,k}y_{t-k}+e_{kt}$。  
第 $k$ 个方程得到的 $\hat\phi_{k,k}$ 称为 lag-$k$ 样本 PACF。  
对平稳高斯 AR(p)，$\hat\phi_{k,k}$ 收敛到 $\phi_{k,k}$；  
$k>p$ 时 $\phi_{k,k}=0$，$\hat\phi_{k,k}$ 渐近 $N(0,1/T)$。这里可以对 $\hat{\phi}_{k,k}=0$ 与否进行检验。

1. 一般先用PACF定一下阶数，也就是AR(p). 得到回归残差 ut. 
2. 计算 ut的ACF和PACF. 拟合一个MA(q)

---

## 9. Box-Jenkins 建模

**※【方法】Box-Jenkins 方法(Box-Jenkins approach)**  
三阶段：
- 识别(identification)：观察时间图、样本 ACF、样本 PACF，与理论 ARMA 过程比较，提出候选模型；用 AIC、BIC 选择模型。
- 估计(estimation)：
	- AR 模型可用最小二乘或最大似然；
	- MA 与 ARMA 模型通常用最大似然。
- 模型诊断检查(diagnostic checking)：残差图、残差 ACF/PACF、Q 统计量、子样本检验、样本外预测。

**※【准则】信息准则(information criteria)**  
$AIC(I)=\log(\tilde\sigma_I^2)+\frac{2I}{T}$。  
$BIC(I)=\log(\tilde\sigma_I^2)+\frac{I\log(T)}{T}$。  
其中 $I$ 是估计参数个数，$\tilde\sigma_I^2=\frac{SSR}{T}$。  
选择 AIC 或 BIC **最小**的模型。  
BIC 大样本性质更好，渐近选择正确模型；AIC 偏向过参数化。小样本中 AIC 可能更好。  
若二者选择不同：BIC 模型更简约，应检查残差是否白噪声；AIC 模型可能过参数化，应检查系数 t 统计量是否显著。

数据处理步骤
1. 先画时序图
2. 计算样本ACF、样本PACF

---

## 10. 可逆性

**※【定义】可逆性(invertibility)**  
MA(q) 模型：
$x_t=\epsilon_t+\sum_{j=1}^{q}\beta_j\epsilon_{t-j}$，  
或 $x_t=(1+\beta_1L+\beta_2L^2+\cdots+\beta_qL^q)\epsilon_t$。  
若 MA 多项式 $1+\beta_1z+\beta_2z^2+\cdots+\beta_qz^q=0$ 的根均在单位圆外，则模型可逆。  
可逆性意味着模型有 AR 表示。  
ARMA(p,q) 的可逆性完全由 MA 部分决定。

**※【性质】MA(1) 可逆性**  
$x_t=\epsilon_t+\beta_1\epsilon_{t-1}$，$\epsilon_t\sim i.i.d.N(0,\sigma^2)$。  
可逆条件：$|\beta_1|<1$。  
可逆时，$\epsilon_t=-\beta_1\epsilon_{t-1}+x_t$，可写成观测的 AR 表示。  
MA 模型的可逆性是 AR 模型平稳性的对偶性质。

---

## 11. 预测

**※【定义】条件期望预测**  
信息集 $\mathcal F_t=\{y_t,y_{t-1},y_{t-2},\dots\}$。  
最优 1 步预测：
$E_t y_{t+1}=E(y_{t+1}|\mathcal F_t)=\arg\min_g E[(y_{t+1}-g)^2|\mathcal F_t]$。  
$j$ 步预测：
$E_t y_{t+j}=E(y_{t+j}|\mathcal F_t)$。  
假设 $E(\epsilon_{t+1}|\mathcal F_t)=E(\epsilon_{t+1})=0$。

**※【性质】MA(1) 预测**  
$x_t=\mu+\beta_1\epsilon_{t-1}+\epsilon_t$。  
1 步预测：
$E_t x_{t+1}=\mu+\beta_1\epsilon_t$。  
$j\ge2$ 步预测：
$E_t x_{t+j}=\mu=E(x_t)$。  
1 步预测误差：
$e_t(1)=x_{t+1}-E_t x_{t+1}=\epsilon_{t+1}$，方差 $\sigma^2$。  
$j\ge2$ 预测误差方差：
$(1+\beta_1^2)\sigma^2=var(x_t)$。  
95% 置信区间：
1 步：$[\mu+\beta_1\epsilon_t-1.96\sigma,\ \mu+\beta_1\epsilon_t+1.96\sigma]$。  
$j\ge2$：$[\mu-1.96\sqrt{1+\beta_1^2}\sigma,\ \mu+1.96\sqrt{1+\beta_1^2}\sigma]$。

**※【性质】AR(1) 预测**  
$y_t=\phi_0+\phi_1y_{t-1}+\epsilon_t$。  
1 步预测：
$E_t y_{t+1}=\phi_0+\phi_1y_t$。  
2 步预测：
$E_t y_{t+2}=\phi_0+\phi_1E_t y_{t+1}=\phi_0+\phi_0\phi_1+\phi_1^2y_t$。  
递推：
$E_t y_{t+j}=\phi_0+\phi_1E_t y_{t+j-1}$。  
闭式：
$E_t y_{t+j}=\phi_0\sum_{i=0}^{j-1}\phi_1^i+\phi_1^j y_t$。  
当 $j\to\infty$：
$E_t y_{t+j}\to\frac{\phi_0}{1-\phi_1}=E(y_t)$。  
对任何平稳 ARMA 模型，条件预测 $y_{t+j}$ 当 $j\to\infty$ 时收敛到无条件均值。长期预测中序列依赖不再重要。

预测误差：
$e_t(1)=\epsilon_{t+1}$。  
$e_t(2)=\phi_1\epsilon_{t+1}+\epsilon_{t+2}$。  
$e_t(j)=\sum_{i=0}^{j-1}\phi_1^i\epsilon_{t+j-i}$。  
$var(e_t(j))=\sigma^2\sum_{i=0}^{j-1}\phi_1^{2i}$。  
当 $j\to\infty$：
$var(e_t(j))\to\frac{\sigma^2}{1-\phi_1^2}=var(y_t)$。  
95% 置信区间：
$[E_t y_{t+j}-1.96\sqrt{var(e_t(j))},\ E_t y_{t+j}+1.96\sqrt{var(e_t(j))}]$。

**※【定义】样本外预测评价**  
保留部分观测不参与估计，用估计模型预测。  
均值平方预测误差(mean squared prediction error,MSPE)：
$MSPE=\frac{1}{H}\sum_{j=1}^{H}e_j^2$。  
平均绝对误差(mean absolute error,MAE)：
$MAE=\frac{1}{H}\sum_{j=1}^{H}|e_j|$。  
平均绝对百分比误差(mean absolute percentage error,MAPE)：
$MAPE=\frac{1}{H}\sum_{j=1}^{H}\left|\frac{e_j}{y_{T+j}}\right|\times100$。  
通常选择 MSPE、MAE 或 MAPE 最小的模型。

**※【检验】预测精度比较**  
损失 $g(e_j)$，差分损失：
$d_j=g(e_{1j})-g(e_{2j})$。  
原假设：等预测精度，$E(d_j)=0$。  
平均损失：
$\bar d=\frac{1}{H}\sum_{j=1}^{H}d_j$。  
若 $\{d_j\}$ 序列不相关，方差估计为 $\hat\gamma/H$，则：
$\frac{\bar d}{\sqrt{\hat\gamma/H}}\xrightarrow{D}N(0,1)$。  
若 $\{d_j\}$ 序列相关，需用 Newey-West 等估计 $var(\bar d)$。

---

## 12. 季节 ARMA 模型

**※【定义】季节 ARMA(seasonal ARMA)**  
数据可建模为考虑周期或季节性的 ARMA 模型。  
季节 AR 系数例如：
$(1-\phi_1L-\phi_5L^5)y_t=\epsilon_t$，加性季节；
$(1-\phi_1L)(1-\phi_5L^5)y_t=\epsilon_t$，乘性季节。  
季节 MA 系数例如：
$y_t=(1+\beta_1L+\beta_5L^5)\epsilon_t$，加性季节；
$y_t=(1+\beta_1L)(1+\beta_5L^5)\epsilon_t$，乘性季节。  
拟合模型应同时考虑季节模式与非季节模式。

---

## 13. 假设层次

**※【定义】白噪声假设层次**  
高斯白噪声：
$\epsilon_t\sim i.i.d.N(0,\sigma^2)$。  
独立白噪声：
$\epsilon_t$ 独立同分布，$E(\epsilon_t)=0$，$var(\epsilon_t)=\sigma^2$。  
白噪声：
$E(\epsilon_t)=0$，$var(\epsilon_t)=\sigma^2$，$cov(\epsilon_t,\epsilon_\tau)=0,\ \tau\ne t$。

同方差(homoskedasticity)：
$var(\epsilon_t|\mathcal F_t)=\sigma^2$。  
异方差(heteroskedasticity)：
$var(\epsilon_t|\mathcal F_t)=\sigma_t^2$。  
均值独立：
$E(\epsilon_{t+1}|\mathcal F_t)=E(\epsilon_{t+1})=0$。