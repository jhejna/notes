# 1. Distributions, Data, and Probability

Machine learning begis with data -- it's what we use to make our decisions and develop reasoning about the world. In the field of machine learning, we make the strong assumption that all of our data is derived from a _probability distribution_. It's OK if you don't know what that means yet. This section will be about data and how we interpret it using a probablistic framework. Additionally, it will provide a brief background on probability necesary for topics down the line.

## Probability
Probability is mathematical framework used to understand the randomness. Our world is random, and thus so is our data. Here are a few examples of randomness present in data:

* Ordering of the deck in card games
* The actions of other players or humans, including mistakes in labeling data.
* Quantum level interactions of light that change images.
* noise in sensors

By applying a probablistic framework to our data, we hope to make sense of all these sources of randomness. Let's begin with a simple example, we'll use throughout this note:

!!! example "Ex: Tossing Coins"
    You have a coin, but aren't sure if it is fair or not. We define the coin to be fair if it truly lands on heads 50% of the time. How can we figure out if this is true? By collecting an analyzing data. Every time we flip the coin, there are two things that can happen. The coin can land on heads or it can land on tails. This means our random experiment of flipping the coin has two probablistic outcomes $\omega$, either $\omega = H$ for heads or $\omega = T$ for tails.


In probability, we denote each possible result of a random trial by an outcome, $\omega$ and the set of all possible outcomes as $\Omega$. In our coin flipping example, $\Omega = \{H,T\}$. If the coin is fair, we believe the probability of getting heads, or $P(H) = 0.5$ or 50%. Using this intuition, we can define the basics of a probability space.

**Definition**: A _probability space_ $(\Omega, P)$ is a set of collectively exhaustive outcomes $\Omega$ and a function $P: \Omega \rightarrow \mathbb{R}$ such that the following are true:

1. $P(\omega) \geq 0,  \forall \omega \in \Omega$ or every outcome has some assigned likelihood.
2. $\sum_{\omega \in \Omega} P(\omega) = 1$ or the total probability has to sum to one.

In our coin flipping example, we had two outcomes $H, T$. However, this notion of probability generalizes to more scenarios. Let's say we can collect three data points to figure out if the coin is fair or not. Our new outcomes are defined by the eight possible results, including $HHH, TTT, HTH, THH,$ etc. The probability of each of these outcomes is $0.5 \times 0.5 \times 0.5 = 0.125$ as assuming the coin is fair, each flip has a $0.5$ chance of landing on heads. The probability of any outcome happening is one -- after we flip the coin three times, one of the outcomes in our set must have occured.

## Random Variables
When we're dealing with thousands of data points, dealing with probability spaces becomes rather cumbersome. Imagine that you want to flip the coin 10,000 times to determine whether or not it's fair. The outcome space contain $2^10000$ possible outcomes! We can define abstractions on top of our probability space that makes dealing with large amounts of data or randomness easier. To do so, we define a _random variable_ that converts each outcome to a real value.

**Definition**: A _random variable_ is a function $X: \Omega \rightarrow \mathbb{R}^d$. 

When flipping a coin, we can assign $X = 1$ when the result is heads, or $X = 0$ when the result is tails. Rather than say "heads" or "tails" our data is just zeros and ones. What if we flip the coin 10 times? Well, we collect 10 data points $X_1, X_2, ..., X_{10}$. The total number of heads $Z$ would be $Z = \sum_{i=1}^{10} X_i$, which is just an integer between 0 and 10.

That's great, but what about probabilities? They simply carry over from the sample space:

* $P(H) = P(X=1) = 0.5$. 
* $P(HHHHHHHHHH) = P(Y=10) = (0.5)^{10}$ 


## Distributions
The possible values of a random variable together with the probabilities that it takes on each value defines the _distribution_ of the random variable. This allows us to reason about the characteristics of a random variable.

**Definition**: The _distribution_ of a random variable $X$ is the set of all values the random variable can take on $x \in \mathcal{X}$, and their probabilities $P(X = x)$. Formally, $(x, P(X=x)) \forall x \in \mathcal{X}$.

So far, this has worked great for coin-flipping, as everything has been an integer value! But what if we have a continuous set of outcomes, where the data we collect can take on any value in the interval $[0,1]$. Let's try to assign probabilities. Let's say that $P(0.025) = 0.0000001$, however there are an infinite number of possible outcomes, as there are infinitely many values between zero and one. If each outcome has non-zero probability, the total probability will sum to over 1! If we only assign non-zero probabilities to some outcomes, we reduce back to the discrete case. Thus, each outcome must have zero probability, yet they all sum to 1. To reconcile this disparity, we argue about continuous probability using random variables and distributions.

**Definition** The _distribution_ of a _continuous_ random variable $X$ is defined by a _probability density function_ $f_X : X \rightarrow \mathbb{R}$ such that:

1. $P(a \leq X \leq b) = \int_a^b f_X(x) dx$
2. $\int_{-\infty}^{\infty} f_X(x) dx = 1$

Intuitively, this defintion makes sense. If every value between 0 and 1 is equally likely, the probability $P(X \leq 0.5)$ should be 0.5. If we define f_X to be uniform and equal to 1 over the interval $[0,1]$ this happens to be the case. Note that the values of the density function $f_X(x)$ can be larger than one, and even infinite!

Below are some examples of distributions:

### Expectation

### Variance



## Data in ML
In machine learning, we create a dataset containing samples from the real world.