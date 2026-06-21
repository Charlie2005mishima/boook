## 1 linear factor model
### 1.1 portforlio sorting

- procedure
	1. Determine the relevant values of the sorting variable.
	2. Determine breakpoints.
	3. Portfolio formation.
	4. Average portfolio returns
- paramters
	- Rebalancing frequency
	- Sorting variable
	- Number of portfolios
	- Weighting scheme (value-weighted based on size / equal-weighted)
- presenting the results
	- t-test of return difference
	- cumulative return
- double sorting
	- independent double sorting
		- not orderly
		- the different group number
	- dependent double sorting
		- control variable first
		- the same group number

### 1.2 fama-macbeth regression

$r_{it}=\alpha+\beta_{1,t}x_{1,t-1}+\beta_{2,t}x_{2,t-1}+\varepsilon_{it}$ 

### 1.3 fama-french model

$R_{it}^e=\alpha _{i}+\beta _{iM}R_{Mt}^e+\beta _{iSMB}SMB_{t}+\beta _{iHML}HML_{t}+u_{it}$ 

- SMB $SMB_{t}=\frac{1}{3}()-\frac{1}{3}()$
- HML $HML_{t}=\frac{1}{2}()-\frac{1}{2}()$

![[Pasted image 20260610163333.png|300]]

### 1.4 momentum

### 1.5 carhart model

$R_{it}^e=\alpha _{i}+\beta _{iM}R_{Mt}^e+\beta _{iSMB}SMB_{t}+\beta _{iHML}HML_{t}+\beta _{iWML}WML_{t}+u_{it}$ 

- WML $WML_{t}=\frac{1}{2}()-\frac{1}{2}()$ 
![[Pasted image 20260610163639.png|300]]

## 2 anomalies

- reasons for anomalies
	- market ineffienciency: behavioral biases + limits to arbitrage
	- Inadequacies in the underlying asset-pricing model

### 2.1 IVOL

- higher IVOL, lower future return
- short-sell constraints: many investors who would buy a stock they see as underpriced are reluctant or unable to short a stock they see as overpriced
### 2.2 asset / liabilities based anomalies

- new equity issuance puzzle: poor long-run performance
- asset growth: lower returns
- accruals: higher accruals earn low returns
- failure: higher distress firms earn lower returns
- higher profit, higher returns
- earning momentum
- higher momentum when:
	- Firms followed by fewer analysts
	- Growth stocks
	- Higher information uncertainty stocks
	- High turnover stocks

### 2.3 bubble

- optimal timing problem
	- Trading off the benefits of riding the bubble and the costs of being trapped in the market crash
- Dispersion of opinions
	- Arbitrageurs are sequentially informed of the mispricing
- Arbitrageurs may lack the incentive or the ability to synchronize their attack on a bubble
- Bubbles can survive for a relatively long period
- Even when arbitrageurs are risk-neutral, capital unconstrained, and well informed

### 2.4 the casual effect of limits to arbitrage

- SHO: The SHO program removed this requirement for a randomly selected group of pilot stocks, which relaxes the short-sell constraint.
- 有SHO处理（解除卖空限制）的异象因子的超额收益率是更少的，因为超额收益可能是缺乏做空渠道导致的
- N $\times$ T N: 11 anomaly * 2 SHO period: pre, during, post
- 当我们认为只有pre和during两期的时候
	- $R_{i,t} = b_0 + b_1 Treat_i + b_2 Post_t + b_3 Treat_i \times Post_i +\epsilon_{i,t}$ 
	- $Post_t = I\{t \in during\}$ $Treat_i = I\{i \in SHO\}$ $R_{i,t} = \{多空投资组合，多头，空头\}$
	- $H_0: b_3 < 0$
- 研究三期
	- $R_{i,t} = b_0 + b_1 SHO_i + b_2 Post_t + b_3 During_t + b_3 SHO_i \times Post_i + b_4 SHO_i \times During_i +\epsilon_{i,t}$ 
	- $H_0: b_3 < 0, b_4 = 0$ 

## 3 attention

- limited atterntion results in categorial learning: allocate more attention to market‐ and sector‐level factors than to firm‐specific factors
	- enhance the impact of behavioral biases on asset prices >> excessive co-movement
- Limited attention to continuous information
	- frog-in-the-pan $ID=sgn(PRET)\times(\%neg-\%pos)$ 
- attention measurement
	- trading volume, extreme returns: noisy and catch-all
	- google search frequency: capturing retail attention: Increased retail attention → positive price pressure
	- Bloomberg search frequency: institutional attention
	- using random front‐page news positioning on the Bloomberg terminals
- Limited attention to long‐term information: population
- Limited attention to economic links

## 4 market microstructure

|      | price below limit | price above limit |
| ---- | ----------------- | ----------------- |
| buy  | limit-buy order   | stop-buy order    |
| sell | stop-loss order   | limit-sell order  |
- trading costs
	- brokerage commission: fee paid to broker for making the transaction
	- spread: difference between the bid and asked prices
- margin trading 
	- percentage margin = equity / asset = equity / ( liability + equity )
	- margin rate= equity / liability
## 5 liquidity I

- liquidity: ability to buy or sell significant quantities of a security quickly and with minimal or no price impact.
- sources of illiquidity
	- transaction costs (brokerage fees, order processing costs)
	- Price pressure and inventory risks
	- Asymmetric information (adverse selection component)
	- Search friction
- liquidity measure
	- trading volumn
	- bid-ask spread
	- Amihud illiquidity ratio: $\text{Illiq}_{i,t}=\left( \frac{1}{N_{i,t}} \right)\sum_{d} \frac{|r_{i,d}|}{vol_{i,d}}$ 
	- Kyle's lambda: $\Delta p_{t}=a+\lambda q_{t}+\varepsilon _{t}$ 
	- zero-return measure
- premium for illquidity
	- illiquidity related with size and volatility
	- Construct portfolios of illiquid stocks and liquid stocks, within each country (𝑐), in month (𝑡).
	- Run Fama-McBeth regressions to estimate illiquidity premium
		- $R_{c,j,t}=b_{0\,c,t}+b_{1\,c,t}+ILLIQ_{c,j,t-1}+ctrls_{c,j,t-1}+\varepsilon_{c,j,t}$ 
	- IMLc,t = Illiquid-minus-liquid return for country c in month t.

## 6 liquidity II
- Some liquidity cost regressions
	- ASPR：买卖价差；Rm：市场收益率；Ri：个股收益率；STD：波动率；TURN换手率
## 7 macro economic analysis
![[Pasted image 20260621105731.png|500]]
- Three factors determine a firm’s sensitivity to the business cycle
	- Sensitivity of sales
	- Operating leverage
	- Financial leverage
- peak phrase: commodity
- contraction phrase: defensicve(pharmaceuticals, food, utilities)
- trough phrase: capital goods
- expansion phrase: cyclical consumer sectors (autos, luxury goods, durables)
## 8 equity valuation

- 相对估值法（Valuation by Comparables）
	- 常用估值倍数
		-  P/E（市盈率）= Price / Earning
		- EV/EBITDA | 适用于折旧/摊销差异大的公司，可比性强 
		- EV/Sales | 适用于尚未产生正利润的初创企业 
		- EV/EBIT | 类似EV/EBITDA，但包含折旧摊销影响 
		- P/B（市净率） | 适用于银行、保险等资产密集型行业 
- 账面价值、清算价值与托宾Q
	- 账面价值（Book Value）= 总资产 - 总负债
	- 清算价值（Liquidation Value）
	- 重置成本与托宾Q（Tobin's Q）托宾Q = 市场价值 / 重置成本
- 股息贴现模型（Dividend Discount Model, DDM）
	- $V_0 = \frac{D_1}{1+k} + \frac{D_2}{(1+k)^2} + \frac{D_3}{(1+k)^3} + \cdots$.
	- Constant Growth DDM: $V_0 = \frac{D_0(1+g)}{k-g} = \frac{D_1}{k-g}$.
- 股息增长率的估计 $g = ROE \times b$
	- \(ROE\)：净资产收益率
	- \(b\)：留存比率（1 - 股息支付率）
- 增长机会现值（PVGO）: $P_0 = \frac{E_1}{k} + PVGO$
	- \(E_1/k\)：无增长价值（公司将所有盈利作为股息分配时的价值）
	- PVGO：未来投资机会的净现值之和
- 多阶段增长模型
- 市盈率（P/E）分析
	- P/E与PVGO的关系: $\frac{P_0}{E_1} = \frac{1}{k}\left(1 + \frac{PVGO}{E_1/k}\right)$.
	- P/E的决定因素: $\frac{P_0}{E_1} = \frac{1-b}{k - ROE \times b}$
- 自由现金流模型
	- 公司自由现金流（FCFF）
		- $FCFF = EBIT \times (1-t) + 折旧 - 资本性支出 - \Delta NOWC$
		- $FCFF = EBITDA \times (1-t) + 折旧 \times t - 资本性支出 - \Delta NOWC$
		- 公司价值
			- 公司价值 $= \sum_{t=1}^{T} \frac{FCFF_t}{(1+WACC)^t} + \frac{V_T}{(1+WACC)^T}$
			- 其中终值：$V_T = \frac{FCFF_{T+1}}{WACC - g}$
		- WACC = $\frac{D}{D+E} \cdot r_d \cdot (1-t) + \frac{E}{D+E} \cdot k$
	- 股权自由现金流（FCFE）$= FCFF - 利息 \times (1-t) + \Delta 债务$
- 股权价值 = \sum_{t=1}^{T} \frac{FCFE_t}{(1+k_E)^t} + \frac{E_T}{(1+k_E)^T}
- CAPM在估值中的应用与问题
	- $k = r_f + \beta \times [E(r_M) - r_f]$
- CAPM的实证问题：SML过于平坦
- 实际数据拟合的SML比CAPM预测的**平坦得多**
- **低\(\beta\)股票**：CAPM要求的回报率\(r_1\)过低 → 折现不足 → **被高估**
- **高\(\beta\)股票**：CAPM要求的回报率\(r_1\)过高 → 折现过度 → **被低估**


## 9 bonds

$B_{0}=\sum_{t=1}^T \frac{\text{coupon}_{t}}{(1+\text{yield}_{t})^t}+\frac{\text{par value}_{T}}{(1+yield_{T})^T}$ 

- yield to maturity YTM $P_{B}=\sum_{t=1}^T \frac{\text{coupon}_{t}}{(1+r)^t}+\frac{\text{par value}_{T}}{(1+r)^T}$ 

nominal yield: coupon rate

current yield

realized yield: $B_{0}=\frac{FV_{N}}{(1+y)^N}$ 

yield to call: $B_{0}=\sum_{t=1}^{T_{c}} \frac{\text{coupon}_{t}}{(1+y_{C1})^t}+\frac{\text{1st call price}_{T_{c}}}{(1+y_{C1})^{T_{c}}}$ 

short rate v.s. spot rate
![[Pasted image 20260617090137.png|300]]

$(1+f_{n})=\frac{(1+y_{n})^n}{(1+y_{n-1})^{n-1}}$ 

forward rate v.s. expected rate
$1/(1+y_{2})^{2}=1/(1+r_{1})(1+f_{1})<1/(1+r_{1})(1+f_{1})$ liquidity premium

Trading Strategies
If expect fall in interest rate: maximum bond price appreciation for long-maturity, low-coupon bonds
If expect rise in interest rate: maximum protection with short-term, high-coupon bonds

macaulay duration $w_{t}=[CF_{t}/(1+y)^t]/PV,D=\sum_{t=1}^Tt\times w_{t}$ 
modified duration $D^*=D/(1+y)$ 如果是半年，$y$应该是半年的
with price: $\Delta P/P=-D^*\times\Delta y$ 但这里即使是半年付息，也要用年化利率的变化
duration 有线性性

1. duration of zero-coupon bond = maturity
2. given maturity, lower coupon rate, longer duration
3. given coupon rate, longer maturity, longer duration
4. given others, higher YTM, shorter duration
5. duration of perpetuity = (1+y)/y

floating rate note
at reset, price ≈ par value; discount spread may change with time
duration ≈ time to next reset

convexity
$\text{convexity}=\frac{1}{(1+y)^{2}}\sum_{i=1}^n\left[ \frac{\text{CF}_{t}(t^{2}+t)}{(1+y)^tP} \right]$ 
$\Delta P/P=-D\times\Delta y+[\text{convexity}\times(\Delta y)^{2}]/2$ 
same direcation as duration

callable bond
effective duration $\text{effective duration}=\frac{\Delta P/P}{\Delta r}$ 

- passive management
	- indexing
	- immunization
		- match duration and PV of assets and liabilities
		- Price risk and reinvestment rate risk exactly cancel out
		- rebalanceing: when interest rate changes or time goes
	- cash flow matching (seldom used)

- active management
	- swapping strategies
		- Substitution swap
		- Intermarket spread swap
		- Rate anticipation swap
		- Pure yield pickup swap
		- Credit Default Swap


## 10 option

- call
- put
- covered call: stock + write call
- straddle: call + put
- spread: call +  write call

binomial option pricing

hedge ratio = number of shares / number of calls = (Cu-Cd)/(uS0-dS0)
$C_{0}=H\times(S_{0} -PV(dS_{0} ))$ 也可以用中性概率来算
black-scholes call valuation $C_{0}=S_{0}\times N(d_{1})-Xe^{-rT}\times N(d_{2})$ 
black-scholes put valuation $P_{0}=Xe^{-rT}\times [1-N(d_{2})]-S_{0}\times [1-N(d_{1})]$ 
black-scholes call valuation with dividend $C_{0}=S_{0}e^{-\delta T}\times N(d_{1})-Xe^{-rT}\times N(d_{2})$ 
put-call parity: $P=C+Xe^{-rT}-S_{0}$ 
hegde ratio or delta: $\frac{dC_{0}}{dS_{0}}=N(d_{1})$ 


- Profit to long = Spot price at maturity - Original futures price
- Profit to short = Original futures price - Spot price at maturity

## 11 porfolio performance evaluation

- tracking error
	- R-squared
	- Beta
	- with sign tracking error
	- without sign tracking error: absolute difference
- three key elements of active investing
	- alpha forecasting
	- risk management
	- trading cost management
- types of risk
	- total risk
		- sharpe ratio $\text{sharpe ratio}_{p}= \frac{R_{p}-R_{f}}{\sigma_{p}}$ 
		- M^2 Measure $M^{2}=r_{p}-r_{M}$ 先要把收益率调整一致
- risk adjusted performance
	- Jensen's alpha: $R_{p,t}-RF_{t}=\alpha_{p}+\beta_{p}(R_{M,t}-RF_{t})+u_{p,t}$ 
	- Treynor ratio: $\text{Treynor Ratio}_{p}= \frac{R_{p}-R_{f}}{\beta_{p}}$ 
- information ratio
	- $\text{information ratio}=\frac{\alpha_{p}}{\sigma(u_{p})}$ 
	- $R_{p,t}-RF_{t}=\alpha_{p}+\beta_{p}(R_{M,t}-RF_{t})+u_{p,t}$ 
	- Good managers typically have IR of 0.5-1.0


![[Pasted image 20260619145136.png|500]]


