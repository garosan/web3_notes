# State Variables

Up until now, our functions have just returned values that purely depend on the arguments. They don't depend on anything else than the immediate input, that's why they're called _pure_ functions. They are not aware of the blockchain state or anything that has happened in the past.

If we wanted to keep track of how much money is owed to someone or the points of someone in a game, we would fall short.

Now we introduce the **state variable**.

Let's look at an example and then analyze it:

```solidity
contract StorageVariables01 {
    uint256 internal x;

    function setX(uint256 newValue) public {
        x = newValue;
    }

    function getX() public view returns (uint256) {
        return x;
    }
}
```

**Variables declared outside of functions are state variables. They keep their value after the transaction ends.**

Note that `getX()` has the modifier `view` instead of `pure`. That's because it views the blockchain state, I.e. what is stored in the variable `x`. If you change `view` to `pure` in this example, the code will not compile. You can also think of `view` as read-only.

Second, note that `setX()` does not have a `view` or a `pure` modifier. That's because it is a state changing function. Functions that change storage variables, or make some other lasting change to the blockchain cannot have the `view` or `pure` modifier, this is because they are not read only and thus cannot be labelled as `view`, and certainly not `pure`. (This function is implicitly by default `non-payable`).

Note that the variable `x` itself has the modifier `internal`. This means other smart contracts cannot see the value.

**Just because a variable is internal does not mean it is hidden. It's still stored on the blockchain and anyone can parse the blockchain to get the value!**

The code will still compile if you don't set it to `internal` but it's considered bad practice because you aren't being explicit about your intentions for the visibility of `x`.

You could also use `public` instead of `internal`. When a variable is declared `public`, it means other smart contracts can read the value but not modify it.

Sounds confusing but remember: **public functions can modify variables, but public variables cannot be modified unless there is a function to change their value.**

Most important to remember:

- Storage variables are declared outside of functions
- Public functions that do not have a view or pure modifier can change storage variables
- Pure functions cannot access storage variables

## More notes about state variables, visibility and mutability

### Visibility

Let's talk a little bit about visibility, functions and state variables have to declare whether they are accessible by other contracts.

State variables can be declared as:

- `public`: Public state variables differ from internal ones only in that the compiler automatically generates getter functions for them, which allows other contracts to read their values.
- `internal`: Internal state variables can only be accessed from within the contract they are defined in and in derived contracts.
- `private`: Private state variables are like internal ones but they are not visible in derived contracts.

Functions can be declared as:

- `public` - Any contract and account can call. These functions are accessible both internally (within the contract and derived contracts) and externally (by other contracts or accounts).
- `internal`- can only be accessed from within the contract they are defined in and in derived contracts
- `private` - like internal ones but they are not visible in derived contracts.
- `external` - Functions declared as external can only be called from outside the contract, either by other contracts or by external accounts.

You can see for example that if we mark functions as internal or private, the buttons to interact with them in Remix will dissappear when we redeploy, try it:

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.7.0 <0.9.0;

contract HelloWorld {
    string private text;

    constructor() {
        text = "Hello World";
    }

    function helloWorld() internal view returns (string memory)  {
        return text;
    }

    function setText(string memory newText) private {
        text = newText;
    }
}
```

If you declare a function as external, you can't call it within your own contract. This would produce an error:

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.7.0 <0.9.0;

contract HelloWorld {
    string private text;

    constructor() {
        setText("Hello from constructor!");
    }

    function helloWorld() public view returns (string memory)  {
        return text;
    }

    function setText(string memory newText) external {
        text = newText;
    }
}
```

The difference between internal and private is that private state variables or functions can't be accessed from contracts that inherit from the original contract where the variable or function was declared.

### State mutability

Now let's talk about state mutability. **State mutability refers to the ability of a function to modify the state of the blockchain.**

A function can alter the contract's state, read it, or neither.

Types of State Mutability

- `pure` - these do not read or modify the blockchain state. They consume the least amount of gas when called externally, as they don't interact with the blockchain state.
- `view` - can read data but can't modify the blockchain state. Calling them directly is gasless.
- `payable` -
- `non payable` - default mutability. They can read and modify state variables, allowing for full interaction with the contract, and thus pay gas.

[Solidity Docs - State variables](https://docs.soliditylang.org/en/latest/structure-of-a-contract.html#state-variables)

Also, read [the docs here](https://docs.soliditylang.org/en/v0.8.28/introduction-to-smart-contracts.html#locations) to learn more about data locations.

[Original Article](https://www.rareskills.io/learn-solidity/storage-variables)
