# Development Environment

In this first lesson we just learn about [Remix](remix.ethereum.org)

We create a 'Hello World' contract by creating a `HelloWorld.sol` file:

```solidity
contract HelloWorld {
  function helloWorld() public pure returns (uint256) {
    return 100;
  }

  function returnsBool() public pure returns (bool) {
    return true;
  }
}
```

Then we learn to:

- Compile the contract in Remix
- Deploy the contract in Remix
