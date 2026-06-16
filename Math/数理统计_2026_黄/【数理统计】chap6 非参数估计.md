## 符号检验

$X\sim F(x)$威连续型随机变量，$X$的$p$分位数$t_p$满足$F(t_{p})=p$.
考察 $H_{0}:t_{p}\leq t_{0}\leftrightarrow H_{1}: t_{p}>t_{0}$ 
令$Y_{i}=\begin{cases}1 & Xi-t_{0}>0 \\ 0 & else\end{cases}$，记$\theta=P(Y_{i}=1)=P(X_{i}-t_{0}>0)$. 则$Y_{1},\dots,Y_{n}\sim B(1,\theta)$ 
容易得到假设检验问题等价于$H_{0}:\theta\leq1-p\leftrightarrow H_{1}:\theta>1-p$ 
则拒绝域$\left\{  \tilde{x}:\sum_{i=1}^ny_{i}\geq C^*  \right\}$ 

## 秩和检验

**※【定义】** 秩
$x_{1},x_{2},\dots,x_{n}$威两两互不相交的实数，$x_{i}=x_{(R_{i})}$，$R_{i}$为$x_{i}$的秩

**※【定理】** 
设$X_{1},\dots,X_{n}$为来自连续型总体$X\sim F(x)$的简单随机样本，则秩统计量取$(1,2,\dots,n)$的任意置换的概率都是$1/n!$ 

$X\sim F(x),Y\sim G(y)$，独立
$X_{1},X_{2},\dots,X_{m};Y_{1},Y_{2},\dots,Y_{n}$分别来自这两个总体的两个独立样本
记$Y_{i}$在合样本中的秩为$R_{i}$，把$W=\sum_{i=1}^nR_{i}$作为检验统计量
$F(x)>G(x)\implies P(X>Y)<1/2\implies D=\{\tilde{x},\tilde{y}:W\geq c  \}$ 
$P(W=i)= \frac{t_{m,n}(i)}{{m+n}\choose{n}}$，$t_{m,n}(i)$为$1,2,\dots,m+n$中不重复地取出$n$个数，其和恰好为$i$的组合种数
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

## 两样本置换检验
