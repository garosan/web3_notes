# Chapter 02. Ethereum Basics

## Ether Currency Units

One ether is 1 quintillion wei (1 \* 10^18 or 1,000,000,000,000,000,000).

## Wallet Control and Responsibility

- Do not improvise security. Use tried-and-tested standard approaches.
- The more important the account, the higher security measures should be taken.
- The highest security is gained from an air-gapped device, but this level is not required for every account.
- Never store your private key in plain form, especially digitally.
- Private keys can be stored in an encrypted form, as a digital "keystore" file.
- Do not store any passwords in digital documents, digital photos, screenshots, online drives, encrypted PDFs, etc. Use a password manager or pen and paper.
- When you create a new account, start by sending only a small test transaction to the new address. Once you receive the test transaction, try sending back again from that account.

## Externally Owned Accounts (EOAs) and Contracts

Accounts can be EOAs or contracts. A contract account has smart contract code, which a simple EOA can't have. EOAs have private keys.

Only EOAs can initiate transactions, but contracts can react to transactions by calling other contracts, building complex execution paths.

## A Simple Contract: A Test Ether Faucet

We will write a contract that controls a faucet.
A faucet is a relatively simple thing: it gives out ether to any address that asks, and can be refilled periodically.
You can implement a faucet as a wallet controlled by a human or a web server.
