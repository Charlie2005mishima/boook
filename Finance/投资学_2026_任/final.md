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