
## chap2 lebesgue测度

广义实数$\overline{\mathbb{R}}=\mathbb{R}\cup \{ \pm \infty\}$

**※【定义】** 测度（以$\mathbb{R}$为例）
(1) $m(E)\geq0$
(2) 可合同的集合具有相同的测度
(3) 令$I=(a,b)$，则$m(I)=b-a$
(4) 若$E_{1},\dots,E_{k},\dots$是互不相交的点集，则$m\left( \sum_{i=1}^\infty E_{i} \right)=\sum_{i=1}^\infty m(E_{i})$ $\sigma$-可加性
### 2.2 lebegue外测度

**※【定义】** lebegue外测度
> 对任意实数子集E，定义$m^*(E)=\inf \{ \sum_{n} \mathscr{l}(I_{n}):\{ I_{n} \}_{n\geq1}$ 是一列可数个开区间且 $E\subset \cup_nI_{n}\}$，此时$m^*(E)$称为E的lebegue外测度，$\{ I_{n} \}_{n\geq1}$是E的一个lebegue覆盖
> > $0\leq m^*(E)=\alpha<+\infty\Leftrightarrow\forall\,\{ I_{k} \},\sum_{k=1}^\infty|I_{k}|\geq\alpha$且$\forall\,\varepsilon>0,\exists\,\{ I_{k} \},\sum_{k=1}^\infty|I_{k}|<\alpha+\varepsilon$ 

**※【定理】** $\mathbb{R}^n$中点集的外测度性质（外测度的定义）
非负性：$m^*(E)\geq 0,m^*(\emptyset)=0$ 
单增性：若$E_{1}\subset E_{2}$，则$m^*(E_{1})\leq m^*(E_{2})$
次可数可加性：$m^*(\cup_{n}E_{n})\leq \sum_{n}m^*(E_{n})$

e.g. 可数集的外测度为零
e.g. 方体的外测度等于本身
e.g. Cantor三分集的外测度为零，即不可数集的外测度也可为零

**※【定理】** 外测度的等价定义
> 1. $m^*(E)=\inf \{ m^*(G):G\supset E,G \text{是开集}\}$
> 2. $m^*(E)=\inf \{ \sum_{n} \mathscr{l}(I_{n}):\{ I_{n} \}_{n\geq1} \text{是一列开聚体且}E\subset \cup_nI_{n},|I_{k}|<\delta\}$ 

e.g. $E_{1},E_{2}\subset \mathbb{R}$，满足$d(E_{1},E_{2})>0$，则$m^*(E_{1}\cup E_{2})=m^*(E_{1})+m^*(E_{2})$ 
e.g. 设$E\subset[a,b]$，$0<c<m^*(E)$，则存在$E$的子集$A$，使得$m^*(A)=c$ 
e.g. 设$E\subset \mathbb{R},\lambda \in \mathbb{R}$，记$\lambda E=\{ \lambda x:x\in E \}$，则$m^*(\lambda E)=|\lambda|m^*(E)$ 

**※【定义】** 平移

**※【命题】** $m^*(E)=m^*(E+y)$

### 2.3 Lebesgue可测集与Lebesgue测度

**※【定义】** 外测度的有限可加性
$\forall\,E_{1},E_{2}\subset \mathbb{R},E_{1}\cap E_{2}=\emptyset$，有$m^*(E_{1}\cup E_{2})=m^*(E_{1})+m^*(E_{2})$

**※【定义】** lebesgue可测
设$E\subset \mathbb{R}^n$，若对$\forall\,T\subset \mathbb{R}^n$，有$m^*(T)=m^*(T\cap E)+m^*(T\cap E^c)$，则E为lebesgue可测集，我们用$\Omega$表示可测集全体，对每一$E\in\Omega/\mathcal{M}$，记$m(E)=m^*(E)$，$m(E)$称为可测集E的lebesgue测度
注：$\mathcal{M} \subset \mathcal{P}(\mathbb{R}^n)$。

**※【命题】** $E \in \mathcal{M} \iff \forall$ 方体 $I \subset \mathbb{R}^n$ 有$|I| = m^*(I) = m^*(I \cap E) + m^*(I \cap E^c)$

**※【命题】** $E \in \mathcal{M} \iff \forall A \subset E, \; B \subset E^c$ 有$m^*(A \cup B) = m^*(A) + m^*(B)$
**证明**：  
“$\Rightarrow$”：取 $T = A \cup B$，有 $m^*(T) = m^*(T \cap E) + m^*(T \cap E^c)$。  
“$\Leftarrow$”：对任意 $T \subset \mathbb{R}^n$，取 $A = T \cap E \subset E$，$B = T \cap E^c \subset E^c$，由条件即得可测性。

e.g. 若 $m^*(E) = 0$，则 $E \in \mathcal{M}$ 且 $m(E) = 0$。（外测度为零 ⇒ 可测）
**证明**：  
$m^*(T \cap E^c) \le m^*(T)$，$m^*(T \cap E) \le m^*(E) = 0$，于是$m^*(T) \ge m^*(T \cap E) + m^*(T \cap E^c)$

e.g. 若 $E \subset \mathbb{R}^n$ 是方体，则 $E \in \mathcal{M}$，$m(E) = |E|$。
**证明**：  
设 $m^*(T) < \infty$，则对任意 $\varepsilon > 0$，存在开方体列 $\{I_k\}$ 覆盖 $T$ 使得$m^*(T) \le \sum_{k=1}^{\infty} |I_k| < m^*(T) + \varepsilon$
且$T \cap E \subset \bigcup_k (I_k \cap E),\quad T \cap E^c \subset \bigcup_k (I_k \cap E^c)$

**※【定理】** 可测集的性质（(1)-(5)也是测度的定义）
(1) $\emptyset\in \mathcal{M}$
(2) 若$A\in \mathcal{M}$，则$A^c\in \mathcal{M}$
(3) 若$A,B\in \mathcal{M}$，则$A\cap B,A\cup B,A-B\in \mathcal{M}$ 
(4) 若$A_{k}\in \mathcal{M},k=1,2,\dots,N\implies \cup_{k=1}^N A_{k}\in \mathcal{M},\cap_{k=1}^N A_{k}\in \mathcal{M}$
(5) 若$A_{k}\in \mathcal{M},k=1,2,\dots,N\implies \cup_{k=1}^\infty A_{k}\in \mathcal{M}$ ，若进一步$A_{i}\cap A_{j}=\emptyset(i\not=j)$，则$m(\cup_{i=1}^\infty A_{k})=\sum_{i=1}^\infty m(A_{k})$  可列可加性（$\sigma-$可加性）

**※【推论】**
$\mathbb{R}^n$ 中的开集、闭集、$G_\delta$ 集、$F_\sigma$ 集均为 Lebesgue 可测。
(6) 若 $A \subset B$，$A, B \in \mathcal{M}$，则 $m(A) \le m(B)$（外测度单调）。
(7) 若 $A \subset B$，$A, B \in \mathcal{M}$，且 $m(A) < \infty$，则$m(B \setminus A) = m(B) - m(A)$
(8) 设 $A_1 \subset A_2 \subset \cdots \subset A_k \subset \cdots$，$A_k \in \mathcal{M}$，则$\lim_{k\to\infty} A_k = \bigcup_{k=1}^{\infty} A_k \in \mathcal{M}$ 且 $m\left(\lim_{k\to\infty} A_k\right) = \lim_{k\to\infty} m(A_k)$ 
(9) 设 $A_1 \supset A_2 \supset \cdots \supset A_k \supset \cdots$，$A_k \in \mathcal{M}$，且$m(E_{1})<+\infty$，则
$\lim_{k\to\infty} A_k = \bigcap_{k=1}^{\infty} A_k \in \mathcal{M}$且$m\left(\lim_{k\to\infty} A_k\right) = \lim_{k\to\infty} m(A_k)$ 
(10) 若 $A_k \in \mathcal{M}$，则 $\varliminf_{k\to\infty} A_k \in \mathcal{M}$，$\varlimsup_{k\to\infty} A_k \in \mathcal{M}$。

e.g. 设 $E_k$ 单增，求证：$m^*\left(\lim_{n\to\infty} E_n\right) = \lim_{n\to\infty} m^*(E_n)$

e.g. 若有可测集列$\{ E_{k} \}$，且有$\sum_{i=1}^\infty m(E_{k})<+\infty$，则$m(\overline{\lim}_{ k \to \infty }E_{k})=0$ 
e.g. 若有可测集列$\{ E_{k} \}$，则$m(\underline{\lim}_{ k \to \infty }E_{k})\leq\underline{\lim}_{ k \to \infty }m(E_{k})$ 
e.g. 若有可测集列$\{ E_{k} \}$，则$m(\overline{\lim}_{ k \to \infty }E_{k})\geq\overline{\lim}_{ k \to \infty }m(E_{k})$ 

### 2.4 可测集和Borel集的关系

**※【引理】** Caratheodory引理
设$G\not=\mathbb{R}^n$是开集，$E\subset G$，令$E_{k}=\{ x\in E:d(x,G^c)\geq1/k \}(k=1,2,\dots)$，则$\lim_{ k \to \infty }m^*(E_{k})=m^*(E)$ 

**※【定理】** 非空闭集$F$是可测集

**※【推论】** Borel 集是 Lebesgue 可测的

**※【命题】** 可测集的等价表述
设 $E \subset \mathbb{R}^n$，则下列命题等价：
1. $E \in \mathcal{M}$；
2. $\forall \varepsilon > 0$，存在开集 $G \supset E$ 使得 $m^*(G \setminus E) < \varepsilon$；
3. $\forall \varepsilon > 0$，存在闭集 $F \subset E$ 使得 $m^*(E \setminus F) < \varepsilon$。

**※【定理】** 
若$E\in \mathcal{M}$，则
(1) $E=H\setminus Z_{1}$，$H$ 是 $G_{\delta}$ 集，$m(Z_{1})=0$ 
(2) $E=K\cup Z_{2}$，$K$ 是 $F_{\sigma}$ 集，$m(Z_{2})=0$ 

**※【定理】** 外测度的正则性
若$E\subset \mathbb{R}^n$，则存在包含$E$的$G_{\delta}$集$H$，使得$m(H)=m^*(E)$ 

**※【推论】** 设$E_{k}\subset \mathbb{R}^n(k=1,2,\dots)$，则$m^*(\underline{\lim}_{k\to \infty}E_{k})\leq \underline{\lim}_{k\to \infty}m^*(E_{k})$ 

**※【推论】** 若$\{ E_{k} \}$是单增集合列，则$\lim_{ k \to \infty }m^*(E_{k})=m^*(\lim_{ k \to \infty }E_{k})$ 

### 2.5 正测度集与矩体的关系

**※【定理】** 设$E$是$\mathbb{R}^n$中的可测集，且$m(E)>0,0<\lambda<1$，存在矩体$I$，使得$\lambda|I|<m(I\cap E)$ 

**※【定理】** Steinhaus定理
设$E$是$\mathbb{R}^n$中的可测集，且$m(E)>0$，作点集$E-E=\{ x-y:x,y\in E \}$，则存在$\delta_{0}>0$，使得$E-E\supset B(0,\delta_{0})$ 


### 2.6 不可测集的存在性

对任意 $x \in [0,1]$，定义$E(x) = \{ y \in [0,1] : y - x \in \mathbb{Q} \}$
易知：
- $x \in E(x)$，且 $[0,1] = \bigcup_{x \in [0,1]} E(x)$；
- 对任意 $x_1, x_2 \in [0,1]$，要么 $E(x_1) = E(x_2)$，要么 $E(x_1) \cap E(x_2) = \varnothing$；
- $E(x_1) = E(x_2) \iff x_1 - x_2 \in \mathbb{Q}$。

取集合 $F \subset [0,1]$ 使得$[0,1] = \bigcup_{x \in F} E(x)$且对任意 $x_1 \neq x_2 \in F$ 有 $E(x_1) \cap E(x_2) = \varnothing$。下证 $F$ 是不可测集。令 $\{r_n\} = [-1,1] \cap \mathbb{Q}$，并记$F_n = F + r_n = \{ x + r_n : x \in F \}$
- 若 $m \neq n$，则 $F_m \cap F_n = \varnothing$（否则存在 $x_m, x_n \in F$ 使 $x_m + r_m = x_n + r_n$，从而 $x_m - x_n \in \mathbb{Q}$，导致 $E(x_m) = E(x_n)$，矛盾）。
- 有包含关系$[0,1] \subset \bigcup_{n=1}^{\infty} F_n \subset [-1,2]$
因为对任意 $y \in [0,1]$，存在 $x \in F$ 使 $y \in E(x)$，则 $y - x \in \mathbb{Q} \cap [-1,1] = \{r_n\}$，故 $y = x + r_n \in F_n$。
假设 $F$ 可测，则每个 $F_n$ 可测且 $m(F_n) = m(F)$。于是$1 \le m\left( \bigcup_{n=1}^{\infty} F_n \right) = \sum_{n=1}^{\infty} m(F_n) \le 3$矛盾。因此 $F$ 不可测。

e.g. 设 $f(x) \in C[0,1]$，$f(x) \ge 0$，令$E = \{ (x,y) : 0 \le x \le 1,\; 0 \le y < f(x) \}$
求证：$E \in \mathcal{M}$ 且 $m(E) = \int_0^1 f(x) \, dx$。（可测性与定积分）

e.g. 设 $f$ 定义在 $\mathbb{R}$ 上，且对任意 $E \in \mathcal{M}$ 有 $f(E) \in \mathcal{M}$。则对任意满足 $m(N) \neq 0$ 的集合 $N$，有 $m(f(N)) \neq 0$。
**证明**：若 $m(f(N)) = 0$，则正测度集一定含有不可测集（矛盾）。

### 2.7 乘积空间的可测性

**※【定理】** 乘积空间的性质
1. $A\times \emptyset=\emptyset$
2. $|I\times J|=|I|\times|J|$
3. $A_{1}\subset A_{2},B_{1}\subset B_{2}\implies A_{1}\times B_{1}\subset A_{2}\times B_{2}$
4. $A_{1}\cap A_{2}=\emptyset, B_{1}\cap B_{2}=\emptyset\implies A_{1}\times B_{1}\cap A_{2}\times B_{2}=\emptyset$
5. $(\cup_{i}A_{i})\times(\cup_{j}B_{j})=\cup_{i,j}(A_{i}\times B_{j})$
6. $(\cap_{k}A_{k})\times(\cap _{k}B_{k})=\cap_{k}(A_{k}\times B_{k})$

**※【定理】** 可测集的维数扩张
设 $A \subset \mathbb{R}^p$，$B \subset \mathbb{R}^q$ 是可测集，$n = p+q$，则 $A \times B$ 是 $\mathbb{R}^n$ 中的可测集，且  $m_n(A \times B) = m_p(A) m_q(B).$
证明 
首先对区间成立，然后利用可测集的结构（开集、$G_\delta$ 集等）和测度的连续性推广。

e.g. 设 $f \in C(\mathbb{R})$，则 $f$ 将可测集映为可测集 当且仅当  $m(f(N)) = 0 \quad \text{对所有 } m(N)=0 \text{ 成立}.$
证明
- 必要性：若 $m(N)=0$ 而 $m(f(N))>0$，则 $f(N)$ 包含不可测子集，矛盾。  
- 充分性：  
	* 对任意有界可测集 $E$，存在 $F_\sigma$ 集 $F \subset E$ 使得 $m(E\setminus F)=0$，则 $f(E)=f(F)\cup f(E\setminus F)$。 
	* 无界情形可分解为可数多个有界集的并。
