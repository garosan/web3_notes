# Section 4. Smart Contract Lottery

- [Github Repo](https://github.com/Cyfrin/foundry-smart-contract-lottery-cu)

We will learn:

- Events
- On-chain randomness
- Modulo
- Chainlink automation
- Using natspec

## Setup

```shell
mkdir foundry-smart-contract-lottery
cd foundry-smart-contract-lottery

forge init
```

Delete all the `Counter` files.

Let's start with adding the details of this project to the `README.md`:

```markdown
# Proveably Random Raffle Contracts

## About

This code is to create a proveably random smart contract lottery.

## What we want it to do?

1. Users should be able to enter the raffle by paying for a ticket. The ticket fees are going to be the prize the winner receives.
2. The lottery should automatically and programmatically draw a winner after a certain period.
3. Chainlink VRF should generate a provably random number.
4. Chainlink Automation should trigger the lottery draw regularly.
```

Let's now create a file named `Raffle.sol`:

```solidity
// SPDX-License-Identifier: MIT

pragma solidity 0.8.26;

contract Raffle {


}
```

Let's create some NatSpec for our contract, if you have the Nomic Foundation VSCode extension, you can trigger the code by doing `/** + tab`.

Let's now think for a moment, what does our Raffle need to work? We need 2 things:

- A function to enter the raffle by paying a ticket price
- A function to pick a winner out of the registered users

```solidity
contract Raffle{
    function enterRaffle() public {}

    function pickWinner() public {}

}
```

Let's make our fee immutable and add a getter function to get the fee to enter:

```solidity
// SPDX-License-Identifier: MIT

pragma solidity 0.8.26;

/**
 * @title A simple Raffle contract
 * @author Garo Sanchez
 * @notice This contract creates a simple raffle
 * @dev This contract implements Chainlink VRF v2.5 and Chainlink Automation
 */
contract Raffle {
    uint256 private immutable i_entranceFee;

    constructor(uint256 entranceFee) {
        i_entranceFee = entranceFee;
    }

    function enterRaffle() public payable {}

    function pickWinner() public {}

    /** Getter Function */

    function getEntranceFee() external view returns (uint256) {
        return i_entranceFee;
    }
}
```

## Solidity Style Guide

The Solidity Docs provide an [Order of Layout](https://docs.soliditylang.org/en/latest/style-guide.html#order-of-layout) that looks like this:

```
// Layout of the contract file:
// version
// imports
// errors
// interfaces, libraries, contract

// Inside Contract:
// Type declarations
// State variables
// Events
// Modifiers
// Functions

// Layout of Functions:
// constructor
// receive function (if exists)
// fallback function (if exists)
// external
// public
// internal
// private

// view & pure functions
```

## Creating custom errors

Previously we defined the `i_entranceFee` variable. This is the amount the user has to send to enter the raffle. How do we check this?

```solidity
function enterRaffle() external payable {
    require(msg.value >= i_entranceFee, "Not enough ETH sent");

}
```

First, we changed the visibility from `public` to `external`. `external` is more gas efficient, and we won't call the `enterRaffle` function internally.

We used a `require` statement to ensure that the `msg.value` is higher than `i_entranceFee`. If that is false, we will yield an error message `"Not enough ETH sent"`.

## Random Numbers - VRF

https://updraft.cyfrin.io/courses/foundry/smart-contract-lottery/solidity-random-number-chainlink-vrf?lesson_format=transcript
