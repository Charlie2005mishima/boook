
## 3.1 forward and back substitutions

**※【定义】** 前代法(Forward substitution)：
设 $L \in \mathbb{R}^{n \times n}$ 为非奇异下三角矩阵，前代法是求解线性方程组 $L\mathbf{x}=\mathbf{b}$ 的算法，按 $x_1, x_2, \dots, x_n$ 的顺序依次求解：
$x_i = \frac{1}{l_{ii}} \left( b_i - \sum_{j=1}^{i-1} l_{ij} x_j \right), \quad \forall i = 1, 2, \dots, n$ (3.1)

**※【定义】** 回代法(Back substitution)：
设 $U \in \mathbb{R}^{n \times n}$ 为非奇异上三角矩阵，回代法是求解线性方程组 $U\mathbf{x}=\mathbf{b}$ 的算法，按 $x_n, x_{n-1}, \dots, x_1$ 的顺序依次求解：
$x_i = \frac{1}{u_{ii}} \left( b_i - \sum_{j=i+1}^{n} u_{ij} x_j \right), \quad \forall i = n, n-1, \dots, 1$ (3.2)

**※【例题】** 考虑如下下三角方程组：
$\begin{bmatrix} l_{11} & 0 \\ l_{21} & l_{22} \end{bmatrix} \begin{bmatrix} x_1 \\ x_2 \end{bmatrix} = \begin{bmatrix} b_1 \\ b_2 \end{bmatrix}$。
若 $l_{11}l_{22} \neq 0$，则可依次求得未知量：
$x_1 = \frac{b_1}{l_{11}}$，
$x_2 = \frac{b_2 - l_{21}x_1}{l_{22}}$。

**※【定理】** 三角系统的向后误差(backward error of triangular systems)：
通过前代法和回代法得到的计算解 $\hat{\mathbf{x}}$ 分别满足
$(L + F)\hat{\mathbf{x}} = \mathbf{b}, \quad |F| \leq n\epsilon_u |L| + O(\epsilon_u^2)$ (3.3)
和
$(U + F)\hat{\mathbf{x}} = \mathbf{b}, \quad |F| \leq n\epsilon_u |U| + O(\epsilon_u^2)$ (3.4)
其中 $\epsilon_u$ 是定义 1.28 中的单位舍入误差。

**证明**：仅证明 (3.3)，(3.4) 类似。
令 $\hat{s} = \mathrm{fl}\left(b_i - \sum_{j=1}^{i-1} l_{ij}\hat{x}_j\right)$，
由定义 3.3 和定理 1.42 可得存在 $\epsilon_j, \delta_k$ ($j,k=1,2,\dots,i-1$)，$|\epsilon_j|, |\delta_k| < \epsilon_u$，
使得 $\hat{s} = \mathrm{fl}\left(b_i - \sum_{j=1}^{i-1} \mathrm{fl}(l_{ij}\hat{x}_j)\right)$
$= b_i \prod_{k=1}^{i-1}(1+\delta_k) - \sum_{j=1}^{i-1} l_{ij}\hat{x}_j (1+\epsilon_j) \prod_{k=j}^{i-1}(1+\delta_k)$。
于是，
$\hat{x}_i = \mathrm{fl}\left(\frac{\hat{s}}{l_{ii}}\right) = \frac{\hat{s}}{l_{ii}}(1+\delta_i) = \frac{b_i}{l_{ii}} \prod_{k=1}^{i}(1+\delta_k) - \sum_{j=1}^{i-1} \frac{l_{ij}}{l_{ii}} \hat{x}_j (1+\epsilon_j) \prod_{k=j}^{i}(1+\delta_k)$
$\Leftrightarrow b_i = l_{ii}\hat{x}_i \frac{1}{\prod_{k=1}^{i}(1+\delta_k)} + \sum_{j=1}^{i-1} l_{ij}\hat{x}_j \frac{(1+\epsilon_j)}{\prod_{k=1}^{j-1}(1+\delta_k)}$ (3.5)
其中 $|\delta_i| < \epsilon_u$。

由定理 1.30 和定理 1.43 的证明可知，
存在 $\mu_{ij}$ ($j=1,2,\dots,i$)，$|\mu_{ij}| < j\epsilon_u + O(\epsilon_u^2)$，
使得 $(1+\mu_{ij}) = \begin{cases} \frac{1}{\prod_{k=1}^{i}(1+\delta_k)} & \text{若 } j=i; \\ \frac{(1+\epsilon_j)}{\prod_{k=1}^{j-1}(1+\delta_k)} & \text{若 } j \neq i. \end{cases}$

令 $f_{ij} = l_{ij}\mu_{ij}$，则
$\sum_{j=1}^{i} l_{ij}(1+\mu_{ij})\hat{x}_j = \sum_{j=1}^{i} (l_{ij} + f_{ij})\hat{x}_j = b_i$
$|f_{ij}| = |\mu_{ij}||l_{ij}| \leq j\epsilon_u |l_{ij}| + O(\epsilon_u^2)$ (3.6)
将 (3.6) 重写为矩阵形式并注意 $j \leq n$，即证。$\square$

**※【定理】** 三角系统的向前误差(Forward error of triangular systems)：
设三角系统 $T\mathbf{x}=\mathbf{b}$，其中 $T \in \mathbb{R}^{n \times n}$ 非奇异。
通过代入法得到的计算解 $\hat{\mathbf{x}}$ 满足 $\frac{\|\mathbf{x} - \hat{\mathbf{x}}\|_\infty}{\|\mathbf{x}\|_\infty} \leq \frac{\kappa(T, \mathbf{x})\gamma_n}{1 - \kappa(T)\gamma_n}$ (3.7)
其中
$\kappa(T, \mathbf{x}) = \frac{\left\| |T^{-1}||T||\mathbf{x}| \right\|_\infty}{\|\mathbf{x}\|_\infty}$，
$\kappa(T) = \left\| |T^{-1}||T| \right\|_\infty$，
$\gamma_n = \frac{n\epsilon_u}{1 - n\epsilon_u}$。

**※【例题】** $\kappa(T, \mathbf{x})$ 可以任意大。上三角矩阵
$U(\alpha) = (u_{ij}), \quad u_{ij} = \begin{cases} 1, & \text{若 } i=j; \\ -\alpha, & \text{若 } i<j, \end{cases}$
其逆为
$(U(\alpha)^{-1})_{ij} = \begin{cases} 1, & \text{若 } i=j; \\ \alpha(1+\alpha)^{j-i-1}, & \text{若 } j>i. \end{cases}$
当 $\alpha \to \infty$ 时，$\kappa(U(\alpha), \mathbf{e}) = \kappa(U(\alpha)) \sim 2\alpha^{n-1}$。因此不能断言所有三角系统都能高精度求解。

**※【引理】** 设上三角矩阵 $U \in \mathbb{R}^{n \times n}$ 满足
$\forall j > i, \quad |u_{ii}| > |u_{ij}|$ (3.8)
则单位上三角矩阵 $W = |U^{-1}||U|$ 满足 $w_{ij} \leq 2^{j-i}$ 对所有 $j>i$ 成立，从而 $\kappa(U) \leq 2^n - 1$。

**证明**：可写 $W = |V^{-1}||V|$，其中 $V = D^{-1}U$，$D = \mathrm{diag}(u_{ii})$。矩阵 $V$ 是单位上三角矩阵且 $v_{ij} \leq 1$，易证 $|(V^{-1})_{ij}| \leq 2^{j-i-1}$ ($j>i$)。因此对 $j>i$，
$w_{ij} = \sum_{k=i}^{j} |(V^{-1})_{ik}| v_{kj} \leq 1 + \sum_{k=i+1}^{j} 2^{k-i-1} \cdot 1 = 2^{j-i}$。$\square$

**※【定理】** 在引理 3.9 的条件下，通过代入法求解 $U\mathbf{x}=\mathbf{b}$ 得到的计算解 $\hat{\mathbf{x}}$ 满足
$\forall i = 1, \dots, n, \quad |x_i - \hat{x}_i| \leq 2^{n-i+1} n\epsilon_u \max_{j \geq i} |\hat{x}_j| + O(\epsilon_u^2)$ (3.9)

**证明**：由定理 3.6 有 $|\mathbf{x} - \hat{\mathbf{x}}| = |U^{-1}F\hat{\mathbf{x}}| \leq n\epsilon_u |U^{-1}||U||\hat{\mathbf{x}}| + O(\epsilon_u^2)$。应用引理 3.9 可得
$|x_i - \hat{x}_i| \leq n\epsilon_u \sum_{j=i}^{n} w_{ij}|\hat{x}_j| + O(\epsilon_u^2)$
$\leq n\epsilon_u \max_{j \geq i} |\hat{x}_j| \sum_{j=i}^{n} 2^{j-i} + O(\epsilon_u^2)$
$\leq 2^{n-i+1} n\epsilon_u \max_{j \geq i} |\hat{x}_j| + O(\epsilon_u^2)$。$\square$

**※【定理】** 
对于满足 $\forall j < i, \quad |l_{ii}| \geq |l_{ij}|$ (3.10)
的下三角矩阵 $L$，引理 3.9 和定理 3.10 的类似结论成立。

**※【例题】** 上三角矩阵 $T$ 满足 (3.8) 并不意味着 $T^\top$ 满足 (3.10)。事实上，$\kappa(T^\top)$ 可以任意大，例如：
$T = \begin{bmatrix} 1 & 1 & 0 \\ 0 & \epsilon & \epsilon \\ 0 & 0 & 1 \end{bmatrix}$，
$\kappa(T) = 5, \quad \kappa(T^\top) = 1 + \frac{2}{\epsilon}$。

## 3.2 高斯消元法

**※【符号】** 书写（稀疏）矩阵时，对角线左侧的零几乎总是省略不写，除非显示某些零能清楚地表明矩阵结构。省略号 $\vdots, \cdot, \cdots$ 表示由连接的两个元素可明显看出的模式。

### 3.2.1 高斯变换

**※【定义】** 高斯变换(Gauss transformations)：
高斯变换是形如
$L_k := I_n - \ell_k \mathbf{e}_k^\top = \begin{bmatrix} 1 & & & & & \\ & \ddots & & & & \\ & & 1 & & & \\ & & -l_{k+1,k} & 1 & & \\ & & \vdots & & \ddots & \\ & & -l_{n,k} & & & 1 \end{bmatrix}$ (3.11)
的方阵，其中 $I_n \in \mathbb{R}^{n \times n}$ 是单位矩阵，高斯向量 $\ell_k \in \mathbb{R}^n$ 的前 $k$ 个分量为零，$\mathbf{e}_k$ 是第 $k$ 个分量为 1、其余为 0 的向量。高斯向量的非零分量称为乘子(multipliers)。

**※【定理】** 设 $C \in \mathbb{R}^{n \times r}$，$L_k = I_n - \ell_k \mathbf{e}_k^\top$ 为高斯变换。则
$L_k C = (I_n - \ell_k \mathbf{e}_k^\top)C = C - \ell_k C(k,:)$ (3.12)
是一个外积更新(outer product update)。
若 $\hat{\ell}_k$ 是精确高斯向量 $\ell_k$ 的计算版本，则
$\hat{\ell}_k = \ell_k + \mathbf{e}, \quad |\mathbf{e}| \leq \epsilon_u |\ell_k|$ (3.13)
若在更新中使用 $\hat{\ell}_k$，且 $\mathrm{fl}((I_n - \ell_k \mathbf{e}_k^\top)C)$ 表示计算结果，则
$\mathrm{fl}((I_n - \ell_k \mathbf{e}_k^\top)C) = (I_n - \ell_k \mathbf{e}_k^\top)C + E$，
$|E| \leq 3\epsilon_u (|C| + |\ell_k| C(k,:)) + O(\epsilon_u^2)$ (3.14)

**※【定义】** 高斯消元法(Gaussian elimination)：
由一系列行运算组成，将矩阵 $A \in \mathbb{R}^{n \times m}$ 变换为上三角矩阵 $U$。在第 $k$ 步（$k=1,\dots,n-1$），使用主元 $a_{kk}^{(k)}$ 消去 $i>k$ 的元素 $a_{ik}^{(k)}$。
第 $k$ 步的变换为
$A^{(k+1)} = L_k A^{(k)} = A^{(k)} - \ell_k A^{(k)}(k,:)$ (3.15)
其中 $L_k$ 是高斯变换，乘子由下式给出
$\forall i = k+1, \dots, n, \quad l_{ik} = \frac{a_{ik}^{(k)}}{a_{kk}^{(k)}}$ (3.16)
$A^{(k)}$ 是经过第 $(k-1)$ 次变换后的矩阵，$A^{(1)}=A$。此外，假设 $a_{kk}^{(k)} \neq 0$。

**※【例题】** 对列向量 $\alpha = [a_{1k}^{(k)}, \dots, a_{nk}^{(k)}]^\top$，令 $\ell_k = [0, \dots, 0, l_{k+1,k}, \dots, l_{nk}]^\top$，其中 $l_{ik}$ 同 (3.16)，则
$L_k \alpha = \begin{bmatrix} 1 & & & & & \\ & \ddots & & & & \\ & & 1 & & & \\ & & -l_{k+1,k} & 1 & & \\ & & \vdots & & \ddots & \\ & & -l_{n,k} & & & 1 \end{bmatrix} \begin{bmatrix} a_{1k}^{(k)} \\ \vdots \\ a_{kk}^{(k)} \\ a_{k+1,k}^{(k)} \\ \vdots \\ a_{nk}^{(k)} \end{bmatrix} = \begin{bmatrix} a_{1k}^{(k)} \\ \vdots \\ a_{kk}^{(k)} \\ 0 \\ \vdots \\ 0 \end{bmatrix}$。

**※【例题】** 对 $A \in \mathbb{R}^{4 \times 4}$ 应用高斯消元法：
$\begin{bmatrix} \times & \times & \times & \times \\ \times & \times & \times & \times \\ \times & \times & \times & \times \\ \times & \times & \times & \times \end{bmatrix} \xrightarrow{L_1} \begin{bmatrix} \times & \times & \times & \times \\ 0 & \times & \times & \times \\ 0 & \times & \times & \times \\ 0 & \times & \times & \times \end{bmatrix} A^{(2)}$
$\xrightarrow{L_2} \begin{bmatrix} \times & \times & \times & \times \\ & \times & \times & \times \\ & 0 & \times & \times \\ & 0 & \times & \times \end{bmatrix} \xrightarrow{L_3} \begin{bmatrix} \times & \times & \times & \times \\ & \times & \times & \times \\ & & \times & \times \\ & & 0 & \times \end{bmatrix}$。

### 3.2.2 LU 分解

**※【引理】** 设 $A \in \mathbb{R}^{n \times n}$ 非奇异。主元 $a_{kk}^{(k)}$ 全部非零当且仅当 $A$ 的 $i$ 阶顺序主子式 $A_i$ 对每个 $i=1,2,\dots,n$ 都非奇异。

**※【定理】** LU 分解(LU factorization)：
设 $A \in \mathbb{R}^{n \times n}$。若 $\forall i=1,2,\dots,n$，$A$ 的 $i$ 阶顺序主子式 $A_i$ 非奇异，则存在单位下三角矩阵 $L$ 和上三角矩阵 $U$ 使得
$A = LU$ (3.17)
此外，$L$ 对角线以下的元素即为乘子，即 $(L)_{ik} = l_{ik}$ 对所有 $i>k$ 成立，且 $U = A^{(n)}$。

**证明**：由定理 3.19，可对 $A$ 应用高斯消元法。由 (3.15) 得
$U = A^{(n)} = L_{n-1} \dots L_2 L_1 A \implies A = L_1^{-1} L_2^{-1} \dots L_{n-1}^{-1} U$。
$\ell_k$ 的稀疏模式意味着对所有 $i \leq k$ 有 $\mathbf{e}_i^\top \ell_k = 0$。因此 $(I - \ell_k \mathbf{e}_k^\top)(I + \ell_k \mathbf{e}_k^\top) = I - \ell_k \mathbf{e}_k^\top \ell_k \mathbf{e}_k^\top = I$，即 $L_k^{-1} = I + \ell_k \mathbf{e}_k^\top$。进一步，$L_k^{-1} L_{k+1}^{-1} = \dots$ （注：原文此处截断）。