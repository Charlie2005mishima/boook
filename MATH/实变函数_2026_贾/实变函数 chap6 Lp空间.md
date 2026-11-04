## 6.1 Lp空间的定义与不等式

**※【定义】** $L^p$ 空间

1. $f\in \mathcal{M}(E),E\subset \mathbb{R}^n$，记$\parallel f\parallel_{p}=\left( \int_{E}|f(x)|^p \right)^{1 /p},0<p<\infty$. 用$L^p(E)$表示$\parallel f\parallel_{p}<\infty$的$f$全体
2. $f\in \mathcal{M}(E),E\subset \mathbb{R}^n$，若存在$M,s.t.\parallel f\parallel\leq M,a.e.$，则$f$在$E$本性有界 $M$为$f$的本性上界 $\parallel f\parallel_{\infty}=\inf\{ M:M\text{为本性上界} \}$称为$f$在$E$上的本性上确界，此时用$L^\infty(E)$表示本性有界函数全体

**※【定理】** $L^p$ 空间是线性的

例 $m(E)<\infty$，则有界可测函数$\in L^p(E)$ 
例 $m(E)<\infty,p\geq1\Rightarrow L^p(E)\subset L^1(E)$ 

**※【定义】** 共轭指标
$p,p'>1$且$\frac{1}{p}+\frac{1}{p'}=1$，则称$p,p'$为共轭指标

**※【定义】** Holder不等式
$p,p'$为共轭指标 若$f\in L^p(E),g\in L^p(E)$，则$\parallel fg\parallel_{1}\leq \parallel f\parallel_{p}\cdot \parallel g\parallel_{p'},p\in[1,\infty]$ 

例 $m(E)<\infty$且$0<p_{1}<p_{2}\leq \infty$，则$L^{p_{2}}(E)\subset L^{p_{1}}(E)$且有$\parallel f\parallel_{p_{1}}\leq(m(E))^{(1/p_{1}-1/p_{2})}\parallel f\parallel_{p_{2}}$.

例 插值不等式

例

**※【定理】** Minkowski不等式
$f,g\in L^p(E),1\leq p\leq \infty$，则$\parallel f+g\parallel_{p}\leq\parallel f\parallel_{p}+\parallel g\parallel_{p}$. 

**※【定理】** 
$f_{k}\in L^p(E),1\leq p\leq \infty,k\in \mathbb{N}$且$\sum_{k=1}^\infty f_{k}(x)$在$E$上几乎处处收敛，
则$\parallel \sum_{k=1}^\infty f_{k}(x)\parallel_{p}\leq \sum_{k=1}^\infty \parallel f_{k}\parallel_{p}$. 

**※【定义】** $L^p$收敛
$f_{k},f\in L^p(E),1\leq p\leq \infty$ 若$\lim_{ k \to \infty }d(f_{k},f)=\lim_{ k \to \infty }\parallel f_{k}-f\parallel_{p}=0$，则$f_{k}(x)$按$L^p$范数收敛于$f$，$f_{k} \stackrel{ L^p}{ \longrightarrow}f$. 

**※【命题】** 
1. $f_{k} \stackrel{ L^p}{ \longrightarrow}f,f_{k} \stackrel{ L^p}{ \longrightarrow}g\Rightarrow f=g$.
2. $f_{k} \stackrel{ L^p}{ \longrightarrow}f,g_{k} \stackrel{ L^p}{ \longrightarrow}g\Rightarrow \alpha f_{k} +\beta g_{k}\stackrel{ L^p}{ \longrightarrow}\alpha f+\beta g$.
3. $f_{k} \stackrel{ L^p}{ \longrightarrow}f\Rightarrow \parallel f\parallel_{p}$.

**※【定理】** $L^p$收敛$\Rightarrow$依测度收敛

**※【定义】** cauchy列
$f_{k}\subset L^p(E)$ 若$\lim_{ j,k \to \infty }\parallel f_{j}-f_{k}\parallel_{p}=0$，则$f_{k}$是$L^p$ cauchy列

**※【定义】** 在$L^p$上稠密
$K\subset L^p(E)$ 称$K$在$L^p$上稠密，即$\forall\,f\in L^p,\forall\,\varepsilon>0,\exists\,g\in K,s.t.\int_{E}|f-g|^pdx<\varepsilon$.