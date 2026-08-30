

## 一、摘要

本文提出 **Chain-of-Alpha**，一种基于大语言模型（LLM）的自动化公式化Alpha因子挖掘框架。核心贡献包括：

- 采用**双链架构**：因子生成链（Factor Generation Chain）和因子优化链（Factor Optimization Chain）
- 仅使用市场数据，迭代生成、评估和优化候选Alpha因子
- 利用回测反馈和先验优化知识，无需人工干预
- 在A股市场（CSI 500、CSI 1000）上验证了有效性和可扩展性


## 二、引言 (Introduction)

### 2.1 研究背景

- **量化交易**：基于数学模型、统计分析和算法执行的系统性交易方法
- **Alpha因子挖掘**：发现能够预测资产收益、超越市场系统性风险的信号
- **公式化Alpha vs. 神经Alpha**：公式化方法更具可解释性、鲁棒性、泛化能力和数据效率

### 2.2 现有LLM方法的三点局限性

| 问题 | 具体表现 |
|------|----------|
| **非完全自动化** | AlphaGPT、AlphaAgent依赖人工反馈；FAMA依赖预存在因子进行上下文学习 |
| **通用性受限** | 部分方法依赖多模态输入；QuantAgent关注策略生成而非因子发现 |
| **效率低下** | LLM+MCTS因树搜索的序列性质，可并行性差，扩展性受限 |

### 2.3 本文贡献

1. 提出 **Chain-of-Alpha** 框架：新颖、简洁、高效、全自动
2. 设计**双链协同机制**：生成链保证多样性，优化链保证质量
3. 在A股真实数据集上验证有效性，提供深入分析

---

## 三、相关工作

### 3.1 LLM推理能力

- **CoT prompting**：引导模型逐步推理（Wei et al. 2022, 2023）
- **自洽性**（Wang et al. 2023b）
- **结构化推理**：树式（Yao et al. 2023a）、图式（Besta et al. 2024; Cao 2024）
- **推理-行动集成**：ReAct（Yao et al. 2023b）、Toolformer（Schick et al. 2023）、SciAgent（Ma et al. 2024）、RAG-Star（Jiang et al. 2024）

### 3.2 LLM在量化交易中的应用

- **交易决策代理**：FinMem（Yu et al. 2023b）、MASS（Guo et al. 2025）
- **Alpha挖掘**：FAMA（Li et al. 2024）、LLM+MCTS（Shi, Duan, and Li 2025）

### 3.3 传统公式化Alpha挖掘方法

| 方法 | 核心技术 |
|------|----------|
| GPLearn（Lin et al. 2019）| 遗传编程（GP）|
| AutoAlpha（Zhang et al. 2020）| 分层进化算法 |
| AlphaEvolve（Cui et al. 2021）| 学习框架 |
| AlphaGen（Yu et al. 2023a）| 强化学习优化因子集 |
| AlphaForge（Shi et al. 2024）| 生成-预测架构 |

**LLM增强方法**：AlphaGPT、AlphaAgent（人机交互）；QuantAgent（双环LLM+知识库）；FAMA（上下文学习）；LLM+MCTS。

## 四、预备知识 

### 任务定义：日截面股票收益预测

**输入**：
- 股票池：$n$ 只股票，$T$ 个交易日
- 特征张量：$\mathbf{X} \in \mathbb{R}^{T \times n \times m}$（包含开盘价、最高价、最低价、收盘价、成交量等）
- 未来收益矩阵：$\mathbf{Y} \in \mathbb{R}^{T \times n}$，$y_{i,t}$ 为股票 $i$ 在 $t$ 日后 $h$ 天的收益

**Alpha因子**：映射历史特征到预测信号
$$
\mathbf{v}_t = f(\mathbf{X}_{t-\tau+1:t}) \in \mathbb{R}^n
$$
其中 $\tau$ 为回溯窗口。

**目标**：发现 $K$ 个Alpha因子 $\mathcal{F} = \{f_1, \ldots, f_K\}$，通过组合模型 $g$ 得到复合信号
$$
\mathbf{z}_t = g(\{\mathbf{v}_{k,t}\}_{k=1}^K; \theta_g) \in \mathbb{R}^n
$$

**双层优化问题**：
- 内层：给定 $\mathcal{F}$，学习最优组合参数 $\theta_g^*$
- 外层：搜索最优因子集 $\mathcal{F}^*$ 最大化复合信号性能

![[Pasted image 20260803132153.png]]

## 五、方法论

### 5.1 因子选择

每个因子通过回测得到四维评分 $\mathbf{Score} = [S, C, E, D]$：

| 维度 | 符号 | 度量指标 | 含义 |
|------|------|----------|------|
| **强度 (Strength)** | $S$ | RankIC | 预测能力 |
| **一致性 (Consistency)** | $C$ | RankICIR | 时间稳定性 |
| **效率 (Efficiency)** | $E$ | Turnover | 交易成本（越低越好）|
| **多样性 (Diversity)** | $D$ | $1 - \text{Corr}(f, f_k)$ | 与已有因子的低相关性 |

评分公式：
$$
\mathbf{Score} = \mathbf{Evaluate}(f) \tag{1}
$$

有效性检查：
$$
\mathbf{E} = \mathrm{Check}(f, \mathbf{S}) \tag{2}
$$

- 有效因子 $\to$ 候选池 $\mathcal{F}^e$
- 无效因子 $\to$ 废弃池 $\mathcal{F}^d$（作为负面参考）

### 5.2 因子生成链

**目标**：最大化**多样性**

每次迭代，LLM基于候选池、废弃池和外部提示词（数据域，运算符，任务特殊引导）生成新因子：
$$
f^{\text{seed}} = \mathrm{LLM}(\mathcal{F}^e, \mathcal{F}^d \mid \mathcal{P}_{\text{generation}}) \tag{3}
$$

生成过程形式化：
$$
f_{k+1}^{\text{seed}} = \mathrm{Chain\text{-}of\text{-}Alpha}_{\text{generation}}(f_1^{\text{seed}}, f_2^{\text{seed}}, \ldots, f_k^{\text{seed}}) \tag{4}
$$

**特点**：
- 自演化链：已有因子指导新因子生成
- 可无限自主运行
- 产生多样化数学结构和行为特征

### 5.3 因子优化链

**目标**：最大化**有效性**

给定种子因子 $f_k^{\text{seed}}$，LLM基于回测反馈 $\mathcal{B}$ 生成优化变体序列：
$$
f_k^{(m+1)} = \mathrm{LLM}(f_k^{\text{seed}}, f_k^{(m)}, \mathcal{B}, H_k \mid \mathcal{P}_{\text{optimization}}) \tag{5}
$$

其中 $H_k$ 为优化历史。

优化过程形式化：
$$
f_k^{(m+1)} = \mathrm{Chain\text{-}of\text{-}Alpha}_{\text{optimization}}(f_k^{\text{seed}}, f_k^{(1)}, \ldots, f_k^{(m)}) \tag{6}
$$

**特点**：
- 反馈感知：低IC $\to$ 增强信号强度；低RankICIR $\to$ 改善时间稳定性
- 上限迭代次数，无有效变体则提前终止
- **高度可并行**：不同种子因子的优化链互不干扰

**复杂度分析**：
- 优化阶段：$\mathcal{O}(Km)$，$K$ 为种子因子数，$m$ 为每因子优化步数（$m \leq 5$）
- 树搜索方法：$\mathcal{O}(b^d)$ 每因子，指数级复杂度且难以并行

### 5.4 Alpha集成与建模

**因子信号生成**：
$$
\mathbf{v}_{k,t} = f_k(\mathbf{X}_{t-\tau+1,t}) \in \mathbb{R}^n \tag{7}
$$

**复合信号**：
$$
\mathbf{z}_t = g(\{\mathbf{v}_{k,t}\}_{k=1}^K; \theta_g) \in \mathbb{R}^n \tag{8}
$$

**信号矩阵**：
$$
\mathbf{Z}(\mathcal{F}, \theta_g) \in \mathbb{R}^{T \times n} \tag{9}
$$

**优化目标**：
$$
\theta_g^*(\mathcal{F}) = \arg\max_{\theta_g} \mathcal{P}(\mathbf{Z}(\mathcal{F}, \theta_g), \mathbf{Y}) \tag{10}
$$

其中 $\mathcal{P}$ 为性能度量（如IC、夏普比率、组合收益）。


## 六、实验

### 6.1 实验设置

**数据集**：
- A股市场：CSI 500（中盘）、CSI 1000（小盘）
- 时间范围：2010-01-01 至 2025-06-30
	- 训练集：2010-01-01 至 2019-12-31
	- 验证集：2020-01-01 至 2021-12-31
	- 测试集：2022-01-01 至 2025-06-30
- 预测周期：10个交易日

**基线方法**：

| 类别 | 方法 |
|------|------|
| **经典因子** | Alpha 101, Alpha 158, Alpha 360 |
| **传统方法** | GP, DSO, AlphaGen, AlphaForge |
| **LLM方法** | LLM+CoT, LLM+ToT, LLM+MCTS |

**控制条件**：所有方法生成最多1000个候选因子，选择Top 100（按RankIC）



### 6.2 主要结果

**Table 1 核心发现**：

| 指标 | Chain-of-Alpha (CSI 500) | Chain-of-Alpha (CSI 1000) |
|------|--------------------------|---------------------------|
| IC | 0.0485 | 0.0672（最优）|
| RankIC | 0.0771 | 0.0902（最优）|
| ICIR | 0.3047 | 0.4630（最优）|
| RankICIR | 0.5013 | 0.6228（最优）|
| **AR** | **0.1324（最优）** | **0.1471（最优）** |
| **IR** | **1.4178（最优）** | **1.4043（最优）** |

**关键结论**：
- Chain-of-Alpha在12个指标中取得10项最优
- AR和IR（最直接反映投资表现的指标）均显著领先
- 树式探索（ToT、MCTS）并非必要，链式方法更高效



### 6.3 消融实验

**Table 2 结果（CSI 1000）**：

| 配置 | IC | RankIC | ICIR | RankICIR | AR | IR |
|------|-----|--------|------|----------|----|----|
| 完整Chain-of-Alpha | 0.0672 | 0.0902 | 0.4630 | 0.6228 | 0.1471 | 1.4043 |
| 仅生成链 | 0.0586 | 0.0867 | 0.4078 | 0.6203 | 0.1346 | 1.3492 |
| 仅优化链 | 0.0620 | 0.0847 | 0.4464 | 0.6152 | 0.1181 | 1.2625 |

**结论**：两条链缺一不可，协同工作实现最佳性能



### 6.4 LLM骨干模型影响

**Table 3 结果（CSI 1000）**：

| 骨干模型 | IC | RankIC | ICIR | RankICIR | AR | IR |
|----------|-----|--------|------|----------|----|----|
| 最佳基线 | 0.0655 | 0.0889 | 0.4602 | 0.6152 | 0.1325 | 1.3342 |
| GPT-4o | 0.0672 | 0.0902 | 0.4630 | 0.6228 | 0.1471 | 1.4043 |
| DeepSeek-V3 | 0.0671 | 0.1011 | 0.4492 | 0.6063 | **0.1517** | 1.4020 |
| Qwen3-32B | 0.0653 | 0.0939 | 0.4597 | **0.6342** | 0.1365 | **1.5804** |

**结论**：Chain-of-Alpha与骨干模型无关，更强LLM可进一步提升性能



### 6.5 策略收益可视化

- **Figure 2(a)**：累计绝对收益 — Chain-of-Alpha持续领先，2023年中后差距扩大
- **Figure 2(b)**：相对于基准的超额收益 — Chain-of-Alpha全程领先

### 6.6 案例研究

**Table 4** 展示了代表性因子示例，包括符号表达式、自然语言描述和评价指标。生成因子具有：
- 可解释性
- 多样性
- 统计显著的预测能力（高RankIC和RankIR）
- 捕获多种市场行为（VWAP偏离、成交量调整波动率、价格-成交量秩相关）

### 6.7 效率分析 

**Chain-of-Alpha效率优势**：

| 维度 | Chain-of-Alpha | 树搜索方法 (ToT/MCTS) |
|------|----------------|----------------------|
| 时间复杂度 | $\mathcal{O}(Km)$ | $\mathcal{O}(b^d)$ 每因子 |
| 并行性 | 高度并行（独立优化链）| 受节点依赖限制 |
| 人工干预 | 无 | 部分方法需人工反馈 |

## 七、结论

- Chain-of-Alpha是LLM驱动的自动化Alpha挖掘框架
- **新颖性**：双链设计（生成+优化）协同工作
- **高效性**：相比CoT、ToT、MCTS更具可扩展性
- 实验结果验证了LLM在量化金融中的巨大潜力

---

## 八、附录关键内容

### 8.1 Alpha选择标准

| 指标 | 阈值 |
|------|------|
| RankIC | $\geq 0.015$ |
| RankICIR | $\geq 0.2$ |
| Turnover | $\leq 1.5$ |
| Diversity | $\geq 0.2$ |

默认选择Top 100因子进入集成模型。

### 8.2 模型配置

- **LLM**：GPT-4o (gpt-4o-2024-11-20)，温度=1.0
- **集成模型**：LightGBM
	- Loss: MSE, early stopping=200轮, leaves=24, estimators=2000, max_depth=8, lr=0.005, $L_1=0.1$, $L_2=0.1$

### 8.3 回测策略

- Top-10%等权组合，预测周期 $w$ 内最多允许 $n = k/w$ 只股票调仓！
- 交易成本：开仓0.03%，平仓0.1%

### 8.4 评估指标公式

**信息系数 (IC)**：
$$
\mathrm{IC}_t = \mathrm{Corr}(f_{1:N_t,t}, r_{1:N_t,t+1}) \tag{11}
$$
$$
\mathrm{IC} = \frac{1}{T}\sum_{t=1}^{T}\mathrm{IC}_t \tag{12}
$$

**秩信息系数 (RankIC)**：
$$
\mathrm{RankIC}_t = \mathrm{Corr}(\mathrm{rank}(f_{1:N_t,t}), \mathrm{rank}(r_{1:N_t,t+1})) \tag{13}
$$
$$
\mathrm{RankIC} = \frac{1}{T}\sum_{t=1}^{T}\mathrm{RankIC}_t \tag{14}
$$

**IC信息比率 (ICIR)**：
$$
\mathrm{ICIR} = \frac{\mathrm{IC}}{\mathrm{Std}(\mathrm{IC}_t)} \tag{15}
$$

**RankIC信息比率 (RankICIR)**：
$$
\mathrm{RankICIR} = \frac{\mathrm{RankIC}}{\mathrm{Std}(\mathrm{RankIC}_t)} \tag{16}
$$

**年化收益 (AR)**：
$$
R_{p,t+1} = \frac{1}{k}\sum_{i \in \mathrm{Top}_k} r_{i,t+1} \tag{17}
$$
$$
\mathrm{AR} = \left(\frac{1}{T_p}\sum_{j=1}^{T_p} R_{p,j}\right) \times P \tag{18}
$$

**信息比率 (IR)**：
$$
\mathrm{IR} = \frac{\mathrm{AR}}{\sigma(R_p)\sqrt{P}} \tag{19}
$$

**换手率 (Turnover)**：
$$
\mathrm{Turnover} = \frac{|\mathrm{Top}_k(t) \triangle \mathrm{Top}_k(t-1)|}{k} \tag{20}
$$
$\triangle$ denotes the symmetric difference between the top-k sets at time t and t−1.

**多样性 (Diversity)**：
$$
\mathrm{Diversity}(f) = 1 - \frac{1}{k}\sum_{i=1}^{k}|\rho(f, g_i)| \tag{21}
$$
$\rho(f,g_{i})$ is the average Spearman correlation between $f$ and $g_i$ over all trading days
### 8.5 数据字段与操作符

**数据字段**：开盘价($open$)、最高价($high$)、最低价($low$)、收盘价($close$)、成交量($volume$)、成交额($amount$)、价格变化($change$)、VWAP($swap$)

**操作符分类**：
- **数学运算**：Add, Sub, Mul, Div, Log, Abs, Power, Sign
- **时间序列**：Mean, Std, Var, Sum, Max, Min, Med, Mad, Rank, Quantile, Count, Ref
- **回归**：Delta, IdxMax, IdxMin
- **统计**：Resi, Slope, Rsquare, Skew, Kurt, Corr, Cov
- **逻辑**：If, Gt, Lt, Ge, Le, Eq, Ne, And, Or, Not

## Prompt of Seed Factor Generation

```
# Objective

You are an expert in alpha factor generation for quantitative trading.

Your task is to design a new **cross-sectional alpha factor**—a mathematical expression that assigns a score to each stock on

a given trading day based on its recent market data.

This score will be used to rank and select stocks on the same day (i.e., across the market cross-section).

You are provided with:

- A list of available data fields and operators.

- Example sets of **effective** and **non-effective** factors.

Your goal is to generate a factor expression that is:

- Distinct from the examples

- Inspired by effective ones

- Potentially more predictive than the non-effective ones

# Available Data Fields

You may use the following data fields to construct the expression.

Make sure to include a dollar sign ($) before each field name in your expression:

{available_data_fields}

# Available Operators

You may use the following operators to construct your factor expression:

{available_operators}

# Reference Factors

Use the following for reference:

**Effective factors:**

{effective_factors}

**Non-effective factors:**

{non_effective_factors}

# Requirements

1. Take inspiration from effective factors and avoid patterns seen in non-effective ones (e.g., poor RankIC or IR).

2. The factor should produce a unitless value, independent of price scale or volume units.

3. Use **only** the provided data fields and operators.

4. You are **not** required to use all data fields. The factor expression should have NO MORE than 3 data fields in total.

5. Avoid excessive operator nesting or overly complex expressions to reduce overfitting risk.

6. The expression should be concise, readable, and interpretable.

# Response Format

You need to return a JSON object with the following fields:

- factor_name: a brief name of the factor

- factor_expression: the expression of the factor

- description: a 1-2 sentence description of the factor's intuition or financial meaning
```

## Prompt of Factor Optimization

```
# Objective

You are an expert in alpha factor optimization for quantitative trading.

Your task is to optimize an existing **cross-sectional alpha factor**—a mathematical expression that assigns a score to each

stock on a given trading day based on its recent market data.

This score will be used to rank and select stocks on the same day (i.e., across the market cross-section).

You are provided with:

- A list of available data fields and operators.

- The existing factor expression and its performance (e.g., RankIC, RankIR, Turnover, Diversity).

- The optimization history of the factor.

Your goal is to optimize the factor expression to improve its performance.

# Available Data Fields

You may use the following data fields to construct the expression.

Make sure to include a dollar sign ($) before each field name in your expression:

{available_data_fields}

# Available Operators

You may use the following operators to construct your factor expression:

{available_operators}

# Existing Factor Information

The factor name is: {factor_name}

The factor expression is: {factor_expression}

The factor description is: {description}

The RankIC is: {rankic}

The RankIR is: {rankicir}

The Turnover is: {turnover}

The Diversity is: {diversity}

# Optimization History

The optimization history is:

{optimization_history}

# Requirements

1. A good factor should have: RankIC > 0.015, RankIR > 0.2, Turnover < 1.5, Diversity < 0.8.

2. Your goal is to optimize the factor based on these performance metrics. Typically, improving RankIC and RankIR is the

primary focus; turnover and diversity are secondary.

3. The factor should produce a unitless value, independent of price scale or volume units.

4. Use **only** the provided data fields and operators.

5. You are **not** required to use all data fields. The factor expression should have NO MORE than 3 data fields in total.

6. Avoid excessive operator nesting or overly complex expressions to reduce overfitting risk.

7. The expression should be concise, readable, and interpretable.

8. Review the optimization history to avoid repeating previous attempts.

9. Inspect the factor expression to guard against overfitting.

# Response Format

You need to return a JSON object with the following fields:

- factor_name: a brief name of the factor

- factor_expression: the expression of the factor

- description: a 1-2 sentence description of the factor's intuition or financial meaning

- reason: a short explanation of why this optimization was made, such as which metric was improved or which weakness in the

previous version was addressed
```