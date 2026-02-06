```stata

help command 查看所有的命令

sysuse auto 导入数据auto.dta 
sysuse dir 查看stata自带的数据包
cd 'D:\Stata 13' 更改工作路径
log using p64.dta,text replace
use 1.dta, clear

merge m:m Stkcd year using 'file1.dta'

describe var1 具体内容
summarize var1 if q>=10000 初步统计
detail var1
list var1 var2 if missing(var1) 列出缺失的var1-var2
list tc q in 1/5 列出前五行的tc与q
list tc q if q>=100000 graph twoway 
tabulate pl 经验积累分布函数
duplicates tag Stkcd year, gen(dup_tag) 用来标记重复的小技巧

gen logprice = log(price)
gen n=_n 用n来代表第n个观测值
gen larg=(q>=10000) 生成虚拟变量
bysort industry: egen wage_p50 = median(wage) //计算行业工资水平中位数
rename larg large 改名字
drop large 删除
drop ln* 删除所有ln开头的
replace large=(q>=6000) 替换
real() 将字符转化为数字
label variable famfirm 给变量贴一个标签

scatter price weight 价格-重量散点图
scatter tc q, mlabel(n) mlabpos(6) 标签为n，在点的六点钟方向
twoway (scatter price weight) (lfit price weight) 散点图上再添线性回归
twoway (scatter price weight) (gfit price weight) 散点图上再添二次回归
histogram q,width(1000) frequency q的直方图，组宽1000，纵轴频数
kdensity q 连续的经验分布图（核密度图）
avplot lnq 画出lnq的新增变量散点图
avplots 画出全部变量的新增变量散点图
graph save scatter1 将图存为s1
graph combine scatter1.gph scatter2.gph

regress price weight 回归
regress price weight, nonconstant 回归但没有常系数
regress price weight if ~large 不大的回归
reg y x1 x2, level(99) 把显著性水平调到1%
reg y x1 x2, robust 采用稳健标准误报告结果
qui reg y x1 x2 静悄悄地进行一次回归
reg y x1 x2 x3 dum dum#c.x1 dum#c.x2 dum#c.x3 这是三个不用提前gen虚拟变量的因子方法
xi: reg y i.dum*x1 i.dum*x2 i.dum*x3
reg y dum##c.x1 dum##c.x2 dum##c.x3
stepwise, pr(0.1): reg lny lx1-lx6 逐步回归法：以10%为标准从高到低剔除解释变量
constraint def 1 lnpl+lnpk+lnpf=1 定义约束条件1
cnsreg lntc lnq lnpl lnpk lnpf, c(1) 在约束条件1下进行回归
chowreg y x1 x2 x3, dum(31) type(3)` Chow检验，type(3)同时通过加法和乘法引入虚拟变量

predict p_price 预测价格
predict e1, residual 预计残差并记作e1
estat ic 回归后计算信息准则IC统计量
estat vif 输出所有变量的VIF

display 1/_b[lnq] 进行运算，其中_b[lnq]是对lnq估计的系数
test lnq=1
test (lnq=1)(lnpl+lnpk+lnpf=1)
test lnpl lnpk 对这两系数的显著性进行Wald检验test dum dum_x1 dum_x2 dum_x3` 检验虚拟变量及其交互项
testn1 _b[lnpl]=_b[lnq]^2 非线性假设检验
pwcorr pl pf pk, sig star(.05) 相关系数显著性水平小于5%的打星

estat hettest 异方差检验
estat ovtest 模型设定误差检验
outreg2 using results.xls, replace 将回归结果输出到Excel文件
bootstrap, reps(1000): reg y x1 x2 自助法回归
xtset id year 设置面板数据
xtreg y x1 x2, fe 固定效应模型
areg y x1 x2, absorb(id) 高维固定效应
ivregress 2sls y x1 (x2=z1 z2) 工具变量法

log using p64.dta,text append
log close
```


```stata
reg lny lnx1-lnx6 // 用OLS估计上述模型
pwcorr lny lnx1-lnx6, sig star(.05) // 检验简单相关系数
estat vif // 方差膨胀检验VIF, 估计后统计量
collin lnx1-lnx6 // 多重共线性检验
stepwise, pr(0.1): reg lny lnx1-lnx6 // 逐步回归法，逐步剔除变量
stepwise, pe(0.1): reg lny lnx1-lnx6 // 逐步回归法，逐步增加变量
```

异方差检验 - BP 检验 $\mu_{i}^{2}=\beta_{0}+\beta_{1}X_{1}+\dots+\beta_{n}X_{n}+\varepsilon_{i}$
异方差检验 - White 检验 $\mu_{i}^{2}=\beta_{0}+\beta_{1}X_{1}+\dots+\beta_{n}X_{n}+\text{二阶交互}+\varepsilon_{i}$ 
异方差修正 - 加权回归假设 $\mu_{i}^{2}=\sigma^{2}\exp(\alpha_{0}+\alpha_{1}X_{1}+\dots+\alpha_{n}X_{n})$
异方差修正 - 稳健标准误
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
esttab ols wls ols_robust, scalar(r2 r2_a N F) mtitle(ols wls ols_robust) // 输出这几个回归的这几个值，改子标题
```

内生性检验 - Hausman内生性检验 $\upsilon_{i}=X-\delta_{1}Z_{1}-\delta_{2}Z_{2}$
内生性检验 - 过度识别约束 $\mu_{i}=\delta_{0}+\delta_{1}Z_{i1}+\delta_{2}Z_{i2}+\delta_{3}Z_{i}+\varepsilon_{i}$ 
内生性修正 - IV 工具变量法 
内生性修正 - 2SLS 二阶段 $\hat{X}=\beta_{0}+\beta_{1}Z_{1}+\beta_{2}Z_{2}+\beta_{3}Z$
内生性修正 - GMM（当随机扰动项存在异方差）
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

模型设定偏误 - RESET 检验 
模型设定偏误 - linktest 检验
```stata
qui reg lnq lny lnp
predict lnq_hat
gen lnq_hat2=lnq_hat^2
reg lnq lny lnp lnq_hat2
qui reg lnq lny lnp
linktest
estat ovtest  // Ramsey RESET检验，默认加入拟合值平方、立方、四次方项
```

序列相关检验 - 回归检验（这里未作展示）
序列相关检验 - DW 检验 $DW=\frac{\sum(e-e)^{2}}{\sum e^{2}}$
序列相关检验 - BGLM 检验 $\hat{e}_{t}=\beta_{0}+\beta_{1}X_{1}+\delta \hat{e}_{t-1}$
序列相关修正 - 广义最小二乘法（这里未作展示）
序列相关修正 - 广义差分法 - 科克伦奥克特迭代，普莱斯温斯特变换
序列相关修正 - Newey稳健标准误
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
qui reg y x 
estat bgodfrey // BGLM 检验，lag(1)
estat bgodfrey,lag(2) // BGLM 检验，lag(2)
estat bgodfrey，lag(2) small // BGLM 检验，lag(2)，小样本调整
// 采用广义差分法估计模型，stata没有二阶滞后项可以操作
prais y x
prais y x, corc // 剔除第一个样本
// 采用序列相关文件估计法，这个可以滞后两期
newey y x, lag(1)
```

平稳性检验 - Bartlett 检验
平稳性检验 - Q统计量检验
```stata
tsset time
corrgram random1,lags(17)
ac random1,lags(17)
wntestb random1 // Bartlett 定理，滞后期大致等于超出范围的数量
wntestq random1,lags(17) //
```

平稳性检验 - 单位根法
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

协整检验 - 双变量的Engle - Granger 检验
```stata
tsset year
gen lny = log(y)
gen lnx = log(x)
*** 检验 lnY 和 lnX 是否为 I(1)
dfuller lny, reg lag(0)
dfuller D.lny, reg lag(0)
*** 协整回归
reg lny lnx
predict e, residual
*** 检验残差平稳性（注意滞后阶数选择）
dfuller e, regress nocon lags(4)
```

协整变量回归 - 误差修正模型
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

因果检验 - 格兰杰因果检验： $\begin{cases}Y_t = \beta_0 + \sum_{i=1}^p \beta_i Y_{t-i} + \sum_{i=1}^p \alpha_i X_{t-i} + \mu_t \\ X_t = \delta_0 + \sum_{i=1}^p \delta_i Y_{t-i} + \sum_{i=1}^p \lambda_i X_{t-i} + \nu_t\end{cases}$ 
```stata
*** 生成增长率序列
gen gy = D.y / L.y
gen gx = D.x / L.x
dfuller gy, reg // 检验 gx,gy 是平稳的
dfuller gx, reg
*** 格兰杰因果关系检验（滞后1阶）
gcause gy gx, lags(1) // gx 是否是 gy 的原因
qui reg gy L(1).(gx gy) 
estat bgodfrey, lags(1)
estat ic
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