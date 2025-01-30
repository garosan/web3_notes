# Lesson 18 - Gas limit and loops

The problem: Estimates show that there could be around 100,000 smart contracts out there that could be vulnerable to DoS attacks due to how their looping patterns are implemented.

Consumer-grade computers can manage terabytes of computation per seconds or minutes. In other words, if you have a process that requires looping over a huge amount of data most probably your M3 Mac or your Lenovo won't have any issues to handle it. Smart contracts and the EVM, not so much.

Read more:

- [How to Avoid Unbounded “For” Loops in Solidity Smart Contracts](https://blog.b9lab.com/getting-loopy-with-solidity-1d51794622ad)

There is [this chart](https://etherscan.io/chart/blocksize) in Etherscan that indicates the historical average block size in bytes of the Ethereum blockchain.

Remember: Ethereum's blocks don't have a fixed size but are constrained by gas limits (currently ~30 million gas per block).

[Here](https://etherscan.io/blocks) you can see each block that is being produced and the amount of gas units it took.

## ❓ Questions and 💪 Exercises

## 🛠️ Links and Resources

- [Notes](https://github.com/Encode-Club-Solidity-Bootcamp/Lesson-18)
- [Video](https://www.youtube.com/watch?v=kTaQeZtXJj4)
