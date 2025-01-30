# Development with Hardhat

- Hardhat Setup and Overview
- Testing with Typescript
- Etherscan
- Deploying Smart Contracts
- Verifying Smart Contracts
- Mainnet Forking

## Hardhat Setup and Overview

yarn add --dev hardhat
npx hardhat

`> Create a typescript project`
then ok everything

- [Intro to basics of Hardhat here](https://docs.base.org/base-learn/docs/hardhat-setup-overview/hardhat-setup-overview-sbs)

## Testing with Typescript

Here we will learn

- Use TypeChain to make writing tests easier
- Write unit tests using Mocha, Chai and Hardhat Toolkit
- Set up multiple signers and call smart contract functions with different signers

Setup Typechain

you created a new project using the init command that by default installs @nomicfoundation/hardhat-toolbox. This package already contains Typechain, which is a plugin that generates static types for your smart contracts.

After compiling, a new folder called typechain-types was created

## Etherscan

Objectives:

- List some of the features of Etherscan
- Read data from the Bored Ape Yacht Club contract on Etherscan
- Write data to a contract using Etherscan.

One of the things you can do with Etherscan is interact with already-deployed contracts.

This is the BAYC contract on Etherscan: https://etherscan.io/token/0xbc4ca0eda7647a8ab7c2061c2e118a18a936f13d

You are able to see information such as:

- The ETH balance it holds
- The contract creator
- The transaction when it was created
- Latest transactions
- And the verified contract

In the Contract tab, you can see the full source code of BAYC.

Verifying contracts is important since it gives transparency to your users. Bear in mind this means bad actors will be able to see the source code of your contract and will try to exploit it.

## Deploying Smart Contracts

[Link](https://docs.base.org/base-learn/docs/hardhat-deploy/hardhat-deploy-sbs):

- Setting up Hardhat deploy
- Testing your deployment
- Deploying to a test network

## Verifying Smart Contracts

- Verify a deployed smart contract on Etherscan
- Connect a wallet to a contract in Etherscan
- Use etherscan to interact with your own deployed contract

## Mainnet Forking

Objectives:

- Use Hardhat Network to create a local fork of mainnet and deploy a contract to it
- Utilize Hardhat forking features to configure the fork for several use cases

- Overview
- Creating the Balance Reader contract
- Configuring Hardhat to support forking
- Creating a test for the BalanceReader.sol contract
