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