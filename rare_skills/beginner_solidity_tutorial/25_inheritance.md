# Inheritance

Implementing an ERC20 contract from scratch each time would no doubt get tiring. Solidity behaves like an object oriented language and allows for inheritance. Here is a minimal example:

```solidity
contract Parent {
    function theMeaningOfLife() public pure returns (uint256) {
        return 42;
    }
}

contract Child is Parent {

}
```

We use the keyword `is` to denote `Child` is inheriting functionality from `Parent`.

Like in other OOP languages, functions can be overriden:

```solidity
contract Parent {
    function theMeaningOfLife() public pure virtual returns (uint256) {
        return 42;
    }
}

contract Child is Parent {
    function theMeaningOfLife() public pure override returns (uint256) {
        return 3;
    }
}
```

Notice we introduced these 2 keywords: `virtual` and `override`.

From the [documentation](https://docs.soliditylang.org/en/v0.8.28/cheatsheet.html):

- `virtual` is for functions and modifiers: Allows the function's or modifier's behavior to be changed in derived contracts.
- `override`: States that this function, modifier or public state variable changes the behavior of a function or modifier in a base contract.

Also, when a function overrides, **it must match exactly, both in name, arguments, and return type**.

## Multiple inheritance

You can do multiple inheritance:

```solidity
contract Child is Parent1, Parent2 {

}
```

If the two parents had a function with the same name, the child must override it or the behavior will be ambiguous. If you end up in this situation, you probably did something wrong in your software design.

## Private vs Internal

There are two ways to make a function not accessible from the outside world: giving them a `private` or `internal` modifier. The distinction is simple.

Private functions (and variables) cannot be _seen_ by the child contracts.

Internal functions and variables can.
