---
layout: post
title: "Blurb on Loans"
date: 2025-08-20
tags: [DeFi, lending, borrowing, loan, money market, liquidity]
comments: true
mathjax: true
author: IV
---

# Economics of loans

Alice has a potato. She has no urgency to act with it now - she only plans to consume it in a week. Bob doesn't have a potato, but wants one *now*. Bob could buy Alice's potato in exchange for some stuff, end of story. Alas, things may not be so simple:
* Maybe Bob *can't* buy the potato because he doesn't own anything for which Alice is willing to trade.
* Even if Bob can, perhaps he *doesn't want to* trade his stuff away into Alice's ownership.

In either case, Bob must find a different kind of trade. The astute reader will have noticed that the spot trade above made no use of the asymmetric urgency between Alice and Bob. A *loan* is a trade founded precisely on asymmetric urgency.

In an effort to satisfy his urgent want for a potato, Bob makes the following offer to Alice:
* Alice will give Bob a potato now.
* Bob will repay a potato to Alice in a week.
* Over the course of the week, Bob will also pay Alice for allowing him to delay repayment by a week.

Bob is offering to borrow a potato from Alice. As a lender, Alice is able to generate revenue on her *current* ownership of a potato *together with her lack of urgency* in acting with it, by temporarily foregoing ownership in exchange for some interest payment (in addition to repayment of the potato). In economic literature, urgency is often referred to as 'high time preference', whereas 'low time preference' means a lack of urgency.

In case of trust issues, Alice may require Bob to deposit some stuff as collateral, to insure herself should he wander off into the night with her potato. Undercollateralized loans are customary in the context of legal enforcement. In DeFi, the standard is either overcollateralized loans or trustless loans (e.g. atomic/flash loans).

Let's formalize a little. Recall a contract has *margin requirements* if it specifies how much collateral/margin all parties must provide to enter the contract and maintain in order to keep the position open without liquidation.

**Definition.** An expiring/perpetual loan is a financial contract between two parties (borrower, lender) and comprised of a principal, margin requirements (including collateral and interest rate), and an expiring/perpetual repayment schedule. It is structured as follows.
* At inception:
    * the lender transfers a principal amount of asset X to the borrower;
    * the borrower transfers some amount of asset Y to the lender as collateral/margin.
* The borrower takes on obligation to fulfill a pre-specified future repayment schedule subject to margin requirements.
* Violation of margin requirements results in liquidation, i.e. confiscation of the borrower's collateral.

# Loans in DeFi

It may seem counterintuitive that borrowers would enter overcollateralized loans. The motivation is to maintain exposure to their assets instead of forgoing it. At any rate, motivations for entering overcollateralized positions as part of profit strategies is outside the scope of this post but not required for what follows. Our focus will be architecture of on-chain loan markets.

## AAVE

Each version of AAVE consists of a single `LendingPool` contract that specifies logic for managing loans across multiple asset pools internal to `LendingPool`. The user interface consists of the following functions. There's another method to enable/disable a given asset as collateral, but we'll disregard it in this post.
1. `supply()` capital into a collateral pool. In AAVE, a collateral supply is required for both lenders (obviously) and borrowers.
2. `withdraw()` principal + interest from a collateral pool.
3. `borrow()` assets if the resulting loan remains above AAVE's computed liquidation threshold.
4. `repay()` a loan (principal + interest), either fully or partially.
5. `liquidationCall()` allows a liquidator to cover part of an existing debt (asset X) and receive part of the collateral (asset Y). The liquidators payout is determined by AAVE logic.

Before telling a story, let's summarize the plot. Anyone can supply assets. Borrowers of asset X must repay with interest, also denominated in asset X. Suppliers for each pool are eligible for shares of the interest, in proportion to their fraction of the supply. More borrows → more interest payments → more yield for suppliers/lenders. Borrowing asset X requires the borrower to have supplied a sufficient amount of asset Y, which is viewed as collateral. If the collateral in a borrowed position depreciates with respect to the borrowed asset, the position may be liquidated by anyone. Liquidators cover debt and receive the collateral at a discount. AAVE itself takes a cut of all interest payments. AAVE governance specifies the various parameters e.g. liquidation thresholds, etc.

Now with the soulless summary behind us, We'll tell the story first from the lender PoV first. Then we'll dive into borrowing, collateral, and liquidations.

### Lending

Alice the lender supplies the protocol some ERC-20 token TKN. In exchange she receives an ERC-20 IOU called aTKN, redeemable 1:1 for TKN. There is some subtlety around aTKN worth clearing up before moving on. AAVE only burns aTKN when users withdraw and only mints when users supply, but not upon repayment: when Bob repays 12 TKN on his 10 TKN loan, the 2 TKN of interest does not have a *minted* aTKN counterpart. Thus if we account only for mints and burns, AAVE's TKN supply will exceed the aTKN supply. But it ain't so! Suppose Alice supplied 100 TKN. She received 100 newly minted aTKN. Suppose Bob was the sole borrower. If Alice queries her aTKN balance after Bob has fully repaid his 10 TKN loan, she will discover 101 aTKN (not 102). So what's going on? What are aTKN's ERC-20 methods `balanceOf` and `totalSupply` really doing? Why 101 and not 102?
* First of all, the aTKN contract performs "naive accounting" via internal mappings. For instance, it has a `_balances`_ mapping that disregards interest payments and tracks only principal. Specifically, `_balances` changes only upon `supply()`, `withdraw()`, and `transfer()`.
* The methods defined by the ERC-20 interface simply account for interest on top of the principal. Specifically, there is a `liquidityIndex` representing interest, and the ERC-20 `balanceOf(address)` simply returns the `_balances(address)*liquidityIndex`. We will dive into interest accounting below.
* So why does Alice have 101 aTKN and not 102? Well, the AAVE protocol takes a cut of interest payments. In this example the cut is 50％: half of the 2 TKN interest is diverted into the treasury.

At this point you may be suspicious of redemptions, and rightly so.
* By construction, AAVE operates on full reserves: the supply of aTKN cannot exceed the amount of supplied TKN.
* However, if part of TKN supply is borrowed then only part of aTKN supply can be instantly redeemed at a given moment. Specifically, the amount of instantly redeemable aTKN equals the *idle* supply of TKN. If 3/10 TKN is borrowed then 7/10 TKN is idle whence only 7/10 aTKN can be instantly redeemed.
* Note any injection into the TKN pool (via borrowers repaying or lenders supplying more) increases the capacity of instant redemption. Still, borrowed positions bound the fraction of instantly redeemable aTKN.

So how does Alice earn?
* In the happy flow, Bob the borrower repays his 10 TKN loan with interest, for a total of 12 TKN repaid. The AAVE protocol treasury takes a cut of the interest, say 1 TKN. Alice's aTKN balance accounts for interest minus the protocol's cut, and grows to 101 aTKN, which she can redeem for 101 TKN. As remarked above, redemptions require enough idle funds, so instant redemption may be temporarily out of reach. Note Alice's earning are denominated in the asset she lends.
* In the unhappy flow, Bob the borrower's position enters liquidation territory. Anyone can initiate a liquidation on such positions. A liquidator pays off some amount of the borrowed asset, and receives some of the borrower's collateral at a discount rate. More on liquidations below.
* In the *very* unhappy flow, the collateral itself depreciates with respect to the borrowed asset so the borrower's position becomes nominally undercollateralized. In this extreme case, liquidating the entire position is unprofitable, since the face value of the debt (which the liquidator covers) exceeds the face value of the collateral even after discount (which the liquidator receives). In such a scenario rational liquidators will remain idle since they cannot profit. As a last resort, AAVE governance could cover debt from the protocol's treasury. More on risk and solvency below.

### Borrowing

Good, good. So how does Bob borrow?
1. To borrow asset X, Bob must first lend (i.e. `supply()`) asset Y and mark it as collateral. !@!@$!@$!@$
2. The numbers are dictated by...

### Liquidations

That leaves us with a final question: how does Saruman liquidate?

As mentioned above, liquidation requires some minimal capital bla bla bla...

### Risk and solvency

Solvency means assets cover liabilities. Strictly speaking, even in the happy flow AAVE may be insolvent

Full reserves...

Who manages risk? "AAVE as an entity manages all the risk" https://x.com/martinkrung/status/1783092537353248972

**Remark.** aTKN is a tradable asset and its value as a 1:1 redeemable IOU derives largely from risk and redemption delay... MORE ON THIS L8A

## Morpho

See https://ethereum.stackexchange.com/questions/166788/is-it-an-invariant-in-aave-v3-that-1-ausdc-can-always-be-redeemed-for-1-usdc
See also https://morpho.mirror.xyz/hfVXz323zp9CmJ1PLDL4UhdLITGQ865VIJUyvZYyMA4

and also https://governance.aave.com/t/arfc-onboard-pendle-pt-tokens-to-aave-v3-core-instance/20541