
> 记号：
> > 变量上加波浪号表示对数偏离稳态；
> > $\bar E_t[\cdot]$为平均预期；
> > $\bar E_t^h[\cdot]$为$h$阶平均信念；
> > $\Theta_T$为未来第$T$期外生基本面；
> > $T$为新闻/政策行动期限。  

---

## 0. 背景补充

### 0.1 共同知识与高阶信念

*理性预期均衡 (rational expectations equilibrium, REE)* 要求预期与均衡结果一致。
标准宏观金融分析还假设 *共同知识(common knowledge, CK)* ：每个人知道新闻，且知道别人知道，知道别人知道自己知道，无限递推。

本文去掉的不是“人知不知道新闻”，而是“人是否确信别人知道、别人会如何反应”。在NK模型中，当前消费和定价依赖对未来总需求、通胀、他人行为的预期；共同知识会把GE反馈环开到最大。

一阶信念：$E_{i,t}[\Theta_T]$。
二阶信念：$E_{i,t}[E_{j,t}[\Theta_T]]$。
$h$阶信念：对他人信念迭代$h$次。
*高阶不确定性(higher-order uncertainty)* 指个体对他人信息、信念、反应不确定。
它可来自分散私人信息，也可来自 *理性疏忽(rational inattention)*。

### 0.2 选美比赛与NK最小背景

*选美比赛(beauty contest)*：最优行动依赖对他人行动的预期。动态版本中，今天行动依赖对未来他人行动的预期。本文把NK的消费块、定价块都写成动态选美比赛。

NK最小背景：
消费欧拉方程 $c_t=-\sigma E_t[r_{t+1}]+E_t[c_{t+1}]$；
新凯恩斯菲利普斯曲线(New Keynesian Phillips curve, NKPC)为$\pi_t=\kappa mc_t+\beta E_t[\pi_{t+1}]+\kappa\mu_t$；
卡尔沃定价(Calvo pricing)中每期比例$\theta$的厂商不能调价；
零利率下限(zero lower bound, ZLB)下名义利率不能降，只能承诺未来低利率；
前向指引谜题(forward guidance puzzle)指标准NK预测未来降息效果随期限$T$增大甚至爆炸，与“远期新闻应被贴现”的PE直觉矛盾。

### 0.3 本文一句话

去掉共同知识、引入高阶不确定性后，GE反馈环被“锚定”，新闻的GE效应衰减；期限越长衰减越强；极限下相对完全信息效果趋于0。这解释前向指引谜题，并为财政刺激“前置(front-loading)”提供理由。

**※【例题】** 共同知识与高阶信念：设未来基本面$\Theta_T$有公共新闻，但每个人观察到“新闻+独立噪声”。每个人自己的一阶预期随新闻调整；但当他预测“别人如何预测”时，别人的噪声被平均掉，二阶预期更靠近先验。于是高阶信念对新闻的反应小于一阶信念。这是本文$\beta_h<1$的直觉。

---

## 1. 模型框架

### 1.1 消费者

连续统$i\in[0,1]$。
偏好：$U_{i,0}=\sum_{t=0}^{\infty}\beta^tU(c_{i,t},n_{i,t})$, $U(c,n)=\frac{1}{1-1/\sigma}c^{1-1/\sigma}-\frac{1}{1+\epsilon}n^{1+\epsilon}$.
预算约束：$c_{i,t}+s_{i,t}=\frac{R_{t-1}}{\pi_t}s_{i,t-1}+w_{i,t}n_{i,t}+e_{i,t}$ (2).

#### 1.1.1 效用函数的导数

记$u(c)\equiv\frac{1}{1-1/\sigma}c^{1-1/\sigma}$，$v(n)\equiv\frac{1}{1+\epsilon}n^{1+\epsilon}$，则$U(c,n)=u(c)-v(n)$。

边际效用：$u'(c)=c^{-1/\sigma}$ (2a), $v'(n)=n^{\epsilon}$ (2b)
二阶导数：$u''(c)=-\frac{1}{\sigma}c^{-1/\sigma-1}<0$, $v''(n)=\epsilon n^{\epsilon-1}>0$
其中$\sigma>0$是 跨期替代弹性(elasticity of intertemporal substitution)，
$\epsilon>0$是Frisch劳动供给弹性的倒数。
#### 1.1.2 消费者问题的拉格朗日形式

把预算约束(2)移项：$c_{i,t}+s_{i,t}-\frac{R_{t-1}}{\pi_t}s_{i,t-1}-w_{i,t}n_{i,t}-e_{i,t}=0$.
消费者在每期$t$选择$\{c_{i,t},n_{i,t},s_{i,t}\}_{t=0}^{\infty}$最大化$U_{i,0}$。
拉格朗日函数：
$\mathcal L=\sum_{t=0}^{\infty}\beta^t\left\{u(c_{i,t})-v(n_{i,t})+\mu_{i,t}\left[\frac{R_{t-1}}{\pi_t}s_{i,t-1}+w_{i,t}n_{i,t}+e_{i,t}-c_{i,t}-s_{i,t}\right]\right\}$
其中$\mu_{i,t}$是$t$期预算约束的拉格朗日乘子。

#### 1.1.3 一阶条件

对$c_{i,t}$求导：$\frac{\partial\mathcal L}{\partial c_{i,t}}=\beta^t[u'(c_{i,t})-\mu_{i,t}]=0$
所以 $\mu_{i,t}=u'(c_{i,t})=c_{i,t}^{-1/\sigma}$ (2c)

对$n_{i,t}$求导：$\frac{\partial\mathcal L}{\partial n_{i,t}}=\beta^t[-v'(n_{i,t})+\mu_{i,t}w_{i,t}]=0$
所以 $v'(n_{i,t})=\mu_{i,t}w_{i,t}$, 即 $n_{i,t}^{\epsilon}=c_{i,t}^{-1/\sigma}w_{i,t}$ (2d)

对$s_{i,t}$求导：$s_{i,t}$出现在$t$期预算约束（系数$-1$）和$t+1$期预算约束（系数$\frac{R_t}{\pi_{t+1}}$）。
所以 $\frac{\partial\mathcal L}{\partial s_{i,t}}=\beta^t(-\mu_{i,t})+\beta^{t+1}\mu_{i,t+1}\frac{R_t}{\pi_{t+1}}=0$

两边除以$\beta^t$：$\mu_{i,t}=\beta\frac{R_t}{\pi_{t+1}}\mu_{i,t+1}$ (2e)

#### 1.1.4 劳动供给条件(28)

由(2d)：$n_{i,t}^{\epsilon}=c_{i,t}^{-1/\sigma}w_{i,t}$

两边取对数，用小写波浪号表示对数偏离稳态： $\epsilon\tilde n_{i,t}=-\frac{1}{\sigma}\tilde c_{i,t}+\tilde w_{i,t}$

所以 $\tilde n_{i,t}=\frac{1}{\epsilon}\left(\tilde w_{i,t}-\frac{1}{\sigma}\tilde c_{i,t}\right)$ (28)

#### 1.1.5 欧拉方程(29)

由(2e)和(2c)： $c_{i,t}^{-1/\sigma}=\beta\frac{R_t}{\pi_{t+1}}c_{i,t+1}^{-1/\sigma}$ 这是**确定性**版本。

引入预期后，消费者在$t$期不知道$\pi_{t+1}$和$c_{i,t+1}$，
所以用条件期望：$c_{i,t}^{-1/\sigma}=\beta E_{i,t}\left[\frac{R_t}{\pi_{t+1}}c_{i,t+1}^{-1/\sigma}\right]$ (2f)

对数线性化。设稳态$\pi=1$，$\beta R=1$，$c=c^*$。

对(2f)两边取对数：
$-\frac{1}{\sigma}\ln c_{i,t}=\ln\beta+\ln R_t-E_{i,t}[\ln\pi_{t+1}]-\frac{1}{\sigma}E_{i,t}[\ln c_{i,t+1}]$
写成对数偏离：
$-\frac{1}{\sigma}\tilde c_{i,t}=\ln\beta+\tilde R_t-E_{i,t}[\tilde\pi_{t+1}]-\frac{1}{\sigma}E_{i,t}[\tilde c_{i,t+1}]$

因为稳态$\beta R=1$，所以$\ln\beta+\tilde R_t=\tilde R_t-\ln R^*$。但$\tilde R_t$本身就是$\ln(R_t/R^*)$，所以$\ln\beta+\tilde R_t=\ln(\beta R_t)=\ln(R_t/R^*)=\tilde R_t$。更直接地，稳态时$\ln\beta+\ln R^*=0$，所以$\ln\beta+\tilde R_t=\tilde R_t$（把$\tilde R_t$定义为$\ln R_t-\ln R^*$）。

于是：$-\frac{1}{\sigma}\tilde c_{i,t}=\tilde R_t-E_{i,t}[\tilde\pi_{t+1}]-\frac{1}{\sigma}E_{i,t}[\tilde c_{i,t+1}]$

两边乘以$-\sigma$：$\tilde c_{i,t}=E_{i,t}[\tilde c_{i,t+1}]-\sigma(\tilde R_t-E_{i,t}[\tilde\pi_{t+1}])$

定义实际利率$\tilde r_{t+1}\equiv\tilde R_t-\tilde\pi_{t+1}$，得 $\tilde c_{i,t}=E_{i,t}[\tilde c_{i,t+1}-\sigma\tilde r_{t+1}]$ (29)

#### 1.1.6 从(27)(28)(29)到消费函数(30)

这一步是把预算约束(27)和欧拉方程(29)结合，解出当前消费。组会上可以只讲思路，代数放备查。

由(28)：$\tilde n_{i,t+k}=\frac{1}{\epsilon}\left(\tilde w_{i,t+k}-\frac{1}{\sigma}\tilde c_{i,t+k}\right)$

代入(27)右边：$\tilde y_{i,t+k}=\Omega(\tilde w_{i,t+k}+\tilde n_{i,t+k})+(1-\Omega)\tilde e_{i,t+k}$
$=\Omega\left(1+\frac{1}{\epsilon}\right)\tilde w_{i,t+k}-\frac{\Omega}{\epsilon\sigma}\tilde c_{i,t+k}+(1-\Omega)\tilde e_{i,t+k}$

所以(27)变为：
$\sum_{k=0}^{\infty}\beta^k\tilde c_{i,t+k}=\tilde a_{i,t}+\sum_{k=0}^{\infty}\beta^k\left\{\frac{(\epsilon+1)\Omega}{\epsilon}\tilde w_{i,t+k}-\frac{\Omega}{\epsilon\sigma}\tilde c_{i,t+k}+(1-\Omega)\tilde e_{i,t+k}\right\}$

把右边的$\tilde c$项移到左边：
$\left(1+\frac{\Omega}{\epsilon\sigma}\right)\sum_{k=0}^{\infty}\beta^k\tilde c_{i,t+k}=\tilde a_{i,t}+\sum_{k=0}^{\infty}\beta^k\left\{\frac{(\epsilon+1)\Omega}{\epsilon}\tilde w_{i,t+k}+(1-\Omega)\tilde e_{i,t+k}\right\}$

注意$1+\frac{\Omega}{\epsilon\sigma}=\frac{\epsilon\sigma+\Omega}{\epsilon\sigma}$。两边乘以$\frac{\epsilon\sigma}{\epsilon\sigma+\Omega}$：
$\sum_{k=0}^{\infty}\beta^k\tilde c_{i,t+k}=\frac{\epsilon\sigma}{\epsilon\sigma+\Omega}\tilde a_{i,t}+\sum_{k=0}^{\infty}\beta^k\left\{\frac{(\epsilon+1)\sigma\Omega}{\epsilon\sigma+\Omega}\tilde w_{i,t+k}+\frac{\epsilon\sigma(1-\Omega)}{\epsilon\sigma+\Omega}\tilde e_{i,t+k}\right\}$ (2g)

现在用欧拉方程把未来消费表示成当前消费加未来利率。

由(29)迭代：
$\tilde c_{i,t+k}=\tilde c_{i,t}+\sigma\sum_{j=1}^{k}E_{i,t}[\tilde r_{t+j}]+\text{预期误差}$

在$t$期信息下取期望：
$E_{i,t}[\tilde c_{i,t+k}]=\tilde c_{i,t}+\sigma\sum_{j=1}^{k}E_{i,t}[\tilde r_{t+j}]$ (2h)

对(2g)两边取$E_{i,t}[\cdot]$，并用(2h)：
左边$=\sum_{k=0}^{\infty}\beta^k E_{i,t}[\tilde c_{i,t+k}]=\sum_{k=0}^{\infty}\beta^k\left\{\tilde c_{i,t}+\sigma\sum_{j=1}^{k}E_{i,t}[\tilde r_{t+j}]\right\}$
$=\frac{1}{1-\beta}\tilde c_{i,t}+\sigma\sum_{k=1}^{\infty}\beta^k\sum_{j=1}^{k}E_{i,t}[\tilde r_{t+j}]$

交换求和：
$\sum_{k=1}^{\infty}\beta^k\sum_{j=1}^{k}E_{i,t}[\tilde r_{t+j}]=\sum_{j=1}^{\infty}E_{i,t}[\tilde r_{t+j}]\sum_{k=j}^{\infty}\beta^k=\sum_{j=1}^{\infty}E_{i,t}[\tilde r_{t+j}]\frac{\beta^j}{1-\beta}$

所以：
$\frac{1}{1-\beta}\tilde c_{i,t}+\frac{\sigma}{1-\beta}\sum_{j=1}^{\infty}\beta^jE_{i,t}[\tilde r_{t+j}]=\frac{\epsilon\sigma}{\epsilon\sigma+\Omega}\tilde a_{i,t}+\sum_{k=0}^{\infty}\beta^kE_{i,t}\left\{\frac{(\epsilon+1)\sigma\Omega}{\epsilon\sigma+\Omega}\tilde w_{i,t+k}+\frac{\epsilon\sigma(1-\Omega)}{\epsilon\sigma+\Omega}\tilde e_{i,t+k}\right\}$

两边乘以$(1-\beta)$，并把利率项移到右边：
$\tilde c_{i,t}=\frac{(1-\beta)\epsilon\sigma}{\epsilon\sigma+\Omega}\tilde a_{i,t}-\sigma\sum_{j=1}^{\infty}\beta^jE_{i,t}[\tilde r_{t+j}]+(1-\beta)\sum_{k=0}^{\infty}\beta^kE_{i,t}\left\{\frac{(\epsilon+1)\sigma\Omega}{\epsilon\sigma+\Omega}\tilde w_{i,t+k}+\frac{\epsilon\sigma(1-\Omega)}{\epsilon\sigma+\Omega}\tilde e_{i,t+k}\right\}$

把$k=0$项单独写出：
$\tilde c_{i,t}=\frac{(1-\beta)\epsilon\sigma}{\epsilon\sigma+\Omega}\tilde a_{i,t}-\sigma\sum_{k=1}^{\infty}\beta^kE_{i,t}[\tilde r_{t+k}]+(1-\beta)\left[\frac{(\epsilon+1)\sigma\Omega}{\epsilon\sigma+\Omega}\tilde w_{i,t}+\frac{\epsilon\sigma(1-\Omega)}{\epsilon\sigma+\Omega}\tilde e_{i,t}\right]+(1-\beta)\sum_{k=1}^{\infty}\beta^kE_{i,t}\left[\frac{(\epsilon+1)\sigma\Omega}{\epsilon\sigma+\Omega}\tilde w_{i,t+k}+\frac{\epsilon\sigma(1-\Omega)}{\epsilon\sigma+\Omega}\tilde e_{i,t+k}\right]$ (30)

#### 1.1.7 后面用到的三个聚合事实

把(30)对$i$聚合时，用到：

1. 资产平均为零：$\int a_{i,t}di=0$，所以$\tilde a_t=0$。
2. 未来异质冲击不可预测：$E_{i,t}[\xi_{i,t+k}]=E_{i,t}[\zeta_{i,t+k}]=0$对$k\ge1$，所以聚合时未来工资/分红中的异质部分消失。
3. 市场出清与工资利润关系：

$\tilde w_t=(\epsilon+1/\sigma)\tilde y_t$，$\tilde e_t=[1-\Omega(\epsilon+1/\sigma)/(1-\Omega)]\tilde y_t$，$\frac{(\epsilon+1)\sigma\Omega}{\epsilon\sigma+\Omega}\tilde w_t+\frac{\epsilon\sigma(1-\Omega)}{\epsilon\sigma+\Omega}\tilde e_t=\tilde y_t$ (34)

把(34)代入(31)，并利用$\tilde y_t=\tilde c_t$，即得正文(5)：

$c_t=-\sigma\{\sum_{k=1}^{\infty}\beta^{k-1}\bar E_t^c[r_{t+k}]\}+\frac{1-\beta}{\beta}\{\sum_{k=1}^{\infty}\beta^k\bar E_t^c[c_{t+k}]\}$ (5)

### 1.2 厂商与Calvo定价

厂商$j\in(1,2]$，线性技术$y_t^j=l_t^j$ (3)。
最终品CES加总，弹性$\zeta>1$。每期比例$\theta$厂商不能调价，$1-\theta$可重设价格。

### 1.3 货币政策与信息

暂不指定政策规则；可设想短期ZLB绑定，但对更远未来政策不确定。
全文坚持REE，但去掉共同知识，允许高阶不确定性。

### 1.4 辅助冲击：给市场信号“加噪”

为保持“没有共同知识”，又不让价格等市场信号完全揭示信息，作者给消费者工资/分红、厂商工资/加成加i.i.d.异质冲击：$w_{i,t}=w_t\xi_{i,t}$，$e_{i,t}=e_t\zeta_{i,t}$，$w_t^j=w_tu_t^j$，$\mu_t^j=\mu_t\nu_t^j$。这些冲击的唯一作用是让个体无法从市场信号中完美推断总体状态。

---

## 2. NK模型作为一对动态选美比赛

### 2.1 两个“开放的”均衡方程

**※【命题】** Proposition 1（Beauty Contests）：
无论高阶不确定性水平如何，均衡中：
$c_t=-\sigma\{\sum_{k=1}^{\infty}\beta^{k-1}\bar E_t^c[r_{t+k}]\}+\frac{1-\beta}{\beta}\{\sum_{k=1}^{\infty}\beta^k\bar E_t^c[c_{t+k}]\}$ (5)
$\pi_t=\kappa\{mc_t+\sum_{k=1}^{\infty}(\beta\theta)^k\bar E_t^f[mc_{t+k}]\}+\frac{1-\theta}{\theta}\{\sum_{k=1}^{\infty}(\beta\theta)^k\bar E_t^f[\pi_{t+k}]\}+\kappa\mu_t$ (6)
其中$\kappa\equiv(1-\theta)(1-\beta\theta)/\theta$，$mc_t=(\epsilon+1/\sigma)y_t$，$y_t=c_t$。

**证明（Appendix A, Proposition 1 四步）：**

Step 1：消费者。
设$a_{i,t}=R_{t-1}s_{i,t-1}/\pi_t$。

$c_{i,t}+\left(  \frac{R_{t}}{\pi_{t+1}} \right)^{-1}a_{i,t+1}=a_{i,t}+w_{i,t}n_{i,t}+e_{i,t}$,
$\left(  \frac{R_{t}}{\pi_{t+1}} \right)^{-1}c_{i,t+1}+\left(  \frac{R_{t}}{\pi_{t+1}} \right)^{-1}\left(  \frac{R_{t+1}}{\pi_{t+2}} \right)^{-1}a_{i,t+2}=\left(  \frac{R_{t}}{\pi_{t+1}} \right)^{-1}(a_{i,t+1}+w_{i,t+1}n_{i,t+1}+e_{i,t+1})$,
跨期预算约束为
$\sum_{k=0}^{\infty}\{[\prod_{j=1}^{k}(\frac{R_{t+j-1}}{\pi_{t+j}})^{-1}]c_{i,t+k}\}=a_{i,t}+\sum_{k=0}^{\infty}\{[\prod_{j=1}^{k}(\frac{R_{t+j-1}}{\pi_{t+j}})^{-1}](w_{i,t+k}n_{i,t+k}+e_{i,t+k})\}$ (26)

对数线性化得
$\sum_{k=0}^{\infty}\beta^k\tilde c_{i,t+k}=\tilde a_{i,t}+\sum_{k=0}^{\infty}\beta^k\{\Omega(\tilde w_{i,t+k}+\tilde n_{i,t+k})+(1-\Omega)\tilde e_{i,t+k}\}$ (27)
其中$\Omega$为稳态劳动收入占总收入比。

劳动供给与个体欧拉方程：
$\tilde n_{i,t}=\frac1\epsilon(\tilde w_{i,t}-\frac1\sigma\tilde c_{i,t})$ (28)
$\tilde c_{i,t}=E_{i,t}[\tilde c_{i,t+1}-\sigma\tilde r_{t+1}]$ (29)

将(28)代入(27)，并用(29)把未来消费递推为当前消费与预期实际利率，解出
$\tilde c_{i,t}=-\sigma\sum_{k=1}^{\infty}\beta^kE_{i,t}[\tilde r_{t+k}]+(1-\beta)[\cdots]+(1-\beta)\sum_{k=1}^{\infty}\beta^kE_{i,t}[\cdots]$ (30)
聚合。

资产平均为0，未来异质冲击不可预测，得(31)。

利用市场出清与工资/利润关系
$\tilde w_t=(\epsilon+1/\sigma)\tilde y_t$，$\tilde e_t=[1-\Omega(\epsilon+1/\sigma)/(1-\Omega)]\tilde y_t$，$\frac{(\epsilon+1)\sigma\Omega}{\epsilon\sigma+\Omega}\tilde w_t+\frac{\epsilon\sigma(1-\Omega)}{\epsilon\sigma+\Omega}\tilde e_t=\tilde y_t$ (34)
以及$\tilde y_t=\tilde c_t$，可将(31)整理为(5)。

Step 2：厂商。
可重设价格厂商的reset price为
$\tilde p_t^{j*}=(1-\beta\theta)\{(\tilde mc_t^j+\tilde p_t)+\sum_{k=1}^{\infty}(\beta\theta)^kE_{j,t}[\tilde mc_{t+k}^j+\tilde p_{t+k}]\}+(1-\beta\theta)\tilde\mu_t^j$ (32)
聚合，利用$\tilde\pi_t=(1-\theta)(\tilde p_t^*-\tilde p_{t-1})$，得
$\tilde\pi_t=\kappa\tilde mc_t+\kappa\sum_{k=1}^{\infty}(\beta\theta)^k\bar E_t^f[\tilde mc_{t+k}]+\frac{1-\theta}{\theta}\sum_{k=1}^{\infty}(\beta\theta)^k\bar E_t^f[\tilde\pi_{t+k}]+\kappa\tilde\mu_t$ (33)
其中$\kappa=(1-\theta)(1-\beta\theta)/\theta$。再代入$mc_t=(\epsilon+1/\sigma)y_t$与$y_t=c_t$，即(6)。

Step 3：市场出清。
$\tilde y_t=\tilde n_t=\tilde c_t$，工资利润关系如(34)。

Step 4：将(34)代入(31)得(5)，将(34)代入(33)得(6)。

### 2.2 共同知识下回到标准Euler与NKPC

共同知识下$\bar E_t^c=E_t$，迭代期望律成立：
$\bar E_t^c[\bar E_{t+1}^c[r_{t+1+k}]]=E_t[r_{t+1+k}]$
因此(5)化为
$c_t=-\sigma E_t[r_{t+1}]+E_t[c_{t+1}]$ (9)
(6)化为
$\pi_t=\chi mc_t+\beta E_t[\pi_{t+1}]+\chi\mu_t$ (10)
即标准欧拉方程与NKPC。离开共同知识后，平均信念不满足迭代期望律，所以(5)(6)不能化成单代理人方程。

### 2.3 三个GE机制与高阶信念

1. 需求块：Keynesian income-spending multiplier。今天多消费因为预期别人未来多消费，未来收入高。
2. 供给块：价格设定中的动态战略互补。我的reset price依赖我对未来价格水平的预期，而未来价格水平依赖别人定价。
3. 跨块：inflation-spending feedback。预期未来通胀高，实际利率低，当前消费高；预期未来产出高，边际成本高，当前通胀高。

PE vs GE：在(5)中，固定$\bar E_t^c[c_{t+k}]$，一单位$\bar E_t^c[r_{t+k}]$下降使$c_t$升$\beta^{k-1}\sigma$，这是PE。GE来自未来消费预期本身的调整。理解GE调整等价于理解信念层级。

**※【例题】** 需求块PE：若只看未来第$T$期实际利率新闻，PE效应是$\sigma\beta^{T-1}$，随$T$递减。但标准NK中GE反馈可使总效应不降甚至上升，这就是前向指引谜题的来源。

---

## 3. 动态选美比赛：抽象理论

### 3.1 Setup

连续统agent $i$，每期选择$a_{i,t}$。

最优反应：
$a_{i,t}=\Theta_t+\gamma E_{i,t}[a_{i,t+1}]+\alpha E_{i,t}[a_{t+1}]$ (11)
$\gamma>0$为PE参数，$\alpha>0$为GE参数。

向前迭代、聚合得
$a_t=\Theta_t+\gamma\{\sum_{k=1}^{\infty}\gamma^{k-1}\bar E_t[\Theta_{t+k}]\}+\alpha\{\sum_{k=1}^{\infty}\gamma^{k-1}\bar E_t[a_{t+k}]\}$ (12)
共同知识下$\bar E_t=E_t$，迭代期望得
$a_t=\Theta_t+\delta E_t[a_{t+1}]$ (13)
$\delta\equiv\gamma+\alpha$。

嵌套关系：
供给块：$\Theta_t=\kappa mc_t$，$a_t=\pi_t$，$\gamma=\beta\theta$，$\alpha=\beta(1-\theta)$。
需求块：$\Theta_t=-\sigma r_t$，$a_t=c_t$，$\gamma=\beta$，$\alpha=1-\beta$。
资产定价：$p_t=d_t-s_t+\beta\bar E_t[p_{t+1}]$，$\gamma=0$，$\alpha=\beta$。

### 3.2 完全信息基准

只让$\Theta_T$随机，其余$\Theta_t=0$。

**※【引理】** Lemma 1：
完全信息下 $a_0=\phi_T^*E_0[\Theta_T]$，$\phi_T^*=\delta^T=(\gamma+\alpha)^T$。

**证明：** 
因$\Theta_t=0$ 对$t\neq T$，有$a_T=\Theta_T$。
对$t\le T-1$，由(13)向前迭代：$a_{T-1}=\delta E_{T-1}[\Theta_T]$，$a_{T-2}=\delta E_{T-2}[a_{T-1}]=\delta^2E_{T-2}[\Theta_T]$，
归纳得$a_t=\delta^{T-t}E_t[\Theta_T]$。
令$t=0$即得。递归形式：$\phi_0^*=1$，$\phi_{T-t}^*=\delta\phi_{T-t-1}^*$，故$\phi_T^*=\delta^T$。

**※【定义】** Definition 1：信息完全（共同知识）
若$E_{i,t}[\Theta_T]=E_{j,t}[\Theta_T]$ w.p.1 对所有$i\neq j,t\le T-1$。  

**※【定义】** Definition 2：
对未来路径有共识若$E_{i,t}[a_\tau]=E_{j,t}[a_\tau]$ w.p.1 对所有$i\neq j,t<\tau\le T$。

**※【命题】** Proposition 2：
沿REE，只有信息不完全时才可能缺乏共识。

**证明（Appendix A）：** 
因$\Theta_t=0$对$t>T$，$a_t=0$对$t>T$。
迭代(12)，$a_t$可写成平均一阶及高阶信念关于$\Theta_T$的线性函数。完全信息下所有人共享一阶信念且此为共同知识，高阶信念坍缩为一阶信念，故$a_t$是$E_t[\Theta_T]$的已知线性函数。对任意$t<\tau\le T$和任意$i,j$，完全信息保证$E_{i,t}[E_\tau[\Theta_T]]=E_t[\Theta_T]=E_{j,t}[E_\tau[\Theta_T]]$ w.p.1；又$a_\tau$是$E_\tau[\Theta_T]$的已知线性函数，故$E_{i,t}[a_\tau]=E_{j,t}[a_\tau]$。即完全信息排除无共识。

### 3.3 不完全信息、无学习

**※【假设】** Assumption 1：
$t=0$到$T$之间无学习；$\Theta_T$在$T$时刻成为共同知识。
于是$t\le T-1$时$\bar E_t=\bar E_0$。
信号$\omega_i$为Harsanyi类型，条件分布$\phi(\omega|s)$。

只T期冲击时：
$a_t=\gamma^{T-t}\bar E_t[\Theta_T]+\alpha\sum_{k=1}^{T-t}\gamma^{k-1}\bar E_t[a_{t+k}]$ (16)

**※【引理】** Lemma 2：
存在正系数$\{\chi_{h,T}\}_{h=1}^T$，使 $a_0=\sum_{h=1}^T\chi_{h,T}\bar E_0^h[\Theta_T]$ (17)。

**证明（Appendix A，归纳）：** 证明更强结论：
对任意$t\le T-1$，$a_t=\sum_{h=1}^{T-t}\chi_{h,T-t}\bar E_t^h[\Theta_T]$ (35) 其中$\bar E_t^1=\bar E_t$，$\bar E_t^h=\bar E_t[\bar E_t^{h-1}]$。
$t=T-1$时，由(16)得$a_{T-1}=(\gamma+\alpha)\bar E_{T-1}[\Theta_T]$，故$\chi_{1,1}=\gamma+\alpha$。
假设(35)对$t+1$成立，代入(16)。
利用无学习$\bar E_t[\bar E_{t+1}^h[\Theta_T]]=\bar E_t^{h+1}[\Theta_T]$，得
$a_t=\gamma^{T-t-1}(\gamma+\alpha)\bar E_t[\Theta_T]+\sum_{h=1}^{T-t-1}\sum_{k=1}^{T-t-h}\alpha\gamma^{k-1}\chi_{h,T-t-k}\bar E_t^{h+1}[\Theta_T]$ (36)
故
$\chi_{1,T-t}=\gamma^{T-t-1}(\gamma+\alpha)$
$\chi_{h+1,T-t}=\sum_{k=1}^{T-t-h}\alpha\gamma^{k-1}\chi_{h,T-t-k}$ (37)
归纳完成。完全信息下高阶=一阶，故$\phi_T^*=\sum_{h=1}^T\chi_{h,T}$ (18)。

### 3.4 核心定理：GE衰减与期限效应

定义投影系数：$\beta_h\equiv\frac{Cov(\bar E_0^h[\Theta_T],\bar E_0^1[\Theta_T])}{Var(\bar E_0^1[\Theta_T])}$ (21)
完全信息下$\beta_h=1$。

**※【定理】** Theorem 1：
不完全信息下存在$\phi_T$使 $a_0=\phi_T\bar E_0[\Theta_T]+\epsilon$ (19) 其中$\epsilon$与$\bar E_0[\Theta_T]$正交；且  
(i) $\phi_T/\phi_T^*<1$；  
(ii) $\phi_T/\phi_T^*$随$T$严格递减。

**证明（Appendix A；依赖后文Prop3与Theorem2）：** 
Part (i)由投影定义直接得：将$a_0$对$\bar E_0[\Theta_T]$投影，$\phi_T$为投影系数，$\epsilon$为残差，故与$\bar E_0[\Theta_T]$正交。
Part (ii)：
由Lemma 2，$\phi_T=\sum_{h=1}^T\chi_{h,T}\beta_h$ (20) 完全信息下$\beta_h=1$，故$\phi_T^*=\sum_{h=1}^T\chi_{h,T}$。
由Prop3，$\beta_h<1$对所有$h\ge2$，且$\beta_h$随$h$严格递减。
由Theorem 2，期限$T$越长，高阶信念的相对权重$s_{h,T}/s_{T,T}$越大。两者结合可得$\phi_T/\phi_T^*<1$且随$T$严格递减。详细代数见下Prop3与Theorem2证明；Theorem1的完整证明正是将(20)与$s_{h,T}$分解结合。

**※【命题】** Proposition 3：
不完全信息下，$\beta_h\in(0,1)$对所有$h\ge2$，且$\beta_h$随$h$严格递减。

**证明（Appendix A）：** 
由信号i.i.d.与对称性，对$h\ge2$，$Cov(\bar E_0^h[\Theta_T],\bar E_0^1[\Theta_T])=Cov(\bar E_0^{h-1}[\Theta_T],\bar E_0^2[\Theta_T])$,
更一般地，对$h\ge2$和$j\in\{1,\dots,h-1\}$，
$Cov(\bar E_0^h[\Theta_T],\bar E_0^1[\Theta_T])=Cov(\bar E_0^{h-j}[\Theta_T],\bar E_0^{1+j}[\Theta_T])$ (38)
由此对$k\ge1$，
$\beta_{2k}=\frac{Cov(\bar E_0^k[\Theta_T],E_{i,0}[\bar E_0^k[\Theta_T]])}{Var(\bar E_0^1[\Theta_T])}=\frac{Var(E_{i,0}[\bar E_0^k[\Theta_T]])}{Var(\bar E_0^1[\Theta_T])}\ge0$ (39)
$\beta_{2k-1}=\frac{Var(\bar E_0^k[\Theta_T])}{Var(\bar E_0^1[\Theta_T])}\ge0$ (40)
由总方差不等式$Var(\mathbb E[X|I])\le Var(X)$，得$\beta_{2k+1}\le\beta_{2k}\le\beta_{2k-1}$。
故$\beta_h\in[0,1]$且弱递减。
严格性：若$\beta_2=1$，则$Var(E_{i,0}[\bar E_0[\Theta_T]])=Var(\bar E_0[\Theta_T])$对所有$i$，结合对称性与迭代期望推出$E_{i,0}[\Theta_T]=\bar E_0[\Theta_T]$几乎必然对所有$i$，与信息不完全矛盾，故$\beta_2<\beta_1=1$。若$\beta_h$在某处不严格递减，取最小$h^*>1$使$\beta_{h^*+1}=\beta_{h^*}$。分$h^*=2k$与$h^*=2k-1$两种情形，利用(39)(40)及上述等号条件，均推出$\beta_{h^*-1}=\beta_{h^*}$或$\beta_{h^*}=\beta_{h^*+1}$的矛盾。故严格递减。又若某$\beta_h=0$，由严格递减则$\beta_{h+1}<0$，矛盾；故$\beta_h>0$。

**※【定理】** Theorem 2：
令$s_{h,T}=\sum_{r=1}^h\chi_{r,T}$。相对贡献$s_{h,T}/s_{T,T}$随$T$严格递减，且$T\to\infty$时趋于0。

**证明（Appendix A）：** 
定义$d_\tau=s_{\tau,\tau}$。由(43)得 $s_{h,\tau}=\gamma^{\tau-1}(\gamma+\alpha)+\sum_{l=1}^{\tau-1}\alpha\gamma^{l-1}s_{h-1,\tau-l}$ (43)
$d_\tau=\gamma^{\tau-1}(\gamma+\alpha)+\sum_{l=1}^{\tau-1}\alpha\gamma^{l-1}d_{\tau-l}$ (44)
归纳得$d_\tau=(\gamma+\alpha)^\tau$ (45)。

对$h\ge1$，由(43)：
$s_{h,\tau+1}=\gamma^\tau(\gamma+\alpha)+\alpha s_{h-1,\tau}+\sum_{l=1}^{\tau-1}\alpha\gamma^l s_{h-1,\tau-l}=\gamma s_{h,\tau}+\alpha s_{h-1,\tau}<(\gamma+\alpha)s_{h,\tau}$ (46)
又$s_{\tau+1,\tau+1}=d_{\tau+1}=(\gamma+\alpha)d_\tau=(\gamma+\alpha)s_{\tau,\tau}$。

故 $s_{h,\tau+1}/s_{\tau+1,\tau+1}<s_{h,\tau}/s_{\tau,\tau}$
对$\tau\ge h$严格递减。极限：$h=1$时$s_{1,\tau}=\gamma^{\tau-1}(\gamma+\alpha)$，$d_\tau=(\gamma+\alpha)^\tau$，故$s_{1,\tau}/d_\tau=\gamma^{\tau-1}/(\gamma+\alpha)^{\tau-1}\to0$。

假设存在最小$h^*>1$使极限不为0，则$s_{h^*,\tau}/d_\tau\to\Gamma>0$。由(45)(46)取极限得$\Gamma=\frac{\gamma}{\gamma+\alpha}\Gamma+\frac{\alpha}{\gamma+\alpha}\cdot0$，故$\Gamma=0$，矛盾。故所有$h$极限为0。

**※【推论】** Corollary 1：
固定$(\alpha,\gamma,T)$，不完全信息经济对未来的预期反应更小，即使其个体对$\Theta_T$的一阶信息比完全信息经济更精确。

**证明：** 直接由Theorem 1 part (ii)：$\phi_T<\phi_T^*$，而$\phi_T^*$只由$(\alpha,\gamma,T)$决定，与一阶信息精度无关。

### 3.5 极限结果

**※【命题】** Proposition 4（Limit）：
若$\lim_{h\to\infty}Var(\bar E^h[\Theta_T])=0$，则 $\lim_{T\to\infty}\phi_T/\phi_T^*=0$。

**证明（Appendix A）：** 
由Theorem 1，$\phi_T/\phi_T^*$严格递减且位于$(0,1)$，故收敛到某$\varphi\in[0,1)$。
由Prop3，$\beta_h$严格递减有界，故$\beta_h\to\beta\in[0,1)$。
先证$\varphi=\lim_{h\to\infty}\beta_h$：对任意$\vartheta>0$，存在$h^*$使$|\beta_h-\beta|<\vartheta/2$对所有$h\ge h^*$；
由Theorem 2，存在$T^*$使对$T\ge T^*$，$\sum_{h=h^*}^T\chi_{h,T}/s_{T,T}\le\vartheta/2$。
则 $|\phi_T/\phi_T^*-\beta|=|\sum_{h=1}^T\chi_{h,T}(\beta_h-\beta)/s_{T,T}|\le\sum_{h=1}^{h^*-1}\chi_{h,T}/s_{T,T}+\sum_{h=h^*}^T\chi_{h,T}\vartheta/(2s_{T,T})\le\vartheta$
故$\varphi=\beta$。若$\lim_{h\to\infty}Var(\bar E^h[\Theta_T])=0$，由(40)得$\beta_{2k-1}=Var(\bar E_0^k[\Theta_T])/Var(\bar E_0^1[\Theta_T])\to0$；由单调性$\beta_h\to0$。故$\varphi=0$。

### 3.6 作为“总体层面额外贴现”的myopia

**※【命题】** Proposition 5（Discounting）：
设$\Theta_T=z+\eta$，$z\sim N(0,\sigma_z^2)$，$\eta\sim N(0,\sigma_\eta^2)$，信号$\omega_i=z+\nu_i$，$\nu_i\sim N(0,\sigma_\nu^2)$。
则 $\bar E_0^h[\Theta_T]=\lambda\bar E_0^{h-1}[\Theta_T]$, $\lambda=\frac{\sigma_\nu^{-2}}{\sigma_\nu^{-2}+\sigma_z^{-2}}\in(0,1)$
不完全信息的$\phi_T$等于一个代表性代理经济中的$\phi_T^*$，但把$\delta=\gamma+\alpha$换成$\delta'=\gamma+\alpha\lambda$。

**证明（Appendix A）：** 在该信息结构下，对任意$h\in\{1,\dots,T\}$和$0\le t_1<\cdots<t_h<T$，
$\bar E_{t_1}[\bar E_{t_2}[\cdots[\bar E_{t_h}[\Theta_T]]\cdots]]=\lambda^hz$ (48)
归纳证明：对$t\le T-1$，
$a_t=(\gamma+\alpha)\{\prod_{\tau=t+1}^{T-1}(\gamma+\lambda\alpha)\}\bar E_t[\Theta_T]$ (49)
$t=T-1$时由(16)得$a_{T-1}=(\gamma+\alpha)\bar E_{T-1}[\Theta_T]$，成立。设对$t+1$成立，代入(16)，用(48)得
$a_t=\gamma^{T-t}\bar E_t[\Theta_T]+\alpha\bar E_t[a_{t+1}]+\alpha\sum_{k=2}^{T-t}\gamma^{k-1}\bar E_t[a_{t+k}]$
$\gamma\bar E_t[a_{t+1}]=\lambda\gamma^{T-t}\bar E_t[\Theta_T]+\lambda\alpha\sum_{k=2}^{T-t}\gamma^{k-1}\bar E_t[a_{t+k}]$
故$a_t=(\gamma/\lambda+\alpha)\bar E_t[a_{t+1}]$，结合$t+1$的表达式得(49)。令$t=0$：$\phi_T=(\gamma+\alpha)(\gamma+\lambda\alpha)^{T-1}$。这等价于完全信息递归中把$\alpha$换成$\lambda\alpha$，即$\delta'=\gamma+\alpha\lambda$。

**※【推论】** Corollary 2：
$\delta-\delta'=\alpha(1-\lambda)$ 随$\alpha$增大；GE越强，衰减和“as-if myopia”越大。

**证明：** 直接求导：$\partial(\delta-\delta')/\partial\alpha=1-\lambda>0$。

**※【例题】** 取$\gamma=\alpha=0.5$，$T=2$。完全信息$\phi_2^*=(\gamma+\alpha)^2=1$。不完全信息下$\chi_{1,2}=0.5$，$\chi_{2,2}=0.5$；若$\beta_2=0.5$，则$\phi_2=0.5+0.5\times0.5=0.75$，比率0.75。若$T$增大，高阶权重更大且$\beta_h$更小，比率进一步下降。

### 3.7 允许学习

Assumption 1可放松。
无学习时“跨期高阶信念”坍缩为“同期高阶信念”，便于Lemma 2。
允许学习后分析复杂，但两类学习可恢复定理：粘性信息（Mankiw-Reis）与每期新私人信号（Woodford, Nimark）。
直觉：预期未来会有更多信息会缓解当前摩擦；随时间推移高阶信念收敛于一阶信念，衰减随新闻到达后的滞后衰减。这与Coibion-Gorodnichenko (2012)的证据一致。

---

## 4. 重新审视前向指引

### 4.1 仅需求块

若实际利率路径外生或价格完全刚性$\theta=1$，则直接由第3节得：不完全信息降低对未来的反应，期限越长越强。

### 4.2 包含通胀-支出反馈

**※【引理】** Lemma 3：
若厂商有完全信息，则均衡产出满足
$y_t=-\sigma R_t-\sigma\sum_{k=1}^{\infty}\beta^k\bar E_t^c[R_{t+k}]+\sum_{k=1}^{\infty}(1-\beta+k\sigma\kappa)\beta^{k-1}\bar E_t^c[y_{t+k}]$ (22)
其中$\kappa\equiv(\epsilon+1/\sigma)(1-\theta)(1-\beta\theta)/\theta$。

**证明（Appendix A）：** 厂商完全信息下标准NKPC(10)成立。将(10)代入消费选美(5)，并用未来加成冲击不可预测，得(22)。这是消费者之间的选美比赛，但已把通胀-支出反馈纳入。

### 4.3 具体政策假设

**※【假设】** Assumption 2（Monetary Policy）：
存在已知 $T\ge2$：  
(i) $t<T$时名义利率钉住0；  
(ii) $t>T$时政策复制灵活价格结果；  
(iii) $R_T=z+\eta$ (23)，$z\sim N(0,\sigma_z^2)$在$t=0$实现，$\eta\sim N(0,\sigma_\eta^2)$在$T$实现且之前不可预测。  
$z$可解释为前向指引新闻，$\eta$为残余不确定性。

**※【假设】** Assumption 3（Information）：  
(i) $t=0$时每个消费者/厂商观察到$\omega_i=z+\nu_i$，$\nu_i$独立正态，方差$\sigma_c^2$或$\sigma_f^2$；  
(ii) $T$之前无其他外生信息；  
(iii) 加成与异质冲击方差相对$z$任意大，以关闭内生学习。  
定义$\lambda_c=\frac{\sigma_c^{-2}}{\sigma_c^{-2}+\sigma_z^{-2}}$，$\lambda_f=\frac{\sigma_f^{-2}}{\sigma_f^{-2}+\sigma_z^{-2}}$；完全信息为$\lambda_c=\lambda_f=1$，摩擦越大$\lambda$越小。

### 4.4 完全信息基准与谜题

**※【命题】** Proposition 6（Forward Guidance with CK）：
若$\lambda_c=\lambda_f=1$，存在$\phi_T^*>\sigma$使 $y_0=y_0^{trap}-\phi_T^*E_0[R_T]$ (24)
且$\phi_T^*$随$T$严格递增，$T\to\infty$时$\phi_T^*\to\infty$。这就是标准NK的前向指引谜题。

**证明（Appendix A）：** 先证更强结论：
存在正标量$\{\phi_\tau^*,\pi_\tau^*\}_{\tau\ge0}$，当Assumption 2成立且$z$为共同知识时，对任意$t\le T$，
$\tilde y_t-\tilde y_t^{trap}=-\phi_{T-t}^*E_t[\tilde R_T]$ (52)
$\tilde\pi_t-\tilde\pi_t^{trap}=\kappa(\tilde y_t-\tilde y_t^{trap})-\varpi_{T-t}^*E_t[\tilde R_T]$ (53) 归纳。
$t=T$时，$\tilde y_T-\tilde y_T^{trap}=-\sigma\tilde R_T$，$\tilde\pi_T-\tilde\pi_T^{trap}=\kappa(\tilde y_T-\tilde y_T^{trap})$，故$\phi_0^*=\sigma$，$\pi_0^*=0$ (54)。
设对$t$成立，由迭代期望，
$E_{t-1}[\tilde y_t-\tilde y_t^{trap}]=-\phi_{T-t}^*E_{t-1}[\tilde R_T]$
$E_{t-1}[\tilde\pi_t-\tilde\pi_t^{trap}]=-(\kappa\phi_{T-t}^*+\pi_{T-t}^*)E_{t-1}[\tilde R_T]$
代入线性化欧拉与NKPC，得
$\phi_{T-t+1}^*=(1+\sigma\kappa)\phi_{T-t}^*+\sigma\pi_{T-t}^*$
$\pi_{T-t+1}^*=\beta\kappa\phi_{T-t}^*+\beta\pi_{T-t}^*$ (56)
即(57)。因$\kappa>0$，$\pi_\tau^*>0$对$\tau\ge1$，故$\phi_\tau^*>\sigma$且严格递增；$1+\sigma\kappa>1$导致$\phi_\tau^*\to\infty$。

直观：降$R_T$使$T$期支出与通胀上升；$T-1$到$T$实际利率下降；$T-1$需求上升；通胀上升；反馈环随$T$累积。

### 4.5 不完全信息：衰减与期限效应

**※【命题】** Proposition 7（Forward Guidance without CK）：
若$\lambda_c<1$和/或$\lambda_f<1$，存在$\phi_T$使 $y_0=y_0^{trap}-\phi_T\bar E_0[R_T]$ (25)
且：  
(i) $\sigma\beta^T<\phi_T<\phi_T^*$；  
(ii) $\phi_T$随$\lambda_c,\lambda_f$严格递增；$\phi_T/\phi_T^*$随$T$严格递减，$T\to\infty$时趋于0；若$\lambda_c$足够低，$\phi_T$本身也趋于0。

**证明（Appendix A）：** 
定义$\phi(\lambda_c,\lambda_f,\tau)$与$\pi(\lambda_c,\lambda_f,\tau)$递归。
由(66)初值$\phi(\lambda_c,\lambda_f,1)=\sigma(1+\sigma\kappa)$，$\pi(\lambda_c,\lambda_f,1)=\sigma\kappa\beta$（或相应形式）。
对$\tau\ge2$，
$\phi_\tau=(\beta+(1-\beta+\sigma\kappa)\lambda_c)\phi_{\tau-1}+\sigma\lambda_f\pi_{\tau-1}$ (71)
$\pi_\tau=\kappa\beta\lambda_c\phi_{\tau-1}+\beta[\theta+(1-\theta)\lambda_f]\pi_{\tau-1}$ (72)
先证$\phi_\tau$对$\lambda_c,\lambda_f$严格递增：
$\tau=2$由初值直接；归纳用(71)(72)及$\phi_{\tau-1},\pi_{\tau-1}>0$。
故$\phi_\tau<\phi_\tau^*=\phi(1,1,\tau)$。下界：由(68)得$\phi_\tau>\sigma\beta^\tau$。  
再证比率递减。对$\tau\ge3$，由(71)(72)可证
$\phi_\tau/\phi_{\tau-1}+\beta\phi_{\tau-2}/\phi_{\tau-1}\le1+\beta+\sigma\kappa\lambda_c\le1+\beta+\sigma\kappa$ (74)
而完全信息递归满足
$\phi_\tau^*/\phi_{\tau-1}^*+\beta\phi_{\tau-2}^*/\phi_{\tau-1}^*=1+\beta+\sigma\kappa$ (73)
结合$\phi_2/\phi_2^*<1=\phi_1/\phi_1^*$，归纳得$\phi_{\tau+1}/\phi_{\tau+1}^*<\phi_\tau/\phi_\tau^*$。  
极限：比率递减有下界，设极限$\Gamma\in[0,1)$。若$\Gamma>0$，则$\phi_\tau/\phi_{\tau-1}\to\Gamma^*$，$\phi_{\tau-2}/\phi_{\tau-1}\to1/\Gamma^*$；由(73)(74)取极限，当$\lambda_c<1$时矛盾，故$\Gamma=0$。若$\lambda_c=1$但$\lambda_f<1$，用更强不等式
$\phi_\tau/\phi_{\tau-1}+(1+\sigma\kappa(1-\lambda_f))\beta\phi_{\tau-2}/\phi_{\tau-1}\le1+\beta+\sigma\kappa$ (76)
同样得$\Gamma=0$。若$\lambda_c$足够低，递归系统特征值均小于1（$\lim_{\lambda_c\to0}m_2=\beta<1$），故$\phi_\tau\to0$。

**※【例题】** 数值例子：取季度模型，$\beta=0.99$，Frisch弹性1，跨期替代弹性1，调价率$1/3$，$\lambda_c=\lambda_f=0.75$。解释为每个听到公告的人认为别人有25%概率没听到/不信。图1显示$T=20$（五年）时$\phi_T/\phi_T^*\approx0.1$；且该衰减已按一阶预期变动幅度归一化，不是机械噪声效应。

### 4.6 折扣欧拉方程与折扣NKPC

**※【命题】** Proposition 10：
若$\lambda_c=\lambda_f=\lambda$，不完全信息的$\phi_T$等价于一个代表性代理经济，其中
$y_t=-\sigma\{R_t-\lambda E_t[\pi_{t+1}]\}+M_cE_t[y_{t+1}]$ (106)
$\pi_t=\kappa'y_t+\beta M_fE_t[\pi_{t+1}]+\kappa\mu_t$ (107)
$M_c=\beta+(1-\beta)\lambda$，$M_f=\theta+(1-\theta)\lambda$，$\kappa'=\kappa\lambda$。

**证明（Appendix C）：** 
先看不完全信息$\phi_T$。当厂商缺乏对同期边际成本的共同知识时，通胀选美变为
$\tilde\pi_t-\tilde\pi_t^{trap}=\kappa\sum_{k=0}^{T-t}(\beta\theta)^k\bar E_t^f[\tilde y_{t+k}-\tilde y_{t+k}^{trap}]+\frac{1-\theta}{\theta}\sum_{k=1}^{T-t}(\beta\theta)^k\bar E_t^f[\tilde\pi_{t+k}-\tilde\pi_{t+k}^{trap}]$
可找到$\phi,\omega$使
$\tilde y_t-\tilde y_t^{trap}=-\phi(\lambda,T-t)\bar E_t^f[\tilde R_T]$
$\tilde\pi_t-\tilde\pi_t^{trap}=-\omega(\lambda,T-t)\bar E_t^f[\tilde R_T]$ (108)
初值$\phi(\lambda,1)=\sigma(1+\sigma\lambda\kappa)$，$\omega(\lambda,1)=\kappa\lambda\sigma(1+\sigma\lambda\kappa)+\kappa\beta(\theta+(1-\theta)\lambda)\sigma$，且
$\phi(\lambda,T-t)=(\beta+(1-\beta)\lambda)\phi(\lambda,T-t-1)+\sigma\lambda\omega(\lambda,T-t-1)$
$\omega(\lambda,T-t)=\beta(\theta+(1-\theta)\lambda)\omega(\lambda,T-t-1)+\kappa\lambda\phi(\lambda,T-t)$ (110)
另一方面，在折扣代表性代理经济(106)(107)中，完全信息$\phi_T^*,\omega_T^*$满足同样递归：
$\phi_{T-t}^*=(\beta+(1-\beta)\lambda)\phi_{T-t-1}^*+\sigma\lambda\omega_{T-t-1}^*$
$\omega_{T-t}^*=\beta(\theta+(1-\theta)\lambda)\omega_{T-t-1}^*+\kappa\lambda\phi_{T-t}^*$ (113)
两系统初值相同，故$\phi=\phi^*$。即不完全信息的$\phi_T$等于折扣经济的完全信息$\phi_T^*$。

### 4.7 价格灵活性与“flexibility paradox”

**※【命题】** Proposition 11（Price Flexibility）：
令$\phi_T$为Prop7或Prop8的斜率，设$\lambda_f=1$。
则$\partial\phi_T/\partial\kappa>0$，且$\frac{\partial}{\partial\kappa}(\frac{\partial\phi_T}{\partial\kappa})>0$（原文表述：flexibility提高会提高前向指引/财政乘数，但共同知识越远，提高速度越慢；即高阶不确定性减弱“flexibility paradox”）。

**证明（Appendix C）：** 先看第5节环境。由(66)得$\partial\phi_1/\partial\kappa=\sigma^2>0$，$\partial\varpi_1/\partial\kappa=\sigma\beta>0$ (114)。$\lambda_f=1$时递归(71)(72)变为
$\phi_\tau=(\beta+(1-\beta+\sigma\kappa)\lambda_c)\phi_{\tau-1}+\sigma\varpi_{\tau-1}$
$\varpi_\tau=\kappa\beta\lambda_c\phi_{\tau-1}+\beta\varpi_{\tau-1}$
对$\kappa$求导：
$\partial\phi_\tau/\partial\kappa=(\beta+(1-\beta+\sigma\kappa)\lambda_c)\partial\phi_{\tau-1}/\partial\kappa+\sigma\lambda_c\phi_{\tau-1}+\sigma\partial\varpi_{\tau-1}/\partial\kappa$ (115)
$\partial\varpi_\tau/\partial\kappa=\kappa\beta\lambda_c\partial\phi_{\tau-1}/\partial\kappa+\beta\lambda_c\phi_{\tau-1}+\beta\partial\varpi_{\tau-1}/\partial\kappa$ (116)
归纳得$\partial\phi_\tau/\partial\kappa,\partial\varpi_\tau/\partial\kappa>0$。再对$\lambda_c$求交叉偏导：由(114)(115)(116)及$\phi_\tau$对$\lambda_c$递增，得$\partial^2\phi_\tau/\partial\kappa\partial\lambda_c>0$。财政情形类似，由Prop8递归得同样结论。

---

## 5. 财政乘数：为何前置刺激

引入政府支出$g_t$，边际成本变为
$mc_t=\Omega_c c_t+(1-\Omega_c)g_t$
政府支出通过边际成本进入通胀与消费动态。

**※【命题】** Proposition 8：Proposition 6和7对财政政策同样成立，只需把$\phi_T^*,\phi_T$重新解释为$y_0$对$\bar E_0[g_T]$的完全/不完全信息斜率。

**证明（Appendix A）：** 政府支出由一次性税融资，个体预算约束加入税收项。聚合消费仍满足消费选美(81)，形式同(5)。

厂商边际成本变为
$\tilde mc_t=\tilde w_t=\epsilon\tilde y_t+\frac1\sigma\tilde c_t=(\epsilon\Omega_3+\frac1\sigma)\tilde c_t+\epsilon(1-\Omega_3)\tilde g_t$ (79)
其中$\tilde y_t=\Omega_3\tilde c_t+(1-\Omega_3)\tilde g_t$，$\Omega_3$为稳态消费/产出比。故通胀选美为
$\tilde\pi_t=\kappa(\Omega_c\tilde c_t+(1-\Omega_c)\tilde g_t)+\kappa\sum_{k=1}^{\infty}(\beta\theta)^k\bar E_t^f[\Omega_c\tilde c_{t+k}+(1-\Omega_c)\tilde g_{t+k}]+\frac{1-\theta}{\theta}\sum_{k=1}^{\infty}(\beta\theta)^k\bar E_t^f[\tilde\pi_{t+k}]+\kappa\tilde\mu_t$ (80)
完全信息下，可找到$\{\phi_{g,\tau}^*,\pi_{g,\tau}^*\}$使
$\tilde c_t-\tilde c_t^{trap}=\phi_{g,T-t}^*E_t[\tilde g_T]$
$\tilde\pi_t-\tilde\pi_t^{trap}=\kappa(\Omega_c(\tilde c_t-\tilde c_t^{trap})+(1-\Omega_c)\tilde g_t)+\pi_{g,T-t}^*E_t[\tilde g_T]$ (84)
递归初值$\phi_{g,0}^*=0,\pi_{g,0}^*=0,\phi_{g,1}^*=\sigma\kappa(1-\Omega_c),\pi_{g,1}^*=\beta\kappa(1-\Omega_c)$，且
$\phi_{g,\tau+1}^*=(1+\sigma\kappa\Omega_c)\phi_{g,\tau}^*+\sigma\pi_{g,\tau}^*$
$\pi_{g,\tau+1}^*=\beta\kappa\Omega_c\phi_{g,\tau}^*+\beta\pi_{g,\tau}^*$ (89)
故$\phi_{g,\tau}^*>0$严格递增且发散到无穷。不完全信息下，类似Prop7得
$\phi_{g,\tau}=(\beta+(1-\beta+\sigma\kappa\Omega_c)\lambda_c)\phi_{g,\tau-1}+\sigma\lambda_f\varpi_{g,\tau-1}$
$\varpi_{g,\tau}=\kappa\beta\lambda_c\Omega_c\phi_{g,\tau-1}+\beta[\theta+(1-\theta)\lambda_f]\varpi_{g,\tau-1}$ (96)
同样证明$\phi_{g,\tau}\in(0,\phi_{g,\tau}^*)$、随$\lambda_c,\lambda_f$递增、比率随$T$递减并趋于0；$\lambda_c$足够低时$\phi_{g,\tau}\to0$。因此Prop6/7的结论全部成立。

标准NK：ZLB下给定规模财政刺激，后置（现在宣布、未来实施）更有效，因为可累积GE反馈。本文：不完全信息下，期限越长GE衰减越强；因此前置刺激减少对长期高阶信念协调的依赖，提供front-loading的理由。开放问题：与借贷约束、hand-to-mouth消费者的交互。

---

## 6. 理性疏忽与任意学习下的极限

**※【假设】** Assumption 4（Non-Vanishing Higher-Order Uncertainty）：
存在$\epsilon>0$，使  
(i) 对所有$t\in\{0,\dots,T-1\}$，至少质量$\epsilon$的agent满足$Var(E_t[x]|\omega_i^t)\ge\epsilon Var(E_t[x])$，对所有相关高阶信念$x$与$\tau$；  
(ii) $Var(\bar E_0[\Theta_T])\ge\epsilon$。

**※【命题】** Proposition 9（Limit）：
Assumption 4下，$T\to\infty$时$\phi_T/\phi_T^*\to0$，对任意学习形式成立。

**证明（Appendix B）：** 
先证对任意$t\le T-1$和$y=\bar E_t\bar E_{t_2}\cdots\bar E_{t_k}[\Theta_T]\in B_t^k$，
$Var(y)\le(1-\epsilon^2)^kVar(\Theta_T)$ (103)
令$x=\bar E_{t_2}\cdots\bar E_{t_k}[\Theta_T]$（$k\ge2$）或$x=\Theta_T$（$k=1$）。

由Assumption 4，至少质量$\epsilon$的agent满足$Var(E_t[x]|\omega_i^t)\ge\epsilon Var(E_t[x])$，故
$E[Var(E_t[x]|\omega_i^t)|\Omega_t]\ge\epsilon^2Var(E_t[x])$

由总方差公式$Var(E_t[x])=E[Var(E_t[x]|\omega_i^t)|\Omega_t]+Var(E[x|\omega_i^t])$，得$Var(y)=Var(\bar E_t[x])\le Var(E[x|\omega_i^t])\le(1-\epsilon^2)Var(E_t[x])\le(1-\epsilon^2)Var(x)$。迭代得(103)。  
由$\phi_T^*=s_{T,T}$及方差不等式，有
$(\phi_T/\phi_T^*)^2\le[\sum_{k=1}^T(\chi_{k,T}/s_{T,T})(1-\epsilon^2)^{k/2}]^2Var(\Theta_T)/Var(\bar E_0[\Theta_T])$ (105)
对任意$\theta>0$，取$h$使$(1-\epsilon^2)^{h/2}/(1-(1-\epsilon^2)^{1/2})\le\theta/2$；
由Theorem 2，取$T^*$使对$T\ge T^*$，$\sum_{k=1}^{h-1}\chi_{k,T}/s_{T,T}\le\theta/2$。
则对$T\ge\max\{T^*,h\}$，
$\sum_{k=1}^T(\chi_{k,T}/s_{T,T})(1-\epsilon^2)^{k/2}\le\sum_{k=1}^{h-1}\chi_{k,T}/s_{T,T}+\sum_{k=h}^T(1-\epsilon^2)^{k/2}\le\theta$
故该和趋于0。
结合$Var(\bar E_0[\Theta_T])\ge\epsilon$，得$\phi_T/\phi_T^*\to0$。

理性疏忽解释：约束
$\mathcal I(\omega_{i,t},\Theta_T|\omega_i^{t-1})\le\kappa^{RI}$ (100)
Gaussian先验+二次目标下最优信号为$\omega_{i,t}=\Theta_T+v_{i,t}$，噪声独立。可重解释为“costly contemplation”。两类学习（粘性信息、每期新私人信号）均得
$\phi_T=(\gamma+\alpha)\prod_{t=1}^{T-1}(\gamma+\lambda_t\alpha)$
恢复Theorem 1。

---

## 7. 结论

- 标准做法把共同知识硬编码进模型，最大化GE乘数。
- 高阶不确定性是现实摩擦：个体难以预测、理解政策新闻的宏观效应，也难以确信别人会如何反应。
- 结果：GE反馈被抑制；期限越长越严重；政策对长期新闻的掌控力下降。
- 政策上：前向指引效果减弱；财政刺激宜前置；沟通应同时面向Wall Street与Main Street，且不仅沟通政策工具路径，也应沟通意图中的产出/通胀路径。
- 更广泛：依赖长串前瞻性GE反馈的预测对共同知识放松很脆弱；在战略替代环境中GE衰减可能转为overreaction（companion paper）。
