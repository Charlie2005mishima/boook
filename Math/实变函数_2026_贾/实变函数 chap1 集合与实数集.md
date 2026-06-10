## 绪言
1. 注重反例的积累：反例的用处不仅仅是能让你在考试的时候构造出对应的反例，更在于能让你更好的理解定理的内容和条件，例如记住为什么egoroff定理要求测度有限，以及为什么fatou引理是不等号（我在记忆 fatou 引理的不等号方向的时候，就是在脑海中过一遍反例）；
2. 实变里面函数的性质并不像复变里的函数那么好，会有一些很怪的函数，所以为了让定理成立，就必须要加条件
3. 整本书最重要的是第三章和第四章的内容，可测函数以及可测函数的积分。更加具体的，其实就是那几个最常见的定理，Egoroff, lusin, riesz, levi, fatou, DCT, tonelli-fubini。比起定理本身的证明，更重要的是会如何使用这些定理，比如在积分换序中 tonelli-fubini 一般都是搭配着用的，对于一个一般的被积函数，先用 tonelli 证明它绝对可积，然后就可以利用 fubini 进行换序操作。
4. 以及在[这个帖子](https://www.cc98.org/topic/5538286)中讲述了如何使用积分收敛定理（优先考虑单调收敛和控制收敛） 
5. 如果想要期末考取得一个更好的分数，建议在期末考前将课上和 Holder 不等式相关的例题和习题都自己写一遍，因为 Holder 的配凑需要一些技巧，而基本上每年期末考第六章都只是考 Holder
## chap1 集合与实数集
### 1.1 集合及其运算
**※【定义】集族**

**※【定理】分配律**
> $A\cap(\cup_{\alpha\in\Lambda}B_{\alpha})=\cup_{\alpha\in\Lambda}(A\cap B_{\alpha})$
> $A\cup(\cap_{\alpha\in\Lambda}B_{\alpha})=\cap_{\alpha\in\Lambda}(A\cup B_{\alpha})$

**※【定理】DeMorgan公式**
> $(\cup_{\lambda\in\Lambda}A_{\lambda})^C=\cap_{\lambda\in\Lambda}A_{\lambda}^C$
> $(\cap_{\lambda\in\Lambda}A_{\lambda})^C=\cup_{\lambda\in\Lambda}A_{\lambda}^C$ 

### 1.2 集合序列的极限
**※【定义】单调集列**

**※【定义】上下极限**
> $\overline\lim_{ n \to \infty }A_{n}=\cap_{i=1}^\infty \cup_{k=n}^\infty A_{k}=\limsup_{n\to \infty}A_{n}$ 出现无限次
> $\underline\lim_{ n \to \infty }A_{n}=\cup_{i=1}^\infty \cap_{k=n}^\infty A_{k}=\liminf_{n\to \infty}A_{n}$ 只有有限次不出现

**※【定理】上下极限的等价表示**
> (1) $x\in \overline\lim_{ n \to \infty }A_{n}\Leftrightarrow\forall\,N,\,\exists\,n\geq N,\,\text{ s.t. }x\in A_{n}$
> (2) $x\in \underline\lim_{ n \to \infty }A_{n}\Leftrightarrow\exists\,N_{x},\,\forall\,n\geq N_{x},\,\text{ s.t. }x\in A_{n}$
> (3) $\underline\lim_{ n \to \infty }A_{n}\subset\overline\lim_{ n \to \infty }A_{n}$

> 这一部分内容主要就是并交语言和任意存在语言的转化

### 1.3 映射
**※【定义】映射 满射、完全映射 单射、一一映射 双射、一一对应**
**※【定理】映射与逆映射的性质**
> (1) $\forall\,A\subset X,\,A\subset f^{-1}(f(A))$ 反例 $A={1},\,f(1)=f(2)=3,\,f^{-1}(3)=\{1,2\}$ 
> (2) $\forall\,B\subset Y,\,f(f^{-1}(B))\subset B$ 反例 当$B\not\subset f(X)$
> (3) 若$A_{1}\subset A_{2}$，则$f(A_{1})\subset f(A_{2})$
> (4) 若$B_{1}\subset B_{2}$，则$f^{-1}(B_{1})\subset f^{-1}(B_{2})$
> (5) $f(\cup_{\lambda \in\Lambda}A_{\lambda})=\cup_{\lambda \in\Lambda}f(A_{\lambda}),\,f(\cap_{\lambda \in\Lambda}A_{\lambda})\subset\cap_{\lambda \in\Lambda}f(A_{\lambda})$ 反例 $A_{1}=\{0\},A_{2}=\{ 1 \}$
> (6) $f^{-1}(\cup_{\lambda \in\Lambda}B_{\lambda})=\cup_{\lambda \in\Lambda}f^{-1}(B_{\lambda}),\,f^{-1}(\cap_{\lambda \in\Lambda}B_{\lambda})=\cap_{\lambda \in\Lambda}f^{-1}(B_{\lambda})$
> (7) $f^{-1}(B^C)=(f^{-1}(B))^C$ 

**※【定义】** 特征函数 $\mathscr{X}_{A}(x)=I\{ x\in A \}$

**※【命题】** 特征函数的性质
> (1) $A\subset B\Leftrightarrow \mathscr{X}_{A}(x)\leq\mathscr{X}_{B}(x),\,\forall\,x\in X$
> (2) $\mathscr{X}_{A\cup B}(x)=\mathscr{X}_{A}(x)+\mathscr{X}_{B}(x)-\mathscr{X}_{A\cap B}(x)$
> (3) $\mathscr{X}_{A\cap B}(x)=\mathscr{X}_{A}(x)\cdot\mathscr{X}_{B}(x)$
> (4) $\mathscr{X}_{A\backslash B}(x)=\mathscr{X}_{A}(x)\cdot(1-\mathscr{X}_{B}(x))$
> (5) $\mathscr{X}_{\cup_{\alpha\in \Lambda}A_{\alpha}}(x)=\sup_{\alpha} \mathscr{X}_{A_{\alpha}}(x),\,\mathscr{X}_{\cap_{\alpha\in \Lambda}A_{\alpha}}(x)=\inf_{\alpha} \mathscr{X}_{A_{\alpha}}(x)$

### 1.4 集合的等价、基数
**※【定义】** 对等/等价
> 集A, B. 若存在$A\to B$的双射，则称A, B等价，记$A\sim B$. 此时，A与B有相同的基数/势，记$\overline{\overline{A}}=\overline{\overline{B}}$

**※【定理】** 对等的性质
> (1) 自反性 (2) 对称性 (3) 传递性

**※【定理】** $A_{\alpha}\sim B_{\alpha}\implies \cup_{\alpha\in\Lambda}A_{\alpha}\sim\cup_{\alpha\in\Lambda}B_{\alpha}$
> 两族集合$\{ A_{\alpha} \}_{\alpha\in\Lambda}\{ B_{\alpha} \}_{\alpha\in\Lambda}$且当$\alpha_{1}\not=\alpha_{2}$，则$A_{\alpha_{1}}\cap A_{\alpha_{2}}=\emptyset,\,B_{\alpha_{1}}\cap B_{\alpha_{2}}=\emptyset$且$A_{\alpha}\sim B_{\alpha}\forall\,\alpha\in\lambda\implies \cup_{\alpha\in\Lambda}A_{\alpha}\sim\cup_{\alpha\in\Lambda}B_{\alpha}$

**※【定理】** Cantor-Bernstein定理
> 集X, Y的真子集 A, B. 满足 $X\sim B,\,Y\sim A$，则$X\sim Y$

**※【命题】** 
> 若$f:X\to Y,\,g:Y\to X$为单射，则$\exists\,A_{1},\,A_{2}\subset X,\,B_{1},B_{2}\subset Y;A_{1}\cap A_{2}=\emptyset,\,B_{1}\cap B_{2}=\emptyset;$ $A_{1}\cup A_{2}=X,B_{1}\cup B_{2}=Y;f(A_{1})=B_{1},g(B_{2})=A_{2}$

**※【推论】** 
> 集A, B, C. 满足 $A\subset B\subset C,\,A\sim C$则$A\sim B\sim C$

**※【定义】** 幂集 $\mathcal{P}(A)=\{ B:B\subset A \}$

**※【定理】** 无最大势
> $\overline{\overline{A}}\leq \overline{\overline{\mathcal{P(A)}}}$

**※【定义】** 可数集 $A\sim \mathbb{N},\,\overline{\overline{\mathbb{N}}}=\aleph_{0}$

**※【定理】** 
> (1) 任意无限集必含可数子集
> (2) 可数集的任意无限子集为可数集
> (3) 至多可数个可数集的并是可数集

**※【定理】** 设$n\geq2$，则n元数列全体有连续统集

**※【定理】** 可数集的子集的全体有连续统势

### 1.5 Rn中的拓扑
#### 1 领域
$B(x_{0},r)={x\in \mathbb{R}:d(x,x_{0})<r}$
**※【定理】** $B(x_{0},r)$是其每一点的领域
**※【定理】** $x\not=y$，则存在$B(x)\cap B(y)=\emptyset$
#### 2 Rn中点的分类
**※【定义】** 内点 外点 边界点
> 设$x_{0}\in \mathbb{R}^n,E\subset \mathbb{R}^n$
> (1) 若存在r>0，s.t. $B(x_{0},r)\subset E$，则称$x_{0}$为E的内点，称E的全体内点为$E^\circ$
> (2) 若存在r>0，s.t. $B(x_{0},r)\subset E^c$，则称$x_{0}$为E的外点，称E的全体外点为$(E^c)^\circ$
> (3) 任意r>0，s.t. $B(x_{0},r)\cap E\not=\emptyset,\,B(x_{0},r)\cap E^c\not=\emptyset$，则称$x_{0}$为E的边界点，称E全体边界点为$\partial E$ 

**※【定义】** 聚点 导集 闭包 孤立点 完备集
> 聚点：$x_{0}\in \mathbb{R}^n,E\subset \mathbb{R}^n$ 若$\forall\,r>0$，有$B(x_{0},r)\cap(E\setminus \{ x_{0} \})\not=\emptyset$ 
> 导集：聚点全体记作E'称作导集
> 闭包：$E\cup E'=\overline{E}$为闭包 
> 孤立点：若存在$r>0$，有$B(x_{0},r)\cap(E\setminus \{ x_{0} \})=\emptyset$，则$x_{0}$为孤立点
> 完备集：$E'=E$为完备集

**※【定义】** 开集 闭集
> $E\subset \mathbb{R}^n$
> (1) 若任意$x\in E$，存在$r>0$，$B(x,r)\subset E$，则称E是开集 $\Leftrightarrow E=E^\circ$ 
> (2) 若$E'\subset E$，则称E为闭集
> 注：约定空集既是开集又是闭集

**※【命题】** 
> $E\subset \mathbb{R}^n,\,x_{0}\in \mathbb{R}^n$
> (1) $x_{0}\in E'\Leftrightarrow \exists\,\{ p_{k} \}\subset E\setminus \{ x_{0} \},p_{k}\to x_{0}$ 
> (2) $x_{0}\in\overline{E}\Leftrightarrow\forall\,r>0,B(x_{0},r)\cap E\not=\emptyset$

**※【命题】** 单调性
> 若$A\subset B\subset \mathbb{R}^n$，则$A^\circ\subset B^\circ\,A'\subset B'\,\overline{A}\subset\overline{B}$

**※【命题】** 
> (1) $(A\cup B)^\prime=A'\cup B'\quad\overline{A\cup B}=\overline{A}\cup \overline{B}\quad(A\cup B)^{\circ}\supset A^{\circ}\cup B^{\circ}$
> (2) $(A\cap B)^{\circ}=A^{\circ}\cap B^{\circ}\quad(A\cap B)'\subset A'\cap B'\quad \overline{A\cap B}\subset \overline{A}\cap \overline{B}$ 

**※【定理】** $E^\circ$是开集 $E'$是闭集 $\bar{E}$是闭集

**※【定理】** 若G是开集，则$G^c$是闭集；若G是闭集，则$G^c$是开集

**※【引理】** $(E^\circ)^c=\overline{E^c},\,(E^c)^\circ=(\overline{E})^c$ 

**※【定理】** 
> 1. 任意个闭集的交仍闭，有限个闭集的并仍闭
> 2. 任意个开集的并仍开，有限个开集的交仍开

**※【定义】** $F_{\sigma},\,G_{\delta}$
> 若$E=\cup_{k=1}^\infty F_{k}$，$F_{k}$是闭集，则E是$F_{\sigma}$集
> 若$E=\cap_{k=1}^\infty G_k$，$G_{k}$是开集，则E是$G_{\delta}$集
> > Ferme comme(闭集的可数并集)
> > Gebiet Durchschnitt(开集的可数交集)

**※【定义】** 疏集 稠集
> 疏集：任何非空开集必有非空开子集与E不相交
> 稠集：任何开集与E有非空交

**※【定理】** 疏稠的充要
> (1) E疏 $\Leftrightarrow$ $(\overline{E})^{\circ}=\emptyset$
> (2) E稠 $\Leftrightarrow$ $\overline{E}=\mathbb{R}^n$

#### 3 开集的结构
##### R中开集的结构
**※【定理】** R中开集的结构
> $\forall\,G\in \mathbb{R}$为一非空开集，则G是至多可数个两两不相交的开区间的并且开区间的端点不属于G，即$G=\cup_{k=1}^\infty I_{k},\,I_{k}=(\alpha _{k} ,\beta _{k})$且$\{ \alpha _{k},\beta _{k} \}\not\subset G$

**※【定义】** Cantor三分集
> $\begin{align}C&=[0,1]-G \\ &=[0,1]-\cup \{ I_{n,k}:1\leq k\leq2^{n-1},n\geq1 \} \\ &=\cap_{n=1}^\infty \cup_{k=1}^{2^n}F_{n,k}\end{align}$ 称为Cantor三分集

**※【定理】** Cantor集C的性质
> (1) $C\not=\emptyset$且为有界闭集
> (2) $C=C'$
> (3) Cantor集是疏集即$(C')^{\circ}=(C)^{\circ}=\emptyset$
> (4) Cantor集有连续统势

##### Rn中开集的结构
**※【定理】** $\mathbb{R}^n$中开集的结构
> 对$\mathbb{R}^{n}$中任意非空开集一定可以表示为可数个两两不相交的半开方体的并

##### Rn上的连续函数、点与集的距离
**※【定义】** 集合上的连续函数
> 设$E\subset \mathbb{R}^n$. $f:E\to \mathbb{R}$定义在E上，$x_{0}\in E$
> (1) f在$x_{0}$处沿E连续：$\lim_{ x \to x_{0} }f(x)=f(x_{0})$ 
> $\Leftrightarrow \forall\,\varepsilon>0,\exists\,\delta>0,\forall\,x\in E$且$d(x,x_{0})<\delta$，有$|f(x)-f(x_{0})|<\varepsilon$
> $\Leftrightarrow\forall\,\varepsilon>0,\exists\,\delta\geq0,\forall\,x\in B(x_{0},\delta)\cap E$，有$f(B(x_{0},\delta)\cap E)\subset B(f(x_{0}),\varepsilon)$ 
> (2) 若$x_{0}$时孤立点，则f在$x_{0}$处一定连续
> (3) $C(E)$连续函数的全体
> (4) 一致连续性：$\forall\,\varepsilon>0,\exists\,\delta>0,\forall\,x_{1},x_{2}\in E$且$d(x_{1},x_{2})<\delta$时，恒有$|f(x_{1})-f(x_{2})|<\varepsilon$

**※【定义】** 相对开集，相对闭集
> 设$A\subset B\subset \mathbb{R}^{n}$中
> (1) 若$\exists\,G\subset \mathbb{R}^{n}$是开集，使$A=B\cap G$是A相对于B是开集
> (2) 若$\exists\,G\subset \mathbb{R}^{n}$是闭集，使$A=B\cap G$是A相对于B是闭集

**※【定理】** 连续函数的等价刻画
> $f:E\to \mathbb{R},E\subset \mathbb{R}^n$ 则下述命题等价
> (1) $f\in C(E)$
> (2) $\forall\,\alpha\in \mathbb{R}$ $E(f<\alpha)=\{ x\in E:f(x)<\alpha \},\,E(f>\alpha)=\{ x\in E:f(x)>\alpha \}$均是相对于E的开集
> (3) $\forall\,\alpha\in \mathbb{R}$ $E(f\leq\alpha)=\{ x\in E:f(x)\leq\alpha \},\,E(f\geq\alpha)=\{ x\in E:f(x)\geq\alpha \}$均是相对于E的闭集

**※【推论】** 
> $f:E\to \mathbb{R}$
> (1) $f\in C(E)$
> (2) 任意开集$G\subset \mathbb{R}$是开集，则$f^{-1}(G)$相对于E是开集
> (3) 任意闭集$G\subset \mathbb{R}$是开集，则$f^{-1}(G)$相对于E是闭集

**※【定义】** 支集
> $\text{supp }f=\overline{\{ x\in E:f(x)\not=0 \}}$ 若支集为有界闭集，则称f为有紧支集，记作$C_{c}(E)$

**※【定义】** 振幅
> $f(X)$在$x_{0}$处的振幅，$w_{f}(\delta)=\sup \{|f(x_{1})-f(x_{2})|:x_{1},x_{2}\in B(x_{0},\delta)  \},\,w_{f}(x_{0})=\lim_{ \delta \to 0 }w_{f}(\delta)$

**※【定理】** f在$x_{0}$处连续$\Leftrightarrow w(x_{0})=0$

**※【定义】** 点集之间的距离
> 设A, B$\subset \mathbb{R}^{n}$. $d(A,b)=\inf\{ d(x,y):x\in A,y\in B \}$

**※【命题】** 设A, B是非空闭集且有一个有界集，则$\exists\,x\in A,\,y\in B,\text{ s.t. }d(x,y)=d(A,B)$

**※【命题】** 设A, B是非空闭集且有一个有界集，则$\exists\,$开集$G_{1},G_{2}$满足$G_{1}\cap G_{2}=\emptyset$，则$A\subset G_{1},B\subset G_{2}(d(A,B)=d)$

##### 闭集上连续函数的延拓
**※【定理】** 设$F\subset \mathbb{R}^{n}$是闭集. $f\in C(F)$且$|f(x)|\leq M$. $\forall\,x\in F$，则$\exists\,g(x)\in C(\mathbb{R}^{n})$满足$g(x)=f(x),x\in F;|g(x)|\leq M,\forall\,x\in \mathbb{R}^{n}$ 

**※【引理】** 设A, B为非空闭集. $A\cap B=\emptyset$ 则$\exists\,f:\mathbb{R}^{n}\to[0,1]$满足$f(x)=0,x \in A;1,x\in B$且$f(x)\in C(\mathbb{R}^{n})$

**※【定义】** 开覆盖 子覆盖
设$E\subset \mathbb{R}^n$，$\Gamma$是$\mathbb{R}^n$中的一个开集族. 若$\forall\,x\in E,\exists\,G\in\Gamma,\text{ s.t. }x\in G$，则称$\Gamma$是$E$的一个开覆盖. 设$\Gamma'$是$E$的一个开覆盖，且$\Gamma'\in\Gamma$，则$\Gamma'$为$\Gamma$的一个子覆盖

**※【引理】** 
$R^n$中点集$E$的任一开覆盖$\Gamma$都含有一个可数子覆盖

**※【定理】** Heine-Borel 有限子覆盖定理


