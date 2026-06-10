## 5.1 单调函数的可微性
### 5.1.1 Vitali覆盖定理

**※【定义】** Vitali覆盖
设$E\subset \mathbb{R}$，$\Gamma=\{ I_{n} \}$是一个区间族. 若对任意的$x\in E$以及$\varepsilon>0$，存在$I_{n}\in\Gamma$，使得$x\in I_{n},|I_{n}|<\varepsilon$，则称$\Gamma$是$E$在Vitali意义下的一个覆盖

**※【定理】** Vitali覆盖定理
设$E\subset \mathbb{R}$且$m^*(E)<+\infty$. 若$\Gamma$是$E$的Vitali覆盖，则对于任意的$\varepsilon>0$，存在有限个互不相交的$I_{j}\in\Gamma$，使得$m^*(E\setminus\cup_{j=1}^nI_{j})<\varepsilon$ 

### 5.1.2 单调函数的可微性
**※【定义】** Dini导数
设$f(x)$是定义在$\mathbb{R}$中点$x_{0}$的一个领域上的实值函数，令
$D^+f(x_{0})=\limsup_{ h \to 0^+} \frac{f(x_{0}+h)-f(x_{0})}{h},D_+f(x_{0})=\liminf_{ h \to 0^+} \frac{f(x_{0}+h)-f(x_{0})}{h}$ $D^-f(x_{0})=\limsup_{ h \to 0^-} \frac{f(x_{0}+h)-f(x_{0})}{h},D_-f(x_{0})=\liminf_{ h \to 0^-} \frac{f(x_{0}+h)-f(x_{0})}{h}$

**※【定义】** 可微 右导数存在 左导数存在
可微：四个Dini数都等于一个有限值
右导数存在：$D^+f(x_{0})=D_{+}f(x_{0})$ 为有限值
左导数存在：$D^-f(x_{0})=D_{-}f(x_{0})$ 为有限值

例 设$f\in C([a,b])$，则存在$x_{0}\in(a,b)$以及常数k，使得$D_{-}f(x_{0})\geq k\geq D^+f(x_{0}),D^-\leq k\leq D_{+}$ 

**※【定理】** Lebesgue定理
若$f(x)$是定义在$[a,b]$上的递增函数，则
1. $f(x)$的不可微点集为零测集
2. $\int_{a}^bf'(x)dx\leq f(b)-f(a)$ 

下例说明，单调函数是几乎处处可微的这一结论，一般来说是不能改进的
例 设$E\subset(a,b)$，且$m(E)=0$，我们可以做一个在$[a,b]$上连续且递增的函数$f(x)$，使得$f'(x)=+\infty,x\in E$ 

**※【定理】** Fubini逐项微分定理
设$\{ f_{n}(x) \}$是$[a,b]$上的递增函数列，且$\sum_{n=1}^\infty f_{n}(x)$在$[a,b]$上收敛，则$\frac{d}{dx}\left( \sum_{n=1}^\infty f_{n}(x) \right)=\sum_{n=1}^\infty \frac{d}{dx}f_{n}(x),a.e.x\in[a,b].$

例 $f(x)$在$[0,1]$上严格递增，但$f'(x)=0,a.e.$ 
$f_{n}(x)=\frac{1}{2^n}\mathbb{I}\{ r_{n}<x<1 \},f(x)=\sum_{i=1}^\infty f_{n}(x)$ 

## 5.2 有界变差函数

**※【定义】** 变差 全变差 有界变差
全变差：$\bigvee_{a}^b(f)=\sup\{ v_{\Delta}:\Delta\in\Delta[a,b] \}$ 
有界变差：$\bigvee_{a}^b(f)<\infty$ 

例 $[a,b]$上的单调函数是有界变差函数
例 $f(x)$是定义在$[a,b]$上的可微函数，且$|f'(x)|\leq M$，则是有界变差函数

**※【定理】** 
若$f(x)$是$[a,b]$上的实值函数，$a<b<c$，则$\bigvee_{a}^b(f)=\bigvee_{a}^c(f)+\bigvee_{c}^b(f)$  

**※【定理】** Jordan分解定理
$f\in BV([a,b])$当且仅当$f(x)=g(x)-h(x)$，其中$g(x)$与$h(x)$是$[a,b]$上的递增函数

## 5.3 不定积分的微分

**※【引理】** 
设$f\in L([a,b])$，令$F_{h}(x)=\frac{1}{h}\int_{x}^{x+h} f(t) \, dt$，则有$\lim_{ h \to 0 }\int_{a}^b|F_{h}(x)-f(x)|dx=0$ 

**※【定理】** 
设$f\in L([a,b])$，令$F(x)=\int_{a}^{x} f(t) \, dt$，则有$F'(x)=f(x),a.e.x\in[a,b]$  

**※【推论】** 
若$f\in L[a,b]$，则对$[a,b]$中几乎处处的点$x$，都有$\lim_{ h \to 0 }\int_{0}^h|f(x+t)-f(x)|dt=0$ 我们成满足上式的点为$f(x)$的Lebesgue点

例 设$f\in L(\mathbb{R})$. 若对区间$[a,b]$，有$\int_{a}^b|f(x+h)-f(x)|dx=o(|h|)(h\to0)$，则$f(x)=C,a.e.$ 

## 5.4 绝对连续函数与微积分基本定理

**※【引理】** 
设$f(x)$在$[a,b]$上几乎处处可微，且$f'(x)=0,a.e.x\in[a,b]$. 若$f(x)$在$[a,b]$上不是常数函数，则必存在$\varepsilon>0$，使得对任意的$\delta>0$，$[a,b]$内存在有限个互不相交的的区间$(x_{k},y_{k})$，其长度的总和小于$\delta$，满足$\sum_{i=1}^n|f(y_{i})-f(x_{i})|>\varepsilon$ 

**※【定义】** 绝对连续函数
设$f(x)$是$[a,b]$上的实值函数. 若对任给的$\varepsilon>0$，存在$\delta>0$，使得当$[a,b]$中任意有限个互不相交的开区间$(x_{i},y_{i})$ 满足$\sum_{i=1}^n(y_{i}-x_{i})<\delta$时，有$\sum_{i=1}^n|f(y_{i})-f(x_{i})|<\varepsilon$，则称$f(x)$是$[a,b]$上的绝对连续函数

绝对连续函数一定是连续函数
绝对连续函数的全体构成一个线性空间

**※【定理】** 
若$f\in L[a,b]$，则其不定积分$F(x)=\int_{a}^xf(t)dt$是$[a,b]$上的绝对连续函数

**※【定理】** 
若$f(x)$是$[a,b]$上的绝对连续函数，则$f(x)$是$[a,b]$上的有界变差函数，则$f(x)$在$[a,b]$上是几乎处处可微的，且$f'(x)$是$[a,b]$上的可积函数

**※【定理】** 微积分基本定理
若$f(x)$是$[a,b]$上的绝对连续函数，则$f(x)-f(a)=\int_a^xf'(t)dt,x\in[a,b]$.

例2 设$g_{k}(x)(k=1,2,\dots)$是$[a,b]$上的绝对连续函数. 若
(1) 存在c，$a\leq c\leq b$，使得级数$\sum_{k=1}^\infty g_{k}(c)$收敛
(2) $\sum_{i=1}^\infty \int_{a}^b|g'_{i}(x)|dx<\infty$ 
则级数$\sum_{i=1}^\infty g_{k}(x)$在$[a,b]$上是收敛的. 设其极限函数为$g(x)$，则$g(x)$是$[a,b]$上的绝对连续函数，且有$g'(x)=\sum_{k=1}^\infty g_{k}'(x),a.e.x\in[a,b]$ 

例3 无处单调的绝对连续函数
在$[0,1]$中作点集$E$，使得$[0,1]$中任一区间

## 5.5 分部积分公式与积分中值公式

**※【定理】** 分部积分公式
$f,g\in L[a,b],\alpha,\beta\in \mathbb{R}$，令$F(x)=\alpha+\int_{a}^xf(t)dt,G(x)=\beta+\int_{a}^xg(t)dt$，则$\int _a^bG(x)f(x)dx+\int_{a}^bg(x)F(x)dx=F(b)G(b)-F(a)G(a)$ 

**※【定理】** 积分第一中值公式
若$f(x)\in C[a,b],g(x)\in L^+[a,b]$，则$\exists\,\xi\in[a,b],\int_{a}^b f(x)g(x)dx=f(\xi)\int_{a}^bg(x)dx$ 

**※【定理】** 积分第二中值定理
若$f\in L[a,b],g\in M[a,b]$，则$\exists\,\xi\in[a,b],s.t.\int_{a}^b f(x)g(x)dx=g(a)\int_{a}^\xi f(x)dx+g(b)\int_{\xi}^b f(x)dx$ 

## 5.6 R上的积分换元公式

欲探索积分换元公式成立的条件

**※【定理】** 
若$f\in AC[a,b],E\in \mathcal{M}\cap[a,b]$，则$f(E)$是可测集

**※【引理】** 
设$f(x)$是$[a,b]$上的实值函数，$E\subset[a,b]$. 如果$f'(x)$在$E$上存在且$|f'(x)|\leq M$ ，则$m^*(f(E))\leq Mm^*(E)$.

**※【推论】** 
若$f(x)\in L[a,b],E\in \mathcal{M}\cap[a,b]$，且$f(x)\in D(E)$，则$m^*(f(E))\leq \int _E|f'(x)|dx$. 

**※【定理】** 
设$f(x)\in R[a,b]$，在$[a,b]$的子集$E$上是可微的，则有 $f'(x)=0,a.e.x\in E$ 与 $m(f(E))=0$ 等价