## recap of linear factor models
$E[R_{i}]=a_{\text{mvo}}+b_{\text{mvo}}\beta _{i}\text{,where }\beta _{i}=\frac{\text{Cov}(R_i,R_{\text{mvo}})}{\text{Var}(R_{\text{mvo}})}$ 

## inputs
### risk-free rate, Rf
risk-free rates: Short-term Treasury rates
### beta, beta i
$\beta _{i}=\frac{\text{Cov}(R_i,R_{\text{mvo}})}{\text{Var}(R_{\text{mvo}})}$ 
it can be estimated by $R_{it}^e=\alpha _{i}+\beta _{i}R_{Mt}^e+\varepsilon_{it}$ 
market index (most common): value-weighted portfolio of US-based common stocks in the Center for Research in Security Prices (CRSP) database 
other choices: Periodicity, Length, Minimum number of data point

## portfolio sorting
The general approach is to form portfolios of stocks, where the stocks in each portfolio have different levels of the variable examined, and then to study the future returns of these portfolios.
- steps:
	1. Determine the relevant values of the sorting variable
	2. Determine breakpoints
	3. Portfolio formation
	4. Average portfolio returns
- important parameters
	1. rebalancing frequency
	2. sorting variable
	3. number of portfolios
	4. weighting scheme
- double sorting
	- independent double sorting
	- dependent double sorting
![[Pasted image 20260519113008.png|300]]

## fama-macbeth regression
- step1 时间序列回归: $r_{it}-r_{ft}=\alpha_{i}+\beta_{i1}F_{1t}+\beta_{i2}F_{2t}+\varepsilon _{it}$ 
	- $\beta _{ik}$: 资产i对因子k的暴露 # 资产特定的，与实践无关
	- $F_{kt}$: 因子k在时间t的收益率
- step2 横截面回归: $r_{it}-r_{ft}=\lambda_{0t}+\lambda_{1t}\hat{\beta}_{i1}+\lambda_{2t}\hat{\beta}_{i2}+\varepsilon_{it}$ 
	- $\lambda _{kt}$: 因子k在时间t的风险溢价
	- $\hat{\beta} _{ik}$: 第一步估计的资产i对因子k的暴露
- time-series test
	- important assumption: tradable factors
	- the model applies to all factors: $f_{jt}=\alpha _{j}+\beta_{j1}f_{1t}+\dots+\beta_{jj}f_{jt}+\dots++\beta_{jK}f_{Kt}+\varepsilon _{jt}$ 
	- the coefficient should be: $\beta _{j}=1$ and $\alpha _{j}=\beta _{j1}=\dots=\beta_{jK}=0$ , $\mathbb{E}[f_{j}]=\lambda _{j}$ 
	- taking the expectation: $\mathbb{E}[R_{i}^e]=\alpha _{i}+\beta _{i1}\lambda_{1}+\dots+\beta _{iK}\lambda_{K}$ 
	- steps:
		1. run regression: $r_{it}-r_{ft}=\alpha_{i}+\beta_{i1}F_{1t}+\beta_{i2}F_{2t}+\varepsilon _{it}$ 
		2. The estimate of the factor risk premiums: $\hat{\lambda}_{j}=\hat{\mathbb{E}}[f_{jt}]$
		3. t-test: $\text{H}_{0}:\alpha _{i}=0$ 
		4. jointly zero: $\text{H}_{0}:\alpha_{1}=\alpha_{2}=\dots=\alpha_{N}=0$ 
- cross-sectional test
	- steps:
		1. run: $r_{it}-r_{ft}=\alpha_{i}+\beta_{i1}F_{1t}+\beta_{i2}F_{2t}+\varepsilon _{it}$ 
		2. average returns: $\mathbb{E}[R_{i}^e]=\alpha _{i}+\beta _{i1}\lambda_{1}+\dots+\beta _{iK}\lambda_{K}$ 
		3. 检验每个风险溢价是否显著不为零: $t= \frac{\hat{\lambda}_{j}}{\sqrt{ \text{Var}(\hat{\lambda})_{jj} }}$ 
## size and value portfolio
- size factor
	- proxy the size factor as the return on a zero-cost portfolio that buys 1$ worth of small stocks and short-sells 1$ worth of big stocks
	- $R_{\text{big,t}}-R_{\text{small,t}}=f_{\text{size,t}}\times(\beta_{\text{big}}-\beta_{\text{small}})$ 
- value effect BE/ME
	- value firms: high BE/ME; growth firms: low BE/ME. 
## fama-french model
- $\mathbb{E}[R_{i}^e]=\beta _{iM}\mathbb{E}[R_{M}^e]+\beta _{iSMB}\mathbb{E}[SMB]+\beta _{iHML}\mathbb{E}[HML]$
	- market, size, value
	- $\text{SMB}_{t}=\frac{1}{3}(R_{t,\text{SG}}+R_{t,\text{SN}}+R_{t,\text{SV}})-\frac{1}{3}(R_{t,\text{BG}}+R_{t,\text{BN}}+R_{t,\text{BV}})$ 
	- $\text{HML}_{t}=\frac{1}{2}(R_{t,\text{SV}}+R_{t,\text{BV}})-\frac{1}{2}(R_{t,\text{SG}}+R_{t,\text{BG}})$ 
![[Pasted image 20260519115528.png|300]]
## momentum
- Rebalancing frequency: the sorting of stocks into portfolios is done at the end of every month (monthly rebalancing).
- Sorting variable: at the end of month t, sort stocks based on their returns during month t − 11 to month t − 1 (thus ignoring the return in month t)
## carhart model
- $\mathbb{E}[R_{i}^e]=\beta _{iM}\mathbb{E}[R_{M}^e]+\beta _{iSMB}\mathbb{E}[SMB]+\beta _{iHML}\mathbb{E}[HML]+\beta_{iWML}\mathbb{E}[WML]$ 