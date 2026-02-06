【定义】Hessian 矩阵
$H_{f}=\begin{bmatrix}\frac{ \partial^{2}f }{ \partial x^{2} } & \frac{ \partial^{2} f  }{ \partial x\partial y } & \dots \\\frac{ \partial^{2} f  }{ \partial x\partial y } & \frac{ \partial^{2}f }{ \partial y^{2} }  & \dots \\ \dots\end{bmatrix}$ 

【定理】Hessian 矩阵的性质
1. 实对称矩阵：$H_{ij}=H_{ji}\implies$ 可对角化
2. $\forall\,v\in \mathbb{R}^n,\parallel v\parallel=1\implies v^THv\leq\lambda_{max}$ 

【定义】Condition Number of Hessian
$\max_{i,j} \frac{\lambda_{i}}{\lambda_{j}}$

【推论】最佳学习率下界
$\begin{align}f(x)&\approx f(x^{(0)})+(x-x^{(0)})^Tg+\frac{1}{2}(x-x^{(0)})^TH(x-x^{(0)})\\f(x^{(0)}-\varepsilon g)&\approx f(x^{(0)})-\varepsilon g^Tg+\frac{1}{2}\varepsilon^{2}g^THg\end{align}$
当$g^THg<0$，$\varepsilon$会使$f(x)$减小，但$\varepsilon$太大会使Taylor失效
当$g^THg>0$，增大$\varepsilon$未必使得损失函数增大或减小
最佳学习率 $\varepsilon^*= \frac{g^Tg}{g^THg}\geq\frac{g^Tg}{g^Tg\lambda_{max}}=\frac{1}{\lambda_{max}}\implies$ hessian 矩阵决定最佳学习率下界