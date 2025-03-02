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
