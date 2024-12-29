# [Smart Contract Development](https://docs.base.org/base-learn/docs/introduction-to-solidity/introduction-to-solidity-overview)

Solidity was developed by the Ethereum Project's Solidity team and was first previewed in 2014 at DevCon0.

- [History overview](https://soliditylang.org/blog/2020/07/08/solidity-turns-5/) of Solidity.

The EVM is a much smaller, slower, and less-powerful computer than a desktop, or even a mobile device.

When you were learning about time complexity, you probably heard the term space complexity once, and then it was never mentioned again. This is because normally, computation is expensive, and storage is practically free. The opposite is true on the EVM. It costs a minimum of 20,000 gas to initialize a variable, and a minimum of 5,000 to change it.

Meanwhile, the cost to add two numbers together is 3 gas. This means it is often much cheaper to repeatedly derive a value that is calculated from other values than it is to calculate it once and save it.

You also have to be careful to write code with predictable execution paths. Each transaction is sent with a gas limit and which various frameworks, such as ethers.js, in order to do their best to estimate. If this estimate is wrong, the transaction will fail, but it will still cost the gas used up until the point it failed!

EIP-170 introduced a compiled byte-code size limit of 24kb to smart contracts.

There are solutions to get around this like modularizing code, abstracting reusable code into libraries or even more advanced solutions like [EIP-2535](https://eips.ethereum.org/EIPS/eip-2535).

### Stack limit

Programs written for computers or mobile devices often work with hundreds of variables at the same time. The EVM operates with a stack that can hold 1,024 values, but it can only access the top 16.

In Solidity/EVM, your functions are limited to a total of 16 variables that are input, output, or initialized by the function.

## Basic Types

Types can be cast from one type to another, but not as freely as you may expect. For example, to convert a uint256 into a int8, you need to cast twice:

```solidity
uint256 first = 1;
int8 second = int8(int256(first));
```
