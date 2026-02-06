## II deterministic models
### 3 mathematical preliminaries
#### 3.1 metric spaces and normed vector spaces
###### ※【定义】a real vector space VS
> X is a set of elements with addition and scalar multiplication. $x,y\in \mathbf{X},\,\alpha,\beta\in \mathbb{R}$ 
> (1) $x+y\in \mathbf{X}$
> (2) $\alpha x\in \mathbf{X}$
> (a) $x+y=y+x$
> (b) $x+(y+z)=(x+y)+z$
> (c) $\alpha(x+y)=\alpha x+\alpha y$
> (d) $(\alpha+\beta)x=\alpha x+\beta x$
> (e) $(\alpha\beta)x=\alpha(\beta x)$
> (f) $x+\theta=x$
> (g) $0x=\theta$
> (h) 1x=x

###### ※【定义】a metric space MS
> a set S with a metric $\rho:S\times S\to R$ s.t. $\forall\,x,y,z\in S$
> a. $\rho(x,y)\geq0$ with equality if and only if $x=y$
> b. $\rho(x,y)=\rho(y,x)$
> c.$\rho(x,z)\leq \rho(x,y)+\rho(y,z)$

###### ※【定义】normed vector space NVS
> a vector space S with a norm $\parallel \cdot \parallel:\mathbf{S}\to \mathbb{R},\,\text{ s.t. } \forall\, x,y\in \mathbf{S},\,\alpha\in \mathbb{R}$
> a. $\parallel x\parallel\geq0$ with equality if and only if $x=\theta$
> b. $\parallel \alpha x\parallel=|\alpha|  \cdot \parallel x\parallel$
> c. $\parallel x+y\parallel\leq\parallel x\parallel+\parallel y\parallel$

###### ※【定义】 converges
> a sequence $\{ x_{n} \}_{n=0}^\infty\in \mathbf{S}$ converges to $x\in \mathbf{S}$，if for each $\varepsilon>0$，there exists $N_{\varepsilon}$ s.t. $\rho(x_{n},x)<\varepsilon,\,\forall\,n>N_{\varepsilon}$

###### ※【定义】cauchy sequence
> a sequence $\{ x_{n} \}_{n=0}^\infty\in \mathbf{S}$ is a cauchy sequence if for each $\varepsilon>0$, there exists $N_{\varepsilon}$ s.t. $\rho(x_{n},x_{m})<\varepsilon,\forall\,n,m\geq N_{\varepsilon}$ 

###### ※【定义】complete 
> a metric space $(\mathbf{S},\rho)$ is complete if every cauchy sequence in $\mathbf{S}$ converges to an element in $\mathbf{S}$

###### ※【定理】C(X) with sup norm is CNVS
> let $X\subset \mathbb{R}^l$ and let $C(X)$ be the set of bounded continuous function $f:\mathbf{X}\to \mathbb{R}$ with a sup norm $\parallel f \parallel=sup_{x\in \mathbf{X}}|f(x)|$. Then C(X) is a CNVS

#### 3.2 contraction mapping theorem
###### ※【定义】contraction mapping
> a metric space $(\mathbf{S},\rho)$ and $T:\mathbf{S}\to \mathbf{S}$ be a function. T is a contraction mapping with modulus $\beta$ if for some $\beta\in(0,1),\rho(Tx,Ty),\forall\,x,y\in \mathbf{S}$

###### ※【定理】contraction mapping theorem
> a metric space $(\mathbf{S},\rho)$ and $T:\mathbf{S}\to \mathbf{S}$ be a contraction mapping with modulus $\beta$ , then
> a. T has exactly one fixed point v in $\mathbf{S}$
> b. for any $v_{0}\in S$, $\rho(T^nv_{0},v)\leq\beta^n\rho(v_{0},v),n=0,1,2,\dots$

###### ※【推论】
> let $(S,\rho)$ be a complete metric space and let $T:S\to S$ be a contraction mapping with fixed point $v \in S$. if $S'$ ia a closed subset of S and $T(S')\subset S''$, then it follows that $v=Tv\in S''$ 

###### ※【推论】N-stage contraction theorem

###### ※【定理】contraction mapping theorem

#### 3.3 Theorem of the Maximum
###### ※【定义】lower hemi-continuous l.h.c.
> a correspondence $\Gamma:X\to Y$ is l.h.c. at x if $\Gamma(x)$ is nonempty and if, for every $y\in\Gamma(x)$ and every sequence $x_{n}\to x$, there exists $N\geq1$ and a sequence $\{ y_{n} \}_{n=N}^\infty$ s.t. $y_{n}\to y$ and $y_{n}\in\Gamma(x_{n})$, all $n\geq N$
###### ※【定义】upper hemi-continuous u.h.c.
> a compact-valued correspondence $\Gamma:X\to Y$ is upper hemi-continuous at x if $\Gamma(x)$ is nonempty and if for every sequence $x_{n}\to x$ and every sequence of $y_{n}$ whose limit point y is in $\Gamma(x)$
###### ※【定义】continuous
> a correspondence $\Gamma:X\to Y$ is continuous at $x\in X$ if it's both l.h.c. and u.h.c. at x
###### ※【定理】graph A closed + set $\Gamma(\hat{x})$ bounded \implies $\Gamma$ compacted-valued and u.h.c
>   let $\Gamma: X \to Y$ be nonempty-valued correspondence, and let A be the graph of $\Gamma$. Suppose that A is closed and that for any bounded set  $\hat{X}\subset X ,\,\Gamma(\hat{X})$ is bounded. Then $\Gamma$ is compact-valued and l.h.c..
###### ※【定理】graph A convex +  $\Gamma(x)\cap \hat{Y}\neq\emptyset \implies$ $\Gamma$ l.h.c.
>  let $\Gamma: X \to Y$ be nonempty-valued correspondence, and let A be the graph of $\Gamma$. Suppose that A is convex and that for any bounded set  $\hat{X}\subset X$ there is a bounded $\hat{Y}\subset Y$ such that $\Gamma(x)\cap \hat{Y}\neq\emptyset,\,\forall\,x\in \hat{X}$. Then $\Gamma$ is lower hemi-continuous at every interior point of X.
###### ※【定理】Theorem of the Maximum
> let $\mathbf{X}\subset \mathbb{R}^l$ and $\mathbf{Y}\subset \mathbb{R}^m$ , ① let $f:\mathbf{X}\times \mathbf{Y}\to \mathbb{R}$ be a continuous function and ② let $\Gamma:\mathbf{X}\to \mathbf{Y}$ be a compact-valued and continuous correspondence. Then ① the function $h:\mathbf{X}\to \mathbb{R},\,h(x)=\max_{y\in\Gamma(x)}f(x,y)$ is continuous, and ② the correspondence $G:\mathbf{X}\to \mathbf{Y},\,G(x)=\{ y\in\Gamma(x):f(x,y)=h(x) \}$ is nonempty, compact-valued and u.h.c.. 