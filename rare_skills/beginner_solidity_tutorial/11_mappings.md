# Mappings

Mapping, Hashmap, maps, whatever you want to call it, Solidity has it.

Example:

```solidity
contract Mappings01 {
    mapping(uint256 => uint256) public myMapping;

    function setMapping(uint256 key, uint256 value) public {
        myMapping[key] = value;
    }

    function getValue(uint256 key) public view returns (uint256) {
        return myMapping[key];
    }
}
```

**If you access a mapping with a key that has not been set, you will NOT get a revert. The mapping will just return the “zero value” of the datatype for the value.**

In the following example, the mapping will return false if you supply a number that hasn't been set.

```solidity
contract Mappings02 {
    // returns false by default
    mapping(uint256 => bool) public mapToBool;

    // returns 0 by default
    mapping(uint256 => uint256) public mapToUint;

    // returns 0x0000000000000000000000000000000000000000 by default
    mapping(uint256 => address) public mapToAddress;

    function getMapToBoolValue(uint256 key) public view returns (bool) {
        return mapToBool[key];
    }

    function getMapToUintValue(uint256 key) public view returns (uint256) {
        return mapToUint[key];
    }

    function getMapToAddressValue(uint256 key) public view returns (address) {
        return mapToAddress[key];
    }
}
```

I forgot to add the setter functions in the code above lol.

By the way, ERC20 tokens use mappings to store how many tokens someone has! They map an address to how many tokens someone owns.

```solidity
contract ERC20Token {

    mapping(address => uint256) public balances;

    function setSomeonesBalance(address owner, uint256 amount)
        public {
            balances[owner] = amount;
    }

    function transferTokensBetweenAddresses(
            address sender,
            address receiver,
            uint256 amount)
        public {
            balances[sender] -= amount;   // deduct/debit the sender's balance
            balances[receiver] += amount; // credit the reciever's balance
    }
}
```

This implementation has a flaw that anyone can invoke the public functions and send tokens between addresses willy-nilly, but don't mind that for now.

**ERC20 tokens are not stored in cryptocurrency wallets, they are simply a uint256 associated with your address in a smart contract. “ERC20 tokens” are simply a smart contract.**

[This](https://etherscan.io/token/0x4d224452801aced8b2f0aebe155379bb5d594381) is the token for ApeCoin, the currency of the Bored Ape Yacht Club ecosystem.

If we click on 'Contract' and then check File 5 of 5 : ERC20.sol and see this line:

```solidity
mapping(address => uint256) private _balances;
```

This is saying we will have a mapping called balances that will look like this (this is how it would look in JavaScript):

```javascript
{
    '0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045': 100
}
```

The above line is saying Vitalik's wallet has 100 tokens of whatever ERC-20 token that is.

### Surprise 1: Mappings can only be declared as storage, you cannot declare them inside a function

This may seem like a very odd restriction, but this has to do with how the EVM works. Blockchains in general don't like hashmaps because of their unpredictable runtime.

Example code that won't compile:

```solidity
contract BrokenContract {

    function wontWork()
        public
        view {
            mapping(uint256 => uint256) someMap;
            // This won't compile, mappings must be state variables
    }
}
```

### Surprise 2: Mappings cannot be iterated over

There is no way to iterate over the keys of a mapping. Every key is technically valid, it just defaults to zero.

### Surprise 3: Mappings cannot be returned

The following code is invalid. Maps are not a valid return type for solidity functions.

```solidity
contract BrokenContract {
    mapping(uint256 => uint256) public someMap;

    function wontWork()
        public
        view
        returns (mapping(uint256 => uint256)) {
            return someMap; // This will not compile, as mappings cannot be returned from public functions
    }
}
```

So just remember:

- Mappings can only be declared as storage variables.
- You can't declare mappings as a function argument, inside a function or as the return type of a function.

[Original Article](https://www.rareskills.io/learn-solidity/mapping)
