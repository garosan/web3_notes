# Section 06. Upgradeable Smart Contracts

## Introduction

Upgradable smart contracts are a complex subject with potential drawbacks, which isn't the best route to default on. They sound great in theory, promising flexibility and adaptability, but a lot of problems may arise when there's too much centralized control over contracts.

Remember: The priority should always be minimizing the deployment of upgradeable smart contracts.

From hacks to lost funds, the risks are real.

Let's look at three different patterns or philosophies we can use:

1. Not really upgrading
2. Social migration
3. Proxy (with subcategories like metamorphic contracts, transparent upgradable proxies, and universal upgradable proxies)

### Not really upgrading

The idea here is parameterizing everything, having setter functions that can change certain parameters. For instance, if you have a set reward that distributes a token at a 1% rate every year, you can have a setter function to adjust that distribution rate. The obvious drawback is, unless you anticipated all possible future functionality when writing the contract, you won't be able to add it in the future.

Another question that arises is—who gets access to these functions? If a single person holds the key, it becomes a centralized smart contract, going against decentralization's core principle. To address this, you can add a governance contract to your protocol, allowing proportional control.

### Social Migration

Another method is social migration. It involves deploying a new contract and socially agreeing to consider the new contract as the 'real' one. For this one, the contract will function the same way, whether invoked now or in 50,000 years. But one major disadvantage is that you'd now have a new contract address for an already existing token. This would require every exchange listing your token to update to this new contract address.

Moving the state of the first contract to the second one is also a challenging task. You need to devise a migration method to transport the storage from one contract to the other. You can learn more about the social migration method from [this blog post](https://blog.trailofbits.com/2018/09/05/contract-upgrade-anti-patterns/) written by Trail of Bits.

### Proxies

Proxies are the holy grail of smart contract upgrades. They allow for state continuity and logical updates while maintaining the same contract address. Users may interact with contracts through proxies without ever realizing anything changed behind the scenes.

There are a ton of proxy methodologies, but three are worth discussing here:

- Transparent Proxies
- Universal Upgradable Proxies (UPS)
- The Diamond Pattern

Each has its benefits and drawbacks, but the focus is on maintaining contract functionality and decentralization.

## Using delegatecall

In this section we're going to be learning how to build our own proxies and in order to do this we first need an understanding of the delegateCall function.

## Overview of EIP-1967

In this lesson we'll apply what we've learnt and get our hands dirty with a small proxy example. The code for this exercise can be found **[here](https://github.com/Cyfrin/foundry-upgrades-f23/tree/main/src/sublesson)**.

## ❓ Questions and 💪 Exercises

## 🛠️ Links and Resources

- [Trail of Bits - Contract upgrade anti-patterns](https://blog.trailofbits.com/2018/09/05/contract-upgrade-anti-patterns/)
- [Yul Docs](https://docs.soliditylang.org/en/latest/yul.html)
