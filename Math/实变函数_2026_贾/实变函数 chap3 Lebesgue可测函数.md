
## chap3 Lebesgue 可测函数

### 3.1 可测函数的定义

**※【定义】** Lebesgue可测函数
设 $E \subset \mathbb{R}^n$ 是可测集，$f: E \to \overline{\mathbb{R}}$ 是广义实值函数。若对任意实数 $t \in \mathbb{R}$，集合  $E(f>t)=\{x \in E : f(x) > t\} \in \mathcal{M}$, 则称 $f$ 是 **Lebesgue 可测函数**。记 $\mathcal{M}(E)$ 为 $E$ 上可测函数的全体。
**注**：当 $E$ 可测时，常简称为 $f$ 可测。

**※【定理】** 
设 $E \in \mathcal{M}\cap \mathbb{R}^n$ ，$f: E \to \overline{\mathbb{R}}$ $D\subset \mathbb{R}$是一个稠密集，若$\forall\,r\in D$，$\{ x: f(x)>r \}\in \mathcal{M}$，则$\forall\,t\in \mathbb{R}$，$\{ x: f(x)>t \}\in \mathcal{M}$.

e.g. 常值函数 $f(x) = c$ 可测，因为  $E\{f > t\} = \begin{cases}\emptyset, & t \ge c, \\ E, & t < c.\end{cases}$ 
e.g. 特征函数 $\chi_E(x) = \begin{cases}1,& x\in E\\0,& x\notin E\end{cases}$ 可测当且仅当 $E$ 可测。  
e.g. 单调函数 $f$ 在 $[a,b]$ 上可测（因为 $\{f > t\}$ 是区间或空集）。  
e.g. 若 $m(E)=0$，则 $E$ 上的任何函数都可测（因为子集都是零测集，从而可测）。

**※【命题】** 可测函数的等价刻画
设 $f: E \to \overline{\mathbb{R}}$，则下列条件等价：
1. $f$ 可测（即 $\forall t\in\mathbb{R},\; E\{f > t\} \in \mathcal{M}$）。  
2. $\forall t\in\mathbb{R},\; E\{f \ge t\} \in \mathcal{M}$。   $E\{f \ge t\} = \bigcap_{k=1}^{\infty} E\{f > t - \tfrac{1}{k}\}$.
3. $\forall t\in\mathbb{R},\; \{f < t\} \in \mathcal{M}$（因为 $\{f < t\} = E \setminus E\{f \ge t\}$）。  
4. $\forall t\in\mathbb{R},\; \{f \le t\} \in \mathcal{M}$（因为 $\{f \le t\} = \bigcap_{k=1}^{\infty} E\{f < t + \tfrac{1}{k}\}$）。  
 $\forall t\in\mathbb{R},\; \{f = t\} \in \mathcal{M}$（可测集之差）。  
 $\forall t\in\mathbb{R},\; \{f = +\infty\} = \bigcap_{k=1}^{\infty} \{f > k\} \in \mathcal{M}$。  

**※【定理】** 
1. $f\in \mathcal{M}(E_{1})\cap \mathcal{M}(E_{2})\implies f\in \mathcal{M}(E_{1}\cap E_{2})$ 
2. $f\in \mathcal{M}(E),A\in E\cap \mathcal{M}\implies f\in \mathcal{M}(A)$ 

**※【命题】** $f:E\to \mathbb{R}$，则$f\in\mathcal{M}(E)$ $\Leftrightarrow$ 对任意开集 $G \subset \mathbb{R}$，原像 $f^{-1}(G)$ 可测
   **证明**：开集 $G$ 可表示为可数个开区间的并，而每个开区间的原像可表为 $\{f > a\} \cap \{f < b\}$ 等形式，均为可测集。

**※【推论】** 若 $E$ 可测，$f \in C(E)$，则 $f$ 可测。  
**理由**：对任意开集 $G \subset \mathbb{R}$，$f^{-1}(G)$ 是 $E$ 中的相对开集，从而可测（因为 $E$ 可测，相对开集是 $E$ 与某个 $\mathbb{R}^n$ 中开集的交，而开集可测，交仍可测）。

e.g. $f(x)=\begin{cases}x, & x\in A \\ -x, & x\not\in A\end{cases}$ $A\not\in \mathcal{M}(E)$，故f 是不可测的

**※【定理】** 
设函数f 和函数g 都在可测集D 上可测，则
(i) $\{ f=\lambda \},\{ \alpha<f<\beta \},\{ \alpha<f\leq\beta \},\{ \alpha\leq f<\beta \}$都是可测集，其中$-\infty \leq\alpha<\beta\leq \infty$，$\lambda$是广义实数
(ii) $\{ f>g \}$是可测集

**※【定理】** 
设$\{f_{n}(x)  \}_{n\geq1}$是可测集D 上的一列可测函数，则函数$(1)\sup_{n\geq1}f_{n}(x),(2)\inf_{n\geq1}f_{n}(x),$
$(3)\overline{\lim}_{n\to \infty}f_{n}(x),(4)\underline{\lim}_{n\to \infty}f_{n}(x),(5)\lim_{ k \to \infty }f_{k}(x)=f(x)$ 均是可测的

**※【定理】** 四则运算
$f,g\in \mathcal{M}(E)$:
(1) $\forall\,\lambda \in\mathbb{R},\,\lambda f\in \mathcal{M}(E)$ 
(2) $f+g$在其有意义的集合上可测 
(3) $f\cdot g\in \mathcal{M}(E)\;f^{2}\in \mathcal{M}(E)$ 
(4) $\frac{1}{g}\in \mathcal{M}(E)$

e.g. $f\in \mathcal{M}(E)\implies|f|\in \mathcal{M}(E)$，反之不然.
e.g. 若$f(x,y)$是定义在$\mathbb{R}^{2}$上的实值函数，且对固定的$x/y\in \mathbb{R}$，$f(x,y)$是$y/x\in \mathbb{R}$上的连续函数、可测函数，则$f(x,y)$是$\mathbb{R}^{2}$上的可测函数
e.g. 设$E\in \mathcal{M}$，若$f\in C(E)$，则$f(x)$是$E$上的可测函数. 
e.g. 设$0<m(A)<+\infty$，$f(x)$是$A\subset \mathbb{R}^N$上的可测函数，且有$0<f(x)<\infty$，$a.e.x\in A$，则对任给的$\delta:0<\delta<m(A)$，存在$B\subset A$以及自然数$k_{0}$，使得$m(A\setminus B)<\delta, \frac{1}{k_{0}}\leq f(x)\leq k_{0},x\in B$ 

**※【定义】** 几乎处处
若命题p(x)对E中除了零测集之外的x均成立，则p(x)为几乎处处成立，记作p(x) a.e. $x\in E$ 

e.g. $f(x)=g(x)\quad a.e. x\in E\Leftrightarrow m(\{ x:f(x)\not=g(x) \})=0$ 
e.g. $\sin x\not=0\quad a.e.x\in \mathbb{R}$ 

**※【定义】** 几乎处处收敛
设$f(x),f_{1}(x),\dots,f_{k}(x),\dots$是定义在点集$E\subset \mathbb{R}^n$上的广义实值函数. 若存在$E$中的点集$Z$，有$m(Z)=0$及$\lim_{ k \to \infty }f_{k}(x)=f(x),x\in E\setminus Z$，则称$\{ f_{k}(x) \}$在$E$上几乎处处收敛于$f(x)$ 

**※【命题】** 与可测函数几乎处处相等的函数亦可测
若$f(x)=g(x)\quad a.e.x\in E$ 且$f\in \mathcal{M}(E)$，则$g\in \mathcal{M}(E)$

e.g. Dirichlet函数 不连续但几乎处处连续

例 局部有界化
$0<m(A)<\infty,f\in \mathcal{M}\cap A\subset \mathcal{M}\cap \mathbb{R}^n,0<f(x)<\infty,x\in A$，则$\forall\,0<\delta<m(A),\exists\,B\subset A,k\in \mathbb{N}$，使得$m(A\setminus B)<\delta,1/k_{0}\leq f(x)\leq k_{0},x\in B$

**※【定义】** 正部 负部
$f:E\to \mathbb{R}$ $f^+(x)=\max\{ f(x),0 \}$为 f 的正部 $f^-(x)=\max\{ -f(x),0 \}$为 f 的负部
$f(x)=f^+(x)-f^-(x)\;|f(x)|=f^+(x)+f^-(x)$ 

**※【推论】** 可测函数的极限运算封闭
设$f_{k}(x)\in\mathcal{M}(E)$ $f_{k}(x)\to f(x)\,a.e.\,x\in E$，则$f(x)\in \mathcal{M}(E)$ 

**※【定义】** 简单函数
设$E\subset \mathbb{R}^n$为可测集，$\varphi:E\to \mathbb{R}$为可测函数，若$\varphi(E)$是有限集，则称$\varphi$为简单函数，记作$s(E)$ 

**※【定理】** 可测函数可用简单函数逼近
$E\subset \mathbb{R}^n$为可测集 
1. $f\in \mathcal{M}^+(E)\Leftrightarrow \exists\,\{ \varphi_{k}(x) \}$ 且 $\varphi _{k}\in s^+(E)$且$\varphi _{k}(x)\leq\varphi _{k+1}(x)$且$\lim_{ k \to \infty }\varphi _{k}(x)=f(x)$ 
2. $f\in \mathcal{M}(E)\Leftrightarrow \exists\,\{ \varphi_{k}(x) \}$ (1) $\varphi _{k}\in s(E)$ (2) $|\varphi _{k}(x)|\leq|f(x)|$ (3) $\lim_{ k \to \infty }\varphi _{k}(x)=f(x)\,\forall\,x\in E$ 若$f(x)$ 还是有界的，则上述收敛是一致的.
3. 上述两个$\varphi _{k}(x)$均可以写成具有紧支集的简单函数，如$\psi _{k}(x)=\varphi _{k}(x)\chi_{B(0,k)}(x)$ 

**※【定义】** 支集
对于定义在$E\subset \mathbb{R}^n$上的函数$f(x)$，称点集$\{ x: f(x)\not=0 \}$的闭包为$f(x)$的支集，记作$supp(f)$. 
若$supp(f)$是紧集，则称$f(x)$是具有紧支集的函数. 

### 3.2 可测函数的收敛

#### （一）几乎处处收敛与一致收敛

**※【定义】** 几乎处处收敛
$f(x),f_{1}(x),\dots,f_{k}(x),\dots$定义在$E$上. 若存在$Z\subset E,m(Z)=0,\lim_{ k \to \infty }f_{k}(x)=f(x),a.e.x\in E\setminus Z$，则$f_{k}(x)\to f(x),a.e.x\in E$ 

**※【定义】** 几乎一致收敛
$f_{n}(x)$ 在E上几乎一致收敛于$f(x)$ $f_{n}(x)\overset{\text{a.u.}}{\rightrightarrows}f(x)\quad(E)$
$\Leftrightarrow\forall\,\delta>0,\,\exists\,E_{\delta}\subset E,$ 满足$m(E\setminus E_{\delta})<\delta$ 且 $f_{n}\rightrightarrows f(x)\quad(E_{\delta})$

**※【命题】** 几乎一致收敛$\implies$几乎处处收敛
设$f_{k}(x),f(x)$是定义在可测集E 上的几乎处处有限的可测函数. 若$f_{k}(x)\overset{\text{a.u.}}{\rightrightarrows}f(x)$，则$f_{k}(x)\to f(x),a.e.$ 

**※【引理】** 
设$m(E)<\infty$，$f_{k}(x)\to f(x),a.e.$，则$\forall\,\varepsilon>0,$有$\lim_{ k \to \infty }m(\cup_{j=k}^{\infty}E_{j}(\varepsilon))=0$，其中$E_{j}(\varepsilon)=\{ x\in E:|f_{j}(x)-f(x)|\geq\varepsilon \}$

**※【定理】** Eropoв叶果若夫定理 ==Egoroff定理==
$m(E)<\infty,f_{k}(x),f(x)$是定义在可测集$E$上的几乎处处有限的可测函数. 
若$f_{k}(x)\to f(x),a.e.$，则$f_{k}(x)\overset{\text{a.u.}}{\rightrightarrows}f(x)\quad(E)$ 
等价表述
$m(E)<\infty,f_{k}(x),f(x)$是定义在可测集$E$上的几乎处处有限的可测函数. 
若$f_{k}(x)\to f(x),a.e.$，则$\forall\,\varepsilon>0$，有$E$的可测子集$E_{\delta}$，使得 (1) $m(E_{\delta})<\varepsilon$，(2) $f_{n} \rightrightarrows f \quad E\setminus E_{\delta}$  

e.g. 给定自然数n，我们总可以找到唯一的自然数$k,i$，使得$n=2^k+i,0\leq i<2^k,k=1,2,\dots$.令$E_{k}=\left[ \frac{i}{2^k}, \frac{i+1}{2^k} \right]$，$f_{n}(x)=\chi_{E_{k}}(x),n=1,2,\dots,x\in[0,1]$. 故不收敛.
$m(\{ x\in[0,1]: |f_{n}(x)|\geq\varepsilon \})=\frac{1}{2^k}$. 依测度收敛.

#### （二）几乎处处收敛与依测度收敛

**※【定义】** 依测度收敛
$f_{k},f\in \mathcal{M}(D)$，且几乎处处有限. 若$\forall\,\delta>0$，有$\lim_{ n \to \infty }m(\{ |f_{n}-f|\geq\delta \})=0$，则我们说$f_{n}$在D 上测度收敛于f . 记作$f_{n} \stackrel{ m}{ \longrightarrow}f$ 

e.g. $[\frac{k-1}{n},\frac{k}{n}]\to \chi_{n,k}$，令$f=0,f_{1}=\chi_{1,1},f_{2}=\chi_{2,1},f_{3}=\chi_{2,2},\dots$，容易看出是测度收敛但不收敛的,

**※【定理】** Riesz定理
设$f_{k}(x),f(x)$是定义在可测集D 上的几乎处处有限的可测函数. 则：
(i) 若$f_{n}$测度收敛于$f$，则$\{ f_{n} \}_{n}$中必有子列几乎处处收敛于$f$. 
(ii) 若$m(D)<\infty$ 并且$f_{n}$几乎处处收敛于$f$，则$f_{n}$测度收敛于$f$.

**※【定理】** 依测度收敛的极限唯一
若$f_{k} \stackrel{ m}{ \longrightarrow}f,f_{k} \stackrel{ m}{ \longrightarrow}g$，则$f(x)=g(x),a.e.$

**※【定理】** Lebesgue定理
$f_{k}(x),f(x)$是定义在可测集E 上的几乎处处有限的可测函数. 设$m(E)<\infty$ 若$f_{k}\to f,a.e.$ 则$f_{k} \stackrel{ m}{ \longrightarrow}f\,(E)$ 

**※【定义】** 测度基本列
$\forall\,\delta>0,$有$\lim_{ m,n\to \infty }m(\{ |f_{m}-f_{n}|\geq\delta \})=0$

**※【定理】** 测度基本列一定测度收敛
设$\{ f_{k}(x) \}$是测度基本列，则$\exists\,f\in \mathcal{M}(E)$，使得$f_{k}(x) \stackrel{m}{ \longrightarrow}f(x)\,E$ 

### 3.3 可测函数与连续函数的关系

#### （一）Lusin定理

**※【定理】** 设F是一个紧集，$\{ f_{n} \}_{n\geq1}$是一列沿F连续的函数. 若$\{ f_{n} \}$在F上一致收敛于f，则f也沿F连续.

**※【引理】** Tietze延拓定理的特殊情况
设F 是$\mathbb{R}$中的闭集，函数f 沿F 连续且有界，则f 可以开拓成$\mathbb{R}$ 上的连续函数$f^*$，并且$\sup_{x\in \mathbb{R}}|f^*(x)|=\sup_{x\in \mathbb{F}}|f(x)|$. 

**※【引理】** 
设$f\in \mathcal{s}(D),D\in \mathcal{M},m(D)<\infty$，则$\forall\,\varepsilon>0$，有沿D 连续的函数$f^*$使$m(f\not=f^*)<\varepsilon$

**※【定理】** Lusin定理 Лузин
设 $f$ 是定义在可测集 $D\subset \mathbb{R}^n$ （ $m(D)<\infty$ ）上的**几乎处处有限的可测函数**. 则对 $\forall\,\varepsilon>0$，存在**闭集** $F\subset D$ ，使得：
1. $m(D\setminus F)<\varepsilon$
2. $f|_{F}$在 $F$ 上**连续** 

**※【定理】** Lusin定理2 延拓
设$f\in \mathcal{M}(E)$，$f$ 在 $D$ 上**有界**（即 $\sup _{⁡x\in D}|f(x)|=M<∞$ ），则$\forall\,\delta>0$，$\exists\,$ Ferme $F\subset E$ 以及 $g(x)\in C(\mathbb{R}^n),s.t.$
(1) $m(E\setminus F)<\delta$
(2) $g(x)=f(x),x\in F$ 
(3) $\sup_{x\in \mathbb{R}}|g(x)|=\sup_{x\in F}|f(x)|$ $\inf_{x\in \mathbb{R}}|g(x)|=\inf_{x\in F}|f(x)|$.

**※【定理】** Frechet定理
设$f:E\to \overline{\mathbb{R}},|f|<\infty,a.e.$，则$f\in \mathcal{M}(E)\Leftrightarrow\exists\,E$上一列连续函数$\{ \varphi _{k}(x) \}\,s.t.\varphi _{k}(x)\to f(x),\,a.e.x\in E$

**※【推论】** 
设$f$是$[a,b]$上的几乎处处有限的可测函数. 则$\forall\,\varepsilon>0$，有$[a,b]$上连续的函数$f^*$，使$m(f\not=f^*)<\varepsilon$ 且 $\max|f^*(x)|\leq\sup|f(x)|$. 若$E$是有界集，则可使上述$g(x)$有紧支集. 

**※【推论】** 
$f\in \mathcal{M}(D)$，且几乎处处有限. 则存在$\mathbb{R}^n$上的连续函数列$\{ g_{k}(x) \}$，使得$\lim_{ k \to \infty }g_{k}(x)=f(x),a.e.$

#### （二）复合函数的可测性

**※【引理】** 
若$f(x)$是定义在$\mathbb{R}^n$上的实值函数，则$f(x)$在$\mathbb{R}^n$上可测的充分必要条件是对于$\mathbb{R}$中的一个开集$G$，$f^{-1}(G)$是可测集. 

**※【命题】** 
设$f:\mathbb{R}^n\to \mathbb{R}$连续，则$f\in \mathcal{M}(\mathbb{R}^n)\Leftrightarrow\forall\,$ Gebiet $G\subset \mathbb{R},f^{-1}(G)\in \mathcal{M}$ 

**※【定理】** 
设$f:\mathbb{R}\to \mathbb{R}$连续，$g:\mathbb{R}^n\to \mathbb{R}$可测，则$f{\circ}g:\mathbb{R}^n\to \mathbb{R}$是可测的

**※【定理】** 
设$f:\mathbb{R}^n\to\mathbb{R}$可测，$\Phi:\mathbb{R}^n\to \mathbb{R}^n$连续且$\forall\,N\in \mathbb{R}^n,m(N)=0$ 有$m(\Phi ^{-1}(N))=0$，则$f{\circ}\Phi\in \mathcal{M}(\mathbb{R}^n)$ 



