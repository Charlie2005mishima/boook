## 2.1 向前误差与向后误差

**※【定义】** 绝对误差(absolute error)与相对误差(relative error)：
设 $\hat{x}$ 是 $x \in \mathbb{R}$ 的近似值。其精度可由绝对误差衡量：
$E_{\mathrm{abs}}(\hat{x}) = |\hat{x} - x|$ (2.1)
当 $x \neq 0$ 时，也可由相对误差衡量：
$E_{\mathrm{rel}}(\hat{x}) = \frac{|\hat{x} - x|}{|x|}$ (2.2)

**※【定义】** 向前误差(forward error)与向后误差(backward error)：
对于由 $\hat{y} = \hat{f}(x)$ 计算的 $y = f(x)$ 的近似 $\hat{y}$：
- **向前误差**是 $\hat{y}$ 近似 $y$ 的相对误差。
- **向后误差**是满足 $f(\hat{x}) = \hat{f}(x)$ 的 $\hat{x}$ 近似 $x$ 的最小相对误差（假设这样的 $\hat{x}$ 存在）。

**※【定义】** 精度(Accuracy)：
计算函数 $y = f(x)$ 的算法 $\hat{y} = \hat{f}(x)$ 是精确的，若其向前误差对所有 $x$ 都很小，即 $\forall x \in \mathrm{dom}(f)$，$E_{\mathrm{rel}}(\hat{f}(x)) \leq c\epsilon_u$，其中 $c$ 是小常数。

## 2.2 向后稳定性与数值稳定性

**※【定义】** 向后稳定(Backward stability)：
计算 $y = f(x)$ 的算法 $\hat{f}(x)$ 是向后稳定的，若其向后误差对所有 $x$ 都很小，即
$\forall x \in \mathrm{dom}(f), \exists \hat{x} \in \mathrm{dom}(f), \text{ s.t. } \hat{f}(x) = f(\hat{x}) \implies E_{\mathrm{rel}}(\hat{x}) \leq c\epsilon_u$ (2.3)
其中 $c$ 是小常数。

**※【定义】** 多变量向后稳定：
计算 $y = f(x_1, x_2)$ 的算法 $\hat{f}(x_1, x_2)$ 是向后稳定的，若
$\forall (x_1, x_2) \in \mathrm{dom}(f), \exists (\hat{x}_1, \hat{x}_2) \in \mathrm{dom}(f) \text{ s.t. } \hat{f}(x_1, x_2) = f(\hat{x}_1, \hat{x}_2) \implies \begin{cases} E_{\mathrm{rel}}(\hat{x}_1) \leq c_1\epsilon_u, \\ E_{\mathrm{rel}}(\hat{x}_2) \leq c_2\epsilon_u, \end{cases}$ (2.4)
其中 $c_1, c_2$ 是两个小常数。

**※【引理】** 对于 $f(x_1, x_2) = x_1 - x_2$，$x_1, x_2 \in \mathcal{R}(\mathcal{F})$，算法 $\hat{f}(x_1, x_2) = \mathrm{fl}(\mathrm{fl}(x_1) - \mathrm{fl}(x_2))$ 是向后稳定的。

**证明**：由定理 1.42，$\hat{f}(x_1, x_2) = (\mathrm{fl}(x_1) - \mathrm{fl}(x_2))(1 + \delta_3)$。由定理 1.29：
$\hat{f}(x_1, x_2) = (x_1(1 + \delta_1) - x_2(1 + \delta_2))(1 + \delta_3) = x_1(1 + \delta_1 + \delta_3 + \delta_1\delta_3) - x_2(1 + \delta_2 + \delta_3 + \delta_2\delta_3)$。
取 $\hat{x}_1$ 和 $\hat{x}_2$ 为上述两项，则 $E_{\mathrm{rel}}(\hat{x}_1) = |\delta_1 + \delta_3 + \delta_1\delta_3|$，$E_{\mathrm{rel}}(\hat{x}_2) = |\delta_2 + \delta_3 + \delta_2\delta_3|$。由定义 2.6 即证。$\square$

**※【例题】** 对于 $f(x) = 1 + x$，$x \in (0, \mathrm{OFL})$，算法 $\hat{f}(x) = \mathrm{fl}(1.0 + \mathrm{fl}(x))$ 不是向后稳定的。
**证明**：对于每个 $x \in (0, \epsilon_u)$，由定义 1.26 得 $\hat{f}(x) = 1.0$。则 $\hat{f}(x) = f(\hat{x})$ 意味着 $\hat{x} = 0$，进而 $E_{\mathrm{rel}}(\hat{x}) = 1$。$\square$

**※【定义】** 数值稳定(Numerically stable)：
计算 $y = f(x)$ 的算法 $\hat{f}(x)$ 是稳定的或数值稳定的，当且仅当
$\forall x \in \mathrm{dom}(f), \exists \hat{x} \in \mathrm{dom}(f) \text{ s.t. } \begin{cases} \left| \frac{\hat{f}(x) - f(\hat{x})}{f(\hat{x})} \right| \leq c_f\epsilon_u, \\ E_{\mathrm{rel}}(\hat{x}) \leq c\epsilon_u, \end{cases}$ (2.5)
其中 $c_f, c$ 是两个小常数。

**※【引理】** 若一个算法是向后稳定的，则它是数值稳定的。
**证明**：由定义 2.6，$f(\hat{x}) = \hat{f}(x)$，故 $c_f = 0$。另一条件也显然成立。$\square$

**※【例题】** 对于 $f(x) = 1 + x$，$x \in (0, \mathrm{OFL})$，算法 $\hat{f}(x) = \mathrm{fl}(1.0 + \mathrm{fl}(x))$ 是数值稳定的。

## 2.3 标量函数的条件数

**※【定义】** 绝对条件数(absolute condition number)：
函数 $f : \mathbb{R} \to \mathbb{R}$ 的绝对条件数为
$\hat{C}_f(x) = \lim_{\delta \to 0} \sup_{|\Delta x| \leq \delta} \frac{|f(x + \Delta x) - f(x)|}{|\Delta x|}$ (2.6)

**※【定义】** 相对条件数(relative condition number)：
连续可微函数 $f : \mathbb{R} \to \mathbb{R}$ 的相对条件数是输出相对变化与输入微小变化的比值：
$C_f(x) = \left| \frac{x f'(x)}{f(x)} \right|$ (2.7)

**※【定义】** 良态(well-conditioned)与病态(ill-conditioned)：
条件数低的问题称为良态的；条件数高的问题称为病态的。

**※【例题】** 由定义 2.14 可得
$E_{\mathrm{rel}}(\hat{y}) \lesssim C_f E_{\mathrm{rel}}(\hat{x})$ (2.8)
近似符号 $\approx$ 表示忽略了二次项 $(\Delta x)^2$。这解释了病态问题的计算解可能具有较大的向前误差。

**※【例题】** 对于函数 $f(x) = \arcsin(x)$，由定义 2.14 得其条件数为
$C_f(x) = \left| \frac{x f'(x)}{f(x)} \right| = \frac{x}{\sqrt{1 - x^2} \arcsin x}$。
因此当 $x \to \pm 1$ 时，$C_f(x) \to +\infty$。

**※【引理】** 考虑在单根 $r$ 附近求解 $f(x) = 0$，即 $f(r) = 0$ 且 $f'(r) \neq 0$。假设将函数 $f$ 扰动为 $F = f + \epsilon g$，其中 $f, g \in C^2$，$g(r) \neq 0$，且 $|\epsilon g'(r)| \ll |f'(r)|$。则 $F$ 的根为 $r + h$，其中
$h \approx -\epsilon \frac{g(r)}{f'(r)}$ (2.9)

**证明**：假设 $r + h$ 是新根，即 $f(r + h) + \epsilon g(r + h) = 0$。泰勒展开得 $f(r) + h f'(r) + \epsilon[g(r) + h g'(r)] = O(h^2)$，故 $h \approx -\epsilon \frac{g(r)}{f'(r) + \epsilon g'(r)} \approx -\epsilon \frac{g(r)}{f'(r)}$。$\square$

**※【例题】** Wilkinson 小变化带来大扰动的例子：
定义 $f(x) := \prod_{k=1}^p (x - k)$，$g(x) := x^p$。根 $x = p$ 如何受 $f$ 扰动为 $f + \epsilon g$ 的影响？由引理 2.18，答案为 $h \approx -\epsilon \frac{g(p)}{f'(p)} = -\epsilon \frac{p^p}{(p-1)!}$。
对于 $p = 20, 30, 40$，$\frac{p^p}{(p-1)!}$ 分别约为 $8.6 \times 10^8$，$2.3 \times 10^{13}$，$5.9 \times 10^{17}$。因此单项式 $x^p$ 系数的微小变化会导致根的巨大变化。高次多项式求根问题是极其病态的。

## 2.4 向量函数的条件数

**※【定义】** 向量函数的条件数(condition number of a vector function)：
向量函数 $\mathbf{f} : \mathbb{R}^m \to \mathbb{R}^n$ 的条件数为
$\mathrm{cond}_{\mathbf{f}}(\mathbf{x}) = \frac{\|\mathbf{x}\| \|\nabla \mathbf{f}\|}{\|\mathbf{f}(\mathbf{x})\|}$ (2.10)
其中 $\|\cdot\|$ 表示向量范数或矩阵范数。

**※【定义】** 向量 $p$-范数：
对于向量 $\mathbf{v} \in \mathbb{F}^n$，其 $l_\infty$ 范数为 $\|\mathbf{v}\|_\infty = \max_i |v_i|$。
对于 $p \in [1, \infty)$，其 $l_p$ 范数为 $\|\mathbf{v}\|_p = (\sum_{i=1}^n |v_i|^p)^{1/p}$。

**※【定义】** 矩阵范数：
由向量 $p$-范数诱导的矩阵 $A \in \mathbb{F}^{m \times n}$ 的范数为
$\|A\|_p = \sup_{\mathbf{x} \neq 0} \frac{\|A\mathbf{x}\|_p}{\|\mathbf{x}\|_p}$。

**※【例题】** 求解线性系统 $A\mathbf{u} = \mathbf{b}$ 可视为输入 $\mathbf{b}$ 并返回输出 $A^{-1}\mathbf{b}$，即 $\mathbf{f}(\mathbf{b}) = A^{-1}\mathbf{b}$。显然 $\nabla \mathbf{f} = A^{-1}$。由定义 2.20：
$\mathrm{cond}_{\mathbf{f}}(\mathbf{x}) = \frac{\|\mathbf{b}\| \|A^{-1}\|}{\|\mathbf{u}\|} = \frac{\|A\mathbf{u}\| \|A^{-1}\|}{\|\mathbf{u}\|}$。
输入 $\mathbf{b}$ 可取任意值，故
$\max \mathrm{cond}_{\mathbf{f}}(\mathbf{x}) = \max_{\mathbf{u} \neq 0} \frac{\|A\mathbf{u}\| \|A^{-1}\|}{\|\mathbf{u}\|} = \|A\| \|A^{-1}\|$，
其中使用了常见定义 $\|A\| := \max_{\mathbf{u} \neq 0} \frac{\|A\mathbf{u}\|}{\|\mathbf{u}\|}$ (2.11)。这解释了定义 2.22。

**※【定义】** 非奇异方阵的条件数：
非奇异方阵 $A$ 的条件数为
$\mathrm{cond} A := \|A\| \|A^{-1}\|$ (2.12)

**※【引理】** 基于 2-范数，非奇异方阵 $A$ 的条件数为
$\mathrm{cond}_2 A := \|A\|_2 \|A^{-1}\|_2 = \frac{\sigma_{\max}}{\sigma_{\min}}$，
其中 $\sigma_{\max}$ 和 $\sigma_{\min}$ 分别是 $A$ 的最大和最小奇异值。
若 $A$ 还是正规矩阵，则 $\mathrm{cond}_2 A = \frac{|\lambda_{\max}|}{|\lambda_{\min}|}$，其中 $\lambda_{\max}$ 和 $\lambda_{\min}$ 分别是模最大和最小的特征值。进一步，若 $A$ 是酉矩阵，则 $\mathrm{cond} A = 1$。

**※【例题】** 考虑线性系统 $A\mathbf{x} = \mathbf{b}$，$A \in \mathbb{C}^{m \times n}$ 有解。若将 $\mathbf{b}$ 扰动为 $\hat{\mathbf{b}} = \mathbf{b} + \epsilon \mathbf{v}_j$，$\epsilon \in \mathbb{C}$，由定理 C.255，解从 $\mathbf{x}$ 变为 $\hat{\mathbf{x}} = \mathbf{x} + \frac{\epsilon}{\sigma_j} \mathbf{u}_j$。则解的变化与 $\mathbf{b}$ 的变化之比为
$\frac{\|\hat{\mathbf{x}} - \mathbf{x}\|_2}{\|\hat{\mathbf{b}} - \mathbf{b}\|_2} = \frac{1}{\sigma_j}$ (2.13)
按照定义 2.13 的精神，$\frac{1}{\sigma_{\min}}$ 可视为 $A$ 的绝对条件数。
注：这里的 $\mathbf{u}_{j},\mathbf{v}_{j}$ 是 $A=U\Sigma V$ 奇异值分解之后 $U,V$ 中的向量

**※【定理】** 线性系统的可解性：
线性系统 $A\mathbf{x} = \mathbf{b}$，$A \in \mathbb{C}^{m \times n}$ 有解当且仅当 $\mathbf{b}$ 在 $A$ 的值域中，或等价地，$\mathbf{b}$ 垂直于 $A$ 的伴随的零空间，即
$\forall z \in \{y \in \mathbb{C}^m : A^*y = 0\}, \langle \mathbf{b}, z \rangle = 0$
此时线性系统的一个特解为
$\mathbf{x}^\oplus = \sum_{j=1}^r \frac{1}{\sigma_j} \langle \mathbf{b}, \mathbf{v}_j \rangle \mathbf{u}_j$
其中 $r$ 是 $A$ 的秩，$(\sigma_j, \mathbf{u}_j, \mathbf{v}_j)$ 是 $A$ 的奇异系统。

**※【例题】** 对于矩阵 $A = \begin{bmatrix} 1 & 1 - \delta \\ 1 & 1 + \delta \end{bmatrix}$ (2.14) 且 $\delta = 10^{-8}$，有 $\mathrm{cond} A = 199999999.137258$。

**※【定理】** 设 $A$ 是可逆矩阵，$\hat{A}$ 是 $A$ 的扰动，满足
$\|A^{-1}\| \|\hat{A} - A\| < 1$ (2.15)
$\mathbf{x}$ 和 $\hat{\mathbf{x}}$ 分别是线性系统 $A\mathbf{x} = \mathbf{b}$ 和 $\hat{A}\hat{\mathbf{x}} = \hat{\mathbf{b}}$ (2.16) 的解。则 $\hat{\mathbf{x}}$ 近似 $\mathbf{x}$ 的相对误差界为
$\frac{\|\hat{\mathbf{x}} - \mathbf{x}\|}{\|\mathbf{x}\|} \leq \frac{\mathrm{cond} A}{1 - (\mathrm{cond} A) \frac{\|\hat{A} - A\|}{\|A\|}} \left( \frac{\|\hat{\mathbf{b}} - \mathbf{b}\|}{\|\mathbf{b}\|} + \frac{\|\hat{A} - A\|}{\|A\|} \right)$ (2.17)

**证明**：写 $\hat{A} = A[I + A^{-1}(\hat{A} - A)]$，由引理 C.304 和 (2.15) 知 $\hat{A}$ 可逆且 $\hat{A}^{-1} = [I + A^{-1}(\hat{A} - A)]^{-1} A^{-1}$。由定理 G.76 和三角不等式得
$\|\hat{A}^{-1}\| \leq \frac{\|A^{-1}\|}{1 - \|A^{-1}\| \|\hat{A} - A\|}$ (2.18)
由 (2.16) 可写 $\hat{\mathbf{x}} - \mathbf{x} = \hat{A}^{-1}[\hat{\mathbf{b}} - \mathbf{b} - (\hat{A} - A)\mathbf{x}]$，结合 (2.18) 和三角不等式得
$\frac{\|\hat{\mathbf{x}} - \mathbf{x}\|}{\|\mathbf{x}\|} \leq \frac{\mathrm{cond} A}{1 - \|\hat{A} - A\| \|A^{-1}\|} \left( \frac{\|\hat{\mathbf{b}} - \mathbf{b}\|}{\|A\| \|\mathbf{x}\|} + \frac{\|\hat{A} - A\|}{\|A\|} \right)$。
由 $\|A\| \|\mathbf{x}\| \geq \|\mathbf{b}\|$ 即得 (2.17)。$\square$

**※【定义】** 向量函数的分量条件数(componentwise condition number)：
向量函数 $\mathbf{f} : \mathbb{R}^m \to \mathbb{R}^n$ 的分量条件数为
$\mathrm{cond}_{\mathbf{f}}(\mathbf{x}) = \|A(\mathbf{x})\|$ (2.19)
其中矩阵 $A(\mathbf{x}) = [a_{ij}(\mathbf{x})]$，每个分量为
$a_{ij}(\mathbf{x}) = \left| \frac{x_j \frac{\partial f_i}{\partial x_j}}{f_i(\mathbf{x})} \right|$ (2.20)

**※【例题】** 对于向量函数 $\mathbf{f}(\mathbf{x}) := \begin{bmatrix} \frac{1}{x_1} + \frac{1}{x_2} \\ \frac{1}{x_1} - \frac{1}{x_2} \end{bmatrix}$，其雅可比矩阵为 $\nabla \mathbf{f} = -\frac{1}{x_1^2 x_2^2} \begin{bmatrix} x_2^2 + x_1^2 \\ x_2^2 - x_1^2 \end{bmatrix}$。由定义 2.27 的条件数清晰地捕捉到 $x_1 \pm x_2 \approx 0$ 导致病态的事实，$C_c = \|A_c\|$，其中 $A_c = \begin{bmatrix} \left| \frac{x_2}{x_1 + x_2} \right| & \left| \frac{x_1}{x_1 + x_2} \right| \\ \left| \frac{x_2}{x_1 - x_2} \right| & \left| \frac{x_1}{x_1 - x_2} \right| \end{bmatrix}$，而基于 1-范数的定义 2.20 则无法捕捉病态，$C_1 = \frac{\|\mathbf{x}\|_1 \|\nabla \mathbf{f}\|_1}{\|\mathbf{f}\|_1} = \frac{|x_1| + |x_2|}{|x_1 x_2|} \frac{2 \max(x_1^2, x_2^2)}{|x_1 + x_2| + |x_1 - x_2|}$，因为 $x_1 \pm x_2 \approx 0$ 时 $C_1 \approx 2$。注意使用了公式 $\forall A \in \mathbb{R}^{n \times n}, \|A\|_1 = \max_j \sum_i |a_{ij}|$。

**※【例题】** 若参数 $t_1, t_2, \ldots, t_n$ 在 $[-1, 1]$ 上等距分布，基于 $\infty$-范数的范德蒙德矩阵条件数为 $\mathrm{cond}_\infty V_n \sim \frac{1}{\pi} \exp\left[ -\frac{\pi}{4} + \frac{n}{4}(\pi + 2\ln 2) \right]$，当 $n = 20$ 时约为 $9.86 \times 10^8$。

**※【定义】** 希尔伯特矩阵(Hilbert matrix)：
希尔伯特矩阵 $H_n \in \mathbb{R}^{n \times n}$ 定义为 $h_{i,j} = \frac{1}{i + j - 1}$ (2.21)

**※【例题】** 基于 2-范数的希尔伯特矩阵条件数为 $\mathrm{cond}_2 H_n \sim \frac{(\sqrt{2} + 1)^{4n + 4}}{2^{15/4} \sqrt{\pi n}}$，当 $n = 10$ 时约为 $9.22 \times 10^{14}$。

## 2.5 算法的签名

**※【定义】** 算法(algorithm)：算法是接受一组值作为输入并产生一组值作为输出的逐步过程。

**※【定义】** 前置条件(precondition)：前置条件是算法执行前对输入成立的条件。

**※【定义】** 后置条件(postcondition)：后置条件是算法执行后对输出成立的条件。

**※【定义】** 签名(signature)：算法的签名包括其输入、输出、前置条件、后置条件，以及如何处理违反前置条件的输入参数。

## 2.6 算法的条件数

**※【定义】** 算法 $\mathbf{f}_A$ 的条件数：
考虑用算法 $\mathbf{f}_A : \mathcal{F}^m \to \mathcal{F}^n$ 近似函数 $\mathbf{f} : \mathbb{R}^m \to \mathbb{R}^n$。假设
$\forall \mathbf{x} \in \mathcal{F}^m, \exists \mathbf{x}_A \in \mathbb{R}^m \text{ s.t. } \mathbf{f}_A(\mathbf{x}) = \mathbf{f}(\mathbf{x}_A)$ (2.22)
算法 $\mathbf{f}_A$ 的条件数由下式给出
$\mathrm{cond}_A(\mathbf{x}) = \frac{1}{\epsilon_u} \inf_{\{\mathbf{x}_A\}} \frac{\|\mathbf{x}_A - \mathbf{x}\|}{\|\mathbf{x}\|}$ (2.23)
其中 $\epsilon_u$ 是定义 1.28 中 $\mathcal{F}$ 的单位舍入误差。若算法条件数接近 1，则算法是良态的，否则是病态的。

**※【例题】** 考虑计算 $y = \ln x$ 的算法 $A$。假设对于任意正数 $x$，程序产生的 $y_A$ 满足 $y_A = (1 + \delta)\ln x$，其中 $|\delta| \leq 5\epsilon_u$。算法的条件数是多少？
显然有 $y_A = \ln x_A$，其中 $x_A = x^{1 + \delta}$。因此
$E_{\mathrm{rel}}(x_A) = \left| \frac{x^{1 + \delta} - x}{x} \right| = |x^\delta - 1| = |e^{\delta \ln x} - 1| \approx |\delta \ln x| \leq 5|\ln x| \epsilon_u$。
因此除了 $x \to 0^+$ 外，$A$ 是良态的。

**※【定理】** 假设光滑函数 $f : \mathbb{R} \to \mathbb{R}$ 由算法 $A : \mathcal{F} \to \mathcal{F}$ 近似，产生 $f_A(x) = f(x)(1 + \delta(x))$，其中 $|\delta(x)| \leq \varphi(x)\epsilon_u$。若 $\mathrm{cond}_f(x)$ 有界且非零，则
$\forall x \in \mathcal{F}, \mathrm{cond}_A(x) \leq \frac{\varphi(x)}{\mathrm{cond}_f(x)}$ (2.24)

**证明**：假设 $\forall x, \exists x_A$ 使得 $f(x_A) = f_A(x)$。写 $x_A = x(1 + \epsilon_A)$，有
$f(x)(1 + \delta) = f(x_A) = f(x(1 + \epsilon_A)) = f(x + x\epsilon_A) = f(x) + x\epsilon_A f'(x) + O(\epsilon_A^2)$。
忽略二次项得 $x\epsilon_A f'(x) = f(x)\delta \implies \left| \frac{x_A - x}{x} \right| = |\epsilon_A| = \left| \frac{f(x)}{x f'(x)} \right| |\delta(x)|$。两边除以 $\epsilon_u$ 得 $\frac{1}{\epsilon_u} \left| \frac{x_A - x}{x} \right| = \frac{\delta(x)}{\epsilon_u \mathrm{cond}_f(x)}$。对所有 $x_A$ 取下确界并应用 $|\delta(x)| \leq \varphi(x)\epsilon_u$ 即得 (2.24)。$\square$

**※【例题】** 假设 $\sin x$ 和 $\cos x$ 以机器舍入误差内的相对误差计算（通过截断泰勒级数可轻松满足）。应用定理 2.38 分析算法
$f_A = \mathrm{fl}\left[ \frac{\mathrm{fl}(1 - \mathrm{fl}(\cos x))}{\mathrm{fl}(\sin x)} \right]$ (2.25)
计算 $f(x) = \frac{1 - \cos x}{\sin x}$，$x \in (0, \pi/2)$ 的条件数。
由定义 2.14，易算得 $\mathrm{cond}_f(x) = \frac{x}{\sin x}$。此外，由定理 1.42 及对 $\sin x, \cos x$ 的假设，
$f_A(x) = \frac{(1 - (\cos x)(1 + \delta_1))(1 + \delta_2)}{(\sin x)(1 + \delta_3)} (1 + \delta_4)$，
其中 $|\delta_i| \leq \epsilon_u$，$i=1,2,3,4$。忽略二次项，上式等价于
$f_A(x) = \frac{1 - \cos x}{\sin x} \left\{ 1 + \delta_2 + \delta_4 - \delta_3 - \delta_1 \frac{\cos x}{1 - \cos x} \right\}$，
因此 $\varphi(x) = 3 + \frac{\cos x}{1 - \cos x}$，且
$\mathrm{cond}_A(x) \leq \frac{\sin x}{x} \left( 3 + \frac{\cos x}{1 - \cos x} \right)$。
故当 $x \to 0$ 时 $\mathrm{cond}_A(x)$ 可能无界。另一方面，当 $x \to \pi/2$ 时，$\mathrm{cond}_A(x)$ 受 $\frac{6}{\pi}$ 控制。

## 范数补充

$\|B\|_1 = \max_{1 \leq j \leq n} \sum_{i=1}^n |b_{ij}|$
**证明**：$\|B\mathbf{x}\|_1 = \sum_{i=1}^n |\sum_{j=1}^n b_{ij}x_j| \leq \sum_{i=1}^n \sum_{j=1}^n |b_{ij}||x_j| = \sum_{j=1}^n (\sum_{i=1}^n |b_{ij}|)|x_j| \leq \max_j (\sum_{i=1}^n |b_{ij}|) \sum_{j=1}^n |x_j|$。

$\|B\|_\infty = \max_{1 \leq i \leq n} \sum_{j=1}^n |b_{ij}|$
**证明**：$\|B\mathbf{x}\|_\infty = \max_i |\sum_{j=1}^n b_{ij}x_j| \leq \max_i \sum_{j=1}^n |b_{ij}| \max_j |x_j| = (\max_i \sum_{j=1}^n |b_{ij}|) \|\mathbf{x}\|_\infty$。

$\|B\|_2 = \sqrt{\lambda_{\max}(B^\top B)} = \sigma_{\max}(B)$
**证明**：$\|B\|_2^2 = \max \frac{\|B\mathbf{x}\|_2^2}{\|\mathbf{x}\|_2^2} = \max \frac{\mathbf{x}^\top B^\top B \mathbf{x}}{\mathbf{x}^\top \mathbf{x}} = \frac{\sum \lambda_i y_i^2}{\sum y_i^2} \leq \lambda_1$，其中 $B^\top B = V\Lambda V^\top$。