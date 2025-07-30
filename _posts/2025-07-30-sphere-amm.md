---
layout: 
title: "Sphere AMM"
date: 2025-07-26
tags: [DeFi, AMM, liquidity]
comments: true
mathjax: true
author: IV
---

We live in $\mathbb R^n$. Consider the sphere centered about $p$ of radius $r$ (with respect to the Euclidean metric). The variable $x$ represents a vector of supplies $(x_1,\dots ,x_n)$ of assets $(X_1,\dots ,X_n)$.

$$r= d(x,p)$$

For $p=(r,\dots,r)$, this is the sphere inscribed in the standard cube of side-length $2r$. It is tangent to the cube at the points with exactly one coordinate equal to zero, and the rest all equal to $r$.

Any supply state $x$ with a coordinate larger than $r$ is open to arbitrage. Thus, assuming normal market conditions, we may restrict attention to the spherical cap lying within the unit cube.

To explain this, we begin with the special case of the circle embedded in the plane and centered at $(r,r)$. Draw the diameter perpendicular to the y-axis. Symmetry with respect to it exhibits the points $(x_1,r-d),(x_1,r+d)$ as reflections: indeed $(r-(r-d))^2=d^2=(r-(r+d))^2$. This means that without changing the supply of $x_1$ at all, the circle is willing to give up $2d$-worth of asset $X_2$ for free. By symmetry we can apply the same argument to the diameter perpendicular to the $x$-axis. Restricting to the bottom-left quarter of the circle removes the hassle. This quarter slice is precisely the intersection of the circle with the unit square. In $n$-dimensions the same computation holds.

Now assume a trader wishes to trade $X_i\to X_j$ i.e sell token $X_i$ in exchange for token $X_j$. The marginal $X_j$-price per unit of $X_i$ with respect to $f$ is geometric in nature. Specifically, if we take a tangent to the fiber in question $(dx_1,\dots ,dx_n)\in \mathrm T_pf^\leftarrow(fp)$ then the marginal price is precisely the rate of change $\frac{dx_j}{dx_i}$. It is easy computed using the gradient, which is perpendicular to the fibers by definition $\nabla f|_p\perp \mathrm T_pf^\leftarrow(fp)$. Indeed $\nabla f\cdot (dx_1,\dots,dx_n)=0$, so if we fix all directions besides $i,j$ we find $\frac{\partial f}{\partial x_i}dx_i+\frac{\partial f}{\partial x_j}dx_j=0$ whence

$$\frac{dx_j}{dx_i}=-\frac{\frac{\partial f}{\partial x_i}}{\frac{\partial f}{\partial x_j}}.$$

* $f(x,y)=xy$. Hence $\nabla f=(y,x)$ and the marginal price is $\frac{dy}{dx}=-\frac yx$. Thus we have the characterizing feature of constant-product AMMs: marginal price equals supply ratio.

* $f(x)=d(x,p)$ for the Euclidean metric and some fixed $p\in \mathbb R^n$. The fibers are spheres centered about $p$. Since $\nabla \| x \| =\frac{x}{\|x\|}$, the gradient is just $\nabla f =\frac{x}{d(x,p)}.$ Hence the marginal price is $\frac{dx_j}{dx_i}=-\frac{p_i-x_i}{p_j-x_j}$. Taking $p=(r,\dots,r)$ we have $\frac{dx_j}{dx_i}=-\frac{r-x_i}{r-x_j}$. Here the value $r$ is critical, behaving analogously to $\infty$ in the constant-product case: 

$$
\frac{dx_j}{dx_i}\overset{x_i\to r}{\longrightarrow}0,\quad \frac{dx_j}{dx_i}\overset{x_j\to r}{\longrightarrow}\infty.
$$

The formulas above show that pairwise exchange rates equal one precisely when all tokens are in equal supply. In the stablecoin case, market equilibrium thus occurs when all supplies are equal. This point is a multiple of $(1,\dots,1)$ whose distance from $(r,\dots,r)$ equals $r^2$. It is straightforward to compute. First,

$$r^2=\|c (1,\dots,1)-(r,\dots,r)\|^2=n(c-r)^2.$$

The solutions to this quadratic give two antipodal points on our sphere $c=r(1\pm\frac{1}{\sqrt n}$). We'll take $c=r(1-\frac{1}{\sqrt n })<r$ to remain in the first orthant, out of the arbitrage zone.

Recall the Pythagorean theorem (whose converse holds for symmetric inner products, so e.g. over the reals) $$v\perp w\implies \|v\|^2+\|w\|^2=\|v+w\|^2.$$

We can use it to simplify 

https://www.paradigm.xyz/2025/06/orbital
