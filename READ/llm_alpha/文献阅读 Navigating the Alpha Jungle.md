
## 一、摘要 (Abstract)

本文提出将**大语言模型 (LLMs)** 与**蒙特卡洛树搜索 (MCTS)** 相结合的框架，用于公式化Alpha因子挖掘。核心贡献包括：

- 将Alpha挖掘建模为**MCTS驱动的搜索问题**，每个树节点代表一个候选Alpha公式
- 利用LLM的指令遵循和推理能力，迭代生成和精炼符号化Alpha公式
- 通过**定量回测反馈**指导MCTS探索，高效导航巨大搜索空间
- 引入**频繁子树避免 (Frequent Subtree Avoidance, FSA)** 机制，增强搜索多样性，防止公式同质化
- 在真实股票数据上验证了优于现有方法的预测准确性和交易表现，且生成的公式具有更强的可解释性

---

## 二、引言 (Introduction)

### 2.1 研究背景

- 金融市场**低信噪比**特性使得价格走势预测成为核心挑战
- **Alpha因子**：从股票数据中提取的预测信号，是提升模型预测能力的关键策略

### 2.2 Alpha挖掘的两种范式

| 类型 | 代表方法 | 优点 | 缺点 |
|------|----------|------|------|
| **神经网络式** | FactorVAE, HIST, REST | 捕捉复杂模式 | 缺乏可解释性 |
| **公式化** | Fama-French因子, 金融异象 | 可解释性强，反映市场洞察 | 传统上依赖人工设计 |

### 2.3 现有自动化公式化Alpha挖掘的局限性

1. **可解释性差**：无约束的数据驱动探索缺乏金融理论指导，生成公式过于复杂和不透明
   - 阻碍对经济逻辑的理解
   - 难以准确归因组合表现
   - 侵蚀信任，阻碍实际采纳

2. **搜索效率低**：需要生成和评估大量候选公式
   - 低信号密度导致**过拟合**风险升高（Harvey, Liu, and Zhu 2016）
   - 样本外泛化能力差

### 2.4 本文方法

- 利用LLM的**先验知识和推理能力**生成可解释Alpha
- 将Alpha挖掘建模为**MCTS驱动的搜索问题**
	- 每个节点代表候选Alpha公式
	- 系统性探索和迭代精炼
- **关键创新**：利用回测提供的**细粒度反馈**指导搜索
- **频繁子树避免**：识别并避免使用最频繁的子结构，增强多样性

### 2.5 与一般推理任务的区别

- 一般推理任务：MCTS探索预定义动作或评估抽象状态
- 本文：LLM作为**符号化Alpha公式的生成先验**，MCTS由**领域特定的定量回测反馈**引导

### 2.6 主要贡献

1. 提出**LLM驱动的MCTS框架**用于公式化Alpha挖掘，将任务建模为树搜索推理问题
2. 设计**频繁子树避免方法**，引导LLM探索罕见但可能有效的公式结构
3. 实验验证了框架的有效性，挖掘的Alpha在预测性能和可解释性上均优于对比方法

## 三、预备知识

### 3.1 Alpha因子挖掘

**市场设定**：
- $n$ 只股票，$T$ 个交易日
- 特征向量 $\pmb{x}_{i,t} \in \mathbb{R}^m$：OHLC价格、成交量、VWAP Volume-Weighted Average Price
- 市场历史张量：$\pmb{X} \in \mathbb{R}^{T \times n \times m}$
- 未来收益矩阵：$\mathbf{Y} \in \mathbb{R}^{T \times n}$，$y_{i,t}$ 为股票 $i$ 在 $t$ 日后的未来收益

**Alpha因子**：使用回溯窗口 $\tau$，$X_{t-\tau+1:t}=\{ X_{s}|t-\tau<s\leq t \}$，将历史特征映射为预测分数
$$
v_t = f(X_{t-\tau+1:t}) \in \mathbb{R}^n
$$

**目标**：发现 $K$ 个Alpha因子的集合 $\mathcal{F} = \{f_1, \ldots, f_K\}$，通过组合模型 $g$ 得到复合信号
$$
\pmb{z}_t = g(\{v_{k,t}\}_{k=1}^K; \pmb{\theta}_g)
$$

**组合模型参数优化**：
$$
\pmb{\theta}_g^*(\mathcal{F}) = \arg\max_{\pmb{\theta}_g} \mathcal{L}(g(\{v_{k,t}\}_{k=1}^K; \pmb{\theta}_g), \pmb{Y}) \tag{1}
$$

**整体双层优化**：
$$
\mathcal{F}^* = \arg\max_{\mathcal{F} \subset \mathcal{A}} \mathcal{L}(g^*(\mathcal{F}), \mathbf{Y})
$$
其中 $\mathcal{A}$ 为所有可能Alpha因子的搜索空间。

### 3.2 公式化Alpha

- 由**操作符**和**操作数**构成的数学表达式 operators and operands.
- 操作数：原始输入特征（如 $close_{i,t}$）和数值常量
- 操作符：数学变换，如时间序列操作符
- 示例：$\mathrm{Ma}(close, 5) - \mathrm{Ma}(close, 20)$（捕捉价格趋势）
- 自然表示为**表达式树**（叶节点：操作数；内部节点：操作符）

## 四、方法论

### 4.1 框架总览

**核心迭代过程**：
1. **选择 (Selection)**：使用UCT(Upper Confidence Bound for Trees)准则选择有前景的节点
2. **扩展 (Expansion)**：LLM基于多维性能反馈生成精炼Alpha
3. **评估 (Evaluation)**：通过回测评估新Alpha，形成树中新节点
4. **反向传播 (Backpropagation)**：更新路径统计量

**LLM的双重角色**：
- 提出**针对性精炼建议**
- 将建议转化为**具体Alpha公式** $f \in \mathcal{A}$

**有效Alpha仓库**：满足预定义标准 $\mathcal{C}$（如 IC $> 0.02$）的高性能Alpha被收集到 $\mathcal{F}_{zoo}$

![[Pasted image 20260803102313.png]]

### 4.2 选择 (Selection)

**UCT准则**：
$$
a^* = \arg\max_{a \in A(s)} \left( Q(s, a) + c \sqrt{\frac{\ln(N_s)}{N_{s'}}} \right) \tag{2}
$$

- $A(s)$：状态 $s$ 的现有动作集合
- $N_s$：父状态 $s$ 的访问次数
- $N_{s'}$：子状态 $s' = \text{child}(s, a)$ 的访问次数
- $c$：探索权重
- $Q(s, a)$：在该动作产生的子节点为根的子树中观察到的最大奖励

**关键创新**：允许**任意节点**（不仅叶节点）被选择扩展
- 动作空间 $A(s)$ 增加"虚拟"扩展动作 $a_e$
- 虚拟访问计数：$N_{s_e'} = 1 + |C(s)|$，$C(s)$ 为 $s$ 的现有子节点集合
- 使有前景的非叶节点可被继续精炼

这是一个非常深入且关键的问题。在MCTS（蒙特卡洛树搜索）中，**访问次数**是整个算法的“计分板”，它驱动着探索与利用的平衡。为了让你透彻理解，我们从标准MCTS的定义出发，再深入到本文的特定创新。


#### 1. 什么是“访问次数”（$N_s$ 和 $N_{s'}$）？

在MCTS中，**访问次数**（Visit Count）是指**该节点在搜索过程中被“选中”或“遍历”的总次数**。

- **物理含义**：每当搜索算法从根节点出发，沿着一条路径向下走时，路径上经过的每一个节点的计数器 $N$ 都会加 1。
- **数学角色**：在UCT公式 $Q + c \sqrt{\frac{\ln(N_s)}{N_{s'}}}$ 中：
  - $N_s$（父节点访问次数）：代表你对**当前局面**的熟悉程度。$\ln(N_s)$ 会随着搜索的进行缓慢增长。
  - $N_{s'}$（子节点访问次数）：代表你对**特定子动作**尝试了多少次。

**UCT的直觉逻辑**：
如果子节点 $s'$ 被尝试的次数很少（即 $N_{s'}$ 很小），那么分母就很小，整个分式 $\sqrt{\frac{\ln(N_s)}{N_{s'}}}$ 就会变得很大。这会鼓励算法去选择这个“冷门”的子节点——这就是**探索（Exploration）**。反之，如果 $N_{s'}$ 很大，分式变小，算法就会更依赖 $Q$ 值（历史收益）——这就是**利用（Exploitation）**。


#### 2. 父状态和子状态的关系

- **父子关系**：在本文中，**父状态 $s$** 是当前的 Alpha 公式，**子状态 $s' = \text{child}(s, a)$** 是**对父公式施加了特定精炼动作 $a$ 之后**得到的新公式。
- **逻辑纽带**：父节点的访问次数 $N_s$ 一定大于或等于其所有子节点访问次数的总和（因为每次访问父节点时，未必会走同一个子节点）。
  $$ N_s \ge \sum_{s' \in \text{children}(s)} N_{s'} $$
- **为什么用 $N_s$ 做分子，$N_{s'}$ 做分母？** 这是因为父节点代表了你探索的“总预算”，而子节点代表“该预算在该分支上的消耗”。即使父节点被访问了 100 次（$N_s=100$），如果某个子节点只被访问了 1 次（$N_{s'}=1$），说明这个分支虽然隶属于一个高频节点，但本身还是“处女地”，值得去探索。

#### 3. 标准MCTS的限制与本文的“关键创新”

**标准MCTS的限制**：
在标准的MCTS（如围棋或游戏）中，**只有叶子节点（终结状态）才能被扩展**。这假设了一旦你向下走了一步，就不能回头再对这个节点的父节点进行“同级别”的改动了。

**本文面对的特殊情况**：
在Alpha挖掘中，一个“有前景但不完美”的Alpha公式（内部节点）可能比一个“已经完美”的叶子节点更有继续打磨的价值。作者希望能够**回头反复精炼同一个节点**。

#### 4. 什么是“虚拟扩展”动作 $a_e$ 和“虚拟访问计数”？

为了解决上述问题，作者修改了动作空间。

**（1）什么是虚拟扩展 $a_e$？**
作者在节点 $s$ 原有的“精炼动作”集合 $A(s)$（即将其改造成特定的子节点 $s'$）之外，增加了一个 **“虚拟动作”（Virtual Action） $a_e$**。
这个动作的含义是：**“我不想走任何现有的精炼子路径，我要在这个节点上凭空生成一个全新的、从未出现过的子节点。”**

**（2）为什么要计算虚拟访问计数 $N_{s_e'} = 1 + |C(s)|$？**
这是最关键的地方。因为虚拟动作指向的“新子节点”还没有被生成，它在现实中**根本不存在**，也没有实际的访问记录。但是，UCT公式强制要求分母必须有一个数值 $N_{s'}$。因此，作者必须给它**人为设定一个合成访问次数（Synthetic Visit Count）**。

设 $|C(s)|$ 为节点 $s$ **现有子节点的数量**，则虚拟访问计数定义为：
$$
N_{s_e'} = 1 + |C(s)|
$$

**（3）为什么要这样计算（背后的精妙博弈论）？**
这个公式完美地平衡了“生成新结构”与“精炼旧结构”的吸引力，我们可以分两种情况看：

- **情况A：节点 $s$ 还很小（比如只有 1 个子节点）**
  - 此时 $|C(s)| = 1$，虚拟访问计数 $N_{s_e'} = 1 + 1 = 2$。
  - 分母很小，UCT 的探索项 $\sqrt{\frac{\ln(N_s)}{2}}$ 会非常大。
  - **结论**：算法会非常倾向于点击“虚拟扩展”，生成全新的公式。这符合直觉——你手里选项太少，应该大胆尝试新结构。

- **情况B：节点 $s$ 已经很大（比如已经有 10 个子节点）**
  - 此时 $|C(s)| = 10$，虚拟访问计数 $N_{s_e'} = 1 + 10 = 11$。
  - 分母很大，UCT 的探索项 $\sqrt{\frac{\ln(N_s)}{11}}$ 会相对较小。
  - **结论**：算法会**降低**生成全新公式的冲动，而更倾向于去精炼现有的 10 个子节点中表现好的那个。
  - **直觉**：既然你已经尝试了 10 种不同的精炼方向，说明你对这个父公式已经挖掘得比较充分了，此时“过度探索”新方向不如“深耕”旧方向。


### 4.3 扩展

#### 4.3.1 维度定向精炼建议 

每个节点 $s$ 关联多维评估分数向量 $\pmb{E}_s = [e_1, \ldots, e_q] \in [0, e_{\max}]^q$。

**维度采样概率**（优先改进低分维度）：
$$
P(i^* = i | s) = \mathrm{Softmax}\left( (e_{\max} \cdot \mathbf{1}_q - \mathbf{E}_s) / T \right)_i \tag{3}
$$

其中 $\mathbf{1}_q$ 为 $q$ 维全1向量，$T$ 为温度参数。

选定维度 $i^*$ 后，LLM生成文本精炼建议 $d_{s, i^*}$，以 $\mathcal{F}_{zoo}$ 中的有效Alpha作为少样本示例。

#### 4.3.2 Alpha公式生成与验证

**两步生成过程**：
$$
\begin{array}{rl}
& d_{s, i^*} \sim p_{\mathrm{LLM}}(\cdot | s, i^*, \mathcal{F}_{zoo}) \\
& f_{new} \sim p_{\mathrm{LLM}}(\cdot | d_{s, i^*}, f_s)
\end{array} \tag{5}
$$

- 先生成**概念假设**，再生成**具体公式**
- 确保公式与清晰的投资逻辑一致
- 自动验证 $\mathrm{IsValid}(f_{new})$，若不合法则迭代修正


### 4.4 多维Alpha评估

**相对排名方法**：由于有效Alpha仓库 $\mathcal{F}_{zoo}$ 不断进化，采用相对排名避免固定阈值过严或过松。

指标 $m$ 下的排名：
$$
R(f, m, \mathcal{F}_{zoo}) = \frac{1}{|\mathcal{F}_{zoo}|} \sum_{f' \in \mathcal{F}_{zoo}} \mathbb{I}(m(f) < m(f')) \tag{6}
$$

**评估维度**：
- **Effectiveness (有效性)**：核心预测能力
- **Stability (稳定性)**：预测性能的时间一致性
- **Turnover (换手率)**：交易成本
- **Diversity (多样性)**：与已有因子的新颖性
- **Overfitting Risk (过拟合风险)**：LLM定性评估

各维度（除Overfitting Risk外）得分：
$$
e_i(f) = 1 - R(f, m_i, \mathcal{F}_{zoo}) \tag{7}
$$

**过拟合风险评估**：
$$
e_{\mathrm{overfit}} = \mathrm{LLM}_{\mathrm{eval}}(f, H(s)) \tag{8}
$$

**整体Alpha得分**（作为MCTS奖励信号）：
$$
S(f) = \frac{1}{|\mathcal{D}|} \sum_{i=1}^q e_i(f) \tag{8}
$$


### 4.5 反向传播 (Backpropagation)

更新从根节点到父节点路径上所有节点的统计量：
$$
\begin{array}{r}
N_{s_k} \leftarrow N_{s_k} + 1 \\
Q(s_k, a_k) \leftarrow \max(Q(s_k, a_k), S(f_{new}))
\end{array} \tag{10}
$$

**上下文增强**：LLM获得当前节点的父节点、子节点和兄弟节点的精炼历史，避免冗余建议。

这是一个极其敏锐的问题，直击MCTS（蒙特卡洛树搜索）在**组合优化**与**博弈**场景下的核心区别。为了让你完全通透，我们从“定义本源”和“算法目的”两个角度拆解。

#### 1. 为什么是 $Q \leftarrow \max(Q, S(f_{new}))$？

这是本文区别于传统MCTS（如AlphaGo）的**最核心设计决策**。

- **传统博弈MCTS（求平均）**：在围棋中，$Q$ 代表**胜率**。你从节点 $s$ 出发，走了动作 $a$，后续可能赢也可能输。求平均值 $\frac{\sum V_i}{N}$ 可以客观评估这个动作的“平均实力”，因为对手会抵抗，你无法保证每次都能走到最好的那一步。

- **本文的Alpha挖掘（取最大值 $\max$）**：
  - **数学表达**：$Q(s_k, a_k) \leftarrow \max(Q(s_k, a_k), S(f_{new}))$
  - **逻辑直觉**：Alpha挖掘是一个“寻宝”问题（组合优化），而不是“打仗”问题。当你探索一个公式的某个精炼方向时，你并不关心这个方向“平均”能生成多好的因子，你只关心**“这个方向有没有可能孕育出一颗明珠”。
  - **实际效果**：即使一条路径上生成了 99 个垃圾因子，但只要它曾经生成过 1 个得分极高的因子（$S(f_{new})$ 很高），这个分支的 $Q$ 值就会被拉得很高。这会引导 MCTS 的 UCT 公式 $\arg\max(Q + \text{bonus})$ **死死咬住**这个曾经辉煌过的分支，不断地“深耕细作”，试图在其附近挖出更好的因子。这完美契合了“迭代精炼”的算法目标。

#### 2. 为什么 $N_{s_k} \leftarrow N_{s_k} + 1$ 要加一？

**这与新因子是否被采用无关，而是为了给 MCTS 的“探索策略”记账。**

- $N$（访问次数）在 MCTS 中代表“计算资源的消耗次数”。无论生成的新因子是好是坏，只要你调用了 LLM 生成了一次、回测了一次，你就消耗了一次宝贵的计算预算。
- 更重要的是，$N$ 加一是为了**惩罚“已经探索过的路径”**。

#### 3. 新的因子 $f_{new}$ 一定会被采用吗？

不一定会被采用。
你观察到的 $Q$ 更新和 $N$ 更新，仅仅发生在 **MCTS 内部的数据结构（搜索树）** 中，用于指导*下一次*搜索的方向。这与“是否被纳入最终的投资策略”是**两码事**。

### 4.6 频繁子树避免 (Frequent Subtree Avoidance, FSA)

**动机**：缓解Alpha公式同质化，防止过度利用常见结构模式。

**定义**：
- **根基因 (root gene)**：表达式树中**叶节点全为原始输入特征**的子树
- **抽象化**：使用 $\mathrm{Abs}()$ 操作符将具体参数值抽象化
	- 例如：$\mathrm{Abs}(\mathrm{Ma}(vwap, 20)) \rightarrow \mathrm{Ma}(vwap, t)$
- 抽象化根基因集合：$\bar{\mathcal{G}}(f)$

**支持度计算**：
$$
\mathrm{Support}(\bar{g}) = \frac{1}{|\mathcal{F}_{zoo}|} \sum_{f' \in \mathcal{F}_{zoo}} \mathbb{I}(\bar{g} \subseteq \bar{\mathcal{G}}(f')) \tag{11}
$$

- 选择 Top-$k$ 最频繁的**闭合根基因**（无超树具有相同支持度）作为 $\mathcal{G}_{\mathrm{forbidden}}$
- 生成时约束：
$$
\mathrm{Constraint}: \bar{\mathcal{G}}(f_{new}) \cap \mathcal{G}_{\mathrm{forbidden}} = \emptyset \tag{12}
$$

**作用**：作为正则化手段，引导MCTS搜索向结构更多样化的区域探索。


## 五、实验 (Experiments)

### 5.1 研究问题 (RQs)

- **Q1**：框架在预测性能上如何对比基线？
- **Q2**：MCTS和FSA是否为有效组件？
- **Q3**：挖掘的Alpha公式可解释性如何？

### 5.2 实验设置 (Settings)

**数据**：
- 中国A股市场：CSI 300（大盘股）、CSI 1000（中小盘股）
- 预测目标：10日收益、30日收益
- 训练期：2011/01/01–2020/12/31
- 测试期：2021/01/01–2024/11/30

**基线方法**：

| 类别        | 方法                                                         |
| --------- | ---------------------------------------------------------- |
| **传统方法**  | DSO (Deep Symbolic Optimization), GP, AlphaGen, AlphaForge |
| **LLM方法** | CoT chain-of- thought, ToT tree-of-thought, FAMA           |

- 所有LLM方法使用GPT-4.1
- **公平比较策略**：基于"搜索计数"（生成和评估的唯一Alpha公式数量）控制计算预算
	- LLM方法：1,000/2,000/3,000次搜索
	- 传统方法：逐步增加至600,000次
1. **搜索次数设置**（3k vs 600k）：是为了证明 LLM 拥有**碾压级的“样本效率”**——用传统方法 1/200 的尝试次数，就能找到更优解。
2. **LLM 模型的区别**：基线统一用 GPT-4.1 是为了**公平比算法**；附录里换不同模型是为了**证明框架的通用性**，并给大家提供**“成本-性能”的选型指南。
3. 
### 5.3 实验1：预测性能比较

**评估模型**：
- LightGBM (Ke et al. 2017)
- 3层MLP

**最后构建投资组合的Alpha集大小**：10、50、100

**评估指标**：
- IC, RankIC（预测能力）
- Annualized Excess Return (AER), Information Ratio (IR)（交易表现）

**结果**（Figure 4）：
- 本文框架在所有指标上**一致优于基线**
- 挖掘的Alpha在预测未来收益方面具有**优越的预测能力**，并能有效转化为**交易盈利**
- 结果在**美股市场**（S&P 500）上得到验证（Appendix H）


### 5.4 实验2：消融研究 (Ablation Study)

**评估组件**：MCTS、多维反馈、FSA

**Table 1 关键发现**：

| 配置 | IC | RankIC | AER | IR (LightGBM) |
|------|-----|--------|-----|---------------|
| CoT | 0.0434 | 0.0395 | 0.0707 | 0.7461 |
| ToT | 0.0459 | 0.0427 | 0.0868 | 0.9337 |
| MCTS (仅Eff.+Div.) | 0.0501 | 0.0476 | 0.1003 | 1.0106 |
| MCTS (全维度) | 0.0515 | 0.0479 | 0.1075 | 1.1121 |
| **MCTS+FSA (全维度)** | **0.0549** | **0.0512** | **0.1107** | **1.1792** |

**结论**：
- MCTS优于CoT和ToT
- 多维反馈逐步提升性能（Turnover反馈虽略降IC，但提升AER和IR）
- FSA进一步提升所有指标
- 所有组件均有独立贡献

**搜索动态分析** (Figure 5)：
- 搜索效率：本文框架（即使无FSA）高于其他LLM方法，FSA进一步放大优势
- 样本外性能：提升的样本内性能有效转化为更优的样本外表现

**成本-性能分析** (Figure 6)：
- 统一估算运行成本（服务器+API费用）
- 本文框架达到最高IR等值线
- 轻量模型（Gemini-2.0-flash-lite）以 $7.5 成本实现IR=1.27
- 提供性能与预算的灵活平衡


### 5.5 实验3：Alpha公式可解释性

**评估方法**：
- 每方法随机选1个Alpha，LLM排序可解释性
- 重复50次，聚合3个不同LLM的排名

**结果** (Figure 7)：
- 本文框架生成的Alpha公式**可解释性排名第二**（仅次于CoT）
- 显著优于非LLM基线方法（其公式持续排名最低）
- **定性分析** (Table 7)：
	- 本文公式：如 $\mathrm{Zscore}(\mathrm{Ma}(close - vwap, 20), 30)$ 可解释为"异常日内动量"度量
	- 非LLM公式：存在**量纲不一致**（如将成交量与价格对数相加）、统计上可疑的操作（如对二值序列取标准差）
	- AlphaForge公式：嵌套三角函数应用，缺乏经济理论基础


## 六、相关工作 (Related Work)

### 6.1 自动化公式化Alpha挖掘

| 方法 | 核心技术 |
|------|----------|
| GPLearn (Lin et al. 2019) | 遗传编程 + 时间序列操作符 |
| AutoAlpha (Zhang et al. 2020) | 分层进化算法 |
| AlphaEvolve (Cui et al. 2021) | 计算图进化 |
| AlphaGen (Yu et al. 2023) | 强化学习优化Alpha集 |
| AlphaForge (Shi et al. 2024) | 深度学习生成-预测架构 |
| FAMA (Li et al. 2024b) | LLM + 上下文学习 + 经验链 |
| AlphaAgent (Tang et al. 2025) | LLM + 启发式搜索 |

### 6.2 树搜索推理 (Tree Search-based Reasoning)

- LATS (Zhou et al. 2023)：将LLM视为通用智能体，在推理和动作层面探索
- 本文区别：将Alpha发现建模为**形式化推理任务**，通过MCTS系统性探索**结构化数学公式空间**


## 七、结论 (Conclusion)

- 提出**LLM驱动的MCTS框架**用于公式化Alpha挖掘
- 将Alpha挖掘建模为树搜索，LLM迭代生成和精炼候选公式
- **关键创新**：定量回测反馈引导 + 频繁子树避免机制
- 实验证明：
	- 优越的预测准确性和交易表现
	- 增强的可解释性
	- 更高的搜索效率
- 为LLM+MCTS在金融自动化Alpha挖掘中的应用开辟了新方向

## 八、附录

### 8.1 方法细节 (Appendix D)

**Alpha生成的两步过程**：
1. **Alpha画像 (Alpha Portrait)**：名称、投资逻辑描述、伪代码表达式
2. **公式生成**：从画像生成具体公式，使用符号参数（非固定数值）
	- 提出多组参数值（3组），选择最佳配置

**精炼建议生成**：
- Effectiveness/Stability：过滤高相关性Alpha后选择Top-$k$ 示例
- Diversity：选择与当前Alpha最低相关性的示例
- Turnover/Overfitting Risk：零样本直接提示

**动态搜索预算分配**：
- 初始预算 $B$，增量 $b$
- 发现"突破"（新Alpha得分创新高）时增加预算
- 防止过早放弃有前景的搜索路径

### 8.2 操作符列表 (Appendix E)

| 类型 | 操作符 |
|------|--------|
| **一元** | $-x$, $\|x\|$, $x^2$, $1/x$, Sign, Sin, Cos, Tanh, Log, Delay, Diff, Pct, Ma, Med, Sum, Std, Max, Min, Rank, Skew, Kurt, Vari, Autocorr, Zscore |
| **二元** | $x+y$, $x-y$, $x\cdot y$, $x/y$, Greater, Less, Cov, Corr |

### 8.3 有效Alpha检查标准 (Appendix G)

**基本标准**：
- RankIC $\geq 0.015$
- RankIR $\geq 0.3$
- $R_{\text{RankIC}} \leq 0.95$
- $R_{\text{RankIR}} \leq 0.95$

**换手率标准**：日换手率 $\leq 1.6$

**多样性标准**：与仓库中Alpha的最大相关性 $< 0.8$

### 8.4 数据泄露调查 (Appendix H)

**实验设计**：
- 直接提示LLM（GPT-4.1, Gemini2.0-flash-lite, Deepseek-v3）生成"高性能"Alpha
- 与随机基线和本文框架对比

**结果 (Table 5)**：
- 直接提示生成的Alpha性能与随机基线无显著差异
- 均远低于本文框架
- **结论**：LLM未利用泄露的先验知识

### 8.5 成本估算详情 (Appendix H)

**服务器成本**：
- CPU服务器（LLM方法）：$2.07/小时
- GPU服务器（非LLM方法）：$3.076/小时

**API成本**：

| 模型 | 输入 ($/M) | 输出 ($/M) |
|------|------------|------------|
| GPT-4.1 | 2.00 | 8.00 |
| Gemini-2.0-flash-lite | 0.075 | 0.30 |
| Deepseek-v3 | 0.071 | 1.14 |

### 8.6 关键Prompt设计 (Appendix K)

1. **Alpha画像生成**：生成含名称、描述、伪代码的Alpha画像
2. **Alpha公式生成**：从画像生成可执行的公式结构
3. **过拟合风险评估**：LLM基于公式和历史评分（0-10分）
4. **Alpha精炼**：基于精炼建议改进原公式

### 8.7 局限性 (Appendix J)

1. **新颖性差距**：与人类专家设计的Alpha在复杂性和新颖性上仍有差距
2. **多样性约束**：受限于LLM内部知识库，搜索空间广度受限
3. **扩展性挑战**：极端大规模Alpha挖掘任务面临挑战


## 九、两篇论文的核心差异对比

| 维度 | Chain-of-Alpha | LLM+MCTS (本文) |
|------|----------------|-----------------|
| **搜索架构** | 双链并行（生成链 + 优化链） | MCTS树搜索 |
| **核心机制** | 链式迭代生成与优化 | UCT选择 + 维度假发精炼 |
| **多样性维护** | 通过废弃池和多样性评分 | 频繁子树避免 (FSA) |
| **并行性** | 高度并行（优化链独立） | 受限（树搜索的序列性） |
| **复杂度** | $\mathcal{O}(Km)$，$m \leq 5$ | $\mathcal{O}(b^d)$ 指数级 |
| **过拟合控制** | 基于阈值检查 | LLM定性评估 (ORS) |
| **参数生成** | LLM直接生成具体参数 | 符号参数 + 多组参数值枚举 |
![[Pasted image 20260803112842.png]]


## Alpha Portrait Generation Prompt

```
Task Description:

You are a quantitative finance expert specializing in factor-based investing. Please design an alpha factor used in investment strategies according to the following requirements, and then provide the content of the alpha in the required format.

Available Data Fields:

The following data fields are available for use:
{available_fields}

Available Operators:

The following operators are available for use:
{available_operators}

Alpha Requirements:

1. The alpha value should be dimensionless (unitless).

2. The alpha should incorporate at least two distinct operations from the ”Available Operators” list to ensure it has sufficient complexity.Avoid creating overly simplistic alphas.

3. All look-back windows and other numerical parameters used in the alpha calculation MUST be represented as named parameters in the pseudo-code. These parameter names MUST follow Python naming conventions (e.g., lookback period, volatility window, smoothing factor).

4. The alpha should have NO MORE than 3 parameters in total.

5. The pseudo-code should represent the alpha calculation step-by-step, using only the ”Available Operators” and clearly defined parameters. Each line in the pseudo-code should represent a single operation.

6. Use descriptive variable names in the pseudo-code that clearly indicate the data they represent.

7. When designing alpha expressions, try to avoid including the following sub-expressions:
{freq_subtrees}

Formatting Requirements:

The output must be in JSON format with three key-value pairs:
1. ”name”: A short, descriptive name for the alpha (following Python variable naming style, e.g., price volatility ratio).
2. ”description”: A concise explanation of the alpha’s purpose or what it measures. Avoid overly technical language. Focus on the intuition behind the alpha.
3. ”pseudo code”: A list of strings, where each string is a line of simplified pseudo-code representing a single operation in the alpha calculation. Each line should follow the format: variable name = op name(input=[input1, input2, ...], param=[param1, param2, ...]), where:
• variable name is the output variable of the operation.
• op name is the name of one of the ”Available Operators”.
• input1, input2, . . . are input variables (either from ”Available Data Fields” or previously calculated variables, cannot be of a numeric type).
• param1, param2, . . . are parameter names defined in the alpha requirements.

The format example is as follows:
{
"name": "volatility_adjusted_momentum",
"description": "......",
"pseudo_code": [......]
}

```

## Alpha Formula Generation Prompt

```
Task Description:

Please design a quantitative investment alpha expression according to the following requirements.

Available Data Fields:

• The following data fields are available for use: {available fields}

Available Operators:

• The following operators are available for use: {available operators}

Alpha Requirements:

• {alpha portrait prompt}

Formatting Requirements:

1. Provide the output in JSON format.
2. The JSON object should contain two fields: "formula", and "arguments".
• "formula": Represents the mathematical expression for calculating the alpha.
• "arguments": Represents the configurable parameters of the alpha.
3. "formula" is a list of dictionaries. Each dictionary represents a single operation and must contain four keys: "name", "param", "input", and "output".
• "name": The operator’s name (a string), which MUST be one of the operators provided in the ”Available Operators” section.
• "param": A list of strings, representing the parameter names for the operator. These parameter names MUST be used as keys in the "arguments" section.
• "input": A list of strings, representing the input variable names for the operator. These MUST be data fields from the ”Available Data Fields” or output variables from previous operations in the "formula", cannot be of a numeric type.
• "output": A string, representing the output variable name for the operator. This output can be used as an input for subsequent operations.
4. "arguments" is a list of dictionaries. Each dictionary represents a set of parameter values for the alpha.
• The keys of each dictionary in "arguments" MUST correspond exactly to the parameter names defined in the "param" lists of the "formula".
• The values in each dictionary in "arguments" are the specific numerical values for the parameters.
5. You may include a maximum of 3 sets of parameters within the "arguments" field.
6. The parameter value that indicates the length of the lookback window (if applicable) must be within the {window range} range.
7. Ensure that the alpha expression is both reasonable and computationally feasible.
8. Parameter names should be descriptive and follow Python naming conventions (e.g., window size, lag period, smoothing factor). Avoid using single characters or numbers as parameter names.
9. Refer to the following example:
{
"formula": [......],
"arguments": [......]
}

```

## Alpha Overfitting Risk Assessment Prompt

```
Task: Critical Alpha Overfitting Risk Assessment

Critically evaluate the overfitting risk and generalization potential of the provided quantitative investment alpha, based on its expression and refinement history. Your assessment must focus on whether complexity and optimization appear justified or are likely signs of overfitting.

Input:
• Alpha Expression:
{alpha_formula}
• Refinement History:
{refinement_history}

Evaluation Criteria:
1. Justified Rationale vs. Complexity:
Critique: Is the complexity of the alpha expression plausibly justified by an inferred economic rationale, or does it seem arbitrary/excessive, suggesting fitting to noise?
2. Principled Development vs. Data Dredging:
Critique: Does the refinement history indicate hypothesis-driven improvements, or does it suggest excessive optimization and curve-fitting (e.g., frequent, unjustified parameter tweaks)?
3. Transparency vs. Opacity:
Critique: Is the alpha’s logic reasonably interpretable despite its complexity, or is it opaque, potentially masking overfitting?

Scoring & Output:
• Assign a single Overfitting Risk Score from 0 to 10.
– 10 = Very Low Risk (High confidence in generalization)
– 0 = Very High Risk (Low confidence in generalization)
• Use the full 0-10 range to differentiate risk levels effectively.
• Provide a concise, one-sentence Justification explaining the score, citing the key factors from the criteria.
• Format the output as JSON, like the examples below:

Example JSON Outputs:
{
"reason": "Complexity is justified by a strong rationale; principled refinement
history suggests low risk.",
"score": 9
}
{
"reason": "Plausible rationale, but some expression opacity and parameter tuning in history indicate moderate risk.",
"score": 5
}
{
"reason": "High risk inferred from opaque expression lacking clear rationale, supported by history showing excessive tuning.",
"score": 1
}

```

## Alpha Refinement Prompt

```
Task Description:

There is an alpha factor used in quantitative investment to predict asset price trends. Please improve it according to the following suggestions and provide the improved alpha expression.

Available Data Fields:
The following data fields are available for use: {available fields}

Available Operators:
The following operators are available for use:{available operators}

Alpha Suggestions:
1. The alpha value should be dimensionless (unitless).
2. All look-back windows and other numerical parameters used in the alpha calculation MUST be represented as named parameters in the pseudo-code. These parameter names MUST follow Python naming conventions (e.g., lookback period, volatility window, smoothing factor).
3. The alpha should have NO MORE than 3 parameters in total.
4. The pseudo-code should represent the alpha calculation step-by-step, using only the ”Available Operators” and clearly defined parameters. Each line in the pseudo-code should represent a single operation.

5. Use descriptive variable names in the pseudo-code that clearly indicate the data they represent.

6. When designing alpha expressions, try to avoid including the following sub-expressions: {freq subtrees}

Original alpha expression:

{origin_alpha_formula}

Refinement suggestions:

NOTE: The following improvement suggestions do not need to be all adopted; they just need to be considered and reasonable ones selected for adoption.

{refinement_suggestions}

Formatting Requirements:

The output must be in JSON format with three key-value pairs:

1. ”name”: A short, descriptive name for the alpha (following Python variable naming style, e.g., price volatility ratio).

2. ”description”: A concise explanation of the alpha’s purpose or what it measures. Avoid overly technical language. Focus on the intuition behind the alpha.

3. ”pseudo code”: A list of strings, where each string is a line of simplified pseudo-code representing a single operation in the alpha calculation. Each line should follow the format: variable name = op name(input=[input1, input2, ...],

param=[param1, param2, ...]), where:

• variable name is the output variable of the operation.

• op name is the name of one of the ”Available Operators”.

• input1, input2, ... are input variables (either from ”Available Data Fields” or previously calculated variables, cannot be of a numeric type).

• param1, param2, ... are parameter names defined in the alpha requirements.

The format example is as follows:

{

"name": "volatility_adjusted_momentum",
"description": "......",
"pseudo_code": [......]

}

```