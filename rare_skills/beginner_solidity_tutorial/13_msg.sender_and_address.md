# msg.sender and address(this)

Previously we saw an example of a badly architectured ERC20 token code:

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

The issue with the above code is that we have no idea who is calling the function.

Luckily, Solidity has a mechanism to identify who is calling the smart contract: `msg.sender`.
`msg.sender` returns the address of who is invoking the smart contract function.

The following code will return the address that called the contract:

```solidity
contract MsgSender01 {
    function whoami() public view returns (address) {
        address sender = msg.sender;
        return sender;
    }
}
```

**By combining msg.sender with an if statement, you can give certain addresses special privileges:**

```solidity
contract ERC20Token {
    address public banker = 0x5B38Da6a701c568545dCfcB03FcB875f56beddC4;

    mapping(address => uint256) public balances;

    function setSomeonesBalance(address owner, uint256 amount) public {
        if (msg.sender == banker) {
            balances[owner] = amount;
        }
        // do nothing
    }

    function transferTokensBetweenAddresses(
        address sender,
        address receiver,
        uint256 amount
    ) public {
        if (msg.sender == banker) {
            balances[sender] -= amount; // deduct/debit the sender's balance
            balances[receiver] += amount; // credit the reciever's balance
        }
        // do nothing
    }
}
```

The code above lets people view their balances (because balances is a public variable), but only the banker can change it.

For reference, these are the 1st 5 Remix accounts:

```
0x5B38Da6a701c568545dCfcB03FcB875f56beddC4
0xAb8483F64d9C6d1EcF9b849Ae677dD3315835cb2
0x4B20993Bc481177ec7E8f571ceCaE8A9e22C02db
0x78731D3Ca6b7E34aC0F824c42a7cC18A495cabaB
0x617F2E2fD72FD9D5503197092aC168c91465E7f2
```

Try to play a little bit with the code above, try assigning tokens to Acct1, Acct2, Acct3, etc. You can do this only if you are the banker (Acct1 in this case). You also can only transfer tokens from one account to the other if you are the banker.

Now consider this code, with just a small change, we can allow anyone to transfer **from their own balance** to someone else, just by using this line `balances[msg.sender] -= amount;` we make sure that the balance deducted when calling `transfer` is from `msg.sender`.

```solidity
contract ERC20 {
    address public banker = 0x5B38Da6a701c568545dCfcB03FcB875f56beddC4;

    mapping(address => uint256) public balances;

    function setSomeonesBalance(address owner, uint256 amount) public {
        if (msg.sender == banker) {
            balances[owner] = amount;
        }
        // do nothing
    }

    function transfer(address receiver, uint256 amount) public {
        balances[msg.sender] -= amount;
        balances[receiver] += amount;
    }
}
```

### tx.origin

There is another mechanism to get the sender, `tx.origin`, but **you should not use it**. We won't explain the security issues around `tx.origin` yet, but the point is, **don't use `tx.origin` unless you understand exactly what it does and its security issues**.

### address(this)

A smart contract can know its own address with the following code

```solidity
contract MsgSender02 {
    function whoami() public view returns (address) {
        return address(this);
    }
}
```

In my case this returns `0xB9e2A2008d3A58adD8CC1cE9c15BF6D4bB9C6d72`. This is the address of the smart contract we just deployed to our virtual blockchain inside Remix.

[Original Article](https://www.rareskills.io/learn-solidity/msg-sender-address-this)
