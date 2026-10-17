
## 1.1 Sample Spaces

sample space Ω. 
One such potential outcome $\omega \in \Omega$ is a sample outcome; it is an element of the set Ω. 
We are usually interested in an event that is a subset $A \subseteq \Omega$ of the sample space.

## 1.2 Conditional Probability and Independence

**※【定义】** independent of each other if and only if
$$

\mathbf {P r} (A \mid B) = \mathbf {P r} (A).

$$
Equivalently, by algebraic manipulation, they are independent if and only if $\mathrm{ P r } ( B \mid$ $A ) = { \mathrm{ P r } } ( B )$ and also if and only if $\mathrm{ P r } ( A \cap B ) = \mathrm{ P r } ( A ) \mathrm{ P r } ( B )$ . This implies that knowledge about B has no efect on the probability of A (and vice versa from A to B).

## 1.3 Density Functions

## 1.4 Expected Value

## 1.5 Variance

## 1.6 Joint, Marginal, and Conditional Distributions

## 1.7 Bayes' Rule

### 1.7.1 Model Given Data

In data analysis, $M$ represents a "model" and $D$  data.
Then $\mathrm{ P r } ( M \mid D )$ is interpreted as the probability of model $M$ given that we have observed $D$. 
A maximum aposteriori (or MAP) estimate is the model $M \in \Omega _ { M }$ that maximizes $\mathrm{ P r } ( M \mid D )$ . That is, 
$$

M ^ {*} = \underset {M \in \Omega_ {M}} {\operatorname{argmax}} \mathbf {P r} (M \mid D) = \underset {M \in \Omega_ {M}} {\operatorname{argmax}} \frac {\mathbf {P r} (D \mid M) \mathbf {P r} (M)}{\mathbf {P r} (D)} = \underset {M \in \Omega_ {M}} {\operatorname{argmax}} \mathbf {P r} (D \mid M) \mathbf {P r} (M).

$$
  
*※【例题】*  Gaussian MLE

Let the data D be a set of points in $\mathbb { R } ^ { 1 } : \{ 1 , 3 , 1 2 , 5 , 9 \}$ . 
Let $\Omega _ { M }$ be R so that the model is parametrized by a point $M \in \mathbb { R }$ . 

If we assume that each data point is observed with independent Gaussian noise with $\sigma = 2$ , 
its pdf is described as $\begin{array} { r } { \Re _ { M , 2 } ( x ) = g ( x ) = \frac { \hat { 1 } } { \sqrt { 8 \pi } } \exp ( - \frac { 1 } { 8 } ( M - x ) ^ { 2 } ) } \end{array}$ , then
$$

\mathbf {P r} (D \mid M) = \prod_ {x \in D} g (x) = \prod_ {x \in D} \left(\frac {1}{\sqrt {8 \pi}} \exp (- \frac {1}{8} (M - x) ^ {2})\right).

$$

To find $M ^ { * } = \operatorname { a r g m a x } _ { M } \ \mathrm{ P r } ( D \mid M )$ is equivalent to  $\operatorname{argmax}_ M \ln ( \mathrm{ P r } ( D \mid M ) )$ the log-likelihood which is
$$

\begin{array}{c} \ln (\mathbf {P r} (D \mid M))  & = \ln \left(\prod_ {x \in D} \left(\frac {1}{\sqrt {8 \pi}} \exp (- \frac {1}{8} (M - x) ^ {2})\right)\right) \\  & =  \sum_ {x \in D} \left(- \frac {1}{8} (M - x) ^ {2}\right) + | D | \ln (\frac {1}{\sqrt {8 \pi}}). \end{array}

$$

We can ignore the last term in the sum since it does not depend on M. 
The first term is maximized when $\textstyle \sum _ { x \in D } ( M - x ) ^ { 2 }$ is minimized, which occurs precisely as $\begin{array} { r } { \mathrm { E } [ D ] = \frac { 1 } { | D | } \sum _ { x \in D } x } \end{array}$ , the mean of the data set D. 
That is, the maximum likelihood model is exactly the mean of the data $D _ { ; }$ , and is quite easy to calculate.

## 1.8 Bayesian Inference

Bayesian inference focuses on a simplified version of Bayes's rule:
$$

\mathbf {P r} (M \mid D) \propto \mathbf {P r} (D \mid M) \cdot \mathbf {P r} (M).

$$

*※【例题】* Average Height（后续）

We can construct a data set $D = \{ x _ { 1 } , . . . , x _ { n } \}$ by measuring the height of a set of random students in inches. 
There may be an error in the measurement, and this is an incomplete set, so we do not entirely trust the data.

So we introduce a prior $\pi ( M )$ . 
Consider we read that the average height of a fully grown person is 66 inches, with a standard deviation of $\sigma = 6$ inches. 
So we assume that (Gaussian)
$$

\pi (M) = \mathcal {N} _ {6 6, 6} (M) = \frac {1}{\sqrt {\pi 7 2}} \exp (- (\mu_ {M} - 6 6) ^ {2} / (2 \cdot 6 ^ {2}))

$$

is normally distributed around 66 inches.

### Weight for Prior

*※【例题】* Weighted Prior for Height
Let us continue the example about the height of an average student from our university, 
and assume (as in the MLE example) the data is generated independently from a model M with Gaussian noise with $\sigma = 2$ . 
Thus the likelihood of the model, given the data, is 
$$

f (D \mid M) = \prod_ {x \in D} g (x) = \prod_ {x \in D} \left(\frac {1}{\sqrt {8 \pi}} \exp (- \frac {1}{8} (\mu_ {M} - x) ^ {2})\right).

$$

Now using that the prior of the model is $\begin{array} { r } { \pi ( M ) = \frac { 1 } { \sqrt { \pi 7 2 } } \exp ( - ( \mu _ { M } - 6 6 ) ^ { 2 } / 7 2 ) } \end{array}$ , the posterior is given by
$$

p (M \mid D) \propto f (D \mid M) \cdot \frac {1}{\sqrt {\pi 7 2}} \exp (- (\mu_ {M} - 6 6) ^ {2} / 7 2).

$$
前面一部分是已知 $\mu_{M},\sigma$ 时，$x$ 的概率；后者是 $\mu_{M}$ 的分布。随后log取最大值。

  
