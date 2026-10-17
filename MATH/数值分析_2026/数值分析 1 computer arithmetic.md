
## 1.1 浮点数系统

**※【定义】** 浮点数(floating-point number, FPN)：
形如 $\pm m \times \beta^e$ 的数，其中：
- 尾数(mantissa) $m = d_0 + \frac{d_1}{\beta} + \dots + \frac{d_{p-1}}{\beta^{p-1}}$，$d_i \in [0, \beta-1]$；
- 基数(base) $\beta$，通常计算机中 $\beta=2$；
- 指数(exponent) $e \in [L, U]$。

**※【定义】** 规格化浮点数(normalized FPN)：
满足 $1 \le m < \beta$，即 $d_0 \in [1, \beta-1]$。所有规格化浮点数的集合记为 $\mathcal{F}$。

**※【定义】** 可表示实数范围 $\mathcal{R}(\mathcal{F})$：
介于下溢限 $\mathrm{UFL}(\mathcal{F})$ 和上溢限 $\mathrm{OFL}(\mathcal{F})$ 之间的实数集合。

**※【例题】** 设 $\beta=2, p=3, L=-1, U=1$：
- 最大数：$(1 + \frac{1}{2} + \frac{1}{4}) \times 2^1 = 3.5$；
- 最小规格化正数：$1 \times 2^{-1} = 0.5$。

**※【定义】** 次正规浮点数(subnormal FPN)：
形如 $\pm m \times \beta^e$，其中 $m = 0 + \frac{d_1}{\beta} + \dots + \frac{d_{p-1}}{\beta^{p-1}}$，$d_i \in [0, \beta-1]$，且 $e = L$。
扩展浮点数系统：$\mathcal{F} \cup \{\text{次正规数}\}$。

**※【定义】** 上溢限(overflow limit, OFL)与下溢限(underflow limit, UFL)：
- $\mathrm{OFL}(\mathcal{F}) = \max |\mathcal{F}|$，对应尾数所有位均为 $\beta-1$；
- $\mathrm{UFL}(\mathcal{F}) = \min |\mathcal{F} \setminus \{0\}|$，对应 $d_0=1$，其余位为 $0$。

**※【例题】** IEEE-754 标准：
- 单精度：$\beta=2, p=23+1, L=-126, U=127$。前导位可省略，共需 32 位：$1$（符号）+ $23$（尾数）+ $8$（指数）。$e=-127$ 保留给次正规数，$e=128$ 保留给无穷大和 NaN。
- 双精度：$\beta=2, p=52+1, L=-1022, U=1023$。共需 64 位。

## 1.2 单个数的舍入

**※【定义】** 舍入(rounding)：
映射 $\mathrm{fl}: \mathbb{R} \to \mathcal{F} \cup \{+\infty, -\infty, \mathrm{NaN}\}$。默认舍入模式为“最近舍入”(round to nearest)，即对于 $x \in \mathcal{R}(\mathcal{F})$，$\mathrm{fl}(x)$ 选取使 $|\mathrm{fl}(x) - x|$ 最小的浮点数。若出现平局，则采用“向偶数舍入”(round to even)，即选取最后一位数字 $d_{p-1}$ 为偶数的那个。

*注：对二进制，$d_{p-1}$ 的奇偶性决定该数的奇偶性；对奇数进制，可能需要转换到十进制比较。*

**※【例题】** 设 $\beta=2, p=3, L=-1, U=1$：
$x=2.1 \to \mathrm{fl}(x)=2$；$x=2.75 \to \mathrm{fl}(x)=2.75$。

**※【定义】** 机器精度(machine epsilon)：
单位舍入误差(unit roundoff)定义为
$\epsilon_u := \frac{1}{2} \epsilon_M = \frac{1}{2} \beta^{1-p}$ (1.11)
其中 $\epsilon_M = \beta^{1-p}$ 是机器精度。

**※【定理】** 舍入误差范围：
对于 $x \in \mathcal{R}(\mathcal{F})$，有
$\mathrm{fl}(x) = x(1+\delta), \quad |\delta| < \epsilon_u$ (1.12)
等价地，$\frac{|\mathrm{fl}(x)-x|}{|x|} < \frac{1}{2}\beta^{1-p}$。

**证明**：设 $m \times \beta^e \le x \le (m+\beta^{1-p}) \times \beta^e$，则 $|\mathrm{fl}(x)-x| \le \frac{1}{2}\beta^{1-p+e}$。对于规格化数 $x$，有 $|x| \ge \beta^e$，故 $\frac{|\mathrm{fl}(x)-x|}{|x|} \le \frac{1}{2}\beta^{1-p}$。$\square$

## 1.3 浮点运算

**※【定义】** 两个浮点数的加法/减法：
设 $a, b \in \mathcal{F}$，表示为 $a = M_a \times \beta^{e_a}$，$b = M_b \times \beta^{e_b}$，其中 $M_a = \pm m_a$，$M_b = \pm m_b$。假设 $|a| \ge |b|$，和 $c := \mathrm{fl}(a+b) \in \mathcal{F}$ 在精度至少为 $2p$ 的寄存器中按如下步骤计算：
(i) 指数比较：若 $e_a - e_b > p+1$，则令 $c=a$ 并返回；否则令 $e_c \gets e_a$，$M_b \gets M_b / \beta^{e_a - e_b}$。
(ii) 在寄存器中执行加法 $M_c \gets M_a + M_b$，采用最近舍入。
(iii) 规格化：
- 若 $|M_c| = 0$，返回 $0$；
- 若 $|M_c| \in (\beta, \beta^2)$，令 $M_c \gets M_c / \beta$，$e_c \gets e_c + 1$；
- 若 $|M_c| \in (0, \beta - \epsilon_u(p))$，重复 $M_c \gets M_c \beta$，$e_c \gets e_c - 1$，直到 $|M_c| \in [1, \beta)$；
- 若 $|M_c| \in [\beta - \epsilon_u(p), \beta]$，令 $|M_c| \gets 1.0$，$e_c \gets e_c + 1$。
(iv) 范围检查：若 $e_c$ 上溢返回 NaN，下溢返回 $0$。
(v) 将 $M_c$ 舍入到精度 $p$。
(vi) 令 $c \gets M_c \times \beta^{e_c}$。

**※【定理】** 对于 $a, b \in \mathcal{F}$，若 $a+b \in \mathcal{R}(\mathcal{F})$，则
$\mathrm{fl}(a+b) = (a+b)(1+\delta), \quad |\delta| < \epsilon_u$ (1.15)

**※【例题】** 在 $\mathcal{F}:(10,4,-7,8)$ 中计算 $c := \mathrm{fl}(a+b)$，其中 $a = 1.234 \times 10^4$，$b = 5.678 \times 10^0$：
(i) $b \gets 0.0005678 \times 10^4$，$e_c \gets 4$；
(ii) $m_c \gets 1.2345678$；
(iii) 无需操作；
(iv) 无需操作；
(v) $m_c \gets 1.235$；
(vi) $c = 1.235 \times 10^4$。
若 $b = 5.678 \times 10^{-2}$，则在步骤 (i) 返回 $c=a$。

**※【例题】** 计算 $c := \mathrm{fl}(a+b)$，$a = 1.000 \times 10^0$，$b = -9.000 \times 10^{-5}$，在 $\mathcal{F}:(10,4,-7,8)$ 中：
(i) $b \gets -0.0000900 \times 10^0$，$e_c \gets 0$；
(ii) $m_c \gets 0.9999100$；
(iii) $e_c \gets -1$，$m_c \gets 9.9991000$；
(iv) 无需操作；
(v) $m_c \gets 9.999$；
(vi) $c = 9.999 \times 10^{-1}$。
若 $b = -9.000 \times 10^{-6}$，则在步骤 (i) 返回 $c=a$。

**※【定义】** 两个浮点数的乘法：
设 $a, b \in \mathcal{F}$，表示为 $a = M_a \times \beta^{e_a}$，$b = M_b \times \beta^{e_b}$。乘积 $c := \mathrm{fl}(ab) \in \mathcal{F}$ 在精度至少为 $2p$ 的寄存器中计算：
(i) 指数相加：$e_c \gets e_a + e_b$；
(ii) 执行乘法 $M_c \gets M_a M_b$；
(iii) 规格化：若 $|M_c| \in (\beta, \beta^2)$，令 $M_c \gets M_c / \beta$，$e_c \gets e_c + 1$；若 $|M_c| \in [\beta - \epsilon_u(p), \beta]$，令 $|M_c| \gets 1.0$，$e_c \gets e_c + 1$；
(iv) 范围检查；
(v) 将 $M_c$ 舍入到精度 $p$；
(vi) 令 $c \gets M_c \times \beta^{e_c}$。

**※【引理】** 对于 $a, b \in \mathcal{F}$，若 $|ab| \in \mathcal{R}(\mathcal{F})$，则
$\mathrm{fl}(ab) = (ab)(1+\delta), \quad |\delta| < \epsilon_u$ (1.16)

**证明**：误差仅来自步骤 (v) 的舍入，由定理 1.29 即得。$\square$

**※【例题】** 在 $\mathcal{F}:(10,4,-7,8)$ 中计算 $c := \mathrm{fl}(ab)$，$a = 2.345 \times 10^4$，$b = 6.789 \times 10^0$：
(i) $e_c \gets 4$；
(ii) $M_c \gets 15.920205$；
(iii) $m_c \gets 1.5920205$，$e_c \gets 5$；
(iv) 无需操作；
(v) $m_c \gets 1.592$；
(vi) $c = 1.592 \times 10^5$。

**※【定义】** 两个浮点数的除法：
设 $a, b \in \mathcal{F}$，表示为 $a = M_a \times \beta^{e_a}$，$b = M_b \times \beta^{e_b}$。商 $c = \mathrm{fl}(a/b) \in \mathcal{F}$ 在精度至少为 $2p+1$ 的寄存器中计算：
(i) 若 $m_b = 0$，返回 NaN；否则 $e_c \gets e_a - e_b$；
(ii) 在寄存器中执行除法 $M_c \gets M_a / M_b$，采用最近舍入；
(iii) 规格化：若 $|M_c| < 1$，令 $M_c \gets M_c \beta$，$e_c \gets e_c - 1$；
(iv) 范围检查；
(v) 将 $M_c$ 舍入到精度 $p$；
(vi) 令 $c \gets M_c \times \beta^{e_c}$。

**※【引理】** 对于 $a, b \in \mathcal{F}$，若 $a/b \in \mathcal{R}(\mathcal{F})$，则
$\mathrm{fl}(a/b) = (a/b)(1+\delta), \quad |\delta| < \epsilon_u$ (1.17)

**证明**：
- 若 $|M_a| = |M_b|$，无舍入误差，结论显然。
- 若 $|M_a| > |M_b|$，有 $|M_a/M_b| \ge \frac{\beta - \epsilon_M}{\beta - 2\epsilon_M} > 1 + \beta^{-1}\epsilon_M$，故步骤 (iii) 不触发。寄存器精度为 $2p+1$ 时，步骤 (ii) 的单位舍入误差为 $\beta^{-2}\epsilon_u\epsilon_M$，步骤 (v) 的单位舍入误差为 $\epsilon_u$。于是 $M_{c2} = M_{c1} + \delta_2 = \frac{M_a}{M_b} + \delta_1 + \delta_2 = \frac{M_a}{M_b}(1+\delta)$，其中 $|\delta_1| < \beta^{-2}\epsilon_u\epsilon_M$，$|\delta_2| < \epsilon_u$，且 $|\delta| = \left|\frac{\delta_1+\delta_2}{M_a/M_b}\right| < \frac{\epsilon_u(1+\beta^{-2}\epsilon_M)}{1+\beta^{-1}\epsilon_M} < \epsilon_u$。
- 若 $|M_a| < |M_b|$，则 $|M_{c1}| < 1$，步骤 (iii) 触发。$M_{c1} = \frac{M_a}{M_b} + \delta_1$，$M_{c2} = \beta M_{c1} + \delta_2 = \beta \frac{M_a}{M_b} \left(1 + \frac{\beta\delta_1 + \delta_2}{\beta M_a/M_b}\right)$。分母满足 $\beta |M_a/M_b| \ge \frac{\beta}{\beta - \epsilon_M} > 1 + \beta^{-1}\epsilon_M$。故 $|\delta| = \left|\frac{\beta\delta_1 + \delta_2}{\beta M_a/M_b}\right| < \frac{\beta^{-1}\epsilon_u\epsilon_M + \epsilon_u}{1+\beta^{-1}\epsilon_M} = \epsilon_u$。$\square$

**※【例题】** 精度至少 $2p+1$ 的必要性：
取 $a=6, b=7$，$\mathcal{F} = \{10, 1, -10, 10\}$。若仅用 $2p$ 精度：
$a/b = 0.857142\dots$，保留 2 位有效数字（不含前导零）得 $8.5 \times 10^{-1}$，再舍入得 $m_c = 8 \times 10^{-1}$，误差较大。

**※【定理】** 机器算术模型：
设 $\mathcal{F}$ 为精度 $p$ 的规格化浮点数系统。对每种算术运算 $\odot = +, -, \times, /$，有
$\forall a,b \in \mathcal{F}, \ a \odot b \in \mathcal{R}(\mathcal{F}) \implies \mathrm{fl}(a \odot b) = (a \odot b)(1+\delta)$ (1.19)
其中 $|\delta| < \epsilon_u$，当且仅当这些二元运算在精度为 $2p+1$ 的寄存器中执行。
**证明**：由引理 1.36、1.39、1.41 可得。

## 1.4 舍入误差的传播

**※【定理】** 若 $\forall i = 0,1,\dots,n$，$a_i \in \mathcal{F}$，$a_i > 0$，则
$\mathrm{fl}\left(\sum_{i=0}^n a_i\right) = (1+\delta_n) \sum_{i=0}^n a_i$ (1.20)
其中 $|\delta_n| < (1+\epsilon_u)^n - 1 \approx n\epsilon_u$。

*注：若 $a_i > 0$ 不总成立，定理不保证。*

**证明**：定义 $s_k := \sum_{i=0}^k a_i$，$s_0^* := a_0$，$s_{k+1}^* := \mathrm{fl}(s_k^* + a_{k+1})$。令 $\delta_k := \frac{s_k^* - s_k}{s_k}$，$\epsilon_k := \frac{s_{k+1}^* - (s_k^* + a_{k+1})}{s_k^* + a_{k+1}}$。则
$\delta_{k+1} = \frac{s_{k+1}^* - s_{k+1}}{s_{k+1}} = \frac{(s_k^* + a_{k+1})(1+\epsilon_k) - s_{k+1}}{s_{k+1}} = \epsilon_k + \delta_k(1+\epsilon_k)\frac{s_k}{s_{k+1}}$。
因 $a_i > 0$，$s_k < s_{k+1}$，且 $|\epsilon_k| < \epsilon_u$，故 $|\delta_{k+1}| < \epsilon_u + |\delta_k|(1+\epsilon_u)$。归纳得 $|\delta_{k+1}| < (1+\epsilon_u)^{k+1} - 1$。$\square$

**※【定理】** 三项和误差：
$a,b,c \in \mathcal{F}$，$\mathrm{fl}((a+b)+c) = (a+b+c)(1+\delta)$，若 $a,b,c > 0$，则 $|\delta| \le (1+\epsilon_u)^2 - 1$。
**证明**：$\mathrm{fl}(\mathrm{fl}(a+b)+c) = \mathrm{fl}((a+b)(1+\delta_1)+c) = [(a+b)(1+\delta_1)+c](1+\delta_2) = (a+b+c)\left[1 + \frac{a+b}{a+b+c}\delta_1\right](1+\delta_2)$。因 $a,b,c>0$，$\frac{a+b}{a+b+c} \le 1$，故 $1+\delta \le (1+\delta_1)(1+\delta_2)$，$|\delta| \le (1+\epsilon_u)^2 - 1$。推广至 $n$ 项同理。

**※【定理】** 乘积误差界：
给定 $\mu \in \mathbb{R}^+$ 和正整数 $n \le \lfloor \frac{\ln 2}{\mu} \rfloor$，若 $|\delta_i| \le \mu$，$i=1,\dots,n$，则
$1 - n\mu \le \prod_{i=1}^n (1+\delta_i) \le 1 + n\mu + (n\mu)^2$ (1.22)
等价地，对于 $I_n := [-\frac{1}{1+n\mu}, 1]$，存在 $\theta \in I_n$ 使得
$\prod_{i=1}^n (1+\delta_i) = 1 + \theta(n\mu + n^2\mu^2)$ (1.23)

**证明**：由 $|\delta_i| \le \mu$ 得 $(1-\mu)^n \le \prod (1+\delta_i) \le (1+\mu)^n$。对 $(1-\mu)^n$ 在 $\mu=0$ 处 Taylor 展开得 $(1-\mu)^n \ge 1 - n\mu$。又 $e^x \le 1 + x + \frac{x^2}{2}e^x$，令 $x=n\mu$，结合 $n\mu \le \ln 2$ 得 $e^{n\mu} \le 1 + n\mu + (n\mu)^2$，从而 $(1+\mu)^n \le e^{n\mu} \le 1 + n\mu + (n\mu)^2$。最后由介值定理得 (1.23)。$\square$

## 1.5 避免灾难性抵消

**※【例题】** 灾难性抵消(catastrophic cancellation)：
对于 $x, y \in \mathcal{R}(\mathcal{F})$，由定理 1.29 和 1.42，
$\mathrm{fl}(\mathrm{fl}(x) \odot \mathrm{fl}(y)) = (\mathrm{fl}(x) \odot \mathrm{fl}(y))(1+\delta_3) = (x(1+\delta_1) \odot y(1+\delta_2))(1+\delta_3)$，$|\delta_i| \le \epsilon_u$。
- 乘法是精确的：$\mathrm{fl}(\mathrm{fl}(x) \times \mathrm{fl}(y)) = xy(1+\delta_1)(1+\delta_2)(1+\delta_3) = xy[1 + \theta(3\epsilon_u + 3\epsilon_u^2 + \epsilon_u^3)]$，$\theta \in [-1,1]$。
- 除法也精确：$\mathrm{fl}(\mathrm{fl}(x)/\mathrm{fl}(y)) = \frac{x(1+\delta_1)}{y(1+\delta_2)}(1+\delta_3) \approx \frac{x}{y}(1+\delta_1)(1-\delta_2)(1+\delta_3)$。
- 但加法/减法可能不精确：$\mathrm{fl}(\mathrm{fl}(x) + \mathrm{fl}(y)) = (x+y)\left(1 + \delta_3 + \frac{x\delta_1 + y\delta_2}{x+y} + \delta_3 \frac{x\delta_1 + y\delta_2}{x+y}\right)$。当 $x+y \to 0$ 时，相对误差可任意大。

**※【例题】**
$\sqrt{x^2+1} - x = \frac{1}{\sqrt{x^2+1} + x}$，前者在 $x$ 很大时不准；
$x - \sin x = x - (x - \frac{x^3}{3!} + O(x^5))$，在 $0 < x \ll 1$ 时不准。

**※【定理】** 最高有效位损失：
设 $x, y \in \mathcal{F}$，$x > y > 0$，且
$\beta^{-t} \le 1 - \frac{y}{x} \le \beta^{-s}$ (1.24)
则在减法 $x - y$ 中，最多损失 $t$ 位、至少损失 $s$ 位最高有效数字。

**证明**：写 $x = m_x \times \beta^n$，$y = m_y \times \beta^m$，$1 \le m_x, m_y < \beta$。对齐指数后 $y = (m_y \times \beta^{m-n}) \times \beta^n$。则 $x - y = (m_x - m_y \times \beta^{m-n}) \times \beta^n$，故 $m_{x-y} = m_x(1 - \frac{y}{x})$。由条件得 $\beta^{-t} \le m_{x-y} < \beta^{1-s}$。要将 $m_{x-y}$ 规格化到 $[1,\beta)$，需左移至少 $s$ 次、至多 $t$ 次，即损失 $s$ 到 $t$ 位有效数字。$\square$

**规则 1.47**：应尽可能避免灾难性抵消。

**※【例题】** 计算 $y = f(x) = x - \sin x$ 当 $x \to 0$。直接计算会因 $x \approx \sin x$ 产生灾难性抵消。应使用 Taylor 展开：
$x - \sin x = x - (x - \frac{x^3}{3!} + \frac{x^5}{5!} - \frac{x^7}{7!} + \dots) = \frac{x^3}{3!} - \frac{x^5}{5!} + \frac{x^7}{7!} + \dots$

**※【例题】** 对长序列 $n$ 个浮点数求和，宜采用补偿求和(compensated summation)，使用高精度补偿变量，使总误差不依赖于 $n$，仅取决于结果的浮点精度。

**总结 1.50**：由机器算术模型（定理 1.42），相对舍入误差小于单位舍入误差；舍入误差的积累最坏是线性的（因 $\epsilon_u \ll 1$）。但这并不意味着可以总是直接进行浮点运算。当两个相近的数相减时，必须用数学等价的方式重新表述计算，以避免灾难性抵消。