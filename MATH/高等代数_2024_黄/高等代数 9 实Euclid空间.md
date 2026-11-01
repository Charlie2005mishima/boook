
### 91双线性函数
实双线性型 $B(x,y)$ 满足对每个变量分别线性。若 $B(x,y)=B(y,x)$，则为对称双线性型，且可写为 $x^TAy$（$A=A^T$）。由二次型 $q(x)=B(x,x)$ 可用极化公式恢复：$B(x,y)=\tfrac12[q(x+y)-q(x)-q(y)]$。
### 92实Euclid空间
实 Euclid 空间是带内积 $\langle\cdot,\cdot\rangle$ 的实线性空间，范数为 $\|x\|=\sqrt{\langle x,x\rangle}$。Cauchy–Schwarz：$|\langle x,y\rangle|\le\|x\|\|y\|$；正交向量组满足 Pythagoras 定理。

Gram–Schmidt 正交化可将线性无关组化为标准正交组。若 $W$ 有标准正交基 $q_1,\ldots,q_r$，则 $\operatorname{proj}_W x=\sum_i\langle x,q_i\rangle q_i$，且 $x-\operatorname{proj}_W x\perp W$。

对 $A\in\mathbb R^{m\times n}$，有 $\operatorname{Row}(A)^\perp=\ker A$、$\operatorname{Col}(A)^\perp=\ker A^T$。满列秩矩阵可作 QR 分解 $A=QR$，其中 $Q^TQ=I$、$R$ 为上三角矩阵；它常用于稳定地求解最小二乘问题。
### 93实方阵在实正交相似下的标准形
实正规矩阵（$AA^T=A^TA$）可经实正交相似化为由 $1\times1$ 实块与 $\begin{pmatrix}a&-b\\b&a\end{pmatrix}$ 块组成的分块对角阵。正交相似保持内积、范数、迹、行列式与特征多项式。
### 94实对称方阵的特征根*
谱定理：实对称矩阵 $A$ 存在正交矩阵 $Q$，使 $Q^TAQ=\operatorname{diag}(\lambda_1,\ldots,\lambda_n)$，且 $\lambda_i\in\mathbb R$。不同特征值的特征向量正交；正定性等价于全部特征值为正。

正交矩阵 $Q$ 满足 $Q^TQ=I$，故 $Q^{-1}=Q^T$、$\det Q=\pm1$，并保持内积、长度和距离。注意谱定理的正确结论是 $Q^TAQ=\Lambda$，而不是一般地等于 $A$。
### 95实线性不等式*
实线性不等式组 $Ax\le b$ 的可行域是凸多面体。Farkas 引理给出“可行”与“存在非负线性组合构成矛盾”的替代关系，是线性规划对偶理论的基本工具。
