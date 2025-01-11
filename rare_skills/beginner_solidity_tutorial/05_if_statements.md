# If Statements

If statements behave exactly the same as other languages. But, unlike in dynamic languages such as Python or JavaScript, you cannot do the following:

```solidity
function isNotZero(uint256 x)
    public
    pure
    returns (bool) {
        if (x) {
            return true;
        } else {
            return false;
        }
}
```

Solidity also supports the _else if_ construction.

Solidity does not have a switch statement.

An exercise:

```solidity
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.13;

contract IfStatement {
    function max(uint256 a, uint256 b) public pure returns (uint256) {
        // return the maximum of a and b
    }

    function min(uint256 a, uint256 b) public pure returns (uint256) {
        // return the minimum of a and b
    }
}
```

Answer:

```solidity
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.13;

contract IfStatement {
    function max(uint256 a, uint256 b) public pure returns (uint256) {
        if (a > b) {
            return a;
        } else if (b > a) {
            return b;
        }
    }

    function min(uint256 a, uint256 b) public pure returns (uint256) {
        if (a > b) {
            return b;
        } else if (b > a) {
            return a;
        }
    }
}
```

Keep in mind though this will produce a warning and if both arguments are equal the functions will return `0`.

[Original Article](https://www.rareskills.io/learn-solidity/if-statements)
