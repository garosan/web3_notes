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

## ❓ Questions and 💪 Exercises

Exercise 💪:

Question ❓:

## 🛠️ Links and Resources

- [GitHub Repo](https://github.com/Cyfrin/remix-fund-me-cu)
- [Ethereum Unit Converter](https://eth-converter.com/)
- [Ether Units](https://docs.soliditylang.org/en/latest/units-and-global-variables.html#ether-units)
