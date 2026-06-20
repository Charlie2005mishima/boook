anomaly: empirical results that seem to be inconsistent with maintained theories of asset-pricing behavior
## value effect
- cross section
	- risk-based explanation
		- High BM stocks have with poor earning prospects and bear more distress risks
	- behavior explanation
		- Market wrongly extrapolates high past earnings growth of low BM stocks
		- Over-demand pushes up current prices, which eventually reverse
		- Stronger among stocks with the “mispricing” is harder to be corrected: high transaction costs, low coverage, small firms etc.
- time series
	- Aggregate book-to-market ratio positively predicts market returns
## size effect
- small firms earn higher average returns, especially in January
- small riskier than large firm: high leverage, low production efficiency
- other characteristics: lower analyst coverage, high transaction costs
## return based anomalies
- short-term reversals
- medium term momentum
- long-term reversal
## IVOL
- idiosyncratic volatility IVOL=residuals from FF3 model
- IVOL represents risk that deters arbitrage—amplifies short-sell constraints
	- Among overpriced (underpriced) stocks, the higher IVOL stocks are even more overpriced (underpriced) → negative (positive) future return
![[Pasted image 20260520134913.png|300]]

## Asset/Liabilities based Anomalies
- IPOs and SEOs have poor long-run performance
- Growth in the asset side (asset investment) and claim side (debt and equity issuance) have lower returns
## Accounting Performance based Anomalies
- high accruals earn low returns 应收的
	- net operating income = CF + accruals
- High distress firms (high probability of failure) earn lower returns or FF-3 alpha 大家会低估破产风险
- high gross-margin (profitable) stocks earn higher returns
- High earnings firms have high quarterly stock returns 也会低估好消息

## momentum
- Risk neutral bounded rational informed traders+fully rational uninformed traders
	- Overconfidence by the informed: overestimate the value/precision of their private information—overact to private information
	- Self-attribution bias
![[Pasted image 20260601211731.png|300]]

- One representative investor: Risk-neutral investor with constant discount rate
	- The investor does not realize that earnings is random walk
	- Rather, he thinks that the world moves between two regimes
		- Regime 1: Earnings shocks are likely to be reversed in the following period
		- Regime 2: Shocks are more likely to be followed by another shock of the same sign
	- update belief using one signal
		- $E(r_{t+1}|z_{t}=G)=?$
			- A typical biased investor thinks that he's in a mean-reverting regime (Regime 1)
			- $E(r_{t+1}|z_{t}=G)>E(r_{t+1}|z_{t}=B)$ 短期动量
		- $E(r_{t+1}|z_{t}=G;z_{t-1}=G;\dots;z_{t-i}=G)=?$
			- Biased investor thinks he is in Regime 2
			- $E(r_{t+1}|z_{t}=G;z_{t-1}=G;\dots;z_{t-i}=G)<E(r_{t+1}|z_{t}=B;z_{t-1}=B;\dots;z_{t-i}=B)$ 长期反转
	- , momentum is higher for
		- Firms followed by fewer analysts: Dominated by private information: DHS
		- Growth stocks: Harder to evaluate low B/M stocks
		- Higher information uncertainty stocks