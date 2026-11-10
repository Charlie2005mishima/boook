
## 1. 不完全信息博弈与海萨尼转换 (Harsanyi Transformation)

**※【例题】** 胆小鬼博弈 (Chicken Game)：
两个青少年相向飙车，每个人可以选择转向 ($C$) 或直行 ($D$)。
如果发生撞，会有惩罚 $k$。标准式收益矩阵为：

|     | $C$   | $D$             |
| --- | ----- | --------------- |
| $C$ | $0,0$ | $0,R$           |
| $D$ | $R,0$ | $0.5R-k,0.5R-k$ |

其中 $R>0, k\ge 0$。

**※【定义】** 不完全信息博弈 (Games of incomplete information)：
玩家对对手的某些特征（如偏好、成本）存在不确定性。

完美信息：玩家行动；完全信息：玩家特征。

**※【例题】** 不完全信息胆小鬼博弈：
假设惩罚 $k$ 是每个玩家的私人信息 (private information)。$k_i \in \{H, L\}$ 且 $H > L$。
只有玩家自己知道自己的 $k_i$，且假设双方为严父 ($H$) 或慈父 ($L$) 的概率各为 $1/2$。

**※【定理】** 海萨尼转换 (Harsanyi Transformation)：
任何不完全信息静态博弈，都可以通过引入一个虚拟玩家“自然” (Nature)，
转化为完全但不完美信息的动态博弈。

**※【例题】** 胆小鬼博弈的海萨尼转换：
1. **自然** (Nature) 先行动，等概率选择类型组合 $LL, LH, HL, HH$。
2. **玩家 1** 观察到自己的类型 $k_1$（但不知道 $k_2$），选择 $C$ 或 $D$。
3. **玩家 2** 观察到自己的类型 $k_2$（但不知道 $k_1$ 及玩家 1 的行动），选择 $c$ 或 $d$。
假设 $R=8, H=16, L=0$，可写出标准式矩阵。唯一的纯策略纳什均衡为 $(DC, dc)$。
这意味着：慈父类型 ($L$) 直行 ($D$)，严父类型 ($H$) 转向 ($C$)。

![[Pasted image 20261110213855.png|500]]

$\tilde{v}_{1}(A_{L},A_{H},a_{L},a_{H})=\frac{1}{4}v_{1}(A_{L},a_{L};LL)+\frac{1}{4}v_{1}(A_{L},a_{H};LH)+\frac{1}{4}v_{1}(A_{H},a_{L};HL)+\frac{1}{4}v_{1}(A_{H},a_{H};HH)$,
$\tilde{v}_{2}(A_{L},A_{H},a_{L},a_{H})=\frac{1}{4}v_{2}(A_{L},a_{L};LL)+\frac{1}{4}v_{2}(A_{L},a_{H};LH)+\frac{1}{4}v_{2}(A_{H},a_{L};HL)+\frac{1}{4}v_{2}(A_{H},a_{H};HH)$.

![[Pasted image 20261110214934.png|500]]


---

## 2. 贝叶斯博弈的基本概念

**※【定义】** 贝叶斯博弈 (Bayesian game) 或静态不完全信息博弈的标准式表示：
$$
(N, \{A_i\}_{i=1}^n, \{\Theta_i\}_{i=1}^n, \{v_i\}_{i=1}^n, \mathbb{P})
$$
- $N$：玩家集合。
- $A_i$：玩家 $i$ 的行动空间。
- $\Theta_i$：玩家 $i$ 的类型空间 (type space)。$\theta_i \in \Theta_i$ 为具体类型。
- $v_i: A \times \Theta \to \mathbb{R}$：支付函数，其中 $A = \times A_i$，$\Theta = \times \Theta_i$。
- $\mathbb{P}$：共同先验 (common prior)，即类型组合 $\Theta$ 上的概率分布。

**※【定义】** 贝叶斯法则 (Bayes' rule)：
给定玩家 $i$ 知道自己的类型 $\theta_i$，他对对手类型 $\theta_{-i}$ 的后验信念 (posterior belief) 为：
$$
\phi_i(\theta_{-i}|\theta_i) = \frac{\mathbb{P}(\theta_i, \theta_{-i})}{\mathbb{P}(\theta_i)}
$$
其中 $\mathbb{P}(\theta_i) = \sum_{\theta_{-i}' \in \Theta_{-i}} \mathbb{P}(\theta_i, \theta_{-i}')$ 为边缘分布。

**※【定义】** 纯策略 (Pure strategy)：函数 $s_i: \Theta_i \to A_i$，即一个完整的依类型而定的行动计划 (type-contingent plan)。
混合策略 (Mixed strategy)：函数 $\sigma_i: \Theta_i \to \Delta(A_i)$。

**※【定义】** 贝叶斯纳什均衡 (Bayesian Nash Equilibrium, BNE)：策略组合 $s^* = (s_1^*, \dots, s_n^*)$ 是 BNE，如果对每个玩家 $i$ 及其每个类型 $\theta_i \in \Theta_i$，都有：
$$
\sum_{\theta_{-i}} \phi_i(\theta_{-i}|\theta_i) v_i(s_i^*(\theta_i), s_{-i}^*(\theta_{-i}); \theta_i, \theta_{-i}) \ge \sum_{\theta_{-i}} \phi_i(\theta_{-i}|\theta_i) v_i(a_i, s_{-i}^*(\theta_{-i}); \theta_i, \theta_{-i}), \quad \forall a_i \in A_i
$$
即：没有任何类型愿意单方面偏离。

**※【定理】** 一个策略组合是 BNE，当且仅当它是其等价的扩展式博弈（海萨尼转换后）的纳什均衡。

---

## 3. 贝叶斯博弈例题

**※【例题】** 不完全信息古诺双寡头 (Cournot duopoly with private information)：
市场需求 $P(q) = \max\{a-q, 0\}$。企业 1 的边际成本 $c_1 \in \{c_h, c_\ell\}$ 是私人信息，概率分别为 $\mu_0$ 和 $1-\mu_0$。企业 2 的成本 $c_2$ 是共同知识。
企业 1 的策略是 $(q_h, q_\ell)$，企业 2 的策略是 $q_2$。
BNE 条件：
$$
q_h^* = \arg\max_{q_h} [P(q_h + q_2^*) - c_h]q_h = \frac{a - q_2^* - c_h}{2}
$$
$$
q_\ell^* = \arg\max_{q_\ell} [P(q_\ell + q_2^*) - c_\ell]q_\ell = \frac{a - q_2^* - c_\ell}{2}
$$
$$
q_2^* = \arg\max_{q_2} \mu_0[P(q_h^* + q_2) - c_2]q_2 + (1-\mu_0)[P(q_\ell^* + q_2) - c_2]q_2 = \frac{a - \mu_0 q_h^* - (1-\mu_0)q_\ell^* - c_2}{2}
$$
联立即可解出均衡产量。

**※【例题】** 学习小组 (Study group)：
两名学生选择努力 $e_i=1$ 或偷懒 $e_i=0$。努力成本 $0<c<1$。只要有人努力，作业就成功。失败收益为 0。成功的价值为 $\theta_i^2$，其中 $\theta_i \sim U[0,1]$ 是私人信息。
假设对称的截断策略 (cut-off strategy)：存在阈值 $\hat{\theta}$ 使得当 $\theta_i > \hat{\theta}$ 时努力，否则偷懒。
给定对手努力概率 $p^{\sigma_j} = \int_0^1 \sigma_j(\theta_j) d\theta_j$，玩家 $i$ 努力的收益为 $\theta_i^2 - c$；偷懒的收益为 $\theta_i^2 \times p^{\sigma_j}$。
最优反应：当 $\theta_i^2 > c / (1 - p^{\sigma_j})$ 时努力。
在对称均衡中，$p^{\sigma_1} = p^{\sigma_2} = p$。代入截断条件 $p = 1 - \sqrt{c/(1-p)}$，解得 $p = 1 - c^{1/3}$。
因此，本质唯一的 BNE 是：当 $\theta_i \ge c^{1/3}$ 时努力，否则偷懒。

---

## 4. 应用一：逆向选择 (Adverse Selection)

**※【例题】** 二手车市场 (Lemons Market)：
卖家（玩家 1）拥有一辆二手车，车况 $\theta \in \{P(\text{差}), F(\text{一般}), G(\text{好})\}$，等概率。这是卖家的私人信息。
卖家保留价值：$v_1(P)=10, v_1(F)=20, v_1(G)=30$。
买家（玩家 2）支付意愿：$v_2(P)=14, v_2(F)=24, v_2(G)=34$。
博弈时序：买家报价 $p$，卖家决定接受 ($a$) 或拒绝 ($r$)。
- **完全信息基准**：各状态分别以 $(10, 20, 30)$ 成交，交易有效率。
- **不完全信息**：
  - 证明：不存在 $G$ 接受均衡价格的均衡。若 $G$ 接受，则 $p \ge 30$。此时 $P, F, G$ 都会接受，买家期望收益为 $24 - p < 0$，买家会选择 $p=0$ 放弃。
  - 证明：不存在 $F$ 接受均衡价格的均衡。若 $F$ 接受，则 $p \ge 20$。已知 $G$ 不接受，则 $P, F$ 接受，买家期望收益为 $(14+24)/2 - p = 19 - 20 < 0$，买家仍会放弃。
  - 结论：唯一 BNE 是仅 $P$ 接受，成交价 $p=10$。只有最差质量的车进入市场，称为**逆向选择**。

---

## 5. 应用二：委员会投票 (Committee voting)

**※【例题】** 陪审团投票：
两名陪审员投票 $A$ (无罪) 或 $C$ (有罪)，若都投 $C$ 则定罪。被告有罪 ($G$) 的先验概率 $\mu_0 > 1/2$。正确判断收益为 1，错误为 0。
- 若无私人信息，均衡为 $(A,A)$ 和 $(C,C)$。
- 若每人收到关于被告是否有罪的私人信号 $s_i \in \{\theta_G, \theta_I\}$，精度 $p > 1/2$。
- **单人决策**：观察 $\theta_G$ 时，后验概率 $P(G|\theta_G) = \frac{p\mu_0}{p\mu_0 + (1-p)(1-\mu_0)} > \mu_0$，故投 $C$。观察 $\theta_I$ 时，若信号精度高 ($p > \mu_0$)，后验概率使他认为无罪，故投 $A$。
- **双人博弈**：按自己的信号投票是 BNE 吗？
  假设对方按信号投票 ($c$ if $\theta_G$, $a$ if $\theta_I$)。
  若玩家 1 观察到 $\theta_I$：
  若投 $A$，仅在被告确实无罪时得 1；若投 $C$，在 {有罪且对方观察 $\theta_G$} 或 {无罪且对方观察 $\theta_I$} 时得 1。
  计算后验概率可知，$P(G, s_2=\theta_G | s_1=\theta_I) > P(I, s_2=\theta_G | s_1=\theta_I)$ 且 $p > \mu_0$。这导致投 $C$ 的期望收益大于投 $A$。
  因此，按自己信号投票**不是** BNE（存在“信息级联”或从众倾向）。

---

## 6. 应用三：拍卖理论 (Auction Theory)

### 6.1 独立私人价值 (Independent Private Value, IPV) 环境
假设：竞标者 $i$ 的估值 $\theta_i$ 是私人信息，独立同分布于 $[\underline{\theta}, \bar{\theta}]$，分布函数为 $F$，密度为 $f$。

**※【例题】** 二价密封拍卖 (Second-price sealed-bid auction)：
赢家支付第二高出价。
支付函数：若 $b_i > \max_{j\neq i} b_j$，收益为 $\theta_i - \max b_j$；若平局平分；若输则 0。
**※【定理】** 在二价拍卖中，真实报价 $b_i = \theta_i$ 是弱占优策略（从而也是唯一的 BNE）。
- 若 $b_i < \max b_j < \theta_i$，报价 $\theta_i$ 能赢且获正收益，报价 $b_i$ 则输。
- 若 $\max b_j > \theta_i$，报价 $b_i > \max b_j$ 会赢但亏本，报价 $\theta_i$ 则安全不输。
- 若 $\max b_j = \theta_i$，任何报价收益均为 0。
该结果**不依赖于**对手类型的分布，且结果有效（估值最高者赢）。

**※【例题】** 一价密封拍卖 (First-price sealed-bid auction)：
赢家支付自己的出价。假设对称严格递增策略 $s(\theta)$。
期望收益：$F^{n-1}(\theta) \times (\theta - s(\theta))$。
一阶条件 (FOC)：$F^{n-1}(\theta)s'(\theta) + (n-1)f(\theta)F^{n-2}(\theta)s(\theta) = (n-1)\theta f(\theta)F^{n-2}(\theta)$。
积分得对称 BNE：$s(\theta) = \theta - \frac{\int_{\underline{\theta}}^\theta F^{n-1}(x)dx}{F^{n-1}(\theta)}$。
若 $F$ 为 $[0,1]$ 上的均匀分布，则 $s(\theta) = \frac{n-1}{n}\theta$。

**※【定理】** 收益等价定理 (Revenue Equivalence Theorem)：
在 IPV 环境下，满足以下条件的任何拍卖机制，都会给卖家带来相同的期望收益：
1. 竞标者风险中性；
2. 估值最高者赢得拍卖；
3. 最低类型估值的竞标者期望收益为 0。
*推论*：一价拍卖和二价拍卖的卖家期望收益相等。例如均匀分布下均为 $\frac{n-1}{n+1}$。

### 6.2 共同价值 (Common Value) 环境

**※【例题】** 赢家的诅咒 (Winner's Curse)：
两名竞标者参与一价拍卖。拍品对两人的真实价值相同，为 $v = t_1 + t_2$，其中 $t_i \sim U[0,1]$ 是双方各自收到的私人信号。
设对称递增策略为 $s(t_i)$。
期望收益：$\int_0^{s^{-1}(b_i)} (t_i + t_j - b_i) dt_j$。
解 FOC 并积分得对称 BNE：$s(t) = t$。
**现象**：若竞标者估值为 $t_1$，其无条件期望价值为 $t_1 + E[t_2] = t_1 + 1/2$。但如果他赢了，说明对手信号 $t_2 < t_1$，此时他应该修正估值为 $t_1 + E[t_2|t_2 < t_1] = t_1 + t_1/2$。由于 $t_1 + t_1/2 < t_1 + 1/2$，**赢下拍卖是一件“坏消息”**，意味着自己高估了拍品价值。