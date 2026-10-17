## 2.1 采样与估计(Sampling and Estimation)

**※【定理】** 中心极限定理(Central Limit Theorem, CLT)：考虑 $n$ 个独立同分布(independent and identically distributed, iid)随机变量 $X_1, X_2, \ldots, X_n$，其中 $X_i \sim f$，分布 $f$ 具有均值 $\mu$ 和有界方差 $\sigma^2$。则 $\bar{X} = \frac{1}{n} \sum_{i=1}^n X_i$ 收敛于均值为 $\mu = \mathbb{E}[X_i]$、方差为 $\sigma^2 / n$ 的正态分布。

## 2.2 概率近似正确(Probably Approximately Correct, PAC)

**※【定义】** 概率近似正确(Probably Approximately Correct, PAC)界：$\mathbf{Pr}[|\bar{X} - \mathbf{E}[\bar{X}]| \geq \varepsilon] \leq \delta$。即，$\bar{X}$（通常是独立同分布随机变量的和）偏离其期望值（即 $\mu$）超过 $\varepsilon$ 的概率至多为 $\delta$。

## 2.3 集中不等式(Concentration of Measure)

### 2.3.1 马尔可夫不等式(Markov Inequality)

**※【定义】** 马尔可夫不等式(Markov Inequality)：令 $X$ 为一个随机变量，满足 $X \geq 0$，即它不能取负值。则对于任意参数 $\alpha > 0$，马尔可夫不等式表明：$\mathbf{Pr}[X > \alpha] \leq \frac{\mathbf{E}[X]}{\alpha}$。

注：这是一个 PAC 界，其中 $\varepsilon = \alpha - \mathbf{E}[X]$，$\delta = \mathbf{E}[X] / \alpha$。或者可将该界重述为：$\mathbf{Pr}[X - \mathbf{E}[X] > \varepsilon] \leq \delta = \mathbf{E}[X] / (\varepsilon + \mathbf{E}[X])$。

### 2.3.2 切比雪夫不等式(Chebyshev Inequality)

**※【定义】** 切比雪夫不等式(Chebyshev Inequality)：令 $X$ 为一个随机变量，已知其 $\mathbf{Var}[X]$ 和 $\mathbf{E}[X]$。对于任意参数 $\varepsilon > 0$，切比雪夫不等式表明：$\mathbf{Pr}[|X - \mathbf{E}[X]| \geq \varepsilon] \leq \frac{\mathbf{Var}[X]}{\varepsilon^2}$。这显然是一个 PAC 界，其中 $\delta = \mathbf{Var}[X] / \varepsilon^2$。

**※【定义】** 单边切比雪夫不等式(One-sided Chebyshev Inequality)：令 $X$ 为一个随机变量，已知其 $\mathbf{Var}[X]$ 和 $\mathbf{E}[X]$。对于任意参数 $\varepsilon > 0$，单边切比雪夫不等式表明：$\mathbf{Pr}[X - \mathbf{E}[X] \geq \varepsilon] \leq \frac{\mathbf{Var}[X]}{\mathbf{Var}[X] + \varepsilon^2}$。

证明：令 $\bar{X} = (X_1 + X_2 + \ldots + X_n) / n$，其中 $X_i$ 独立同分布，方差为 $\sigma^2$。则 $\mathbf{Var}[\bar{X}] = \sigma^2 / n$。因此，$\mathbf{Pr}[|\bar{X} - \mathbf{E}[X_i]| \geq \varepsilon] \leq \frac{\sigma^2}{n \varepsilon^2}$。

### 2.3.3 Chernoff-Hoeffding不等式(Chernoff-Hoeffding Inequality)

**※【定义】** Chernoff-Hoeffding不等式(Chernoff-Hoeffding Inequality)：考虑 $n$ 个独立同分布随机变量 $X_1, X_2, \ldots, X_n$，其中 $\bar{X} = \frac{1}{n} \sum_{i=1}^n X_i$。假设已知每个 $X_i \in [b, t]$，$\Delta = t - b$。对于任意参数 $\varepsilon > 0$，$\mathbf{Pr}[|\bar{X} - \mathbf{E}[\bar{X}]| > \varepsilon] \leq 2 \exp(\frac{-2\varepsilon^2 n}{\Delta^2})$。PAC 界：$\delta = 2 \exp(-2\varepsilon^2 n / \Delta^2)$。

证明：
令常数 $s > 0$，$P[\bar{X} - E[\bar{X}] \geq \varepsilon] = P(\exp(s(\bar{X} - E[\bar{X}]) \geq \exp(s\varepsilon)) \leq \frac{\mathbb{E}[\exp(s(\bar{X} - E[\bar{X}]))]}{\exp(s\varepsilon)}$。
$\mathbb{E}[\exp(s(\bar{X} - E[\bar{X}]))] = \prod_{i=1}^n \mathbb{E}[\exp(\frac{s}{n}(X_i - E[X_i]))]$。
定义 $Y_i = X_i - \mathbb{E}[X_i]$，$\mathbb{E}[Y_i] = 0$，$Y_i \in [a - \mathbb{E}[X_i], b - \mathbb{E}[X_i]]$，$\Delta = b - a$。
$\implies \mathbb{E}[\exp(\frac{s}{n} Y_i)] \leq \exp(\frac{1}{8} (\frac{s}{n})^2 (b-a)^2)$。
$\implies \mathbb{E}[\exp(s(\bar{X} - E[\bar{X}]))] \leq \exp(\frac{1}{8} \frac{s^2}{n} (b-a)^2)$。
$\implies P[\bar{X} - E[\bar{X}] \geq \varepsilon] \leq \exp(\frac{1}{8} \frac{s^2}{n} (b-a)^2 - s\varepsilon)$。
令 $s = \frac{4\varepsilon n}{(b-a)^2}$，下尾同理。
对于期望误差容限 $\varepsilon$ 和失败概率 $\delta$，可令 $n = (\Delta^2 / (2\varepsilon^2)) \ln(2/\delta)$。当采样概率大于 $n$ 时，失败概率小于 $\delta$。

**※【引理】** 霍夫丁引理(Hoeffding Lemma)：$\mathbb{E}[X] = 0$，$X \in [a,b]$。对于 $\lambda \in \mathbb{R}$，$\mathbb{E}[\exp(\lambda x)] \leq \exp(\frac{1}{8} \lambda^2 (b-a)^2)$。

证明：
$\exp(\lambda x)$ 是凸函数，有 $\exp(\lambda x) \leq \frac{b-x}{b-a} \exp(\lambda a) + \frac{x-a}{b-a} \exp(\lambda b), \forall x \in [a,b]$。
$\implies \mathbb{E}(\exp(\lambda x)) \leq \frac{b}{b-a} \exp(\lambda a) + \frac{-a}{b-a} \exp(\lambda b), \forall x \in [a,b]$。
定义 $h = \lambda(b-a)$，$P = -\frac{a}{b-a}$。
$\frac{b}{b-a} \exp(\lambda a) + \frac{-a}{b-a} \exp(\lambda b) = \exp(\lambda a + \ln(1-P+P\exp h))$。
令 $\mathscr{L}(h) = \lambda a + \ln(1-P+P\exp h) \leq \frac{h^2}{8}$。$\mathscr{L}(0) = \mathscr{L}'(0) = 0$，$\mathscr{L}'' \leq \frac{1}{4}$，由泰勒定理可得。

**例题：Chernoff-Hoeffding与骰子**
掷骰子 120 次，记录出现 3 的次数。令 $T$ 为出现 3 的总次数。每次掷骰子 $P(Z=3) = 1/6$，所以 $\mathbb{E}[T] = 20$。求超过 40 次出现 3 的概率？
$\mathbf{Pr}[T \geq 40] \leq \mathbf{Pr}[|\bar{T} - \mathbf{E}[T_i]| \geq 1/6] \leq 2 \exp(\frac{-2(1/6)^2 \cdot 120}{1^2}) = 2 \exp(-20/3) \leq 0.0026$。

### 2.3.4 联合界与示例(Union Bound and Examples)

**联合界(Union Bound)**
考虑 $s$ 个可能相关的随机事件 $Z_1, \ldots, Z_s$。$P(\cap_{i=1}^s Z_i) \leq 1 - \sum_{i=1}^s (1 - P(Z_i))$。

**分位数估计(Quantile Estimation)**
PDF：$f(x) \to$ CDF：$F(x) = \int_{-\infty}^x f(t)dt$。
对于随机变量 $X$ 的独立同分布样本 $P = \{p_1, p_2, \ldots, p_n\}$，估计 CDF $F_X$。对于任意 $t$，定义经验秩(empirical rank) $0 \leq \mathrm{Rank}_P(t) = \frac{|\{p_i \in P | p_i \leq t\}|}{n} \leq 1$。$\mathrm{Rank}_P(t) \to 0$ 当 $t \to -\infty$；$\mathrm{Rank}_P(t) \to 1$ 当 $t \to +\infty$。

**※【定义】** $\phi$-分位数($\phi$-quantile)：
样本集的一个值 $t$，满足 $\mathrm{Rank}_P(t) \leq \phi < \mathrm{Rank}_P(t+\eta), \forall \eta > 0$。

定义随机变量 $Y_i = 1$ 若 $p_i \leq t$ 否则为 $0$。观察到 $\mathbb{E}[Y_i] = F_X(t)$。此外，未来独立同分布样本 $P$ 上 $\mathrm{Rank}_P(t)$ 的随机变量正是 $\bar{Y} = \frac{1}{n} \sum_{i=1}^n Y_i$。因此，我们可以提供 PAC 界：$\mathbf{Pr}[|\bar{Y} - F_X(t)| \geq \varepsilon] \leq 2 \exp(\frac{-2\varepsilon^2 n}{1^2}) = \delta$。

若给定误差 $\varepsilon$（如 $0.05$）和失败概率 $\delta$（如 $0.01$），求解所需样本数：$n \geq \frac{1}{2\varepsilon^2} \ln \frac{2}{\delta}$。

**近似所有分位数(Approximating All Quantiles)**
上述分析仅对单个 $t$ 有效。若要对所有 $t$ 同时进行类似分析，即至少以 $1-\delta$ 的概率，对所有 $t$ 有 $|\mathrm{Rank}_P(t) - F_X(t)| \leq \varepsilon$，需要多少样本 $n$？
考虑 $1/\varepsilon_1$ 个不同分位数 $\{\phi_1, \phi_2, \ldots, \phi_{1/\varepsilon_1}\}$，其中 $\phi_j = j \cdot \varepsilon_1 + \varepsilon_1/2$。这将概率空间（即区间 $[0,1]$）划分为 $1/\varepsilon_1 + 1$ 部分，各部分边界间隙为 $\varepsilon_1$。
对于所有 $t \in T = \{t_1, \ldots, t_{1/\varepsilon_1}\}$，随机变量 $\bar{Y}(t_j)$ 满足 $\mathbf{Pr}[|\bar{Y}(t_j) - F_X(t_j)| \leq \varepsilon_2] \leq \delta$。由Chernoff-Hoeffding界，对每个 $t_j$，$1 - \delta' = 1 - 2 \exp(-2(\varepsilon_2)^2 n)$。对这些 $s = 1/\varepsilon_1$ 个事件应用联合界，得到所有事件成立的概率为 $1 - \sum_{j=1}^s 2 \exp(-2(\varepsilon_2)^2 n) = 1 - \frac{1}{\varepsilon_1} 2 \exp(-2(\varepsilon_2)^2 n)$。
令 $\varepsilon_1 = \varepsilon_2 = \varepsilon/2$，则对任意 $t$，有 $|\mathrm{Rank}_P(t) - F_X(t)| \leq \varepsilon$，概率至少为 $1 - \frac{4}{\varepsilon} \exp(-\frac{1}{2} \varepsilon^2 n)$。
令失败概率 $\frac{4}{\varepsilon} \exp(-\varepsilon^2 n / 2) = \delta$，解得 $n = \frac{2}{\varepsilon^2} \ln(\frac{4}{\varepsilon \delta})$。
基于 VC 维的论证可将样本数从 $\frac{1}{\varepsilon^2} \log \frac{1}{\varepsilon}$ 降至 $\frac{1}{\varepsilon^2}$。

## 2.4 重要性采样(Importance Sampling)

### 2.4.1 权重的样本均值(Sample Average of Weights)

考虑集合 $A = \{a_1, a_2, \ldots, a_n\}$，权重 $w(a_i)$。注意，这个权重不是分布权重，更像是某种映射，诸如员工-工资。
目标是估计期望权重 $\bar{w} = \mathbf{E}[w(a_i)] = \frac{1}{n} \sum_{a_i \in A} w(a_i)$。
采样 $k$ 个独立同分布项 $\{a'_1, a'_2, \ldots, a'_k\}$，获取权重 $w(a'_j)$，估计平均权重 $\hat{w} = \frac{1}{k} \sum_{j=1}^k w(a'_j)$。

若所有权重均在 $[0,1]$ 内，应用Chernoff-Hoeffding界：$\mathbf{Pr}[|\bar{w} - \hat{w}| \geq \varepsilon] \leq 2 \exp(-2\varepsilon^2 k)$。
因此，以至少 $1-\delta$ 的概率，使用 $k = \frac{1}{2\varepsilon^2} \ln \frac{2}{\delta}$ 个样本，误差不超过 $\varepsilon$。

然而，若无权重界限，该近似效果较差。
例如，若 $\bar{w} = 0.0001$，因大多数权重非常小，则 $\varepsilon = 0.01$ 的近似可能无意义。类似地，若大多数权重接近 1，$\bar{w} = 2$，但存在少数异常值权重 $\Delta = 1000$，则Chernoff-Hoeffding界仅给出 $\mathbf{Pr}[|\bar{w} - \hat{w}| \geq \varepsilon] \leq 2 \exp(\frac{-2\varepsilon^2 k}{\Delta^2}) = \delta$。这意味着需要 $k = \frac{\Delta^2}{2\varepsilon^2} \ln(2/\delta)$，是之前的 $\Delta^2 = 1,000,000$ 倍！注：权重未必是归一化的。

### 2.4.2 重要性采样(Importance Sampling)

集合 $A = \{a_1, a_2, \ldots, a_n\}$，权重 $w(a_i)$。估计 $\bar{w} = \frac{1}{n} \sum_{a_i \in A} w(a_i)$。
将 $W_i$ 视为每个采样权重的随机变量，则 $\bar{w} = \frac{1}{n} \sum_{i=1}^n W_i$ 也是随机变量。

已知每个 $a_i$ 的权重范围 $[0, \psi_i]$，即 $0 \leq w(a_i) \leq \psi_i$。上界 $\psi_i$ 作为 $a_i$ 的**重要性(importance)**。
令 $\Psi = \sum_{i=1}^n \psi_i$ 为所有重要性之和。

**※【算法】** 重要性采样(Importance Sampling)：
依概率按重要性 $\psi_i$ 比例从 $A$ 中独立采样 $k$ 个项 $\{a'_1, a'_2, \ldots, a'_k\}$。
估计 $w_I = \frac{1}{k} \sum_{j=1}^k (\frac{\Psi}{n \psi_j} \cdot w(a'_j))$，其中 $\Psi = \sum_{i=1}^n \psi_i$。
注：根据值的大小决定抽样概率的大小。

无偏性证明：定义随机变量 $Z_j = \frac{\Psi}{n \psi_j} w(a'_j)$。
由期望线性性和样本独立性，$\mathbb{E}[w_I] = \frac{1}{k} \sum_{j=1}^k \mathbb{E}[Z_j] = \mathbb{E}[Z_j]$。按 $\psi_i$ 比例采样意味着对象 $a_i$ 以概率 $\psi_i / \Psi$ 被选中。
对所有元素求和，得 $\mathbb{E}[w_I] = \mathbb{E}[Z_j] = \sum_{i=1}^n \mathbf{Pr}[a'_j = a_i] \cdot \frac{\Psi}{n \psi_i} w(a_i) = \sum_{i=1}^n \frac{\psi_i}{\Psi} \cdot \frac{\Psi}{n \psi_i} w(a_i) = \frac{1}{n} \sum_{i=1}^n w(a_i) = \bar{w}$。

### 2.4.3 改进的集中度(Improved Concentration)

为改进集中度，关键步骤是分析每个估计量 $\frac{\Psi}{n \psi_j} \cdot w(a'_j)$ 的范围。
由于 $w(a'_j) \in [0, \psi_j]$，因此 $\frac{\Psi}{n \psi_j} \cdot w(a'_j) \in \frac{\Psi}{n \psi_j} \cdot [0, \psi_j] = [0, \frac{\Psi}{n}]$。
应用Chernoff-Hoeffding界，可上界 $w_I$ 误差超过 $\varepsilon$ 的概率：$\mathbf{Pr}[|\bar{w} - w_I| \geq \varepsilon] \leq 2 \exp(\frac{-2\varepsilon^2 k}{(\Psi/n)^2}) = \delta$。
固定允许误差 $\varepsilon$ 和失败概率 $\delta$，可解出所需样本数：$k = \frac{(\Psi/n)^2}{2\varepsilon^2} \ln(2/\delta)$。
现在样本复杂度不再依赖于最大可能值 $\Delta$ 的平方，而是依赖于所有值的平均上界 $\Psi/n$ 的平方。
换言之，重要性采样将样本复杂度从取决于最大重要性 $\max \psi_i$ 降低到取决于平均重要性 $\Psi/n$。

*※【例题】* 公司薪资估计(Company Salary Estimation)
公司有 $n = 10,000$ 名员工，估计平均薪资。薪资极不平衡，CEO 薪资远高于普通员工。
假设 CEO 年薪至多 200 万，其他 9,999 名员工年薪至多 5 万。
仅使用均匀采样 $k = 100$ 名员工，应用Chernoff-Hoeffding界估计平均薪资 $\hat{w}$ 与真实平均薪资 $\bar{w}$ 的误差超过 $8,000$ 的概率：$\mathbf{Pr}[|\hat{w} - \bar{w}| \geq 8,000] \leq 2 \exp(\frac{-2(8,000)^2 \cdot 100}{(2 \text{ million})^2}) = 2 \exp(\frac{-2}{625}) \approx 1.99$。该界无效，概率大于 1。若将误差容限增至 50 万，仍只能以 0.42 的概率得到好估计。问题在于是否采样到 CEO。
使用重要性采样，CEO 重要性为 200 万，其他员工重要性为 5 万。平均重要性 $\Psi/n = 50,195$。新估计 $w_I$ 与 $\bar{w}$ 误差超过 $8,000$ 的概率至多 $\mathbf{Pr}[|w_I - \bar{w}| \geq 8,000] \leq 2 \exp(\frac{-2(8,000)^2 \cdot 100}{(51,950)^2}) \leq 0.017$。因此，98.3% 的情况下估计误差在 $8,000$ 以内。事实上，至少 38% 的情况下误差在 $4,000$ 以内。这基本有效，因为我们期望采样到 CEO 约两次，但加权略高；而当采样到其他员工时，其薪资影响增加约 4%。

### 2.4.4 无放回优先级采样(Sampling Without Replacement with Priority Sampling)

许多例子假设数据是独立同分布抽取的 $k$ 个元素。
然而，算法生成样本时，无放回采样可能更有利。
分析稍复杂，但存在针对无放回随机样本的Chernoff-Hoeffding界变体。
实践中，无放回采样通常能改善集中度和近似质量，加权采样时尤其如此。

按权重比例采样数据时，若存在权重足够大的元素，最好总是采样这些高权重元素。
低权重元素需以一定概率选择，该概率应与其权重成正比，然后按重要性采样重新加权。
优先级采样(Priority Sampling)优雅地结合了这些特性。

新权重函数 $w': A \to [0, \infty)$ 仅有 $k$ 个非零值项；只需在样本 $A'$ 中保留这些项。
这有许多良好性质，最重要的是 $\mathbb{E}[w'(a_i)] = w(a_i)$。
因此，对任意子集 $S \subset A$，可使用 $\sum_{a_i \in S \cap A'} w'(a_i)$ 估计权重和 $\sum_{a_i \in S} w(a_i)$，且具有正确期望值。
故对于类似重要性采样的 $w_P = \frac{1}{n} \sum_{i=1}^n w'(a_i)$，也有 $\mathbb{E}[w_P] = \bar{w}$。

此外，权重非常大的元素（权重高于 $\tau$ 的元素）总是被保留。
因为对所有 $i$，$\rho_i \geq w(a_i)$（由于 $1/u_i \geq 1$），所以若 $w(a_i) > \tau$，则 $\rho_i > \tau$，它总是被选中，其新权重为 $w'(a_i) = \max(w(a_i), \tau) = w(a_i)$，与原来相同。因此，对于固定 $\tau$，该项对估计的方差贡献为零。其余项按重要性采样方式赋权，因此整体估计方差较小（且接近最优）。

*※【例题】* 优先级采样(Priority Sampling)
展示 10 个项，权重从 $w(a_{10}) = 0.08$ 到 $w(a_1) = 1.80$。为清晰起见，按降序排列。然后每个项通过除以其不同的 $u_i \sim \mathrm{unif}(0,1]$ 得到优先级。要采样 $k = 4$ 个项，第 5 大优先级值 $\rho_4 = \tau = 1.10$（属于 $a_4$）用水平虚线标记。然后所有优先级高于 $\tau$ 的元素被赋予非零权重。最大权重元素 $a_1$ 保留其原始权重 $w(a_1) = w'(a_1) = 1.80$，因为它高于 $\tau$。其他保留元素权重低于 $\tau$，因此被赋予新权重 $w'(a_2) = w'(a_4) = w'(a_5) = \tau = 1.10$。其他元素隐式赋予新权重 0。
注意 $W' = \sum_{i=1}^{10} w'(a_i) = 5.10$ 非常接近 $W = \sum_{i=1}^{10} w(a_i) = 5.09$。
理解为何保留更多元素时新估计 $W'$ 不一定增加是有用的。本例中，若保留 $k = 5$ 个元素而非 $k = 4$，则 $\tau$ 变为 $\rho_7 = 0.69$，即第 6 大优先级。于是若干元素的新权重将从 1.10 降至 0.69。