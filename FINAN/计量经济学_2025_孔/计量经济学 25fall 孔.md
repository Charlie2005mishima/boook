# 第一章 绪论
- 数据的要求：完整，准确，可比性，一致性
- 数据类型：面板，截面，序列
- 估计方法：OLS，LM，GMM，WLS，2SLS
- 检验方法：拟合优度，显著
- 分析方法：边际分析，弹性分析
---
# 第二章 一元线性回归模型
## 一、回归分析概述
### 1. 变量间的关系
- **函数关系**：确定性关系，如圆面积与半径。
- **相关关系**：非确定性关系，如农作物产量与气温、降雨量等。
	- 正相关，负相关，不相关，非线性相关
### 2. 相关分析 vs 回归分析
- **相关分析**：对称处理变量，无因果关系。
- **回归分析**：区分因变量与自变量，研究依赖关系，可用于预测。
	- 条件分布，条件期望，回归线，回归函数
### 3. 总体回归函数（PRF）
- 总体回归函数：$E(Y|X_i)=f(X_i)$
- 总体回归**函数**：$E(Y|X_i) = \beta_0 + \beta_1 X_i$
- 随机扰动项（离差）：$\mu_i=Y_i-E(Y|X_i)$
- 总体回归**模型**：$Y_i =\beta_0 + \beta_1 X_i + \mu_i$
### 4. 样本回归函数（SRF）
- 用样本数据估计的回归线：
- 样本回归**函数**：$\hat{Y}_i = \hat{\beta}_0 + \hat{\beta}_1 X_i$
- 样本回归**模型**：$Y_i = \hat{\beta}_0 + \hat{\beta}_1 X_i+e_i$
- 残差项（residual）： $e_i = \hat{\mu_i} = Y_i - \hat{Y}_i$ 
## 二、一元线性回归模型的参数估计

### 1. 经典假设【必考】
1. 回归模型是正确设定的
2. 解释变量 $X$在所抽取的样本中具有**变异性**，而且随着样本容量的无限增加，解释变量$X$的**样本方差**趋于一个非零的有限常数（平稳性）
3. 随机误差项 $\mu_i$均值为零，即$E(\mu_i|X) = 0$；又有$cov(X_i,\mu_i)=0$，则$E(\mu_i)=0$
4. 随机误差项 $\mu_i$具有同方差性与不序列相关性，即$var(\mu_i|X)=\sigma_{\mu}^2\ \&\ cov(\mu_i,\mu_j|X)=0$
5. 随机误差项 $\mu_i$服从正态分布

假设一定是针对总体的
零均值，同方差，序列不相关
可以把假设理解成一个生成机制，离差与解释变量没有关系
### 2. 普通最小二乘法（OLS）
- 最小化残差平方和：$\min Q = \sum (Y_i - \hat{Y}_i)^2$
- ***正规方程组：***
	- $\begin{cases}\sum e_i=0\\\sum e_iX_i=0\end{cases}$
	- $\begin{cases}\sum (Y_i-\hat{\beta_0}-\hat{\beta}_1X_i)=0\\\sum (Y_i-\hat{\beta_0}-\hat{\beta}_1X_i)X_i=0\end{cases}$
- $\begin{cases}\hat{\beta}_1 = \frac{n\sum X_iY_i-\sum X_i \sum Y_i}{n\sum X_i^2-(\sum X_i)^2}=\frac{cov(X,Y)}{var(X)}=\rho_{XY}\frac{\sigma_Y}{\sigma_X}\\\hat{\beta}_0 = \bar{Y} - \hat{\beta}_1 \bar{X}\end{cases}$
- 离差形式：$\hat{\beta}_1 = \frac{\sum x_i y_i}{\sum x_i^2}, \quad \hat{\beta}_0 = \bar{Y} - \hat{\beta}_1 \bar{X}$
	- ***离差形式***：$\hat{y}_i=\hat{\beta}_1x_i$
称呼上标准差指称数据的标准差，标准误指称统计量的标准差
### 3. 最大似然估计（ML）
- 条件：总体服从某一种分布（这里我们取正态分布）
- $max\{P\{Y_1,Y_2,...,Y_n\}=\Pi_1^n \frac{1}{\sqrt{2\pi}\sigma}exp(-\frac{(Y_i-\hat{\beta}_0-\hat{\beta}_1X_i)^2}{2\sigma^2})\}$
- 可以得到$\begin{cases}\hat{\beta}_0=\bar{Y}-\hat{\beta}_1\bar{X}\\\hat{\beta}_1=\frac{\sum x_iy_i}{\sum x_i^2}\end{cases}$，与最小二乘法点估计恰好相同；但$\sigma_{ML}^2=\frac{\sum e_i^2}{n}$，并不是无偏估计量

## 三、最小二乘估计量的统计性质
### 1. 高斯-马尔可夫定理【证明】
- 在经典假设下，OLS估计量是**最小方差的线性无偏估计量（BLUE）**
  - **线性性**：即估计量$\hat{\beta}_0,\hat{\beta}_1$是$Y_i$线性组合
	  - $\hat{\beta}_1=-\frac{\sum x_iY_i}{\sum x_i^2}$
  - **无偏性**：即估计量的均值$\hat{\beta}_0,\hat{\beta}_1$等于总体回归参数真值$\beta_0,\beta_1$
  - **有效性（最小方差）**：即在所有线性无偏估计量中，最小二乘法得到的估计量具有最小方差
	  - 可以计算得到$var(\beta_1)=\frac{\sigma^2}{\sum x_i^2},var(\beta_0)=\frac{\sum X_i^2}{n \sum x_i^2}$，再在线性假设上添加一点点
	  - 大样本的一致性$plim(\hat{\beta_1})$

### 2. 估计量的分布
- 若 $\mu_i \sim N(0, \sigma^2)$，则：
  $$\hat{\beta}_1 \sim N\left(\beta_1, \frac{\sigma^2}{\sum x_i^2}\right), \quad \hat{\beta}_0 \sim N\left(\beta_0, \frac{\sigma^2 \sum X_i^2}{n \sum x_i^2}\right)$$
	- 注：$\frac{\sigma^2 \sum X_i^2}{n \sum x_i^2}=(\frac{1}{n}+\frac{\bar{X}^2}{\sum x_i^2})$

### 3. 随机误差项方差 $\sigma^2$的估计
- 无偏估计（也称作回归标准误）：
$$\hat{\sigma}^2 = \frac{\sum e_i^2}{n-2}$$

## 四、一元线性回归模型的基本统计检验

### 1. 拟合优度检验
- **判定系数** $R^2$：
  - $R^2 = \frac{ESS}{TSS} = 1 - \frac{RSS}{TSS}=\frac{\sum \hat{y}_i^2}{\sum y_i^2}=\hat{\beta}_1^2(\frac{\sum x_i^2}{\sum y_i^2})=r^2$
	  - TSS：总体平方和；RSS：残差平方和；ESS：回归平方和
- 反映模型对样本数据的拟合程度，越接近1越好。

### 2. 变量的显著性检验（t检验）
- 检验 $H_0: \beta_1 = 0$，构造 t 统计量：
  $t_{\hat{\beta}_1} = \frac{\hat{\beta}_1}{S_{\hat{\beta}_1}} \sim t(n-2)$
- 若 $|t_{\hat{\beta}_1}| > t_{\alpha/2}(n-2)$，拒绝原假设，认为变量显著不等于零。

### 3. 参数的置信区间
- 在置信水平 $1-\alpha$下：$\hat{\beta}_1 \pm t_{\alpha/2} \cdot S_{\hat{\beta}_1}$
- 缩小置信区间：
	- 增加样本容量
	- 提高拟合程度
	- 提高样本分散程度

## 五、预测问题

### 1. 点预测
- 给定 $X_0$，预测$Y_0$：$\hat{Y}_0 = \hat{\beta}_0 + \hat{\beta}_1 X_0$
$$\hat{Y}_0\sim N(Y_0,\sigma^2(\frac{1}{n} + \frac{(X_0 - \bar{X})^2}{\sum x_i^2})$$

### 2. 区间预测
- **条件均值** $E(Y|X_0)$ 的置信区间：
	 - $\hat{Y}_0 \pm t_{\alpha/2} \cdot \hat{\sigma} \sqrt{\frac{1}{n} + \frac{(X_0 - \bar{X})^2}{\sum x_i^2}}$ 
	 - $var(\hat{Y_0})=var(\hat{\beta}_0)+X_0^2var (\hat{\beta}_1)+2X_0cov(\hat{\beta}_0,\hat{\beta}_1)$
	 - $cov(\hat{\beta}_0,\hat{\beta}_1)=-\sigma^2\frac{\bar{X}}{\sum x_i^2}$
	 - t统计量：$t=\frac{\hat{Y}_0}{\sqrt{\sigma^2(\frac{1}{n} + \frac{(X_0 - \bar{X})^2}{\sum x_i^2})}}\sim t(n-2)$

- **个别值** $Y_0$ 的预测区间：
	- $Y_0\sim N(\beta_0+\beta_1 X_0,\sigma^2)$
	- $\hat{Y}_0-Y_0\sim N(0,\sigma^2(1 + \frac{1}{n} + \frac{(X_0 - \bar{X})^2}{\sum x_i^2}))$
		- 前者是$Y_i$线性的，后者是$\mu$的，所以不相关
	 - $Y_0\in\hat{Y}_0 \pm t_{\alpha/2} \cdot \hat{\sigma} \sqrt{1 + \frac{1}{n} + \frac{(X_0 - \bar{X})^2}{\sum x_i^2}}$

## 六、实例与应用

### 例：家庭消费支出与可支配收入
- 样本回归方程：$\hat{Y}_i = 142.4 + 0.67X_i$
- $R^2 = 0.9935$，拟合优度高
- t 检验显著，置信区间可计算

## 七、总结

- 一元线性回归是计量经济学的基础模型
- OLS 是常用估计方法，具有良好统计性质
- 需进行模型检验（拟合优度、显著性、置信区间）
- 可用于条件均值与个别值的预测

# 第三章 多元线性回归模型
## §3.1 多元线性回归模型

### 一、模型设定

**一般形式**：
$$Y_i = \beta_0 + \beta_1 X_{i1} + \beta_2 X_{i2} + \cdots + \beta_k X_{ik} + \mu_i$$

**矩阵形式**：
$$\mathbf{Y} = \mathbf{X}\boldsymbol{\beta} + \boldsymbol{\mu}$$

其中：
- $\mathbf{Y}$: $n \times 1$ 因变量向量
- $\mathbf{X}$: $n \times (k+1)$ 设计矩阵（**第一列全为1**）
- $\boldsymbol{\beta}$: $(k+1) \times 1$ 参数向量
- $\boldsymbol{\mu}$: $n \times 1$ 误差项向量

**样本回归函数**：
$$\hat{Y_i} = \hat{\beta_0} + \hat{\beta_1} X_{i1} + \hat{\beta_2} X_{i2} + \cdots + \hat{\beta_k} X_{ik}$$

### 二、经典假设【记忆】
1. 回归模型是正确设定的
2. 解释变量在所抽取的样本中具有==变异性==，且变量之间不存在严格线性相关性（无完全多重共线性）：$Rank(\mathbf{X}) = k+1$
	随着样本量的无限增加，$X'X/n$依概率收敛于一可逆有限常矩阵$Q$，即
$$P\lim_{n\to\infty}\frac{X'X}{n}\to Q$$
3. **条件零均值**：给定任意解释变量，随机==误差项均值为零==，即$E(\mu_i | \mathbf{X}) = 0$
4. **同方差性**：随机扰动项具有==条件同方差==与==序列不相关性==，即$Var(\mu_i | \mathbf{X}) = \sigma^2,\quad Cov(\mu_i,\mu_j|\mathbf{X})=0$ ($i \neq j$)
$$E(\mu\mu')=\left[ \begin{matrix}var(\mu_1) & \cdots&cov(\mu_1,\mu_n)\\ &\cdots &\\cov(\mu_1,\mu_n)&\cdots& var(\mu_n)\end{matrix}\right]=\sigma^2\bf{I}$$
5. **正态性**（可选）：随机扰动项服从零均值同方差的正态分布，即$\mu_i | \mathbf{X} \sim N(0, \sigma^2)\quad \mu \sim N(0, \sigma^2\bf{I})$

> 大样本的经典假设：
> OLS估计量在大样本情况下具有一致性、渐进正态性、渐进有效性和渐进偏差等性质

## §3.2 参数估计

### 一、普通最小二乘法(OLS)

**目标函数**：
$$Q = \sum e_i^2 = (\mathbf{Y} - \mathbf{X}\hat{\boldsymbol{\beta}})'(\mathbf{Y} - \mathbf{X}\hat{\boldsymbol{\beta}})$$

**正规方程组**：
$$\mathbf{X}'\mathbf{X}\hat{\boldsymbol{\beta}} = \mathbf{X}'\mathbf{Y}$$

**OLS估计量**：
$$\hat{\boldsymbol{\beta}} = (\mathbf{X}'\mathbf{X})^{-1}\mathbf{X}'\mathbf{Y}$$

**残差性质**：$\mathbf{X}'\mathbf{e} = 0$或$\begin{cases}\sum e_i=0\\\sum_i X_{ij}e_i=0\end{cases}$

**离差形式**（降维）：$y_i=\mathbf{\beta x}+e_i$，得到$\hat\beta=(\mathbf{x'x})^{-1}\mathbf{x'Y}$

注：$\mathbf{X}=\begin{bmatrix}1,X_{11},X_{12},\cdots,X_{1k}\\1,X_{21},X_{22},\cdots,X_{2k}\\\cdots\\1,X_{n1},X_{n2},\cdots,X_{nk}\end{bmatrix}_{n\times(k+1)}$
信息矩阵：$\mathbf{X}'\mathbf{X}_{(k+1)\times(k+1)}$

也可以表示为Cramer的形式，这里以二元为例
$\hat{\beta}_{1}=\frac{\det \begin{bmatrix}n & \sum y_{i} & \sum x_{i2} \\ \sum x_{i1} &  \sum x_{i1}y_{i} & \sum x_{i1}x_{i2} \\ \sum x_{i2} & \sum x_{i2}y_{i}  & \sum x_{i2}^{2}\end{bmatrix}}{\det \begin{bmatrix}n & \sum x_{i1} & \sum x_{i2} \\ \sum x_{i1} &  \sum x_{i1}^{2} & \sum x_{i1}x_{i2} \\ \sum x_{i2} & \sum x_{i1}x_{i_{2}}  & \sum x_{i2}^{2}\end{bmatrix}}=\frac{S_{11}S_{1y}-S_{12}S_{1y}}{S_{11}S_{22}-S_{12}^{2}}$


### 二、最大似然估计(ML)

**似然函数**：
$$L(\boldsymbol{\beta}, \sigma^2) = \frac{1}{(2\pi\sigma^2)^{n/2}} \exp\left(-\frac{1}{2\sigma^2}(\mathbf{Y}-\mathbf{X}\boldsymbol{\beta})'(\mathbf{Y}-\mathbf{X}\boldsymbol{\beta})\right)$$

**ML估计量**：
- $\hat{\boldsymbol{\beta}}_{ML} = \hat{\boldsymbol{\beta}}_{OLS}$
- $\hat{\sigma}^2_{ML} = \frac{\mathbf{e}'\mathbf{e}}{n}$

### 三、估计量性质

1. **线性性**：$\hat{\boldsymbol{\beta}} = \mathbf{C}\mathbf{Y}$
2. **无偏性**：$E(\hat{\boldsymbol{\beta}}) = \boldsymbol{\beta}$
3. **有效性**（Gauss-Markov定理）：
   $$Cov(\hat{\boldsymbol{\beta}}) = \sigma^2(\mathbf{X}'\mathbf{X})^{-1}$$
4. **一致性**：$plim\ \hat{\boldsymbol{\beta}} = \boldsymbol{\beta}$

### 四、方差估计

**误差方差的无偏估计**：
$$\hat{\sigma}^2 = \frac{\mathbf{e}'\mathbf{e}}{n-k-1}$$

**系数估计量的方差**：
$$Var(\hat{\beta}_j) = \sigma^2 c_{jj}=\frac{\sigma^2}{\sum x_j^2}·VIF_j\quad Se(\hat{\beta}_j) = \frac{\sum e_i^2}{n-k-1} c_{jj}$$
$$Cov(\hat{\beta})=\sigma^2(\mathbf{X'X})^{-1}$$
其中 $c_{jj}$ 是 $(\mathbf{X}'\mathbf{X})^{-1}$ 的第 $j$ 个对角元素

## §3.3 统计检验
一般认为样本量要大于$3(k+1)$【记】

### 一、拟合优度检验

**总平方和分解**：
$$TSS = ESS + RSS$$

**可决系数**：
$$R^2 = \frac{ESS}{TSS} = 1 - \frac{RSS}{TSS}$$

**ANOVA表**：

|     | 平方和                             | 自由度   | 约束                      |
| --- | ------------------------------- | ----- | ----------------------- |
| 回归  | $ESS=\sum(\hat{Y_i}-\bar{Y})^2$ | k     | k+1个参数，加上$\beta_0$被均值捕获 |
| 残差  | $RSS=\sum(Y_i-\hat{Y_i})^2$     | n-k-1 | k+1个离差方程                |
| 总变差 | $TSS=\sum(Y_i-\bar{Y})^2$       | n-1   | 均值一个限制                  |


**调整的可决系数**：
$$\bar{R}^2 = 1 - \frac{RSS/(n-k-1)}{TSS/(n-1)}\leq R^2$$

**信息准则**（是否纳入新变量）：
- 赤池信息准则 AIC: $\ln\left(\frac{\mathbf{e}'\mathbf{e}}{n}\right) + \frac{2(k+1)}{n}$，$\frac{2(k+1)}{n}$又可以叫做惩罚函数
- SC: $\ln\left(\frac{\mathbf{e}'\mathbf{e}}{n}\right) + \frac{k}{n}\ln n$
- 仅当所增加的解释变量能够减少AIC值或SC值时才在元模型中增加解释变量
- 代码
```stata
qui reg y x1 x2
estat ic
```
### 二、方程显著性检验(F检验)

**假设**：
- $H_0: \beta_1 = \beta_2 = \cdots = \beta_k = 0$
- $H_1: \beta_j$ 不全为0

**F统计量**（在一定意义上就是在比较ESS和RSS的大小关系）：
$$F = \frac{ESS/k}{RSS/(n-k-1)} \sim F(k, n-k-1)$$

**与 $R^2$ 的关系**：
$$F = \frac{R^2/k}{(1-R^2)/(n-k-1)}$$
- 这里可以反推出对$R^2$的要求，会发现其实显著本身对$R^2$的要求较小
### 三、变量显著性检验(t检验)

**假设**：
- $H_0: \beta_j = 0$
- $H_1: \beta_j \neq 0$

**t统计量**：
$$t_j = \frac{\hat{\beta}_j}{se(\hat{\beta}_j)} \sim t(n-k-1)$$

### 四、置信区间

**系数置信区间**：
$$\hat{\beta}_j \pm t_{\alpha/2}(n-k-1) \cdot se(\hat{\beta}_j)$$

## §3.4 预测【记忆】

### 一、均值预测

**点预测**：$\hat{Y}_0 = \mathbf{X}_0\hat{\boldsymbol{\beta}}$

**区间预测**：
$$\hat{Y}_0 \pm t_{\alpha/2} \cdot \hat{\sigma} \sqrt{\mathbf{X}_0(\mathbf{X}'\mathbf{X})^{-1}\mathbf{X}_0'}$$

### 二、个值预测

**预测误差方差**：
$$Var(e_0) = \sigma^2[1 + \mathbf{X}_0(\mathbf{X}'\mathbf{X})^{-1}\mathbf{X}_0']$$

**预测区间**：
$$\hat{Y}_0 \pm t_{\alpha/2} \cdot \hat{\sigma} \sqrt{1 + \mathbf{X}_0(\mathbf{X}'\mathbf{X})^{-1}\mathbf{X}_0'}$$

## §3.5 模型的其他函数形式

### 一、常见函数形式变换

#### 1. **倒数模型**：$Y = \beta_0 + \beta_1\frac{1}{X} + \mu$
#### 2. **多项式模型**：$Y = \beta_0 + \beta_1 X + \beta_2 X^2 + \mu$
#### 3. **对数模型**：
   - 双对数：$\ln Y = \beta_0 + \beta_1 \ln X + \mu$
	   - 系数$\beta_1$解释为 Y 对 X 的弹性。当 X 提高1%时，Y 在保持其他变量不变时平均上升 $\beta_{1}$ %
   - 半对数：$Y = \beta_0 + \beta_1 \ln X + \mu$ 或 $\ln Y = \beta_0 + \beta_1 X + \mu$
	   - 系数$\beta_{1}$解释为 X 增加一个单位时，price会提升约100$\beta_{1}$%. 计算精确的百分比变化$\%\Delta\hat{price}=100·[exp(\hat\beta_2·\Delta rooms)-1]$ 
### 二、可化为线性的非线性回归实例
**形式**：$Y = AK^\alpha L^\beta$ 
**对数形式**：$\ln Y = \ln A + \beta_{1} \ln K + \beta_{2} \ln L$ 
**约束**：$\beta_{1}+\beta_{2}=1$
可以化为：$\ln\left( \frac{Y}{L} \right)=\beta_{0}+\beta_{1}\ln\left( \frac{K}{L} \right)+\mu$ 

## §3.6 受约束回归
### 一、模型参数的线性回归
#### 1. F检验 / Wald检验
- 基本假设：如果约束条件为真，则受约束回归模型与无约束回归模型具有相同的解释能力，$RSS_{R}$与$RSS_{U}$的差异较小，$H_0: RSS_{R}=RSS_{U}$ 
- **检验统计量**：$F = \frac{(RSS_R - RSS_U)/(k_{U}-k_{R})}{RSS_U/(n-k_{U}-1)} \sim F(k_{U}-k_{R}, n-k_{U}-1)$ 
- 其中：
	- $RSS_R$: 受约束回归残差平方和
	- $RSS_U$: 无约束回归残差平方和
	- $k_{R},k_{U}$: 受约束回归和无约束回归的自变量个数
- 代码
```stata
test lnk+lnl =1 // 这个是在检验是不是规模报酬不变
** 包含线性约束的估计
constraint define 1 lnk+lnl=1
cosreg lny lnk lnl, constrains(1)
```
### 二、对回归模型增加或减少解释变量
- 考虑两个模型
	$Y=\beta_{0}+\beta_{1}X_{1}+\dots+\beta_{k}X_{k}+\mu$ 
	$Y=\beta_{0}+\beta_{1}X_{1}+\dots+\beta_{k}X_{k}+\beta_{k+1}X_{k+1}+\dots+\beta_{k+q}X_{k+q}+\mu$ 
- 零假设 $H_{0}:\beta_{k+1}=\dots=\beta_{k+q}=0$ 
- F 统计量：$F= \frac{(RSS_{R}-RSS_{U})/q}{RSS_{U}/(n-k-q-1)}=\frac{(R_{R}^2-R_{U}^2)/q}{1-R_{U}^2/(n-k-q-1)}$ 
- 代码
```stata
test p1 p2 p3 // 联合检验
```
### 三、检验不同组之间回归函数的差异

$$F = \frac{[RSS_R - (RSS_1 + RSS_2)]/k}{(RSS_1 + RSS_2)/(n_1+n_2-2k-2)}$$
### 四、一般情况下的约束条件（线性假设）
$$F=\frac{(\mathbf{Rb-r})'[\mathbf{R(X'X)R'}]^{-1}(\mathbf{Rb-r})/m}{s^2}\sim F(m,n-k)$$

$$H_0:RB=r$$
e.g.$$\begin{bmatrix}0&1&1&0\\0&0&1&-1\end{bmatrix}\begin{bmatrix}\beta_1\\\beta_2\\\beta_3\\\beta_4\end{bmatrix}=\begin{bmatrix}1\\0\end{bmatrix}$$

## §3.7 虚拟变量模型
### 3.7.1.虚拟变量的设置
#### 1. 单一维度的虚拟变量
如果我们考察性别的影响
$$Y_i=\beta_0+\beta_1X_i+\beta_2D_i+\mu_i$$
女职工：$E(Y_i|X_i,D_i=0)=\beta_0+\beta_1X_i$
男职工：$E(Y_i|X_i,D_i=1)=\beta_0+\beta_2+\beta_1X_i$

如果我们考察学历的影响(分成三层)，可以引入两个虚拟变量`D1`,`D2`
$$D_1=\begin{cases}1&高中\\0&高中以下\end{cases}，D_2=\begin{cases}1&大学\\0&高中\end{cases}$$
$$Y_i=\beta_0+\beta_1X_i+\beta_2D_{1i}+\beta_2D_{2i}+\mu_i$$
这里，高中以下作为对照组
#### 2. 多维度的虚拟变量
同时考虑性别和学历
$$D_1=\begin{cases}1&本科\\0&本科以下\end{cases}，D_2=\begin{cases}1&男\\0&女\end{cases}$$
这里，本科以下的女性是对照组
#### 3. 注意事项
虚拟变量个数要比该定性变量的状态类别数少一
### 3.7.2.虚拟变量的引入
#### 1. 用加法引入虚拟变量：改变截距项
#### 2. 用乘法引入虚拟变量：改变斜率
$$Y_i=\beta_0+\beta_1X_i+\beta_2D_i+\beta_3D_iX_i+\mu_i$$
#### 3. 用加法和乘法引入虚拟变量

|                    | $\alpha_1=\beta_1$ | $\neq$ |
| ------------------ | ------------------ | ------ |
| $\alpha_2=\beta_2$ | 重合回归               | 平行回归   |
| $\neq$             | 汇合回归               | 相异回归   |

```stata
reg y dum x1 dum_x1 x2 dum_x2 x3 dum_x3
test (dum=0)(dum_x1=0)(dum_x2=0)(dum_x3=0)
chowreg y x1 x2 x3, dum(31) type(3) // 两个样本的邹氏检验
```

# 第四章 放宽条件
> 概念与类型，背景，后果，检验，修正与改进

## 4.1. 多重共线性
---

### 4.1.1. 概念与类型
- **多重共线性**：两个或多个解释变量间出现了相关性
- **完全线性**：$\sum_{j=1}^kc_jX_{ij}=0,i=1,\cdots,n$
- **近似共线性 / 交互相关**：$\sum_{j=1}^kc_jX_{ij}+\upsilon_i=0,i=1,\cdots,n,\, \upsilon_{i}$是随机误差项

### 4.1.2. 背景
- 经济变量同趋势，模型设定不谨慎，样本资料的限制

### 4.1.3. 后果
- 完全线性：参数估计量不存在
- **近似共线性**：参数估计量不有效。 $|X'X|\approx 0$，使$Cov(\hat{\boldsymbol{\beta}}) = \sigma^2(\mathbf{X}'\mathbf{X})^{-1}$变得很大，t检验变量显著性检验、F检验模型预测功能、预测区间、置信区间失去价值。
	-  $1/(1-r^2)$ 是二元方差膨胀因子，$r$是$X_{1}X_2$的二元情况下的线性相关系数（VIF，我们一般经验地认为VIF>10时，我们认为两个变量有较强的近似共线性）
- 参数估计量经济含义不合理
- 变量的显著性检验失去意义

### 4.1.4. 检验
#### 1. 检验多重共线性是否存在
1. 对两个解释变量的模型，采用简单相关系数法，直接比较 r
2. 对多个解释变量的模型，采用综合统计检验法，$R^{2},F$ 值较大，但是 t 检验值较小
#### 2. 判断存在多重共线性的范围
- 判别系数检验法：对$X_{ji}=\alpha_1X_{1i}+\cdots+\alpha_kX_{ki}$进行 $F= \frac{R^{2}/k}{(1-R^2)/(n-k-1)}$ 检验，此处$1/(1-R_j^2)$是多元方差膨胀因子（VIF>10则认为有较强的近似线性性）
- 逐步回归法：逐个引入解释变量
	- 如果拟合优度变化明显，则新引入的变量是一个独立解释变量
	- 如果拟合优度变化很不明显，则可能存在多重共线性
- 相关性
- 多元综合检验法
- 方差膨胀因子检验

### 4.1.5. 修正与改进
#### 1. 排除引起共线性的变量 - 逐步回归法
#### 2. 减少参数估计量的方差 - 岭回归 
- 趋势：取对数，差分法

```stata
reg lny lnx1-lnx6 // 用OLS估计上述模型
pwcorr lny lnx1-lnx6, sig star(.05) // 检验简单相关系数
estat vif // 方差膨胀检验VIF, 估计后统计量
collin lnx1-lnx6 // 多重共线性检验
stepwise, pr(0.1): reg lny lnx1-lnx6 // 逐步回归法，逐步剔除变量
stepwise, pe(0.1): reg lny lnx1-lnx6 // 逐步回归法，逐步增加变量
```

## 4.2. 异方差性
### 4.2.1. 概念与类型
- 概念：对于不同的样本点，随机误差项的方差不再是常数
- 类型：$\sigma_i^2=f(X_i)$，单调递增 / 单调递减 / 复杂型

### 4.2.2. 背景
> 一般来说，截面数据，存在异方差问题的可能性比较高；时间序列数据，存在序列相关的可能性比较高
> 例：高收入家庭，储蓄差异较大。数据钟形分布带来较大的误差，进而产生方差异质性。

### 4.2.3. 后果
1. 参数估计量非有效
	- $Var(\beta)=\sum_ix_i^2E(\mu_i^2)/(\sum_i x_i^2)^2$
2. 变量显著性检验失去了意义：不满足最小方差性，导致t、f显著性检验、置信区间失效
3. 模型的预测失效

### 4.2.4. 检验
- $Var(\mu_i)=E(\mu_i^2)\approx\tilde{e}_i^2\quad \tilde{e}_i=y_i-(\hat{y}_i)_{OLS}$ (大数定理，方差趋同性)
1. 图示法：画 $X-\tilde{e}_i^2$ 的散点图（但是不能确定）
	- 同方差，递增异方差，递减异方差，复杂型异方差
2. Breusch-Pagan 检验
	- $Y_i=\beta_0+\beta_1X_{i1}+\cdots+\beta_kX_{ij}+\mu_i$
	- $\mu_i^2=\delta_0+\delta_1X_{i1}+\delta_kX_{ik}+\epsilon_i$ 辅助回归模型
	- $H_0:\delta_0=\cdots=\delta_k=0$ 
	- F 检验系数 or LM 统计量：$F= \frac{R^2_{e^2}/k}{(1-R^{2}_{e^{2}})/(n-k-1)}$  $LM=n · R_{e^2}^2 \sim \chi^2 (k)$ 在大样本下有效（卡方分布均值为自由度，方差为两倍自由度，一均方收敛可以推出一概收敛）
		- normal: $e_i^2=\alpha_0+\alpha_1 \ln\hat{y}_i+\epsilon$ 
3. White 检验（更精细，有严格证明）
	- $Y_i=\beta_0+\beta_1X_{i1}+\beta_2X_{i2}+\mu_i$
	- $\mu_i^2=\delta_0+\delta_1X_{i1}+\delta_2X_{i2}+\delta_3X_{i1}^2+\delta_4X_{i2}^2+\delta_5 X_{i1}X_{i2}+\epsilon_i$ 交互项一定是*两次*的
	- $H_0:\delta_0=\cdots=\delta_k=0$  F 检验
	- 同方差假设：$nR^2\dot\sim\chi^2(h)$ 渐进服从卡方分布

### 4.2.5. 修正与改进
-  加权使得方差一致
	- 假设：$Var(\mu_i|X_{i1},\cdots,X_{ik})=\mu_i^2=\sigma^2 · \text{exp}(\delta_0+\alpha X_{i1}+\cdots+\alpha_k X_{ik})$
	- 估计模型：$ln(\tilde{e}_i^2)=\delta_0+\alpha_1 X_{i1}+\cdots+\alpha_k X_{ik}+\upsilon_i$ 
	- 估计的权：$\hat{w}_i=1/\hat{\sigma}_i=1/\sqrt{\hat{f}_i}=1/\sqrt{\text{exp}(\hat{\alpha}_0+\hat{\alpha_1} X_{i1}+\cdots+\hat{\alpha}_k X_{ik})}$ 
- robust 回归 / 异方差稳健标准误法
	- 这个仅仅是把方差进行了调整

```stata
reg lny lnx1 lnx2
estimate store ols // 把回归结果存在ols中
predict e,residual
gen e2 = e^2
twoway (scatter e2 lnx2) // 图示法
qui reg e2 lnx1 lnx2
estat hettest,rhs // BP检验
estat hettest,normal // normal BP检验
estat imtest, white // white检验
gen lne2=log(e2)
gen lnx2_2=lnx2^2
qui reg lne2 lnx2 lnx2_2
predict p_lne2
gen w = 1/exp(p_lne2)
reg lny lnx1 lnx2 [aweight=w] // 这里会自行开方
estimate store wls
reg lny lnx1 lnx2, robust
estimate store ols_robust
esttab ols wls ols_robust, scalar(r2 r2_a N F) mtitle(ols wls ols_robust) // 输出这几个回归的这几个值，改子标题
```

## 4.3. 内生解释变量问题

### 4.3.1.内生解释变量问题
- 分类
	- 内生随机解释变量与随机干扰项同期无关异期相关（在大样本情况下，估计量满足一致性）
	- 内生随机解释变量与随机干扰项同期相关
- 注：一定是针对大样本的这样才能满足一致性

### 4.3.2.实际经济问题中的内生解释变量问题
- 情形 `互为因果，遗漏变量，测量误差，选择偏误`
	- 被解释变量与解释变量具有**联立因果**关系
	- 模型设定时遗漏了重要的解释变量，而且所**遗漏的变量**与模型中的一个或多个解释变量具有同期相关性（并非蒸发而是被挪到随机扰动项中）
	- 解释变量存在**测量误差**
	- 样本选择偏误

> 联立因果关系：风投投资资金与初创公司创新
> 遗漏解释变量：工资与教育程度、工作经验（个人能力无法度量）

### 4.3.3.内生解释变量的后果
-  内生随机解释变量与随机干扰项同期无关异期相关
	- 大样本情况下：**无偏**
-  内生随机解释变量与随机干扰项同期相关
	- 大样本情况下：有偏

### 4.3.4.工作变量法
#### 1. 工具变量的选取
- **相关**：与所替代的内生解释变量高度相关
- **外生**：与随机干扰项不相关
- **无严重多重共线性**：与模型中其他解释变量不高度相关
#### 2. 工具变量的应用
- 离差形式：$y_{i}=\beta_{1}x_{i}+\mu_{i}\to \sum x_{i}y_{i}=\beta \sum x_{i}^{2}+\sum x_{i}\mu_{i}$
- 内生：$E(X_{i}\mu_{i})=0$ 不满足
- 工具变量：$\sum z_{i}y_{i}=\beta_{1} \sum z_{1}x_{i}+\sum z_{i}\mu_{i},\;\tilde{\beta}_{1}=\frac{\sum z_{i}y_{i}}{\sum z_{i}x_{i}}$
- 高维工具变量：$\mathbf{Z'Y}=\mathbf{Z'X\beta},\tilde{\beta}=(\mathbf{Z'X})^{-1}\mathbf{Z'Y}$
	- $\mathbf{Z'}=\begin{bmatrix}1 & 1 & \dots & 1 \\ X_{11}  & X_{21} & \dots & X_{n1} \\ Z_{1} & Z_{2} & \dots &  Z_{n} \\ \dots & \dots & \dots & \dots \\ X_{1k} & X_{2k} & \dots  & X_{nk}\end{bmatrix}$
	- 恰好识别：内生解释变量和工具变量数量相同
	- 无法识别：内生解释变量少于工具变量数量
	- 过度识别：内生解释变量多于工具变量数量
3. 工具变量法估计量是一致估计量
	- $\tilde{\beta_{1}}=\frac{\sum z_{i}y_{i}}{\sum z_{i}x_{i}}=\frac{\sum z_{i}Y_{i}}{\sum z_{i}x_{i}}=\frac{\sum z_{i}(\beta_{0}+\beta_{1}X_{i}+\mu_{i})}{\sum z_{i}x_{i}}=\beta_{1}+\frac{\sum z_{i}\mu_{i}}{\sum z_{i}x_{i}}$ 
	- 大样本下$P\lim\tilde{\beta_{1}}=\beta_{1}$，这是一个一致估计量，小样本的情况下还是有偏的
		- 要求：$P\lim \frac{1}{n}\sum z_{i}\mu_{i}=0,\,P\lim \frac{1}{n}\sum z_{i}x_{i} \neq 0$
			- 应该是个强工具变量
	- 小样本下，工具变量法估计量是有偏的
4. 三种估计方法：IV，2SLS，GMM
	- IV：一个内生解释变量只有唯一工具变量（恰好识别）
		- $\hat{X_{i}}=\hat{\alpha_{0}}+\hat{\alpha_{1}}Z_{i}$
		- $\hat{Y}_{i}=\tilde{\beta}_{0}+\tilde{\beta}_{1}\hat{X}_{i},\, \tilde{\beta}_{1}=\frac{\sum z_{i}y_{i}}{\sum z_{i}x_{i}}$ 
	- 2SLS：一个内生解释变量有多个工具变量（与Hausman进行区分！）
		- 本质：把内生解释变量的外生因素去做回归
		- $\hat{X_{i}}=\hat{\alpha_{0}}+\hat{\alpha_{1}}Z_{i1}+\hat{\alpha_{2}}Z_{i2}+\hat{\alpha_{3}}Z_{i}$ ，注意$Z_{i}$也要放进去哦
		- $\hat{Y}_{i}=\tilde{\beta}_{0}+\tilde{\beta}_{1}\hat{X}_{i}+\tilde{\beta}_{2}{Z}_{i}$ 一致估计量
			- $Y_{i}=\beta_{0}+\beta_{1}X_{i}+\beta_{2}Z_{i}+\mu_{i}$ 二者的标准差不同
	- GMM：当随机扰动项存在异方差（即随机扰动项不满足球型假设的的时候）

```stata
reg lnq lny lnp
est store ols
ivregress 2sls lnq lny (lnp=tax)
est store iv // 工具变量：lny，tax
ivregress 2sls lnq lny (lnp=tax taxs)
est store 2sls // 工具变量：lny，tax，taxs
ivregress gmm lnq lny (lnp=tax taxs)
est store gmm // 工具变量：lny，tax，taxs，如果还存在异方差的话
ivregress 2sls y x1 x2 (x3 x4 = z1 z2 w1-w3) //内生：x3,x4.工具：z1,z2,w1-w3,x1,x2
esttab ols iv 2sls gmm, scalar(r2 r2_a N) mtitle(ols iv 2sls gmm)
/*工具变量要在大样本的情况下才会成立，同时在大样本情况下，z统计量和t统计量是一致的*/
ivregress gmm lnq lny (lnp=tax taxs), small // 给出t统计量
ivregress gmm lnq lny (lnp=tax taxs), first //给出第一阶段回归结果

// Hausman-Wu 内生性检验
qui reg lnp lny tax taxs
predict e, residual // 提取残差
reg lnq lny lnp e
ereturn list // 列出回归的信息
mat Var_b=e(V) // 提取估计参数的协方差矩阵
mat list Var_b // 列出协方差矩阵
scalar se_e=sqrt(Var_b[3,3]) // 单值变量定义，提取e的估计参数标准误
scalar t_e=_b[e]/se_e // e的估计参数的t统计量
scaler p_t_e=ttail(e(df_r),abs(t_e))*2 // 计算对应的概率
dis "t_e="%6.4f t_e
dis "P值="%4.3f p_t_e

// 这里进行的是过度识别约束检验
qui ivregress 2sls lnq lny (lnp = tax taxs)
predict u, residual // 2sls 回归残差
reg u tax taxs lny 
ereturn list
scalar J=e(N)*e(r2)
scalar p_chi2=chi2tail(1,J)
dis "J="%6.4f J // Sargan-J统计量
dis "P值="%4.2f p_chi2
estat overid

// Durbin-Wu-Hausman 内生性检验
qui reg lnq lny lnp
qui ivregress 2sls lnq lny (lnp = tax taxs)
hausman tsls ols // Hausman检验前提模型同方差，但这条命令其实是做了2sls和ols，然后比较系数的差是否显著
qui ivregress 2sls lnq lny (lnp = tax taxs)
estat endogenous // Durbin-Wu-Hausman 检验
qui ivregress 2sls lnq lny (lnp = tax taxs), r
estat endogenous // Durbin-Wu-Hausman 检验
ivreg2 lnq lny (lnp = tax taxs), endog(lnp) // 检验估计一体化：识别不足，弱IV检验，过度识别，内生性
ivreg2 lnq lny (lnp = tax taxs),r endog(lnp)
```
### 4.3.5.内生性检验与过度识别约束检验

1. Hausman 内生性检验
	- $Y_{i}=\beta_{0}+\beta_{1}X_{i}+\beta_{2}Z_{i1}+\mu_{i}$ Z1外生，X内生
	- $X_{i}=\alpha_{0}+\alpha_{1}Z_{i1}+\alpha_{2}Z_{i2}+\nu_{i}$  Z2是工具变量
	- $Y_{i}=\beta_{0}+\beta_{1}X_{i}+\beta_{2}Z_{i1}+\delta \hat{\nu}_{i}+\varepsilon_{i}$ 
		- 若 $\delta$ 为零，则表示随机扰动项 $\nu$ 与 Y 同期无关，进而与 $\nu$ 与原模型 $\mu$ 同期无关，则 X 与 $\mu$ 同期无关
		- 如果原模型有**多个解释变量被怀疑是内生变量，则需寻找多个外生变量**，并**将每个怀疑的解释变量与所有外生变量(含原模型外生变量)作OLS**，取得残差项，将它们全部引入原模型再做0LS估计，通过t检验或多种情形的受约束F检验，判断哪些解释变量确实是内生变量。
2. 过度识别约束检验
	- $Y_{i}=\beta_{0}+\beta_{1}X_{i}+\beta_{2}Z_{i}+\mu_{i}$
	- $\tilde{\mu}_{i}=Y_{i}-(\tilde{\beta}_{0}+\tilde{\beta}_{1}X_{i}+\tilde{\beta}_{2}Z_{i})$
		- 注意：$\tilde{\beta}_{0},\tilde{\beta}_{1},\tilde{\beta}_{2}$ 是通过 2sls 算出来的
	- $\tilde{\mu}_{i}=\delta_{0}+\delta_{1}Z_{i1}+\delta_{2}Z_{i2}+\delta_{3}Z_{i}+\varepsilon_{i}$
		- $\mathbf{J}=nR^2\sim \chi^{2}(1)$ 
			- 注意：卡方分布的自由度为额外工具变量的个数
		- 当J统计量大于给想显著水平下的临界值时拒绝Z1Z2同时为外生解释变量的零假设
notes.
工具变量一定不是一个显著影响模型的变量
滞后一期，区域的加总，制度性的，地理位置（河流）

## 4.4. 模型设定偏误问题

### 一、模型设定偏误的类型

#### 1. 相关变量的遗漏（Omitting Relevant Variables）
- **正确模型**：
  $$Y = \beta_0 + \beta_1 X_1 + \beta_2 X_2 + \mu$$
- **设定模型**：
  $$Y = \alpha_0 + \alpha_1 X_1 + \nu$$
- 漏掉相关解释变量 $X_2$。

#### 2. 无关变量的误选（Including Irrelevant Variables）
- **正确模型**：
  $$Y = \beta_0 + \beta_1 X_1 + \beta_2 X_2 + \mu$$
- **设定模型**：
  $$Y = \alpha_0 + \alpha_1 X_1 + \alpha_2 X_2 + \alpha_3 X_3 + \mu$$
- 多选了无关变量 $X_3$。

#### 3. 错误的函数形式（Wrong Functional Form）
- **正确模型**（如Cobb-Douglas生产函数）：
  $$Y = A X_1^{\beta_1} X_2^{\beta_2} e^\mu$$
- **设定模型**（线性形式）：
  $$Y = \beta_0 + \beta_1 X_1 + \beta_2 X_2 + \nu$$

### 二、模型设定偏误的后果

#### 1. 遗漏相关变量偏误
- **估计量性质**：
  - 若遗漏变量 $X_2$ 与 $X_1$ 相关，则 $\alpha_1$ 的估计量 **有偏且不一致**。
  - 若 $X_2$ 与 $X_1$ 不相关，则 $\alpha_1$ 的估计量 **无偏且一致**，但 $\alpha_0$ 的估计量有偏。随机扰动项方差估计 $\hat{\sigma}^2$ 有偏。$\hat{\alpha}_1$ 的方差估计有偏。
- **公式推导**：
  $$\hat{\alpha}_1 = \frac{\sum x_{i1} y_i}{\sum x_{i1}^2} = \beta_1 + \beta_2 \frac{\sum x_{i1} x_{i2}}{\sum x_{i1}^2} + \frac{\sum x_{i1} (\mu_i - \bar{\mu})}{\sum x_{i1}^2}$$
  $$E(\hat{\alpha}_1) = \beta_1 + \beta_2 \frac{\sum x_{i1} x_{i2}}{\sum x_{i1}^2}$$
- **方差比较**：
  $$Var(\hat{\alpha}_1) = \frac{\sigma^2}{\sum x_{i1}^2}$$
  $$Var(\hat{\beta}_1) = \frac{\sigma^2}{\sum x_{i1}^2 (1 - r_{x_1 x_2}^2)}$$

#### 2. 包含无关变量偏误
- **估计量性质**：
	- 参数估计量 **无偏**，但 **不具有最小方差性**。
	- **方差增大**，估计精度下降。
- **方差比较**：
  $$Var(\hat{\alpha}_1) = \frac{\sigma^2}{\sum x_{i1}^2}$$
  $$Var(\hat{\beta}_1) = \frac{\sigma^2}{\sum x_{i1}^2 (1 - r_{x_1 x_2}^2)} \geq Var(\hat{\alpha}_1)$$

#### 3. 错误函数形式偏误
- 导致 **全方位的偏误**：参数估计、经济解释、预测均受影响。

### 三、模型设定偏误的检验

#### 1. 检验是否含有无关变量
- **基本思想**：无关变量的真系数应为零。
- **方法**：
	- **t检验**：检验单个变量系数是否显著。
	- **F检验**：检验多个变量系数是否联合显著。$H_0: \beta_2 = \beta_3 = \cdots = 0$ 
#### 2. 检验遗漏变量或函数形式偏误
##### （1）残差图示法
- 对设定模型OLS回归，得到残差序列 $\hat{e}_t$。
- 绘制 $\hat{e}_t$ 与时间 $t$ 或解释变量 $X$ 的散点图。
- **判断**：
	- **趋势变化**：可能遗漏随时间趋势上升的变量。
	- **循环变化**：可能遗漏呈循环变化的变量。
	- **正负交替**：可能函数形式设定错误。

##### （2）RESET检验（Regression Error Specification Test）
- **基本思想**：用被解释变量拟合值 $\hat{Y}$ 的高次幂作为“替代变量”，检验其是否应加入模型。
- **步骤**：
  1. 估计原模型，得到 $\hat{Y}$。
  2. 将 $\hat{Y}^2, \hat{Y}^3$ 等加入模型重新估计。
  3. 检验这些高次项系数是否联合显著（F检验或t检验）。
- **判断**：若显著，则存在设定偏误；若不显著，则模型设定可能正确。

##### （3）Stata操作示例
- **RESET检验命令**：
```stata
qui reg lnq lny lnp
predict lnq_hat
gen lnq_hat2=lnq_hat^2
reg lnq lny lnp lnq_hat2
qui reg lnq lny lnp
linktest
estat ovtest  // Ramsey RESET检验，默认加入拟合值平方、立方、四次方项
  ```
- **Linktest原理**：
  - 如果模型设定正确，拟合值 $\hat{Y}$ 的平方项不应有解释能力。
  - 命令：
``` stata
regress Y X1 X2
linktest  // 自动生成_hat（拟合值）和_hatsq（拟合值平方），检验_hatsq的显著性
```
- 如果有显著性，就要加入二次的变量再做二次的检验


#### 3. 实例：美国香烟消费模型
- **原模型（双对数）**：
  $$\ln Q = \beta_0 + \beta_1 \ln Y + \beta_2 \ln P + \mu$$
  - RESET检验：$F=1.132 < F_{0.05}(1,43)=4.06$，不拒绝原假设，**无设定偏误**。
- **线性模型**：
  $$Q = \beta_0 + \beta_1 Y + \beta_2 P + \mu$$
  - RESET检验：$F=7.6865 > 4.06$，拒绝原假设，**存在设定偏误**。
- **加入 $P^2$ 的新模型**：
  $$Q = \beta_0 + \beta_1 Y + \beta_2 P + \beta_3 P^2 + \mu$$
  - RESET检验：$F=1.132 < 4.06$，不拒绝原假设，**无设定偏误**

# 第五章 時間序列計量經濟學模型
## 5.1 時間序列模型的序列相關性 
### 一、序列相關性概念 
#### 1. 模型及基本假設 
- 模型表達式 (OLS)：
    $$Y_{t}=\beta_{0}+\beta_{1}X_{t1}+\beta_{2}X_{t2}+...+\beta_{k}X_{tk}+\mu_{t}, \quad t=1,2,...,T$$
- 基本假設（無序列相關）：隨機項互不相關。
    $$Cov(\mu_{i},\mu_{j})=0 \quad i \ne j, \quad i,j=1,2,...,T$$
#### 2. 序列相關的定義

- 如果對於不同的樣本點，隨機誤差項 ($\mu_t$) 之間存在某種相關性，則認為出現了序列相關性 。
    - 以**截面數據**為樣本：稱為空間自相關（Spatial Autocorrelation） 。
    - 以**時序數據**為樣本：稱為序列自相關（Serial Autocorrelation） 。
    - 習慣上統稱為序列相關性 。
- 存在序列相關時隨機擾動項的特性：
    $$Cov(\mu_{i},\mu_{j})=E(\mu_{i}\mu_{j})\ne0$$
$$Var(\mu)=E(\mu\mu^{\prime})=\begin{pmatrix}\sigma^{2}&...&E(\mu_{1}\mu_{\tau})\\ \vdots&...&\vdots\\ E(\mu_{\tau}\mu_{1})&...&\sigma^{2}\end{pmatrix}=\sigma^{2}\Omega\ne\sigma^{2}I$$
#### 3. 一階序列相關 (Autocorrelation)
- 僅存在 $E(\mu_{t}\mu_{t+1})\ne0$ 
- 自相關常寫成一階自迴歸形式 AR(1)：$\mu_{t}=\rho\mu_{t-1}+\epsilon_{t}, \quad -1<\rho<1$ 
    - $\rho$ 稱為自協方差係數或一階自相關係數
    - $\epsilon_{i}$ 是滿足 OLS 假定的隨機干擾項（白噪聲）：$E(\epsilon_{i})=0, \quad Var(\epsilon_{i})=\sigma^{2}, \quad Cov(\epsilon_{i},\epsilon_{i-s})=0 \quad s\ne0$ 
### 二、實際經濟問題中的序列相關性原因
#### 1. **經濟變量固有的慣性**：大多數經濟時間數據具有慣性，表現為時間序列前後的關聯性
- _例如_：居民總消費函數模型 $C_{t}=\beta_{0}+\beta_{1}Y_{t}+\mu_{t}$ 中，消費習慣的影響可能導致 $\mu_t$ 出現序列相關（通常是正相關） 
#### 2. **模型設定的偏誤 ：所設定的模型“不正確” 
- **丟掉重要解釋變量**：本應包含 $X_{t3}$，但實際模型中 $v_{t}=\beta_{3}X_{t3}+\mu_{t}$，若 $X_{t3}$ 影響 $Y$，則 $v_t$ 存在序列相關
- **模型函數形式有偏誤**：本應是二次函數 $Y_{t}=\beta_{0}+\beta_{1}X_{t}+\beta_{2}{X_{t}}^{2}+\mu_{t}$，卻設為線性 $Y_{t}=\beta_{0}+\beta_{1}X_{t}+v_{t}$，則 $v_{t}=\beta_{2}X_{t}^{2}+\mu_{t}$ 包含系統性影響，呈現序列相關性
#### 3. **數據的“編造”**
- 通過已知數據生成的數據（如月度數據來自季度數據的簡單平均、時間點之間的“內插”技術），會產生內在聯繫，導致序列相關性
### 三、序列相關性的後果
1. **參數估計量非有效**：OLS 估計量雖然在大樣本下具有一致性，但由於 $E(\mu\mu^{\prime})\ne\sigma^{2}I$，估計量失去有效性，不具有漸近有效性
2. **變量的顯著性檢驗失去意義**
3. **模型的預測失效**
### 四、序列相關性的檢驗 
**基本思路**：通過分析 OLS 殘差 $\tilde{e}_t=Y_{t}-(\hat{Y}_{t})_{OLS}$  之間的相關性，來判斷 $\mu_t$ 是否具有序列相關性
1. 圖示法
	- 繪製殘差 $e_t$ 與時間序列 $t$ 的散點圖，或 $e_t$ 與 $e_{t-1}$ 的散點圖，觀察是否存在系統性模式
```stata
gen t=_n
tsset t
reg lcon lin
predict e,res
twoway (scatter e t)(line e t), yline(0,lp(dash) lc(0.5*blue)) legend(off) xtitle(时间序列) ysize(3) xsize(4) 
```

2.  回归检验法：檢驗殘差 $e_t$ 是否與其滯後項 $e_{t-1}, e_{t-2}, \dots$ 或其他變量存在顯著的函數形式關係
	- **優點**：能夠確定序列相關的形式，適用於任何類型序列相關性問題的檢驗
3. 杜賓-瓦森（Durbin-Watson, D-W）檢驗法
	- **假定條件**：（条件比较苛刻，一般仅作参考）
	    1. 解釋變量 $X$ 非隨機（严格外生）
	    2. 隨機誤差項 $\mu_{t}$ 為一階自迴歸形式：$\mu_{t}=\rho\mu_{t-1}+\epsilon_{t}$ 
	    3. 迴歸模型中**不應含有滯後應變量**作為解釋變量
	    4. 迴歸含有截距項 
	- **原假設**：$H_{0}: \rho=0$ （即不存在一階自迴歸）
	- D.W. 統計量：
	    $$D.W.=\frac{\sum_{t=2}^{T}(\tilde{e}_{t}-\tilde{e}_{t-1})^{2}}{\sum_{t=1}^{T}\tilde{e}_{t}^{2}}=\frac{\sum_{t=2}^{T}\tilde{e}_{t}^{2}+\sum_{t=2}^{T}\tilde{e}_{t-1}^{2}-2\sum_{t=2}^{T}\tilde{e}_{t}\tilde{e}_{t-1}}{\sum_{t=1}^{T}\tilde{e}_{t}^{2}}\approx 2(1-\rho)$$
	    - 其中 $e_{t}$ 為 OLS 殘差 
	    - D.W. 展開後近似於 $2(1-\hat{\rho})$，其中 $\hat{\rho}$ 為一階自迴歸參數估計 
	- **判斷標準**：根據樣本容量 $n$ 和解釋變量個數 $k$，查表得臨界值 $d_{L}$ 和 $d_{U}$ 
	    - 若 $0 < D.W. < d_{L}$：存在**正**自相關 
	    - 若 $d_{L} < D.W.< d_{U}$：不能確定 
	    - 若 $d_{U} < D.W.< 4-d_{U}$：**無**自相關 
	    - 若 $4-d_{U} < D.W.< 4-d_{L}$：不能確定 
	    - 若 $4-d_{L} < D.W.< 4$：存在**負**自相關 
4. 拉格朗日乘數（LM）檢驗法 (Breusch-Godfrey, BG 檢驗) 
	- **優點**：適合於**高階序列相關**以及模型中存在**滯後被解釋變量**的情形 
	- **懷疑 $p$ 階序列相關**：$\mu_{t}=\rho_{1}\mu_{t-1}+\rho_{2}\mu_{t-2}+...+\rho_{p}\mu_{t-p}+\epsilon_{t}$ 
	- **原假設**：$H_{0}: \rho_{1}=\rho_{2}=...=\rho_{p} =0$ 
	- BG 統計量：通過輔助迴歸$\tilde{e}_{t}=\beta_{0}+\beta_{1}X_{t1}+\dots+\beta_{k}X_{tk}+\rho_{1}\tilde{e}_{t-1}+\dots+\rho_{p}\tilde{e}_{t-p}+\varepsilon_t$，計算 $R^2$ 
	    $$LM \approx n \cdot R^{2} \sim \chi^{2}(p)$$
	    - $n$ 為樣本容量，$R^{2}$ 為輔助迴歸的可決係數
	    - 为了保证样本容量为 n ，令 $\tilde{e}_{0}=\tilde{e}_{-1}=\dots=\tilde{e}_{-p+1}=0$  (McKinnon)
	- **判斷**：將 $LM$ 值與臨界值 $\chi^{2}_{\alpha}(p)$ 比較，若 $LM$ 顯著大於臨界值，則拒絕 $H_{0}$ 
### 五、序列相關的補救 
#### 1. 廣義最小二乘法 (GLS) 
- 當存在序列相關和異方差時 $E(\mu\mu^{\prime})=\sigma^{2}\Omega$ 
- 方法：對原模型 $Y=X\beta+\mu$ 進行變換，可以找到可逆矩陣 $D$ 使 $\Omega=DD^{\prime}$，將原模型左乘 $D^{-1}$，$Y^{*}=X^{*}\beta + \mu^{*} \quad \text{其中 } Y^{*}=D^{-1}Y, X^{*}=D^{-1}X, \mu^{*}=D^{-1}\mu$ 
	- $E(\mu_*\mu_*')=\sigma^2\mathbf{I}$
- **結果**：對變換後的模型 $(*)$ 進行 OLS 估計，得到廣義最小二乘估計量 (GLS estimators) 。該估計量是**無偏的、有效的** 、唯一的。
	- $\hat{\beta}_{*}=(\mathbf{X}'\Omega^{-1} \mathbf{X})^{-1}\mathbf{X}'\Omega^{-1}\mathbf{Y}$
	- 在理论上证明了一般的模型（异方差、序列相关等）也是可以估计的
#### 2. 廣義差分法 
- **一階自相關 (AR(1)) 的廣義差分模型**：
	- 原模型：$Y_{t}=\beta_{0}+\beta_{1}X_{t}+\mu_{t}$；誤差項：$\mu_{t}=\rho\mu_{t-1}+\epsilon_{t}$ 
		- 差分模型： $Y_{t}-\rho Y_{t-1} = \beta_{0}(1-\rho) + \beta_{1}(X_{t}-\rho X_{t-1}) + \epsilon_{t}\Rightarrow \Delta Y_{t} = \beta_{0}' + \beta_{1}' \Delta X_{t} + \epsilon_{t}$
	- 對此模型進行 OLS 估計，$\epsilon_{t}$ 為白噪聲，故不存在序列相關問題 
- **普萊斯-溫斯特變換**：對第一個觀測值進行變換，以避免差分過程中的觀測值損失，$Y_{1}^*=\sqrt{ 1-\rho^2 }Y_{1},X_{1j}^*=\sqrt{ 1-\rho^{2} }X_{ij}$ （二阶就没有了）
#### 3. 隨機誤差項相關係數 $\rho_{j}$ 的估計
- GLS 或廣義差分法需要已知 $\rho_{j}$ 的具體數值，但實際需對其進行估計
- **科克倫-奧科特 (Cochrane-Orcutt) 迭代法**：
	1. OLS 估計原模型得到殘差 $\hat{\mu}_{i}$ ( $\mu_{i}$ 的近似估計值) 
	2. 對殘差進行自迴歸估計 $\hat{\mu}_{i}=\rho_{1}\hat{\mu}_{i-1}+\dots+\rho_{l}\hat{\mu}_{i-l}+\epsilon_{i}$，得到 $\hat{\rho}_{j}$ 
	3. 利用 $\hat{\rho}_{j}$ 進行廣義差分$Y_{t}-\hat{\rho}_{1}Y_{t-1}-\dots-\hat{\rho}_{l}Y_{t-l}=\dots$，得到新的殘差 $\hat{\mu}_{i}'$，重複步驟 2，直到 $\hat{\rho}_{j}$ 的估計值收斂 
	- **科克倫-奧科特兩步法**：只迭代兩次
- **可行的廣義最小二乘法 (FGLS, Feasible Generalized Least Squares)**：通過估計 $\Omega$ 或 $\rho_{j}$ 來實現 GLS。FGLS 估計量是**一致的**，且在科克倫-奧科特迭代法下具有**漸近有效性** 
#### 4. 序列相關穩健標準誤法
- **Newey-West 標準誤 (1987)**：修正 OLS 參數估計量的標準誤 
- **優點**：能得到存在序列相關或**異方差-序列相關同時存在**時的正確標準誤 
- **結果**：OLS 參數估計量本身雖非有效，但因方差估計正確，使得各種統計檢驗、預測區間更為可信 
### 六、虛假序列相關問題
- **原因**：通常是模型設定中**遺漏了重要的解釋變量**或對模型的**函數形式設定有誤**所致 
- **避免措施**：應建立一個“一般”模型，然後逐漸剔除確實不顯著的變量，以排除虛假序列相關 
### 七、案例：中國居民消費總量函數 (1978~2018) 
- **模型**：居民總消費 ($Y$) 由居民實際可支配收入 ($X$) 唯一決定 
####  1. 序列相關性檢驗 
- **D-W 檢驗**：
    - 結果 $DW=0.1970$
    - 結論：$DW < d_{L}=1.44$，故存在**正**自相關
- **拉格朗日乘數 (LM) 檢驗**：
    - **1 階滯後**：$LM(1)=33.36 > \chi^{2}_{0.05}(1)=3.84$，故存在**1 階序列相關性** 
    - **2 階滯後**：$LM(2)=32.91 > \chi^{2}_{0.05}(2)=5.99$，但 $\epsilon_{t-2}$ 未通過 5% 顯著性檢驗，故**不存在 2 階序列相關性** 
        

####  2. 模型估計 (補救) 
- **廣義差分法 (Cochrane-Orcutt)**：`prais y x, corc`
- **廣義差分法 (Prais-Winsten)**：`prais y x` 
- **序列相關穩健估計法 (Newey-West)**：`newey y x, lag(1)` 
```stata
tsset year // 设置时间序列
reg y x 
est store ols
predict e, residual
twoway (scatter e L1.e), legend(off) xtitle() ytitle ysize(3) xsize(4)
qui reg y x
estat dwatson // DW 检验 （外部命令 dwstat）
// 从结果来看 DW 落在 0 到 dl 之间，可能存在正相关
reg e x L1.e // rho1 显著
reg e x L1.e L2.e // rho2 显著
qui reg e x 
estat bgodfrey // BGLM 检验，lag(1)
estat bgodfrey,lag(2) // BGLM 检验，lag(2)
estat bgodfrey，lag(2) small // BGLM 检验，lag(2)，小样本调整
// 采用广义差分法估计模型，stata没有二阶滞后项可以操作
prais y x // 仅针对一阶序列线管
prais y x, corc // 剔除第一个样本
// 采用序列相关稳健估计法，这个可以滞后两期
newey y x, lag(1)
```

## 5.2.时间序列的平稳性及其检验
### 一、问题的提出
- 自变量的选取并不是随机的
### 二、时间序列数据的平稳性
【定义】宽平稳 
> $E(X_t)=\mu,Var(X_t)=\sigma^2,Cov(X_t,X_{t+k})=\gamma_k$ 

例子：白噪音是平稳的；随机游走是非平稳的，但是随机游走的一阶差分是平稳的。
注：非平稳序列在两次差分后就可以实现平稳；对$X_t=\phi X_{t-1}+\mu_{t}$，只有当$\phi \in(-1,1)$，该随机过程才是平稳的

### 三、平稳性检验的图示判断
【定义】自相关函数
> $\rho= \frac{\gamma_{k}}{\gamma_{0}}$

【定义】样本自相关函数
> $r_{k}= \frac{\sum_{t=1}^{n-k}(X_t-\bar{X})(X_{t+k}-\bar{X})}{\sum_{t=1}^n(X_{t}-\bar{X})^{2}}$
> > notes. 对于平稳序列会快速趋于零

【定理】Bartlett 定理
>如果时间序列由白噪声过程生成，则对所有的 k>0，样本自相关系数近似地服从于以 0 为均值，1/n 为方差的正态分布，其中 n 为样本数，即$r_k\sim N(0,1/n)$

【定理】Q统计量检验
> $H_{0}:\quad \rho_{1}=\dots=\rho_{k}=0 \implies$
> $Q_{LB}=n(n+2)\sum_{k=1}^m\left(  \frac{r_{k}^{2}}{n-k} \right) \sim \chi^{2}(m)$，其中 m 为滞后长度

```stata
tsset time
corrgram random1,lags(17)
ac random1,lags(17)
wntestb random1 // Bartlett 定理，滞后期大致等于超出范围的数量
wntestq random1,lags(17) //
```

### 四、平稳性的单位根检验
#### 1.DF检验
$X_t=\alpha+\rho X_{t-1}+\mu_{t}\implies\Delta X_{t}=\alpha+\delta X_{t-1}+\mu_{t}$
将检验是否存在单位根$\rho=1$ 转化为是否有$\delta=0$ 这里应该是单侧检验
然而在零假设的情况下，t 检验无法使用。这时我们把 t 统计量称作 $\tau$ 统计量，服从DF分布。
如果计算得到的 t 统计量的绝对值大于临界值的绝对值，则拒绝 $\rho=0$ 的假设，原序列不存在单位根，为平稳序列。
#### 2.ADF检验【重要】
$\begin{align}模型一 & \Delta X_{t}  =&\delta X_{t-1}+\sum_{i=1}^m\beta _i\Delta X_{t-i}+\mu_{t}&\\模型二 &\Delta X_{t} =&\alpha+\delta X_{t-1}+\sum_{i=1}^m\beta _i\Delta X_{t-i}+\mu_{t}&\\模型三  &\Delta X_{t} =&\alpha+\beta t+\delta X_{t-1}+\sum_{i=1}^m\beta _i\Delta X_{t-i}+\mu_{t}\end{align}$
该模型是在检验 **AR(m+1)** 模型。模型3 中的t是时间变量，代表了时间序列随时间变化的某种趋势（如果有的话）。模型1与另两模型的差别在于是否包含有常数项和趋势项。另外，三个模型都增加了 Xt 滞后项，保证随机项是白噪声，主要**消除序列相关性**。**实际检验时从模型3开始，然后模型2、模型1。**
何时检验拒绝零假设，即原序列不存在单位根，为平稳序列，何时检验停止。否则，就要继续检验，直到检验完模型1为止。检验原理与DF检验相同，只是对模型1、2、3进行检验时，有各自相应的临界值。
### 五、单整事件序列
一般地，如果一个时间序列经过d次差分后变成平稳序列，则称原序列是d 阶单整（integrated of d）序列，记为$I(d)$。 $I(0)$代表一平稳时间序列。
> 大多数指标的时间序列是非平稳的。常用经济变量多为I(1)序列，即只需经过一次差分即可变为平稳变量。例如，以不变价格表示的消费额、收入等常表现为1阶单整，以当年价表示的消费额、收入等常是2阶单整的。
> 但也有一些时间序列，无论经过多少次差分，都不能变为平稳的。这种序列被称为非单整的（non integrated）。

### 六、趋势平稳与差分平稳随机过程

### 六、案例
#### 1.对 X 水平序列的ADF检验
$\begin{align}\Delta X_{t}&=\delta X_{t-1}+\sum_{i=1}^m\beta _i\Delta X_{t-i}+\mu_{t}\\\Delta X_{t}&=\alpha+\delta X_{t-1}+\sum_{i=1}^m\beta _i\Delta X_{t-i}+\mu_{t}\\\Delta X_{t}&=\alpha+\beta t+\delta X_{t-1}+\sum_{i=1}^m\beta _i\Delta X_{t-i}+\mu_{t}\end{align}$

```stata
tsset year
** 1.对 Y 水平序列的ADF检验
dfuller y, regress trend lag(1) // 模型三：滞后一期，这里要关注滞后一期的t、ADF的临界值
dfuller y, regress lag(1) // 模型二：滞后一期，这里要关注滞后一期的t、ADF的临界值
dfuller y, regress nocon lag(1) // 模型二：滞后一期，这里要关注滞后一期的t、ADF的临界值
** 2.对 X 的1次差分序列的ADF检验
dfuller D.y, regress trend lag(1) // 模型三：滞后一期，这里要关注滞后一期的t、ADF的临界值
dfuller D.y, regress lag(1) // 模型二：滞后一期，这里要关注滞后一期的t、ADF的临界值
dfuller D.y, regress nocon lag(1) // 模型二：滞后一期，这里要关注滞后一期的t、ADF的临界值
** 3.对 X 的2次差分序列的ADF检验
dfuller D2.y, regress trend lag(1) // 模型三：滞后一期，这里要关注滞后一期的t、ADF的临界值
dfuller D2.y, regress lag(1) // 模型二：滞后一期，这里要关注滞后一期的t、ADF的临界值
dfuller D2.y, regress nocon lag(1) // 模型二：滞后一期，这里要关注滞后一期的t、ADF的临界值

```

#### 2.对 X 的1次差分序列的ADF检验
$\begin{align}\Delta^{2} X_{t}&=\delta \Delta X_{t-1}+\sum_{i=1}^m\beta _i\Delta^{2} X_{t-i}+\mu_{t}\\\Delta^{2} X_{t}&=\alpha+\delta \Delta X_{t-1}+\sum_{i=1}^m\beta _i\Delta^{2} X_{t-i}+\mu_{t}\\\Delta^{2} X_{t}&=\alpha+\beta t+\delta \Delta X_{t-1}+\sum_{i=1}^m\beta _i\Delta^{2} X_{t-i}+\mu_{t}\end{align}$ 

#### 2.对 X 的2次差分序列的ADF检验
$\begin{align}\Delta^{3} X_{t}&=\delta \Delta^{2} X_{t-1}+\sum_{i=1}^m\beta _i\Delta^{3} X_{t-i}+\mu_{t}\\\Delta^{3} X_{t}&=\alpha+\delta \Delta^{2} X_{t-1}+\sum_{i=1}^m\beta _i\Delta^{3} X_{t-i}+\mu_{t}\\\Delta^{3} X_{t}&=\alpha+\beta t+\delta \Delta^{2} X_{t-1}+\sum_{i=1}^m\beta _i\Delta^{3} X_{t-i}+\mu_{t}\end{align}$ 

## 5.3. 协整与误差修正模型

### 一、长期均衡与协整分析

#### 1. 问题的提出
- **经典回归模型**要求变量是**平稳**的，若直接对**非平稳变量**进行回归，可能导致**虚假回归**等问题。
- 许多经济变量是**非平稳**的，但若它们之间存在**长期稳定关系**（即**协整关系**），仍可使用经典回归方法建模。
- **例如**：居民实际消费水平与收入水平理论上存在长期稳定关系，即二者协整。

#### 2. 长期均衡
- 经济变量间的**长期均衡关系**意味着系统存在内在的**均衡调整机制**。
- 若变量在某一期偏离长期均衡，则下一期会有调整机制使其**回归均衡**。
- 设长期均衡关系为：
  $$Y_t = \alpha_0 + \alpha_1 X_t + \mu_t$$
- 若 $t-1$ 期末 $Y$ 小于均衡值，则 $t$ 期 $Y$ 的变化往往更大；反之亦然。
- 这意味着偏离是**临时性**的，**随机扰动项 $\mu_t$ 必须是平稳序列**，否则偏离会被长期累积。
- $\mu_t$ 称为**非均衡误差**，若 $X$ 与 $Y$ 协整，则 $\mu_t$ 应为**零均值的 $I(0)$ 序列**。

#### 3. 协整的定义与含义
- 若序列 $\{X_{1t}, X_{2t}, \ldots, X_{kt}\}$ 均为 $d$ 阶单整（$I(d)$），且存在向量 $\alpha = (\alpha_1, \alpha_2, \ldots, \alpha_k)$，使得：
  $$Z_t = \alpha X^T \sim I(d-b),\quad b>0$$
  则称序列为 $(d, b)$ 阶协整，记为 $X_t \sim CI(d, b)$，$\alpha$ 为**协整向量**。
- **两个变量协整的前提**：单整阶数**相同**。
- **$(d, d)$ 阶协整**的意义：两变量虽然各自非平稳，但存在**长期稳定的比例关系**。
- **例如**：居民收入 $X$ 和消费 $Y$ 均为 $I(2)$，若为 $(2,2)$ 阶协整，则存在长期稳定关系，可建立经典回归模型。

### 二、协整的检验

#### 1. 两变量的 Engle–Granger 检验（EG检验）
- **适用条件**：$Y_t \sim I(1), X_t \sim I(1)$。
- **步骤**：
	1. **协整回归（静态回归）**：$Y_t = \alpha_0 + \alpha_1 X_t + \mu_t$
	 用 OLS 估计，得到残差序列 $\hat{e}_t = Y_t - \hat{Y}_t$。
	2. **检验残差的平稳性**：$\Delta e_t = \delta e_{t-1} + \sum_{i=1}^p \theta_i \Delta e_{t-i} + \varepsilon_t$
	 若 $\hat{e}_t$ 为 $I(0)$，则 $Y_t$ 与 $X_t$ 为 $(1,1)$ 阶协整。
- **注意**：
  - 检验残差平稳性时，应使用**协整检验专用临界值**（通常比标准 ADF 临界值更小）。
  - OLS 估计会使 $\delta$ 向下偏倚，容易过度拒绝“不存在协整”的原假设。

- **Stata 示例**：
-
  ```stata
  tsset year
  gen lny = log(y)
  gen lnx = log(x)
  *** 检验 lnY 和 lnX 是否为 I(1)
  dfuller D.lny, reg lag(0)
  dfuller D.lnx, reg lag(0)
  *** 协整回归
  reg lny lnx
  predict e, residual
  *** 检验残差平稳性（注意滞后阶数选择）
  dfuller e, regress nocon lags(4) // 这里检验的统计量要进行调整
  ```

#### 2. 多变量协整关系的检验（扩展EG检验）
- 多变量间可能存在**多个协整向量**，即多种长期均衡关系。
- **检验步骤**：
  1. 检验变量是否为**同阶单整**。
  2. 依次以每个变量为被解释变量，进行 OLS 回归，检验残差是否平稳。
  3. 若所有组合均无法得到平稳残差，则认为不存在协整关系。
- **注意**：多变量协整检验的临界值受变量个数影响，需查专用表。

#### 3. 高阶单整变量的 EG 检验
- EG 检验主要适用于 $I(1)$ 变量。
- 对于高阶单整变量（如 $I(2)$），虽然可形式上使用 EG 两步法，但**残差单位根检验的分布已改变**，且缺乏成熟的临界值表，**理论上不严谨**。

### 三、关于均衡与协整的讨论
- **协整方程 ≠ 均衡方程**：
  - 协整方程具有**统计意义**，均衡方程具有**经济意义**。
  - 经济上的均衡关系必然导致统计上的协整关系，但反之不一定成立。
  - 协整是均衡的**必要条件，非充分条件**。
  - 均衡方程应包含系统中所有变量，而协整方程可只包含部分变量。
  - 协整方程的扰动项只需平稳，均衡方程的扰动项应为白噪声。

### 四、误差修正模型

#### 1. 一般差分模型的问题
- 对非平稳序列进行差分可使其平稳，但建立的差分模型：$Y_{t}=\alpha_{0}+\alpha_{1}X_{t}+\mu_{t},\,\Delta Y_t = \alpha_1 \Delta X_t + v_t$. 前者为长期均衡模型，但后者的误差项是序列相关的，所以只能仅反映**短期关系**，忽略了**长期水平值信息**。
#### 2. 误差修正模型的构建
- 从分布滞后模型出发$Y_t = \beta_0 + \beta_1 X_t + \beta_2 X_{t-1} + \delta Y_{t-1} + \mu_t$ 
- 经变形得到**误差修正模型（ECM）**：$\Delta Y_t = \beta_1 \Delta X_t - \lambda (Y_{t-1} - \alpha_0 - \alpha_1 X_{t-1}) + \mu_t$
  其中：$\lambda = 1-\delta,\quad \alpha_0 = \frac{\beta_0}{1-\delta},\quad \alpha_1 = \frac{\beta_1 + \beta_2}{1-\delta}$
- **简化形式**：$\Delta Y_t = \beta_1 \Delta X_t - \lambda ecm_{t-1} + \mu_t$   $ecm_{t-1}$ 为上一期的非均衡误差。
- **经济解释**：
	- $\lambda \in (0, 1)$，表示对非均衡的**调整速度，短期调整参数**。
	- 若 $Y_{t-1}$ 高于均衡值，则 $\Delta Y_t$ 会减小；反之则增大。
	- $\alpha_{1}$：Y关于X的**长期弹性**
	- $\beta_{1}$：Y关于X的**短期弹性**
	- ecm：**非均衡误差项** / 长期均衡偏差项
	- $\lambda$：**短期调整参数**
#### 3. Granger 表述定理
- 若 $X$ 与 $Y$ 协整，则它们之间的短期关系总能用**误差修正模型**表示。
- 模型形式： $\Delta Y_t = \text{lagged}(\Delta Y, \Delta X) - \lambda ecm_{t-1} + \mu_t$

#### 4. 误差修正模型的建立方法
- **Engle–Granger 两步法**：
	  1. 检验协整关系，估计协整向量，得到残差序列 $e_t$。
	  2. 将 $e_{t-1}$ 作为误差修正项引入 ECM，用 OLS 估计：$\Delta Y_t =\beta_1 \Delta X_t + \gamma \Delta Y_{t-1} - \lambda e_{t-1} + \varepsilon_t$
- **直接估计法**：
	- 将 ECM 展开，直接对包含水平项和差分项的模型进行 OLS 估计。
	- 仍需先检验协整关系。
- **注意**：
	- 若误差修正项系数 $\lambda$ 估计值为**正**，则模型有误。
	- 滞后阶数可通过检验残差**是否存在自相关**来确定（如 LM 检验）。

- **Stata 示例**：
1
  ```stata
  tsset year
  gen lny = log(y)
  gen lnx = log(x)
  reg lny lnx
  predict e,residual
  *** 建立 ECM
  reg D.lny D.lnx LD.lny L.e, nocon
  *** ECM模型滞后项阶数确定
  *** 检验残差自相关
  qui reg D.lny D.lnx LD.lny L.e
  estat bgodfrey, lags(1)
  *** 或者采用Q统计量对残差检验
  predict e_ecm, residual
  corrgram e_ecm
  wntestb e_ecm, table
  ```


## 5.4. 格兰杰因果关系检验

### 一、时间序列自回归模型

#### 1. 随机时间序列模型
- **结构模型**：揭示变量间在每个时点的结构关系（如协整模型）。
- **随机时间序列模型**：揭示同一变量在不同时点观测值之间的关系，用于**无条件预测**。
	- 包括：$AR(p)$、$MA(q)$、$ARMA(p,q)$。
	- 此类模型不属于经典计量经济学范畴。

#### 2. 自回归模型 $AR(p)$
- **定义**：$X_t = \phi_1 X_{t-1} + \phi_2 X_{t-2} + \cdots + \phi_p X_{t-p} + \varepsilon_t$ 
- **平稳性条件**：
	- 特征方程 $\Phi(z) = 1 - \phi_1 z - \cdots - \phi_p z^p = 0$ 的所有根**均在单位圆外**（模大于1）。
	- **$AR(1)$ 平稳**：$|\varphi| < 1$。
	- **$AR(2)$ 平稳**：需满足 $\varphi_1 + \varphi_2 < 1,\ \varphi_2 - \varphi_1 < 1,\ |\varphi_2|<1$。

#### 3. 移动平均模型 $MA(q)$ 与 $ARMA(p,q)$
- $MA(q)$ 总是平稳的
- $ARMA(p,q)$ 的平稳性取决于其 $AR(p)$ 部分

### 二、向量自回归模型（VAR）

#### 1. 定义
- 将 $AR$ 模型扩展至多个变量： $Y_t = \mu + A_1 Y_{t-1} + \cdots + A_p Y_{t-p} + \varepsilon_t$
	- $Y_t$ 为 $k$ 维向量，$A_j$ 为 $k \times k$ 系数矩阵。

#### 2. 建模与估计
- **优点**：无需预设经济理论结构，由数据决定动态关系。
- **关键选择**：变量个数 $k$ 和滞后阶数 $p$。
- **滞后阶数确定**：常用信息准则（AIC、SC）或 LR 统计量。
- **应用**：主要用于预测、脉冲响应分析和方差分解。

### 三、格兰杰因果关系检验
#### 1. 原理
- 检验一个变量的**过去值**是否对另一个变量的**当前值**有预测能力。
- 基于 VAR 模型，检验滞后项的联合显著性。

#### 2. 检验模型
- 对于 $X$ 和 $Y$，建立两个方程：
  $\begin{cases}Y_t = \beta_0 + \sum_{i=1}^p \beta_i Y_{t-i} + \sum_{i=1}^p \alpha_i X_{t-i} + \mu_t \\ X_t = \delta_0 + \sum_{i=1}^p \delta_i Y_{t-i} + \sum_{i=1}^p \lambda_i X_{t-i} + \nu_t\end{cases}$ 
- **检验假设**：
	- $X$ 不是 $Y$ 的格兰杰原因：$H_0: \alpha_1 = \alpha_2 = \cdots = \alpha_p = 0$
	- $Y$ 不是 $X$ 的格兰杰原因：$H_0: \delta_1 = \delta_2 = \cdots = \delta_p = 0$
- **检验统计量**：$F$ 检验 $F= \frac{(RSS_{R}-RSS_{U})/m}{RSS_{U}/(n-k)}>F_{\alpha}(m,n-k)$ 

#### 3. 应用中需注意的问题
1. **滞后阶数选择**：结果对滞后阶数敏感，应通过信息准则或序列相关检验确定。
2. **平稳性要求**：
	- 理论上要求 X, Y 序列平稳。
	- 对于同阶单整的非平稳序列，检验结果仍具有一定可靠性，但经济意义可能变化。
3. **样本容量**：样本量越大，检验效力越高。对样本量也比较敏感。
4. **统计意义 vs. 经济意义**：
	- 格兰杰因果关系是**统计上的预测关系**，不一定是经济上的因果机制。
	- 它是经济因果关系的**必要条件，非充分条件**。

#### 4. Stata 示例
```stata
*** 生成增长率序列
gen gy = D.y / L.y
gen gx = D.x / L.x
dfuller gy, reg // 格兰杰使用前提：gx,gy 是平稳的
dfuller gx, reg
*** 格兰杰因果关系检验（滞后1阶）
gcause gy gx, lags(1) // gx 是否是 gy 的原因
qui reg gy L(1).(gx gy) 
estat bgodfrey, lags(1) // 要求是白噪音
estat ic // AIC 越小越好 —— 这两项是用来选到底是哪个滞后期更优的
gcause gx gy, lags(1) // gy 是否是 gx 的原因
qui reg gx L(1).(gx gy) 
estat bgodfrey, lags(1)
estat ic
*** 格兰杰因果关系检验（滞后2阶）
gcause gy gx, lags(2) // gx 是否是 gy 的原因
qui reg gy L(1/2).(gx gy) 
estat bgodfrey
estat ic
gcause gx gy, lags(2) // gy 是否是 gx 的原因
qui reg gx L(1/2).(gx gy) 
estat bgodfrey
estat ic
*** VAR模型格兰杰因果检验
varsoc gy gx, maxlag(6) // 只有滞后一期显著
var gy gx, lags(1)
vargranger
varlmar // 模型序列相关检验，LM检验
```


