---
layout: post
title: "AMMs: Hyperbolas vs Circles"
date: 2025-08-16
tags: [DeFi, AMM, liquidity]
comments: true
mathjax: true
author: IV
---

[We've looked](https://unthoughts.github.io/2025-07-12-blurb-on-liquidity/) at hyperbolas: the constant-product AMM defined by fibers of $f(x,y)=xy$. Now it's circle time.

Too busy with your perpetual motion machine? Here's a quick summary: the circle AMM is like a compactified hyperbola AMM. It features roughly 2.5x more depth around the equal-price point. Unfortunately, the capital efficiency is still shit, so we still need concentrated liquidity.

# Circle AMM

A circle centered about the origin makes little sense economically since it allows for negative supplies. Instead, let's take the circle centered about some $(a,b)\in \mathbb R^2$ in the positive quadrant. Our circles are fibers of the Euclidean distance from it, and we'll only look those of sufficiently small radius $r\leq\|(a,b)\|_2$ to avoid negative supplies.

$$
f(x,y)=d_2((x,y),(a,b))=\sqrt{(x-a)^2+(y-b)^2}
$$

So what now?

First of all, circles are compact (in stark contrast to the unbounded hyperbolas from constant-product AMMs). Thus, supply of each asset is capped. We'll unpack the financial meaning of this global difference in geometry in the next section.

Second, we should note that the symmetry of the circle actually makes trades ambiguous and undefined. Let's explain. Starting from an initial market state $(x,y)$ suppose Alice wishes to sell $\Delta x$ in exchange for $\Delta y$, i.e. shes wishes to execute a trade $(x,y)\rightarrow (x+\Delta x,y+\Delta y)$. If the AMM constraint is nice enough we can express what Alice will receive $\Delta y$ as a function of what she sells $\Delta x$. We [solved this exercise](https://unthoughts.github.io/2025-07-12-blurb-on-liquidity/) for the hyperbola $xy=k^2$ and found

$$
\Delta y=\frac{k^2-y(x+\Delta x)}{x+\Delta x}=\frac{k^2}{x+\Delta x}-y=\frac{-y\Delta x}{x+\Delta x}.
$$

Repeating for the circle constraint we find that $\Delta x,\Delta y$ are related by a quadratic polynomial. Hence are two allowed returns $\Delta y$ for any sold amount $\Delta x$ (and vice versa):

$$
\Delta y=-(y-b)\pm \sqrt{(y-b)^2-[2(x-a)\Delta x+(\Delta x)^2]}.
$$

So which increment should Alice receive in exchange for her $\Delta x$? The peculiarity becomes especially obvious when we take $\Delta x=0$, i.e. Alice gives nothing. Plugging into our formula we find $\Delta y\in \lbrace 0,-2(y-b) \rbrace$. So if the initial market state had $y<b$ then $-2(y-b)>0$ meaning it's possible for Alice to give nothing and receive something - for free!

This is obviously madness, and we will stay clear of it by restricting ourselves to the lower left quarter slice of the sphere, where $x\leq a,y\leq b$. This slice of the sphere intersects each horizontal/vertical line at most once, ensuring trades are unambiguous and preventing any free arbitrage. Another solution would simply be to always return the lowest value of $\Delta y$, but restricting the curve is more convenient.

Lastly, our choice of radius determines both minimal and maximal supply of both assets $X,Y$. Having our circle tangent to the axes precisely means each asset has a minimal supply of zero. Moreover, this minimal supply is attained precisely when the other asset attains a maximal supply, which is easily computed to be $x_\text{max}=a, y_\text{max}=b$. Regardless of radius and the value of the minimal supply, the marginal $Y$-price per unit $X$ will explode as $X$ approaches this minimum; it is a design question whether the AMM should maintain some positive minimal reserve threshold for whatever reason.

# Side-by-side

Enough words, time for pictures. Below are three superimposed plots: the hyperbola $xy=k^2$ (green), the circle $(x-k)^2+(y-k)^2=k^2$ (blue), and the normalized circle (red) that has been scaled and moved to have the same equal supply point as the hyperbola. The normalization is important for a real apples-to-apples, since it is the outcome of the same initial deposit $x_0=y_0=k$.

The key takeaway is simple: our quarter circle looks like a compactified version of the hyperbola: the axes are now tangents at finite points instead of only asymptotically.

<iframe src="https://www.geogebra.org/graphing/gze6tau4?embed" width="800" height="600" allowfullscreen style="border: 1px solid #e4e4e4;border-radius: 4px;" frameborder="0"></iframe>

# Price and supply

[Insert profound yet relevant proverb.]

## Derivation

Let's recall the derivation of marginal prices for constant function AMMs. Assume Alice wishes to trade $X_i\to X_j$ i.e. sell token $X_i$ in exchange for token $X_j$. The marginal $X_j$-price per unit of $X_i$ with respect to $f$ is geometric in nature. Specifically, if we take a tangent to the fiber in question $(dx_1,\dots ,dx_n)\in \mathrm T_pf^\leftarrow(fp)$ then the marginal price is precisely the rate of change $\frac{dx_j}{dx_i}$. It is easy computed using the gradient, which is perpendicular to the fibers by definition $\nabla f|_p\perp \mathrm T_pf^\leftarrow(fp)$. Indeed $\nabla f(x)\cdot dx=0$, so if we fix all directions besides $i,j$ we find $\partial _if\cdot dx_i+\partial _jf\cdot dx_j=0$. The implicit function theorem ensures

$$\frac{dx_j}{dx_i}=-\frac{\partial _if}{\partial _jf}.$$

### Hyperbola

In the constant-product case $f(x,y)=xy=k^2$ and the gradient is $\nabla f=(y,x)$ and the marginal $Y$-price per unit of $X$ is

$$
p=\frac{dy}{dx}=-\frac yx=-\frac {k^2}{x^2}.
$$

Thus we have the characterizing feature of constant-product AMMs: marginal price equals global supply ratio. Furthermore,

$$
|p|\overset{x\to 0}{\longrightarrow}\infty,\quad |p|\overset{y\to 0}{\longrightarrow}0.
$$

We can also extract supply as a function of price:

$$
x(p)=\sqrt{-\frac {k^2}p}.
$$

### Circle

If we look at our circles $f(x,y)=(x-a)^2+(y-b)^2$, the gradient is $\nabla f=2(x-a,y-b)$. Hence we have formulas for the marginal price. As usual, we have ambiguity since each $y$-value has two $x$-values. Since negative price means an actual exchange of quantities, we take the $+$-branch, which is negative for $x\in [0,a)$.

$$
p=\frac{dy}{dx}=-\frac{x-a}{y-b}=\pm\frac{x-a}{\sqrt{r^2-(x-a)^2}}.
$$

Here the lines $y=b,x=a$ are critical, behaving analogously to $x=0,y=0$ in the constant-product case: 

$$
|p|\overset{y\to b}{\longrightarrow}\infty,\quad |p|\overset{x\to a}{\longrightarrow}0.
$$

We can extract supply as a function of price: squaring gives us

$$\begin{aligned}
0 &=p^2(r^2-(x-a)^2)-(x-a)^2 \\
 &= p^2r^2-(p^2+1)(x-a)^2,
\end{aligned}$$

whence we have the following formula. We take the lower branch to remain in the first quadrant $x<a$.

$$
x(p)=a\pm\frac{rp}{ \sqrt{1+p^2} }.
$$

# Comparison of price and depth

[Insert witty sentence.]

## Price comparison

Need pictures. Below are the price graphs for three AMMs: the hyperbola $xy=k^2$, the circle $(x-k)^2+(y-k)^2=k^2$, and another circle, merely normalized to have the same equal price point as the hyperbola.

<iframe src="https://www.geogebra.org/graphing/paearf63?embed" width="800" height="600" allowfullscreen style="border: 1px solid #e4e4e4;border-radius: 4px;" frameborder="0"></iframe>

Note the circle has the full price range from zero to negative infinity (infinite $Y$-price per unit $X$).

Note also that marginal utility diminishes much faster for the circles: a price of zero is reached at a finite supply instead of asymptotically at infinite supply.

## Depth comparison

Equipped with formulas for supply as a function of price we can compute depth. We consider the hyperbola and the associated normalized circle, with constants $k,r=k(2+\sqrt 2)$ and the same initial supply $x_0=y_0=k$.

$$\begin{aligned}
\text{Depth}_\mathrm{H}[a,b] &=\frac{k}{\sqrt{a}}-\frac{k}{\sqrt{b}} \\
\text{Depth}_\mathrm{C}[a,b] &=\frac{rb}{\sqrt{1+b^2}}-\frac{ra}{\sqrt{1+a^2}} 
\end{aligned}$$

### Globally

For the global comparison, imagine we partition the entire price range into intervals of length $a>0$. The following graphs show the depth in price interval $[x,x+a]$ for the hyperbola and the circle. The key takeaway is that the circle has deeper liquidity in small intervals around low prices $0.1 \lesssim p\lesssim 3$, by a factor that starts close to 2, peaks around 2.4, and then gradually diminishes. The hyperbola has deeper liquidity at higher prices, but the difference also diminishes as the price goes higher. 

<iframe src="https://www.geogebra.org/graphing/gt3m863a?embed" width="800" height="600" allowfullscreen style="border: 1px solid #e4e4e4;border-radius: 4px;" frameborder="0"></iframe>

### Locally about the equal-price point

The following graphs capture depth as a function of $0<\delta<0.8$ in units of $k$. Functions with ↑ subscripts pertain only to upward depth i.e. the amount of $X$ that will move price upwards $1\to 1+\delta$, and similarly for ↓ subscripts.

<iframe src="https://www.geogebra.org/graphing/vvubrt6s?embed" width="800" height="600" allowfullscreen style="border: 1px solid #e4e4e4;border-radius: 4px;" frameborder="0"></iframe>

We'll emphasize two key takeaways.

First of all, upon highlighting only the purple ratio graphs we see the circle has more depth than the hyperbola around equal price. The factor is over 2.4x for $\delta <0.25$ and remains over 2x for $\delta ≲0.6$. The upward ratio remains relatively stable, but the others begin to destabilize as we approach $\delta =1$: this represents a price of zero and the hyperbola requires unbounded supply to approach it.

Second of all, they're both pretty shitty. [We have previously computed](https://unthoughts.github.io/2025-07-12-blurb-on-liquidity/) the severe capital inefficiency of hyperbolas for a stable pair around equal price, and the circle is just a bit better. As you can see by examining $H_\uparrow,C_\uparrow$, an upward price movement of 10％ i.e. $1\to 1+\delta$ for $\delta =0.1$ requires a mere ≈5％ of supply on the hyperbola and a mere ≈11％ on the circle.

# Conclusion
See intro. Drink more water.