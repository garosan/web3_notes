# Lesson 01 - Introduction

Instructor: Matheus Pagani (@matheuspagani77)

Week 1: Solidity, Remix
Week 2: Scripts, Deployments w/Hardhat
Week 3: Building tokens, etc.
Week 4: Fullstack development (frontend and backend)
Week 5 & 6: Scalability, security, gas optimizations, DeFi

We can start thinking about blockchain from a usability point of view. As we can say that internet is a place for searching for cat pictures, what can blockchain be used for?

Our gateway to the blockchain is a wallet.

So if the internet is a place to search for cat pictures, blockchain is a place for making transactions with your wallet.

- [Example Transaction from Google Faucet to my wallet](https://sepolia.etherscan.io/tx/0x6102ee028a9c4f565bf20b3f59c93825d28b2a563471ab1d7196d8323ee281ad)

If you click on `State` you can see info about the sender and the receiver of the transaction, the block producer address and the _before_ and _after_ balance of each.

Our first Hello World smart contract:

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.7.0 <0.9.0;

contract HelloWorld {

    constructor() {}

    function helloWorld() public view returns (string memory) {}

}
```

Try to compile this contract in Remix and then you'll see that at the bottom you have access to 2 new things:

- The ABI
- The bytecode

This is just a curiosity, but you can check a map of the language grammar [here](https://docs.soliditylang.org/en/v0.8.28/layout-of-source-files.html).

## ❓ Questions and 💪 Exercises

## 🛠️ Links and Resources

- [Notes](https://github.com/Encode-Club-Solidity-Bootcamp/Lesson-01)
- [Video](https://youtu.be/DLRsQvjJ6rQ)

- [Metamask Support Hub](https://support.metamask.io/)
- [Etherscan Docs](https://info.etherscan.com/)
- [Solidity Docs](https://docs.soliditylang.org/en/latest/)
- [Remix IDE](https://remix.ethereum.org/)
- [Remix Website](https://remix-project.org/?lang=en)
- [Remix Docs](https://remix-ide.readthedocs.io/en/latest/)

- [The Journey from Secret Recovery Phrase (Mnemonic Phrase) to Address](https://medium.com/mycrypto/the-journey-from-mnemonic-phrase-to-address-6c5e86e11e14)
