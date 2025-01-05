# Course Introduction and Review

Taught by:

- [Tincho](https://x.com/tinchoabbate)
  - [notonlyowner.com](https://www.notonlyowner.com/)
  - [DamnVulnerableDeFi](https://www.damnvulnerabledefi.xyz/)
  - [theredguild.org](https://theredguild.org/)
- [Pashov](https://x.com/pashovkrum)
  - [pashov.net](https://www.pashov.net/)
  - [Github](https://github.com/pashov)
- [Juliette](https://x.com/_juliettech)
- Johnny Time from Gingersec
- Andy Li
  - [Youtube](https://www.youtube.com/@andyli)
- [Josselin Feist](https://x.com/Montyly) from Trail of Bits
- Alex Roan
- Owen from Guardian Audits
  - [X](https://x.com/0xOwenThurm)
  - [Youtube](https://www.youtube.com/@0xOwenThurm)
- Hans Friese, one of the top competitive auditors

Topics covered:

- Reentrancy
- MEV
- Reward Manipulation
- Failure to initialize
- Invariant Breaks
- Mishandling of ETH
- Missing Access Controls
- Oracle Manipulation
- Signature Replays
- Storage Collisions
- Weak Randomness

## Prerequisites

- [Github Repo](https://github.com/Cyfrin/security-and-auditing-full-course-s23)

## Current State of Web3 Security

- [Chainalysis Blog](https://www.chainalysis.com/blog/)
- [Chainalysis Podcast](https://www.chainalysis.com/blog/category/podcast/)
- [Blockchain Threat Intelligence by Peter Kacherginsky](https://x.com/blockthreat)

## Exercises

- [DeFi Security Summit](https://defisecuritysummit.org/)
- [DeFi Security Summit Channel](https://www.youtube.com/@defisecuritysummit2088)
- [DeFi Security Summit 2023 - Session 1: DeFi Protocols 1 - Peter Kacherginsky](https://www.youtube.com/watch?v=jSpvDhuaCgc)
- [The State of DeFi Security - 2024 | Peter Kacherginsky](https://www.youtube.com/watch?v=rEWu0jZWLMo)

🎯 Exercise: `Write yourself a message about why you want this`

This will be important for when things get hard
Is it money? Save web3? Become someone?

## Review - Tooling Prerequisites

- VSCode or [VSCodium](https://vscodium.com/): VSCode without the Microsoft telemetry.
- Foundry
- wsl

## Review - Solidity Prerequisites

- Be familiar with compiling and deploying to local and testnet blockchains.
- Familiar with Foundry's working tree and basic commands.
- Familiarty with regular and fuzz testing
- Stateful Fuzzing/Invariant Tests (covered in next lesson)

## Review - Fuzzing and invariant tests

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
