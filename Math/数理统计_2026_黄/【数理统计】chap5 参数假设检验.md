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

## 5.3 假设检验与区间估计 

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