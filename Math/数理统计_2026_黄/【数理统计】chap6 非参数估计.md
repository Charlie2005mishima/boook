## 符号检验

$X\sim F(x)$为连续型随机变量，$X$的$p$分位数$t_p$满足$F(t_{p})=p$.
考察 $H_{0}:t_{p}\leq t_{0}\leftrightarrow H_{1}: t_{p}>t_{0}$ 
令$Y_{i}=\begin{cases}1 & Xi-t_{0}>0 \\ 0 & else\end{cases}$，记$\theta=P(Y_{i}=1)=P(X_{i}-t_{0}>0)$. 则$Y_{1},\dots,Y_{n}\sim B(1,\theta)$ 
容易得到假设检验问题等价于$H_{0}:\theta\leq1-p\leftrightarrow H_{1}:\theta>1-p$ 
则拒绝域$\left\{  \tilde{x}:\sum_{i=1}^ny_{i}\geq C^*  \right\}$ ，$C^*=\min\left\{  C:\sum_{i=C}^n {n\choose i}(1-p)^i p^{n-i}\leq\alpha,C\in \mathbb{N} \right\}$. 

## 秩和检验

**※【定义】** 秩
$x_{1},x_{2},\dots,x_{n}$威两两互不相交的实数，$x_{i}=x_{(R_{i})}$，$R_{i}$为$x_{i}$的秩

**※【定理】** 
设$X_{1},\dots,X_{n}$为来自连续型总体$X\sim F(x)$的简单随机样本，则秩统计量取$(1,2,\dots,n)$的任意置换的概率都是$1/n!$ 

$X\sim F(x),Y\sim G(y)$，独立
$X_{1},X_{2},\dots,X_{m};Y_{1},Y_{2},\dots,Y_{n}$分别来自这两个总体的独立样本
记$Y_{i}$在合样本中的秩为$R_{i}$，把$W=\sum_{i=1}^nR_{i}$作为检验统计量
$F(x)>G(x)\implies P(X>Y)<1/2\implies D=\{\tilde{x},\tilde{y}:W\geq c  \}$ 
$P(W=i)= \frac{t_{m,n}(i)}{{m+n}\choose{n}}$，$t_{m,n}(i)$为$1,2,\dots,m+n$中不重复地取出$n$个数，其和恰好为$i$的组合数
当$F(x)=G(x)$时，$W$的分布关于$n(m+n+1)/2$对称，即$W$与$n(n+m+1)-W$同分布

**※【定理】** 大样本
假设$F(x)=G(x)$，则$EW=\frac{n(n+m+1)}{2},Var\{ W \}=\frac{mn(m+n+1)}{12}$. 
当$m,n\to \infty$时，$W^*=\frac{W-n(m+n+1)/2}{\sqrt{ mn(m+n+1)/12 }} \stackrel{ D}{ \longrightarrow}N(0,1)$ 

## 成对数据的检验问题

数据结构：$X_{i}=\mu _{i}+\xi _{i},Y_{i}=\mu _{i}+\eta _{i}$
假设检验：$H_{0}:F(x)=G(x)\leftrightarrow H_{1}:F(x)\not=G(x)$ 
数据改造：$Z=X_{i}-Y_{i}=\xi _{i}-\eta _{i}$ 
符号检验法：令$N^+=\sum_{i=1}^nI\{ Z_{i}>0 \}\sim B(n,1/2)$，则$D=\{N^+\geq c或N^+\leq d  \}$ 
秩和检验法：$W^+=\sum_{i=1}^nV_{i}R_{i}$，其中$V_{i}=I\{ Z_{i}>0 \}$，$R_{i}$为$Z_{i}$在$(|Z_{1}|,\dots,|Z_{n}|)$中的秩
注意：$W^+,n(n+1)/2-W^+$同分布

**※【定理】** 大样本
假设$F(x)=G(x)$，则$EW^+=\frac{n(n+1)}{4},Var\{ W^+ \}=\frac{n(n+1)(2n+1)}{24}$.
当$n\to \infty$，$(W^+)^*=\frac{W^+-\frac{n(n+1)}{4}}{\sqrt{ n(n+1)(2n+1)/24 }} \stackrel{D}{ \longrightarrow}N(0,1)$ 

## $\chi^{2}$拟合优度检验
### 一、分类数据的$\chi^{2}$检验
#### 1. 总体可分成有限个类，总体理论分布不含未知数

设总体$X$可以分成$r$类：$A_{1},A_{2},\dots ,A_{r}$要检验的假设为$H_{0}:P(A_{i})=p_{i}$ 
$\chi^{2}=\sum_{i=1}^r \frac{(n_{i}-np _{i})^{2}}{np _{i}}$，拒绝域$D=\{ \chi^{2}>c \}.$ 

**※【定理】**  
$H_{0}$为真时，$\chi^{2} \stackrel{D}{ \longrightarrow}\chi^{2}(r-1),as\,n\to \infty$ 

#### 2. 总体可分成有限个类，总体理论分布含未知参数

设总体$X$可以分成$r$类：$A_{1},A_{2},\dots ,A_{r}$要检验的假设为$H_{0}:P(A_{i})=p_{i}$ 
其中各个$p _{i}= p _{i}(\theta_{1},\dots,\theta_{m})$依赖于$m$个未知参数
取极大似然估计$\hat{\theta}_{1},\dots,\hat{\theta}_{m}$，代入$\chi^{2}=\sum_{i=1}^r \frac{(n_{i}-n\hat{p} _{i})^{2}}{n\hat{p} _{i}} \stackrel{ D}{ \longrightarrow}\chi^{2}(r-m-1)$. 

### 二、分布拟合的$\chi^{2}$检验
#### 1. 离散型分布
把若干个$a_{i}$值并成一类，使其可能取值分成有限个类$B_{1},\dots,B_{r}$，并使样本观测值落在每一个$B_{i}$内的个数$n_{i}$都不小于5

#### 2. 连续性分布
把$F_{0}(x)$的取值范围分成$r$个区间，不妨设$-\infty=a_{0}<a_{1}<\dots<a_{r-1}<a_{r}=\infty$，
记$A_{1}=(a_{0},a_{1}),\dots$ 


## Kolmogorov 检验

 $X_1,\dots,X_n$ 来自总体 $F(x)$ 的 i.i.d. 样本. 经验分布函数定义为
$$
F_n(x)=\begin{cases}
0, & x<X_{(1)},\\[4pt]
\dfrac{i}{n}, & X_{(i)}\le x < X_{(i+1)},\quad i=1,\dots,n-1,\\[6pt]
1, & x\ge X_{(n)}.
\end{cases}
$$
由 Glivenko‑Cantelli 定理，$\sup_x |F_n(x)-F(x)|\xrightarrow{a.s.}0$. 

已知理论分布 $F_0(x)$（完全已知，不含未知参数），检验
$$
H_0: F(x)=F_0(x),\quad H_1: F(x)\ne F_0(x).
$$

取
$$
D_n = \sup_{x\in\mathbb R} |F_n(x)-F_0(x)|.
$$
直观上，若 $H_0$ 成立，$F_n$ 与 $F_0$ 应接近，$D_n$ 不应过大. 拒绝域为 $\{D_n > D_{n,\alpha}\}$，其中 $D_{n,\alpha}$ 为给定显著性水平 $\alpha$ 下的临界值. 

由于 $F_n$ 是阶梯函数且仅在样本点处跳跃，而 $F_0$ 单调非降，故 $|F_n-F_0|$ 的上确界必在某个样本点 $X_{(i)}$ 处取得。在 $X_{(i)}$ 处，左极限为 $(i-1)/n$，右极限为 $i/n$，因此
$$
D_n = \max_{1\le i\le n}\left\{\, \left|F_0(X_{(i)})-\frac{i-1}{n}\right|\vee\ \left|F_0(X_{(i)})-\frac{i}{n}\right| \,\right\}.
$$
实际计算时，可对每个次序统计量计算上述两个差值绝对值，取最大。

临界值的确定
- 小样本：查 Kolmogorov 检验临界值表，给出满足 $P(D_n > D_{n,\alpha}\mid H_0)=\alpha$ 的临界值。
- 大样本（$n\to\infty$）：利用 Kolmogorov 分布
$$
  \lim_{n\to\infty} P(\sqrt{n}D_n < \lambda) = K(\lambda)=
  \begin{cases}
  \sum_{j=-\infty}^{\infty} (-1)^j e^{-2j^2\lambda^2}, & \lambda>0,\\
  0, & \lambda\le 0.
  \end{cases}
$$ 
  故对于大样本，可令 $\sqrt{n}D_n$ 近似服从 $K(\lambda)$，从而反解临界值 $\lambda_\alpha$，即 $D_{n,\alpha}\approx \lambda_\alpha/\sqrt{n}$。

## Smirnov 检验

设有两个独立样本，分别来自总体 $F_1(x)$ 和 $F_2(x)$，样本量分别为 $n_1,n_2$，经验分布函数为 $F_{1n_1}(x)$ 和 $F_{2n_2}(x)$。检验
$$
H_0: F_1(x)=F_2(x),\quad H_1: F_1(x)\ne F_2(x).
$$

定义
$$
D_{n_1,n_2} = \sup_x |F_{1n_1}(x)-F_{2n_2}(x)|.
$$
拒绝域为 $\{D_{n_1,n_2} > C\}$，临界值由两样本的联合分布确定。

若想检验一个总体是否随机占优于另一个，例如
$$
H_0: F_1(x)\le F_2(x),\quad H_1: F_1(x)>F_2(x).
$$
则取单边统计量
$$
D_{n_1,n_2}^+ = \sup_x (F_{1n_1}(x)-F_{2n_2}(x)).
$$
拒绝域为 $\{D_{n_1,n_2}^+ > C\}$。

当 $H_0$ 成立且 $F_1=F_2$ 连续时，令 $n_1,n_2\to\infty$，有
$$
P(\sqrt{\frac{n_1 n_2}{n_1+n_2}}\,D_{n_1,n_2}^+\leq x)=\begin{cases}1-e^{-2x^2} & x>0 \\0 & x\leq0\end{cases} 
$$
且
$$
\sqrt{\frac{n_1 n_2}{n_1+n_2}}\,D_{n_1,n_2} \xrightarrow{D} K(x)\quad (x>0),
$$
其中 $K(x)$ 即 Kolmogorov 分布函数。据此可近似计算 p 值或临界值。

## 列联表独立性检验

将 $n$ 个个体按两个属性 $X_1$（分 $r$ 类：$A_1,\dots,A_r$）和 $X_2$（分 $s$ 类：$B_1,\dots,B_s$）进行交叉分类，得到 $r\times s$ 列联表，频数为 $n_{ij}$（$i=1,\dots,r;\,j=1,\dots,s$）。记
- 行合计：$n_{i\cdot}=\sum_{j=1}^s n_{ij}$，$i=1,\dots,r$；
- 列合计：$n_{\cdot j}=\sum_{i=1}^r n_{ij}$，$j=1,\dots,s$；
- 总频数 $n=\sum_i\sum_j n_{ij}$。

对应概率为 $p_{ij}=P(X_1=A_i,\, X_2=B_j)$，行边缘概率 $p_{i\cdot}=\sum_j p_{ij}$，列边缘概率 $p_{\cdot j}=\sum_i p_{ij}$。

$$
H_0: X_1 \text{ 与 } X_2 \text{ 独立}\quad\Longleftrightarrow\quad p_{ij}=p_{i\cdot}p_{\cdot j},\ \forall i,j,
$$
$$
H_1: \exists (i,j),\ p_{ij}\ne p_{i\cdot}p_{\cdot j}.
$$

### 检验统计量

#### （1）边缘概率已知
若已知 $p_{i\cdot}$ 和 $p_{\cdot j}$（少见），则理论频数为 $n p_{i\cdot}p_{\cdot j}$，统计量为
$$
\chi^2 = \sum_{i=1}^r\sum_{j=1}^s \frac{(n_{ij}-n p_{i\cdot}p_{\cdot j})^2}{n p_{i\cdot}p_{\cdot j}} \xrightarrow{D} \chi^2(rs-1).
$$
拒绝域 $\{\chi^2>\chi_\alpha^2(rs-1)\}$。

#### （2）边缘概率未知（最常见）
用 **极大似然估计** 估计边缘概率：
$$
\hat{p}_{i\cdot}=\frac{n_{i\cdot}}{n},\quad \hat{p}_{\cdot j}=\frac{n_{\cdot j}}{n}.
$$
于是理论频数估计为 $\hat{n}_{ij}= n\,\hat{p}_{i\cdot}\hat{p}_{\cdot j} = \dfrac{n_{i\cdot}n_{\cdot j}}{n}$。检验统计量为
$$
\chi^2 = \sum_{i=1}^r\sum_{j=1}^s \frac{(n_{ij}-\hat{n}_{ij})^2}{\hat{n}_{ij}}
= \sum_{i,j} \frac{n_{ij}^2}{\hat{n}_{ij}} - n.
$$
在 $H_0$ 下，当 $n$ 较大时，
$$
\chi^2 \xrightarrow{D} \chi^2\big((r-1)(s-1)\big).
$$
拒绝域 $\{\chi^2 > \chi_\alpha^2((r-1)(s-1))\}$。
