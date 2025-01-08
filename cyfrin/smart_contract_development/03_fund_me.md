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

## Solidity interfaces

Let's continue writing code for our `FundMe.sol` contract and create these 2 functions:

```solidity
function getPrice() public {}
function getConversionRate() public {}
```

You can see all about Chainlink Data Feeds in [this](https://docs.chain.link/data-feeds) link.

Now to interact with the Chainlink contract that will give us the price data, we will need its address and ABI. You can get the contract address from this page:

- [Price Feed Contract Addresses Page](https://docs.chain.link/data-feeds/price-feeds/addresses?network=ethereum&page=1)

What about the ABI? Well if we think about it, the ABI is basically just a detail of the functions of a contract and their properties, like if the function is payable, external, etc. There are other ways to call the functions of an external smart contract like knowing the function selector, but for now, we will use an _interface_.

Basically what an interface does in this case is it is just code for the functions and their properties, signatures, etc. (like an ABI!) so we will import the `AggregatorV3Interface` from Chainlink and use it.

## Import from GitHub

Let's import the `AggregatorV3Interface` from GitHub instead of what Patrick did of copying the whole interface into the file.

```solidity
 pragma solidity 0.8.26;
 import {AggregatorV3Interface} from "@chainlink/contracts/src/v0.8/interfaces/AggregatorV3Interface.sol";
 contract FundMe {}
```

## Getting real world data from Chainlink

So, to recap the `AggregatorV3Interface` provides a streamlined ABI for interacting with the Data Feed contract.

Back to our code, first we need to declare a new variable, `priceFeed`, of type `AggregatorV3Interface` and pass it the correct contract address:

`AggregatorV3Interface priceFeed = AggregatorV3Interface(0x1b44F3514812d835EB1BDB0acB33d3fA3351Ee43);`

Then, we will use the `latestRoundData()` function, first let's take a look at its signature:

`function latestRoundData() external view returns (uint80 roundId, int256 answer, uint256 startedAt, uint256 updatedAt, uint80 answeredInRound);`

For now, we will focus on the `answer` value in our code:

```solidity
function getLatestPrice() public view returns (int) {
    (,int price,,,) = priceFeed.latestRoundData();
    return price;
}
```

Our `getLatestPrice()` function now retrieves the latest ETH price in USD from the `latestRoundData()` function of the Data Feed contract.

### Decimals

- `msg.value` is a `uint256` value with 18 decimal places.
- `answer` is an `int256` value with 8 decimal places (USD-based pairs use 8 decimal places, while ETH-based pairs use 18 decimal places).

This means the `price` returned from our `latestRoundData` function isn't directly compatible with `msg.value`. To match the decimal places, we multiply `price` by 1e10:

```solidity
return price * 1e10;
```

### Typecasting

Typecasting, or type conversion, involves converting a value from one data type to another. In Solidity, not all data types can be converted due to differences in their underlying representations and the potential for data loss. However, certain conversions, such as from `int` to `uint`, are allowed.

```solidity
return uint(price) * 1e10;
```

We can finalize our `view` function as follows:

```solidity
function getLatestPrice() public view returns (uint256) {
    (,int answer,,,) = priceFeed.latestRoundData();
    return uint(answer) * 1e10;
}
```

This complete `getLatestPrice` function retrieves the latest price, adjusts the decimal places, and converts the value to an unsigned integer, making it compatible for its use inside other functions.

## Solidity math

So up to now, we have `getPrice()` and `getConversionRate()` functions.

The `getPrice` function returns the current value of Ethereum in USD as a `uint256`.
The `getConversionRate` function converts a specified amount of ETH to its USD equivalent.

In Solidity, only integer values are used, as the language does not support floating-point numbers.

```solidity
function getConversionRate(uint256 ethAmount) internal view returns (uint256) {
    uint256 ethPrice = getPrice();
    uint256 ethAmountInUsd = (ethPrice * ethAmount) / 1e18;
    return ethAmountInUsd;
}
```

> The line `uint256 ethAmountInUsd = (ethPrice * ethAmount)` results in a value with a precision of 1e18 \* 1e18 = 1e36. To bring the precision of `ethAmountInUsd` back to 1e18, we need to divide the result by 1e18.

**Always multiply before dividing to maintain precision and avoid truncation errors.**

### Example of `getConversionRate`

- `ethAmount` is set at 1 ETH, with 1e18 precision.
- `ethPrice` is set at 2000 USD, with 1e18 precision, resulting in 2000e18.
- `ethPrice * ethAmount` results in 2000e36.
- To scale down `ethAmountInUsd` to 1e18 precision, divide `ethPrice * ethAmount` by 1e18.

### Checking Minimum USD Value

We can verify if users send at least 5 USD to our contract:

```solidity
require(getConversionRate(msg.value) >= MINIMUM_USD, "You need to spend more ETH!");
```

Since `getConversionRate` returns a value with 18 decimal places, we need to multiply `5` by `1e18`, resulting in `5 * 1e18` (equivalent to `5 * 10**18`).

### Deployment to the Testnet

Let's deploy the `FundMe` contract to a testnet. After deployment, the `getPrice` function can be called to obtain the current value of Ethereum. It's also possible to send money to this contract, and an error will be triggered if the ETH amount is less than 5 USD.

## ❓ Questions and 💪 Exercises

Exercise 💪:

Question ❓:

## 🛠️ Links and Resources

- [GitHub Repo](https://github.com/Cyfrin/remix-fund-me-cu)
- [Ethereum Unit Converter](https://eth-converter.com/)
- [Ether Units](https://docs.soliditylang.org/en/latest/units-and-global-variables.html#ether-units)
- [Chainlink](https://chain.link/)
- [Chainlink Docs](https://docs.chain.link/)
