---
layout: post
title: "Basic Finance for DeFi: Capital Efficiency"
date: 2025-07-12
tags: [finance, liquidity, DeFi, AMM]
comments: true
mathjax: true
author: IV
---

Capital efficiency is an ordinal property of markets that measures how efficiently capital generates economic outputs such as liquidity and yield.

This post attempts to organize basic financial concepts related to it that arise in DeFi.

# Liquidity

Liquidity is an ordinal property of markets, and is crucial for capital efficiency. Folklore intuition can be summed up in one line:
> A market is highly liquid if it can quickly execute trades with minimal price changes.

For example, major stock exchanges are liquid markets for commonly traded stocks.

It is typical to abuse terminology and say an asset is liquid, meaning "this asset is quickly convertible into money with minimal loss". Formally, this statement asserts the liquidity of an implicit market between the asset and money. For examples, a house is an illiquid asset because it takes a while both to discover the market price and to sell it at that price.

One sometimes encounters the term "funding liquidity", referring to the ease of obtaining credit (i.e., borrowing). Thus an asset has high funding liquidity if it can serve as collateral for money at a good rate.

For some history see [e.g. this stackexchange answer](https://economics.stackexchange.com/a/1581).

Liquidity is measurable by a few key metrics:

| Metric   | Meaning                                                               | Consequence                               | Cause                                                                                                                                       |
| -------- | --------------------------------------------------------------------- | ----------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| Spread   | Lowest sell price (best ask) minus highest buy price (best bid).      | Wide spread → high cost to initiate trade | Insufficient market-making                                                                                                                  |
| Depth    | What asset amount can be traded within some price range?              | Shallow depth → volatility                | Insufficient supply                                                                                                                         |
| Slippage | What I expected when placing my trade vs what I got when it executed. | High slippage → I get less                | App-level: depth & spread<br><br>Core-level: market state discrepancy between simulation & execution, e.g. due to competition, latency, MEV |

Spread is a threshold cost which dominates small trades. Beyond the threshold, the trade begins to consume depth. As trade size increases, depth begins to dominate its cost.

Before diving a bit deeper, we record a common unit of measurement in finance:
**Definition.** A basis point (plural bps) is a hundredth of a percent $0.01％=\frac 1{10,000}$.

## Spread

Assuming the market price is within spread interval, a taker must pay a premium to immediately initiate a trade. Wider spread → higher premium.

$$ [\text{highest bid},\text{lowest ask}] $$

A narrow spread thus increases capital efficiency, especially for small trades. A spread of 10 bps means the highest bid and lowest ask differ by $0.10％$.

## Depth

High depth around an exchange rate leads to price stability: even large trades will not cause volatility. The statement "there's 10K USD of BTC buy-side depth per bp at the current price" means you can buy 10K USD-worth of BTC and incur a price change of at most one basis point, i.e $0.01％$.

The depth of liquidity in a price interval should be thought of as the supply of capital deployed there. High depth at exorbitantly high/low prices is wasted capital. We'll unpack this later for [constant-product AMMs](###Depth-analysis).

Depth is crucial also for stable capital-efficient loan markets because it ensures liquidations are profitable to liquidators. We illustrate the opposite extreme: how shallow depth triggers a liquidation death spiral. The loan market in question is borrowing USD against BTC collateral.
* Suppose Alice loaned 1M USD to Bob against 10 BTC. Suppose Bob defaults, so that Alice seizes 10 BTC. Now suppose Alice wishes to liquidate the loan i.e. sell the BTC for USD. If the BTC/USD market in question has shallow liquidity, the exchange rate would dip against Alice: she may sell 1 BTC at a good price, but any larger amount would sell at an increasing loss to her. In case of an over-collateralized loan, Alice can make a quick profit even if she sells some of her BTC underpriced. Such a sale would lower the BTC/USD exchange rate against BTC, potentially triggering more liquidations.
* Depth is the dampener of liquidations, defending against chain-reactions and death spirals.

## Slippage

Products sold in stores typically have posted prices, which trivialize economic calculation. The naive approach to buying some amount of an asset would be to multiply its market price-per-unit by the amount.

If the market price is a 1 USD/unit, I naively expect 100 USD to buy me 100 units. *Slippage* is any deviation from this expected outcome. Such deviations are determined entirely by trade size, spread, and depth. Slippage of this form can be addressed at the application level, e.g. via aggregators and routers which traverse multiple markets.

There is another, subtler type of slippage. Even if I have perfect knowledge of the market mechanism (e.g. AMM curve) and its current state, I do not know the market state at the execution of my order. Despite my ability to perfectly simulate my trade, I cannot predict the market conditions at execution: reality can differ from my expectations, resulting again in *slippage*. Such slippage can be honest, e.g. competitors pay higher priority fees and overtake me, or manipulative, e.g. sequencers manipulating ordering.

# Market-makers

Markets are not in a constant state of equilibrium, and organic activity (or lack thereof) can result in occasional wide spreads.
1. Buyers and sellers need not show up together. There can be waves of one-sided demand, in terms of both time and volume.
2. Price discovery takes time: buyers would initially post low bids and sellers would initially post high asks.
3. Liquidity may not arise organically, due to wide spreads or shallow depth.

Market makers (henceforth MMs) streamline market operations by providing supply & demand counterparties and optimizing price discovery.

## Order-book market-makers

In order-book markets, MMs act by posting trades that bring the market closer to equilibrium. The naive business model is simple: manufacture spread and profit off it. Specifically, MMs manufacture spread by maintaining a gap between their highest bids and lowest asks. They aim for a narrow spread when competition is high and risk is low, and conversely.

## AMMs (automatic market makers)

An AMM is an implementation of a market that:
1. Automatically computes exchange rates based on its state.
2. Automatically distributes supply across the exchange-rate curve to create liquidity.

# Worked example: constant-product AMMs

We now illustrate the abstract nonsense above in the setting of a constant-product AMM. We start out **without fees**, and address them later.

## Basics

Fix two assets $X,Y$ and denote by $x,y$  their respective supplies. The AMM curve is $xy=k$  for some constant $k$. Evidently the curve is symmetric in $x,y$. As a function of $x$, we see $y=\tfrac kx$ is not only monotone decreasing but also *subadditive*, which enforces diminishing marginal utility. Conceptually, this is the only required property of an AMM curve. In our constant-product case, marginal diminution is visualized by the hyperbola curve: As the supply $x$ grows, a fixed change of supply $\Delta x$ causes a diminishing change of supply $\Delta y$.

Let us quantify supply changes. Suppose the present state of AMM supply is $x,y$. How does a change in supply $\Delta x$  affect the supply of $Y$? By definition, the next state of the AMM supply is $x+\Delta x, y+\Delta y$, so the invariant constraint gives us

$$k=(x+\Delta x) (y+\Delta y)=y(x+\Delta x)+\Delta y(x+\Delta x).$$

Hence

$$\Delta y=\frac{k-y(x+\Delta x)}{x+\Delta x}=\frac{k}{x+\Delta x}-y=\frac{-y\Delta x}{x+\Delta x}.$$

Thus we see how $\Delta y$ inversely depends on the initial supply $x$.

The exchange rate (price) charged by the AMM is just the ratio of supplies. The fraction $\frac{y}{x}$ is the supply of $Y$ fitting into a unit supply of $X$. That is, the $Y$-price of a unit of $X$. Price change is easily computable:

$$\frac{y}{x}=\frac{k}{x^2}\longrightarrow\frac{y+\Delta y}{x+\Delta x}=\frac{k}{(x+\Delta x)^2}. $$

Note the $Y$-price of a unit of $X$ obeys an inverse square law w.r.t to the supply of $X$: price decays quadratically as supply grows. This super-linear decay also exhibits diminishing marginal utility.

Prices are not encoded by the AMM equation, but rather discovered by the market. The equation merely constrains the supplies to satisfy diminishing marginal utility, which is necessary for price discovery. (Play around with a constant-sum equation and see how the resulting market is severely misbehaved.)

## Depth analysis

We can express supply $x$ as a function of the $Y$-price per unit of $X$: $p=\frac yx$.

$$x^2p=k\implies x(p)=\sqrt \frac{k}{p}. $$

Note a larger constant $k$ means more depth: the market sustains a larger supply of $X$ at a given price.

By definition, depth is the supply change required to move between given prices.

$$D_X(p_1\to p_2)=x(p_1)-x(p_2)=\sqrt\frac k{p_1}-\sqrt\frac k{p_2}$$

Depth is nonnegative iff $p_1\leq p_2$: supply decreases iff price increases (in this AMM). Evidently depth is antisymmetric, so the price interval determines a unique number - the absolute value of the associated depth. Note also how depth scales with the interval:

$$\text{Depth}[\alpha^2 p_1,\alpha^2 p_2]=\frac 1\alpha \text{Depth}[p_1, p_2].$$

> Constant-product AMM depth decays slowly as $\frac1\surd$.

Equipped with the depth formula, we can analyze the capital efficiency of constant-product AMMs. Specifically, we'll analyze how the constant product curve causes supply to be inefficiently allocated to price bands with low trading activity.

### Depth on the tail

Assume a current supply of $x_0$ at a price-per-unit of $p_0$. This supply is distributed over the price range $[p_0,\infty)$ precisely according to depth.
* A divergent increasing sequence of prices $p_0<p_1<p_2<\cdots$  partitions the interval $[p_0,\infty)$.
* Current supply is partitioned by the depths of the associated price bands. (The infinite sum makes sense because $x(p)\overset{p\to \infty}{\longrightarrow}0$.)

$$\begin{aligned}
x_0 &=[x(p_0)-x(p_1)]+[x(p_1)-x(p_2)]+\cdots \\
 &=D_X(p_0\to p_1)+D_X(p_1\to p_2)+\cdots 
\end{aligned}$$

> The partition of total current supply by depth specifies the amount of current supply allocated to each price band. 

How much supply is "wasted" on exorbitantly high prices? Consider a $\overbrace{\text{USDC}}^Y/\overbrace{\text{ETH}}^X$ pool with $k=10^{10}$. As of July 2025, 1 ETH ≈ 3000 USD, so $p_0=3000$ whence $x_0=\sqrt\frac{10^{10}}{3000}\approx 1825$. A high USD-price of ETH is denoted $p_\top$. The depth formula gives

$$D_X(p_\top\to \infty)=x(p_\top)-x(\infty)=\sqrt\frac k{p_\top}.$$

Some example computations show the extreme capital inefficiency of the constant-product curve.

$$\begin{aligned}
D_{\text{ETH}}(\mathrm{\$}1\mathrm{M} \to \infty) &\approx 100 \text{ ETH} &\approx 5.5％ \\
D_{\text{ETH}}(\mathrm{\$}10\mathrm{K} \to \infty) &\approx 1000 \text{ ETH} &\approx 55％ \\
D_{\text{ETH}}(\mathrm{\$}5\mathrm{K} \to \infty) &\approx 1414 \text{ ETH} &\approx 77％
\end{aligned}$$


### Depth in general

How to interpret $D_X(p_1\to p_2)$ if the current price $p_\text{now}$ satisfies $p_\text{now}>p_1$? In this case, reaching $p_1$ would require an increase in supply, so we can't use the partition trick above. Or can we? Math shows the way. The equation

$$D_X(p_1\to p_2)=D_X(p_1\to p_\text{now})+D_X(p_\text{now}\to p_2)$$

interprets the LHS as the amount of supply required for a price increase $p_1\nearrow p_\text{now}$, plus the (familiar) depth starting from the current price.

### Depth concentration

We can write an explicit formula for the relative depth concentration in a price band within a larger band $[p_1,p_2]\subset [p_\perp,p_\top]$, notably independent of the constant $k$.

$$\underset{[p_1,p_2]\subset[p_\perp,p_\top]}{\text{Concentration}}=\frac{\text{Depth}[p_1,p_2]}{\text{Depth}[p_\perp,p_\top]}=\frac{\sqrt\frac 1{p_1}-\sqrt\frac 1{p_2}}{\sqrt\frac 1{p_\perp}-\sqrt\frac 1{p_\top}}$$

Let's analyze the capital efficiency of a constant-product AMM for a **stable pair** USDC/USDT. by looking at the relative concentration of a bp interval vs a 1% interval (factor of 100)

$$\begin{aligned}
\underset{[0.9999,1.0001]\subset[0.9,1.1]}{\text{Concentration}} &\approx 0.0994％ \\
\underset{[0.99,1.01]\subset[0.9,1.1]}{\text{Concentration}} &\approx 9.94％
\end{aligned}$$

So assuming the entire market lives within a 10% band, merely 10% of capital is allocated to trading within a %1 band, and merely 1% is allocated to trading within a basis point!
## What about fees?
Percentage fees are crucial for LP revenue. They slightly increase slippage, especially for larger trades.

# Concentrated liquidity

Classically, there is a clear distinction between capitalists, who passively provide capital, and those who actively employ capital (often delegated to them). In DeFi terminology, capitalists are the *passive* liquidity providers. However, *active* liquidity providers are superior market makers, who have strong potential to enhance capital efficiency capital efficiency. We illustrate this with a case study: Uniswap v2 vs Uniswap v3.

Uniswap v2 is a constant-product AMM. LP UX is roughly: choose pair → deposit assets → receive LP token → earn fees → exit. LPs must supply equal value of both tokens (to preserve pool price). Since LP positioned are fully determined by deposited capital, LP tokens are just fungible ERC-20 tokens.

Uniswap v3 is a "piecewise-constant-product" AMM. LP UX is roughly: choose pair → choose fee tier → choose price range → deposit assets → receive LP token → earn fees → reposition/exit. Each pool partitions the allowed price range via *ticks*, via which LPs can specify their desired price bands for deploying capital (adding depth). Due to the added complexity of the liquidity position, LP tokens are (currently) NFTs, encoding more than the ERC-20 standard allows.

The ability to concentrate capital in a chosen price range precisely means LPs are free to choose where to provide depth. Let's illustrate how beneficial this design is for capital efficiency by examining a stable pair, e.g. USDC/USDT (I wonder how well this will age). The vast majority of LPs will choose to provide depth in a narrow band about the 1:1 rate, so liquidity will be very deep in the center and very shallow at depegged exchange rates. If Alice deposits in tick range $[t_1,t_3]$ and Bob deposits in $[t_2,t_4]$, both are active in $[t_2,t_3]$, and both provide liquidity to the constant-product AMM associated to this small interval. The locality of each piece in Uniswap v3 mitigates the slow $\frac1\surd$ decay of depth in constant-product AMMs to small intervals.