# Section 01. Review

## Tooling Prerequisites

- VSCode or [VSCodium](https://vscodium.com/): VSCode without the Microsoft telemetry.
- Foundry
- wsl

## Solidity Prerequisites

- Be familiar with compiling and deploying to local and testnet blockchains.
- Familiar with Foundry's working tree and basic commands.
- Familiarty with regular and fuzz testing
- Stateful Fuzzing/Invariant Tests (covered in next lesson)

## Fuzzing and invariant tests

Often, hacks result from scenarios you didn't anticipate or consider for testing. But what if you could write a test that checks for every possible scenario, not just one? This is known as **fuzz testing**.

An invariant is a property of the system that should always hold.

The Fundamental Principle: Testing Invariants

Each system, from a function to an entire program, has an integral property, often referred to as the _invariant_. This property must always hold true.

### Introducing Fuzz Tests and Invariant Tests

There are two popular methodologies when dealing with edge cases: using _fuzz tests/invariant tests_, or _symbolic execution_ (which we'll save for another day).

Foundry will automatically randomize data and use numerous examples to run through the test script. This test will be supplied random data from 0 to uint256.max(), as many times as you've conifigured runs.

> Reminder: You can configure the number of runs in your foundry.toml under the \[fuzz] variable

Stateless Fuzzing versus Stateful Fuzzing

`Stateful fuzzing`, instead of resetting the contract state for each new run, will use the ending state of your previous run as the starting state of your next.

## Installing Libraries

To install our OpenZeppelin libraries:

`forge install OpenZeppelin/openzeppelin-contracts --no-commit`

Add to our `foundry.toml`:

`remappings = ['@openzeppelin/contracts=lib/openzeppelin-contracts/contracts']`

## Advanced Solidity Prerequisites

The Core of Smart Contracts: Storage

When you create a state variable within your contract, an individual storage slot is assigned.

Constants or immutable variables do not occupy space in storage. This unique trait is due to their nature of being stored directly within the contract's bytecode.

`forge inspect Counter storage`

## 🛠️ Links and Resources

- [Peera](https://app.peera.ai/)
- [Ethereum Stack Exchange](https://ethereum.stackexchange.com/)
