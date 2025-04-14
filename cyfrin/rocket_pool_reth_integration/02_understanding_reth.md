# Section 02. Understanding rETH

## ETH Staking

We will beginning by understanding the challenges of ETH staking and what is the problem that Rocket Pool solves.

The purpose of ETH staking is to:

- Run a validator
- Secure the network
- Earn ETH rewards

The requirements are:

- 32 ETH
- Validator Key
- Withdrawal Key
- Run Validator

So to become a staker you need:

- 32 ETH
- Deep tech skills to run your validator and secure your ETH

## Rocket Pool

Rocket Pool is a decentralized ETH staking protocol.

In Rocket Pool you can add your little amount of ETH to the RocketPool contracts to _pool_ the ETH to stake, but also if you have technical knowledge you can register to participate in the network as a node operator. The difference would be that you would be operating RocketPool's software instead of an Ethereum node.

## rETH

rETH is a liquid staking token issued by Rocket Pool. There are two ways to obtain rETH.

- The first is to mint rETH by directly depositing ETH into the Rocket Pool protocol. For this to happen, minting must be enabled by the Rocket Pool protocol.

- The second way is to obtain rETH from a decentralized exchange. You can swap any tokens to buy rETH.

For the 1st way, the exchange rate is not one-to-one. In the example Alice deposits 1 ETH and gets back 0.83 rETH. This is because the rETH token is expected to accrue value over time if everything goes well.

Minting is activated by the Rocket Pool protocol and is not always active. Depending on market conditions and trading fees, it can be more advantageous to mint the rETH or to buy it in an exchange.

## Value of rETH

Today Apr 13th, 2025 rETH is $1830 and ETH is $1615.

rETH increases in value relative to ETH because it accumulates staking rewards.
Of course, if the node operators of the Rocket Pool are penalized for some kind of misbehavior, then the value of rETH can decrease relative to ETH.

The graph for rETH would look like an ascending stair because the rETH team adjusts the exchange rate periodically.

## Rebase Token

- Rocket Pool rETH is a non-rebase token.
- Lido stETH is a rebase token.

A rebase token means that the token supplies and the balances change algorithmically.

The algorithm will assign extra tokens to the users, the users will burn tokens and this will bring back down the supply.

### Non-rebase token

A non-rebase token means that the token supply and balances of a token only change when there's a mint or a burn.

### Taxes

Due to the way the token balances change (or don't change), there might be different tax implications between rETH and stETH.
