# Section 03. Fund Me

## Introduction

Our project will be like a Blockchain Kickstarter. We will write 2 contracts `FundMe.sol` and `PriceConverter.sol`, with the following features:

- Users can send any native blockchain currency
- Owner of the contract can withdraw all the funds collected
- A minimum USD amount to donate will be set

## Setup

Let's create a `FundMe.sol` file in Remix and lay out the basic structure:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.26;

contract FundMe {
    function fund() public {}
    function withdraw() public {}
}
```

## Sending ETH through a function

When a transaction is sent to the blockchain, a value field is always included in the transaction data. For a function to be able to receive Ethereum, it must be declared `payable`:

```solidity
function fund() public payable {
    // allow users to send $
    // have a minimum of $ sent
    // How do we send ETH to this contract?
    msg.value;

    //function withdraw() public {}
}
```

In Solidity, the **value** of a transaction is accessible through the `msg.value` property.

### Reverting transactions with `require`

We can use the`require` keyword as a checker, to enforce our function to receive a minimum `value` of one (1) whole ether:

`require(msg.value > 1e18); // 1e18 = 1 ETH = 1 * 10 ** 18`

The require statement in Solidity can include a custom error message, which is displayed if the condition isn't met, clearly explaining the cause of the transaction failure:

`require(msg.value > 1 ether, "Didn't send enough ETH"); //if the condition is false, revert with the error message`

Notice how we can replace `1 * 10 ** 18` with `1 ether`. You can check all the ether units available in the links.

## Introduction to Oracles

This lesson will walk you through adding a **currency conversion feature** to the `fundMe` contract and setting **price thresholds** with Chainlink Oracle, a decentralized network for external data.

### USD values

We can begin by specifying the new value with a state variable `uint256 public minimumUSD = 5` at the top of the contract.

Next step would be changing the condition inside the `fund` function, to check if the `value` sent is equal to or greater than our `minimumUSD`. But here's the tricky part: the `minimumUSD` value is in USD while the transaction message value is specified in ETH.

### Decentralized oracles

The USD price of assets like Ethereum cannot be derived from blockchain technology alone but it is determined by the financial markets. To obtain a correct _price information_, a connection between off-chain and on-chain data is necessary. This is facilitated by a _decentralized Oracle network_.

This limitation exists because blockchains need to be deterministic in nature.

For smart contracts to effectively replace traditional agreements, they must have the capability to interact with **real-world data**. Relying on a centralized Oracle for data transmission is inadequate as it reintroduces potential failure points.

### How Chainlink works

Chainlink is a _modular and decentralized Oracle Network_ that enables the integration of data and external computation into a smart contract.

These are some of the Chainlink features that we will learn to use:

- Data Feeds
- Verifiable Random Number
- Automation (previously known as 'Keepers')
- Functions

### Chainlink Data Feeds

_Chainlink Data Feeds_ are responsible for powering over $50B in the DeFi world. Chainlink nodes aggregate data from various exchanges and data providers, with each node independently verifying the asset price.

They aggregate this data and deliver it to a reference contract, the **price feed contract**, in a single transaction. Each contract will store the pricing details of a specific cryptocurrency

### Chainlink VRF (Verifiable Random Function)

VRF provides a solution for generating **provably random numbers**, ensuring true fairness in applications such as NFT randomization, lotteries, and gaming. These numbers are determined off-chain, and they are immune to manipulation.

### Chainlink Automation

These **nodes** listen for specific events and, upon being triggered, automatically execute the intended actions within the calling contract.

### Chainlink Functions

Functions allow **API calls** to be made within a decentralized environment. This feature is ideal for creating innovative applications and is recommended for advanced users with a thorough understanding of Chainlink ecosystem.

## Quick Recap

For now, here's an example of how you could use Chainlink to retrieve on-chain the latest price of gold:

```solidity
import "@chainlink/contracts/src/v0.6/interfaces/AggregatorV3Interface.sol";
contract GoldPriceContract {
    AggregatorV3Interface internal priceFeed;
    // The Chainlink price feed contract address
    constructor() public {
        priceFeed = AggregatorV3Interface(0x8468b2bDCE073A157E560AA4D9CcF6dB1DB98507);
    }
    // Get the latest gold price
    function getLatestGoldPrice() public view returns (int) {
        (,int price,,,) = priceFeed.latestRoundData();
        return price;
    }
}
```

## ❓ Questions and 💪 Exercises

Exercise 💪:

Question ❓:

## 🛠️ Links and Resources

- [GitHub Repo](https://github.com/Cyfrin/remix-fund-me-cu)
- [Ethereum Unit Converter](https://eth-converter.com/)
- [Ether Units](https://docs.soliditylang.org/en/latest/units-and-global-variables.html#ether-units)
- [Chainlink](https://chain.link/)
- [Chainlink Docs](https://docs.chain.link/)
