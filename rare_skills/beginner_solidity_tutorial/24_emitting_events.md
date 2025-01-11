# Emitting Events

Technically, our "ERC20" token is not fully ERC20 compliant. It's missing an important feature: `events`.

General rule of thumb: **If a function causes a state change, it should be logged**.

Why log things? Isn't it the case that the blockchain already immutably stores every transaction?

That's true. But logging certain kinds of events helps us find events (transactions) that we are looking for much faster and in a more organized fashion.

This is how your cryptocurrency wallet can quickly discover your ERC20 balance. It would be pretty annoying to have to look through every transaction that ever occurred on an ERC20 token to discover if you own any. But logs are stored in such a way that this retrieval is efficient.

**Events cannot be seen by other smart contracts. They are optimized for being queried offchain.**

```solidity
contract Events01 {
    event Deposit(address indexed depositor, uint256 amount);

    receive() external payable {
        emit Deposit(msg.sender, msg.value);
    }
}
```

An event can have up to 3 indexed types, but there isn't a strict limit on the number of unindexed parameters.

By the way, argument names after the datatype is optional. We could have written the event above as

```solidity
event Deposit(address indexed, uint256);
```

When should a variable be indexed or not? If you might be interested in finding that value quickly, like _"has an address been involved with this token contract?"_ then you should index it. You're probably not interested in searching if this contract has ever received exactly "x" amount of tokens so we don't index the amount.

And finally, here's our ERC-20 token with events:

```solidity
// SPDX-License-Identifier: MIT
// Final version of our ERC-20 Token
pragma solidity ^0.8.0;

contract ERC20 {
    string public name;
    string public symbol;
    mapping(address => uint256) public balanceOf;
    address public owner;
    uint8 public decimals;
    uint256 public totalSupply;

    // owner -> spender -> allowance
    // this enables an owner to give allowance to multiple addresses
    mapping(address => mapping(address => uint256)) public allowance;

    event Transfer(address indexed _from, address indexed _to, uint256 _value);
    event Approval(address indexed _owner, address indexed _spender, uint256 _value);

    constructor(string memory _name, string memory _symbol) {
        name = _name;
        symbol = _symbol;
        decimals = 18;

        owner = msg.sender;
    }

    function mint(address to, uint256 amount) public {
        require(msg.sender == owner, "only owner can create tokens");
        totalSupply += amount;
        balanceOf[owner] += amount;

        emit Transfer(address(0), owner, amount);
    }

    function transfer(address to, uint256 amount) public returns (bool) {
        return helperTransfer(msg.sender, to, amount);
    }

    function approve(address spender, uint256 amount) public returns (bool) {
        allowance[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);

        return true;
    }

    function transferFrom(address from, address to, uint256 amount) public returns (bool) {
        if (msg.sender != from) {
            require(
                allowance[from][msg.sender] >= amount,
                "not enough allowance"
            );

            allowance[from][msg.sender] -= amount;
        }

        return helperTransfer(from, to, amount);
    }

    function helperTransfer(address from, address to, uint256 amount) internal returns (bool) {
        require(balanceOf[from] >= amount, "not enough money");
        require(to != address(0), "cannot send to address(0)");
        balanceOf[from] -= amount;
        balanceOf[to] += amount;

        emit Transfer(from, to, amount);
        return true;
    }
}
```
