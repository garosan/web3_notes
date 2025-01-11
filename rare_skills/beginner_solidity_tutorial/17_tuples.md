# Tuples

We will introduce the tuple data type, because it is a prerequisite for upcoming sections.

In Solidity, tuples are used to group multiple values together. They allow you to pack variables of different types into a single entity, which can be especially useful when returning multiple values from a function or when assigning values to multiple variables at once.

Here is an example of a tuple declaration:

`(uint256 x, uint256 y) = (10, 20);`

This statement assigns `10` to `x` and `20` to `y` using a tuple assignment.

And here's an example of a function that returns a tuple:

```solidity
contract Tuples01 {
    function getTopLeaderboardScore() public pure returns (address, uint256) {
        return (0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045, 100);
    }
}
```

**Note that tuples are implied. The keyword 'tuple' never appears in Solidity.**

Tuples can also be 'unpacked' to get the variables inside, example:

```solidity
contract Tuples02 {
    function getTopLeaderboardScore() public pure returns (address, uint256) {
        return (0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045, 10000);
    }

    function highestScoreIsOver9000() public pure returns (bool) {
        (address leader, uint256 score) = getTopLeaderboardScore();
        if(score > 9000) {
            return true;
        }
        return false;
    }
}
```

Can you spot exactly which line in the above code is doing the unpacking? For me, unpacking sounds a lot like the destructuring concept of JavaScript. Here, unpacking means taking the values that are inside of the tuple and assigning them to variables so that you can use those variables directly later in your code.

This is the exact line that achieves it:

```solidity
(address leader, uint256 score) = getTopLeaderboardScore();
```

## Ignoring tuple values

If you paste the above code in Solidity, you will notice this warning for the `leader` variable:

`Warning: Unused local variable.`

If you want to get rid of this warning and generally omit a value from a tuple, you can leave an empty space:

```solidity
contract Tuples02 {
    function getTopLeaderboardScore() public pure returns (address, uint256) {
        return (0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045, 10000);
    }

    function highestScoreIsOver9000() public pure returns (bool) {
        // Leave an empty space, followed by a comma
        (, uint256 score) = getTopLeaderboardScore();
        if(score > 9000) {
            return true;
        }
        return false;
    }
}
```

## Why should we use tuples?

- Efficiency: Tuples let you return multiple values from a function without creating a struct (a data structure that we will see a bit later), saving storage costs.
- Readability: They make it easier to work with multiple return values, especially in short-lived calculations within a function.

[Original Article](https://www.rareskills.io/learn-solidity/tuples)
