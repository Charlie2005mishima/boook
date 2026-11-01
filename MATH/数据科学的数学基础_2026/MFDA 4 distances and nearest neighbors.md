## 4.1 度量(Metrics)

**※【定义】** 距离(distance)：
一个距离 $d : \mathcal{X} \times \mathcal{X} \to \mathbb{R}^+$ 是一个二元算子，它接收两个参数 $a, b \in \mathcal{X}$，并映射到 $\mathbb{R}^+ = [0, \infty)$。

**※【定义】** 度量(metric)：
若距离 $d$ 满足以下四个条件，则称其为度量：
- 非负性(non-negativity)：$d(a, b) \geq 0$
- 同一性(identity)：$d(a, b) = 0$ 当且仅当 $a = b$ 
- 对称性(symmetry)：$d(a, b) = d(b, a)$ 
- 三角不等式(triangle inequality)：$d(a, b) \leq d(a, c) + d(c, b)$ 

**※【定义】** 伪度量(pseudometric)：
满足 (M1)、(M3) 和 (M4)，但不一定满足 (M2) 的距离。

**※【定义】** 拟度量(quasimetric)：
满足 (M1)、(M2) 和 (M4)，但不一定满足 (M3) 的距离。

## 4.2 $L_p$ 距离及其相关($L_p$ Distances and their Relatives)

### 4.2.1 $L_p$ 距离($L_p$ Distances)

**※【定义】** $L_p$ 距离($L_p$ distance)：
对于向量 $a = (a_1, a_2, \ldots, a_d)$ 和 $b = (b_1, b_2, \ldots, b_d)$，其 $L_p$ 距离定义为 $d_p(a, b) = \|a - b\|_p = (\sum_{i=1}^d (|a_i - b_i|)^p)^{1/p}$。

**※【定义】** $L_2$ 距离($L_2$ distance, Euclidean distance)：
$d_2(a, b) = \|a - b\|_2 = \sqrt{\sum_{i=1}^d (a_i - b_i)^2}$。

**※【定义】** $L_1$ 距离($L_1$ distance, Manhattan distance)：
$d_1(a, b) = \|a - b\|_1 = \sum_{i=1}^d |a_i - b_i|$。

**※【定义】** $L_0$ 距离($L_0$ distance)：
$d_0(a, b) = \|a - b\|_0 = d - \sum_{i=1}^d \mathbb{1}(a_i = b_i)$，其中 $\mathbb{1}(a_i = b_i) = 1$ 若 $a_i = b_i$，否则为 $0$。
注意 $d_0$ 不是凸函数。当坐标取值为 $0$ 或 $1$ 时，这被称为汉明距离(Hamming distance)。

**※【定义】** $L_\infty$ 距离($L_\infty$ distance)：
$d_\infty(a, b) = \|a - b\|_\infty = \max_{i=1 \ldots d} |a_i - b_i|$。
它是任一坐标上的最大偏差。几何上，在 $\mathbb{R}^2$ 中它是 $L_1$ 距离的旋转。严格来说它不是 $L_p$ 距离，而是 $p \to \infty$ 时的极限。

**※【定义】** 单位球(unit ball)：
在 $\mathbb{R}^2$ 中，单位球是所有满足 $d(\mathbf{0}, a) = 1$ 的点 $a$ 的集合。

**※【命题】** 几何性质
- $L_1$ 单位球永远不会大于 $L_2$ 单位球，$L_2$ 永远不会大于 $L_\infty$。
- 它们仅在坐标轴上相等，这是唯一相等的地方。
- 对于 $p < p'$，$L_p$ 球永远不会大于 $L_{p'}$ 球。
- $L_2$ 球是唯一对坐标轴选择不变的球（旋转后保持不变）。
- 对于 $p < 1$，$L_p$ 球不是凸的，在坐标轴之间“向内弯曲”，这使得算法难以处理，并且违反了三角不等式，因此不是度量。
- 所有 $L_p$ 距离对于 $p \in [1, \infty)$ 都是度量。

**※【定理】** $L_2$ 三角不等式：
$d_2(a, b) \leq d_2(a, \pi_{a,b}(c)) + d_2(\pi_{a,b}(c), b) \leq d_2(a, c) + d_2(c, b)$。
证明：利用勾股定理，$d_2(a, c)^2 = d_2(a, \pi_{a,b}(c))^2 + d_2(\pi_{a,b}(c), c)^2$，且 $d_2(a, b) \leq d_2(a, \pi_{a,b}(c)) + d_2(\pi_{a,b}(c), b)$。

**※【定理】** $L_p$ 三角不等式：
$\parallel a+b\parallel_{p}\leq \parallel a\parallel_{p}+\parallel b\parallel_{p}$。
证明：等价于证明$\parallel \frac{a+b}{\parallel a\parallel+\parallel b\parallel}\parallel\leq{1}$
记 $\bar{a}=\frac{a}{\parallel a\parallel},\bar{b}=\frac{b}{\parallel b\parallel}$，可将原不等式改写为$\parallel \frac{\parallel a\parallel}{\parallel a\parallel+\parallel b\parallel} \frac{a}{\parallel a\parallel} + \frac{\parallel b\parallel}{\parallel a\parallel+\parallel b\parallel} \frac{b}{\parallel b\parallel}\parallel^p\leq{1}$
因为 $f(x)=|x|^p$ 是凸函数，有$|\lambda \bar{a}_{i}+(1-\lambda)\bar{b}_{i}|^p\leq\lambda|\bar{a}_{i}|^p+(1-\lambda)|\bar{b}_{i}|^p$ 

**※【警告】** 当各坐标的单位和含义不同时，不应使用 $L_p$ 距离。例如，身高（英寸）、体重（磅）、收入（美元）的坐标变化范围差异巨大，距离会被数值大的坐标主导。改变单位会改变距离的主导因素。这可以通过手动缩放、标准化（如缩放到 $[0, 1]$）或归一化（均值为 0，方差为 1）来缓解。但这并非解决方案，而是建模选择或启发式方法。

### 4.2.2 马氏距离(Mahalanobis Distance)

**※【定义】** 马氏距离(Mahalanobis distance)：
对于向量 $a, b \in \mathbb{R}^d$ 和 $d \times d$ 矩阵 $M$，定义为 $d_M(a, b) = \sqrt{(a - b)^T M (a - b)}$。
当 $M = I$ 时，$d_M = d_2$。
当 $M$ 为对角矩阵时，可解释为基于 $M$ 的欧几里得空间拉伸（缩小某些坐标，扩展其他坐标）。
当 $M$ 不是对角矩阵时，拉伸仍然成立，但不对齐坐标轴，而是通过 $M$ 的特征向量和特征值定义。只要所有特征值为正实数（即 $M$ 为正定矩阵），$d_M$ 就是度量。

### 4.2.3 余弦与角度距离(Cosine and Angular Distance)

**※【定义】** 余弦距离(cosine distance)：
衡量两个向量 $a, b \in \mathbb{R}^d$ 之间“角度”的余弦值的补，$d_{\cos}(a, b) = 1 - \frac{\langle a, b \rangle}{\|a\| \|b\|} = 1 - \frac{\sum_{i=1}^d a_i b_i}{\|a\| \|b\|}$。
若 $\theta_{a,b}$ 是向量 $a$ 和 $b$ 之间的夹角，则 $d_{\cos}(a, b) = 1 - \cos(\theta_{a,b})$。

注意：$d_{\cos}(a, b) \in [0, 2]$，且不依赖于向量的大小（被归一化了），只关心方向。
这对于比较不同大小的数据集分布很有用。但它是伪度量，因为对于 $a' = 2a$，$d_{\cos}(a, a') = 0$ 但 $a \neq a'$。

若限制在归一化向量 $a, b \in \mathbb{S}^{d-1}$ 上，其中 $\mathbb{S}^{d-1} = \{x \in \mathbb{R}^d \mid \|x\| = 1\}$，则 $d_{\cos}(a, b) = 1 - \langle a, b \rangle$。但此时仍不是度量，因为它可能违反三角不等式。

*※【例题】* 违反三角不等式
$\theta_{\{ a,b,c \}}=\left\{ 0, \frac{\pi}{3}, \frac{2\pi}{3} \right\}$, $d_{\cos}(a,c)>d_{\cos}(a,b)+d_{\cos}(b,c)$. 

**※【定义】** 角度距离(angular distance)：
余弦距离的等距变换，定义为 $d_{\text{ang}}(a, b) = \cos^{-1}(\langle a, b \rangle) = \arccos(\langle a, b \rangle)$。它仅测量角度，$d_{\text{ang}}(a, b) \in [0, \pi]$，且在 $\mathbb{S}^{d-1}$ 上是度量。

三角不等式证明：
$\vec{a}=(\sin\theta_{1}\cos \phi_{1},\sin\theta_{1}\sin \phi_{1},\cos\theta_{1})$,
$\vec{b}=(0,0,1)$,
$\vec{c}=(\sin\theta_{2}\cos \phi_{2},\sin\theta_{2}\sin \phi_{2},\cos\theta_{2})$,
$d_{ang}(\vec{a},\vec{c})=\sin\theta_{1}\sin\theta_{2}(\cos \phi_{1}\cos \phi_{2}+\sin \phi_{1}\sin \phi_{2})+\cos \theta_{1}\cos\theta_{2}$.

**几何理解**：将两个点视为位于单位球 $\mathbb{S}^1 \subset \mathbb{R}^2$ 上。$d_{\text{ang}}(a, b)$ 是角度 $\theta_{a,b}$ 的弧度，等价于在球面上行走时 $a$ 和 $b$ 之间的弧长。由于它测量球面上的最短距离，因此满足三角不等式。

### 4.2.4 KL 散度(KL Divergence)

**※【定义】** Kullback-Leibler 散度(Kullback-Leibler Divergence)：
也称为 KL 散度，它不是度量。它接收离散分布 $X_a$ 和 $X_b$，等价于向量 $a, b \in \Delta_{\circ}^{d-1}$。其中 $\Delta_{\circ}^{d-1} = \{x = (x_1, \ldots, x_d) \in \mathbb{R}^d \mid \|x\|_1 = 1 \text{ 且 } x_i > 0 \text{ 对所有 } i\}$。定义为 $d_{KL}(X_a, X_b) = d_{KL}(a, b) = \sum_{i=1}^d a_i \ln(a_i / b_i)$。

凸函数性质：$\sum_{i=1}^da_{i}\ln(a_{i}/b_{i})\geq \sum_{i=1}^n\ln\left( a_{i}\cdot \frac{b_{i}}{a_{i}} \right)$.

$f,g$ 是概率定义在 $\mathbb{R}^d$。那么$d_{KL}(f,g)=\int_{\mathbb{R}^d}f(x)\ln \frac{f(x)}{g(x)}dx$.

它可以解释为使用分布 $b$ 代替 $a$ 时的“信息增益”。注意 $d_{KL}$ 不是度量，它违反 (M3) 因为不对称，也违反三角不等式 (M4)。

## 4.3 集合与字符串的距离(Distances for Sets and Strings)

**集合快速回顾**：集合 $A$ 是元素 $\{a_1, a_2, \ldots, a_k\}$ 的集合。顺序无关，多重性被忽略（如果考虑多重性则称为多重集(multi-set)）。$|A|$ 表示基数，$A \cap B$ 表示交集，$A \cup B$ 表示并集，$A \setminus B$ 表示差集，$A \triangle B$ 表示对称差，$\bar{A} = \Omega \setminus A$ 表示补集。

**※【例题】** 集合运算：$A = \{1, 3, 5\}$ 和 $B = \{2, 3, 4\}$。$|A| = 3$。$A \cap B = \{3\}$。$A \cup B = \{1, 2, 3, 4, 5\}$。$A \setminus B = \{1, 5\}$，$B \setminus A = \{2, 4\}$。$\bar{A} = \{2, 4, 6\}$。对称差 $A \triangle B = \{1, 2, 4, 5\}$。

### 4.3.1 杰卡德距离(Jaccard Distance)

**※【定义】** 杰卡德距离(Jaccard distance)：
两个集合 $A$ 和 $B$ 之间的杰卡德距离定义为 $d_J(A, B) = 1 - \frac{|A \cap B|}{|A \cup B|}$。
由于并集总是大于交集，分数项在 $0$ 到 $1$ 之间，距离值在 $[0, 1]$ 内。
当集合没有共同元素时为 $1$，当集合完全相同时为 $0$。

**※【例题】** 杰卡德距离：
$A = \{0, 1, 2, 5, 6\}$ 和 $B = \{0, 2, 3, 5, 7, 9\}$。
$d_J(A, B) = 1 - \frac{| \{0, 2, 5\} |}{| \{0, 1, 2, 3, 5, 6, 7, 9\} |} = 1 - \frac{3}{8} = 0.625$。
若向 $A$ 添加一个已在 $B$ 中的元素 $7$，分子增加，分母不变，距离变小。
若向 $A$ 添加一个不在 $A$ 或 $B$ 中的元素 $4$，分子不变，分母增加，距离变大。

杰卡德距离是度量，且对集合大小不变，仅取决于两个集合中相同元素的比例。它不需要知道元素来自的更大全集 $\Omega$。

**※【定理】** 杰卡德距离的度量性质证明：
前三个性质直接成立。
(M4) 三角不等式：$d_J(A, C) + d_J(C, B) \geq d_J(A, B)$。
记 $d_J(A, B) = 1 - \frac{|A \cap B|}{|A \cup B|} = \frac{|A \triangle B|}{|A \cup B|}$。
首先排除 $C$ 中不属于 $A\cup B$ 的元素，移除它们只会减少左边而不影响右边。
假设 $C \subseteq A$ 且 $C \subseteq B$。则 $d_J(A, C) + d_J(C, B) = \frac{|A \setminus C|}{|A|} + \frac{|B \setminus C|}{|B|} \geq \frac{|A \setminus C| + |B \setminus C|}{|A \cup B|} \geq \frac{|A \triangle B|}{|A \cup B|} = d_J(A, B)$。
假设 $\exists\,x\in C,x\not\in A\cap B$，有$|A\triangle C|+|C\triangle B|\geq|A\triangle B|$.

### \*4.3.2 编辑距离(Edit Distance)

**※【定义】** 编辑距离(Edit distance)：设 $\Sigma$ 为字母表，字符串 $a, b \in \Sigma^d$。编辑距离 $d_{ed}(a, b)$ 是将 $a$ 变为 $b$ 所需的操作次数，操作可以是删除或插入字母。字符串不需要长度相同。

**※【例题】** 编辑距离：$a = \text{mines}$ 和 $b = \text{smiles}$。$d_{ed}(a, b) = 3$。操作：1. mines -> minles (插入 l)；2. minles -> miles (删除 n)；3. miles -> smiles (插入 s)。若允许替换操作且成本相同，则编辑距离为 2。

编辑距离是度量。它适用于短字符串，但在大文本上计算慢（最坏情况需要二次时间动态规划），且删除一个句子可能导致大的编辑距离而不改变含义。通常 $d_{ed}(a, b) > 3$ 被认为很大。

## 4.4 用距离对文本建模(Modeling Text with Distances)

### 4.4.1 词袋向量(Bag-of-Words Vectors)

**※【定义】** 词袋模型(Bag-of-Words)：将每个文档转换为一个多重集，仅保留每个单词的计数。将其视为一个（非常稀疏的）向量 $v \in \mathbb{R}^D$，其中 $D$ 是所有可能单词的数量。

**※【例题】** 词袋向量：使用 4 个短文档 $D_1$: I am Pam. $D_2$: Pam I am. $D_3$: I do not like jelly and ham. $D_4$: I do not, do not, like them, Pam I am.。在 $D=11$ 维空间中，单词列表为 (am, and, do, ham, I, jelly, like, not, Pam, them, zebra)。向量为：
$v_1 = (1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0)$
$v_2 = (1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0)$
$v_3 = (0, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0)$
$v_4 = (1, 0, 2, 0, 2, 0, 1, 2, 1, 1, 0)$
使用 $d_2$ 和 $d_{\cos}$ 计算距离，$d_2(v_1, v_2) = 0$，$d_{\cos}(v_1, v_2) = 0$。

另一种表示是将其视为单词上的概率分布，即用 $L_1$ 范数归一化 $v$，得到 $\bar{v} = v / \|v\|_1$，使其位于 $\Delta^{D-1}$ 上。这倾向于使用 KL 散度。由于 KL 散度不允许零值，需要添加正则化项 $\alpha$（例如 $\alpha = 0.01$）。

### 4.4.2 k-Grams

**※【定义】** k-Grams：将文档转换为集合（而非多重集），但不仅使用单词。单词 k-grams 是文档中所有连续 k 个单词的集合。

**※【例题】** 单词 2-Grams：使用上述运行示例。
$G_1 = \{[I\ am], [am\ Pam]\}$
$G_2 = \{[Pam\ I], [I\ am]\}$
$G_3 = \{[I\ do], [do\ not], [not\ like], [like\ jelly], [jelly\ and], [and\ ham]\}$
$G_4 = \{[I\ do], [do\ not], [not\ do], [not\ like], [like\ them], [them\ Pam], [Pam\ I], [I\ am]\}$
可以计算这些集合之间的杰卡德距离。

**※【定义】** 字符 k-grams(Character k-grams)：使用 k 个连续字符而非 k 个连续单词。

**建模选择**：标点符号是否包含？空白字符是否作为字符？大小写是否区分？$k$ 取多大？通常 $k=3$ 或 $k=4$。对于长文章，使用单词和较大的 $k$；对于短文章（如推文），使用较小的 $k$ 和字符 k-grams。

**※【定义】** 连续词袋模型(Continuous Bag-of-Words, CBOW)：为每个单词创建抽象表示。在文本中某个单词的 $r$ 个单词的序列半径内，为该单词的每个实例创建一个 $D$ 维向量（在大小为 $D$ 的词汇表上保持计数）。将这些向量在所有实例上平均。这被称为 CBOW 模型。通常这些表示会被非线性压缩到较低维度（如 300 或 1,000 维）。

## 4.5 相似度(Similarities)

**※【定义】** 相似度(similarity)：与距离对偶的概念。当参数接近时，值应该很大。通常归一化使得 $s(A, A) = 1$，范围为 $[0, 1]$，表示完全不同的对值为 $0$。

从相似度转换为距离的两种标准方法：
- 基于集合的相似度：$d(A, B) = 1 - s(A, B)$
- 基于向量和范数的相似度：$d(A, B) = \sqrt{s(A, A) + s(B, B) - 2s(A, B)}$

### 4.5.1 集合相似度(Set Similarities)

**※【定义】** 杰卡德相似度(Jaccard similarity)：$s_J(A, B) = \frac{|A \cap B|}{|A \cup B|}$。杰卡德距离 $d_J(A, B) = 1 - s_J(A, B)$。

**※【定义】** 广义集合相似度：$s_{x, y, z, z'}(A, B) = \frac{x |A \cap B| + y |\overline{A \cup B}| + z |A \triangle B|}{x |A \cap B| + y |\overline{A \cup B}| + z' |A \triangle B|}$。例如 $s_J = s_{1, 0, 0, 1}$。当 $y = 0$ 时，不需要知道全集 $\Omega$。

**※【定义】** 其他集合相似度：
- 汉明相似度(Hamming similarity)：$s_{\text{Ham}}(A, B) = s_{1, 1, 0, 1}(A, B) = \frac{|A \cap B| + |\overline{A \cup B}|}{|A \cap B| + |\overline{A \cup B}| + |A \triangle B|}$
- 安德伯格相似度(Andberg similarity)：$s_{\text{Andb}}(A, B) = s_{1, 0, 0, 2}(A, B) = \frac{|A \cap B|}{|A \cap B| + 2 |A \triangle B|}$
- 罗杰斯-谷本相似度(Rogers-Tanimoto similarity)：$s_{\text{RT}}(A, B) = s_{1, 1, 0, 2}(A, B) = \frac{|A \cap B| + |\overline{A \cup B}|}{|A \cap B| + |\overline{A \cup B}| + 2 |A \triangle B|}$
- 索伦森-骰子相似度(Sørensen-Dice similarity)：$s_{\text{Dice}}(A, B) = s_{2, 0, 0, 1}(A, B) = \frac{2 |A \cap B|}{2 |A \cap B| + |A \triangle B|}$

对于 $s_J, s_{\text{Ham}}, s_{\text{Andb}}, s_{\text{RT}}$，$d(A, B) = 1 - s(A, B)$ 是度量。特别是 $d_{\text{Ham}}(A, B) = |\Omega| (1 - s_{\text{Ham}}(A, B))$ 被称为汉明距离，通常用于比特向量。

Hamming Distance: 三角不等式在发现 $|A \cap B| + |\overline{A \cup B}| + |A \triangle B|=|\Omega|$ 后容易证明
Andberg Distance: $d_{Andb}(A,B)=\frac{2d_{Ham}}{1+d_{Ham}}$，令$g(t)= \frac{2t}{1+t},t\geq0$.
有 $g'(t)>0$; $g(x)+g(y)-g(x+y)\geq0$.

**※【例题】** 集合相似度：$A = \{0, 1, 2, 5, 6\}$ 和 $B = \{0, 2, 3, 5, 7, 9\}$ 在 $\Omega = \{0, 1, 2, 3, 4, 5, 6, 7, 8, 9\}$ 中。
$s_J(A, B) = \frac{3}{8} = 0.375$
$s_{\text{Ham}}(A, B) = 1 - \frac{5}{10} = 0.5$
$s_{\text{Andb}}(A, B) = \frac{3}{13} \approx 0.231$
$s_{\text{RT}}(A, B) = \frac{10 - 5}{10 + 5} \approx 0.333$
$s_{\text{Dice}}(A, B) = \frac{2(3)}{5 + 6} \approx 0.545$

### 4.5.2 范数相似度(Normed Similarities)

**※【定义】** 点积相似度(dot product similarity)：$s_{\text{dot}}(p, q) = \langle p, q \rangle$。转换为欧几里得距离：$d_2(p, q) = \sqrt{\|p\|_2^2 + \|q\|_2^2 - 2 \langle p, q \rangle}$。相似度可能任意大，$s_{\text{dot}}(p, p) = \|p\|^2$。

**※【定义】** 余弦相似度(cosine similarity)：$s_{\cos}(p, q) = \langle \frac{p}{\|p\|}, \frac{q}{\|q\|} \rangle = \frac{\langle p, q \rangle}{\|p\| \|q\|}$。转换为距离：$\sqrt{s_{\cos}(p, p) + s_{\cos}(q, q) - 2 s_{\cos}(p, q)} = d_2(\frac{p}{\|p\|}, \frac{q}{\|q\|})$。更常用的是余弦距离 $d_{\cos}(p, q) = 1 - s_{\cos}(p, q)$。

**※【定义】** 核相似度(kernel similarity)：最常见的是高斯核(Gaussian kernel) $K(p, q) = \exp(-\|p - q\|^2 / \sigma^2)$。核距离定义为 $d_K(p, q) = \sqrt{K(p, p) + K(q, q) - 2 K(p, q)}$。对于高斯核和特征核(characteristic kernels)，该距离是度量。正定核(positive definite kernel) $K$ 是指对于任何点集 $P = \{p_1, \ldots, p_n\}$，$n \times n$ 的 Gram 矩阵 $G_{i,j} = K(p_i, p_j)$ 是正定的。

### 4.5.3 集合间的范数相似度(Normed Similarities between Sets)

**※【定义】** 集合间的核相似度：设 $P = \{p_1, \ldots, p_n\} \subset \mathbb{R}^d$ 和 $Q = \{q_1, \ldots, q_m\} \subset \mathbb{R}^d$ 是两个向量集合。核 $K$ 定义集合间的相似度 $\kappa(P, Q) = \sum_{p \in P} \sum_{q \in Q} K(p, q)$。核距离为 $d_K(P, Q) = \sqrt{\kappa(P, P) + \kappa(Q, Q) - 2 \kappa(P, Q)}$。只要核是特征核，这就是度量。

**※【定义】** 核密度估计(Kernel Density Estimate, KDE)：将有限点集 $P \subset \mathbb{R}^d$ 和平滑算子核 $K$ 转换为连续函数 $\text{kde}_P : \mathbb{R}^d \to \mathbb{R}$。最常见的是归一化高斯核 $K(p, x) = \frac{1}{\sigma^d \sqrt{(2 \pi)^d}} \exp(-\frac{\|x - p\|^2}{2 \sigma^2})$。核密度估计定义为 $\text{KDE}_P(x) = \frac{1}{|P|} \sum_{p \in P} K(p, x) = \frac{1}{|P|} \kappa(P, \{x\})$。这为每个点赋予质量 $\frac{1}{|P|}$，用高斯核扩散，然后求和。

## 4.6 局部敏感哈希(Locality Sensitive Hashing, LSH)

**※【定义】** 局部敏感哈希家族(Locality Sensitive Hash Family)：对于相似度函数 $s$，局部敏感哈希家族 $\mathcal{H}$ 是一组哈希函数，使得对于任意两个对象 $p, q \in \mathcal{B}$，有 $\mathbf{Pr}_{h \in \mathcal{H}}[h(p) = h(q)] \approx s(p, q)$。

**※【例题】** 三角形相似度的简单网格 LSH：考虑一维数据集 $X \subset \mathbb{R}^1$，三角形相似度 $s_\triangle(p, q) = \max\{0, 1 - |p - q|\}$。定义哈希家族 $\mathcal{H}_\triangle = \{h_\eta \mid \eta \in [0, 1)\}$，其中 $h_\eta(x) = \lceil x + \eta \rceil$。每个 $h_\eta$ 将输入映射到整数（哈希桶索引），网格定义长度为 1 的连续区间，映射到同一个哈希桶。参数 $\eta$ 随机移动网格边界。可以验证对于任意 $p, q \in \mathbb{R}$，$\mathbf{Pr}_{h_\eta \in \mathcal{H}_\triangle}[h_\eta(p) = h_\eta(q)] = s_\triangle(p, q)$。

**※【定义】** 分离哈希函数(separating hash function)：哈希函数 $h : \mathcal{B} \to \mathcal{U}$ 将对象 $b \in \mathcal{B}$ 映射到固定全集 $\mathcal{U}$（通常为整数集合 $\{0, 1, \ldots, u-1\}$）。完美分离哈希函数保证对于任意两个 $b, b' \in \mathcal{B}$，$\mathbf{Pr}_{h \in \mathcal{H}}[h(b) = h(b')] = 1/u$。

### 4.6.1 局部敏感哈希的性质(Properties of Locality Sensitive Hashing)

**※【定义】** $(\gamma, \phi, \alpha, \beta)$-敏感性：哈希家族 $\mathcal{H}$ 关于距离 $d$ 是 $(\gamma, \phi, \alpha, \beta)$-敏感的，当满足：
- $\mathbf{Pr}_{h \in \mathcal{H}}[h(p) = h(q)] > \alpha$ 若 $d(p, q) < \gamma$
- $\mathbf{Pr}_{h \in \mathcal{H}}[h(p) = h(q)] < \beta$ 若 $d(p, q) > \phi$

需要 $\alpha > 0$ 且 $\beta < 1$，对于 $\gamma \leq \phi$。理想情况下 $\alpha$ 大，$\beta$ 小，$\phi - \gamma$ 小。可以通过多个随机哈希函数重复来放大效果。

**※【例题】** 三角形相似度的敏感性：对于三角形相似度 $s_\triangle$ 和其网格哈希家族 $\mathcal{H}_\triangle$，对于相似度阈值 $\tau = d_\triangle(p, q) = 1 - s_\triangle(p, q)$，可以设置 $\tau = \gamma = \phi$，则 $\alpha = 1 - \tau$，$\beta = \tau$。

### 4.6.2 LSH 的典型任务(Prototypical Tasks for LSH)

给定大型数据集 $P$（可能有数亿个对象），LSH 可用于：
- 对于阈值 $\tau$，找出所有满足 $d(p, p') \leq \tau$ 的对 $p, p' \in P$。
- 对于阈值 $\tau$ 和查询 $q$，返回所有满足 $d(p, q) \leq \tau$ 的 $p \in P$。
- 对于查询 $q$，返回点 $\tilde{p} \in P$，使其近似为 $\text{argmin}_{p \in P} d(p, q)$。

前两个任务在去重和抄袭检测中很重要。第三个任务用于分类，通过最近邻方法。

### 4.6.3 分带放大 LSH(Banding to Amplify LSH)

如果只使用单个哈希函数 $h \in \mathcal{H}$，虽然高效但选择性差，假阳性多。两种方法可以改善：
- 增加选择性：使用多个哈希函数 $h_1, \ldots, h_b$，只有当所有 $h_i(p) = h_i(q)$ 时才认为接近。通过创建分带哈希(banded-hash) $H(p) : \mathcal{B} \to \mathcal{U}^b$ 实现。
- 减少假阴性：使用多个哈希函数 $h_1, \ldots, h_r$，若在任一哈希函数中碰撞则报告相似。

**分带(Banding)**：创建 $r$ 个分带哈希函数 $H$，每个由 $b$ 个哈希函数组成。需要 $r \cdot b$ 个哈希函数。若 $q$ 与 $p$ 在任一分带哈希表中相交（需要所有哈希函数碰撞），则返回相似。

**※【例题】** 分带：$r = 3$ 个分带哈希函数 $H_1, H_2, H_3$，每个 $b = 2$ 个哈希函数，映射到 $\mathcal{U} = \{0, 1, 2, \ldots, 5\}$。只有 $p_1$ 和 $p_4$ 被标记为接近。$p_1$ 在 $H_1$ 上碰撞（$h_{1,1}=3, h_{1,2}=5$），$p_4$ 在 $H_2$ 上碰撞（$h_{2,1}=1, h_{2,2}=2$）。$p_2$ 虽在 3 个单独哈希函数上碰撞，但从未在完整分带上碰撞。

**分带分析**：设 $s = s(p, q)$，使用 $r$ 个分带，每个 $b$ 个哈希函数。
$s$ = 任一哈希函数碰撞概率
$s^b$ = 1 个分带中所有哈希函数碰撞概率
$1 - s^b$ = 1 个分带中并非所有哈希函数碰撞概率
$(1 - s^b)^r$ = 没有任一分带中所有哈希函数碰撞概率
$f_{b,r}(s) = 1 - (1 - s^b)^r$ = 至少 1 个分带中所有哈希函数碰撞概率

**参数选择**：通常有哈希函数预算 $t = r \cdot b$。$f$ 的最陡斜率阈值约为 $\tau' \approx (1/r)^{1/b}$。给定相似度 $s$，若要返回更相似于 $\tau'$ 的对象，可解 $b = t/r$ 得到 $b \approx -\log_{\tau'}(t)$，$r = t/b$。通常最好绘制 $f$ 函数并调整 $b$ 和 $r$ 以找到所需权衡。若 $r$ 和 $b$ 无预算限制，随着它们增加，S 曲线变得更陡峭，能更准确地检索所有且仅接近的数据点。

### 4.6.4 角度距离的 LSH(LSH for Angular Distance)

**※【定义】** 角度 LSH 家族：$\mathcal{H}_{\text{ang}} = \{h_u(\cdot) = \text{sign}(\langle \cdot, u \rangle) \mid u \sim \text{Unif}(\mathbb{S}^{d-1})\}$。首先均匀随机选择单位向量 $u \in \mathbb{S}^{d-1}$。然后 $h_u(a) = \text{sign}(\langle a, u \rangle)$，若点积为正返回 $+1$，否则返回 $-1$。

可以证明 $\mathbf{Pr}_{h_u \in \mathcal{H}_{\text{ang}}}[h_u(a) = h_u(b)] = s_{\text{ang}}(a, b) / \pi$。因此，按 $\pi$ 缩放后，放大分析与简单三角形相似度例子完全相同。

**几何理解**：在 $\mathbb{S}^1$ 上，$a, b$ 诱导距离 $d_{\text{ang}}(a, b) \in [0, \pi]$。每个点对应一个集合 $F_{a, \bot} \subset \mathbb{S}^1$，包含所有满足 $\langle a, u \rangle \geq 0$ 的 $u$。向量 $u$ 导致 $h_u(a) \neq h_u(b)$ 仅当它恰好位于 $F_{a, \bot}$ 或 $F_{b, \bot}$ 之一。这些区域的异或弧长为 $2 \cdot d_{\text{ang}}(a, b)$。因此，$h_u(a) \neq h_u(b)$ 的 $u$ 的比例为 $(2 d_{\text{ang}}(a, b)) / (2 \pi)$。由于 $s_{\text{ang}}(a, b) / \pi = 1 - d_{\text{ang}}(a, b) / \pi$，概率 $h_u(a) = h_u(b)$ 为 $s_{\text{ang}}(a, b) / \pi$。

**生成随机单位向量**：生成 $d$ 个均匀向量 $u_1, \ldots, u_d \sim \text{Unif}[-1, 1]$ 然后归一化不会给出均匀方向（倾向于角落）。最简单的方法是通过高斯随机变量。$d$ 维高斯分布 $\mathcal{G}_d(x) = \frac{1}{(2 \pi)^{d/2}} e^{-\|x\|_2^2 / 2}$。若 $u_1, u_2 \sim \text{Unif}[0, 1]$，可以使用 Box-Muller 变换生成两个独立的 1 维高斯随机变量：$y_1 = \sqrt{-2 \ln(u_1)} \cos(2 \pi u_2)$，$y_2 = \sqrt{-2 \ln(u_1)} \sin(2 \pi u_2)$。均匀高斯的所有坐标（在任何正交基中）都是独立的。若 $x \sim \mathcal{G}_d$，则 $u = x / \|x\|_2$ 是均匀的 $u \sim \text{Unif}(\mathbb{S}^{d-1})$。

### 4.6.5 欧几里得距离的 LSH(LSH for Euclidean Distance)

**※【定义】** 欧几里得 LSH 家族：$\mathcal{H}_{L_2, \tau} = \{h_{u, \eta}(\cdot) = \lceil \langle \cdot, u \rangle + \eta \rceil \mid u \sim \text{Unif}(\mathbb{S}^{d-1}), \eta \sim \text{Unif}[0, \tau]\}$。依赖于随机单位向量 $u$ 和随机偏移 $\eta$。哈希操作首先投影到方向 $u$，然后偏移 $\eta$，并向上取整为整数。对于足够大的整数 $t$，通常映射到有限域 $0, 1, \ldots, t-1$ 通过 $h_{u, \eta}(x) \bmod t$。

哈希家族 $\mathcal{H}_{L_2, \tau}$ 关于 $d_2$ 是 $(\tau/2, 2\tau, 1/2, 1/3)$-敏感的。

**几何理解**：投影到 $u$ 是收缩的，所以 $|\langle a, u \rangle - \langle b, u \rangle| \leq \|a - b\|$。若 $\|a - b\| \leq \tau/2$，则 $|\langle a, u \rangle - \langle b, u \rangle| \leq \tau/2$，$a, b$ 落入同一桶的概率至少 $1/2 = \alpha$。若 $\|a - b\| > 2\tau$ 且它们碰撞，则 $u$ 必须足够正交于 $(a - b)$，使得范数减少一半。$|\langle a, u \rangle - \langle b, u \rangle| = \|a - b\| \|u\| |\cos \theta_{u, a-b}|$。$|\cos(\theta_{u, a-b})| \leq 1/2$ 仅当 $\theta_{u, a-b} \geq \pi/3$。固定包含 $0, u, a-b$ 的 2 维子空间，概率 $\theta_{u, a-b} \geq \pi/3$ 至多为 $1/3 = \beta$。

**※【定义】** p-稳定分布(p-stable distribution)：分布 $\mu$ 在 $\mathbb{R}$ 上是 p-稳定的（对于 $p \geq 0$），若对于 $d+1$ 个随机变量 $X_0, X_1, \ldots, X_d \sim \mu$，以及任意 $d$ 个实值 $\{\nu_1, \ldots, \nu_d\}$，随机变量 $\sum_i \nu_i X_i$ 与 $(\sum_i |\nu_i|^p)^{1/p} X_0$ 分布相同。这样的分布对于 $p \in (0, 2]$ 存在。特例：高斯分布是 2-稳定的，柯西分布是 1-稳定的。这允许通过仔细调整系数，用单个随机变量替换随机变量之和。对于 $p=2$，可以通过选择 $d+1$ 个随机高斯向量 $g_0, g_1, \ldots, g_d \sim \mathcal{G}$ 并计算 $(1/g_0) \sum_i g_i \nu_i = (1/g_0) \langle g, \nu \rangle$ 来估计 $\|\nu\|_2$。使用柯西随机变量可以估计 $\|\nu\|_1$。

### 4.6.6 最小哈希作为杰卡德距离的 LSH(Min Hashing as LSH for Jaccard Distance)

**※【定义】** 最小哈希家族(Min hash family)：$\mathcal{H}_J = \{h_\sigma(A) = \min_{\omega \in A} \sigma(\omega) \mid \sigma \sim \text{Unif}(\Pi_n)\}$。其中 $\Pi_n$ 是 $\Omega$ 到 $[n] = \{1, 2, \ldots, n\}$ 中不同整数的所有排列的集合。哈希函数 $h_\sigma$ 对 $A$ 中的每个 $\omega$ 应用 $\sigma$，然后返回最小值。

**※【例题】** 最小哈希：$A = \{a, b, e, f, h\}$ 和 $B = \{b, c, e, g, j\}$ 在 $\Omega = \{a, b, c, d, e, f, g, h, i, j\}$ 上。选择两个排列 $\sigma_1, \sigma_2$。$h_{\sigma_1}(A) = \min\{\sigma_1(a), \sigma_1(b), \sigma_1(e), \sigma_1(f), \sigma_1(h)\} = \min\{4, 6, 2, 8, 5\} = 2$。$h_{\sigma_1}(A) = h_{\sigma_1}(B) = 2$，$h_{\sigma_2}(A) = 3 \neq h_{\sigma_2}(B) = 1$。估计相似度为 $1/2$。

可以证明 $\mathbf{Pr}_{h_\sigma \in \mathcal{H}_J}[h_\sigma(A) = h_\sigma(B)] = s_J(A, B)$。考虑三类元素：$\Omega_{A \cap B}$（在两者中），$\Omega_{A \triangle B}$（在恰好一个中），$\Omega_{\notin A, B}$（都不在）。杰卡德相似度为 $s_J(A, B) = \frac{|\Omega_{A \cap B}|}{|\Omega_{A \cap B}| + |\Omega_{A \triangle B}|}$。哈希碰撞概率恰好为 $\frac{|\Omega_{A \cap B}|}{|\Omega_{A \cap B}| + |\Omega_{A \triangle B}|}$。$h_\sigma$ 返回的值只能是 $\Omega_{A \cap B} \cup \Omega_{A \triangle B}$ 中的元素，这些元素等可能，仅当来自 $\Omega_{A \cap B}$ 时才碰撞。

**快速最小哈希函数**：标准定义需要存储 $\Omega \to [|\Omega|]$ 的哈希函数，对于 IP 地址或字符串等巨大隐式域不实用。替代方案是使用分离哈希函数 $h : \Omega \to [N]$，其中 $N$ 足够大（如最大集合大小的三次方）。使用内置哈希函数 $f$（如 SHA-1 加盐），定义 $f_S(\omega) = f(\omega \oplus S)$。哈希家族为 $\mathcal{H}_J' = \{h_{f, S}(A) = \min_{\omega \in A} f_S(\omega) \mid f: \Omega \to [N], S \sim \text{随机盐}\}$。使用 $k$ 个哈希函数 $h_1, \ldots, h_k \in \mathcal{H}_J'$，得到最小哈希签名(minhash signature)向量 $\nu(A) \in [N]^k$，其中 $\nu(A)_j = h_j(A)$。可以使用单次扫描计算。

**※【算法】** 高效最小哈希签名(Efficient MinHash Signature)：
```
Algorithm 4.6.1 Efficient MinHash Signature(A, {S₁, . . ., Sₖ})
Initialize v = (v₁ = ∞, v₂ = ∞, . . ., vₖ = ∞)
for ω ∈ A do
    for j = 1 to k do
        if f_{Sⱼ}(ω) < vⱼ then
            vⱼ ← f_{Sⱼ}(ω)
return v(A) = v
```
如果集合 $A$ 是文本的 k-gram 表示，可以更流线化：扫描文档时，生成的每个 k-gram $\omega$ 可以由 k 个哈希函数哈希，并与维护的最小哈希签名比较。重复的 k-gram 不会影响最小哈希值。