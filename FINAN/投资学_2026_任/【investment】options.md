## option contract
### call
- Match the duration and PV of the assets and liabilities
- European options: on
- American options: on or before
- payoff $\max[0,\text{MV-EV}]$ 
- X: exercise price
- $S_{T}$: stock price
![[Pasted image 20260527142146.png|300]]
![[Pasted image 20260527142158.png|300]]

### puts
- payoff: $\max[0,\text{EV-MV}]$ 
![[Pasted image 20260527142346.png|300]]
## market & exercise price relationship
- In the Money价内: exercise of the option produces a positive cash flow
	Call:  exercise price < asset price
	Put:  exercise price > asset price
- Out of the Money价外: exercise of the option would not be profitable
	Call:  asset price < exercise price.
	Put: asset price > exercise price.
- At the Money平价: exercise price = asset price

## strategy 
- Strategy A: Invest entirely in stock. 
- Strategy B: Invest entirely in at-the-money call options. 
- Strategy C: Purchase 100 call options for $1,000. Invest your remaining $9,000 in 6-month T-bills, to earn 3% interest.  抑制了下跌的风险，但也让渡了上涨的收益
- protective put strategy: conditional on buying (or owning) stocks, buying put options on a share-for-share basis
- covered calls: write an option with or w/o an offsetting stock position
- straddle: Buy call and put with same exercise price and maturity.
- spreads: A spread is a combination of two or more calls (or puts) on the same stock with
	- Differing exercise prices \[called “money” spread\]
	- Or, times to maturity \[called “time” spread\].
- collars: A collar is an options strategy that brackets the value of a portfolio between two bounds.
- put call parity: $C+\frac{X}{(1+r_{f})^T}=S_{0}+P$ 但尝尝put价格偏高，这是由于做空手段较少导致的

![[Pasted image 20260527143705.png|300]]

![[Pasted image 20260527143547.png|300]]

![[Pasted image 20260527143737.png|300]]

![[Pasted image 20260527143919.png|300]]

## option-like securities
- warrants
- convertible bond
- risky debt

![[Pasted image 20260527151119.png|300]]

![[Pasted image 20260527151314.png|300]]

## option valuation
- Intrinsic value — payoff that could be made if the option was immediately exercised
- Time value — the difference between the option price and the intrinsic value, given immediate expiration
![[Pasted image 20260527151922.png|300]]

### restrictions on option value: call

$C>S_{0}-PV(X)-PV(D)$ 

![[Pasted image 20260527152224.png|300]]

### early exercise 提前行权
- call
	- $C_{t}\geq S_{t}-PV(X)$
	- 只会在要付股利的前一天提前行权
- put
	- 当知道该公司一定要倒闭的行权

### 二叉树定价
- Hedge Ratio: $H= \frac{C_{u}-C_{d}}{uS_{0}-dS_{0}}$
