这些积分公式实际上可以用一条主线串起来：微积分基本定理 → Green 公式 → Gauss 公式 → Stokes 公式 → Green 恒等式。
它们看似不同，本质都是同一思想：区域内部的微分量积分，可以转化为区域边界上的积分。

## §1 从一维微积分基本定理出发

※【定理】 Newton–Leibniz 公式：
$$\int_a^b f'(x)\,dx=f(b)-f(a)$$

它告诉我们：区间内部导数的积分，由区间两个端点的函数值决定。将区间$[a,b]$ 看成一维区域$\Omega$，其边界为$\partial\Omega=\{a,b\}$。边界的外法向分别是

$$ n(a)=-1,\qquad n(b)=1.$$

因此可以形式化地写成：

$$ \int_\Omega \operatorname{div} F\,dx = \int_{\partial\Omega}F\cdot n\,dS.$$

这实际上就是一维的 Gauss 公式。

下面所有积分公式都可以理解为它在高维空间中的推广。

## §2 二维 Green 公式

设$D\subset\mathbb R^2$ 是有界区域，边界$C=\partial D$ 为分段光滑的闭曲线，按正向（区域在行进方向的左侧）定向。

### 2.1 Green 公式的环流形式

※【定理】 二维 Green 公式
设$P,Q$ 具有连续的一阶偏导数，则
$$ \oint_C P\,dx+Q\,dy = \iint_D\left( \frac{\partial Q}{\partial x} -\frac{\partial P}{\partial y} \right)\,dx\,dy$$


它描述的是：边界上的环流 = 区域内部旋度的总和
设二维向量场$\mathbf F=(P,Q)$, 
其二维标量旋度为$\operatorname{curl}\mathbf F=Q_x-P_y$.
则 Green 公式可以写成
$$ \oint_C\mathbf F\cdot d\mathbf r = \iint_D\operatorname{curl}\mathbf F\,dA.$$

### 2.2 Green 公式的通量形式

另一种同样重要的形式：
$$ \oint_C P\,dy-Q\,dx = \iint_D(P_x+Q_y)\,dx\,dy$$

为什么会出现这种形式？设边界上的单位外法向量为$\mathbf n=(n_1,n_2)$，弧长微元为$ds$。
对于逆时针方向的边界，$n_1\,ds=dy,\, n_2\,ds=-dx$.
因此$\mathbf F\cdot\mathbf n\,ds =P\,dy-Q\,dx$.
于是得到：
$$ \iint_D\nabla\cdot\mathbf F\,dA = \oint_C\mathbf F\cdot\mathbf n\,ds$$
它就是二维 Gauss 散度定理。

因此，二维 Green 公式有两个紧密相关的形式：

|形式|区域内部|边界上|
|---|---|---|
|环流形式|旋度$Q_x-P_y$|$P\,dx+Q\,dy$|
|通量形式|散度$P_x+Q_y$|$P\,dy-Q\,dx$|

## §3 三维 Gauss 散度定理

现在推广到三维空间。

设$\Omega\subset\mathbb R^3$，边界为闭曲面$\Gamma=\partial\Omega$，向量场$\mathbf F=(P,Q,R)$.
定义散度：$\nabla\cdot\mathbf F =P_x+Q_y+R_z$.
则 Gauss 散度定理为$\iiint_\Omega\nabla\cdot\mathbf F\,dV = \iint_\Gamma\mathbf F\cdot\mathbf n\,dS$, 其中$\mathbf n$ 为单位外法向量。

散度$\nabla\cdot\mathbf F$ 衡量向量场在一个点附近向外流出的程度。
例如：
-$\nabla\cdot\mathbf F>0$：局部表现为源；
-$\nabla\cdot\mathbf F<0$：局部表现为汇；
-$\nabla\cdot\mathbf F=0$：没有局部净流出。

Gauss 公式说明：区域内部所有源汇的总量 = 穿过闭曲面的净通量.
这也解释了为什么散度定理要求边界是闭曲面。

## §4 三维 Stokes 公式

Gauss 公式处理的是三维区域与其二维边界，而 Stokes 公式处理的是二维曲面与其一维边界。
设$S$ 为定向光滑曲面，其边界为闭曲线$C=\partial S$，向量场$\mathbf F=(P,Q,R)$。
定义旋度：$\nabla\times\mathbf F = \begin{pmatrix} R_y-Q_z\\ P_z-R_x\\ Q_x-P_y \end{pmatrix}$.

则$\iint_S(\nabla\times\mathbf F)\cdot\mathbf n\,dS = \oint_C\mathbf F\cdot d\mathbf r$

它的含义是：曲面内部的旋度通量 = 曲面边界上的环流

注意与 Gauss 公式区分：

|      | Gauss 公式                  | Stokes 公式                  |
| ---- | ------------------------- | -------------------------- |
| 微分算子 | 散度$\nabla\cdot\mathbf F$ | 旋度$\nabla\times\mathbf F$ |
| 内部积分 | 三维体积分                     | 二维曲面积分                     |
| 边界积分 | 二维闭曲面通量                   | 一维闭曲线环流                    |

而平面 Green 公式的环流形式，其实就是 Stokes 公式在平面上的特例。

## §5 Green 第一、第二恒等式

注意：这一节的 Green 恒等式与前面二维平面 Green 公式的环流形式不同，但它们都可以由相应的积分定理推导。设$u,v$ 为足够光滑的标量函数。

### 5.1 Green 第一恒等式

从向量场$\mathbf F=u\nabla v$ 出发。
利用向量分析中的乘积法则 

$$ \nabla\cdot(u\nabla v) = \nabla u\cdot\nabla v+u\Delta v.$$

应用 Gauss 公式：

$$ \int_\Omega\nabla\cdot(u\nabla v)\,dV = \int_{\partial\Omega}u\nabla v\cdot\mathbf n\,dS.$$

记外法向导数

$$ \frac{\partial v}{\partial n} =\nabla v\cdot\mathbf n.$$

因此得到

$$ \int_\Omega u\Delta v\,dV = \int_{\partial\Omega}u\frac{\partial v}{\partial n}\,dS -\int_\Omega\nabla u\cdot\nabla v\,dV.$$

这就是 Green 第一恒等式，也是多维分部积分公式。

特别地，令$v=u$，有

$$ \int_\Omega u\Delta u\,dV = \int_{\partial\Omega}u\frac{\partial u}{\partial n}\,dS -\int_\Omega|\nabla u|^2\,dV.$$

你之前在热方程能量估计中碰到的

$$ \int_\Omega u\Delta u\,dV =-\int_\Omega|\nabla u|^2\,dV$$

正是这一公式在边界项为零时的特例。

### 5.2 Green 第二恒等式

对 Green 第一恒等式，分别写出：

$$ \int_\Omega u\Delta v\,dV = \int_{\partial\Omega}u\frac{\partial v}{\partial n}\,dS -\int_\Omega\nabla u\cdot\nabla v\,dV,$$

$$ \int_\Omega v\Delta u\,dV = \int_{\partial\Omega}v\frac{\partial u}{\partial n}\,dS -\int_\Omega\nabla v\cdot\nabla u\,dV.$$

两式相减，中间的梯度内积项相消：

$$ \int_\Omega(u\Delta v-v\Delta u)\,dV = \int_{\partial\Omega} \left( u\frac{\partial v}{\partial n} -v\frac{\partial u}{\partial n} \right)dS.$$

这就是 Green 第二恒等式。

它在 Laplace 方程、Poisson 方程的唯一性证明、Green 函数与边界积分表示中非常重要。

## §6 所有公式的统一框架

从更高的数学视角看，以上公式都可以归结为广义 Stokes 定理：

$$ \int_M d\omega=\int_{\partial M}\omega. $$

这里$M$ 是有向流形，$\omega$ 是适当次数的微分形式，$d\omega$ 是其外微分。

这揭示了它们的共同结构：对微分形式求导后在区域上积分 = 原微分形式在边界上积分

### 最后：偏微分方程中建议优先掌握的三个公式

$$ \begin{aligned} \text{Gauss：} &\int_\Omega\nabla\cdot\mathbf F\,dV =\int_{\partial\Omega}\mathbf F\cdot\mathbf n\,dS,\\[8pt] \text{Green 第一恒等式：} &\int_\Omega u\Delta v\,dV =\int_{\partial\Omega}u\partial_n v\,dS -\int_\Omega\nabla u\cdot\nabla v\,dV,\\[8pt] 
\text{Green 第二恒等式：}&\int_\Omega(u\Delta v-v\Delta u)\,dV =\int_{\partial\Omega}(u\partial_n v-v\partial_n u)\,dS. \end{aligned} $$

建议不要把 Green 第一、第二恒等式作为孤立公式记忆。 只要记住

$$ \nabla\cdot(u\nabla v) =\nabla u\cdot\nabla v+u\Delta v, $$

再配合 Gauss 散度定理，就能随时推导出两条 Green 恒等式。这比机械记忆更可靠，也更适合后续学习能量方法、弱解和 Sobolev 空间。

1. 梯度（Gradient） $\nabla u= \left(\frac{\partial u}{\partial x}, \frac{\partial u}{\partial y}, \frac{\partial u}{\partial z}\right)$
2. 散度（Divergence） $\nabla\cdot\mathbf F =\frac{\partial P}{\partial x} +\frac{\partial Q}{\partial y} +\frac{\partial R}{\partial z}$
3. 旋度（Curl） $\nabla\times\mathbf F= \begin{vmatrix} \mathbf i&\mathbf j&\mathbf k\\ \partial_x&\partial_y&\partial_z\\ P&Q&R \end{vmatrix} = \begin{pmatrix} R_y-Q_z\\ P_z-R_x\\ Q_x-P_y \end{pmatrix}$
4. Laplace 算子（Laplacian） $\Delta u=\nabla\cdot\nabla u =u_{xx}+u_{yy}+u_{zz}$

5. Newton–Leibniz 公式 $\int_a^b f'(x)\,dx=f(b)-f(a)$
6. Green 公式（二维环流形式） $\oint_{\partial D}P\,dx+Q\,dy = \iint_D(Q_x-P_y)\,dx\,dy$
7. Green 公式（二维通量形式） $\oint_{\partial D}P\,dy-Q\,dx = \iint_D(P_x+Q_y)\,dx\,dy$
8. Gauss 散度定理（三维） $\iiint_\Omega\nabla\cdot\mathbf F\,dV = \iint_{\partial\Omega}\mathbf F\cdot\mathbf n\,dS$
	- 等价形式：$\iiint_\Omega(P_x+Q_y+R_z)\,dV = \iint_{\partial\Omega} (Pn_x+Qn_y+Rn_z)\,dS$
9. Stokes 公式（三维） $\oint_{\partial S}\mathbf F\cdot d\mathbf r = \iint_S(\nabla\times\mathbf F)\cdot\mathbf n\,dS$
	- 等价形式：$\oint_{\partial S}P\,dx+Q\,dy+R\,dz = \iint_S \begin{vmatrix} dy\,dz&dz\,dx&dx\,dy\\ \partial_x&\partial_y&\partial_z\\ P&Q&R \end{vmatrix}$

10. Green 第一恒等式 $\int_\Omega u\Delta v\,dV = \int_{\partial\Omega}u\frac{\partial v}{\partial n}\,dS -\int_\Omega\nabla u\cdot\nabla v\,dV$
	- 特别地，令 $v=u$：$\int_\Omega u\Delta u\,dV = \int_{\partial\Omega}u\frac{\partial u}{\partial n}\,dS -\int_\Omega|\nabla u|^2\,dV$
11. Green 第二恒等式 $\int_\Omega(u\Delta v-v\Delta u)\,dV = \int_{\partial\Omega} \left( u\frac{\partial v}{\partial n} -v\frac{\partial u}{\partial n} \right)dS$
12. 多维分部积分公式 $\int_\Omega u\,\partial_{x_i}v\,dV = \int_{\partial\Omega}uvn_i\,dS -\int_\Omega v\,\partial_{x_i}u\,dV$
13. 向量形式的分部积分 

14. 梯度乘积法则 $\nabla(uv)=u\nabla v+v\nabla u$
15. 散度乘积法则 $\nabla\cdot(u\mathbf F) =\nabla u\cdot\mathbf F+u\nabla\cdot\mathbf F$
16. Laplace 乘积法则 $\Delta(uv)=u\Delta v+v\Delta u+2\nabla u\cdot\nabla v$
17. 旋度乘积法则 $\nabla\times(u\mathbf F) =\nabla u\times\mathbf F+u(\nabla\times\mathbf F)$
18. 梯度的旋度恒为零 $\nabla\times(\nabla u)=\mathbf0$
19. 旋度的散度恒为零 $\nabla\cdot(\nabla\times\mathbf F)=0$
20. 向量 Laplace 恒等式 $\nabla\times(\nabla\times\mathbf F) =\nabla(\nabla\cdot\mathbf F)-\Delta\mathbf F$

21. 广义Stocks公式 $\int_M d\omega=\int_{\partial M}\omega$