
> **论文**：AlphaQCM: Alpha Discovery in Finance with Distributional Reinforcement Learning  
> **作者**：Zhufan Zhu, Ke Zhu  
> **关键词**：公式化 alpha、协同 alpha、分布强化学习、QCM、非平稳 MDP、奖励稀疏

---

## 0. 一句话总结

论文把“自动发现一组可解释、可组合的公式化 alpha”建模为一个**非平稳、奖励稀疏的马尔可夫决策过程**，并提出 **AlphaQCM**：用分布强化学习学习分位数，再用 **QCM（Quantiled Conditional Moment）** 从可能有偏的分位数中估计**无偏方差**，把这个方差作为探索奖励，从而更高效地在巨大公式空间中搜索协同 alpha。

---

## 1. 摘要

1. **问题**：金融研究者需要寻找“协同的公式化 alpha”，但搜索空间巨大。
2. **视角转换**：把 alpha 发现过程看作一个**序列决策问题**，并概念化为**非平稳、奖励稀疏的 MDP**。
3. **方法**：提出 AlphaQCM。
	- 用 Q 网络学习 Q 函数；
	- 用分位数网络学习分位数；
	- 用 QCM 从“可能有偏的分位数”中学习**无偏方差**；
	- 用学到的 Q 和方差指导探索。
4. **实验**：在真实数据上显著优于竞争者，尤其股票数量大时。

**※【定义】** 协同 alpha
- **协同 alpha**：单个 alpha 可能噪声大；把多个 alpha 组合成一个 **meta-alpha**，可以提升预测能力。协同意味着它们互补，而不是高度重复。

## 2. 引言

### 2.1 传统 alpha 与 AI alpha

- 传统金融研究提出了很多著名 alpha，如长期动量、短期反转。
- 但投资者行为和心理会带来复杂、微妙的 alpha，传统金融方法难以形式化。
- 大多数 AI 方法用端到端机器学习模型直接预测收益，形成**非公式化 alpha**。
	- 优点：预测能力可能强；
	- 缺点：黑箱、不可解释、行业信任度低。
- 为提升信任，研究转向自动发现**公式化 alpha**。
- 常用方法是**遗传规划**。
- 但 GP 搜索空间随特征和算子数量指数增长。

### 2.3 AlphaGen 的贡献与两个缺口

- **AlphaGen** 把 alpha 发现问题转化为 MDP，并用强化学习找最优策略。
- 但存在两个理论—实践缺口：
	1. **非平稳（non-stationarity）**：MDP 的奖励函数会随决策轮次动态变化。
	2. **奖励稀疏（reward-sparsity）**：大多数探索得到的奖励为 0，训练低效且不稳定。

**※【定义】** MDP 
- **MDP** 通常由五元组描述：
$$
  \mathcal{M} = (\mathcal{X}, \mathcal{A}, P, R, \gamma)
  $$
  其中 $\mathcal{X}$ 是状态集，$\mathcal{A}$ 是动作集，$P$ 是转移概率，$R$ 是奖励函数，$\gamma$ 是折扣因子。
- **强化学习**：智能体在状态 $x_t$ 选择动作 $a_t$，获得奖励 $r_t$，转移到下一状态 $x_{t+1}$，目标是最大化累计折扣奖励。
- **非平稳 MDP**：奖励函数或转移概率随时间变化。
- **奖励稀疏**：大多数时间奖励为 0，只有少数关键动作有非零奖励。

---

## 3. 预备知识：从 RL 到分布强化学习

### 3.1 普通 RL 与 Q 函数

回顾标准 RL。定义折扣累计奖励：
$$
Z^\pi(x,a) \equiv \sum_{t=0}^{\infty} \gamma^t R_t
$$
Q 函数是它的期望：
$$
Q^\pi(x,a) \equiv \mathbb{E}[Z^\pi(x,a)]
$$
Bellman 最优算子为：
$$
Q^*(x,a) = \mathcal{T}Q^*(x,a)
= \mathbb{E}\left[R_t + \gamma Q^*(X_{t+1},a^*) \mid X_t=x, A_t=a\right]
$$
其中：
$$
a^* = \arg\max_{a'\in\mathcal{A}} Q^*(X_{t+1},a')
$$
传统 RL 主要学习标量期望 $Q^*(x,a)$，忽略了回报分布中的不确定性信息。

### 3.2 分布强化学习

DRL 直接学习回报分布 $Z^\pi(x,a)$。分布 Bellman 最优算子为：
$$
Z^*(x,a) \stackrel{D}{=} R_t + \gamma Z^*(X_{t+1},a^*)
$$
其中：
$$
a^* = \arg\max_{a'\in\mathcal{A}} \mathbb{E}[Z^*(X_{t+1},a')]
$$
$\stackrel{D}{=}$ 表示概率分布意义下相等。

### 3.3 分位数表示

常用分位数表示：
$$
Z_{\theta,\tau}(x,a)
= \sum_{k=0}^{K-1} (\tau_{k+1}-\tau_k)\delta_{\theta_k(x,a)}
$$
其中 $\delta_z$ 是位于 $z$ 的 Dirac 分布，$\tau=(\tau_1,\ldots,\tau_K)'$ 是分位水平，$\theta_k(x,a)$ 是对应分位数。

- **QRDQN**：固定 $\tau$，用多头网络学习 $\theta_k$。
- **IQN**：随机采样 $\tau$。
- **FQF**：额外用网络学习最优 $\tau$。

现有 DRL 通常先学分位数，再直接取期望估计 $Q^*$。论文指出：在非平稳、奖励稀疏 MDP 中，分位数估计可能有偏，直接取期望不可靠。

---

## 4. 方法

### 4.1 问题设定

假设有 $N$ 只股票，目标是找到最优 alpha 池：
$$
\mathcal{F} = \{f_1,\ldots,f_P\}
$$
每个公式化 alpha 定义为：
$$
\alpha_{p,s} = f_p(H_{s-1}) \in \mathbb{R}^N
$$
其中 $H_{s-1}$ 是截至 $s-1$ 的历史信息，$\alpha_{p,s}$ 是第 $p$ 个 alpha 在时间 $s$ 的横截面值。

线性 meta-alpha 为：
$$
\hat{\alpha}_s = \sum_{p=1}^{P} \alpha_{p,s}\hat{\beta}_p \in \mathbb{R}^N
$$
目标是让 $\hat{\alpha}_s$ 能有效预测未来收益 $y_s$，即取得高 IC。

### 4.2 公式化 alpha 的表示

- 算子分为：
	- **时间序列算子**：如 $\mathrm{TsRank}(\text{Close}, d)$、均值、标准差等。
	- **横截面算子**：如 $\mathrm{Rank}(\text{Low})$、$\mathrm{Sign}$、$\mathrm{Abs}$、$\mathrm{Log}$ 等。
- 用 **逆波兰表示（RPN）** 把表达式编码为 token 序列。
- 例：Alpha#4 可编码为：
$$
  \text{BEG} \to \text{Low} \to \text{Rank} \to 9 \to \text{TsRank} \to -1 \to \text{Mul} \to \text{SEP}
  $$
- 这样，RPN 序列可看作一条轨迹，每个位置选择下一个 token，于是 alpha 发现变成序列决策问题。

**※【定义】** RPN
逆波兰表示把操作符放在操作数后面。例如：
$$
(-1) \times \mathrm{TsRank}(\mathrm{Rank}(\text{low}), 9)
$$
可写成：
$$
\text{Low } \text{Rank } 9 \text{ TsRank } -1 \text{ Mul}
$$
它天然对应表达式树的后序遍历，便于用序列模型生成。

### 4.3 alpha 发现 MDP

- **状态 $x_t$**：当前已生成的 token 序列，初始为 BEG。
- **动作 $a_t$**：下一个 token。
- **转移**：确定性地把 $a_t$ 追加到序列末尾；若遇到 SEP 或达到最大长度，则回合结束。
- **奖励 $r_t$**：
	- 未完成表达式：$r_t=0$；
	- 无效表达式：$r_t=-1$；
	- 有效表达式：加入 alpha 池，拟合线性模型，更新 alpha 池，计算 meta-alpha 的 IC 增量：$$r_t = \mathrm{IC}(\mathcal{F}^*) - \mathrm{IC}(\mathcal{F})$$
- **折扣因子**：$\gamma=1$。

奖励计算流程见论文 Algorithm 1：
1. 新 alpha 加入；
2. 标准化；
3. 拟合线性模型：$\hat{\beta} = \arg\min_{\beta} \sum_s \|y_s - A_s\beta\|^2$
4. 若池子超过 $P$，删除贡献最小者；
5. 计算 IC 和奖励。

### 4.4 非平稳与奖励稀疏

- **非平稳**：当 alpha 池更新后，相似 alpha 的奖励会降低，奖励函数发生变化。
- **奖励稀疏**：
	- 只有完整表达式才有非零奖励；
	- 市场数据信噪比低，大多数 alpha 无效，奖励为 0。
因此，传统 RL 训练低效且不稳定。

---

### 4.5 AlphaQCM 核心

#### 4.5.1 QCM：从分位数学习方差

论文使用 **Cornish-Fisher 展开** 把分位数与矩联系起来：
$$
\theta_k(x,a) = Q^*(x,a) + \Phi_k'\delta(x,a) + \omega_k(x,a)
$$
其中：
$$
\Phi_k = (\phi_k, \phi_k^2-1, \phi_k^3-3\phi_k)'
$$
$$
\delta(x,a) = \left(
\sqrt{h(x,a)},
\frac{\sqrt{h(x,a)}s(x,a)}{6},
\frac{\sqrt{h(x,a)}[k(x,a)-3]}{24}
\right)'
$$
这里 $Q^*$ 是均值，$h$ 是方差，$s$ 是偏度，$k$ 是峰度，$\phi_k$ 是标准正态的 $\tau_k^*$ 分位数。

实际估计的分位数有偏：
$$
\hat{\theta}_k(x,a)
= \zeta(x,a) + Q^*(x,a) + \Phi_k'\delta(x,a) + \epsilon_k(x,a)
$$
这可以看成一个线性回归：
- 响应：$\hat{\theta}_k(x,a)$
- 回归元：$\Phi_k$
- 截距：$\zeta(x,a)+Q^*(x,a)$
- 系数：$\delta(x,a)$
- 残差：$\epsilon_k(x,a)$

用 OLS 可得到 $\hat{h}(x,a)$、$\hat{s}(x,a)$、$\hat{k}(x,a)$。

论文 Proposition 3.1 表明，在温和条件下：
$$
\hat{h}(x,a) \xrightarrow{p} h(x,a), \quad
\hat{s}(x,a) \xrightarrow{p} s(x,a), \quad
\hat{k}(x,a) \xrightarrow{p} k(x,a)
$$
其中 $\hat{h}$ 是**无偏方差估计**。它捕捉了参数不确定性和内在不确定性，可作为探索 bonus。

注意：QCM 不能估计 $Q^*$，因为截距是 $\zeta+Q^*$，两者不可识别。因此论文另用 DQN 学 Q。

#### 4.5.2 DRL Backbone

论文用 **IQN** 学分位数，用 **DQN** 学 Q 函数。

动作选择为：
$$
a_t = \arg\max_{a\in\mathcal{A}}
\left[
\hat{Q}(x_t,a) + \lambda \sqrt{\hat{h}(x_t,a)}
\right]
$$
其中 $\lambda$ 控制风险偏好/探索程度。

网络结构：
- $\tau$-embedding 网络把分位水平 $\tau$ 映射为嵌入；
- LSTM 提取 token 序列特征；
- 全连接头输出分位数或 Q 值。

训练：
- 分位数网络用分位数时序差分误差；
- Q 网络用平方时序差分误差；
- 使用优先经验回放。

---

## 5. 实验（Experiments）

### 5.1 数据与设置

- 中国 A 股市场，预测未来 20 日收益。
- 三个股票池：
  - CSI 300；
  - CSI 500；
  - 全市场 Market。
- 时间划分：
  - 训练：2010/01/01–2019/12/31；
  - 验证：2020/01/01–2020/12/31；
  - 测试：2021/01/01–2022/12/31。
- 指标：IC。

### 5.2 基线方法

1. **Alpha101**：人工公式化 alpha。
2. **MLP / XGBoost / LightGBM**：机器学习非公式化 alpha。
3. **GP w/o filter / GP w/ filter**：遗传规划公式化 alpha。
4. **PPO w/ filter / AlphaGen**：强化学习公式化 alpha。

### 5.3 主要结果

| 方法 | CSI 300 | CSI 500 | Market |
|---|---:|---:|---:|
| Alpha101 | 3.44% | 4.38% | 3.15% |
| GP w/ filter | 3.71% | 4.52% | 0.84% |
| AlphaGen | 8.13% | 8.08% | 6.04% |
| **AlphaQCM** | **8.49%** | **9.55%** | **9.16%** |

结论：
- AlphaQCM 在所有股票池均最优。
- AlphaGen 第二，但股票越多，AlphaQCM 优势越大。
- GP 的 mutual IC filter 在大市场反而变差。
- 机器学习方法在 Market 上超过 Alpha101，但 RL 方法更强且可解释。

### 5.4 QCM 与 DRL Backbone 消融

比较三种动作选择：
- No variance：$\lambda=0$；
- Vanilla variance：用普通分位数方差；
- QCM variance：用 QCM 方差。

以及两种 backbone：
- QRDQN；
- IQN。

结果：
- QCM 方差在几乎所有设置下 IC 最高，标准差更低。
- IQN 多数情况下优于 QRDQN。
- 说明 QCM 方差提升训练效率和收敛稳定性。

### 5.5 领域知识影响

- 用 Alpha101 初始化 replay memory。
- 早期（10%、20% 训练）有增益；
- 后期（50%、100% 训练）完全数据驱动反而更好。
- 原因：模仿专家可能陷入局部最优。

### 5.6 美股 S&P 500

- AlphaQCM 仍优于所有竞争者。
- 但整体 IC 下降，因为美股更有效、对新闻更敏感。

---

## 6. 结论（Conclusion）

- 提出 AlphaQCM，用于大数据下的 alpha 发现。
- 核心思想：从可能有偏的分位数中估计**无偏方差**。
- 实现：IQN 学分位数，DQN 学 Q。
- 实验证明在非平稳、奖励稀疏 MDP 中高效。
- 消融研究验证 QCM、backbone、参数规模等影响。

---

## 7. 附录要点

- **IC 定义**：
  $$
  \mathrm{IC}(\mathcal{F})
  = \mathrm{Mean}[\mathrm{Corr}(\hat{\alpha}_s, y_s)]
  = \frac{1}{S}\sum_{s=1}^{S}
  \frac{\sum_{i=1}^{N}(\hat{\alpha}_{i,s}-\bar{\alpha}_s)(y_{i,s}-\bar{y}_s)}
  {\sqrt{\sum_{i=1}^{N}(\hat{\alpha}_{i,s}-\bar{\alpha}_s)^2
  \sum_{i=1}^{N}(y_{i,s}-\bar{y}_s)^2}}
  $$
- **特征与算子**：Open/High/Low/Close/Vwap/Volume、常数、时间差；时间序列算子如 TsRank、Mean、Std、Cov、Corr；横截面算子如 Rank、Sign、Abs、Log、Add/Sub/Mul/Div。
- **渐近性质**：在 Assumption C.1 和 C.2 下，QCM 的矩估计一致。
- **Vanilla 方差估计**：
  $$
  \mathrm{Var}[Z_{\hat{\theta},\tau}(x,a)]
  = \sum_{k=1}^{K}(\tau_k-\tau_{k-1})
  \left\{
  \hat{\theta}_k(x,a) - \mathbb{E}[Z_{\hat{\theta},\tau}(x,a)]
  \right\}^2
  $$
- **训练细节**：分位数 TD 误差、Huber 分位数回归损失、优先经验回放。
- **超参数**：$K=64$，$K'=64$，$\kappa=1.0$，$\eta=0.5$，$\lambda=0.5,1,2$，$P=10,20,50,100$。
- **额外实验**：
  - 参数规模：AlphaQCM 约 572K，AlphaGen 约 298K；小 AlphaQCM 仍优。
  - MAMBA 替换 LSTM：无显著差异。
  - 扩展测试集到 2024：AlphaQCM 仍优，但所有方法 IC 下降。

