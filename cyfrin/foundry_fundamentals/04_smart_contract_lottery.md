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

Let's make our fee immutable and add a getter function to get the fee to enter.
Our contract should look like this:

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

Custom errors were introduced in Solidity v0.8.4 to provide a more gas-efficient way to explain to users why an operation failed.

- [More information on Custom Errors](https://soliditylang.org/blog/2021/04/21/custom-errors/)

Our custom error would look like this:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.26;

/**
 * @title A sample Raffle contract
 * @author Garo Sanchez
 * @notice This contract is for creating a sample raffle
 * @dev Implements Chainlink VRFv2.5
 */

contract Raffle {
    /*Errors */
    error Raffle__NotEnoughEthToEnterRaffle();
    uint256 private immutable i_entranceFee;

    constructor(uint256 entranceFee) {
        i_entranceFee = entranceFee;
    }

    function enterRaffle() public payable {
        // Old way using a require statement:
        // require(msg.value >= i_entranceFee, "Not enough ETH sent!");
        // New way using custom errors:
        if (msg.value < i_entranceFee) {
            revert Raffle__NotEnoughEthToEnterRaffle();
        }
    }

    function pickWinner() public {}

    /** Getter Functions */
    function getEntranceFee() external view returns (uint256) {
        return i_entranceFee;
    }
}
```

2 things:

1. Using an if statement combined with a custom error is the most efficient way to do this. We could use a custom error inside a require statement (supported as of 0.8.26) but it is not as gas efficient and has several caveats.
2. In the docs, the custom error is outside the contract. Patricks says we will put the custom error definition inside the contract to avoid testing headaches, why?

Notice it is a best practice to prefix your custom error with the name of the contract: `Raffle__NotEnoughEthToEnterRaffle()`

## Smart Contract Events

Now we need a storage structure to keep track of all registered users in the lottery.

We will use a dynamic array that is payable:

`address payable[] private s_players;`

We've made it `address payable` because one of the participants registered in this array will be paid the ETH prize, hence the need for the `payable` attribute.

Now in the `enterRaffle()` function, we need to push the address of the entered user into the array:

```solidity
function enterRaffle() public payable {
    // Old way using a require statement:
    // require(msg.value >= i_entranceFee, "Not enough ETH sent!");
    // New way using custom errors:
    if (msg.value < i_entranceFee) {
        revert Raffle__NotEnoughEthToEnterRaffle();
    }
    s_players.push(payable(msg.sender));
}
```

It would be good to create a little notification that a user has entered the raffle. Enter events.

### Events

Events are a way for smart contracts to communicate with the outside world, primarily with the front-end applications that interact with these contracts. Events are logs that the Ethereum Virtual Machine (EVM) stores in a special data structure known as the blockchain log, and are basically a very efficient way to track and retrieve information quickly.

- [Solidity Docs- Events](https://docs.soliditylang.org/en/v0.8.25/contracts.html#events)

An event has parameters and sometimes they have the keyword `indexed`. Indexed parameters, also called `topics`, are much more easy to search than non-indexed ones.

Finally, for an event to be logged we need to emit it.

This is the code to define an event: `event EnteredRaffle(address indexed player);`

And the code to emit it:

```solidity
function enterRaffle() external payable {
    if(msg.value < i_entranceFee) revert Raffle__NotEnoughEthSent();
    s_players.push(payable(msg.sender));
    emit EnteredRaffle(msg.sender);

}
```

This is how our code should be looking so far:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.26;

/**
 * @title A sample Raffle contract
 * @author Garo Sanchez
 * @notice This contract is for creating a sample raffle
 * @dev Implements Chainlink VRFv2.5
 */

contract Raffle {
    /* Errors */
    error Raffle__NotEnoughEthToEnterRaffle();

    uint256 private immutable i_entranceFee;
    address payable[] private s_players;

    /* Events */
    event EnteredRaffle(address indexed player);

    constructor(uint256 entranceFee) {
        i_entranceFee = entranceFee;
    }

    function enterRaffle() public payable {
        // Old way using a require statement:
        // require(msg.value >= i_entranceFee, "Not enough ETH sent!");
        // New way using custom errors:
        if (msg.value < i_entranceFee) {
            revert Raffle__NotEnoughEthToEnterRaffle();
        }
        s_players.push(payable(msg.sender));
        emit EnteredRaffle(msg.sender);
    }

    function pickWinner() public {}

    /** Getter Functions */
    function getEntranceFee() external view returns (uint256) {
        return i_entranceFee;
    }
}
```

## Random Numbers - Block Timestamp

First, let's change our functions that were `public` to `external` to make them more gas efficient.

Now we want to define an interval for our lottery, we want to set a specific time that we want our lottery to last (like PancakeSwap's lottery that runs every 5 minutes).

So let's create this immutable variable:

`uint256 private immutable i_interval;`

And also add it to the constructor. You know how to do this.

Now we would use `block.timestamp` to revert the function `pickWinner()` if not enough time has passed. This is how our code should look like so far:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.26;

/**
 * @title A sample Raffle contract
 * @author Garo Sanchez
 * @notice This contract is for creating a sample raffle
 * @dev Implements Chainlink VRFv2.5
 */

contract Raffle {
    /* Errors */
    error Raffle__NotEnoughEthToEnterRaffle();

    uint256 private immutable i_entranceFee;
    uint256 private immutable i_interval;
    address payable[] private s_players;
    uint256 private s_lastTimestamp;

    /* Events */
    event EnteredRaffle(address indexed player);

    constructor(uint256 entranceFee, uint256 interval) {
        i_entranceFee = entranceFee;
        i_interval = interval;
        s_lastTimestamp = block.timestamp;
    }

    function enterRaffle() external payable {
        // Old way using a require statement:
        // require(msg.value >= i_entranceFee, "Not enough ETH sent!");
        // New way using custom errors:
        if (msg.value < i_entranceFee) {
            revert Raffle__NotEnoughEthToEnterRaffle();
        }
        s_players.push(payable(msg.sender));
        emit EnteredRaffle(msg.sender);
    }

    function pickWinner() external {
        // See if enough time has passed
        if (block.timestamp - s_lastTimestamp < i_interval) {
            revert();
        }
    }

    /** Getter Functions */
    function getEntranceFee() external view returns (uint256) {
        return i_entranceFee;
    }
}
```

## Random Numbers - VRF

https://updraft.cyfrin.io/courses/foundry/smart-contract-lottery/solidity-random-number-chainlink-vrf?lesson_format=transcript
