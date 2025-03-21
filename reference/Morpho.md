## Table of Contents

- Overview
- Concepts
- Tutorials
- Contracts
- Community Contributions
- FAQs

## General Overview

Different areas

- Morpho Protocol
- Morpho Interface
- Morpho Governance
- Morpho Labs
- Morpho Association

'Morpho Procotol' is the most recent version. Before that came 'Morpho Optimizers'.

There are 3 instances of Morpho Optimizers:

- Morpho AaveV3-ETH Optimizer,
- Morpho AaveV2 Optimizer,
- Morpho CompoundV2 Optimizer.

# Morpho Docs

## Concepts Overview

- Morpho is a protocol enabling the overcollateralized lending and borrowing of ERC20 & ERC4626 Tokens on the EVM
- Implemented as an immutable smart contract
- Once deployed, Morpho will function in perpetuity

Basic features and components of the Morpho protocol:

- **Collateralization**:
- **Liquidation Loan-To-Value (LLTV)**:
- **Borrowing**
- **Interest Rates**: The amount of interest paid is based on the interest rate model used by the protocol.
- **Repayment**
- **Liquidation Mechanism**: The position may be liquidated in part or full to repay the loan and any outstanding interest.
- **Lending**
- **Withdrawal**

➡️ Permissionless market creation

Any user can create an isolated market which consists of:

- One loan asset
- One collateral asset
- an LLTV
- an oracle
- an Interest Rate Model (IRM)

This is different from traditional lending platforms like AAVE in that:

- No governance approval required for asset listing and parameter changing
- Each market is isolated, risk is not shared since all the assets are not in one pool

- In Morpho, each parameter is selected at market creation and it is immutable.
- _The LLTV and interest rate model must be chosen from a set of options approved by Morpho Governance_.
- Only these LLTVs have been DAO approved: [0%; 38.5%; 62.5%; 77.0%; 86.0%; 91.5%; 94.5%; 96.5%; 98%]
- The only IRM approved is the [AdaptiveCurveIRM](https://docs.morpho.org/morpho/concepts/irm/#the-adaptivecurveirm)

➡️ Morpho Market Naming

Markets are named based on their individual parameters in the following format:

`CollateralAsset/LoanAsset (LLTV, ORACLE, IRM)`

i.e.: `wstETH/WETH (94.5%, ChainlinkOracle, AdaptiveCurveIRM)`

Each market will have an `id`, which is a hash of the [market parameters](https://docs.morpho.org/morpho/contracts/morpho/#market-id).

## Externalized Risk Management

In traditional lending platforms, token holders decide risk management on behalf of users which:

- Limits the number of listed assets
- Confines users to a single risk profile

In Morpho instead, anyone can create markets with any loan asset, collateral asset, risk parameters, or oracle.

Advanced lenders benefit from the added flexibility, but for regular users, risk management layers can be rebuilt on top of the protocol to simplify the user experience. One example are Morpho Vaults: a risk management protocol that facilitates the creation of lending vaults on top of Morpho markets.

Morpho Vaults are only one example of permissionless risk management on top of Morpho. Any entity, DAO, or protocol can build services and tools that help manage risk on behalf of users.

## Liquidation

When an account becomes unhealthy, meaning its LTV on a given market exceeds the market's Liquidation Loan-To-Value (LLTV), the account's position can be liquidated.

To avoid being liquidated, borrowers can either repay their loan, partially or entirely, or add more collateral to their position.

To compute the LTV of a position, use the formula:

`LTV = (BORROWED AMOUNT * ORACLE_PRICE) / (COLLATERAL AMOUNT) * (ORACLE_PRICE_SCALE)`

where `ORACLE_PRICE_SCALE = 1e36`.

Example of LTV calculation in Solidity:
https://github.com/morpho-org/morpho-blue/blob/12b8a453643d5ef9d55abd88b9f8cfa866882aa5/src/Morpho.sol#L532-L536

➡️ How Liquidations Work

// TODO: Come back and explain this better https://docs.morpho.org/morpho/concepts/liquidation

No fee is taken by the Morpho protocol at this level. The entire Liquidation Incentive Factor (LIF) goes to the liquidator.

## Oracle-Agnostic Pricing

For a lending protocol to operate, it needs an accurate notion of price.

Determining a price without using oracles requires an oracle-less approach. However, this brings additional complexity, increasing gas consumption and compromising auditability and security.

To avoid compromising efficiency and security, Morpho is oracle-agnostic rather than oracle-less.

Anyone can create a market by specifying an oracle address. Some markets can feature oracles such as Chainlink, Redstone or Uniswap, while other markets may have prices hardcoded or use a mechanism similar to the one seen in Ajna.

## Interest Rate Models

## Links

- [Morpho Docs](https://docs.morpho.org/)
- [Addresses](https://docs.morpho.org/addresses)
