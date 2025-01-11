# Constructor

In our previous ERC-20 token code example, we did something a little weird, we set the banker variable directly in the contract:

```solidity
contract ERC20 {
    address public banker = 0x5B38Da6a701c568545dCfcB03FcB875f56beddC4;

    mapping(address => uint256) public balances;
    // ... rest of code
}
```

What if someone wants to deploy the contract and set themselves to be the banker? How could they achieve this?

Smart contracts have a special function that is called at deployment time called the constructor:

```solidity
contract Constructor01 {

    address public banker;

    constructor() {
        deployer = msg.sender;
    }
}
```

Note that we don't specify `public` because **constructors can't be modified with things like pure, view, public**.

If you wanted the banker to be configured by the person deploying the contract, then you could use it as a function argument.

```solidity
contract Constructor02 {

    address public banker;

    constructor(address _banker) {
        banker = _banker;
    }
}
```

So the difference is, in our previous lesson the 'banker' was hardcoded. In our 1st example here, the banker is whoever deploys the contract. In our 2nd example, whoever deploys the contract can set who the banker will be in the arguments.

You'll see this pattern `variable = _variable` a lot in constructors. Solidity doesn't require you to do that, but it's a convention.

For reasons we cannot get into right now, `calldata` cannot be used in constructor arguments. I know, it seems like a very weird and random restriction, but it will make sense later, after you understand how Ethereum works under the hood.

Again, **if you want to pass a string or an array to the constructor function, you need to use `memory` or `storage`**.

```solidity
contract Constructor03 {
    string public name;

    // if you use calldata, it won't compile
    constructor(string memory _name) {
        name = _name;
    }
}
```

Also, in case you were wondering, constructors cannot return values.

As a recap:

- When you pass an argument to a constructor, if its a value type like `address`, `uint`, `bool`, etc. you do not need to specify a data location. They will be stored directly in storage, memory or the stack, depending on where they are used and all this is handled efficiently by the EVM.
- When you pass a reference type to the constructor, like `string`, `bytes`, `arrays`, `structs`, `mappings`, etc. you need to specify the data location, and you can only use `memory` (the most common choice for constructors) or `storage`.

[Original Article](https://www.rareskills.io/learn-solidity/constructor)
