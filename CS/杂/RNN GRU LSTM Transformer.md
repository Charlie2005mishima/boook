
**※【定义】循环神经网络**：循环神经网络（recurrent neural network, RNN）是一类处理序列数据的神经网络结构，其核心特征是在时间步之间共享参数，并通过隐藏状态（hidden state）传递历史信息。

**※【定义】标准RNN（Vanilla RNN）** ：最基础的循环单元，每个时间步仅通过一个线性变换与激活函数更新隐藏状态，结构简单但存在严重的梯度消失（vanishing gradient）问题。

设输入序列为 $\{x_1,x_2,\dots,x_T\}$，其中 $x_t\in\mathbb{R}^{d}$。隐藏状态 $h_t\in\mathbb{R}^{n}$，初始状态 $h_0=\mathbf{0}$。标准RNN的前向计算为：

$h_t=\sigma(x_t^{\top}W_{in}+h_{t-1}^{\top}W_{hh}+b)$ (1.1)

其中 $W_{in}\in\mathbb{R}^{d\times n}$ 为输入权重，$W_{hh}\in\mathbb{R}^{n\times n}$ 为循环权重，$b\in\mathbb{R}^{n}$ 为偏置，$\sigma(\cdot)$ 为逐元素激活函数（如Sigmoid或Tanh）。输出层通常为 $o_t=h_t^{\top}W_{out}$，$W_{out}\in\mathbb{R}^{n\times q}$。

**※【定理】标准RNN的梯度消失**：若循环权重矩阵 $W_{hh}$ 的谱半径（spectral radius）小于1，且激活函数导数有界，则随序列长度增加，反向传播梯度以指数速度衰减。

**※【例题】** 设 $n=1$，$W_{hh}=0.5$，$\sigma'(x)\leq 1$。从时间步 $T$ 到 $t$ 的梯度包含因子 $(0.5)^{T-t}$。当 $T-t=20$ 时，该因子约为 $9.5\times 10^{-7}$，梯度几乎消失，网络无法学习长距离依赖。


**※【定义】长短期记忆网络**：长短期记忆网络（long short-term memory, LSTM）通过引入三个门控（gate）和一个细胞状态（cell state）来缓解梯度消失问题。

LSTM维护两个状态：隐藏状态 $h_t\in\mathbb{R}^{n}$ 与细胞状态 $c_t\in\mathbb{R}^{n}$。输入 $x_t\in\mathbb{R}^{d}$。三个门均为Sigmoid激活的向量：

$f_t=\sigma(W_f x_t+U_f h_{t-1}+b_f)$ (2.1)

$i_t=\sigma(W_i x_t+U_i h_{t-1}+b_i)$ (2.2)

$o_t=\sigma(W_o x_t+U_o h_{t-1}+b_o)$ (2.3)

候选细胞状态：

$\tilde{c}_t=\tanh(W_c x_t+U_c h_{t-1}+b_c)$ (2.4)

细胞状态更新（遗忘门 $f_t$ 控制旧信息保留，输入门 $i_t$ 控制新信息写入）：

$c_t=f_t\odot c_{t-1}+i_t\odot\tilde{c}_t$ (2.5)

隐藏状态输出：

$h_t=o_t\odot\tanh(c_t)$ (2.6)

其中 $W_{\ast}\in\mathbb{R}^{d\times n}$，$U_{\ast}\in\mathbb{R}^{n\times n}$，$b_{\ast}\in\mathbb{R}^{n}$，$\odot$ 为哈达玛积（Hadamard product）。

**※【定理】LSTM的梯度高速公路**：细胞状态 $c_t$ 的更新式 (2.5) 对 $c_{t-1}$ 的偏导为 $\partial c_t/\partial c_{t-1}=\text{diag}(f_t)$。若遗忘门 $f_t$ 在若干步上接近1，则梯度可沿细胞状态几乎无衰减地回传，这是LSTM缓解梯度消失的核心机制。

**※【例题】** 设 $n=2$，$f_t=(0.9,0.95)$ 持续10步，则梯度衰减因子为 $0.9^{10}\approx0.35$ 与 $0.95^{10}\approx0.60$，远大于标准RNN中 $0.5^{10}\approx9.8\times10^{-4}$。


**※【定义】门控循环单元**：门控循环单元（gated recurrent unit, GRU）是LSTM的简化变体，将细胞状态与隐藏状态合并为一个状态，仅使用两个门控。

GRU仅维护一个隐藏状态 $h_t\in\mathbb{R}^{n}$。更新门 $z_t$ 与重置门 $r_t$ 为：

$z_t=\sigma(W_z x_t+U_z h_{t-1}+b_z)$ (3.1)

$r_t=\sigma(W_r x_t+U_r h_{t-1}+b_r)$ (3.2)

候选隐藏状态（重置门作用于上一步状态）：

$\tilde{h}_t=\tanh(W_h x_t+U_h(r_t\odot h_{t-1})+b_h)$ (3.3)

隐藏状态更新（更新门在旧状态与候选状态之间插值）：

$h_t=(1-z_t)\odot h_{t-1}+z_t\odot\tilde{h}_t$ (3.4)

权重维度与LSTM相同。

**※【定理】GRU与LSTM的参数量对比**：单层LSTM含4组 $(W,U,b)$，GRU含3组，故GRU参数量约为LSTM的 $3/4$。每步计算复杂度：RNN为 $\mathcal{O}(n^2)$，GRU为 $\mathcal{O}(3n^2)$，LSTM为 $\mathcal{O}(4n^2)$。

**※【例题】** 设 $d=n=64$。LSTM参数量：$4(64\times64+64\times64+64)=4\times8256=33,024$。GRU参数量：$3\times8256=24,768$，约为LSTM的 $75\%$。


**※【定义】独立循环神经网络**：独立循环神经网络（independently recurrent neural network, IndRNN）将循环权重由矩阵乘法改为逐元素向量乘法，使同层神经元在时间维度上完全解耦。

IndRNN的循环权重为向量 $u\in\mathbb{R}^{n}$，而非矩阵：

$h_t=\sigma(W x_t+u\odot h_{t-1}+b)$ (4.1)

逐神经元展开为：

$h_{n,t}=\sigma(w_n x_t+u_n h_{n,t-1}+b_n)$ (4.2)

其中 $w_n$ 为 $W$ 的第 $n$ 行，$u_n,b_n$ 为标量。

**※【定理】IndRNN的梯度稳定性**：由于循环连接为逐元素乘积，第 $n$ 个神经元的梯度仅依赖其自身的循环权重 $u_n$，不再受其他神经元循环权重的矩阵乘积影响。通过约束 $|u_n|$ 在适当区间，可显式控制记忆长度。

**※【例题】** 若 $u_n=1.0$，则 $h_{n,t}$ 对 $h_{n,t-k}$ 的依赖因子为 $1^k=1$，信息可无限长保持；若 $u_n=0.9$，10步后因子为 $0.9^{10}\approx0.35$，呈现有限记忆。


**※【定义】简单循环单元**：简单循环单元（simple recurrent unit, SRU）通过将时间步之间的矩阵乘法替换为逐元素乘法，使大部分计算可在时间维度上并行完成。

SRU的核心观察是：时间步之间的依赖仅通过一个逐元素门控传递。设 $\tilde{x}_t=W x_t$，$f_t=\sigma(W_f x_t+b_f)$，$r_t=\sigma(W_r x_t+b_r)$。状态更新为：

$c_t=f_t\odot c_{t-1}+(1-f_t)\odot\tilde{x}_t$ (5.1)

$h_t=r_t\odot g(c_t)$ (5.2)

其中 $g$ 为激活函数。关键点：$W x_t$、$W_f x_t$、$W_r x_t$ 均只依赖当前输入 $x_t$，**所有时间步的这些变换可预先并行计算**，时间步之间仅剩逐元素运算。

**※【定理】SRU的并行性**：SRU每层的计算瓶颈由 $\mathcal{O}(T n^2)$ 的序列矩阵乘法降为 $\mathcal{O}(T n^2)$ 的**并行**矩阵乘法加 $\mathcal{O}(T n)$ 的序列逐元素运算，在GPU上可获得显著加速。

**※【例题】** $T=100$，$n=256$。标准RNN需100次 $256\times256$ 矩阵乘法（串行）；SRU可将 $W[x_1,\dots,x_{100}]$ 作为一次 $256\times256$ 乘 $256\times100$ 的矩阵乘法并行完成，序列部分仅剩100次逐元素乘加。


**※【定义】回声状态网络**：回声状态网络（echo state network, ESN）属于储备池计算（reservoir computing）范式。循环权重 $W$ 随机生成后**冻结不训练**，仅训练输出权重 $W_{out}$。

储备池状态更新为：

$x(t+1)=f(W x(t)+W_{in}u(t)+b)$ (6.1)

输出（仅 $W_{out}$ 可训练）：

$y(t)=W_{out}[u(t);x(t)]$ (6.2)

**※【定理】回声状态性质**：若储备池权重矩阵 $W$ 的谱半径 $\rho(W)<1$，则储备池状态对初始条件的依赖随时间指数遗忘，网络具有“回声”性质——当前状态由最近的输入历史唯一决定。

**※【例题】** 设 $\rho(W)=0.9$，初始状态差异 $\|\Delta x(0)\|=1$。10步后差异约为 $0.9^{10}\approx0.35$，100步后约为 $2.7\times10^{-5}$，初始条件的影响基本消失。


**※【定义】Transformer自注意力**：Transformer摒弃循环结构，通过自注意力（self-attention）在**所有时间步之间直接建立连接**，计算完全可并行。

输入矩阵 $X\in\mathbb{R}^{T\times d}$，通过三个线性投影得到查询（query）、键（key）、值（value）：

$Q=XW_Q,\quad K=XW_K,\quad V=XW_V$ (7.1)

缩放点积注意力（scaled dot-product attention）：

$\text{Attention}(Q,K,V)=\text{softmax}\left(\frac{QK^{\top}}{\sqrt{d_k}}\right)V$ (7.2)

其中 $W_Q,W_K,W_V\in\mathbb{R}^{d\times d_k}$，$d_k$ 为键维度。

**※【定理】Transformer的并行性与复杂度**：自注意力层在时间维度上无序列依赖，所有 $T$ 个位置可并行计算。但注意力矩阵 $QK^{\top}\in\mathbb{R}^{T\times T}$ 的复杂度为 $\mathcal{O}(T^2 d_k)$，对长序列代价较高。

**※【例题】** $T=512$，$d_k=64$。注意力矩阵含 $512\times512=262,144$ 个元素，每个元素由 $d_k=64$ 次乘加得到，单层自注意力约 $1.68\times10^7$ 次乘加，但全部可在GPU上并行。


**※【定义】双向循环神经网络**：双向循环神经网络（bidirectional RNN）使用两个独立的循环层分别处理正向和反向序列，最终将两个方向的隐藏状态拼接作为每个时间步的表示。

正向隐藏状态：

$\overrightarrow{h}_t=\mathcal{H}(W_{x\overrightarrow{h}}x_t+W_{\overrightarrow{h}\overrightarrow{h}}\overrightarrow{h}_{t-1}+b_{\overrightarrow{h}})$ (8.1)

反向隐藏状态：

$\overleftarrow{h}_t=\mathcal{H}(W_{x\overleftarrow{h}}x_t+W_{\overleftarrow{h}\overleftarrow{h}}\overleftarrow{h}_{t+1}+b_{\overleftarrow{h}})$ (8.2)

拼接输出：

$h_t=[\overrightarrow{h}_t;\overleftarrow{h}_t]$ (8.3)

**※【注意】** 双向结构使用了未来信息，因此**不能用于因果预测**（如金融时序预测中预测未来收益），仅适用于编码或序列标注等允许看到完整序列的任务。


**※【定义】神经常微分方程循环网络**：将循环网络的离散时间步替换为连续时间演化，隐藏状态由常微分方程（ordinary differential equation, ODE）描述：

$\frac{dh(t)}{dt}=f(h(t),t,\theta)$ (9.1)

给定初始状态 $h(0)=h_0$，通过ODE求解器（如Runge-Kutta）从 $t=0$ 积分到 $T$ 得到 $h(T)$。GRU的连续时间形式可写为：

$\frac{dh(t)}{dt}=(1-z(t))\odot(\tilde{h}(t)-h(t))$ (9.2)

其中 $z(t),\tilde{h}(t)$ 由 $h(t)$ 和输入 $u(t)$ 通过门控网络计算。

**※【定理】Neural ODE的伴随方法**：梯度可通过解另一个反向ODE计算，无需存储前向传播的中间状态，内存代价为 $\mathcal{O}(1)$（相对于时间步数）。

**※【例题】** 设 $f(h,t)=\tanh(Ah+b)$，$h(0)=0$。用Euler法以步长 $\Delta t=0.1$ 从 $t=0$ 积分到 $t=1$，需10步，每步 $h_{k+1}=h_k+\Delta t\cdot\tanh(Ah_k+b)$。


**※【定义】线性循环单元**：线性循环单元（linear recurrent unit, LRU）将循环连接对角化，使隐藏状态各维度独立演化，从而实现高效并行训练。

LRU的连续时间形式为：

$\frac{dh(t)}{dt}=\Lambda h(t)+B u(t)$ (10.1)

其中 $\Lambda=\text{diag}(\lambda_1,\dots,\lambda_n)$ 为对角矩阵，$B\in\mathbb{R}^{n\times d}$。离散化后（步长 $\Delta t$）：

$h_t=\exp(\Lambda\Delta t)\odot h_{t-1}+(\exp(\Lambda\Delta t)-I)\Lambda^{-1}B u_t$ (10.2)

由于 $\Lambda$ 为对角矩阵，各维度独立演化，**时间维的循环可转化为并行扫描（parallel scan）** ，训练速度显著优于LSTM/GRU。

**※【例题】** $n=3$，$\Lambda=\text{diag}(-0.1,-0.5,-1.0)$，$\Delta t=1$。衰减因子为 $(e^{-0.1},e^{-0.5},e^{-1})\approx(0.905,0.607,0.368)$，三个维度以不同时间尺度记忆历史。


**※【定理】序列模型复杂度对比**：设隐藏维度为 $n$，序列长度为 $T$。各模型每步时间复杂度与训练并行性如下：

| 模型              | 每步时间复杂度                        | 时间维可并行 | 参数量（近似）       |
| --------------- | ------------------------------ | ------ | ------------- |
| **RNN**         | $\mathcal{O}(n^2)$             | 否      | $n^2+dn+n$    |
| **LSTM**        | $\mathcal{O}(4n^2)$            | 否      | $4(n^2+dn+n)$ |
| **GRU**         | $\mathcal{O}(3n^2)$            | 否      | $3(n^2+dn+n)$ |
| IndRNN          | $\mathcal{O}(n^2)$             | 否      | $dn+2n$       |
| SRU             | $\mathcal{O}(n^2+dn)$（矩阵部分可并行） | 部分     | $3(dn+n)$     |
| ESN             | $\mathcal{O}(n^2)$             | 否      | 仅 $n$（储备池冻结）  |
| **Transformer** | $\mathcal{O}(T n^2)$           | 是      | $4dn+dn$（单头）  |
| Neural ODE      | 取决于求解器步数                       | 否      | 同基础RNN        |
| LRU             | $\mathcal{O}(n\log T)$（并行扫描）   | 是      | $n+dn$        |

**※【例题】** $n=256$，$d=64$。LSTM参数量约 $4\times(256^2+64\times256+256)\approx4\times82,176\approx328,704$；GRU约 $3\times82,176\approx246,528$；IndRNN仅 $64\times256+2\times256\approx16,896$，参数效率极高。