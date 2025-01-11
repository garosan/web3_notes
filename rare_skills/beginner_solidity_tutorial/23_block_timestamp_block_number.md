# Block.timestamp and Block.number

You can get the unix timestamp on the block with the `block.timestamp`:

```solidity
contract BlockTimeStamp01 {
    function timestamp() public view returns (uint256) {
        return block.timestamp;
    }
}
```

The `timestamp()` function will give you back a long number which corresponds to the amount of seconds elapsed since January 1, 1970 UTC, the traditional unix epoch.

You can use this [epoch converter](https://www.epochconverter.com/) to convert it to human-readable time.

In my case it converted to: `Saturday, November 9, 2024 3:00:10 PM GMT-06:00`.

The timestamp that you get back is what the validator put into the block when they produced it.

So once again, even though when you call the function in Remix sometimes it might give you a timestamp that aligns closely with the current time, **on a live network, `block.timestamp` represents the time recorded by the validator at the moment they included the block in the chain.**

With the next code, you can ensure someone doesn't call a function more than once per day:

```solidity
contract BlockTimestamp02 {

    uint256 public lastCall;

    function hasCooldown() public {
        uint256 day = 60 * 60 * 24;
        require(block.timestamp > lastCall + day, "hasn't been a day");
        lastCall = block.timestamp;
    }
}
```

But you don't need to do that nasty `uint256 day = 60 * 60 * 24;` since Solidity allows for this:

```solidity
contract BlockTimestamp03 {

    uint256 public lastCall;

    function hasCooldown() public {
        require(block.timestamp > lastCall + 1 days, "hasn't been a day");
        lastCall = block.timestamp;
    }
}
```

In fact, `seconds`, `minutes`, `hours`, `days`, and `weeks` are all valid units of time.

## block.number

You can also know what block number you are on with this variable.

Don't use `block.number` to track time, only to enforce ordering of transactions.

```solidity
contract Block03 {
    function whatBlockIsIt() external view returns (uint256) {
        return block.number;
    }
}
```

This is always returning `159` for me in Remix.

You can use the following code to ensure that a function is called after another one, that is, in a later block.

```solidity
contract Block04 {
    // defaults to zero
    uint256 private calledAt;

    function callMeFirst() external {
        calledAt = block.number;
    }

    function callMeSecond() external view {
        require(calledAt != 0 && block.number > calledAt, "callMeFirst() not called");
    }
}
```
