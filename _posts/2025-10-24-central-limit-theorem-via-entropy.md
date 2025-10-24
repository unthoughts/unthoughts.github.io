---
layout: post
title: "Entropy and the Central Limit Theorem"
date: 2025-10-24
tags: [entropy, central limit theory, Gaussian distribution]
comments: true
mathjax: true
author: IV
---

# Introduction

## A short story

The Central Limit Theorem is a fundamental result in probability that seems magical at first sight: normalized sums tends in distribution to Gaussians. The classical proof analyzes the Fourier transform of associated probability densities (known as characteristic functions). It is not very intuitive to me.

But there is another way...

In 1957 Jaynes published his seminal two part work: *Information Theory and Statistical Mechanics*, conceptually relating statistical mechanics and probabilistic inference. In the first paper he formulates his fundamental *principle of maximum entropy*, elucidating the conceptual role of entropy. The Gaussian distribution has an equally conceptual entropic characterization.

In 1959, Linnik published [An Information-Theoretic Proof of the Central Limit Theorem with the Lindeberg Condition](https://sci-hub.red/10.1137/1104028). Barron and others have continued this line of research, resulting in a deep understanding of information inequalities and also simplifications of Linnik's original argument (see e.g. [Entropy and the Central Limit Theorem](https://www.jstor.org/stable/2244098)). More recent works culminated for example in the elegant [Solution of Shannon's Problem on the Monotonicity of Entropy](https://www.tau.ac.il/~shiri/jams/shannon.pdf) by Artstein, Ball, Barthe, and Naor, which founds the "thermodynamic" intuition. For more recent surveys on information inequalities see e.g. [this survey](https://arxiv.org/pdf/0704.1751) by Rioul and [this paper](https://arxiv.org/pdf/cs/0605047) by Madiman and Barron. This line of work is underlain by rich geometry furnished by the heat equation.

In this post I'll try to provide information-theoretic intuition for CLT that builds on both the principle of maximum relative entropy, and follow up with a proof along the same lines.

While CLT was my original motivation for learning this stuff, there's a beautiful story about the interplay of information and dynamics involving lots of rich geometry. Incidentally here are some ramblings by a [really clueless fellow](https://www.ihes.fr/~gromov/wp-content/uploads/2018/08/structre-serch-entropy-july5-2012.pdf).

## CLT

Let $(X_i)_{i\geq 1}$ be i.i.d and consider its $n$-average $\overline X_n=\frac{X_1+\cdots+X_n}{n}$. Note $\operatorname{Var}\overline X_n=\|\overline X_n-\mu\|^2_2=\frac{\sigma^2}{n}\overset{n\to \infty}{\longrightarrow}0$ - the variance vanishes in the limit. This is a weak version of the law of large numbers. The strong law is the satisfying but still unsurprising assertion that as $n\to \infty$, the $n$-average converges almost surely to the mean $\overline X_n\underset{\mathrm{a.s}}{\overset{n\to \infty}{\longrightarrow}}\mu$. It is a special case of Birkhoff's ergodic theorem relating space and time averages, and we won't pursue it further in this post.

Upon scaling we see $\|\sqrt n(\overline X_n-\mu)\|_2=\sigma$: the standard deviation becomes constant and there is stability at the limit. Thus we arrive at a more delicate question: is there perhaps a limiting distribution?

$$
\frac{\overline X_n-\mu}{\sigma/\sqrt n}=\frac{\overline X_n-\mu}{\|\overline X_n\|_2}\overset{?}{\underset{n\to\infty}{\Longrightarrow}} L.
$$

Let us phrase our question more geometrically:

1. Suppose we scale the real line by the standard deviation of the average $\|\overline X_n\|_2=\sigma/\sqrt n$.
2. Now we look at scaled (!) histograms of the deviation of the $n$-average $\overline X_n$ from the mean $\mu$.
3. *Question.* In the limit $n\to \infty$, do these histograms have a shape?

Incredibly, the answer is yes when the $X_i$ have finite variance. Even more incredibly, the limit distribution is Gaussian, and completely independent of the distribution of the $X_i$.

**Central Limit Theorem.** Let $(X_i)_{i\geq 1}$ be i.i.d with mean $\mu$ and variance $\sigma^2<\infty$. Consider $n$-average $\overline X_n=\frac{X_1+\cdots+X_n}{n}$. As $n\to \infty$, the deviation of the average from the mean *in units of its standard deviation* converges to a standard Gaussian distribution:

$$
\frac{\overline X_n-\mu}{\sigma/\sqrt n}=\frac{\overline X_n-\mu}{\|\overline X_n\|_2}\overset{n\to\infty}{\Longrightarrow} N(0,1).
$$

But... *why?*

*Why* is there a limit distribution? *Why* is it independent of the distribution of the $X_i$?

A first step toward some intuition is to examine the case of $X_i$ Bernoulli, i.e. (potentially biased) coin tosses. The [3Blue1Brown video](https://www.youtube.com/watch?v=zeJD6dqJ5lo&ab_channel=3Blue1Brown) features great Galton board simulations that hint at what's going on: for a ball to end up on the far left/right it must hit a long stream of heads/tails, which is less probable than a mixed stream (that would place it closer to the middle). There are more mixed streams and so the distributions are concentrated near the middle. Sufficiently thorough analysis of this example leads to the limit distribution, and is carried out by Jaynes in his immortal monograph, but we will not pursue it here. Instead, we will shift to the "information PoV".

A second step toward some intuition, which points directly at Gaussians, is founded on the conjunction of two core principles:
1. The Gaussian has a universal characterization in terms of differential entropy: in a formal sense, it has minimal structure relative to Lebesgue measure among all distributions with a fixed variance.
2. The operation of averaging always blurs/smoothes, reducing structure.

Combining, we expect the sequence of normalized averages to gradually lose structure, and converge to a Gaussian in the limit. This is the entropic intuition in a nutshell. Note the normalization requirement and (anticipation of) existence of a limit both follow from the universal characterization of Gaussians and the averaging intuition.

# Entropy

The foreword to John Baez's great book [What is Entropy?](https://arxiv.org/pdf/2409.09232) contains a phenomenal one-line formulation, to which I have only appended the word "expected":

> Entropy is the expected amount of information we don’t know about a situation, which in principle we could learn.

Key takeaways:

1. As an expectation, entropy is computed with respect to some hypothesis/model of nature.
2. Information is learned relative to prior belief. Accurate priors diminish information learned. Thus entropy is always relative. "Absolute" entropy is relative to absence of priors.
3. Our model of nature should be chosen to maximize entropy relative to our prior beliefs. This is the fundamental *principle of maximum relative entropy*. It is underlain by the following intuition: to maximize *unknown* information (which can be learned by observation) is to minimize extraneous *assumptions*.

Despite the emphasis on the relative point of view, we shall nevertheless start with what is, in some sense, absolute entropy: Shannon entropy. It's absolute in the sense that it assume absence of any prior beliefs. Indeed by the maximum entropy principle itself applied to a finite (!) space, the reference distribution realizing a total absence of priors is the uniform distribution. Shannon entropy is defined a bit more generally - with respect to the counting measure on countable spaces - but that is immaterial. It is very interesting however that only finite spaces feature a distribution which models complete absence of priors - no analogue is available on infinite spaces (even countably infinite). Infinity imposes priors!

Formally, we define entropy of a probability measure relative to a reference measure $h(\nu\mid \mu)=\mathbb E_\nu \left[\log \frac{\mathrm d\mu}{\mathrm d\nu} \right]$. When the reference is also a probability measure, $h(\nu\mid \mu)$ is the expected information gained by shifting our model $\nu \rightarrow \mu$. According to the Bayesian interpretation, $\nu$-expectation of information in other distributions should be maximized by $\nu$ itself. Hence entropy relative to other probability distributions should be non-positive, and zero precisely at $\nu$. For convenience we introduce also expected *relative information gain*: $\nu$-expectation of moving $\nu\leftarrow \mu$, just by adding a minus sign.
$$
D(\nu\mid\mu)=-h(\nu\mid \mu)=\mathbb E_\nu \left[\log \frac{\mathrm d\nu}{\mathrm d\mu} \right]
$$
Expected information gain is non-negative and sometimes referred to as Kullback–Leibler divergence. We'll stick to the more informative (...) name.

At any rate we arrive at an alternative formulation of the principle of maximum relative entropy which I find more intuitive: the *principle of minimum expected information gain*.

## Information content

In this section we'll define information content, first for events, then for partitions of a measure space, and finally for measures. This will allow us to define Shannon entropy conceptually, as average/expected information content. But before any machinery:

> Information content is amount of information we don’t know about a situation, which in principle we could learn.

Here's an overview of what follows.

1. The information content of an event $A$ is $\iota(A)=-\log P(A)$. This defines a function $\mathscr F \overset{\iota}{\longrightarrow} [0,\infty]$ on all events.
2. Fix a probability space $(\Omega,\mathscr F,P)$. A partition $\Omega \overset{\pi}{\twoheadrightarrow}\mathscr P$ defines by pushforward a probability space $(\mathscr P,\pi_\ast \mathscr F,\pi_\ast P)$. The measure $\pi_\ast P$ is called the *distribution of the partition*. The information content of the partition is the random variable taking a cell to its information content $\mathscr P\overset{\iota_\mathscr{P}}{\longrightarrow}[0,\infty]$.
3. The information content of a probability space $(\Omega,\mathscr F,\nu)$ is the information content of its partition into singletons.

The third notion is not merely a special case of the second but rather equivalent to it: the information content of a partition $\Omega \overset{\pi}{\twoheadrightarrow}\mathscr P$ equals the information content of its distribution $\nu=\pi_\ast P$. In particular the information content of a partition depends only on its distribution.

### Of an event

Fix a probability space. Suppose you are told an event $A$ has occurred. If you believe the event was unprobable, you are surprised. One of Shannon's great insights was to view surprisal as information content: a surprising/unprobable outcome is more informative than a probable one.

We want to quantify surprisal/information content. Intuitively, it should be a function of probability only, decreasing as probability increases. It should also be additive on independent events.

$$
A\perp\negthickspace \negthickspace \perp B\implies \iota(A\cap B)=\iota (A)+\iota (B). 
$$

Moreover, an almost certain event should have zero surprisal. Thus we are led, following Shannon, to take $\iota (A)=\log\frac{1}{P(A)}=-\log P(A)$. Additivity on independent events follows from the product-to-sum functional equation satisfied by the logarithm. Note information content is a function defined on all events of a probability space $(\Omega,\mathscr F,P)$.
$$
\iota: \mathscr F \overset{P}{\longrightarrow} [0,1]\overset{-\log}{\longrightarrow} [0,\infty].
$$

### Of a distribution

Probable events are less informative than improbable events. The generalization: on average (!), concentrated distributions are less informative than spread-out distributions are informative. On average, you learn less from a biased coin toss than from a fair coin toss.

In this section we will formally define the information content of a discrete distribution. We'll go through partitions to appeal to geometric intuition.

Fix a set $X$. Recall functions from $X$ are in canonical bijection with indexed (!) partitions of $X$, where fibers correspond to cells. A random variable is but a measurable function from a probability space, so it has an associated *indexed* measurable partition. But probability distributions are defined for partitions and do not require indexing. The point is that a partition $\mathscr P$ of $X$ always comes with a canonical function $X\overset{\pi}{\longrightarrow}\mathscr P$ sending a point to the unique cell containing it.

**Definition.** The distribution of a measurable partition $\mathscr P$ of a measure space $(X,\mathscr A,\mu)$ is the pushforward of $\mu$ along the projection onto the space of cells $\Omega \overset{\pi}{\twoheadrightarrow}  \mathscr P$. Note an element of $\pi_\ast\mathscr A$ is a collection of cells with measurable union, and its measure $\pi_\ast \mu$ is just the measure of their union.

In more earthly terms, the distribution is the multiset of cell masses (multiset because different cells can have equal mass). Upon indexing/labeling a partition we arrive at the common visualization of distributions as histograms, where the $x$-axis ticks are labels of cells, the $y$-axis denotes probability, and the rectangle areas sum to the total measure of the space. Partitions $\mathscr P_i$ of measure spaces $(X_i,\mathscr A_i,\mu_i)$ have isomorphic distributions precisely iff the induced measure spaces are isomorphic $(\mathscr P_i,\pi_{i\ast}\mathscr A_i,\pi_{i\ast}\mu_i)$, iff the restrictions $\mathscr P_i\overset{\mu\;\mid_{\mathscr{P_i}}}{\longrightarrow}[0,\infty]$ are isomorphic iff they have equals multisets of cell masses. This means both partitions can be represented by the same histogram.

**Remark.** The astute reader may have noticed the restriction of the measure to partition cells is actually the Radon-Nikodym derivative of the pushforward with respect to the counting measure on $\mathscr P$, i.e. $\mu \;\mid_\mathscr{P}=\frac{\mathrm d\pi_\ast \mu}{\mathrm d \text{\#}}$. While every measure is absolutely continuous w.r.t the counting measure, the derivative only exists when the atomic support of $\pi_\ast \mu$ is countable. This observation, and the key role played by the counting measure, will be explored later.

**Definition.** Fix a probability space $(\Omega,\mathscr F,P)$. The information content of a partition $\Omega \overset{\pi}{\twoheadrightarrow} \mathscr P$ is the random variable on $(\mathscr P,\pi_\ast \mathscr F,\pi_\ast P)$ given by information content of the cells:

$$
\mathscr P \overset{P\;\mid_\mathscr{P}}{\longrightarrow}[0,1]\overset{-\log}{\longrightarrow} [0,\infty],\quad \iota (A)=-\log P(A).
$$

Note $\log$ is strictly monotone whence injective, so the fibers of information content are precisely equiprobable collections of cells. Hence, the distribution of information content is precisely the distribution of $\mathscr P \overset{P\;\mid_\mathscr{P}}{\longrightarrow}[0,1]$, which we may compute from probability of cells in $\mathscr P$. Geometrically, we start with the histogram for $\mathscr P$ and merge equiprobable columns. Evidently, partitions with more equiprobable cells have more concentrated information distribution. Let's see some examples:

1. A fair coin toss has just two measurements, both equiprobable, so its distribution is $(\tfrac 12, \tfrac 12)$. Hence the information distribution is $\mathbf 1_{\tfrac 12}$.
2. Consider a biased five-sided die with distribution $(0.1,0.1,0.1,0.1,0.6)$. The information distribution is $$0.4\cdot \mathbf{1}_{0.1}+0.6\cdot \mathbf{1}_{0.6}$$.
3. Consider a four-ticket lottery with distribution $(0.1,0.2,0.2,0.5)$. The information distribution is $$0.1 \mathbf  \cdot 1_{ 0.1}+0.4\cdot \mathbf 1_{0.2}+0.5\cdot \mathbf 1_{0.5}$$.
4. Consider $(0.2,0.2,0.3,0.15,0.15)$. The information distribution is $$0.4\cdot \mathbf 1_{0.2}+0.3\cdot \mathbf 1_{0.3}+0.3 \cdot \mathbf 1_{0.15}$$.

### Of a measure

Every set has a canonical partition into its constituent singletons. The structure map $X\overset{x\mapsto \lbrace x\rbrace}{\longrightarrow} \lbrace \lbrace x\rbrace:x\in X\rbrace $ is canonically isomorphic to the identity by omitting curly braces. Hence the information content of a probability space $(\Omega,\mathscr F,P)$ is the random variable given by information content of points. As previously remarked, if $P$ has countable atomic support then the density function $\omega\mapsto P\lbrace \omega \rbrace$ is none other than the Radon-Nikodym derivative with respect to counting measure $\frac{\mathrm dP}{\mathrm d\text{\#}}$:

**Definition/Lemma.** The information content of a probability measure $P$ equals the composite
$$
\Omega \overset{\frac{\mathrm dP}{\mathrm d\text{#}}}{\longrightarrow}[0,1]\overset{-\log}{\longrightarrow} [0,\infty].
$$

Fix a partition $\Omega\overset{\pi}{\twoheadrightarrow}\mathscr P$. The information content of $(\mathscr P,\pi_\ast \mathscr F,\pi_\ast P)$ is then $A\mapsto -\log P(A)$, equal to the definition of the information content of the partition $\mathscr P$. Moreover, we observe that the restriction of $P$ to the partition is itself the Radon-Nikodym derivative of its pushforward with respect to counting measure.

$$
\mathscr P \overset{P\;\mid_\mathscr{P}=\frac{\mathrm d\pi_\ast P}{\mathrm d\text{#}}}{\longrightarrow}[0,1]\overset{-\log}{\longrightarrow} [0,\infty].
$$

Thus the information content of a partition $\mathscr P$ of a probability space $(\Omega,\mathscr F,P)$ equals the information content of the probability space $(\mathscr P,\pi_\ast \mathscr F,\pi_\ast P)$.

⚠️ **Remark.** The explicit appearance of counting measure suggests our definition misses out on the continuous part of a measure. Indeed, the information content of a continuous measure is infinite: points have zero probability and are therefore infinitely surprising events. This can be remedied by considering information content *relative to other measures*, namely more continuous ones. We shall explore this after defining Shannon entropy.

## Shannon entropy

Shannon entropy is average information content. More precisely, it is average information content *relative to the counting measure*.

For a countable measurable space $(\Omega,\mathscr F)$ we define the Shannon entropy of a probability measure $\nu$ as follows:

$$\begin{aligned}
h(\nu\mid \text{#}) &=- \mathbb E \left[ \log \frac{\mathrm d\nu}{\mathrm d\text{#}} \right] \\
& =-\int_\Omega \frac{\mathrm d\nu}{\mathrm d\text{#}} \log \frac{\mathrm d\nu}{\mathrm d\text{#}} \mathrm d\text{#} \\
&= -\sum_\omega \nu \lbrace \omega \rbrace \log \nu\lbrace \omega\rbrace.
\end{aligned}$$

For an $S$-valued random variable $X$ we therefore have the famous formula

$$\begin{aligned}
h(X\mid \text{#}) &=-\int_S \frac{\mathrm dP_X}{\mathrm d\text{#}} \log \frac{\mathrm dP_X}{\mathrm d\text{#}} \mathrm d\text{#} \\
&= -\sum_{x\in S} P\lbrace X=x\rbrace \log P\lbrace X=x\rbrace
\end{aligned}.$$

**Remark.** The determined reader may try to avoid countability assumptions by using the function $P_\mathscr{P}$ without assuming it realizes the derivative $\frac{\mathrm d\nu}{\mathrm d \text{\#}}$. This does allow for a general definition, but it suffers from the same issue: continuous random variables would still have infinite information content whence infinite Shannon entropy.

We previously remarked that partitions with many equiprobable sets have more concentrated information distributions. Nevertheless, concentration of the information distribution holds no bearing on entropy.

**Example.** Let $X\sim \operatorname{Bern}(1)$ be a "one-sided coin" and $Z\sim \operatorname{Bern}(\frac 12)$. These distributions have equally concentrated information contents, both constant functions. However, the values are different: $X$ has zero bits of information while $Z$ has one bit. Any coin toss $Y\sim \operatorname{Bern}(p)$ with $0<p<\frac 12$ has entropy strictly between zero and one bits.

There is ample literature concerning characterizations of Shannon entropy from first principles, similar to our hasty "derivation" of information content. See e.g. Leinster's book [Entropy and Diversity](https://arxiv.org/abs/2012.02113). We'll avoid these and focus on intuition.

## Relative information content

The information content of a probability measure $\nu$ relative to the counting measure is $\Omega \overset{\frac{\mathrm d\nu}{\mathrm d\text{\#}}}{\longrightarrow}[0,1]\overset{-\log}{\longrightarrow} [0,\infty]$, acting by $\omega\mapsto -\log \nu\lbrace \omega\rbrace $. It captures the information content of the points of $\Omega$.

The presence of the counting measure suggests relativizing with respect to other measures $\nu \ll \mu$. In this section we will try to clarify the epistemology in two morally distinct cases:

1. $\mu$ is a "microstate-counter" measure, e.g. counting, Lebesgue, Haar, Riemann, Liouville, and weighted variants.
2. $\mu$ is a probability measure, serving as an assumed prior model of reality.

Our objective is to provide some intuition for the upcoming notions of relative entropy and expected information gain.

First, the mental model. A measurable space $(\Omega,\mathscr F)$ consists of *microstates*. We can observe random variables, i.e. partitions of $\Omega$, whose cells we refer to as *macrostates*.

1. A "microstate counter" measures how many microstates exist within a region.
2. A probability measures belief that reality is in a particular region of microstates.

Let's go over each.

### Relative to a microstate-counter

Consider the set $\lbrace 1, \lbrace 2,3\rbrace \rbrace$. It has just two points, but one of them feels a little fat: it really consists of two points. What is the natural counting measure to assign this set? Is it just the counting measure, which gives equal weight to both? Or should the fat point receive twice the weight? In a nutshell, this is the dilemma underlying the choice of a microstate-counter measure.

Often we assume the microstate space $\Omega$ is "fully refined" in the sense that its points are the most primitive microstates of our system. Thus it is typical for the microstate-counters to be "uniform" measures such as the counting measure, Lebesgue measure, Haar measure, etc.

So when do we need to add weights for observing macrostates?
* If we have the distribution of the random variable, just use it.
* If we want our macro distribution to honor the ambient micro priors, we need to add weights so the macro distribution is a suitable pushforward of the microstate prior $\mu_\mathrm{ma}=w_\ast\mu_\mathrm{mi}$.
* If our model involves only macrostates without finer microstate information, just use the "uniform" measure (counting, Lebesgue, Haar, etc.).

**Example: colored dice.** Consider a fair die whose faces are colored black, gray, and white. $\Omega$ consists of the six equiprobable microstates.
* If there are two faces of each color, the information content gets an additive factor of $\log 2$: just an offset.
* If faces are colored one black, two gray, three white, then the weights should explicitly reflect this, to preserve the extra information probabilities and avoid an (incorrect) uniform distribution on the colors.


Evidently $-\log \frac{\mathrm d\nu}{c\cdot\mathrm d\text{\#}}=-\log \frac{\mathrm d\nu}{\mathrm d\text{\#}}+\log c$. This function increases with $c$. For positive integer $c$ it's always a positive quantity, which represents precisely the information that would have been lost upon coarse-graining by identifying the $c$ microstates comprising each macrostate. A similar but more delicate phenomenon occurs with non-uniform weights.

The case of a finite microstate-counting measure can be normalized to a probability measure, causing just a constant offset. For an infinite microstate-counting measure e.g. counting on infinite spaces or Lebesgue on Euclidean space, normalization is unavailable.

Information content relative to continuous microstate-counting measures can be negative. This counter-intuitive fact may seem disconcerting at first, so let us clarify.

Firstly note $-\log \frac{\mathrm d\nu}{\mathrm d\mu}<0\iff \frac{\mathrm d\nu}{\mathrm d\mu}>1$ so the whole discussion is about densities take on values greater than one. 

In the discrete case it's natural to compute entropy relative to a microstate-counter that takes positive integral values. Hence the quotient $\frac{\mathrm d\nu}{\mathrm d\mu}(\omega)=\frac{\nu\lbrace \omega \rbrace}{\mu \lbrace \omega\rbrace}\leq \nu \lbrace \omega\rbrace \leq 1$ always takes values in the unit interval. In such cases, information content is always non-negative. Note we could artificially take $\frac{\mathrm d\nu}{\varepsilon\cdot \mathrm d\text{\#}}$ for some tiny $\varepsilon \ll1$ and find ourselves with negative information content, but this is very contrived.

In the continuous case, this without contrivance because densities naturally take values higher than one. Our simplest example involves a probability distribution $\nu$ over the real lines and its density relative to Lebesgue measure. If $\nu$ is highly concentrated in a small region then the density $\frac{\mathrm d\nu}{\mathrm d\lambda}$  relative to Lebesgue measure within this region will be higher than one. For instance consider the uniform distribution on an interval $[a,b]$. The density is $\frac{\mathrm d\nu}{\mathrm d\lambda}=\frac{1}{b-a}\mathbf 1_{[a,b]}$, which blows up for small intervals. For another example consider a very concentrated (low variance) Gaussian: the peak of the bell blows up as it narrows down, to preserve unit integral.

**Sign meaning relative to a microstate-counter.**
* Positive - happens iff $\frac{\mathrm d\nu}{\mathrm d\mu}(\omega)<1$, iff microstate lies in a low-density region.
* Negative - happens iff $\frac{\mathrm d\nu}{\mathrm d\mu}(\omega)>1$, iff microstate lies in a low-density region.

Generally speaking, we can weigh our microstate-counting measure to suit our positivity needs; there is no epistemological meaning to unravel here (as far as I can see at least).

### Relative to a prior: information gain

Suppose we have probability distributions $\nu,\mu$ on the same measurable space $(\Omega,\mathscr F)$, modelling our beliefs. For each event we observe, we want to compare both models' predictions. For an event we consider the ratio of probabilities $0\leq \frac{\nu A}{\mu A}\leq \infty$. Note $\frac{\nu A}{\mu A}>1$ means $\nu$ predicted $A$ better than $\mu$, while the reverse inequality $\frac{\nu A}{\mu A}<1$ means the reverse.

⚠️ Sign conventions result in a slight headache, but the bottom line is this: when both measures are probability distributions, we will only use the notion of information gain, whose expectation is *minus* entropy. Hence we define:

**Definition.** The information gain of $\nu \ll \mu$ (read $\nu$ relative to $\mu$ or $\nu$ over $\mu$) is defined as $\log \frac{\mathrm d\nu}{\mathrm d\mu}$.

**Sign meaning relative to a prior.**
* Positive - happens iff $\frac{\mathrm d\nu}{\mathrm d\mu}(\omega)>1$, iff $\nu$ is a superior predictor to $\mu$ at $\omega$.
* Negative - happens iff $\frac{\mathrm d\nu}{\mathrm d\mu}(\omega)<1$, iff $\nu$ is an inferior predictor to $\mu$ at $\omega$.

Note the sign of information gain may fluctuate: either model can get lucky sometimes. The simplest example is perhaps the most illustrative: a fair coin and a very biased coin. Taking $\nu$ to be biased toward heads $\nu\lbrace\mathrm H\rbrace=(1-\varepsilon)$ vs tails $\nu\lbrace\mathrm T\rbrace=\varepsilon$, we find:
* $\log \frac{\nu\lbrace\mathrm H\rbrace}{\mu\lbrace\mathrm H\rbrace}=\log 2(1-\varepsilon)\approx \log 2>0$, witnessing the heads-biased distribution as a better predictor of heads (obviously).
* For tails we have $\approx \log \varepsilon \overset{\varepsilon\to 0}{\longrightarrow}-\infty$, witnessing $\nu$ as a worse predictor of tails.

The interesting question is: what is the information gain expected by $\nu$ and by $\mu$? We will answer this below.

## Relative entropy

### Entropy of a measure relative to another measure; expected information gain

Fix measures $\mu,\nu$. Recall $\nu$ is *absolutely continuous* w.r.t $\mu$ (denoted $\nu \ll \mu$) if $\nu A=0\impliedby \mu A=0$. When $\mu$ is $\sigma$-finite this is equivalent to existence of the Radon-Nikodym derivative $\frac{\mathrm d\nu}{\mathrm d\mu}$.

**Definition.** Let $\nu\ll \mu$. The entropy of $\nu$ relative to $\mu$ is minus the $\nu$-expectation of the log density.

$$
h(\nu \mid \mu)=\begin{cases}\displaystyle -\int\log \frac{\mathrm d\nu}{\mathrm d\mu}\mathrm d\nu =- \int \frac{\mathrm d\nu}{\mathrm d\mu} \log \frac{\mathrm d\nu}{\mathrm d\mu}\mathrm d\mu & \nu \ll \mu \\
\displaystyle \int\log \frac{\mathrm d\mu}{\mathrm d\nu}\mathrm d\nu & \nu\gg \mu \\
+\infty & \text{otherwise}
\end{cases}
$$

When $\nu,\mu$ are both probability measures, define the *expected information gain of $\nu$ relative to $\mu$* as minus the relative entropy. In the literature this quantity is often refered to as Kullback–Leibler divergence, or KL divergence in short. We'll stick to the more informative name.

$$
D(\nu\mid\mu)=-h(\nu\mid\mu)=\int\log\frac{\mathrm d\nu}{\mathrm d\mu}\mathrm d\nu
$$

⚠️ **Remark.** Our sign convention for relative entropy differs from some sources in the literature. Our choice makes it so that relative entropy with respect to counting/Lebesgue measure recovers Shannon/differential entropy respectively. Thankfully, our definition of KL divergence is standard across the literature.

If $\nu,\mu$ are mutually absolutely continuous then they have reciprocal Radon-Nikodym derivatives, so the definition is consistent.

Perhaps the most familiar formula for entropy is $h(\nu\mid \mu)=-\int \frac{\mathrm d\nu}{\mathrm d\mu} \log \frac{\mathrm d\nu}{\mathrm d\mu}\mathrm d\mu$. Typically, $\nu$ is a probability measure. When $\mu=\text{\#}$ is counting measure then $\frac{\mathrm d\nu}{\mathrm d\text{\#}}(\omega)=\nu \lbrace \omega \rbrace$ is density and we recover Shannon entropy

$$\begin{aligned}
h(\nu\mid \text{#}) &=-\int \nu \lbrace \omega \rbrace \log \nu \lbrace \omega \rbrace \mathrm d\text{#} \\
&= -\sum \nu \lbrace \omega \rbrace \log \nu\lbrace \omega\rbrace.
\end{aligned}$$

Now back to some epistemology. The key one-liner:

> Suppose you believe in a model $\nu$. The quantity $D(\nu \mid \mu)$ is your expectation of information gained by following your model $\nu$ instead of an alternative model $\mu$.

* The log density $\log \frac{\mathrm d\nu}{\mathrm d\mu}(\omega)$ is positive precisely when $\nu$ is a superior predictor of $\omega$ compared to $\mu$.
* The Bayesian interpretation of expectation with respect to $\nu$ is the prediction assuming the world truly follows $\nu$.
* If the world truly follows $\nu$, then $\nu$ should be the unique best predictor among all distributions.

Combining these, we anticipate expected information gain $D(\nu\mid \mu)$ to be non-negative, since it is precisely the $\nu$-expectation of the log density $\log \frac{\mathrm d\nu}{\mathrm d\mu}$. This is indeed so.

**Non-negativity.** Fix probability (!) measures $\nu\ll\mu$. Then $D(\nu\mid \mu)\geq 0$ with equality iff $\nu=\mu$.

*Proof.* Recall

$$
D(\nu\mid\mu)= \int \frac{\mathrm d\nu}{\mathrm d\mu} \log \frac{\mathrm d\nu}{\mathrm d\mu}\mathrm d\mu.
$$

The function $\varphi(x)=x\log x$ is convex for $x>0$ by the second derivative test. $\mu$ is a probability measure so Jensen's inequality facilitates the following computation, where the penultimate equality is due to $\nu$ being a probability measure.

$$\begin{aligned}
\int \varphi \left( \frac{\mathrm d\nu}{\mathrm d\mu} \right) \mathrm d\mu & \geq \varphi \int \frac{\mathrm d\nu}{\mathrm d\mu}\mathrm d\mu \\
&= \varphi \int \mathrm d\nu = \varphi(1)=0.
\end{aligned}$$

**Intuition: heavy penalty for underestimation.** Before taking another step, let us present the simplest and most illuminating example of expected information gain and its asymmetry: a biased coin $\nu$ and a fair coin $\mu$.

* Biased expectation of information gain by moving from "neutral" (fair coin) to biased attains moderate values because $x\log x\overset{x\to 0}{\longrightarrow}0$, causing low probability estimates "cancel out".

$$\begin{aligned}
D(\nu \mid \mu) &=\nu \lbrace \mathrm H \rbrace \log \frac{\nu\lbrace \mathrm H \rbrace}{\mu\lbrace \mathrm H \rbrace}+\nu \lbrace \mathrm T \rbrace \log \frac{\nu\lbrace \mathrm T \rbrace}{\mu\lbrace \mathrm T \rbrace} \\
& = \underbrace{(1-\varepsilon)\log 2(1-\varepsilon)}_{\nearrow \log 2}+ \underbrace{\varepsilon \log 2\varepsilon}_{\nearrow 0}.
\end{aligned}$$

* On the other hand, neutral expectation of information gain by moving from a biased belief to a neutral one is unbounded, because the neutral expectation weights are negligible compared to the huge logarithm for improbable events.

$$\begin{aligned}
D(\mu \mid \nu) &=\mu \lbrace \mathrm H \rbrace \log \frac{\mu\lbrace \mathrm H \rbrace}{\nu\lbrace \mathrm H \rbrace}+\mu \lbrace \mathrm T \rbrace \log \frac{\mu\lbrace \mathrm T \rbrace}{\nu\lbrace \mathrm T \rbrace}\\
& = \underbrace{\frac 12 \log \frac{1}{2(1-\varepsilon)}}_{\searrow \tfrac 12\log\tfrac 12}+\underbrace{\frac 12 \log \frac{1}{2\varepsilon}}_{\nearrow \infty}.
\end{aligned}$$

> $D(\nu\mid\mu)$ explodes when the prior $\mu$ severely underestimates probabilities compared to $\nu$.

The dependence of $D(\nu\mid\mu)$ on $\nu,\mu$ is of interest and we will study it (partly) later on.

### The principle of minimum expected information gain (maximum relative entropy)

In 1957 Jaynes published his seminal, two part work *Information Theory and Statistical Mechanics*, relating statistical mechanics and probabilistic inference. In the first paper he formulates his *principle of maximum entropy*: given some prior observations about moments (expectation, variance, etc.), we should model nature with the distribution that maximizes Shannon entropy subject to our data. Intuitively, principles like Occam's razor suggest our model should minimize unjustified assumptions; Jaynes alludes to an equivalence between minizing assumptions and maximizing *unknown* information which can be learned by observation.

One is naturally led to relativize this principle with respect to a prior *model*, given by a reference *probability distribution* $\mu$. Here mild care is needed with signs. Shannon entropy and entropy relative to the uniform distribution differ by a constant, so they reach maximum jointly.

$$
h(\nu \mid U_n)=h(\nu\mid \text{#}_n)-\log n
$$

More generally, we are maximizing the non-positive $h(\nu\mid \mu)\leq 0$.

Thus we are lead to an alternative formulation that I personally find more intuitive: the *principle of minimal expected information gain*. Given a prior model $\mu$ and some observed constraints (e.g. moments), we should choose a model $\nu$ that minimizes expected information gain $D(\nu\mid \mu)$ relative to $\mu$. Intuitively, minimizing expected information gain amounts to minimizing extraneous assumptions.

### Differential entropy and the role of the Gaussian

Entropy relative to Lebesgue measure $\lambda$ is called *differential entropy*. If $X$ has density $\frac{\mathrm dP_X}{\mathrm d\lambda}=f$ then its differential entropy is given by the familiar formula $h(P_X\mid \lambda)=-\int f \log f \mathrm d\lambda$.

**Differential entropy of Gaussian density.** The differential entropy of the Gaussian density
$$
\Gamma_{\mu,\sigma^2}=\frac{1}{\sqrt{2\pi} \sigma}\exp \left( -\frac 12\left( \frac{x-\mu}{\sigma} \right)^2 \right)
$$

is independent of mean $\mu$ and equals

$$
h(N(\mu,\sigma^2)\mid \lambda)=\tfrac 12(1+\log(2\pi\sigma^2)).
$$

**Remark.** Observe the entropy of a Gaussian is negative for sufficiently small $\sigma$. This is fine - see the discussion under information content relative to a microstate-counting measure.

*Proof.* This is a computation. The last equality is due to the general formula $\operatorname{Var}X=\int (x-\mu)^2f_X\mathrm dx$.

$$\begin{aligned}
h(N(\mu,\sigma^2)\mid \lambda) & = -\int \Gamma_{\mu,\sigma^2}\log \Gamma_{\mu,\sigma^2}\mathrm dx \\
&= -\int\Gamma_{\mu,\sigma^2} \left(-\log(\sqrt{2\pi}\sigma)-\frac 12 \left(\frac{x-\mu}{\sigma} \right)^2 \right)\mathrm dx \\
& = \log(\sqrt{2\pi}\sigma)\int \Gamma_{\mu,\sigma^2}\mathrm dx+\frac 12\frac 1{\sigma^2}\int(x-\mu)^2\Gamma_{\mu,\sigma^2}\mathrm dx \\
& =\frac 12\log(2\pi\sigma^2)+\frac 12\frac 1{\sigma^2}\sigma^2.
\end{aligned}$$

**Entropy relative to matching Gaussian.** Write $\Gamma_{\mu,\sigma^2}$ for the probability density of the Gaussian distribution $N(\mu,\sigma^2)$. For any probability density $f$ with matching mean $\mu$ and variance $\sigma^2$, the relative entropy w.r.t the Gaussian is the difference of entropies (w.r.t Lebesgue measure).

$$\begin{aligned}
h(f\mid N(\mu,\sigma^2)) & =h(f\mid \lambda)-h(N(\mu,\sigma^2)\mid \lambda) \\
&= h(f\mid \lambda) - \tfrac 12(1+\log(2\pi\sigma^2)).
\end{aligned}$$

*Proof.* This is just a computation. We use the formula $h(\nu\mid\mu)=\int \log \frac{\mathrm d\mu}{\mathrm d\nu} \mathrm d\nu$.

$$\begin{aligned}
h(f\mid N(\mu,\sigma^2)) &= \int \log(\Gamma_{\mu,\sigma^2}/f) f\mathrm dx \\
&= \int f \log\Gamma_{\mu,\sigma^2}\mathrm dx-\int f\log f\mathrm dx \\
& = h(f\mid \lambda)+\int f \left(-\log(\sqrt{2\pi}\sigma)-\frac 12 \left(\frac{x-\mu}{\sigma} \right)^2 \right)\mathrm dx \\
& = h(f\mid \lambda) -\log(\sqrt{2\pi}\sigma) \int f\mathrm dx-\frac 12\frac 1{\sigma^2}\int (x-\mu)^2f\mathrm dx \\ 
& = h(f\mid \lambda)-\frac 12\log(2\pi\sigma^2)-\frac 12 \frac{\operatorname{Var}X}{\sigma^2}.
\end{aligned}$$

The fact $\operatorname{Var}X=\int(x-\mu)^2f\mathrm dx$ uses $\mu=\mathbb EX$. Finish by applying $\operatorname{Var}X=\sigma^2$.

We can now use non-negativity of expected information gain to deduce Gaussian density maximizes differential entropy per fixed variance.

**Gaussian maximizes differential entropy per fixed variance.** If $f$ is a probability density on $\mathbb R$ with variance $\sigma^2$, its entropy (relative to Lebesgue measure) is at most that of the Gaussian $N(0,\sigma^2)$, with equality iff $f$ is itself Gaussian with variance $\sigma^2$ (the mean doesn't matter).

*Proof.* Let $\mathbb EX=\mu$. Combining the above proposition with $h(\nu\mid\mu)\leq 0$ we find $h(f\mid \lambda)\leq h(N(\mu,\sigma^2) \mid \lambda)$, as desired.

This result is of paramount epistemic importance: it means Gaussian distributions satisfy the principle of maximum differential entropy given prior belief that variance equals $\sigma ^2$. More geometrically, the Gaussian of variance $\sigma^2$ is the most "spread out" of all distributions with variance $\sigma ^2$. Geometric intuition should also make it clear why the maximum only makes sense upon fixing variance: you can spread out more as you increase it (and indeed the differential entropy of the Gaussian increases monotonically in $\sigma^2$).

I attach the following excerpt from Jaynes for good measure.

>  For example, today most accurate experiments in physics take data electronically, and a physicist usually knows the mean square error of those measurements because it is related to the temperature by the well known Nyquist thermal fluctuation law. But he seldom knows any other property of the noise. If he assigns the first two moments of a noise probability distribution to agree with such information, but has no further information and therefore imposes no further constraints, then a Gaussian distribution fit to those moments will, according to the principle of maximum entropy as discussed in Chapter 11, represent most honestly his state of knowledge about the noise.
>
> But we must stress a point of logic concerning this. It represents most honestly the physicist’s state of knowledge about the particular samples of noise for which he had data. This never includes the noise in the measurement which he is about to make! If we suppose that knowledge about some past samples of noise applies also to the specific sample of noise that we are about to encounter, then we are making an inductive inference that might or might not be justified; and honesty requires that we recognize this. Then past noise samples are relevant for predicting future noise only through those aspects that we believe should be reproducible in the future.
>
> In practice, common sense usually tells us that any observed fine details of past noise are irrelevant for predicting fine details of future noise, but that coarser features, such as past mean square values, may be expected reasonably to persist, and thus be relevant for predicting future mean square values. Then our probability assignment for future noise should make use only of those coarse features of past noise which we believe to have this persistence. That is, it should have maximum entropy subject to the constraints of the coarse features that we retain because we expect them to be reproducible. Probability theory becomes a much more powerful reasoning tool when guided by a little common sense judgment of this kind about the real world, as expressed in our choice of a model and assignment of prior probabilities.
>
> Thus we shall find in studying maximum entropy below that, when we use a Gaussian sampling distribution for the noise, we are in effect telling the robot: ‘The only thing I know about the noise is its first two moments, so please take that into account in assigning your probability distribution, but be careful not to assume anything else about the noise.’

Recall the (local, not expected) information gain $\log\frac{\mathrm d\nu}{\mathrm d\mu}$. When $\mu=\lambda$ is Lebesgue measure we call this differential information gain.

**Gaussian iff affine differential information gain.** The differential information gain of a Gaussian density $\Gamma_{\mu,\sigma^2}=\frac{1}{\sqrt{2\pi} \sigma}\exp \left( -\frac 12\left( \frac{x-\mu}{\sigma} \right)^2 \right)$ is $\log^\prime(\Gamma_{\mu,\sigma^2})(x)=-\frac{x-\mu}{\sigma^2}$. Conversely, a density with affine differential gain $\rho(x)=ax+b$ is Gaussian $N(-\frac ba,-\frac 1a)$ (in particular $a<0$).

*Proof.* Given a Gaussian, just compute. Move the constant $\frac 1{\sqrt {2\pi}\sigma}$ into the exponent. Upon taking $\log$ it disappears under differentiation and we're left with the result. Conversely, taking $\log$ outputs a quadratic whence the density is Gaussian. $a<0$ is required for integrability.

## Expected information gain: semicontinuity and compactness

### Lower semicontinuity

This section will culminate in a proof that $\nu\mapsto D(\nu\mid \mu)$ is lower semicontinuous. However, I want to take the scenic route to clarify the intuition (which is also the proof strategy).

Fix a measure space $X$ with measure $\lambda$ and consider a non-negative function $X\to \mathbb R$, thinking of it as mass-density with respect to $\lambda$ (formally as the Radon-Nikodym derivative of some finite measure relative to $\lambda$). The integral is the total mass. When $f$ takes finitely many values it is a simple sum, which can be further decomposed when $X$ itself is finite.

$$\begin{aligned}
\int f\mathrm d\lambda & =\sum_y y_i\lambda ( f^\leftarrow (y))\\
& = \sum_x f_i(x)\lambda\lbrace x\rbrace  \\
\end{aligned}$$

What can be said about action of the integration operator $f\mapsto \int f \mathrm d\lambda$ on non-negative functions?

* When $X$ is finite, we expect continuity: no matter what sequence $f_n\to f$ we take, mass has nowhere to go in the limit, and $\int f_n\to \int f$.
* When $X$ is infinite, mass can drift off to infinity and disappear at limit. In the discrete case - a wandering sequence of deltas. In the continuous case - a wandering sequence of bumps. Nevertheless, mass cannot *appear* at the limit because it has nowhere to come from. This is the statement of Fatou's lemma.

For a lucid phrasing of Fatou's lemma, recall a real-valued function $f$ is *lower semicontinuous* when its sublevel sets are closed $\lbrace x:f(x) \leq c\rbrace$. Such a function *cannot jump up at the limit*, i.e. stuff cannot appear at the limit. Stuff can *instantly* jump up - from the boundary of the sublevel set - but not down, since superlevel sets are open. Fatou's lemma says integration of non-negative functions is lower semicontinuous. The proof builds on the finite case and is underlain by a simple and important fact.

**Suprema of continuous functions are lower semicontinuous.** Let $f_i$ be a sequence of continuous maps. The pointwise supremum $\sup_i f_i$ is lower semicontinuous.

*Proof.* By the universal property of $\sup$ we have $\lbrace \sup_i f_i \leq c\rbrace = \bigcap_i f_i^\leftarrow (-\infty, c]$.

**Fatou's lemma.** Integration is continuous on finite spaces. Integration of non-negative functions is lower semicontinuous. Moreover, integration of bounded-from-below functions is also lower semicontinuous. Finally, if $\varphi$ is lower semicontinuous and bounded-from-below then the composite $\int \circ \varphi_\ast$ is also lower semicontinuous, i.e. the sublevel sets $\lbrace f\in \mathbb R^X:\int \varphi\circ f \leq c \rbrace$ are closed.

*Proof.* In the finite case, pick a uniform bound on the tail and use it to control the difference between $\int f_n,\int f$. For the infinite case, first fix a finite (measurable) partition $\mathcal P$ of $X$. Define the simple lower approximation $L_\mathcal{P}$ by 
$$
L_\mathcal{P}f=\sum_{A\in \mathcal P} \mathbf 1_A \inf_A f.
$$
By definition of integration of non-negative functions $\int f=\sup_\mathcal{P}\int L_\mathcal{P}f$. By the lemma it suffices to prove each $L_\mathcal{P}$ is lower semicontinuous. Now we'll reduce to the (continuous) finite case. To this end define
$$
L_{\mathcal F \preceq P} f=\sum_{F\in \mathcal F}\mathbf 1_F\min_Ff,
$$
where $\mathcal F\preceq \mathcal P$ denotes a choice of finite subset of each cell in $P$. These operators are continuous due to the *finite* minima. A straightforward verification shows
$$
L_\mathcal{P}= \sup_{\mathcal F\preceq \mathcal P}L_{\mathcal F\preceq \mathcal P}.
$$
Thus $\int = \sup_\mathcal{P}\sup_{\mathcal F\preceq \mathcal P}L_{\mathcal F\preceq \mathcal P}$,
establishing lower semicontinuity. Now suppose the sequence $f_n$ is merely bounded-from-below by $M$. Then we can apply Fatou's lemma to $f_n+M\geq 0$. The result follows by linearity of the integral. To be a bit more explicit, consider the commutative square starting at the subset of functions bounded-from-below by $-M$. The horizontal edges are translations by $M$ while the vertical edges are integration, and commutativity is by linearity of integration. For the last assertion, lower semicontinuity is closed under composition.

Led by Fatou-style intuition that mass can only drift off in the limit (but not appear), intuition suggests similar behavior of expected information gain $D(\nu\mid\mu)=\int \frac{\mathrm d\nu}{\mathrm d\mu}\log \frac{\mathrm d\nu}{\mathrm d\mu}\mathrm d\mu$. Upon inspection we implicitly consider densities (the standard visualiation of histograms) and not the distributions themselves.

**Corollary.** If $\frac{\mathrm d\nu_k}{\mathrm d\mu}\overset{\text{a.e}}{\longrightarrow} \frac{\mathrm d\nu}{\mathrm d\mu}$ and $D(\nu_k\mid\mu)\leq d$ for all $k$ then $D(\nu\mid\mu)\leq d$.

*Proof.* Apply Fatou to $\varphi(x)=x\log x$.

Since pointwise convergence of densities is extremely stringent, it's natural to wonder about lower semicontinuity with respect to coarser topologies. The following theorem gives a geometric characterization of the *weak topology* on the space of measures. We will adopt it as a definition.

**Portmanteau theorem.** A sequence of measures converges in the weak topology $\mu_n\Rightarrow \mu$ iff for all measurable $A$ satisfying $\mu (\partial A)=0$ we have $\mu_nA\to \mu A$.

Intuitively, the condition $\mu(\partial A)=0$ prevents measure from leaking out into the boundary. Thus the weak topology is oblivious to behavior of $\mu_n A$ if there is concentration at the boundary $\mu (\partial A)>0$.

The takeaway:

**Continuity for a fixed continuity partition.** Assume $\mathscr P$ is a finite partition satisfying $A\in \mathscr P\implies \mu(\partial A)=0$. Then $\nu \mapsto D_\mathscr{P}(\nu\mid\mu)$ is continuous with respect to the weak topology, i.e.

$$
\nu_n\Rightarrow \nu \implies D_\mathscr{P}(\nu_n\mid \mu)\to D_\mathscr{P}(\nu\mid \mu).
$$

*Proof.* By Portmanteau $\nu_n\Rightarrow \nu$ implies set-wise convergence over $\mathscr P$. For each summand of $D_\mathscr{P}(\nu\mid\mu)=\sum_{A\in \mathscr P}\nu A \log \frac{\nu A}{\mu A}$, $\mu A$ is fixed and the map $x\log \frac x{\mu A}$ is continuous. Hence set-wise convergence gives the desired result.

Now we anticipate lower semicontinuity $\nu\mapsto D(\nu\mid\mu)$ if we can only compute $D(\nu\mid\mu)$ as a supremum over continuity partitions. First we'll prove the supremum formula and then explain why it suffices to use continuity partitions.

**Partition-supremum formula.** Let $\mathscr P$ be a finite measurable partition of the probability space. Define $D_\mathscr{P}(\nu\mid\mu)=\sum_{A\in \mathscr P}\nu A \log \frac{\nu A}{\mu A}$. Then we have the *coarse-graining inequality* $D_\mathscr{P}(\nu\mid\mu)\leq D(\nu\mid\mu)$, and we can compute on finite partitions $$D(\nu\mid\mu)=\sup_\mathscr{P}D_\mathscr{P}(\nu\mid\mu).$$

*Proof.* Write $f=\frac{\mathrm d\nu}{\mathrm d\mu}$. By using Jensen on each cell $A$ w.r.t the induced probability measure $\mu\;\mid_A=\tfrac 1{\mu A}\mathbf 1_{A}\mu$ we have:

$$\begin{aligned}
D_\mathscr{P}(\nu\mid \mu) & =\sum_{A\in \mathscr P}\nu A \log \frac{\nu A}{\mu A} \\
& = \sum_{A\in \mathscr P}\mu A \underbrace{\frac{\nu A}{\mu A}}_{\frac{1}{\mu A}\int_A f\mathrm d\mu} \log \frac{\nu A}{\mu A} \\
&= \sum_{A\in \mathscr P}\mu A \cdot\varphi \left(\int f \mathrm d \mu \;\mid_A\right) \\
& \leq \sum_{A\in \mathscr P}\mu A \int \varphi(f)\mathrm d \mu\;\mid_A \\
& = \int f\log f\mathrm d\mu= D(\nu\mid \mu).
\end{aligned}$$

For the reverse inequality, some notation. Write $f_\mathscr{P}=\mathbb E_\mu\left[ \frac{\mathrm d\nu}{\mathrm d\mu} \mid \sigma(\mathscr P)\right]$. This function is constant on each cell of $A\in \mathscr P$, taking the value $\frac{\nu A}{\mu A}$. Write shorthand $f_n$ for $f_{\mathscr{P}_n}$ where $\mathscr P_n=\sigma(s_n)$ is generated by increasingly accurate $L^1$ approximations of $f$ by simple functions. Now compute using the fact conditional expectation is an $L^1$ contraction.

$$\begin{aligned}
\| f_n-f \|_1 &= \| \mathbb E[f\mid \sigma(s_n)]-f \|_1 \\
&\leq \| \mathbb E[f\mid \sigma(s_n)]-s_n \|_1 + \| s_n - f \|_1 \\
& = \| \mathbb E[f - s_n\mid \sigma(s_n)] \|_1 + \| s_n - f \|_1 \\
& \leq 2\| f - s_n \|_1\overset{n\to \infty}{\longrightarrow}0.
\end{aligned}$$

Since $D_{\mathscr P_n}(\nu\mid\mu)=\int f_n\log f_n\mathrm d\mu$ we can combine coarse-graining with generalized $L^1$-Fatou for $\varphi(x)=x\log x$ to obtain the following inequalities, which give equality at the limit.

$$
\int f_n\log f_n\mathrm d\mu\leq \int f\log f\mathrm d\mu \leq \liminf \int f_n\log f_n\mathrm d\mu.
$$

**Lower semicontinuity.** Suppose we live in a separable metric space. Expected information gain $\nu \mapsto D(\nu\mid\mu)$ is lower semicontinuous with respect to the weak topology on the space of probability distributions.

*Proof.* We want to use the supremum formula for partitions alongside closedness under suprema. For this we must show the supremum can be computed with continuity partitions. Here we use the separable metric assumption to filter out balls with measure on the boundary. The proof is a straightforward hassle and we omit it.

**Corollary.** (Continuity.)
1. Differential entropy $\nu\mapsto h(\nu\mid \lambda)$, when restricted to a subspace of measures $\nu \ll \lambda$ with prescribed mean and variance, is upper semicontinuous.
2. Moreover, if $f_n\overset{\text{a.e}}{\longrightarrow} f$ is a pointwise convergent and uniformly bounded sequence then $h(f_n\mid \lambda)\to h(f\mid \lambda)$ and $D(f_n\mid N(\mu,\sigma^2))\to D(f\mid N(\mu,\sigma^2))$.

*Proof.* Recall the formula for entropy relative to the matching Gaussian $h(f\mid N(\mu,\sigma^2))=h(f\mid \lambda)-h(N(\mu,\sigma^2)\mid \lambda)$. The LHS is upper semicontinuous in the distribution with density $f$, whence so is the left summand in the RHS. For the second assertion we use uniform boundedness to apply Fatou's lemma, which gives lower semicontinuity and therefore convergence. Specifically, set $c=\sup_{n,x}f_n(x)$ and observe $h(f_n\mid \lambda)=c\int(-\frac{f_n}{c}\log\frac{f_n}{c})\mathrm d\lambda -\log c$ with the RHS integrand non-negative. The relative entropy formula ensures differential entropy and the expected information gain converge together.

### Compactness

We'll need the following theorem.

**Prokhorov theorem.** In the weak topology, a sequence $(\mu_n)_{n\geq 1}$ has a convergent subsequence iff $\forall \varepsilon >0$ there's a compact $K$ such that $\sup_n\mu_n (X\setminus K)<\varepsilon$.

Intuitively, existence of compact sets uniformly  containing most of each measure $\mu_n$ prevents measure from leaking into infinity on our space (at the limit).

**Sequentially compact sublevel sets.** Fix a probability measure $\mu$ (inner regular - automatic for Borel measures on nice spaces). When restricted to probability measures, expected information gain $\nu\mapsto D(\nu\mid \mu)$ has sequentially compact sublevel sets. That is, every sequence in $\lbrace \nu : D(\nu\mid\mu)\leq d \rbrace, d\in \mathbb R$ has a convergent subsequence. By lower semicontinuity, the limit also lies in $\lbrace \nu : D(\nu\mid\mu)\leq d \rbrace$.

*Proof.* We use Prokhorov. Let $\lbrace \nu_ n\rbrace_{n\geq 1} \subseteq \lbrace \nu : D(\nu \mid \mu \leq d\rbrace)$. Given $\varepsilon > 0$ we must find a compact $K$ such that $\sup_n\nu_n (X\setminus K)<\varepsilon$.

The supremum characterization of entropy ensures $D_\mathscr{P}(\nu\mid\mu)\leq D(\nu\mid\mu)$ for a partition $\mathscr P$. Taking a binary partition we have (using $x\log x\geq -\frac 1e$) the following inequality for any $\nu $.

$$\begin{aligned}
D(\nu\mid\mu) &\geq \nu A\log \frac{\nu A}{\mu A} + (1-\nu A)\log \frac{1-\nu A}{1-\mu A} \\
& = \nu A\log \frac 1{\mu A} +\nu A\log \nu A + (1-\mu A)\frac{1-\nu A}{1-\mu A}\log \frac{1-\nu A}{1-\mu A} \\
& \geq \nu A\log \frac 1{\mu A} -\frac 1e - (1-\mu A)\frac 1e \\
& = \nu A\log \frac{1}{\mu A} - \frac {2-\mu A}e \\
& \geq \nu A\log \frac{1}{\mu A} - \frac 1e.
\end{aligned}$$

Hence every $\nu $ in the sublevel set satisfies $\nu  A\leq \frac{d+\frac 1e}{\log 1/\mu A}$. Since we're working over a nice space, $\mu$ is inner regular. Hence we can find a compact $K$ such that $\mu (X\setminus K)$ is arbitrarily small. Given $\varepsilon$, take $\mu(X\setminus K)$ sufficiently small so as to satisfy $\frac{d+\frac 1e}{\log 1/\mu A}<\varepsilon$.

# Information and Dynamics

An invertible measure-preserving flow just moves stuff around. Pure transport/circulation. It's intuitively clear that information is just moving around the space but staying globally constant. Thus we expect *conservation of information*: expected information gain (and therefore entropy) should stay constant over time.

The proof is extremely simple. Fix any deterministic $f$. First, $\mathbb E_\nu [f\left( \frac{\mathrm d T_{t\ast}\nu}{\mathrm d\pi} \right)]=\int \frac{\mathrm d T_{t\ast}\nu}{\mathrm d\pi} f \left(\frac{\mathrm d T_{t\ast}\nu}{\mathrm d\pi}\right)d\pi$. Second, by invariance and the group property, $\frac{\mathrm d T_{t\ast}\nu}{\mathrm d\pi}=\frac{\mathrm d \nu}{\mathrm d\pi}\circ T_{-t}$. By invariance again the integral is independent of $t$ as desired. Taking $f=1$ gives conservation of probability while taking $f=\log$ gives conservation of information gain. (Involving derivatives also gives conservation of Fisher information, which we'll visit later.)

The takeaway: invertible invariant dynamics are uninteresting from the informational perspective.

1. **Pure advection/transport.** We covered this case: invariant invertible flow. Pure circulation, so nothing interesting is happening with information.

2. **Divergence/variance.** Divergence is the opposite of invariance. It means an invertible flow causes accumulation/compression or dispersion/expansion of stuff in regions of our space. Divergence is a local phenomenon, independent of global conservation of stuff, that affects entropy. A concentrating flow increases expected information gain while a dispersive flow evens stuff out and reduces expected information gain.

3. **Mixing/averaging/blurring/smoothing.** Intuitively, mixing/averaging can only even things out - never concentrate. Hence an initial measure can never be mixed into a more concentrated one. Consequently, mixing is fundamentally non-invertible: it is not surjective on the space of probability measures. The same intuition suggests mixing always increases relative entropy/reduces relative information gain - unlike divergence, which is signed and has a signed effect.

# The theory of mixing of distributions: Markov processes

## Building intuition: discrete time

### Finite state space

A finite Markov chain is a directed graph on a state space with edges labeled by positive *conditional* (!) transition probabilities $i\overset{p_{ij}}{\longrightarrow}j$ of next state $j$ *conditioned* on current state $i$. We omit edges for zero probabilities, and we always omit loops since their probability can be calculated from the other arrows emanating from the state in question. To be clear, transition probability is *not* the joint probability of current state *i* and next state $j$. The mixing picture is as follows:

* Each state is a cup with some stuff.
* An initial distribution $\nu$ describes the distribution of the total stuff between the cups. For instance a delta means all of the stuff is in one cup while a uniform distribution means the stuff is evenly split.
* The probabilities $p_{ij}$ say what *fraction of the stuff in each cup* is going where. So $p_{ij}=\frac 12$ means half of the stuff in cup $i$ will next flow through pipe $i\to j$ - regardless of how much stuff cup $i$ holds relative to other cups.
* The key point is the interplay between the initial distribution $\nu$ of the total *amount* of stuff and the mixing operator that says what fractions of each amount move between cups.
* The probability $\nu_i p_{ij}$ is the fraction of *total* stuff going through the pipe $i\to j$: it is the *joint* probability that $\nu_i$ of the stuff is in cup $i$ *and* $p_{ij}$ of that stuff will move into cup $j$. The value $\nu_i p_{ij}=\frac 12$ means half of all the stuff will next flow through the pipe $i\to j$.

⚠️ The simple example $1\underset{q}{\overset{p}{\rightleftarrows}}2$ shows Markov chains can have *concentrating bias*. In the extreme case $1\underset{1}{\rightleftarrows}2$ the first state absorb all the stuff. In the general case, the dynamics moves $p$ of the stuff in the first cup to the second and $q$ of the stuff in the second cup to the first. Initial distributions like deltas are indeed mixed and the result is more evenly distributed. But if we start with the uniform distribution, the result moves stuff in the direction of $\max(p,q)$: concentration!

Intuitively, "pure mixing" cannot alter a uniform distribution, since any such effect is concentrating. Formally we are stipulating invariance of uniform distribution under a "pure mixing" Markov chain.

Since a Markov chain is entirely specified by the $(p_{ij})$, it is equivalently representable by a matrix. We will use the row-stochastic convention, where row $i$ represents outgoing probabilities $i\to \bullet$ (hence row-sums equal one) while column $i$ represents incoming probabilities $i \leftarrow \bullet$. For example the Markov chain $1\underset{q}{\overset{p}{\rightleftarrows}}2$ is represented by the row-stochastic matrix
$$
\begin{pmatrix} 1-p & p \\ q & 1-q \end{pmatrix}
$$
with the extreme case $1\underset{1}{\rightleftarrows}2$ given by the matrix $\begin{pmatrix} 1 & 0 \\ 1 & 0 \end{pmatrix}$. In the row-stochastic convention, the action on an initial distribution is row-mixing $\nu\mapsto \nu P$, or equivalently $\nu\mapsto P^\top \nu$. Thus Markov chains are *linear* mixing operators. The "pure mixing" condition says $\frac 1n \mathbf 1 P=\frac 1n \mathbf 1$. Unpacking, the LHS adds the rows to each other, and the equation means column-sum equals one. Thus pure mixing operators are represented by doubly-stochastic matrices.

But is that the entire story? Well, no. Permutation matrices are doubly-stochastic, but *all* they do is circulate, without any mixing. Even if we look at a doubly-stochastic perturbation of a permutation matrix
$$
\begin{pmatrix} \varepsilon & 1-\varepsilon & 0 \\ 0 & \varepsilon & 1-\varepsilon \\ 1-\varepsilon & 0 & \varepsilon \end{pmatrix}
$$
we still see a circulation dynamic $1 \overset{1-\varepsilon}{\longrightarrow} 2 \overset{1-\varepsilon}{\longrightarrow} 3 \overset{1-\varepsilon}{\longrightarrow} 1$. A pure mixing operator should be circulation-free. To capture this, we require zero circulation at equillibrium! Concretely, for a stationary $\pi$, zero circulation asserts the total volume flowing through pipe $i\to j$ equals the total volume through $i\leftarrow j$. In other words, $\pi_i P_{ij}=\pi_j P_{ji}$. In our doubly-stochastic context $\pi$ is the uniform distribution, and the equation is equivalent to symmetry $P=P^\top$, but this is a peculiarity of the uniform distribution! For infinite state spaces, the reference equillibrium $\pi$ will play a very explicit role.

Every finite Markov chain canonically decomposes into maximal strongly connected components. By collapsing these components into points we obtain a new graph of communication *between components*. By maximality, components are either entirely disconnected from each other, or connected in just one direction. A component disconnected from the others is *closed*, while a component that points outward is *transient*, meaning all the stuff in it will eventually drain out. Components with arrows going into them are *absorbing*. This decomposition reveals highlights the irrelevance of transient components to long-term dynamics: eventually only closed and absorbing components matter.

The topological decomposition above defines the Frobenius normal form of a row-stochastic matrix. Specifically, by permuting so transient states come first, we obtain a nice stochastic block matrix. More formally, $P$ admits a permutation matrix $\Pi$ such that
$$
\Pi^\top P \Pi=\begin{pmatrix} T & R \\ 0 & \text{diag}(P_1,\dots,P_k) \end{pmatrix}
$$
where $T$ has row sums $\leq 1$ and describes movement between transient states, $R$ "complements" $T$ and describes movement from transient states, the zero block says transient components do not receive any stuff, and the block diagonal matrix has a stochastic block for each closed component. Consequently, if $P$ has positive entries then it is already strongly connected. However, positive entries are much stronger: they ensure any state transition can be performed in one step, thereby also precluding any cycles.

Given a stochastic matrix, one typically studies its action on the space of initial distributions. Finite distributions can identified with their density relative to counting measure: $n$-tuples of numbers summing to one. Hence we are studying the action of stochastic matrices on the standard $n$-simplex $\Delta^{n-1}=\lbrace x\geq 0:\sum_i x_i=1 \rbrace\subseteq \mathbb R^n$. Classical theory studies the *semigroup* (!) $P^\mathbb N$, and specifically the limit points of the flow. Even for invertible stochastic $P$, its inverse is generally not stochastic whence not a mixing operator. This is intuitive: unmixing things does not resemble mixing whatsoever, whence *semi*group. Thankfully, *doubly*-stochastic matrices *are* closed under multiplication (intuitively clear), so a "pure-mixing" generator $P$ induces a "pure-mixing" semigroup $P^\mathbb N$.

Since stochastic matrices are $L^1$-contractions and the simplex is compact (!), limit points of the flow are either fixed points (stationary distributions) or cycles. Stationary distributions are attractive iff there are no cycles. Some examples with two states:
* $$
\begin{pmatrix} 1 & 0 \\ 0 & 1 \end{pmatrix}
$$
describes disjoint static states. There is no flow.
* $$
\begin{pmatrix} 0 & 1 \\ 1 & 0 \end{pmatrix}
$$
describes a 2-cycle $1\underset{1}{\overset{1}{\rightleftarrows}}2$. The flow $P^n$ switches at odd $n$ and does nothing at even $n$. The uniform distribution is stationary but not attractive.
* $$\begin{pmatrix} \frac 12 & \frac 12 \\ \frac 12  & \frac 12 \end{pmatrix}$$
describes $1\underset{1/2}{\overset{1/2}{\rightleftarrows}}2$. There is no flow. 
* $$
\begin{pmatrix} 1-p & p \\ p  & 1-p \end{pmatrix}$$
describes $1\underset{p}{\overset{p}{\rightleftarrows}}2$. This is $p$-mixing: each side shares a $p$-fraction of their "stuff". For $p>\frac 12$ most of the "stuff" wants to move over, so the convergence from any initial distribution will be oscillatory. (Imagine Alice has a million dollars and Bob has nothing, and start the game for $p=1-\varepsilon$. After one turn, Bob will have almost everything and Alice almost nothing, after two turns, Alice will be richer again, but by a smaller wealth gap, and so on: converging oscillations.) For $p<\frac 12$ convergence will be monotone. No matter the initial distribution, we even out toward a uniform distribution.
* $$
\begin{pmatrix} 1-p & p \\ q  & 1-q \end{pmatrix}
$$
describes $1\underset{q}{\overset{p}{\rightleftarrows}}2$. So $p$ of the stuff flows right and $q$ flows left. Hence we have a unique attractive stationary distribution given by the expected fraction of stuff $(\frac q{p+q},\frac p{p+q})$. If $p+q> 1$ then most of the stuff is moving, so convergence is oscillatory due to back-and-forth. If $p+q<1$ most stuff stays in place, and convergence is monotone.

For a doubly-stochastic chain with positive entries we therefore anticipate very simple limit dynamics: a single attractive fixed point given by the uniform distribution. Intuitively: "pure mixing" approaches the distribution with maximal Shannon entropy, regardless of circulation. In the disconnected case, we expect componentwise convergence to uniform distributions (think of a block-diagonal doubly-stochastic matrix as separate collections of cups with stuff). Moreover, we expect monotone convergence and monotone entropy increase: every mixing step increases entropy. In discrete time, monotonicty of entropy for arbitrary state spaces is actually an easy consequence of Jensen's inequality via the so-called data-processing inequality. We will revisit this soon.

Larger state spaces accomodate richer dynamics. Thankfully, the limit points are controlled entirely by the spectrum of $P$ as a linear operator on Euclidean space. This is essentially a consequence of the fact $P$ is an $L^1$-contraction, whence its (possibly complex) eigenvalues have at most unit absolute value $\mid\lambda \;\mid\leq 1$. We'll analyze the limit behavior of Jordan blocks, first inside the circle and then on it. Recall Jordan normal form gives $P=VJV^{-1}$ with $J$ block diagonal comprised of Jordan blocks $J_\lambda=\lambda (I+N_\lambda),N_\lambda^{m(\lambda)=0}$ where $m(\lambda)$ is the algebraic multiplicity of $\lambda$ in the characteristic polynomial. For $n>m(\lambda)$ we shall use the expansion $J_\lambda ^n=\lambda ^n(I+N_\lambda)^n=\lambda ^n\sum_{k=0}^{m(\lambda)-1}\binom{n}{k}N_\lambda^k$.

* For $\mid\lambda \;\mid<1$ we have $\lambda^n \binom{n}{k}\overset{n\to \infty}{\longrightarrow}0$ for any $0\leq k<m(\lambda)$. We deduce the effects of eigenvalues of sub-unit magnitude diminish and die in the limit. Thus we are left with eigenvalues on the unit circle.
* For eigenvalues on the unit circle, an $L^1$-contraction must satisfy $m(\lambda)=1$. ontraction. To see this, choose $v\in V\setminus \operatorname{Ker}N^{m(\lambda)-1}$ and consider its resulting linearly independent Jordan chain $v,N_\lambda v,\dots,N_\lambda ^{m(\lambda)-1}v$. Then $J_\lambda ^n v=\lambda ^n\sum_{k=0}^{m(\lambda)-1}\binom{n}{k}N_\lambda^k v$. Using linear independence we can compute the norm in coordinates given by the Jordan chain, and thus bound the $L^1$-norm from below by $\binom{n}{m(\lambda)-1}$. But our map must be a contraction, whence $m(\lambda)=1$.

In summary, the limit dynamics of $P^\mathbb N$ are determined by the peripheral spectrum of $P$: the eigenvalues lying on the unit circle. If $\lambda=1$ is the only eigenvalue then it remains to study its eigenspace. Additional eigenvalues on the circle introduce cycles: periodic limit points (these are never attractive). Stationary distributions are attractive iff there are no cycles. Lastly, the Perron-Frobenius theorem asserts that a *closed* strongly connected chain admits $\lambda = 1$ as a simple eigenvalue. The unique stationary distribution is the unique eigenvector in the one-dimensional eigenspace whose coordinates sum to one. We can globalize. For each component consider the face of $\Delta^{n-1}$ defined by distributions supported only on the component (spanned by the relevant deltas). By Perron-Frobenius, there's unique stationary distribution on each component, i.e. on each face. Each such fixed point is attractive on *its face* iff that face admits no cycles.

The spectral analyis justifies our interpretation of time-symmetric mixing as "pure": if $P$ is symmetric stochastic then its spectrum is real, lying in the interval $[-1,1]$. Hence it can have period of at most two. In the aperiodic (circulation-free) case, e.g. with strictly positive entries, the unique dominating eigenvalue is $\lambda=1$.

### General state space: Markov kernels

So far we defined mixing operators as those with stochastic matrix representation with respect to the standard basis. We then isolated mixing with possible circulation as doubly-stochastic. Lastly, we distilled "pure mixing" as symmetric stochastic matrices (in particular doubly-stochastic), and we emphasized the implicit role of having the uniform distribution as equillibrium. Now we want to start analyzing how chain dynamics affect entropy. Intuition dictates that mixing should dissipate information gain, and that circulation is unimportant. This is qualitatively true, but circulation can actually affect the rate of dissipation by moving stuff into good/bad directions. At any rate, the analysis goes through for general state spaces, and I find measure-theoretic notation to be more conceptual. Moreover, we'll need general state spaces for more geometric applications such as CLT. So now we'll generalize Markov chains and then derive some basic information identities that easily imply dissipation. We'll leave circulation (or lack thereof) for later. We'll disregard the generalization of classical finite state Markov chain theory to infinite state spaces because it's more delicate and we won't need it.

Row-mixing notation $\nu\mapsto \nu P=\sum_i \nu_iP_i$ implicitly performs the "canonical" identification of finite distributions with their density relative to counting measure. Extra precision reveals the action on an initial distribution $\nu$ as the $\nu$-expectation of the family of distributions encoded by the transition matrix.

$$
\begin{aligned}
\frac{\mathrm d\nu}{\mathrm d\text{#}}\frac{\mathrm dP}{\mathrm d\text{#}}& = \sum_i \frac{\mathrm d\nu}{\mathrm d\text{#}}(i)\frac{\mathrm dP}{\mathrm d\text{#}}(i) \\
& = \int \frac{\mathrm d\nu}{\mathrm d\text{#}}\frac{\mathrm dP}{\mathrm d\text{#}}\mathrm d\text{#} \\
& =\int \frac{\mathrm dP}{\mathrm d\text{#}}\mathrm d\nu \\
& =\mathbb E_\nu \left[\frac{\mathrm dP}{\mathrm d\text{#}}\right] 
\end{aligned}
$$

Thus a stochastic matrix is just a family of densities relative to counting measure, indexed by a finite state space. Removing the word 'finite' and dealing with distributions directly gives rise to *Markov kernels*: linear mixing operators between general spaces. Formally, a Markov kernel between measurable spaces $(X,\mathscr A)\overset{P}{\longrightarrow}(Y,\mathscr B)$ is a measurable function
$$
(X,\mathscr A)\overset{P}{\longrightarrow}\mathcal P(Y,\mathscr B)
$$
to the space of probability distributions equipped with the $\sigma$-algebra generated by the set of evaluations $$\lbrace\operatorname{ev}_B,B\in \mathscr B\rbrace$$. A Markov kernel canonically lifts to act on distributions
$$
\mathcal P(X,\mathscr A)\overset{P}{\longrightarrow} \mathcal P(Y,\mathscr B)
$$
via $\nu P=\mathbb E_\nu P$,
i.e. $P$ mixes $\nu$ into the $\nu$-expectation of the family. (Formally, the expectation is a Bochner integral, but I'm in favor of disregarding pedantry in our context.)

Now we come to generalize "pure mixing" operators from *doubly*-stochastic matrices. As discussed above, the additional requirement $\mathbf 1P=\mathbf 1$ means the uniform distribution is stationary, and mild connectedness assumptions give uniqueness, giving a globally attractive fixed point for the flow. Intuitively, this means doubly-stochastic matrices represent mixing operators whose "most mixed" or "most spread out" state is uniform. In general state spaces we follow the same principle and work relative to a chosen stationary reference $\pi$, which is unique under similarly mild connectedness assumptions. Note $\pi$-invariance is equivalent to "$\pi$-sum of the $y$-column equals one". Indeed $\pi P=\pi \iff \frac{\mathrm d\pi P}{\mathrm d\pi}\overset{\text{a.s}}{=}1$ and compute the LHS to discover $\frac{\mathrm d\pi P}{\mathrm d\pi}(y)=\int_\Omega \frac{\mathrm dP_x}{\mathrm d\pi}(y)\mathrm d\mu(x)$.

### Chain rule and change in information gain

To reason about dynamics *of information gain*, it'll be useful review the Bayesian meaning of all this mixing business.

The table below summarizes the key probabilities in play and their generalization from finite to general state space (in discrete time). Before reviewing these in detail we emphasize while $P$ is intrinsic, posteriors depend on the choice of initial distribution.

| Column 1       | Column 2      | Column 3      |
|----------------|---------------|---------------|
| Transition (Next \| Now)   | $P_{ij}$  | $P$  |
| Now   | $(\nu_1,\dots ,\nu_n)$  | $\nu$  |
| Next   | $(\nu P)_i$  | $\nu P=\mathbb E_\nu P$  |
| Joint (Now & Next)   | $\nu_i P_{ij}$  | $\nu \otimes P$  |
| Posterior (Now \| Next) | $\frac{\nu_i P_{ij}}{(\nu P)_j}$ | $\frac{\mathrm d(\nu\otimes P)}{\mathrm d \nu P}$ |
| Posterior for equillibrium $\pi$ | $\frac{\pi_i}{\pi_j}P_{ij}$ | $\frac{\mathrm d(\pi\otimes P)}{\mathrm d \pi}$


$P_{ij}$ is the fraction of stuff currently in state $i$ that will flow through the pipe $i\to j$. This is the probability of the next state $j$ conditioned on the *prior* of present state $i$. (Is it strange to call the present a prior? The mere thought of this makes it past...) The conditional generalizes to a Markov kernel $P$, with $P_x$ the fraction of stuff at state $x$ that will flow into $A$.

$$(\nu P)_{j}$$ is the fraction of the total stuff that will sit inside cup $j$ in the next phase. It depends on $\nu$ because it pertains to the total amount of stuff. It generalizes to the action $\nu P=\mathbb E_\nu P$.

$\nu_i P_{ij}$ is the fraction of the total stuff flowing through $i\to j$. It is the *joint* probability of both present state $i$ *and* next state $j$. The pointwise multiplication should therefore generalize to a joint distribution of "now" and "next" on the product space. Let's think how to compute it on $A\times B$. The second variable appears only in $P$ and won't pose an issue. For the first, we want to integrate $\nu \lbrace x\rbrace P_x$ over $x\in A$. This is the probability measure $\int_A P_x\mathrm d\nu(x) $, which can be evaluated at $B$. That's all there is to it. As for notation, we'll adopt $\nu\otimes P$ following Kallenberg (if I recall correctly). To be honest the moral choice of notation would be asymmetric semidirect product notation $\nu \rtimes P$, but ah well.

To be very concrete, the joint distribution $(\nu \otimes P)(A\times B)$ is the fraction of total stuff that will next flow through pipes $A\rightrightarrows B$. More generally, $(\nu P^m \otimes P^n)(A\times B)$ is the fraction that will flow through from time $m$ for a period of length $n$. If we take an equillibrium $\nu=\pi$ then $\pi P^m \otimes P^n=\pi \otimes P^n$ depends only on the duration of mixing.

That $\nu \otimes P$ is the joint distribution of "now" and "next" implies its $X$ and $Y$ marginals are $\nu$ and $\nu P$ respectively. This is indeed the case:
* Taking $B=Y$ we find the value is $\in_AP_x(Y)\mathrm d\nu (x)=\int_A \mathrm d\nu=\nu A$. Hence $\pi_{X\ast}(\nu \otimes P)=\nu$.
* Taking $A=X$ we find the value is always $\int_XP_x\mathrm d\nu=\mathbb E_\nu P=\nu P$. That is, $\pi_{Y\ast}(\nu \otimes P)=\nu P$.

Now comes a fundamental result.

**Definition.** Fix Markov kernels $P\overset{\text{a.s}}{\ll}Q$. Define *conditional information gain* as the real-valued random variable $D(P\mid Q)\omega=D(P\omega\mid Q\omega)$.

**Chain rule.** Fix Markov kernels $P\overset{\text{a.s}}{\ll}Q$ and probability measures $\nu\ll\mu$. Then:

$$\overbrace{D(\nu \otimes P\mid \mu \otimes  Q)}^{\substack{\text{joint}\\\text{information gain}}}=\overbrace{D(\nu\mid \mu)}^{\substack{\text{prior}\\\text{information gain}}}+\overbrace{\mathbb E_\nu [D(P\mid Q)]}^{\substack{\text{expected conditional} \\ \text{information gain}}}.$$

In fact, the assertion holds when $\nu\ll\mu$ are merely $\sigma$-finite!

*Proof.* Just unpack. If you're concerned with measurability issues, consult the literature (probably Kallenberg, certainly Bogachev).

$$\begin{aligned}
D(\nu \otimes P\mid \mu \otimes  Q) &= \mathbb E_{\nu\otimes  P}\left[\log \frac{\mathrm d\nu \otimes P}{\mathrm d\mu \otimes Q} \right]\\
&= \mathbb E_{\nu \otimes P}\left[\log \frac{\mathrm d\nu}{\mathrm d\mu}+ \log \frac{\mathrm d P}{\mathrm d Q}\right] \\
&= \iint \left[\log \frac{\mathrm d\nu}{\mathrm d\mu}(x)+ \log \frac{\mathrm d P_x}{\mathrm d Q_x}(y)\right]\mathrm d P_x(y) \mathrm d\nu(x) \\
&= D(\nu\mid \mu)+\iint \log \frac{\mathrm d P_x}{\mathrm d Q_x}(y)\mathrm d P_x( y)\mathrm d\nu(x)\\
&= D(\nu\mid \mu)+\mathbb E_\nu [D(P\mid Q)].
\end{aligned}$$

The LHS in the chain rule depends only on the joint distribution while the RHS looks at a particular disintegration: by $\nu$ in the first coordinate. By taking the disintegration by $\nu P$ in the first coordinate, the chain rule will provide an account of how $P$ affects information gain.

Starting with a finite state space, the disintegration is
$$
\nu_i P_{ij}=(\nu P)_j\frac{\nu_i P_{ij}}{(\nu P)_j},
$$
where the quotient is the *posterior* probability of present state $i$ conditioned on next state $j$. Note we condition on the outcome (notably assuming the original chain ran with initial distribution $\nu$). The marginal $\nu P$ thus integrates into the joint distribution via the stochastic matrix 
$$
P^\dagger[\nu]_{ji}=\frac{\nu_i P_{ij}}{(\nu P)_j}.
$$
Geometrically, this *posterior kernel* is measuring *incoming* probabilities instead of outgoing ones.

⚠️ $P_{ij}$ are independent of the initial distribution $\nu$, but the posterior probabilities depend on it very strongly. Specifically, if $\nu$ was concentrated at $i$, meaning most of the stuff started in cup $i$, then the posterior probability 
$$
\frac{\nu_i P_{ij}}{(\nu P)_j}
$$
will be high. Since
$$
P^\dagger[\nu]_{ji}=\frac{\nu_i P_{ij}}{(\nu P)_j},
$$
posterior dynamics behaves "reversely":

> The fraction of stuff in cup $j$ that will flow by the posterior kernel $P^\dagger[\nu]$ through pipe $i\leftarrow j$ equals the volume flowing by $P$ through $i\to j$ divided by the total volume flowing by $P$ into $j$.

Multiplying by the denominator gives $$(\nu P)_jP^\dagger[\nu]_{ji}=\nu_i P_{ij}$$, which geometrically characterizes the posterior kernel $P^\dagger[\nu]$ as time reversal in the sense $\nu P P^\dagger[\nu]=\nu$.

> The total volume flowing by $P$ through $i\to j$ starting from $\nu$, equals the total volume flowing by $P^\dagger[\nu]$ through $i\leftarrow j$ starting from $\nu P$.

⚠️ ️️Upon distilling the mental model we can also see that while $P$ may be autonomous $\nu_k = \nu P^{k-1}$, the posterior chain is very much time dependent due to the strong dependence on initial conditions. In particular, despite the "reverse" behavior, a posterior kernel $P^\dagger[\nu]$ is generally *not* obtained from $P$ by reversing arrows nor by reversing time. That being said, we will only need the first step here, and later on we will only consider the situation relative to equillibrium $\nu=\pi, \pi P=\pi$, in which case the posterior kernel $P^\dagger[\pi]$ will behave like time reversal (relative to $\pi$).

For general state spaces, the posterior generalizes to the posterior Markov kernel given by the family of densities on $Y$ parameterized by events $\mathscr A$ on $X$. The minimal notation $P^\dagger[\nu]=\frac{\mathrm d(\nu\otimes P)}{\mathrm d \nu P}$ evaluates at $(A,y)$ to the probability distribution $\frac{\mathrm d(\nu\otimes P\;\mid_{A\times -})}{\mathrm d \nu P}(y)$.

*Remark.* The above holds for $\sigma$-finite $\nu,\mu$. Moreover, the posterior kernel is itself a Markov kernel even for $\sigma$-finite $\nu$: it remains a family of *probability* measures.

**Information gain and mixing.** Fix a Markov kernel $P$ and two distributions $\nu\ll\mu$ on $(\Omega,\mathscr F)$. Then:

$$
\overbrace{D(\nu\mid \mu)}^{\substack{\text{present} \\ \text{information}\\ {\text{gain}}}}=\overbrace{D(\nu P\mid \mu P)}^{\substack{\text{next} \\ \text{information}\\ {\text{gain}}}}+\overbrace{\mathbb E_{\nu P}\left[D\left( \frac{\mathrm d(\nu\otimes P)}{\mathrm d\nu P} \mid \frac{\mathrm d(\mu\otimes P)}{\mathrm d\mu P} \right) \right]}^{\text{expected posterior information gain}}.
$$

In particular we have the *data-processing inequality* $D(\nu P\mid \mu P)\leq D(\nu\mid \mu)$. Hence, information gain relative to a *stationary* distribution $\pi$ is monotone decreasing (dissipation) $D(\nu P\mid \pi)\leq D(\nu \mid \pi)$.

In fact, the above assertions hold even when $\nu\ll\mu$ are $\sigma$-finite: the formula and also the data-processing inequality, since the posterior kernels are probability measure, ensuring non-negative information gain.

*Proof.* Both sides are equal to joint information gain, by different disintegrations. The LHS disintegrates relative to "now" while the RHS disintegrates relative to "next".

### Duality; posterior action on densities

In this section we'll define how a Markov kernel naturally acts on functions via pullback, and frame the resulting push-pull duality via Hilbert adjoints. This will also justify the adjoint notation for posterior kernels.

Given a Markov kernel $(X,\mathscr A)\overset{P}{\longrightarrow}\mathcal P(Y,\mathscr B)$ we defined its induced pushforward of measures on the source via

$$
\begin{gathered}
\mathcal M(X)\overset{P_\ast}{\longrightarrow}\mathcal M(Y), \\
\nu P(B) = \int P_x(B)\mathrm d\nu(x).
\end{gathered}
$$

Define the pullback of functions on the target via conditional expectation

$$
\begin{gathered}
\mathsf{Meas}_b(X,\mathbb R)\overset{P^\ast}{\longleftarrow}\mathsf{Meas}_b(Y,\mathbb R), \\
Pf(x)=\int f\mathrm dP_x=\mathbb E_{P_x}f.
\end{gathered}
$$

In the discrete case $P$ is a row-stochastic matrix and $f$ a column vector, interpretted as a function on the state space. Write $X_n$ for the random variable whose value is the observed state at time $n$. Then $Pf(i)=\sum_{j}P_{ij}f_j$ is the conditional expectation $\mathbb E[f(X_{n+1})\mid X_n=i]$ of $f$ evaluated on the next state given current value $i$. Taking $f=\mathbf 1_{j}$ recovers the conditional probability
$$
P_{ij}=\mathbb P[X_{n+1}=j\mid X_n=i].
$$
Similarly for continuous time
$$
Pf(x)=\mathbb E[f(X_{s+t})\mid X_s=x].
$$

The following square commutes by Fubini

$$
\begin{array}{ccc}
\mathcal M(X)\times \mathsf{Meas}_b(Y,\mathbb R) & \stackrel{P_\ast \times 1}{\longrightarrow} & \mathcal M(Y)\times \mathsf{Meas}_b(Y,\mathbb R) \\
\Big\downarrow\!{\scriptstyle 1\times P^\ast} & & \Big\downarrow\!{\scriptstyle \int_Y} \\
\mathcal M(X)\times \mathsf{Meas}_b(X,\mathbb R) & \stackrel{\int_X}{\longrightarrow} & \mathbb R
\end{array}
$$

and so unambiguously defines a pairing

$$
\begin{gathered}
\mathcal M(X)\times \mathsf{Meas}_b(Y,\mathbb R)\overset{\langle -,=\rangle_ P}{\longrightarrow} \mathbb R, \\
\int \mathbb E_{P_x}f \mathrm d\nu(x) = \langle \nu, P^\ast f\rangle = \nu P f = \langle P_\ast \nu, f \rangle = \int f \mathrm d\nu P.
\end{gathered}
$$

Now come posterior kernels. Recall a pair $(\nu,P)$ defines a posterior kernel
$$
P^\dagger[\nu]_{ji}=\frac{\nu_i P_{ij}}{(\nu P)_j}
$$
with "reverse dynamics":

> The total volume flowing by $P$ through $i\to j$ starting from $\nu$, equals the total volume flowing by $P^\dagger[\nu]$ through $i\leftarrow j$ starting from $\nu P$.

Every pair $(\nu,P)$ induces a Hilbert adjunction $L^2(\nu)\leftrightarrows L^2(\nu P)$ with top arrow given by pullback $P^\ast$ and bottom arrow given by the pullback along the adjoint $P^\dagger[\nu]^\ast$. When $\nu=\pi$ is an equillibrium $\pi=P\pi$ the posterior kernel simplifies drastically 
$$
P^\dagger[\pi]_{ji}=\frac{\pi_i}{\pi_j}P_{ij}.
$$
In this case $\pi$ is an equillibrium for the posterior kernel too, and the posterior chain is also autonomous, resulting in adjoint *endomorphisms* on $L^2(\pi)$. The defining equation of $P^\dagger[\pi]$ in the discrete case is also equivalent to equality of conditional densities 
$$
\frac{P^\dagger[\nu]_{ji}}{\pi_i}=\frac{P_{ij}}{\pi_j}.
$$
For general state space, this equation says
$$
\frac{\mathrm dP^\dagger_y[\pi]}{\mathrm d\pi}(x)=\frac{\mathrm dP_x}{\mathrm d\pi}(y),
$$
again encoding "reverse dynamics".

Finally we come to action on densities. The key is commutativity of the following square,

$$
\begin{array}{ccc}
\mathcal M_{\ll\mu}(X) & \stackrel{P_\ast}{\longrightarrow} & \mathcal M_{\ll\mu P} (Y) \\
\Big\downarrow\!{\scriptstyle \mathrm{RN}} & & \Big\downarrow\!{\scriptstyle \mathrm{RN}} \\
L^2(\mu) & \stackrel{P^\dagger[\mu]^\ast}{\longrightarrow} & L^2(\mu P)
\end{array}
$$

which expresses the formula $P^\dagger[\mu]\frac{\mathrm d \nu}{\mathrm d \mu}=\frac{\mathrm d \nu P}{\mathrm d \mu P}$. To me this formula seems cryptic at first: the LHS is the *posterior expectation* of the density. For the other side note $(P^\dagger[\mu])^\dagger[\mu P]=P$, so $P\frac{\mathrm d \nu}{\mathrm d \mu P}=\frac{\mathrm d \nu P^\dagger[\mu]}{\mathrm d \mu P}$.

Working relative to an equillibrium $\pi P=\pi$ simplifies matters:

$$
P^\dagger[\pi]\frac{\mathrm d \nu}{\mathrm d \pi}=\frac{\mathrm d \nu P}{\mathrm d \pi},\quad P\frac{\mathrm d \nu}{\mathrm d \pi}=\frac{\mathrm d \nu P^\dagger[\pi]}{\mathrm d \pi}.
$$

The former equation says the family of densities $\frac{\mathrm d \nu P^t}{\mathrm d \pi}$ evolves via the *posterior* process of $(\pi,P^t)$.

### Zero-circulation as self-adjointness

Intuitively, we can detect zero circulation at equillibrium $\pi$ by symmetry: running the dynamics forward $P$ and backwards $P^\dagger[\pi]$ looks the same. Thus we say an equillibrium $(\pi,P)$ is *symmetric*/*reversible*/*self-adjoint* if $P=P^\dagger[\pi]$. The definition is the same for general state space and for continuous time, and expresses *zero circulation*.

On non-compact spaces, Markov monoids often lack any stationary *probability* measures, but nevertheless preserve a natural *ambient* measure $\pi$, e.g. Lebesgue and more generally Haar. Intuitively, the dynamics moves stuff around but neither creates nor destroys volume. The example to keep in mind is heat flow, which distributes temperature without distorting space.

In such cases the viewpoint of densities relative to the invariant ambient measure $\pi$ is useful. The equation $\frac{\mathrm dP^\dagger_y[\pi]}{\mathrm d\pi}(x)=\frac{\mathrm dP_x}{\mathrm d\pi}(y)$ is sensible for arbitrary invariant measures $\pi$. Thus we shall say a dominating equillibrium $P\ll\pi$ has zero-circulation when its density exhibits the spatial symmetry $\frac{\mathrm dP_x}{\mathrm d\pi}(y)=\frac{\mathrm dP_y}{\mathrm d\pi}(x)$. Geometrically, spatial symmetry means the same volume moves $x\to y$ as $x\leftarrow y$, i.e. zero local circulation everywhere.

At any rate, the symmetric case features an especially simple evolution of densities $P^t\frac{\mathrm d \nu}{\mathrm d \pi}=\frac{\mathrm d \nu P^t}{\mathrm d \pi}$.

## Continuous time

### Dissipation of information gain

We've shown that information gain of discrete time Markov processes relative to an equillibrium is monotone decreasing. The story carries over to continuous time by quantization.

A continuous-time (autonomous) mixing flow (Markov chain) is an action of the monoid $\mathbb R_{\geq 0}$ on a space of probability distributions by mixing operators $(P^t)_{t\geq 0}$. It is often called a Markov semigroup in the literature, but the fact it "starts" from the identity is important, so it'd be more accurate to call it a Markov *monoid*.

**Information gain and continuous mixing.** Fix a Markov monoid $(P^t)$ and two distributions $\nu\ll\mu$ on $(\Omega,\mathscr F)$. Then:

$$
\overbrace{D(\nu\mid \mu)}^{\substack{\text{initial} \\ \text{information}\\ {\text{gain}}}}=\overbrace{D(\nu P^t\mid \mu P^t)}^{\substack{\text{information} \\ \text{gain}\\ {\text{at time }t}}}+ \overbrace{\int_0 ^t\lim_{h\searrow 0}\frac 1h\mathbb E_{\nu P^{s+h}}\left[D\left( \frac{\mathrm d(\nu P^s\otimes P^h)}{\mathrm d\nu P^{s+h}} \mid \frac{\mathrm d(\mu P^s\otimes P^h)}{\mathrm d\mu P^{s+h}} \right) \right]}^{\text{total expected posterior information gain}}.
$$

Consequently we have the differential version:

$$\frac{\mathrm d}{\mathrm dt}D(\nu P^t\mid \mu P^t)=-\lim_{h\searrow 0}\frac 1h\mathbb E_{\nu P^{t+h}}\left[D\left( \frac{\mathrm d(\nu P^t\otimes P^h)}{\mathrm d\nu P^{t+h}} \mid \frac{\mathrm d(\mu P^t\otimes P^h)}{\mathrm d\mu P^{t+h}} \right) \right].$$

In particular we have the *data-processing inequality* $D(\nu P^t\mid \mu P^t)\leq D(\nu\mid \mu)$. Hence, information gain relative to a *stationary* distribution $\pi$ is monotone decreasing (dissipation) $D(\nu P^t\mid \pi)\leq D(\nu \mid \pi)$.

*Proof.* Quantize $h=\frac tn$, work with $\nu_k=\nu P^{kh}$, use the discrete time identity and telescope, and argue by Riemann sums while using properties of Lebesgue integrals. Some natural continuity requirements may pop up if you're pedantic, so just assume them. It's a hassle but it works.

### Infinitesimal generators

In the discrete case $P^{\mathbb N_{\geq0}}$ is generated by $P$. In the continuous case, $P^{\mathbb R_{\geq 0}}$ is typically also generated by one object, but this infinitesimal generator is not a member of the monoid and isn't even a mixing operator. The story here is the operator exponential, and we sketch it bug gloss over technical precisions. The exponential action takes a perturbation of the identity by an infinitesimal fraction $\frac 1n$ of our operator - a tiny linear update - and applies this perturbation $n$ times.

$$
e^{tA}=\lim_{n\to \infty}\left( \mathrm I+\frac tn A \right)^n=\sum_{n=0}^\infty \frac{t^n}{n!}A^n.
$$

Just as $u(t)=u_0e^{at}$ is characterized as the unique solution to the scalar ODE $\frac{\mathrm d}{\mathrm dt}u(t)=au(t)$ with initial condition $u_0$, so the operator exponential $u(t)=e^{tA}u_0$ is characterized as providing the unique solution to the vector ODE $\partial_t u=Au$ with initial condition $u_0$ (we suppress the spatial variable). The exponential functional equation (expressing the fact $\exp$ is a group morphism from the reals to the additive structure of our linear space) can be derived either from the limit definitions above, or from the ODE characterization using translations and uniqueness of solutions to ODE. The takeaway is that $\mathbb R$-action $(e^{tA})_{t\in \mathbb R}$ is the flow generated by $A$: while $\partial_t\;\mid_0 u = Au_0$ is just the initial *derivative*, $u=e^{tA}u_0$ is the *position* on the integral curve at time $t$. Conversely, the infinitesimal generator of an exponential group $e^{tA}$ is recoverable as the initial derivative.

As in discrete time, a continuous Markov monoid $P^{\mathbb R_{\geq 0}}$ is often generated by a single object, except it's more delicate: evaluating the exponential characterization $\partial_t e^{tA}=Ae^{tA}$ at zero suggests the Markov generator is $L=\partial_t \;\mid_{t=0^+}P^t$ when this limit exists in a suitably strong sense, in which case $P^t=e^{tL}$. Essentially we're embedding the monoid into an exponential group, keeping in mind that only the non-negative half represents mixing operators.

Before diving into general formalism let's look at the finite state case. Here $L_{ij}=\partial_t\;\mid_{t=0^+} P^t_{ij}$ should be throught of as a *transition rate matrix* because its entries are the initial rates of changes of outgoing probabilities from a given state as the process kicks off. The natural question now is what characterizes transition rate matrices $L$. The answer is simple:

* Zero-sum rows to conserve total row probability, because rows of the flow must always sum to one.
* Non-negative off-diagonal entries, coming from the identity $P_0=\mathrm I$: since $p_{ij}(0)=\delta_{ij}$ the off-diagonal rate of conditional probability accumulation $\partial_t\;\mid_{t=0^+} p_{ij}$ at a different state cannot be negative since "there isn't any probability there" to diminish. Similarly, it cannot be positive on the diagonal.

Evidently $L$ is far from a mixing operator: it even has negative entries. It specifies the initial behavior of the mixing dynamics, and the monoid structure takes over from there.

One can show $P^t=e^{tL}$ is entrywise positive, whence strongly connected and aperiodic. By Perron-Frobenius we deduce $\lambda=1$ is the unique dominating eigenvalue: no "pure circulation" in continuous time. There can still be complex eigenvalues - damped circulation - but they have no effect on limit behavior.

For general state spaces the story is much subtler: the spaces involved admit several topologies, which affect the behavior of the limit. We'll just give the big picture. Recall $P^t$ are now Markov kernels, i.e. measurable maps $(X,\mathscr A)\overset{P^t}{\longrightarrow}\mathcal P(Y,\mathscr B)$. There are three approaches to define $L=\partial_t\;\mid_{t=0^+} P^t$: as a limit of Markov kernels, as a limit in spaces of measures, and as a limit in spaces of functions. The last approach is most familiar and robust, but it's also usually the only one presented. I figured it's worth mentioning all three for pedagogical purposes.

1. Directly: $L_x(B)=\partial_t\;\mid_{t=0^+}P_x^t(B)$. The issue is that there's often no such limit. To see this let's first write out the difference quotient $\frac{P^t_x(B)-\delta_x (B)}{h}$. Now imagine a continuous mixing process that is symmetric about the $y$-axis. The delta will give us trouble at $x=0$. By symmetry and continuity $P^t_0(0,\infty)=P^t_0[0,\infty)=\frac 12$. However, taking $B=(0,\infty)$ makes the numerator equal to $\frac 12$ while taking $B=[0,\infty)$ makes it equal to $-\frac 12$. Both give infinities (of different signs).

2. As a limit of pushforward operators on spaces of *measures*: define $L_\ast=\partial_t\;\mid_{t=0^+} P^t_\ast :\nu\mapsto \lim_{t\searrow 0}\frac{\nu P^t_\ast -\nu}{t}$. The numerator lives in the space of signed measures - much like the diagonal of the transition rate matrix typically has negative entries. There's a few topologies to choose from and generally either fuck up like above or lead to the approach below, via functions.

3. As a limit of pullback operators on spaces of *functions*: define $L^\ast=\partial_t\;\mid_{t=0^+} P^{t\ast} :f\mapsto \lim_{t\searrow 0}\frac{ P^{t\ast}f -f}{t}$. Here again the numerator exits the space of non-negative functions, but this is familiar territory. This admits the the conceptual interpretation as the derivative of the family of conditional expectations since $P^{t\ast}f=\mathbb E_{P^t_x}f$.

⚠️ The literature typically *defines* the infinitesimal generator as an operator on spaces of functions, and denotes it by $L$. It then denotes the adjoint action on measures as $L^\ast$. This convention is unfortunately inconsistent with the canonical push/pull notation, and we will therefore avoid it and stay consistent. Note $L^\ast$ does not mean an underlying $L$ exists.

As in discrete time, one wonders which operators on spaces of functions generate Markov processes. Wonder on, friend - the internet is at your fingertips.

### Time evolution of densities and functions: the forward and backward equations

Returning again to the exponential characterization, $\partial_t e^{tL}=Le^{tL}$ we see $\partial_t,L$ have the same action on the exponential family. Hence the family provides a solution to a time evolution equation.

When a Markov process has an infinitesimal generator, it satisfies the time evolution equation by virtue of being the non-negative part of an exponential family. But what exactly is the equation? We've seen a kernel acts on functions by pullback but on densities by pushforward... We'll start with densities since they will be our main focus.

Recall density evolution is given by the posterior/adjoint $\frac{\mathrm d \nu P^t}{\mathrm d \pi}=P^{t\dagger}[\pi]\frac{\mathrm d \nu}{\mathrm d \pi}$, expressed by commutativity of the square on the left below. Taking its derivative at zero gives the right square (the Radon-Nikodym map is linear).

$$
\begin{array}{ccc}
\mathcal M_{\ll \pi}(X) & \xrightarrow{P^t_\ast} & \mathcal M_{\ll \pi}(Y) \\[1em]
\bigg\downarrow \scriptstyle \mathrm{RN} & & \bigg\downarrow \scriptstyle \mathrm{RN} \\[1em]
L^2(\pi) & \xrightarrow{P^{t\dagger}[\pi]^\ast} & L^2(\pi)
\end{array}
\overset{\partial_t\;\mid_{t=0^+}}{\longrightarrow}
\begin{array}{ccc}
\mathcal M_{\ll \pi}(X) & \xrightarrow{L_\ast} & \mathcal M_{\ll \pi}(Y) \\[1em]
\bigg\downarrow \scriptstyle \mathrm{RN} & & \bigg\downarrow \scriptstyle \mathrm{RN} \\[1em]
L^2(\pi) & \xrightarrow{\;\;L^\dagger[\pi]^\ast\;\;} & L^2(\pi)
\end{array}
$$

Hence $\frac{\mathrm d \nu P^t}{\mathrm d \pi}=P^{t\dagger}[\pi]\frac{\mathrm d \nu}{\mathrm d \pi}=e^{tL^\dagger[\pi]}\frac{\mathrm d \nu}{\mathrm d \pi}$. But this is precisely the form $u(t)=e^{tL}u_0$ of the solution to $\partial_tu=Lu,u(0)=u_0$. Thus we have a time evolution equation for densities:

$$
\partial_t \frac{\mathrm d \nu P^t}{\mathrm d\pi}=L^\dagger[\pi]\frac{\mathrm d \nu P^t}{\mathrm d\pi}.
$$

The literature often calls this the *forward equation* of the process, because we're acting in the direction of the *pushforward* of measures.

The action on functions is adjoint and often called the *backward equation* $\partial_t P^tf=L P^tf$.

## Physical mixing

### Mixing by local operators; maximum principle

In geometric and physical contexts, mixing is typically *local*: points move continuously without teleporting across spacetime. Within an infinitesimal time interval, stuff can only mix in an infinitesimal neighborhood. This can be formalized by saying the infinitesimal generator is *local* in the sense that $Lf(x)$ depends only on the germ of $f$ at $x$. Note the finite mixing operators $P^t$ are typically *not* local, as we shall see by example below. Locality excludes *jump processes*, which we won't touch on.

A celebrated theorem by Peetre states that a local linear operator on smooth real functions is differential of finite order: it's given in coordinates by a polynomial in the partial derivative operators (smooth coefficients). The moral proof has two separate parts. First, one proves local linear operators are continuous in the Fréchet topology, where $f_i\to f$ in the Fréchet topology means all derivatives of finite order converge in the sup-norm over all compacts. The proof is via bump functions and there fails in the real-analytic and complex-analytic categories, which admit discontinuous local linear operators (but not bumps). Second, one proves a continuous linear operator on germs is differential of finite order. This holds in the analytic categories as well, so adding an explicit continuity assumption gives an analytic Peetre. The magic unravels as follows.

* First reduce to the study of functionals by looking at the derivatives of the germ of $Lf$ at $a$.
* Consider the jet map $C_a^\infty\overset{j^\infty_a}{\longrightarrow} J_a^\infty$, whose kernel consists of flat germs. Think of the image as the full set of partials (as a subset of a product of real lines with the product topology).
* Analysis in the Fréchet topology implies a continuous linear functional $C_a^\infty \to \mathbb R$ kills flat germs and consequently factors through as $C_a^\infty \overset{j^\infty_a}{\longrightarrow}J^\infty_a\longrightarrow \mathbb R$. This is how Fréchet continuity manifests derivatives.
* The source of the second factor has the product topology, and continuous *linear* maps from it depend on finitely many coordinates, i.e. we factor through a *finite* jet space. Why does the continuous linear dual of $\mathbb R^I$ always consist of finitely supported sequences? Because the *product topology* is generated by *finite* intersetions of cylinders, so the basic open "balls" look only at finitely many coordinates. Now look at a sufficiently small ball e.g. the preimage $\varphi^\leftarrow{[0,1]}$, choose a direction $e_i$ it disregards, and then look at $\varphi(te_i)$. By assumption its absolute value is $\leq 1$ for all $t$, which forces it to equal zero as desired.
* Some careful bookkeeping gives an explicit global formula for $Lf$ as a finite order differential operator with $C^\infty$ coefficients comprised of the pointwise coefficients of the $Lf(a)$.

To summarize, the locality assumption has reduced us to mixing prescribed given by finite order differential operators.

When $L$ is a *first order* local differential operator the story is simple. In coordinates $L$ is a $C^\infty$-linear combination of partial derivatives alongside a free term which encodes a source/sink. In our context we'll assume the latter is zero. First order differential operators with zero free coefficient are equivalent to vector fields, and the geometry is given by integrating into a flow. Since (sufficiently smooth) vector fields have invertible flows, this is pure circulation without any real mixing. As stuff is only moving around with neither concentration nor diffusion, information gain relative to a flow-invariant measure is static. For a simple example consider $L=\partial_x$. By manipulating Taylor series one anticipates the exponential family of the derivative is given by translation $e^{t\partial_x}f(x)=f(x+t)$ and indeed this family solves the transport equation $\partial_tu=\partial_xu$, whose characteristics are just straight lines.

Real mixing appears for *second order* local differential operators. The core intuition is present already in one dimension, exposed by an elementary but somehow less familiar characterization of the second derivative. Suppose $f\in C^2$ and compute using L'Hôpital's rule w.r.t $h$ to find

$$\begin{aligned}
\lim_{h\to 0}\frac{\frac{f(x+h)-f(x-h)}{2}-f(x)}{h^2} & = \lim_{h\to 0}\frac{f^\prime(x+h)-f^\prime(x-h)}{h} \\
&= \lim_{h\to 0}f^{\prime\prime}(x+h)-f^{\prime\prime}(x-h) \\
&= \frac 12 f^{\prime\prime}(x).
\end{aligned}$$

Hence a positive/negative second derivative means the average is higher/lower. The second derivative is thus proportional to "displacement from the infinitesimal average". With this in mind, the 1D heat equation $\partial_tu=\frac 12 \partial_{xx}u$ describes mixing essentially by definition: it says temperature $u$ evolves precisely according to the average of nearby points: point $x$ gets hotter/colder as long as the average temperature of its neighbors is higher/lower. For computing the exponential we have the general machinery of the Fourier transform, which we'll get to.

In higher dimensions the Laplacian $\Delta = \sum_i \partial_{x_ix_i}$ has a similar characterization: it's proportional to the limit of averaging operators
$$
\Delta \propto \lim_{r\searrow 0}L_rf(x)=\lim_{r\searrow 0}\frac{1}{|\partial B_r|}\int_{\partial B_r(x)}(fy-fx)\mathrm dy,
$$
and also describes mixing. The heat equation $\partial_tu\propto \Delta$ simply generalizes the 1D idea.

*Remark.* Some sources use averaging over balls instead of spheres. For harmonic functions, where the value is *equal* to the average but independent of $r$, both approaches are equivalent. This is a really nice "exercise". I think you need to write the ball average as an integral over $r$ of sphere averages and then differentiate w.r.t $r$ to get some ODE that you can reason with.

Incidentally the above example also produces a natural class of *non-local* mixing operators: averaging over a non-infinitesimal neighborhood
$$
L_rf(x)=\frac{1}{|\partial B_r|}\int_{\partial B_r(x)}(fy-fx)\mathrm dy.
$$
The exponential of this operator represents a Markov kernel that can suddenly *jump* to a uniformly random point on the sphere. But alas, no more on jumps.

Back to our local business. Two questions arise:
1. Is there mixing beyond second order?
2. What about variable coefficient differential operators?

The answer to the first question is a comforting "no". The reason is the *maximum principle for mixing*, which formalizes the following intuitively obvious fact:

> Local averaging of a function at a local maximum cannot increase its value.

In practice we're saying that for a local maximum of $f$ attained at $x_0$ we must have $\lim_{t\searrow 0}\frac{P^tf(x_0)-f(x_0)}{t}\leq 0$, which is precisely $Lf(x_0)\leq 0$. This condition actually forces a *local* $L$ to have at most second order. Thus, *local* averaging is really a second order differential phenomenon!

**A local differential operator that satisfies the maximum principle has order $\leq 2$, with positive semidefinite second order component.**

*Proof.* Take $g$ with trivial 2-jet. We shall show $Lg=0$. Consider the functions $f_\pm=-d(a,x)^2\pm \varepsilon g(x)$, where $d$ is Euclidean distance. The first crucial point is the *square* of Euclidean distance is smooth, unlike $d(a,x)$ itself. Hence $f_\pm$ have smooth germs at $x=a$ and are therefore valid inputs for any differential operator. The second crucial point is that we can control $\varepsilon$ and the ball $B_r(a)$ enough so *both* $f_\pm$ have a local maximum at $x=a$. First, using the integral form of Taylor series remainder we know $g(x)-j^2_a(x)=g(x)$ is a smooth multiple of $(x-a)^3$. Next, working inside our small compact ball $B_r(a)$ we have the bound

$$
f_\pm (x) \leq -|x-a|^2\pm \varepsilon g(x)\leq -|x-a|^2+\varepsilon C|x-a|^3.
$$

The constant $C$ is just the supremum over the continuous remainder. Since we're working inside the ball, $d(a,x)\leq r$. Hence taking a sufficiently small radius so that $\varepsilon Cr<\tfrac 12$ we find $f_\pm$ takes negative values on the *punctured* ball, and equals zero at $x=a$ by construction. Now we can apply the maximum principle to find generator $Lf_\pm\leq 0$. By $\mathbb R$-linearity of $L$ we find $Lg=0$.

As for the positive semidefinite property, I guess we'll leave it as an exercise since we won't need it.

As for variable coefficients, they do complicate things, but only a little. In a nutshell, instead of averaging over infinitesimal balls we average things over their deformations dictated by the coefficients - infinitesimal *ellipsoids*.

### The convection-diffusion equation

We've seen a local linear mixing operator on smooth functions is differential of second order (lower orders not really mixing anything). This actually lands us in very physical/geometric territory: the convection-diffusion equation.

Our mental model for mixing involves isotropic fluid, so we will focus on the isotropic equation where $\alpha>0$ is scalar. In general it is a matrix that encodes direction-dependent transport, e.g. in a layered composite. (Positive semidefinite in physical contexts.)

Below, $T$ is temperature, $\vec v$ is the velocity field of the material (our mixing fluid), and $\alpha$ is diffusivity, specifying the rate at which heat spreads in the coldest direction.

$$\begin{aligned}
\partial_t T+\overbrace{\operatorname{div} (T \vec v)}^{\substack{\text{convective}\\\text{flux (by flow)}}} &=\overbrace{\operatorname{div} (\alpha \nabla T)}^{\substack{\text{diffusive flux/}\\\text{heat conduction}}}+\overbrace{S}^{\substack{\text{sources}\\ \text{and sinks}}}\\
\partial_t T+\underbrace{\nabla T\cdot \vec v}_{\substack{\text{advection}\\\text{by motion}}}+\underbrace{T\operatorname{div}\vec v}_{\substack{\text{compression/expansion}\\\text{heating}}} &= \underbrace{\alpha \Delta T}_{\text{isotropic diffusion}}+ \underbrace{\nabla \alpha \cdot \nabla T}_{\substack{\text{flux from} \\ \text{spatially varying }\alpha}}+S
\end{aligned}$$

We can rewrite the PDE in conservative form $\partial_tu+\operatorname{div}J=S$, where $u$ is density, $J$ is flux, and $S$ is sources/sinks.

$$
\partial_t T= \operatorname{div}(\underbrace{\alpha \nabla T}_{\text{diffusive}}-\underbrace{T\vec v}_{\text{convective}})+S 
$$

Advection by motion means temperature in a region is changing because stuff is being replaced by stuff with a different temperature flowed in. In 1D, suppose $\vec v$ is constant velocity to the right. Suppose the fluid has a uniform temperature of zero and at $t=0$ an intense heat source is turned on at the origin. Then the temperature to the right of the origin will increase as hotter fluid coming from the origin will replace the colder fluid previously there.

When $\operatorname{div}\vec v$ is large, we have expansion and therefore dilution of concentration/temperature.

In the absence of sources and velocity, and when diffusivity $\alpha$ is spatially constant, we arrive at the classical heat equation $\partial_t T=\alpha \Delta T$ which encodes our naive mixing intuition.

The heat equation is really at the heart of continuous time local mixing and not one example out of a huge space.

#### Local mixing = anisotropic convection-diffusion

Reducing a general second order differential operator to the (anisotropic) convection-diffusion equation boils down to a vector-calculus identity. Starting with the 2-jet of $f$, applying $L$ and using the inner product gives 

$$
Lf=cf+\langle b,\nabla f\rangle+\overbrace{\langle A,\operatorname{Hess}f \rangle}^{\operatorname{Tr}(A\operatorname{Hess}f)}
$$

where $A$ is symmetric by symmetry of second partials. The key is $\langle A,\operatorname{Hess}f \rangle=\operatorname{div}(A\nabla f)-\langle\operatorname{div}A,\nabla f\rangle$ (verify by computation), where $\operatorname{div}A$ is the vector of column-wise divergences. It lets us rewrite

$$
Lf=cf+\langle \underbrace{b-\operatorname{div}A}_{\vec v},\nabla f\rangle+\operatorname{div}(A\nabla f).
$$

In the isotropic case we reduce to $A=\alpha I$.

#### Physics digression

The convection diffusion equation is idealized and features non-phyiscal behavior: there is instant spatial propagation of information. Specifically, if we start off with a concentrated initial condition (e.g. a delta) at $t=0$, then temperature is globally nonzero for $t>0$. To see this (jumping slightly ahead of ourselves), recall the heat equation is famously solved via the convolution with the Gaussian kernel, which is globally positive all the time.

The peculiarity stems from Fourier's law of heat conduction, encoding his empirical observations. This is a short story. Writing $\mathbf q$ for heat flux and $Q$ for heat energy density, (local) conservation of energy is $\partial_t Q+\operatorname{div}\mathbf q=0$. Writing $T$ for temperature and assuming our material properties don't change with time (reasonable for short durations) we have $\partial_tQ=c\rho \partial_tT$, where $c,\rho$ are respectively specific heat capacity and density. Fourier observed (in the isotropic case) that heat flows in the negative temperature gradient $\mathbf q= -\alpha \nabla T$, i.e. heat flows in the coldest direction at rate determined by *thermal diffusivity* $\alpha$. Plugging into conservation of energy gives the familiar heat equation. In the anisotropic case $\alpha$ is a matrix encoding directional diffusivity.

Infinite propagation speed of information is incompatible with relativity. To amend this, the Maxwell–Cattaneo conduction equation replaces Fourier's *parabolic* equation with a *hyperbolic* equation involving a second time derivative. The result is better behaved in the sense that propagation speed is finite, but the resulting process is no longer Markovian because second derivatives involve "memory".

For a familiar geometric example of *finite* propagation speed consider the wave equation $\partial_{tt}u=c^2\nabla u$. For simplicity consider the 1D situation. By "factorization" we find two transport equations $0=\partial_{tt}u-c^2\partial_{xx} u=(\partial_t-c\partial_x)(\partial_t+c\partial_x)$ i.e. the wave equations is a pair of opposite-direction transports. Hence, given a solution $u$, we see $\partial_tu\pm c\partial_xu$ satisfies the transport equation $\partial_t\mp c\partial_x$ whose characteristics are straight lines. The solution $u(x,t)$ to the wave equation is supported within a ball of radius $ct$ about $x$.

By the way check out [this video](https://www.youtube.com/watch?v=xaa9h_GAflg) of faster diffusion in hot water hot water.

### Rate of information gain for convection-diffusion; de Bruijn

Assume no sources and consider densities relative to Lebesgue measure. Writing $f_t=\frac{\mathrm d\nu P^t}{\mathrm d\lambda}$, we have our density time evolution PDE. 

$$
\partial_t f_t= \operatorname{div}(\underbrace{\alpha \nabla f_t}_{\text{diffusive}}-\underbrace{f_t\vec v}_{\text{convective}}).
$$

The product rule $\operatorname{div}(f\vec v)=f\operatorname{div}\vec v+\langle \nabla f,\vec v\rangle$ lets us compute via integration by parts (assuming rapid decay of densities) and the identity $\nabla \log u=\frac{\nabla u}u$:

$$\begin{aligned}
\partial_t D(\nu P^t\mid \lambda) &=\partial_t \int (f_t\log f_t)\mathrm d\lambda \\
&= \int (\partial_t f)(1+\log f_t)\mathrm d\lambda \\
&= \int \operatorname{div}(\alpha \nabla f_t-f_t\vec v)(1+\log f_t)\mathrm d\lambda \\
&\overset{\text{IBP}}{=}-\int \langle \alpha \nabla f_t-f_t\vec v,\nabla (1+\log f_t)\rangle \mathrm d\lambda \\
&= \int \langle \vec v,\nabla f_t\rangle \mathrm d\lambda - \int \langle \alpha \nabla f_t,\frac{\nabla f_t}{f}\rangle\mathrm d\lambda \\
&\overset{\text{IBP}}{=} - \int f_t \operatorname{div}\vec v \mathrm d\lambda - \int \alpha \frac{\|\nabla f_t\|^2_2}{f_t}\mathrm d\lambda \\
&= - \int f_t (\operatorname{div}\vec v+\alpha \|\nabla\log f_t\|^2)\mathrm d\lambda
\end{aligned}$$

To summarize,

$$\begin{aligned}
\partial_t D(\nu P^t\mid \lambda) &= - \int f_t (\operatorname{div}\vec v+\alpha \|\nabla\log f_t\|^2_2)\mathrm d\lambda \\
& = - \mathbb E_{\nu P^t}[\operatorname{div}\vec v+\alpha \|\nabla\log f_t\|^2_2] \\
& = - \mathbb E_{\nu P^t}[\operatorname{div}\vec v+\alpha \|D\log f_t\|^2_{\mathrm{op}}].
\end{aligned}$$

In the simple case of scalar diffusivity $\alpha>0$ we can intuitively summarize the components determining the rate of information gain:
* Expected divergence of the velocity field. Positive divergence means expansion and therefore dilusion, diminishing information gain. Negative divergence means compression/concentration, increasing information gain.
* The expected squared norm of the spatial change/derivative of "information gain" $\log f_t$ is a measure of expected total variation. A high expectation means *lots of fine-scale structure*. Intuitively, such structure vanishes quickly under mixing/smoothing, therefore also reduces information gain. For intuition the straight line $y=0$ with the graph of $A \sin(\lambda x)$ with amplitude $A$ and high frequency $\lambda$: both are smooth in the formal sense, but the latter has much more fine-scale structure due to fluctuations, which is diminished by decreasing amplitude. Indeed the expectation is quadratic in both $A,\lambda$.

*Remark.* The quantity $I(\nu\mid \mu)=\mathbb E_{\nu }[\|D\log \frac{\mathrm d\nu}{\mathrm d\mu}\|_\mathrm{op}^2]$ is known as *spatial Fisher information* of the density. Working relative to an equillibrium of a mixing process $(P^t,\pi)$ (e.g. Lebesgue measure relative to heat flow) gives general versions of our equation for the rate of information gain. We shall discuss Fisher information in detail.

When the velocity field has no sources/sinks (a.k.a incompressible) $\operatorname{div}\vec v=0$ we recover the de Bruijn identity

$$\begin{aligned}
\partial_t D(\nu P^t\mid \lambda) &=-\alpha \mathbb E_{\nu P^t}[\|\nabla\log \tfrac{\mathrm d \nu P^t}{\mathrm d\lambda}\|^2_2]\\
&=-\alpha \mathbb E_{\nu P^t}[\|D\log \tfrac{\mathrm d \nu P^t}{\mathrm d\lambda}\|^2_\mathrm{op}].
\end{aligned}$$

Working relative to an equillibrium $\pi\ll \lambda$ incorporates the drift term into the de Bruijn formula, yielding a more elegant formula. Working relative to equillibrium then leads to fancy words like Bakry-Émery calculuss, and Carré du champ (literally "square of field"). Morally, we should work relative to an equillibrium. However, for CLT this leads to a structure known as the Ornstein–Uhlenbeck/Mehler monoid which isn't very appetizing at first glance.

## Translation-invariant mixing

### Convolution and the Fourier transform

When our space is a group it's natural to consider translation invariant Markov kernels $P_{gx}(gA)=P_x(A)$. For intuition take the example $\mathbb Z/n\mathbb Z$ so we have an $n$-state Markov chain with states arranged in a circle. Translation invariance means $P_{i,j}=P_{i+1,j+1}$, so it effectively becomes a univariate function $P_{i-j}$ depending only on the "distance". One could say the dynamics/physics are the same everywhere.

Define $\mu=P_1$ to be the measure at the group's identity element. Then $P_g(A)=P_1(g^{-1}A)=\mu (g^{-1}A)$. Hence a translation invariant kernel is just a family of translations of $\mu=P_1$. In this case $\nu P(A)=\int_G \mu(g^{-1}A)\mathrm d\nu$ is the $\nu$-expected measure of the family of translations of $A$. The integral expression is known as the convolution $\nu\ast\mu$. It is abstractly characterized as the pushforward of $\nu \otimes \mu$ along the group operation $G\times G\to G$. Consequently, convolution is always associate, and becomes commutative for abelian groups.

To summarize, when $P$ is translation invariant then $\nu P=\nu \ast P_1=\nu\ast\mu$. In continuous time we require translation invariance of all $P^t$. Defining $P_1^t=\mu_t$ we have $\nu P^t=\nu\ast \mu_t$, and also the monoid laws $\mu_0=\delta_1$ and $\mu_{s+t}=\mu_s\ast \mu_t$.

Now we'll see what translation invariance imposes on the forward time evolution equation for densities $\partial_t \frac{\mathrm d \nu P^t}{\mathrm d\pi}=L^\dagger[\pi]\frac{\mathrm d \nu P^t}{\mathrm d\pi}$, and take the initial condition to be $\nu=\mu_0$.

* First, the monoid laws simplify the equation to the form $\partial_t \frac{\mathrm d \mu_t}{\mathrm d\pi}=L^\dagger[\pi]\frac{\mathrm d \mu_t}{\mathrm d\pi}$.
* Second, translation invariance actually forces the differential operator $L^\dagger[\pi]$ to have constant coefficients, i.e. to be given by a convolution operator, where the convolution kernel is a distribution, typically encoding a differential operator $\partial_t \frac{\mathrm d \mu_t}{\mathrm d\pi}=\eta^\dagger[\pi]\ast\frac{\mathrm d \mu_t}{\mathrm d\pi}$.

From here on out one can kick ass with the Fourier transform which diagonalizes translation invariant operators on $L^2(\mathbb R^n)$.

The Radon-Nikodym map relative to a translation invariant (i.e. Haar) measure $\lambda$ preserves convolutions (verifiable by computation using Fubini).

$$
\frac{\mathrm d(\nu \ast \mu)}{\mathrm d\lambda}=\frac{\mathrm d \nu}{\mathrm d\lambda}\ast\frac{\mathrm d\mu}{\mathrm d\lambda},\quad \nu,\mu\ll\lambda.
$$

This lets us move from measure monoids to density monoids.

### Heat equation: Gaussian density and de Bruijn 

Consider the heat equation. (We'll just do 1D since we'll focus on 1D CLT.)

$$\begin{aligned}
\partial_t u(x,t) &=\tfrac 12 \partial_{xx}u(x,t), & (x,t)\in \mathbb R\times [0,\infty ) \\
u(x,0) &= f(x).
\end{aligned}$$

One may verify it is solved by convolving the initial condition with Gaussian density $\Gamma_t(x)=\frac{1}{\sqrt{2\pi t}}\exp(-\frac{x^2}{2t})$, so that

$$
u(x,t)=f\ast \Gamma_t (x).
$$

Backtracking our story, the second derivative is self-adjoint on $L^2(\lambda)$ and coefficients are constant, so we can diagonalize via Fourier to discover the multiplier $\omega ^2$ which recovers Gaussian density.

We can now specialize the de Bruijn identity.

**de-Bruijn identity for heat equation.** Write $\gamma_t$ for the Gaussian measure with density $\Gamma_t(x)=\frac{1}{\sqrt{2\pi t}}\exp(-\frac{x^2}{2t})$. Given an initial distribution $\nu$ we have

$$
\begin{aligned}
\partial_t D(\nu \ast\gamma_t \mid \lambda)& =-\tfrac 12 I(\nu\ast \gamma_t\mid \lambda)\\
&=-\tfrac 12 \mathbb E_{\nu \ast\gamma_t}[(\partial_x \log (\tfrac{\mathrm d \nu\ast\gamma_t}{\mathrm d\lambda}))^2] \\
& = -\tfrac 12 \int \tfrac{\mathrm d \nu\ast\gamma_t}{\mathrm d\lambda}(\partial_x \log (\tfrac{\mathrm d \nu\ast\gamma_t}{\mathrm d\lambda}))^2 \mathrm d\lambda,
\end{aligned}\quad t>0.
$$

Consequently, by entropy relative to the matching Gaussian

$$
\partial_t D(\nu \ast\gamma_t \mid \gamma_{\sigma^2+t}) =\tfrac 12\tfrac{1}{\sigma^2+t}-\tfrac 12 \mathbb E_{\nu \ast\gamma_t}[(\partial_x \log (\tfrac{\mathrm d \nu\ast\gamma_t}{\mathrm d\lambda}))^2],\quad t>0.
$$

Note $\gamma_{\sigma^2+t}=\gamma_{\sigma^2}\ast\gamma_t$, so the data-processing inequality ensures the time-derivative is non-positive (dissipation of information gain).

*Remark.* If $\nu\ll\lambda$ we can simplify using the fact Radon-Nikodym preserves convolution $\frac{\mathrm d(\nu \ast \mu)}{\mathrm d\lambda}=\frac{\mathrm d \nu}{\mathrm d\lambda}\ast\frac{\mathrm d\mu}{\mathrm d\lambda}$. However, smoothing a discrete $\nu \not \ll \lambda$ is often the whole point.

*Proof.* The relative entropy assertion is just a computation $\partial_t (1+\log((\sigma^2+t)2\pi))=\frac{2\pi}{2\pi(\sigma^2+t)}$.

**Integral de-Bruijn identities.** 

$$\begin{aligned}
D(\nu\ast \gamma_t\mid \gamma_{\sigma^2+t}) &=\frac 12\int_t^\infty I(\nu\ast\gamma_ s\mid \lambda)-\tfrac{1}{\sigma^2+s}\;\mathrm ds,\quad t>0 \\
D(\nu \mid \gamma_{\sigma^2}) &= \frac 12 \int_0^\infty I(\nu\ast\gamma_ t\mid \lambda)-\tfrac{1}{\sigma^2+t}\;\mathrm dt.
\end{aligned}$$

*Proof.* We prove the first assertion by computation. The fundamental theorem of calculus means the integral on $[t,T]$ is the difference

$$
D(\nu\ast\gamma_T\mid \gamma_{\sigma^2+T})-D(\nu\ast \gamma_t\mid \gamma_{\sigma^2+t}).
$$

Hence it suffices to show $D(\nu\ast\gamma_T\mid \gamma_{\sigma^2+T})\overset{T\to\infty}{\longrightarrow}0$. We already know this quantity is non-negative, so it suffices to prove it's bounded from below by a quantity that tends to zero. Now compute, using the information gain (weakly) increases upon conditioning (the "reverse reading" of the data-processing inequality). Specifically, the conditional of $\nu\ast\gamma_T$ on $\nu$ is just a shift of the Gaussian. More precisely, the kernel $P$ acting via $\nu P=\nu \ast \gamma_t$ satisfies $P_x(B)=\gamma(B-x)$. Anyway, the computation:

$$\begin{aligned}
0\leq D(\nu\ast\gamma_T\mid \gamma_{\sigma^2+T}) &= D(\nu\ast\gamma_T\mid\lambda) - D(\gamma_{\sigma^2+T}\mid\lambda)\\
&=D(\nu\ast\gamma_T\mid \lambda)+\tfrac 12(1+\log2\pi(\sigma^2+T)) \\
& \leq -\tfrac 12(1+\log(2\pi T)) +\tfrac 12(1+\log2\pi(\sigma^2+T)) \\
& = \frac 12 \log \left( 1+\frac{\sigma^2}{T} \right)\overset{T\to\infty}{\longrightarrow}0
\end{aligned}$$

For the second assertion, split in two using the matching Gaussian formula and argue for each summand separately. For the differential entropy term I think Monotone Convergence satisfied while for the other you should probably use Dominated Convergence.

### Stam's inequality and characterization of Gaussians

Stam's inequality is crucial to our proof of the entropic CLT.

Write $f=\frac{\mathrm dP_X}{\mathrm d\lambda}$ and $\rho_X=\frac{\mathrm d}{\mathrm dx}\log f=\frac{f^\prime}{f}$.

**Theorem.** Let $X_1,\dots ,X_n$ be independent variables with $C^1$ positive (!) densities. Write $Y=\sum_i a_i X_i$.
1. Suppose $\varphi$ is compactly supported. Then $$\mathbb E\left[\varphi(Y)\rho_{X_i}(X_i)\right]=-a_i\mathbb E\varphi^\prime (Y).$$ In particular if $f\in C^2$ and $f^\prime$ decays at $\pm \infty$ then $$\langle \rho_Y(Y),\rho_{X_i}(X_i)\rangle=-a_i\|\rho_Y(Y)\|^2.$$
2. (Weighted Stam's inequality.) We have for any $a_1,\dots ,a_n\in \mathbb R$ the following inequality.
$$
\frac 1{\|\rho_Y(Y)\|^2}\geq \sum_{i=1}^n \frac{a_i^2}{\|\rho_{X_i}(X_i)\|^2}.
$$
whence also (by Cauchy-Schwartz - see proof below)
$$
\|\rho_Y(Y)\|^2 \sum_i a_i^2 \leq \sum_i a_i^2\|\rho_{X_i}(X_i)\|^2.
$$
3. (Score projection property for linear combinations.) We have $$\rho_{Y}(Y)=\frac 1{\sum_i a_i}\mathbb E\left[ \sum_i \rho_{X_i}(X_i) \mid Y \right].$$

⚠️️ **Remark.** In fact the assumption of a positive sufficiently smooth density is not required. The general case is reduced to the smooth one via Gaussian smoothing, which we shall use for our proof of CLT. Nevertheless, we shall only require Stam's inequality for the positive smooth case.

*Proof.* First compute via integration-by-parts using compact support. Then use the characterization of conditional expectation on the $L^2$-dense subspace of compactly supported functions. For the first assertion, independence implies the joint density is the product of the marginal ones. Now we can compute via Fubini. We'll write the case $i=1$ for convenience, but without loss of generality.

$$\begin{aligned}
\mathbb E[\varphi(Y)\rho_{X_i}(X_i)] &=\int_{\mathbb R^n}\varphi(\sum_i a_i x_i)\rho_{X_1}(x_1)f_{X_1}(x_1)\cdots f_{X_n}(x_n)\mathrm dx_1\cdots \mathrm dx_n \\
&= \int_{\mathbb R^{n-1}} f_{X_2}(x_2)\cdots f_{X_n}(x_n) \left[ \int_{\mathbb R} \varphi(\sum_i a_i x_i)f^\prime_{X_1}(x_1)\mathrm dx_1 \right] \mathrm dx_2 \cdots \mathrm dx_n\\
&= \int f_{X_2}(x_2)\cdots f_{X_n}(x_n) \left[ -\int a_i\varphi^\prime(\sum_i a_i x_i)f_{X_1}(x_1)\mathrm dx_1 \right] \mathrm dx_2 \cdots \mathrm dx_n \\
& =-\int_{\mathbb R^n} a_i\varphi^\prime(\sum_i a_i x_i)f_{X_1}(x_1)\cdots f_{X_n}(x_n)\mathrm dx_1\cdots \mathrm dx_n \\
& =-a_i\mathbb E\varphi^\prime(Y),
\end{aligned}$$

where the third equality is integration by parts using compact support of $\varphi$. For the second part of the first assertion, use the definition $\rho=\frac{f^\prime}f$ and integrate by parts using the decaying assumption. 

For Stam's inequality, consider the following quadratic form

$$\begin{aligned}
0\leq Q(\lambda_1,\dots ,\lambda_n) &=\left\Vert \sum_i \lambda_i \rho_{X_i}(X_i)-\rho_{Y}(Y) \right\Vert ^2 \\
&= \sum_i \lambda_i^2\|\rho_{X_i}(X_i)\|^2-2\sum_i\lambda_i \langle\rho_{X_i}(X_i),\rho_Y(Y)\rangle +\|\rho_Y(Y)\|^2 \\
&= \sum_i \lambda_i^2\|\rho_{X_i}(X_i)\|^2-2\|\rho_Y(Y)\|^2(\sum_i \lambda_i a_i) +\|\rho_Y(Y)\|^2.
\end{aligned}$$

For each $\lambda_i$ complete the square separately.

$$
\lambda_i^2\|\rho_{X_i}(X_i)\|^2-2\lambda_i a_i\|\rho_Y(Y)\|^2=\left(\lambda_i\|\rho_{X_i}(X_i)\|-a_i\frac{\|\rho_Y(Y)\|^2}{\|\rho_{X_i}(X_i)\|}\right)^2-\left( a_i\frac{\|\rho_Y(Y)\|^2}{\|\rho_{X_i}(X_i)\|}\right)^2.
$$

Now we can rewrite

$$
Q(\lambda) = \sum_i \left(\lambda_i\|\rho_{X_i}(X_i)\|-a_i\frac{\|\rho_Y(Y)\|^2}{\|\rho_{X_i}(X_i)\|}\right)^2 +\left(1- \sum_i a_i^2\frac{\|\rho_Y(Y)\|^2}{\|\rho_{X_i}(X_i)\|^2}\right)\|\rho_Y(Y)\|^2.
$$

Obviously the minimum is attained at $\lambda_i=a_i\frac{\|\rho_Y(Y)\|^2}{\|\rho_{X_i}(X_i)\|^2}$. Plugging this in we obtain the inequality $\frac 1{\|\rho_Y(Y)\|^2}\geq \sum_i \frac{a_i^2}{\|\rho_{X_i}(X_i)\|^2}$ as desired.

For the second inequality, apply Cauchy-Schwarz in $\mathbb R^n$ to the vectors $$(\frac{a_i}{\|\rho_{X_i}(X_i)\|})_i,(a_i \|\rho_{X_i}(X_i)\|)_i$$.

We turn to the score projection formula. Summing up the first assertion over $i$ we find the following equality holds for all compactly supported $\varphi$.

$$\begin{aligned}
\mathbb E\varphi^\prime (Y) &=\frac{1}{\sum_i a_i} \sum_i\mathbb E\left[\varphi(Y)\rho_{X_i}(X_i)\right] \\
&= \mathbb E\left[\varphi(Y)\frac{1}{\sum_i a_i}\sum_i\rho_{X_i}(X_i)\right]
\end{aligned}$$

On the other hand, unpacking the definition $\rho=\frac{f^\prime}{f}$ and integrating by parts gives the following for any compactly supported $\varphi$.

$$\mathbb E\varphi^\prime (Y)=\mathbb E[\varphi(Y)\rho(Y)]$$

But the space of compactly supported functions is $L^2$-dense whence $\rho(Y)=\mathbb E\left [\frac{1}{\sum_i a_i}\sum_i\rho_{X_i}(X_i)\mid Y\right]$ as desired.

The following characterization of Gaussians via Fisher information crucially relies on positivity of densities.

**Stam characterization of Gaussians.** The following are equivalent.
1. Equality in Stam's inequality $\frac 1{\|\rho_Y(Y)\|^2}= \sum_{i=1}^n \frac{a_i^2}{\|\rho_{X_i}(X_i)\|^2}$.
2. Equality in $\|\rho_Y(Y)\|^2 \sum_i a_i^2 \leq \sum_i a_i^2\|\rho_{X_i}(X_i)\|^2.$
3. $X_1,\dots ,X_n$ are all Gaussian.

Consequently, if $X_1,\dots,X_n$ are i.i.d and satisfy $I(X_1)=I\left( \frac{X_1+\cdots+X_n}{\sqrt n} \right)$ then they're all Gaussian.

*Proof.* Equivalence of the first two conditions is due to their relation by Cauchy-Schwarz, which has equality iff the vectors are linearly dependent. Equality is achieved in the Gaussian case by computation. Hence it suffices to prove equality implies Gaussian distributions. Continuing the proof of Stam's inequality, we have equality precisely when the minimal value of $Q$ equals zero
$$
0=\left\Vert \sum_i \lambda^\ast_i \rho_{X_i}(X_i)-\rho_{Y}(Y) \right\Vert
$$

whence $\rho_Y(Y)=\sum_i \lambda^\ast_i \rho_{X_i}(X_i) $. Thus the RHS is a deterministic function of the linear combination $Y=\sum_i a_i X_i$. Morally, we'd like to lift this equality to the coordinate functions of Euclidean space, so that $\rho_Y\left(\sum_i a_i x_i \right)=\sum_i\lambda_i^\ast\rho_{X_i}(x_i)$. Why? Because this means the scores $\rho_{X_i}(x_i)$ are all affine, whence the $X_i$ are Gaussian. To see this, apply $\frac{\partial}{\partial x_i}$ to get $\rho_Y^\prime\left(\sum_i a_i x_i \right)=\frac 1{a_i}\lambda_i^\ast\rho^\prime_{X_i}(x_i)$ for all $i$, meaning each $\rho^\prime_{X_i}(x_i)$ is constant.

*Remark.* Incidentally $\lambda^\ast_i= a_i\frac{\|\rho_Y(Y)\|^2}{\|\rho_{X_i}(X_i)\|^2}$, but these are nevertheless constants. Writing out the equality $\rho_Y(Y)=\sum_i a_i \frac{\|\rho_{Y}(Y)\|^2}{\|\rho_{X_i}(X_i)\|^2} \rho_{X_i}(X_i)$ is potentially misleading because one is tempted to also lift the dependence on the random variables inside the norms.

So how to lift? Since the densities are strictly positive we, all distributions are equivalent to Lebesgue measure. We only need $\lambda \ll  P_X$: if $X$ has density $f>0$ then 

$$
P_X\lbrace g(x)\neq h(x)\rbrace =\int_{\lbrace g(x)\neq h(x) \rbrace}f\mathrm d \lambda,
$$

so if the LHS equals zero we must have $\lambda \lbrace g(x)\neq h(x) \rbrace=0$. This recovers our equality almost-everywhere, which is good enough for differentiation since densities are also defined almost-everywhere.

For the second assertion, the identical distribution gives us $I\left( \frac{X_1+\cdots+X_n}{\sqrt n} \right)^{-1}=I(X_1)=\sum_{i=1}^n \left( \frac 1{\sqrt n}\right) ^2 \frac 1{I(X_i)}$ so we can apply the equality criterion.

We cannot omit the beautiful application by [Itoh](https://www.ism.ac.jp/editsec/aism/pdf/041_1_0009.pdf) (not Itô) of Stam's characterization to prove a famous result by Kac:

**Rotational characterization of Gaussians.** Let $X,Y$ be independent. Suppose the rotation of the vector $\begin{pmatrix} X \\ Y \end{pmatrix}$ by $\theta\notin \frac{\pi}{2}\mathbb Z$ remains independent. 

$$
\begin{pmatrix}
\cos \theta & \sin \theta\\
-\sin \theta & \cos \theta
\end{pmatrix}
\begin{pmatrix} X \\
Y
\end{pmatrix} =
\begin{pmatrix}
\cos \theta X + \sin \theta Y\\
-\sin \theta X + \cos \theta Y
\end{pmatrix}
$$

Then all components of both vectors are Gaussian.

**Remark.** Independence means a separable joint density $f_{X,Y}(x,y)=f_X(x)f_Y(y)$. Evidently this expression is sensitive to coordinates and would typically break under rotation. To build intuition, consider $X,Y$ i.i.d uniform and rotate by $\frac{\pi}{4}$.

*Proof.* The inverse of rotation by $\theta$ is rotation by $-\theta$, represented by the matrix $\begin{pmatrix} \cos \theta & -\sin \theta\\ \sin \theta & \cos \theta \end{pmatrix}$. Now apply Stam's inequality four times: twice to get the equations

$$\begin{aligned}
I(\cos \theta X + \sin \theta Y) &\leq \cos^2\theta I(X)+\sin^2\theta I(Y)  \\
I(-\sin \theta X + \cos \theta Y) &\leq \sin^2\theta I(X)+\cos^2\theta I(Y),
\end{aligned}$$ 

and deduce (by addition)

$$
I(\cos \theta X + \sin \theta Y)+I(-\sin \theta X + \cos \theta Y)\leq I(X)+I(Y),
$$

and twice more for the reverse to deduce equality.

For $\theta\in \frac{\pi}{2}\mathbb Z$ this is a tautology. Otherwise, some extra manipulations show $I(X)=I(Y)$ (exercise) from which all equalities follow and we can apply the Stam equality criterion.

### Normalized averages

**Subadditivity and monotonicity-along multiples.** Let $$(X_i)_{i\in\mathbb N}$$ be a sequence of independent random variables. Write $$S_n=\frac{X_1+\cdots +X_n}{\sqrt n}$$. The sequence $$n\mapsto \|\rho_{S_n}(S_n)\|^2$$ converges. Moreover, it is weakly decreasing along every subsequence of multiples.

*Proof.* Use the consequence of weighted Stam's inequality for two summands with $a_1=\sqrt{\frac m{m+n}}$ and $a_2=\sqrt{\frac n{m+n}}$, and multiply by $m+n$ to obtain:

$$
(m+n)\|\rho_{S_{m+n}}({S_{m+n}})\|^2\leq m \|\rho_{S_{m}}({S_{m}})\|^2+ n\|\rho_{S_{n}}({S_{n}})\|^2.
$$

Hence the sequence $n\mapsto n\|\rho_{S_n}(S_n)\|^2$ is subadditive. By Fekete's lemma, the sequence $n\mapsto \|\rho_{S_n}(S_n)\|^2$ converges. We leave the second assertion to the reader (prove the dyadic case).

# Entropic Central Limit Theorem

## Statement

**Classical Central Limit Theorem.** Let $(X_i)_{i\geq 1}$ be i.i.d with mean $\mu$ and variance $\sigma^2<\infty$. Consider $n$-average $\overline X_n=\frac{X_1+\cdots+X_n}{n}$. As $n\to \infty$, the deviation of the average from the mean *in units of its standard deviation* converges to a standard Gaussian distribution:

$$
\frac{\overline X_n-\mu}{\sigma/\sqrt n}=\frac{\overline X_n-\mu}{\|\overline X_n\|_2}\overset{n\to\infty}{\Longrightarrow} N(0,1).
$$

**Entropic Central Limit Theorem.** Let $(X_i)_{i\geq 1}$ be i.i.d with mean $\mu$ and variance $\sigma^2<\infty$.

$$
D\left(\frac{\overline X_n-\mu}{\sigma/\sqrt n}\mid \gamma_1\right)<\infty\text{ for some }n\implies D\left(\frac{\overline X_n-\mu}{\sigma/\sqrt n}\mid \gamma_1\right)\overset{n\to \infty}{\longrightarrow}0.
$$

Note finiteness precludes $X_i$ from being discrete, so recovery of the classical CLT is not immediate.

## Proof

Our proof is an expanded exposition of the aptly named article ["A short information theoretic proof of CLT"](https://www-syscom.univ-mlv.fr/~vignat/Signal/CLT.pdf).

Write $J(\nu \mid \gamma_1)=I(\nu\ast\gamma_t\mid \lambda)-\tfrac{1}{1+t}$ for standardized Fisher information.

Assuming expected information gain is finite for $m$ we have by integral de Bruijn

$$
\infty > D(S_m\mid \gamma_1) = \frac 12 \int_0^\infty  \frac{J(S_{m,s}\mid \gamma_1)}{1 +s}\mathrm ds,
$$

so $t\mapsto J(S_{m,t}\mid \gamma_1)$ is integrable. The sequence of Fisher informations is convergent (by Fekete), and its subsequences-along-multiples are weakly decreasing. Hence the $m^\text{th}$ Fisher information dominates the tail of the associated subsequence on any interval $[t,\infty),t>0$. Since the sequence is convergent for each $t$ we have pointwise convergence $J(S_{n,t}\mid \gamma_1)\to g\in L^1$ of functions $[0,\infty)\to [0,\infty)$. Hence, by Dominated Convergence and the extended de Bruijn identity we have

$$
\lim_n D(S_{n}\mid\gamma_1)=\lim_n \int_0^\infty \frac{J(S_{n,s}\mid\gamma_1)}{1 +s}\mathrm ds=\int_0^\infty g.
$$

Thus it suffices to prove $g=0$. Another application of Dominated Convergence to de Bruijn gives
$$
\lim_n D(S_{n,t}\mid \gamma_1)=\lim_n \int_t^\infty \frac{J(S_{n,s}\mid \gamma_1)}{1 +s}\mathrm ds=\int_t^\infty g.
$$

Hence, to prove $g=0$ it suffices to prove the LHS is zero for all $t>0$ (as the integral of $g$ will be zero on every interval).

The sublevel set $\lbrace \nu \mid D(\nu\mid \mu) \leq c \rbrace$ is sequentially compact. Since $g\in L^1$ we know the tail of the sequence $\lim_n D(S_{n}\mid \gamma_1)$ lies in such a sublevel set. Hence there's a subsequence $S_{n_m}$ whose distribution weakly converges to that of some $Y$. For $t>0$, smoothing ensures pointwise convergence of densities of $S_{n_m,t}$ to the density of $Y_t$. Moreover, the densities of $S_{2n_m,t}$ converge to the density of $\frac {Y_t+\tilde Y_t}{\sqrt 2}$ where $\tilde Y$ is an i.i.d copy of $Y$. Since the densities are convergent and uniformly bounded, the continuity of expected information gain ensures

$$\begin{aligned}
D(S_{n_m,t} & \mid \gamma_1)\overset{m}{\to} D(Y_t\mid \gamma_1), \\
D(S_{2n_m,t} & \mid \gamma_1)\overset{m}{\to} D\left(\frac {Y_t+\tilde Y_t}{\sqrt 2}\mid \gamma_1\right).
\end{aligned}$$

But then $D(Y_t\mid \gamma_1)=D\left(\frac {Y_t+\tilde Y_t}{\sqrt 2}\mid \gamma_1\right)$. Using the differential de Bruijn identity for $w>t$ gives equality of standardized Fisher informations of $Y_t,\frac {Y_t+\tilde Y_t}{\sqrt 2}$. Hence $Y_t$ is Gaussian by the Stam characterization. This in turn implies the standardized Fisher information equals zero, which also equals $g$.

# Fisher information

Fisher information comes in two flavors: parametric and spatial. I wrote this section mostly to help myself disambiguate and understand how both notions are related.

* While expected information gain is relational between *two* hypotheses, parametric Fisher information is a associated to a *parameterized manifold* $(\nu_\theta)_\theta$ of hypotheses, i.e. a family (locally) parameterized over Euclidean space. The filthy parameterization-dependence can be removed by considering the invariant Fisher information *metric* on the manifold itself; we defer this topic to a later point so we can get a move on.
* Similarly to expected information gain, *spatial* Fisher information is also relative. However, the probability space in this context is typically a structured group and the reference measure is Haar, as opposed to a prior hypothesis.

Fisher information of a family $$(\nu_\theta)_\theta$$ is a non-negative function $$I[(\nu_\theta)_\theta]$$ on the parameter space. It has several manifestations:

1. (Density-based, general.) If there's a $\theta$-independent dominating measure $\nu_\theta\ll \mu$ then Fisher information is defined as $\nu_\theta$-expected squared rate of information gain. A higher value at $\theta_0$ means nearby hypotheses are easily distinguishable by observation. Note the expectation is too coarse to distinguish between local concentration of the rate of information gain in some region of $\Omega$, and a more uniform rate.
$$
I[(\nu_\theta)_\theta]=\mathbb E_{\nu_{\theta}}\left[\left( \frac{\partial}{\partial \theta}\log \frac{\mathrm d\nu_ {\theta} }{\mathrm d \mu}\right)^2\right]
$$
This quantity is indeed independent from $\mu$: any two $\theta$-independent dominating measures $\mu_1,\mu_2$ give the same result because the difference $\log \frac{\mathrm d\nu_ {\theta} }{\mathrm d \mu_1}-\log \frac{\mathrm d\nu_ {\theta} }{\mathrm d \mu_2}=\log \frac{\mathrm d\mu_1 }{\mathrm d \mu_2}$ is $\theta$-independent and therefore dies under $\frac{\partial}{\partial \theta}$.
2. (Density-based, general.) In the above setting, careless differentiation of the formula $\int\frac{\mathrm d\nu_\theta}{\mathrm d\mu}\mathrm d\mu=1$ twice shows Fisher information also equals minus the $\nu_\theta$-expected *curvature* of information gain. Intuitively, high expected squared (!) rate disregard sign cancellations and can therefore only come from highly curved information content.

$$
I[(\nu_\theta)_\theta]=-\mathbb E_{\nu_\theta}\left[\frac{\partial^2}{\partial\theta^2}\log \frac{\mathrm d\nu_\theta}{\mathrm d\mu}\right]
$$

A careful computation includes also boundary terms coming from the changes in the support. In such cases, Fisher information is typically infinite. To illustrate, consider the family $\mathrm{U}[0,\theta]$ of uniform distributions. A single observation outside the interval $[0,\theta_1]$ immediately rules out $\mathrm U[0,\theta_1]$ with infinite certainty. The above identity is valid e.g. when the family of densities $\frac{\mathrm d\nu_\theta}{\mathrm d\mu}$ has $\theta$-independent supports.
3. (Intrinsic, Markov monoid.) When our family is a Markov monoid $(\nu_t)_t$ with dominating stationary distribution $\nu_t\ll\pi$ then de Bruijn's identity asserts Fisher information is the rate of expected information loss by due to blurring.
$$
\frac{\mathrm d}{\mathrm d t}D(\nu_t\mid \pi)=-I[(\nu_t)_t]
$$
4. (Intrinsic, general.) Suppose there's a neighborhood of $\nu_{\theta_0}$ in which it dominates. Then the Fisher information at $\theta_0$ is the curvature of information gain *on the diagonal*.
$$I[(\nu_\theta)_\theta](\theta_0)=\left. \frac{\mathrm d^2}{\mathrm d\theta^2}\right|_{\theta_0} D(\nu_{\theta},\nu_{\theta_0})
$$
This characterization of Fisher information exhibits it as the leading term in the Taylor expansion of information gain. It is also closest to the coordinate free Fisher information metric. Although we won't use this formula, we state it for completeness.

The first and last definitions work for vector-valued parameters and essentially coincide: if the local domination condition from the third definition holds everywhere and the space is second countable then taking a countable subcover and defining $\mu=\sum_k 2^{-k}\nu_{\theta_k}$ gives a globally dominating measure. The second definition assumes significant extra structure relevant only for scalar $\theta$, usually treated as a time parameter in this context.

## Parametric Fisher information

### Density-based viewpoint

Consider a parameterized manifold of hypotheses $(\nu_\theta)_\theta$. We work locally so the parameter $\theta$ takes values in Euclidean space. The parameter could be mean, variance, the Bernoulli $p$, or a vector of such quantities pertaining to the distribution. We'll focus on the one-dimensional case to get the gist of it. The generalization is "just" multivariate calculus.

One way to compare nearby hypotheses in the family $\nu_\theta$ is to consider the *rate of information gain* relative to some fixed prior $\mu$ as $\theta$ varies $\frac{\partial}{\partial \theta}\log \frac{\mathrm d\nu_ \theta }{\mathrm d \mu}$. At each fixed $\theta$ the value $\frac{\partial}{\partial \theta^\prime}\;\mid_{\theta}\log \frac{\mathrm d\nu_ {\theta^\prime} }{\mathrm d \mu}$ is a real-valued random variable. As sketched at the beginning of the chapter, this random variable is independent of choice of dominating measure $\nu_\theta\ll \mu$. What does the value of this random variable at $\omega$ mean?
* A critical point (value zero) means there is no information to be gained about outcome $\omega$ by an infinitesimal perturbation.
* A positive value means $\theta+\varepsilon$ is more informative about outcome $\omega$.
* A negative value means $\theta-\varepsilon$ is more informative about outcome $\omega$.
* A large absolute value means hypotheses near $\theta$ offer wildly different information gains about outcome $\omega$ relative to the prior.

We want to globalize over the probability space and quantify the "total" rate of information gain at $\theta$, not just at a particular outcome. To this end we use the $L^2(\nu_\theta)$-norm of the random variable $\frac{\partial}{\partial \theta^\prime}\;\mid_{\theta}\log \frac{\mathrm d\nu_ {\theta^\prime} }{\mathrm d \mu}$, which sums the squares of the local rates of information gain. A small $L^2$-norm at $\theta$ means the rate of information gain at $\theta$ is typically miniscule, while a large $L^2$-norm means typically substantial information gain. Note the norm is too coarse to determine whether the phenomenon is concentrated locally at some $\omega$ or distributed across the entire probability space.

By the Bayesian interpretation of expectation we anticipate $\mathbb E_{\nu_\theta}\left[\frac{\partial}{\partial \theta^\prime}\;\mid_{\theta}\log \frac{\mathrm d\nu_ {\theta^\prime} }{\mathrm d \mu}\right]=0$. Indeed in a world modeled by $\nu$, the information gain relative to any prior is maximized by $\nu $ itself, and the derivative at this critical point is zero. The proof is simple using properties of the Radon-Nikodym derivative: we know expected information gain is non-negative, so $0\leq \mathbb E_\nu\left[\log \frac{\mathrm d\nu}{\mathrm d\eta} \right]=\mathbb E_\nu\left[\log \frac{\mathrm d\nu}{\mathrm d\mu}\right]-\mathbb E_\nu \left[ \log \frac{\mathrm d\eta}{\mathrm d\mu} \right]$. The result $\mathbb E_{\nu_\theta}\left[\frac{\partial}{\partial \theta^\prime}\;\mid_{\theta}\log \frac{\mathrm d\nu_ {\theta^\prime} }{\mathrm d \mu}\right]=0$ is sometimes referred to as the first Bartlett identity. From it we deduce the $L^2(\nu_\theta)$-norm equals the variance!

**Definition.** Fix a family $$(\nu_\theta)_\theta$$ of probability distributions. Assume there exists a $\theta$-independent dominating measure $\nu_\theta\ll \mu$. The *Fisher information* of a family $$(\nu_\theta)_\theta$$ is the real-valued function on the parameter space whose value at $\theta_0$ is the squared $$L^2(\nu_{\theta_0})$$-norm of the rate of information gain at $$\theta_0$$.

$$I[(\nu_\theta)_\theta]=\mathbb E_{\nu_{\theta}}\left[\left( \frac{\partial}{\partial \theta}\log \frac{\mathrm d\nu_ {\theta} }{\mathrm d \mu}\right)^2\right]=\operatorname{Var}_{\nu_{\theta}}\left[ \frac{\partial}{\partial \theta}\log \frac{\mathrm d\nu_ {\theta} }{\mathrm d \mu} \right].$$

We arrive at the second Bartlett identity, whose intuition was outlined at the beginning of the chapter.

**Parametric second Bartlett identity.** Fisher information equals minus expected curvature of information gain. Assume the family $(\nu_\theta)_\theta$ has $\theta$-invariant support. Then we have an equality
$$
I[(\nu_\theta)_\theta]=\mathbb E_{\nu_\theta}\left[\left(\frac{\partial}{\partial \theta}\log \frac{\mathrm d\nu_\theta}{\mathrm d\mu}\right)^2\right]=-\mathbb E_{\nu_\theta}\left[\frac{\partial^2}{\partial\theta^2}\log \frac{\mathrm d\nu_\theta}{\mathrm d\mu}\right].
$$

*Proof.* Start with $\int \frac{\mathrm d\nu_\theta}{\mathrm d\mu}d\mu=1$. Applying $\frac{\mathrm d^2}{\mathrm d\theta^2}$ gives $\int \frac{\mathrm d^2}{\mathrm d\theta^2}\frac{\mathrm d\nu_\theta}{\mathrm d\mu}d\mu=0$; here we used the assumption on supports to kill boundaty flux terms when differentiating the integral. The reader may verify $\frac{\partial^2}{\partial\theta^2}\frac{\mathrm d\nu_\theta}{\mathrm d\mu}=\frac{\mathrm d\nu_\theta}{\mathrm d\mu}((\frac{\partial}{\partial \theta}\log \frac{\mathrm d\nu_\theta}{\mathrm d\mu})^2+\frac{\partial^2}{\partial\theta^2}\log \frac{\mathrm d\nu_\theta}{\mathrm d\mu})$. Hence $\mathbb E_{\nu_\theta}[(\frac{\partial}{\partial \theta}\log \frac{\mathrm d\nu_\theta}{\mathrm d\mu})^2]=-\mathbb E_{\nu_\theta}[\frac{\partial^2}{\partial\theta^2}\log \frac{\mathrm d\nu_\theta}{\mathrm d\mu}]$ as desired.

The proof shows why expectation is required and the equality is not local on $\Omega$: as we change $\theta$, mass inflows/outflows into certain areas may speed up and slow down. Expectation cancels out such effects due to conservation of mass.

### Intrinsic viewpoint: curvature on the diagonal of expected information gain

**Proposition.** Suppose $\nu_{\theta_0}$ is locally dominating. Then Fisher information at $\theta_0$ equals the curvature on the diagonal of $\nu_\theta$-expected information gain.
$$I[(\nu_\theta)_\theta](\theta_0)=\left. \frac{\mathrm d^2}{\mathrm d\theta^2}\right|_{\theta_0}\mathbb E_{\nu_\theta}\left[ \log \frac{\mathrm d\nu_\theta}{\mathrm d\nu_{\theta_0}}\right]
$$

*Proof.* We'll write $\theta^\prime$ instead of $\theta_0$ for legibility. The equality of the first two formulas is a tiresome but straightforward computation.

$$\begin{aligned}
\frac{\mathrm d^2}{\mathrm d\theta^2}\mathbb E_{\nu_\theta}\left[ \log \frac{\mathrm d\nu_\theta}{\mathrm d\nu_{\theta^\prime}}\right] &= \frac{\mathrm d^2}{\mathrm d\theta^2}\mathbb E_{\nu_{\theta^\prime}}\left[\frac{\mathrm d\nu_\theta}{\mathrm d\nu_{\theta^\prime}}\log \frac{\mathrm d\nu_\theta}{\mathrm d\nu_{\theta^\prime}}\right] \\
& = \mathbb E_{\nu_{\theta^\prime}}\left[ \frac{\mathrm d^2}{\mathrm d\theta^2}\left(\frac{\mathrm d\nu_\theta}{\mathrm d\nu_{\theta^\prime}}\log \frac{\mathrm d\nu_\theta}{\mathrm d\nu_{\theta^\prime}}\right)\right] \\
& = \mathbb E_{\nu_{\theta^\prime}}\left[ \frac{\partial}{\partial \theta}\left(\frac{\partial}{\partial \theta}\frac{\mathrm d\nu_\theta}{\mathrm d\nu_{\theta^\prime}}\cdot\log \frac{\mathrm d\nu_\theta}{\mathrm d\nu_{\theta^\prime}}+\frac{\partial}{\partial \theta}\frac{\mathrm d\nu_\theta}{\mathrm d\nu_{\theta^\prime}}\right) \right] \\
& = \mathbb E_{\nu_{\theta^\prime}}\left[\frac{\mathrm d^2}{\mathrm d\theta^2}\frac{\mathrm d\nu_\theta}{\mathrm d\nu_{\theta^\prime}}\cdot\log \frac{\mathrm d\nu_\theta}{\mathrm d\nu_{\theta^\prime}}+\frac{\left( \frac{\partial}{\partial \theta}\frac{\mathrm d\nu_\theta}{\mathrm d\nu_{\theta^\prime}}\right)^2}{\frac{\mathrm d\nu_\theta}{\mathrm d\nu_{\theta^\prime}}}+\frac{\mathrm d^2}{\mathrm d\theta^2}\frac{\mathrm d\nu_\theta}{\mathrm d\nu_{\theta^\prime}} \right] \\
& = \mathbb E_{\nu_{\theta^\prime}}\left[\frac{\mathrm d^2}{\mathrm d\theta^2}\frac{\mathrm d\nu_\theta}{\mathrm d\nu_{\theta^\prime}}\cdot\left( \log \frac{\mathrm d\nu_\theta}{\mathrm d\nu_{\theta^\prime}}+1\right)+\frac{\left( \frac{\partial}{\partial \theta}\frac{\mathrm d\nu_\theta}{\mathrm d\nu_{\theta^\prime}}\right)^2}{\frac{\mathrm d\nu_\theta}{\mathrm d\nu_{\theta^\prime}}}\right]\\
& = \mathbb E_{\nu_{\theta^\prime}}\left[\frac{\mathrm d^2}{\mathrm d\theta^2}\frac{\mathrm d\nu_\theta}{\mathrm d\nu_{\theta^\prime}}\cdot\left( \log \frac{\mathrm d\nu_\theta}{\mathrm d\nu_{\theta^\prime}}+1\right)\right] +\mathbb E_{\nu_\theta} \left[\underbrace{\left( \frac{ \frac{\partial}{\partial \theta}\frac{\mathrm d\nu_\theta}{\mathrm d\nu_{\theta^\prime}}}{\frac{\mathrm d\nu_\theta}{\mathrm d\nu_{\theta^\prime}}}\right)^2}_{(\frac{\partial}{\partial \theta} \log \frac{\mathrm d\nu_\theta}{\mathrm d\nu_{\theta^\prime}})^2} \right] \\
\end{aligned}.$$

Upon evaluating at $\theta=\theta^\prime$ the left summand in the last row dies: the log density equals zero, the total density is one, and moving the second derivative outside the expectation shows we're differentiating a constant. The remaining summand on the right is precisely the desired $L^2$-norm.

## Spatial Fisher information

### First step from parametric to spatial: translation families

Parametric Fisher information of a family $(\nu_\theta)_\theta$ is the expected squared rate of information gain as $\theta$ varies. To compute its value at $\theta$ we superimpose the information gain histogram of an infinitesimal perturbation $\theta+\varepsilon$ and compute the sum of square of the areas of the difference. Alternatively by the second Bartlett identity we just compute expected curvature.

But now we want to focus on a single distribution $\nu$ and its *spatial* changes. The obvious analogue is to superimpose the histogram of an infinitesimal translation  $\frac{\mathrm d\nu_\varepsilon}{\mathrm d\mu}$, defined by $\nu_\varepsilon(A)=\nu(A-\varepsilon)$, and repeat the same visual procedure. Intuitively, a large expected squared spatial rate of information gain means that on average, the histogram changes sharply. This visualization anticipates the spatial second Bartlett identity: it suffices to compute spatial curvature. The naive intuition fails if the supports of the histograms differ: then any superposition involves information gained from nowhere - infinities. Hence we'll require an assumption to the effect of both histograms having the same support. To formalize, it seems all we must do is modify the definition of parametrized Fisher information by replacing $\frac{\partial}{\partial \theta}$ with spatial derivatives $\frac{\partial}{\partial x}$. Right?

⚠️ Wrong. Very wrong. There is a crucial and subtle detail in our histogram analogy above: the implicit reference measure $\mu$ was implicitly assumed to be translation-invariant. Let us unravel this point.

**Histograms and reference measure.** Start with the familiar: finite distributions. We visualize the associated histogram as a bunch of columns *of equal, unit width*, whose height is probability. The equal unit width is the implicit appearance of the counting measure, and our histogram is that of the derivative $\frac{\mathrm d\nu}{\mathrm d\text{\#}}$. Scaling the counting measure into a uniform distribution would maintain uniform width $\frac 1n$. We will model histograms as follows: the width of each label $x_i$ is $\mu\lbrace x_i \rbrace$ while the height is density $\frac{\nu\lbrace x_i\rbrace}{\mu\lbrace x_i\rbrace}$. Hence the area of each column is $\nu\lbrace x_i\rbrace$.

Our overlay procedure involves cyclically shifting $\nu$ while keeping the ruler $\mu$ fixed. Applying a shift $\sigma$ to the density gives $\left(\sigma \cdot\frac{\mathrm d\nu}{\mathrm d\mu}\right)(i)=\frac{\nu_{\sigma i}}{\mu_i}$. To illustrate how uneven rulers may alter the shape of $\sigma \cdot\frac{\mathrm d\nu}{\mathrm d\mu}$ consider an extreme example of $\mu_1=\varepsilon,\mu_2=1-\varepsilon$ with $\varepsilon\ll 1$. Shifting a biased coin $\nu_1=p,\nu_2=1-p$ for would drastically change the histogram: the unshifted density has one column of approximate height zero and one of approximate height $p$. The shifted column has one column of approximate height $1-p$ and another of approximate height zero.

Back to the continuous case, translation-invariance of $\mu$ plays the same role, ensuring that our moving ruling has "uniform ticks".

____

With a convincing outline in mind, let's tell the story a bit more formally.

Fix a single distribution $\nu$. If our probability space has semigroup structure we can define the family of translation operators $T_\theta(x)=x+\theta$ and consider the pushforwards $$(T_{\theta\ast}\nu)_\theta$$. If our structure has the property of being a group then $$T_{\theta\ast}\nu (A)=\nu(A-\theta)$$. If there's a translation-invariant dominating $\nu_\theta\ll \mu$ (typically $\mu=\lambda$ is Lebesgue/Haar measure) then good things happen. First,
$$
\frac{\mathrm dT_{\theta\ast}\nu}{\mathrm d\mu}(x)=\frac{\mathrm dT_{\theta\ast}\nu}{\mathrm dT_{\theta\ast}\mu}(x)=\frac{\mathrm d\nu}{\mathrm d\mu}(x-\theta).
$$
We now apply the chain rule thrice.

* First, $\frac{\partial}{\partial \theta}\frac{\mathrm dT_{\theta\ast}\nu}{\mathrm d\mu}(x)=\frac{\partial}{\partial \theta}\frac{\mathrm d\nu}{\mathrm d\mu}(x-\theta)$.
* Second, $\frac{\partial}{\partial x}\frac{\mathrm dT_{\theta\ast}\nu}{\mathrm d\mu}(x)=\frac{\partial}{\partial x}\frac{\mathrm d\nu}{\mathrm d\mu}(x-\theta)$.
* Third, $\frac{\partial}{\partial \theta}\frac{\mathrm d\nu}{\mathrm d\mu}(x-\theta)=-\frac{\partial}{\partial x}\frac{\mathrm d\nu}{\mathrm d\mu}(x-\theta)$.

Hence density satisfies the pure transport PDE $\frac{\partial}{\partial \theta}\frac{\mathrm dT_{\theta\ast}\nu}{\mathrm d\mu}(x)=-\frac{\partial}{\partial x}\frac{\mathrm dT_{\theta_\ast}\nu}{\mathrm d\mu}(x)$. Dividing each side by the density and using the formula for differentiating $\log$, we find information gain also satisfies the same pure transport equation. (The reader may generalize to higher dimensions.) Taking $\nu_\theta$-expectations gives $\mathbb E_{\nu_\theta}\left[\left(\frac{\partial}{\partial \theta}\log\frac{\mathrm dT_{\theta\ast}\nu}{\mathrm d\mu}\right)^2\right]=\mathbb E_{\nu_\theta}\left[\left(\frac{\partial}{\partial x}\log\frac{\mathrm dT_{\theta\ast}\nu}{\mathrm d\mu}\right)^2\right]=\int \frac{\mathrm dT_{\theta\ast}\nu}{\mathrm d\mu}\left(\frac{\partial}{\partial x}\log \frac{\mathrm dT_{\theta\ast}\nu}{\mathrm d\mu}\right)^2\mathrm d\mu$. By translation invariance, these integrals are independent of $\theta$ (including the measure). Note these expectations are just numbers: we have averaged over all variables and there is no parameter left.

**Definition.** The spatial Fisher information of a distribution $\nu$ dominated by a translation-invariant $\nu\ll \mu$ is the $\nu$-expectation of the squared spatial rate of information gain.

$$I(\nu\mid\mu)=\mathbb E_{\nu}\left[\left(\frac{\mathrm d}{\mathrm d x}\log\frac{\mathrm d\nu}{\mathrm d\mu}\right)^2\right]
$$

**Parametric equals spatial: translation case.** If the space admits a translation-invariant measure, then parametric Fisher information of a translation family is constant. Moreover, it equals spatial Fisher information relative to any translation-invariant measure on the space.

$$
I[(T_{\theta\ast}\nu)_\theta]=\mathbb E_{\nu_\theta}\left[\left(\frac{\partial}{\partial \theta}\log\frac{\mathrm dT_{\theta\ast}\nu}{\mathrm d\mu}\right)^2\right]=\mathbb E_{\nu}\left[\left(\frac{\partial}{\partial x}\log\frac{\mathrm d\nu}{\mathrm d\mu}\right)^2\right]=I[\nu\mid \mu]
$$

*Proof.* Reconstruct the argument from preceding paragraphs.

By repeated application of the pure transport equation $\frac{\partial}{\partial \theta}\log\frac{\mathrm dT_{\theta\ast}\nu}{\mathrm d\mu}=-\frac{\partial}{\partial x}\log\frac{\mathrm dT_{\theta\ast}\nu}{\mathrm d\mu}$, odd derivatives have opposite sign and even derivatives are equal. Hence the second Bartlett identity for parameteric information implies the following spatial analogue. The hypothesis on postivity of the density ensures the translation family has $\theta$-invariant support.

**Spatial second Bartlett identity.** Spatial Fisher information equals minus expected curvature of information gain. Let $\nu\ll \mu$ with $\mu$ translation-invariant. Suppose the derivative $\frac{\mathrm d\nu}{\mathrm d\mu}$ is a.e positive and smooth enough. Then we have equality:
$$
I(\nu\mid\mu)=\mathbb E_{\nu}\left[\left(\frac{\mathrm d}{\mathrm dx}\log \frac{\mathrm d\nu}{\mathrm d\mu}\right)^2\right]=-\mathbb E_{\nu}\left[\frac{\mathrm d^2}{\mathrm dx^2}\log \frac{\mathrm d\nu}{\mathrm d\mu}\right].
$$

We summarize:
1. If there exists a dominating translation-invariant $\nu\ll\mu$, density and information gain of the translation family both satisfy the pure transport equation $\partial_t f=-\partial_x f$.
2. The parametric Fisher information of the translation family is constant in $\theta$. We may compute it using $\mu$ as the $\nu$-expected squared *spatial* rate of information gain $\frac{\mathrm d}{\mathrm d x}\log \frac{\mathrm d\nu}{\mathrm d\mu}$. In the literature, especially when $\mu=\lambda$ is Lebesgue measure, this rate is often called the Stein score of $\nu$, or Dirichlet score of $\nu$.
3. The spatial Fisher information formula is independent of the choice of translation-invariant dominating measure. ⚠️ It is otherwise sensitive to arbitrary changes in $\mu$ e.g. weighting by some positive function $f\mu$. Note the uniqueness of Haar measure means the choice is made for us in the context locally compact Hausdorff groups.

### Parametric to spatial: intervible flows

Write $T_\theta$ for the shift operator acting by $T_\theta =x-\theta$. We have associated to a distribution $\nu$ the translation family $$(T_{\theta\ast}\nu)_\theta$$. Then we saw that parametric Fisher information of the translation family equals spatial Fisher information relative to a translation-invariant measure $\mu$. The engine was to differentiate $\frac{\mathrm dT_{\theta\ast}\nu}{\mathrm d\mu}$. To do this we used translation-invariance of $\mu$: since
$$
\frac{\mathrm dT_{\theta\ast}\nu}{\mathrm d\mu}=\frac{\mathrm dT_{\theta\ast}\nu}{\mathrm dT_{\theta \ast}\mu}=\frac{\mathrm d\nu}{\mathrm d\mu}\circ T^{-1}_\theta=\frac{\mathrm d\nu}{\mathrm d\mu}\circ T_{-\theta},
$$
the chain rule gives
$$\frac{\partial}{\partial \theta}\frac{\mathrm dT_{\theta\ast}\nu}{\mathrm d\mu}=-\frac{\partial}{\partial x}\frac{\mathrm d\nu}{\mathrm d\mu}\circ T_{-\theta}=-\frac{\partial}{\partial x}\frac{\mathrm dT_{\theta\ast}\nu}{\mathrm d\mu}.
$$
The extremal equality is the *pure transport PDE*. Dividing by the density and using the formula for the derivative of $\log$ we see *information gain also satisfies the pure transport PDE*. This is intuitive: mass density and information gain are both labels traveling together. The last equality is by invariance of $\mu$.

The key properties of our dynamic setting are thus:
* An $\mathbb R$-action on a manifold $X$, i.e. a group of diffeomorphisms $(T_t)_t$.
* Measures $\nu,\pi$ on $X$ such that $\pi$ is dominating $T_{t\ast}\nu\ll \pi$ and invariant $T_{t\ast}\pi=\pi$.
* A (parametric) vector field $\vec v$ whose integral curves are given flowing along the $T_t$.

So when does such a vector field exist? It must satisfy

$$
\vec v(p)=\left.\frac{\mathrm d}{\mathrm dt}\right|_{t=0}T_t(p)\in \mathrm T_pX.
$$

Now we'll show how the abelian group structure ensures the latter equation defines an autonomous (!) ODE $u^\prime(t)=\vec v\circ u(t)$ where $u(t)=T_t$ is a diffeomorphism and

$$
u^\prime(t)\in \mathrm T_{ut}
$$

is its tangent. In earthlier terms, this is a parametric autonomous ODE

$$
\frac{\partial u(t,p)}{\partial t}=\vec v\circ u(t,p)
$$

with parameter given by the basepoint $p$. After we do this, we can apply the fundamental theorem about autonomous parametric flows, which is satisfied with Lipschitz $\vec v$. (Generally, $$\vec v\in C^k\implies u\in C^{k+1}$$ essentially by definition of the ODE.)

The point behind autonomy is

$$
\left. \frac{\mathrm d}{\mathrm dt} \right|_t \gamma(t) = \left. \frac{\mathrm d}{\mathrm dt} \right|_0 \gamma(t+s).
$$

Indeed:

$$
\begin{aligned}
\left. \frac{\partial}{\partial s} \right|_t T_{s}& = \left. \frac{\partial}{\partial s} \right|_0 T_{s+t} \\
& = \left. \frac{\partial}{\partial s} \right|_0 (T_s\circ T_t) = \vec v\circ T_t. \\
\end{aligned}
$$

An equivalent spatial perspective on autonomy is given by the commutation identity
$$
\left. \frac{\partial}{\partial t} \right|_t T_{t}p=\vec v\circ T_tp=\mathrm T_p T_t \circ \vec v (p),
$$
proven by a similar computation but using commutativty of the group structure. In particular
$$
\left. \frac{\partial}{\partial t} \right|_t T_{-t}p=-\mathrm T_p T_t \circ \vec v (p).
$$

**Group action: density and information gain both satisfy pure transport PDE.** In our setting, the family of densities and associated information gains $$(\frac{\mathrm dT_{t\ast}\nu}{\mathrm d\pi})_t,(\log\frac{\mathrm dT_{t\ast}\nu}{\mathrm d\pi})_t$$ both satisfy the following directional pure transport PDE:

$$
\partial_tf_t=-\mathrm df_t \circ \vec v,\quad X\times \mathbb R\overset{f}{\longrightarrow}\mathbb R.
$$

The autonomous (!) vector field chooses the direction at each point in space. In one dimension this becomes $\partial_tf(t,x)=-v(x)\partial_xf(t,x)$ so the velocity function determines the ratio of the temporal and spatial changes of $f$. In Euclidean space, the derivative functional is represented by the gradient $\mathrm df(p)v=\langle \nabla f(p),v\rangle $. Similarly for a Riemannian metric $g$

$$
\partial_tf_t=-\left\langle \nabla_g f_t, \vec v\right\rangle_g.
$$

*Proof.* For densities, compute via the chain rule. For information gain, divide the density transport equation by the density and use the formula for the derivative of $\log$.
$$\begin{aligned}
\left. \frac{\partial}{\partial t}\right|_{s}\frac{\mathrm dT_{t\ast}\nu}{\mathrm d\pi}(p) &= \left. \frac{\partial}{\partial t}\right|_{s} \left( \frac{\mathrm d\nu}{\mathrm d\pi}\circ T_{-t}p\right) \\
&= \mathrm T_{T_{-t}p}\left(\frac{\mathrm d\nu}{\mathrm d\pi} \right)\left( \left. \frac{\partial}{\partial t}\right|_{s} T_{-t}p\right) \\
&=- \mathrm T_{T_{-s}p}\left(\frac{\mathrm d\nu}{\mathrm d\pi} \right)\circ \mathrm T_p(T_{-s})\circ \vec v(p) \\
&=- \mathrm T_p \left(\frac{\mathrm d\nu}{\mathrm d\pi}\circ T_{-s}\right) \circ \vec v (p)\\
&=-\mathrm T_p \left(\frac{\mathrm dT_{s\ast}\nu}{\mathrm d\pi}\right)\circ \vec v(p).
\end{aligned}$$

**Proposition/Definition.** Parametric Fisher along a $\pi$-preserving symmetry is constant, and equals the directional spatial Fisher along the symmetry generator.

$$
I[(T_{t\ast}\nu)_t]=\mathbb E_{\nu_\theta}\left[\left(\frac{\partial}{\partial t}\log\frac{\mathrm dT_{t\ast}\nu}{\mathrm d\pi}\right)^2\right]=\mathbb E_{\nu}\left[\left( \mathrm d\log\frac{\mathrm d\nu}{\mathrm d\pi}\circ \vec v \right)^2\right]=I_{\vec v}[\nu\mid \pi]
$$

Furthermore, we have the spatial second Bartlett identity, which asserts the directional spatial Fisher also equals minus the expected square of the second Lie derivative of information gain along $\vec v$.

*Proof.* Left as an exercise. For the first assertion, replicate previous arguments. The second is clear if you're comfortable with Lie derivatives, and you should skip it otherwise.