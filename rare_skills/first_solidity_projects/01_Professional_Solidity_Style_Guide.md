# Solidity Coding Standards

- Note: There is an official [Solidity Style Guide](https://docs.soliditylang.org/en/latest/style-guide.html) but this is RareSkills' finding of common mistakes:

## First two lines

- Don't forget to include the SPDX-License-Identifier
- Fix the solidity pragma unless you're writing a library

## Imports

### Explicitly set the library version in the import statement

Don't do this:

```solidity
import "@openzepplin/contracts/token/ERC20/ERC20.sol";
```

Do this:

```solidity
import "@openzeppelin/contracts@4.9.3/token/ERC20/ERC20.sol";
```

### Use named imports instead of importing the entire namespace

Just do this bro:

```solidity
import {ERC20} from "@openzeppelin/contracts@4.9.3/token/ERC20/ERC20.sol";
```

### Delete unused imports

And research a smart contract security tool called Slither.

## Contract Level

### Apply contract-level natspec

Example:

```solidity
/// @title Liquidity token for Foo protocol
/// @author Foo Incorporated
/// @notice Notes for non-technical readers/
/// @dev Notes for people who understand Solidity
contract LiquidityToken {

}
```

You can read more about NatSpec [here](https://docs.soliditylang.org/en/latest/natspec-format.html)

### Lay out the contract structure per the style guide

Functions should be ordered as follows: receive and fallback functions (if applicable), external functions, public functions, internal functions, and private functions.

Within those groups, the payable functions go on top, then non-payable, then view, then pure.

You can check more details in the [style guide](https://docs.soliditylang.org/en/latest/style-guide.html#order-of-functions)

## Constants

### Don't use magic numbers

Self explanatory.

### If numbers are used to measure ether or time, use the solidity keywords

Don't do:

```solidity
uint256 secondsPerDay = 60 * 60 * 24;
```

Do:

```solidity
1 days
```

### Use underscores to make large numbers more readable

Do this:

```solidity
uint256 private constant SOME_BIG_NUMBER = 10_000_000_000;
```

## Functions

### Remove the virtual modifier from functions that will not be overridden

## Additional Tricks for organizing large codebases

- If your functions require a significant amount of parameters, use a struct to pass the information around.
- If you need a lot of imports, you can import all the files and types into one solidity file, then import that file (you would need to intentionally break the rule about named imports).
- Use libraries to group functions of the same category together and make files smaller.
- Organizing large codebases is an art. The best way to learn it is to study codebases of large established projects.

[RareSkills Original Article](https://www.rareskills.io/post/solidity-style-guide)
