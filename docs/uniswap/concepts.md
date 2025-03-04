# Uniswap - Concepts

- Uniswap Overview
  - The difference between Uniswap Labs, Foundation, Protocol, Interface and Governance.
- The Uniswap Protocol
- Protocol Concepts
- Governance
- Research
- Resourcse
- Glossary

## What is the Uniswap Protocol?

Uniswap protocol is a peer-to-peer system for exchanging ERC-20 tokens on multiple blockchains. The protocol consists of a set of non-upgradeable smart contracts. It prioritizes censorship resistance, security and self-custody.

There are 4 versions of Uniswap:

- v1 and v2 are open source.
- v3 introduced concentrated liquidity and is open source with slight modifications
- v4 introduces a singleton pool architecture and hooks to enable protocol customization
- Each version of Uniswap, once deployed, will function in perpetuity

## How does the Uniswap protocol compare to a typical market?

Let's first look at 2 subjects:

- How the AMM design deviates from traditional central limit order book-based exchanges
- How permisionless systems are different from permissioned systems

### Order book vs AMM

Instead of using an order-book system, Uniswap uses an Automated Market Maker, sometimes called Constant Function Market Maker.

At a very high level, an AMM replaces the buy and sell orders in an order book market with a liquidity pool of two assets, both valued relative to each other. As one asset is traded for the other, the relative prices of the two assets shift, and a new market rate for both is determined. In this dynamic, a buyer or seller trades directly with the pool, rather than with specific orders left by other parties.

## Protocol Concepts - Hooks

Uniswap v4 inherits all of the capital efficiency gains of Uniswap v3.

The key innovations are the Hook System and Singleton Architecture, which together enable unprecedented protocol customization and gas optimization.

### Hooks

Hooks allow developers to customize and extend the behavior of liquidity pools.

They are external smart contracts that can be attached to individual pools to intercept and modify the execution flow at specific points during pool-related actions.

The logic is executed before and/or after major operations such as pool creation, liquidity addition and removal, swapping, and donations.

Through these hook functions, developers can build sophisticated features like:

- custom AMMs with different pricing curves
- yield farming protocols
- advanced trading features including:
  - limit orders
  - dynamic fee strategies
- custom oracle implementations

Each pool can have one hook (though a hook can serve multiple pools), hooks are optional and specified during pool creation, and developers can implement any combination of hook functions based on their needs.

### Singleton Architecture

Unlike previous versions where each pool was a separate smart contract, v4 manages all pools through a single contract called the PoolManager. Improvements:

- Efficient Pool Creation: Pools are created as state updates rather than contract deployments, significantly reducing gas costs
- Gas Optimization: Multi-hop swaps and complex operations are streamlined through a single contract
- Flash Accounting: Token balances are tracked internally and settled at the end of transactions, minimizing transfers
- Native ETH Support: Direct ETH trading without the need to wrap to WETH, improving user experience
