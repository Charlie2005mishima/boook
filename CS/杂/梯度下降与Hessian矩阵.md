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

**※【定义】** Adam（adaptive moment estimation）
设参数向量为 $\theta_t\in\mathbb{R}^n$ 其中 $n$ 是参数个数。
第 $t$ 步的梯度也是同维向量：$g_t=\nabla L(\theta_{t-1})\in\mathbb{R}^n$
即 $g_t=(g_{t,1},g_{t,2},\dots,g_{t,n})$
Adam 维护的两个状态也是同维向量：
- **一阶矩向量**：$m_t\in\mathbb{R}^n,\quad m_t=(m_{t,1},\dots,m_{t,n})$.
- **二阶矩向量**：$v_t\in\mathbb{R}^n,\quad v_t=(v_{t,1},\dots,v_{t,n})$.
- 初始化：$m_0=\mathbf{0},\quad v_0=\mathbf{0}$, 其中 $\mathbf{0}$ 是 $n$ 维零向量。超参数 $\eta,\beta_1,\beta_2,\epsilon$ 仍然是标量。

- 一阶矩更新：$m_t=\beta_1 m_{t-1}+(1-\beta_1)g_t$.
	- 等价于对每个坐标 $i$：$m_{t,i}=\beta_1 m_{t-1,i}+(1-\beta_1)g_{t,i}$
- 二阶矩更新：$v_t=\beta_2 v_{t-1}+(1-\beta_2)g_t\odot g_t$.
	- 其中 $\odot$ 表示逐元素相乘。对每个坐标：$v_{t,i}=\beta_2 v_{t-1,i}+(1-\beta_2)g_{t,i}^2$.
- 偏差校正
	- $\hat m_t=\frac{m_t}{1-\beta_1^t}$
	- $\hat v_t=\frac{v_t}{1-\beta_2^t}$
	- 这里的除法是逐元素除法

- 参数更新：$\theta_t=\theta_{t-1}-\eta\frac{\hat m_t}{\sqrt{\hat v_t}+\epsilon}$
	- 其中：
		- $\sqrt{\hat v_t}$ 是逐元素开方；
		- 分母加 $\epsilon$ 是给每个坐标都加 $\epsilon$；
		- 除法是逐元素除法。

- 对每个坐标 $i$：$\theta_{t,i} = \theta_{t-1,i} - \eta\frac{\hat m_{t,i}}{\sqrt{\hat v_{t,i}}+\epsilon}$.

向量形式下，Adam 核心是：**每个参数坐标有自己的一阶矩、二阶矩和有效学习率**。
第 $i$ 个参数的有效更新步长大致是：$\eta\frac{\hat m_{t,i}}{\sqrt{\hat v_{t,i}}+\epsilon}$
其中：
- $\hat m_{t,i}$ 决定第 $i$ 个参数往哪个方向走、走多快；
- $\sqrt{\hat v_{t,i}}$ 是第 $i$ 个参数历史梯度大小的度量；
- 如果某个参数历史梯度一直很大，$\hat v_{t,i}$ 大，它的有效学习率会被压低；
- 如果某个参数梯度很小或很稀疏，$\hat v_{t,i}$ 小，它可能获得相对更大的更新。

所以 Adam 不是所有参数共享一个学习率，而是**逐参数自适应学习率**。矩阵或高维张量参数也可以 flatten 成向量，或者直接逐元素做同样运算。
