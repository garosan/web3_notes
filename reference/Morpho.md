- [Morpho](https://docs.morpho.org/)

We can see in the docs that there are 6 products or main topics:

- Morpho
- Morpho Vaults
- Bundlers
- Public Allocator
- Rewards
- Morpho Optimizers (deprecated)

Let's take a look at the main product, the Morpho Protocol:

Morpho enables the deployment of minimal and isolated lending markets by specifying:

- one collateral asset,
- one loan asset,
- a Liquidation Loan To Value (LLTV),
- an Interest Rate Model (IRM),
- and an oracle.

designed to be more efficient and flexible than any other decentralized lending platform.

Protocol addresses:
https://docs.morpho.org/morpho/addresses/

Morpho has a permissionless market creation mechanism, where anyone can create a market with a different collateral token, borrowable token and oracle (IRM & LLTV are allowed by governance).

Such as Uniswap, you can be vulnerable to loss of funds if you use a random market where the oracle or the tokens involved in the market can be dumb. So, we recommend you use only the markets that are listed in the Morpho Interface.

## Morpho Overview

Morpho enables overcollateralized lending and borrowing of ERC20 & ERC4626 tokens on the EVM. The protocol is immutable. Once deployed, Morpho will function in perpetuity.

A decentralized, overcollateralized lending and borrowing protocol is an autonomous system that allows users to borrow assets by providing more collateral than the value of the borrowed assets and lenders to earn interest on supplied assets.

Basic features and components:

- Collateralization
- Liquidation Loan-To-Value (LLTV)
- Borrowing
- Interest Rates
- Repayment
- Liquidation Mechanism: The position may be liquidated in part or full to repay the loan and any outstanding interest.
- Lending
- Withdrawal

A distinctive feature of Morpho is permissionless market creation: the protocol allows users to create isolated markets consisting of one loan asset, one collateral asset, a Liquidation Loan-To-Value (LLTV), an oracle, and an interest rate model (IRM).

The key difference with traditional lending platforms is traditional platforms:

- Require governance approval for asset listing and parameter changes.
- Pool assets into a single lending pool, sharing risk across the entire protocol.

In Morpho, each parameter is selected at market creation and is immutable.

The LLTV and interest rate model must be chosen from a set of options approved by Morpho Governance.

The following LLTVs have been DAO-approved: [0%; 38.5%; 62.5%; 77.0%; 86.0%; 91.5%; 94.5%; 96.5%; 98%].

The only Interest Rate Model (IRM) that has been DAO-approved is the AdaptiveCurveIRM.

Once a market is created users can engage with certainty that:

- It will persist as long as the blockchain in which it was deployed persists
- The parameters will never change.

In Morpho, markets are named in the following format:

`CollateralAsset/LoanAsset (LLTV, ORACLE, IRM)`, i.e.:

`wstETH/WETH (94.5%, ChainlinkOracle, AdaptiveCurveIRM)`

Each market will have an id, which is a hash of the market parameters (more on this later).

## Externalized Risk Management

Traditional lending platforms allow token holders to participate in risk management on behalf of the users.

Example Scenario:

Let's say there's a proposal to add SHIB (Shiba Inu) as collateral on Aave.
AAVE token holders vote on:
Whether SHIB should be listed.
What LTV it should have (e.g., SHIB can be used as collateral, but only up to 30% of its value).
The liquidation threshold (e.g., if SHIB drops below 35% of its value, it gets liquidated).
If the proposal passes, all users of Aave must abide by these risk parameters, even if their personal risk tolerance is different.

This approach restricts the number of listed assets, confines users to a single risk profile, and is not scalable. Morpho removes these limitations by separating risk management from the protocol.

Morpho is designed to leave choices up to the users. It allows anyone to create markets with any loan asset, collateral asset, risk parameters, or oracle and for users to interact with any deployed market.

### Morpho Vaults

Morpho Vaults introduce a permissionless risk management layer on top of Morpho markets, allowing curators to create lending vaults that automate risk management for depositors.

Morpho Vaults are only one example of permissionless risk management on top of Morpho. Any entity, DAO, or protocol can build services and tools that help manage risk on behalf of users.

A lender has to do a lot of analysis to know how to lend their assets. This is a burden for passive lenders, who might not have the expertise or time to do this analysis.

Morpho Vaults automate risk assessment and fund allocation on behalf of lenders.

🔹 A "Vault" is a smart contract that aggregates funds from multiple lenders.
🔹 A "Curator" manages the vault’s risk parameters and strategy.
🔹 The Vault then supplies funds across different Morpho markets in an optimized way.

### **Example Scenario: Lending WETH via Morpho Vaults**

Let's say you want to **lend WETH** but don’t want to research the best markets.

### **Without Morpho Vaults (Traditional Lending)**

1. You deposit WETH directly into **one** Morpho market (e.g., WETH/USDC).
2. You have to **manually check** risk parameters: collateral types, LLTV, IRM, and oracle risk.
3. If a better lending opportunity arises in another market (e.g., WETH/DAI), you need to **move your funds manually**.

### **With Morpho Vaults**

1. You deposit **WETH** into a **Morpho Vault**.
2. The Vault **automatically allocates WETH** to different Morpho markets with **the best balance of yield and risk**.
   - Example: It might supply WETH to **three different markets**: WETH/USDC, WETH/DAI, and WETH/wstETH.
3. The **Curator** defines the vault’s strategy, ensuring:
   - Funds are only supplied to safe, profitable markets.
   - Risk parameters (LLTV, oracles, IRM) are constantly monitored.
   - The Vault rebalances liquidity dynamically.

This means you, as a **passive lender**, don’t need to manually track or optimize your funds. The Vault does it for you.

## Minimize Governance

Morpho is designed to be a base layer for decentralized lending. Alongside immutability, removing the ability for governance to halt or modify markets is key to enabling completely trustless lending and borrowing.

## Liquidation

When an account becomes unhealthy, meaning its Loan-To-Value (LTV) on a given market exceeds the market’s Liquidation Loan-To-Value (LLTV), the account’s position can be liquidated. Anyone can perform this liquidation.

To avoid being liquidated, borrowers can track their LTV in real-time and either repay their loan, partially or entirely, either add more collateral to their position.

### To compute the LTV of a position on Morpho, use the following formula:

LTV = (BORROWED AMOUNT _ ORACLE_PRICE) / (COLLATERAL AMOUNT) _ (ORACLE_PRICE_SCALE)

ORACLE_PRICE_SCALE = 1e36.

Example of LTV calculation in Solidity:
https://github.com/morpho-org/morpho-blue/blob/12b8a453643d5ef9d55abd88b9f8cfa866882aa5/src/Morpho.sol#L532-L536

### How Liquidations Work

No fee is taken by the Morpho protocol at this level. The entire Liquidation Incentive Factor (LIF) goes to the liquidator.

## Oracle-Agnostic Pricing

For a lending protocol to operate, it needs an accurate notion of price.

Determining a price without using oracles requires a lending protocol to have internal trading markets, also known as an oracle-less approach. However, this brings additional complexity, increasing gas consumption and compromising auditability and security.

To avoid compromising efficiency and security, Morpho is oracle-agnostic rather than oracle-less.

Anyone can create a market by specifying an oracle address. Some markets can feature oracles such as Chainlink, Redstone or Uniswap, while other markets may have prices hardcoded or use a mechanism similar to the one seen in Ajna.
