## 18 nominal frictions and business cycles
### 18.3 the new keynesian model
#### 18.3.1 environment
家庭效用函数
$$\max\mathbb{E}_{0}\sum_{t=0}^\infty\beta^t\left[ \frac{C_{t}^{1-\sigma}}{1-\sigma} - \frac{L_{t}^{1+\psi}}{1+\psi}\right]\tag{18.1}$$
最终产品生产过程
$$
Y_{t}=\left( \int_{0}^{1} y_{j,t}^{\frac{\varepsilon-1}{\varepsilon}} \, dj  \right)^{\frac{\varepsilon}{\varepsilon-1}}\tag{18.2}
$$
中间产品生产过程
$$
y_{j,t}=A_{t}\mathscr{l}_{j,t} \tag{18.3}
$$
成本最小化过程
$$
\min \int_{0}^{1} P_{j,t}y_{j,t} \, dj,\,\text{ s.t. }Y_{t}=\left( \int_{0}^{1} y_{j,t}^{\frac{\varepsilon-1}{\varepsilon}} \, dj  \right)^{\frac{\varepsilon}{\varepsilon-1}}\tag{18.4}
$$
有(18.4)可以推导得到

$$
P_{t}=\left( \int_{0}^{1} P_{j,t}^{1-\varepsilon} \, dj  \right)^{\frac{1}{1-\varepsilon}}\tag{18.5}
$$

和
$$
y_{j,t}=\left( \frac{P_{j,t}}{P_{t}} \right)^{-\varepsilon}Y_{t}\tag{18.6}
$$

#### 18.3.2 decision problems
decision problem $\max\mathbb{E}_{0}\sum_{t=0}^\infty\beta^t\left[ \frac{C_{t}^{1-\sigma}}{1-\sigma} - \frac{L_{t}^{1+\psi}}{1+\psi}\right]$
budget constraint $B_{t+1}+P_{t}C_{t}=P_{t}\omega_{t}L_{t}+(1+i_{t-1})B_{t}+P_{t}\int m_{j,t}dj\quad\forall\,t$
simplify $b_{t+1}+C_{t}=\omega_{t}L_{t}+\frac{1+i_{t-1}}{1+\pi_{t+1}}b_{t}+\int m_{j,t}dj\quad\forall\,t$
eular equation
$$
C_{t}^{-\sigma}=\beta \mathbb{E}_{t}\left[ \frac{1+i_{t}}{1+\pi _{t+1}}C_{t+1}^{-\sigma} \right]\tag{18.7}
$$
the intratemporal labor supply condition
$$
C_{t}^{-\sigma}\omega _{t}=L_{t}^{\psi}\tag{18.8}
$$
the relative price $p_{j,t+1}=\frac{P_{j,t}}{P_{t+1}}=\frac{p_{j,t}}{1+\pi_{t+1}}$
the real profits of firm j $m_{j,t}=p_{j,t}y_{j,t}-\omega_{t}l_{j,t}=(p_{j,t}-\omega_{t}/A_{t})p_{j,t}^{-\varepsilon}Y_{t}$
the real function of a firm with relative price p (eular equation) is
$$V(p,\mathcal{S})=u'(C(\mathcal{S}))\left( p-\frac{\omega(\mathcal{S})}{A(\mathcal{S})}\right) p^{-\varepsilon}Y(\mathcal{S})+\beta \mathbb{E}\left[ \theta V\left( \frac{p}{1+\pi(\mathcal{S'})} ,\mathcal{S}'\right) +(1-\theta)V(p^{R}(\mathcal{S'}),\mathcal{S}')\right]$$
so we can express the price as $p^{R}(\mathcal{S})=arg\max_{p}V(p,\mathcal{S})$ 
the first-order necessary condition for the choice of the optimal reset price
$$
0=\mathbb{E}_{t}\sum_{\tau=t}^\infty (\beta\theta)^{\tau-t}u'(C_{\tau})Y_{t}\left[ (p_{t,\tau}^R)^{-\varepsilon}-\varepsilon(p_{t,\tau}^R)^{-\varepsilon-1}\left( p_{t,\tau}^R-\frac{w_\tau}{A_\tau} \right) \right]
$$
rearranging, we can get
$$
p^{R}_{t}=\frac{p^{N}_{t}}{p^{D}_{t}}\tag{18.9}
$$
where 
#### 18.3.3 marketing clearing
$$
L_{t}=\int \mathscr{l}_{j,t}dj 
$$
$$
Y_{t}=C_{t}\tag{18.8}
$$
#### 18.3.4 aggregation
$$
P_{t}^{1-\varepsilon}=\int_{j}P_{j,t}^{1-\varepsilon}dj=\int_{j\not\in \mathcal{J}_{t} }P_{j,t}^{1-\varepsilon}dj+\int_{j\in \mathcal{J}_{t}}P_{j,t}^{1-\varepsilon}dj
$$
where
$$
\int_{j\not\in \mathcal{J}_{t} }P_{j,t}^{1-\varepsilon}dj=\theta P_{t-1}^{1-\varepsilon}
$$
$$
\int_{j\in \mathcal{J}_{t}}P_{j,t}^{1-\varepsilon}dj=(1-\theta)(P_{t}^R)^{1-\varepsilon}
$$
$$
\implies P_{t}=[\theta P_{t-1}^{1-\varepsilon}+(1-\theta)(P_{t}^R)^{1-\varepsilon}]^{1/(1-\varepsilon)}
$$
$$
\implies p_{t}^R=\frac{\left[ \frac{P_{t}^{1-\varepsilon}-\theta P_{t-1}^{1-\varepsilon}}{1-\theta} \right]^{1/(1-\varepsilon)}}{}
$$
#### 18.3.4 state variables
#### 18.3.5 flexible-price equilibrium and output gap
the output gap 
$$
x_{t}=\log Y_{t}-\log Y_{t}^n
$$
#### 18.3.6 interest rate rules
$i_{t}=\bar{i}+\phi_{\pi}(\pi_{t}-\pi^*)+\phi_{x}x_{t}$
#### 18.3.7 monetary policy shocks
#### 18.3.8 three-equation model
log-linearized version of the consumption eular equation IS curve
$$
x_{t}=\mathbb{E}_{t}x_{t+1}-\frac{1}{\sigma}(\hat{i}_{t}-\mathbb{E}_{t}\pi _{t+1}-r_{t}^n)\tag{}
$$
the real natual rate of interest
$$
r_{t}^{n}=-\log\beta+ \frac{\sigma(1+\psi)}{\sigma+\psi}(\hat{A}_{t}-\mathbb{E}_{t}\hat{A}_{t+1})
$$
NKPC
$$\pi _{t}=\kappa x_{t}+\beta \mathbb{E}_{t}[\pi_{t+1}]$$

#### 18.3.9 determinary
#### 18.3.10 the role of nominal demand in determining output

$U'(C_{t})=\beta \mathbb{E}_{t}\left[ U'(C_{t+1}) \frac{(1+i_{t})P_{t}}{P_{t+1}} \right]$
对CRRA的具体形式 $C_{t}^{-\sigma}=\beta \mathbb{E}_{t}[C_{t+1}^{-\sigma}\frac{(1+i_{t})P_{t}}{P_{t+1}}]$
### 2.2 推导：对数线性化
等式两边取对数 $-\sigma c_{t}=\log(\beta)+\log \mathbb{E}_{t}[\exp(-\sigma c_{t+1}+\log(1+i_{t})-\pi_{t+1})]$
近似 $-\sigma c_{t}\approx \log(\beta)+\mathbb{E}_{t}[(-\sigma c_{t+1}+i_{t}-\pi_{t+1})]$
定义稳态实际利率 $r=-\log(\beta)$
整理得 $c_{t}\approx \mathbb{E}_{t}c_{t+1}- \frac{1}{\sigma}(i_{t}-\mathbb{E}_{t}\pi_{t+1}-r)$
