## Bond Characteristics
- face value
- coupon rate
- indenture 合约
- adventure v.s. stock
	- Reasonably good returns for bonds
	- Substantially low risk
	- Significant diversification benefits - low correlation between stocks and bonds
- typical bonds
	- U.S. Treasury Bonds
		- Note maturity: 1-10 years
		- Bond maturity: 10-30 years
		- Interest paid *semiannually*
	- Municipal Bonds
	- Corporate Bonds
		- callable bonds; convertible bonds; puttable bonds; floating-rate bonds
	- Sinking fund specifies that a bond must be paid off systematically over its life rather than only at maturity.
	- Secured Vs Unsecured Bonds
	- prefered stock
## Bond Features and Factors of Bond Price
$B_0=\sum_{t=1}^T\frac{\text{coupon}_t}{(1+\text{yield} _t )^t}+\frac{\text{par value} _T}{(1+\text{yield}_T )^T}$ 
## Risks in Bonds
- Default risk
- Interest rate risk / Inflation risk
- Call risk / Reinvestment risk
- Exchange rate risk
- Liquidity risk
## yields
- yield to maturity
- nominal yield: coupon rate
- current yield
- yield to call $B_{0}=C\sum_{i=1}^n (1+y_{C})^{-i}+C_{1}(1+y_{C})^{-n}$ 
- realized yield (ex-post) $\text{FV}_{N}=C\sum_{i=1}^n(1+r_{i})^{N-i}+\text{par}$ $B_{0}= \frac{\text{FV}_{N}}{(1+y)^N}$ 
- determinants of yield
	- i = Rf + inf + RP
- yield curve
- Forward Rates from Observed Rates $(1+y_{n})^n=(1+y_{n-1})^{n-1}(1+f_{n})$
- Forward rate versus Expected rate (spot rate)
	- 1000/(1+y2)2=1000/\[(1+r1)(1+f1)\]<1000/\[(1+r1)(1+ E(r2)\]→ f2>E(r2): liquidity premium
## Term Structure Theories
- Expectations hypothesis: Long-Term rate = geometric mean of current and future Short-Term rates expected over the maturity
- Liquidity preference: the forward rate can be viewed as the sum of the market's expectation of the future short rate plus a potential risk (or liquidity) premium
- Segmented market hypothesis: investors are grouped by maturity segments
## Duration
$w_{t}=[CF_{t}/(1+y)^t]/Price, D=\sum_{t=1}^T t\times w_{t}$
D = Macaulay duration
D* = modified duration
D* = D / (1+y)
dP/P = -D* xx dy
- Rule 1 The duration of a *zero-coupon* bond equals its time to maturity.
- Rule 2  Holding maturity constant, a bond's *duration is higher when the coupon rate is lower*.
- Rule 3  Holding the coupon rate constant, a bond's duration *generally increases with its time to maturity*.
	-  Except for deeply discounted bonds
- Rule 4  Holding other factors constant, the duration of a coupon bond is higher when the bond's yield to maturity is lower.
	- At higher yields, a higher fraction of the total value of the bond lies in its earlier payments
- Rules 5  The duration of a level perpetuity is equal to:  *(1+y) / y*
	- Portfolio duration = weighted average of component duration
![[Pasted image 20260520152227.png|300]]
expected y up>>decrease exposure>>lower D
## Floating Rate Note (FRN)
- Coupon rate= Reference rate + Fixed spread
## Convexity
$\text{convexity}=\frac{1}{(1+y)^{2}}\sum_{t=1}^n\left[ \frac{\text{CF}_{t}}{P(1+y)^t}(t^{2}+t) \right]$ 
$\frac{\Delta P}{P}=-D\times\Delta y+\frac{1}{2}[\text{Convexity}\times(\Delta y)^{2}]$
- Coupon up, convexity down
- Maturity up­, convexity ­up
- YTM up­, convexity down
- $\text{effective duration}=\frac{\Delta P/P}{\Delta r}$ 
- Callable bond could have negetive convexity

## callable bond & putable bond

![[Pasted image 20260527133401.png|300]]
![[Pasted image 20260527133508.png|300]]

## Cash Flows to Whole Mortgage 
![[Pasted image 20260527133829.png|300]]
A：承担提早还钱的风险（当利率低的时候）
C：承担推迟还钱的风险（当利率高的时候）

## passive management
- indexing: 
- immunization: Match the duration and PV of the assets and liabilities
- rebalancing: when interest rate rises, the durations won't match exactly. or when time flies, durations will change as well.



