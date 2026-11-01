
### 71 Jordan标准形的几何意义
设 $A$ 的特征值为 $\lambda$。广义特征子空间为 $\mathcal G_\lambda=\ker(A-\lambda I)^n$，并且复向量空间分解为各广义特征子空间的直和。每个 Jordan 块对应一条广义特征向量链：
$$ (A-\lambda I)v_1=0,\quad (A-\lambda I)v_{j+1}=v_j. $$
块的个数等于特征值的几何重数，所有块的阶数之和等于其代数重数。矩阵可对角化当且仅当所有 Jordan 块均为 $1\times1$。
### 72 Jordan标准形的应用
若 $A=PJP^{-1}$，则对任意多项式 $f$ 有 $f(A)=Pf(J)P^{-1}$。对 Jordan 块 $J_k(\lambda)=\lambda I+N$（$N^k=0$），
$$f(J_k(\lambda))=\sum_{r=0}^{k-1}\frac{f^{(r)}(\lambda)}{r!}N^r.$$
这给出了求 $A^m$、$e^{tA}$ 及线性微分方程 $x'=Ax$ 的系统方法。

幂等矩阵 $A^2=A$ 的最小多项式整除 $x(x-1)$，故必可对角化，特征值只可能为 $0,1$，并有 $\operatorname{rank}A=\operatorname{tr}A$。幂零矩阵 $A^k=0$ 的全部特征值为 $0$，其 Jordan 块均为 $J_r(0)$。
### 73方阵幂级数和方阵函数
幂级数 $\sum c_kA^k$ 若绝对收敛，即定义方阵函数；特别地 $e^A=\sum_{k\ge0}A^k/k!$。若 $A,B$ 可交换，则 $e^{A+B}=e^Ae^B$；一般情形不能拆开。常系数系统 $x'=Ax$ 的解为 $x(t)=e^{tA}x(0)$。
### 74方阵在复相似下的标准形
有理标准形不要求特征多项式在底域分裂：由不变因子对应的 Frobenius 伴随矩阵块构成。扩张到复数域后，每个不可约因子分裂，便可进一步得到 Jordan 标准形。


### 特征值与特征向量的求解
特征值 $\lambda$ 满足特征方程：$\det(A - \lambda I) = 0$, 其中 $I$ 是单位矩阵。这是一个关于 $\lambda$ 的多项式方程（特征多项式）。
对于每个特征值 $\lambda_i$，解齐次线性方程组：$(A - \lambda_i I) \mathbf{v} = \mathbf{0}$, 得到的非零解向量就是对应于 $\lambda_i$ 的特征向量。
设 $\lambda = \alpha + i \beta$（其中 $\alpha, \beta \in \mathbb{R}$，且 $\beta \neq 0$）是一个复数特征值，其对应的特征向量 $\mathbf{v}$ 是一个复向量。通常我们将 $\mathbf{v}$ 写成实部和虚部之和：$\mathbf{v} = \mathbf{u} + i \mathbf{w},$ 其中 $\mathbf{u}$ 和 $\mathbf{w}$ 都是实向量。
当我们求解$\mathbf{P}^{-1}\mathbf{A}\mathbf{P}=diag\{\lambda_{1},\lambda_{2},\dots,\lambda _{n}\}$，$P=(\vec{v}_{1},\vec{v}_{2},\dots,\vec{v}_{n})$
其中前提是能够取到 $n$ 个线性无关的特征向量；重特征值时必须比较代数重数与几何重数，不能仅凭“有特征值”便断言可对角化。
