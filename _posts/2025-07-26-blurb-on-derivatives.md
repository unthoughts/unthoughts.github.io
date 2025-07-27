---
layout: post
title: "Blurb on Derivatives"
date: 2025-07-26
tags: [finance, derivatives]
comments: true
mathjax: true
author: IV
---

# Risk and Knightian Uncertainty

We don't know the future. This statement is true, but a bit too coarse - there are two distinct ways in which we don't know things:

1. We don't know the results of a fair coin toss, but we know the associated probability distribution. Hence we can quantify risk and reason about these quantities.
2. We don't know anything about a biased coin without explicit information about its bias. We are unable to quantify our uncertainty.

The distinction between risk and uncertainty appears in the seminal text *Risk, Uncertainty, and Profit* by Frank Knight. This foundation provides a clear distinction between:

1. The risk market, comprised of:
    * Risk-bearers - traders, insurers, speculators,
    * Risk managers - hedgers and portfolio managers,
    * Risk intermediaries - brokers, exchanges.
2. The entrepreneurial market, comprised of uncertainty-bearers - entrepreneurs and the venture capital funds who invest in them.

The quantitative nature of risk allows for a developed risk market, which will be the focus of this post.

# Derivatives

The term *derivative* originally referred to something which derives its value from an underlying asset. The simplest example of a derivative in this broad sense is just an IOU: a slip of paper that says "Alice promises to redeem this note for a potato". Evidently the IOU derives its value from that of a potato alongside Alice's reputation.

In the context of finance, derivatives have a narrower meaning: they are a type of instrument for trading risk.

**Definition.** A *derivative* is a financial contract whose value depends on some "observable", e.g. the exchange rates, general market conditions (or e.g. the weather).

Derivatives enable *synthetic exposure* to underlying assets, allowing traders to access the financial outcomes of "direct" trades without requiring direct/physical ownership. Of course you either have a potato or you don't, but derivatives allow you to indirectly participate in the potato market without owning any.

The following classification hits the common types of derivatives, which we'll explore further below.

1. Option to trade or obligation to trade - does the contract impose future trades on all parties, or are they merely optional for one party?
2. Deterministic or not - does the contract specify all assets, amounts, and exchange rates in advance, or do some trades depend on future market conditions?
3. Temporary or perpetual - does the contract expire or not?

Let's illustrate how derivatives are vehicles for trading risk.

**Example.** Alice writes and sells a forward contract to buy 1 BTC for 100K USD at a specified expiration date. In this example, Alice is selling exposure to the difference between the fixed price of 100K USD per BTC and its future market price. Alice wants to isolate herself from this risk, while the buyer wants exposure to it, based on a positive risk assessment. Here's a breakdown: 

* Evidently Alice wants USD because that is her denomination for the forwards contract.
* Instead of writing and selling the forward contract, Alice could hang on to her 1 BTC. But waiting incurs a *risk*: she does not know the future BTC/USD exchange rate. Her 1 BTC may be worth less than 100K USD at the expiration date.
* Alice wishes to transfer this risk to another party at some cost. She is willing to sell the contract for 1000 USD. A buyer is someone who is willing to pay 1000 USD upfront due to a belief that BTC will sufficiently appreciate relative to USD at strike price. What does "sufficiently" mean? At least enough to justify the opportunity cost of paying the 1000 USD upfront.
* Note Alice need not require 100K USD upfront. Such terms would impose severe opportunity cost on buyers, since they'd be parting from their 100K USD long before the expiration date.

One should not confuse facilitators of the risk market with its participants.

**Toy example.** Imagine, if you can, an honest bookie. Bob the bookie facilitates bets. Bob sells gamblers the right to enter a bet at some fixed price. Bob is smart, so he doesn't put any of his own money down. Instead, he lets gamblers bet only against each other. As a pure facilitator, Bob earns a fixed fee per bet, all without any exposure to whatever it is that people are betting on.

This example generalizes e.g. to a derivative exchange: infrastructure whose operators profit from the demand for speculation without any exposure to outcomes. Casinos live in between, since they take on risk in some gambling activities (blackjack), while only facilitating gambler interaction in others (poker).

## Brief demented rant about reserves

Alice can write an IOU ("I owe you"): "Alice promises to redeem this note for one potato". The value of this IOU depends on her reputation, i.e. [common belief](https://link.springer.com/chapter/10.1007/978-1-4613-1139-3_1) in her trustworthiness. While enormously significant, we defer the topic of trust in financial contexts to another post, mainly to avoid a nervous breakdown. Anyway, to avoid fraud, Alice must hold in reserve the underlying asset - one potato. More generally, if the number of such IOUs in circulation exceeds Alice's potato reserves - there is fraud.

The word "fraud" in the context of fractional reserves is ill-received since this practice is standard in the banking industry. Bankers and financial engineers accept the "disadvantage" of insolvency and consequent collapse in case of bank runs. There are many interesting discussions about this "fine line", its risks and uncertainties, and its morality. For instance, some say the public is a voluntary participant in this engineering, and the engineers do not strictly commit to instant redemptions nor to much else. There is also the delicate question of "is it really fraud if we avoid insolvency"? We avoid these worthwhile topics - at least in this post - but we hint at the dormant fury of the author.

The point of the rant is that regardless of their complexity, financial derivatives must obey the same reserve rules: issuers must have the assets to fulfill their obligation. If Alice writes and sells a forward contract to buy 1 BTC for 100K USD, she must hold 1 BTC at expiry; otherwise she is committing fraud. The same holds if Alice writes sells an option to buy her own forward contract: she must hold the forward at expiry.

## Some jargon: denomination, mark-to-market, margin, leverage

**Definition.** A contract is said to be denominated in asset $X$ if all associated PnL (profit & loss) accounting and settlement (unless otherwise specified) are denominated in asset $X$.

**Example.** BTC/USD forward: Alice is obligated to buy 1 BTC for 100K USD at a specified expiry. If the contract is BTC-denominated then Alice will receive 1 BTC. If it's USD-denominated, the 1 BTC will be converted into USD, which will then by paid to Alice.

The forward contract above is unaware of market conditions until expiry. Often people want financial devices with more frequent accounting.

**Definition.** An *index* is a periodically computed reference value that summarizes the market state with respect to a set of assets or data.

**Definition.** A contract has mark-to-market (MtM) if the value of each party's position is periodically re-evaluated based on current market prices (e.g. some index), with consequent settlement according to contract terms.

**Example.** Futures contracts typically have mark-to-market. Consider a BTC/USD futures between Alice long on BTC and Bob short on BTC. Once a day, their positions are re-evaluated according to the BTC/USD exchange rate. Suppose the rate moves in favor of BTC, i.e in favor of Alice. If the contract is BTC-denominated, Bob will pay Alice on this day.

**Example.** In an *interest rate swap* (IRS), two parties exchange interest payments on some notional amount (but without moving money). The sides of the trade are called *legs*. A *fixed* leg involves a fixed interest on the notional amount, with a *floating* legs denoted a variable interest rate, typically index-linked. A fixed/floating index-linked IRS is an MtM derivative; same for floating/floating IRS.

MtM contracts without collateral are possible when counterparty risk is accepted. Otherwise, MtM typically enforces solvency and triggers liquidations.

**Definition.** A contract may have *margin requirements* that specify how much collateral/margin all sides must provide enter the contract and maintain keep the position open without liquidation. A contract is said to be *margined* in asset $X$ if:
1. PnL accounting & settlement are denominated in $X$;
2. $X$ is used as collateral;
3. Margin accounting & settlement are denominated in $X$.

Margin requirements (the logic enforced on the margin account) are out of scope. An $X$-margined contract can, in edge cases, denominate margin requirements in a different asset.

Typically, margin requirements fall into:
* Initial margin - minimal amount to open a position.
* Maintenance margin - minimal balance of a "collateral/margin account" which must be maintained at MtM to keep the position open.
* Variation margin - amount of collateral transferred between counterparties to settle MtM PnL.

**Example.** Let us extend the previous example. Assume the futures contract is BTC-denominated. Daily variation margin will require Alice to pay Bob in case the exchange rate varies against BTC. For an edge-case, assume we add a USD-denominated margin requirement: both Alice and Bob must have at least 1000 USD-worth in their margin account to maintain their positions.

|                           | Margined | MtM |
| ------------------------- | -------- | --- |
| Requires collateral?      | Yes      |     |
| Reflects current market   |          | Yes |
| Adjusts account balances? | Yes      | Yes |
| Liquidation risk?         | Yes      |     |

Sometimes a contract has accounting in one asset but settlement in another.

**Definition.** A contract is said to have *physical delivery* if it's settled by delivering the underlying asset, and *cash delivery* if it's settled in the denominating asset.

**Example.** Consider a USD-margined forward contract to buy 1 BTC at 100K USD. Physical delivery means the holder receives 1 BTC, despite everything else being denominated in USD.

**Definition.** A (margined) contract is leveraged if it allows a trader to control a position whose notional value exceeds the amount of collateral provided.

Leverage increases risk exposure.

**Example.** Consider a leveraged position with a leverage of 10x: Alice deposits 1 USD to control a position of 10 USD. If her position depreciates by 10％, her entire collateral will be depleted and her position liquidated. On the other hand, if her position appreciates by 10％ then its value will rise by 1 USD, i.e. a potential 2x profit on Alice's collateral of 1 USD.

## Examples

This section is dedicated to common types of derivatives. Apart from forwards, here's the table to keep in mind. We'll start with examples and end with a comparative overview.

|                 | Futures     | Options   |
|-----------------|-------------|-----------|
| **Expiring**    |             |           |
| **Perpetual**   |             |           |

### Expiring derivatives

Here we focus on with expiration dates for a trade (either right or obligation).

As we approach expiry, risk diminishes since there is less room for a gap between the position and market conditions at expiry. Thus the market price of an expiring derivative converges to a limit which becomes clearer toward expiry.

For instance, if Alice has a forwards position to buy 1 BTC for 100K USD today at noon, and the spot price at 11:50 is 110K USD per BTC, the market price for the forwards position will approach the 10K USD difference.

Of course convergence need not be monotone and there can be many violent fluctuations along the way. Nevertheless, the expiry date is a clear anchor to the spot market.

#### Forwards

**Definition.** A forwards contract is a contract without MtM specifies a predetermined trade at predetermined exchange rates and at a predetermined trade.

```
ForwardPosition 
  // Core agreement
  maker                   // Party agreeing to sell (e.g. Alice)
  taker                   // Party agreeing to buy (e.g. Bob)
  underlying              // Index tracked for settlement (e.g. BTC/USD)
  denomination            // Settlement currency (e.g. USD)
  notional amount         // Quantity of denomination asset (e.g. 100K USD)
  forward rate            // Agreed exchange rate (e.g. 100K USD per BTC)
  initiation date         // Date of contract creation
  settlement date         // Maturity date

  // Margin and collateral (optional in TradFi, typical in DeFi)
  margin asset            // Collateral currency (e.g. USD)
  maker collateral        // Posted by seller
  taker collateral        // Posted by buyer
```

The required collateral can be low in case parties are willing to assume counterparty risk. In untrusted environments such as DeFi, full collateral would be needed to ensure solvency at expiry, causing tremendous capital inefficiency. Other designs (e.g. synthetic debt model) can eliminate capital costs on the buyer at the expense of exacerbated capital costs on the seller.

#### Expiring futures

**Definition.** A expiring futures contract is a MtM contract with expiry, specifying a sequence of trades at specified dates and with specified notional and payoff structure, but whose effective exchange rates are determined dynamically at each MtM. PnL is settled via specified variation margin logic.

```
FuturesPosition
  // Core agreement
  trader                  // Position holder
  counterparty            // Optional — anonymous if exchange-traded
  underlying              // Index tracked (e.g. BTC/USD)
  denomination            // PnL/collateral denomination (e.g. USD)
  notional amount         // Exposure size in denomination terms
  initial rate            // Entry price of the futures contract
  initiation date         // When the position was opened
  settlement date         //

  // Position terms
  direction               // "long" or "short"
  leverage                // Optional leverage multiplier (1x = unleveraged)

  // Margin and collateral
  margin asset            // Collateral currency (e.g. USD)
  initial margin          // Collateral deposited at entry
  margin balance          // Current collateral after MtM adjustments
  margin requirements     //
    maintenance margin    // Min collateral to avoid liquidation
    variation margin      // Delta from MtM accounting

  // PnL tracking
  last mark price         // Oracle price at last MtM tick
  unrealizedPnL           // On remaining open size
  realizedPnL             // From closed or partially closed trades

```

#### European options

**Definition.** A European options contract specifies a potential trade at predetermined exchange rates and at a predetermined expiry date, where the holder has the right but not the obligation to execute the trade. The contract includes an upfront premium and payoff conditions that depend on the underlying asset's value at expiration.

```
OptionsPosition
  // Core agreement
  holder                  // Option buyer (has the right, not obligation)
  writer                  // Option seller (has the obligation)
  option type             // "call" or "put"
  underlying              // Reference index or asset (e.g. BTC/USD)
  denomination            // Settlement currency (e.g. USD)
  notional amount         // Quantity of underlying (e.g. 1 BTC)
  strike price            // Exercise price (e.g. 30,000 USD/BTC)
  initiation date         // When the contract is created
  expiration date         // Option maturity date
  premium                 // Price paid by holder at initiation
  type                    // European (settle at expiry) or American (exercise anytime)
  delivery                // Physical vs cash

  // Margin and collateral
  margin asset            // Collateral currency (e.g. USD)
  writer collateral       // Margin posted by the writer (may be required)
  holder collateral       // Optional; rare in TradFi, more common in DeFi (see below)
  margin requirements     //
    maintenance margin    // Minimal collateral to avoid liquidation
```

In DeFi, holder collateral serves to facilitate automatic exercise. If the holder is "in the money" (it's profitable to exercise) at expiry, it would be nice to enjoy automatic exercise without requiring the holder to be online. Holder collateral is the budget allocated to cover such auto-exercise costs. It increases in case of physical delivery, since a trade of USD for BTC, then both assets need to be posted upfront in their full amounts.

### Perpetual derivatives

Perpetual derivatives originate in Robert Shiller's *Macro Markets: Creating Institutions for Managing Society's Largest Economic Risks*. They allow parties to enter hedging/speculation positions that never expire.

As remarked above, expiry provides an eventual anchor to the underlying spot market. Due to the absence of expiry, perpetuals use a different, *continuous* anchoring mechanism called *funding* to align position prices with the underlying market. In a nutshell, perpetual positions that are overpriced with respect to the underlying market pay a *funding rate* to the underpriced positions. The resulting PnL is thus closer to the underlying market.

Funding accounts for only *part* of the variation margin in perps. On one hand there is the "pure PnL" due to changes in the perp market, independent of funding. On the other hand, there is the "funding PnL" due to the gap between two markets: perp and its underlying market.

* Suppose Alice longs BTC at 100K USD and the price rises to 110K USD.
  * If the perp market is aligned with the underlying, no funding rate is paid and that's the end of the story.
  * If most of the perp market is long, Alice will need to pay a funding rate which will diminish her profits.

Expiry causes liquidity fragmentation over various expiration dates. Typically, liquidity improves as expiry nears due to lessened risk. Perpetuals benefit from greater liquidity in this regard.

#### Perpetual futures

**Definition.** A perpetual futures contract is a MtM contract without expiry, specifying a sequence of trades at specified dates and with specified notional and payoff structure, but whose effective exchange rates are determined dynamically at each MtM. PnL is settled via specified variation margin logic.

```
PerpetualFuturesPosition
  // Core agreement
  owner                   // Position holder
  counterparty            // Matched trader on the other side (implicitly tracked in DeFi)
  position direction      // "long" or "short"
  underlying              // Reference index or asset (e.g. BTC/USD)
  denomination            // Settlement currency (e.g. USD)
  notional amount         // Quantity of exposure (e.g. 1 BTC)
  entry price             // Execution price at which position was opened
  entry timestamp         // Block timestamp or date of entry
  funding rate model      // Logic or formula for periodic funding calculation
  funding accrued         // Cumulative funding paid/received since entry
  realized pnl            // PnL from closed portion of position
  unrealized pnl          // Mark-to-market PnL on open position

  // Margin and collateral
  margin asset            // Collateral currency (e.g. USDC)
  margin balance          // Collateral currently posted
  maintenance margin      // Minimum margin required to avoid liquidation
  initial margin          // Margin required to open position (based on leverage)
  leverage                // Effective leverage (e.g. 5x)
  liquidation threshold   // Price at which position will be force-closed
  liquidation penalty     // Penalty or fee imposed upon liquidation

  // Liquidation and risk
  insurance coverage      // Portion covered by protocol insurance fund (if any)
  oracle index.           // Price feed used for MtM and funding
```

Check out [this post by Paradigm](https://www.paradigm.xyz/2021/03/the-cartoon-guide-to-perps) for some mental models of perps.
