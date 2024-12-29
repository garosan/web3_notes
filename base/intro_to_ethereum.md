# Introduction to Ethereum

- [Intro to Ethereum video](https://docs.base.org/base-learn/docs/introduction-to-ethereum/intro-to-ethereum-vid)

The decentralized world computer

Key use cases and applications of Ethereum:

1. Token systems (ERC-20, NFTs, etc.)
2. Financial Derivatives and Stablecoins
3. Identity and Reputation Systems (ENS, etc.)
4. Decentralized File Storage (Filecoin, IPFS, etc.)
5. Decentralized Autonomous Organizations

Other:

- On-chain decentralized marketplaces
- On-chain prediction markets
- Smart multisignature escrows
- Decentralized data feeds

- [Ethereum Applications video](https://docs.base.org/base-learn/docs/introduction-to-ethereum/ethereum-dev-overview-vid)

Difference between web1, web2 and web3.

## [Ethereum Applications](https://docs.base.org/base-learn/docs/introduction-to-ethereum/ethereum-applications)

### The Ethos and Goals of Ethereum

The ethos of Ethereum is fundamentally different from Bitcoin's. Bitcoin development is conservative. In Bitcoin, changes are slow and any unnecessary risk-taking is generally frowned upon.

Ethereum development, is focused on innovation and experimentation.

Ethereum's primary goal is to be an all-purpose blockchain that allows developers to create any type of decentralized application. To allow this, smart contracts are crucial.

### Applications on Ethereum

Underlying forces of the smart contracts that make them possible:

#### Scripting

In the Ethereum whitepaper, Vitalik pointed out several limitations to Bitcoin scripting:

- Lack of Turing-completeness.
- Lack of state. Bitcoin's UTXO model only allows for simple contracts and not the complex stateful contracts needed for dapps.

#### Decentralized Finance (DeFi)

DeFi applications are designed to provide traditional financial services, such as lending, borrowing, trading, and much more, in a transparent, open and accessible manner.

Example of how lending and borrowing would work in a DeFi lending platform, such as Aave or Compound.

Although it's beyond the scope of this article, it's worth mentioning that DeFi has been the target of numerous smart contract exploits involving hundreds of millions of dollars in value.

#### Non-Fungible Token (NFT)

Ethereum's ERC-721 standard was the first to introduce NFTs in 2017, and it has since become the most popular standard for creating and trading NFTs on Ethereum.

#### Decentralized Autonomous Organization (DAO)

In simple words, DAOs are software-enabled, community-led organizations.

They allow a community to pool resources toward a shared goal. Because DAOs aren't tied to a physical location, they are able to mobilize quickly and attract resources and capital from all over the world.

The rules of a DAO are established by community members through the use of smart contracts. When a new DAO is created, a contract is deployed to the network.

DAOs typically use a token-based system to govern voting and decision-making. Members of the DAO are issued tokens that represent their ownership and influence within the organization. These tokens can be used to vote on proposals and allocate resources.

#### Other Applications

- Identity Management: The most notable example is the Ethereum Name Service (ENS).
- Gaming: Axie Infinity and Decentraland are both popular examples. They make use of fungible and non-fungible tokens for a variety of purposes.
- Prediction markets: Augur and Gnosis are popular examples of this application. Also [PolyMarket](https://polymarket.com/)

There are many other use cases from supply chain, energy, and intellectual property management to decentralized storage and content management to governance and voting systems. The list goes on and on, and it's worth taking some time to explore these types of applications on your own.

### Evolution of the Web

- The Limitations of Web2

  - Privacy & Control
  - Censorship
  - Lack of transparency
  - Security vulnerabilities
  - Limited interoperability

- The Limitations of Web3
  - Speed: Reliance on decentralized networks and consensus mechanisms result in much slower processing times
  - Storage: Storing data onchain can be very expensive
  - Smart contract limitations: limited to a maximum size of 24 KB, Once a smart contract is deployed, it cannot be updated or changed.
  - All data is public

#### Web2 vs Web3 Development

In Web2, engineering is centered around a client-server architecture. There is a top-down corporate approach to development processes, and code is generally proprietary and closed-sourced.

The Web3 development paradigm is centered around a distributed architecture, where developers build applications that run on decentralized protocols and smart contracts. There is an emphasis on open-source code and open standards.

Developers need to have a strong understanding of blockchain technology, cryptography, and distributed systems, and they also need to be proficient in programming languages like Solidity.

There is also a different approach to testing and deployment. Because onchain apps run on distributed systems, developers need to consider factors like network latency and the possibility of network partitions. They also need to ensure that their applications are extremely secure and resistant to a variety of attacks.

Developers also have to consider concepts like immutability because once code is deployed to a blockchain, it cannot be edited.

Developers also need the ability to think creatively about how to build applications that are constrained by the technical limitations of the Web3 paradigm, such as speed, storage, and scalability.

## [Gas Use in Ethereum Transactions](https://docs.base.org/base-learn/docs/introduction-to-ethereum/gas-use-in-eth-transactions)

### What is gas?

Unlike Bitcoin, where transaction fees only consider the size of a transaction, Ethereum accounts for every computational step performed.

The amount of gas required for an operation depends on its complexity.

#### State of the Network

Gas costs can also vary depending on the congestion of the network. Basically, higher gas prices are more likely to get processed first.

When the network is congested, gas prices increase to encourage more efficient use of the network's resources and decrease when network usage is lower.

### Why is gas necessary?

- Turing Completeness
- Preventing Infinite Loops
- Autonomous Execution

### How does gas work?

- Gas fees are paid in ETH.
- Wei is the smallest denomination of Ethereum and is equivalent to 10^-18 ETH.
- 1 ETH = 1,000,000,000,000,000,000 wei
- Gwei is commonly used to express the price of gas. One Gwei is equivalent to 10^-9 ETH.
- 1 ETH = 1,000,000,000 gwei

#### Gas Price

Gas fee is calculated as the product of the gas price and the amount of gas required for an operation.

For example, if the gas price is 50 gwei, and an operation requires 100,000 units of gas, the gas fee would be 0.005 ETH (50 gwei x 100,000 gas = 0.005 ETH).

#### Gas Limit

It defines the maximum amount of gas a user is willing to spend for a transaction to be processed.

The EVM, upon receiving a transaction, starts deducting the used gas from the gas limit.

If the transaction completes before reaching the gas limit, the remaining unused gas is refunded to the user's account.

Let's now consider a scenario where Alice wants to execute a transaction and she set the gas limit at 100,000 units. The transaction has consumed 99,998 units of gas and there is only one opcode left to execute. The EVM initiates the execution **because there is still some gas left to start the execution** but in the middle of it, gas runs out. At this point, the EVM throws an "Out of Gas" exception and halts the transaction.

In this scenario, Alice will lose all 100,000 units of gas. All state changes are rolled back and the transaction is not completed.

#### Gas Estimation

If the gas limit is set too low, the transaction may fail to execute, while if it is set too high, the sender may end up paying more in transaction fees than is necessary.

Thankfully, most Ethereum wallet applications have built-in gas estimation algorithms that can automatically calculate an appropriate gas limit for a transaction based on the network conditions. This helps to prevent a transaction from failing from the gas limit being too low while optimizing for the best possible cost for the sender.

## [EVM Diagram](https://docs.base.org/base-learn/docs/introduction-to-ethereum/evm-diagram)

### What is the EVM?

The Ethereum Virtual Machine (EVM) is the core engine of Ethereum. It is a Turing-complete, sandboxed virtual machine designed to execute smart contracts.

The EVM employs a resource management system using gas to regulate computation and prevent network abuse.

### EVM Components

- **World State**: Represents the entire Ethereum network, including all accounts and their associated storage.
- **Accounts**: Entities that interact with the Ethereum network, including Externally Owned Accounts (EOAs) and Contract Accounts.
- **Storage**: A key-value store associated with each contract account, containing the contract's state and data.
- **Gas**: A mechanism for measuring the cost of executing operations in the EVM, which protects the network from spam and abuse.
- **Opcodes**: Low-level instructions that the EVM executes during smart contract processing.
- **Execution Stack**: A last-in, first-out (LIFO) data structure for temporarily storing values during opcode execution.
- **Memory**: A runtime memory used by smart contracts during execution.
- **Program Counter**: A register that keeps track of the position of the next opcode to be executed.
- **Logs**: Events emitted by smart contracts during execution, which can be used by external systems for monitoring or reacting to specific events.

### EVM Execution Model

In simple terms, when a transaction is submitted to the network, the EVM first verifies its validity. If the transaction is deemed valid, the EVM establishes an execution context that incorporates the current state of the network and processes the smart contract's bytecode using opcodes. As the EVM runs the smart contract, it modifies the blockchain's world state and consumes gas accordingly. However, if the transaction is found to be invalid, it will be dismissed by the network without further processing. Throughout the smart contract's execution, logs are generated that provide insights into the contract's performance and any emitted events. These logs can be utilized by external systems for monitoring purposes or to respond to specific events.

### Gas and Opcode Execution

Every opcode in a smart contract carries a specific gas cost, which reflects the computational resources necessary for its execution.

Opcodes are the low-level instructions executed by the EVM. They represent elementary operations that allow the EVM to process and manage smart contracts.

During execution, the EVM reads opcodes from the smart contract, and depending on the opcode, it may update the world state, consume gas, or revert the state if an error occurs. Some common opcodes include:

- ADD: Adds two values from the stack.
- SUB: Subtracts two values from the stack.
- MSTORE: Stores a value in memory.
- SSTORE: Stores a value in contract storage.
- CALL: Calls another contract or sends ether.

### Stack and Memory

Stack and memory deal with temporary data during opcode execution.

The stack is a last-in, first-out (LIFO) data structure that is used for temporarily storing values during opcode execution. It supports two primary operations: push and pop.

During contract execution, memory serves as a collection of bytes, organized in an array, for the purpose of temporarily storing data.
It can be read from and written to by opcodes.

### EVM Architecture and Execution Context

![Txn Img](https://docs.base.org/assets/images/evm-architecture-execution-2d3506a463bdd57cc7efc3e6cac09b39.png)

Read more:

- https://ethereum.org/en/developers/docs/evm/
- https://cypherpunks-core.github.io/ethereumbook/13evm.html#evm_architecture

## Guide to Base

Finally read this:
https://www.coinbase.com/developer-platform/discover/protocol-guides/guide-to-base
