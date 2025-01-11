# Calling other Contracts

Everything we've been doing up to this point is calling smart contracts directly. But it's also possible, and in fact, desirable, for smart contracts to be able to communicate with each other.

See example:

```solidity
contract CallingOtherContracts01 {
    function askTheMeaningOfLife(address source) public returns (uint256) {
        (bool ok, bytes memory result) = source.call(
            abi.encodeWithSignature("meaningOfLifeAndAllExistence()")
        );
        require(ok, "call failed");

        return abi.decode(result, (uint256));
    }
}

contract AnotherContract {
    function meaningOfLifeAndAllExistence() public pure returns (uint256) {
        return 42;
    }
}
```

The first interesting thing here is that `askTheMeaningOfLife()` is not a view function. If you try to add the `view` modifier it won't compile. Why? View functions are read only. When our function `askTheMeaningOfLife()` calls another smart contract, we (and the Solidity compiler) don't know if that other contract is going to try to make some state change on the blockchain, therefore, **solidity doesn't let you specify a function as view if it calls another smart contract**.

What is the `bool ok` portion of the tuple? Function calls to other smart contracts can fail, for example if the function reverts. To know if the external call reverted, a boolean is returned. In this implementation, the calling function, `askTheMeaningOfLifeAndAllExistence()`, reverts also, but that is not necessarily a general requirement.

## Calling a non-existing contract

What happens if you call a non-existent smart contract i.e. a wallet address?

Try calling `askTheMeaningOfLife()` with our wallet address from Remix:

`0x4B20993Bc481177ec7E8f571ceCaE8A9e22C02db`

It reverts because you tried to decode empty data. When you open the transaction dropdown in Remix, you see the revert explanation:

```shell
decoded output: {
	"error": "Failed to decode output: Error: data out-of-bounds (length=0, offset=32, code=BUFFER_OVERRUN, version=abi/5.7.0)"
}
```

If you comment out the code that is trying to decode, the function call will no longer revert:

```solidity
contract CallingOtherContracts01 {
    function askTheMeaningOfLife(address source) public returns (uint256) {
        (bool ok, bytes memory result) = source.call(
            abi.encodeWithSignature("meaningOfLifeAndAllExistence()")
        );
        require(ok, "call failed");

        // return abi.decode(result, (uint256));
        return 0;
    }
}

contract AnotherContract {
    function meaningOfLifeAndAllExistence() public pure returns (uint256) {
        return 42;
    }
}
```

## Calling another contract with arguments

What if the other contract takes arguments? Here is the syntax to call a contract with arguments:

```solidity
contract CallingOtherContracts02 {
    function callAdd(address source, uint256 x, uint256 y) public returns (uint256) {
        (bool ok, bytes memory result) = source.call(
            abi.encodeWithSignature("add(uint256,uint256)", x, y)
        );
        require(ok, "call failed");

        uint256 sum = abi.decode(result, (uint256));
        return sum;
    }
}

contract Calc {
    function add(uint256 x, uint256 y) public pure returns (uint256) {
        return x + y;
    }
}
```

[Original Article](https://www.rareskills.io/learn-solidity/cross-contract-call)
