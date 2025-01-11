# Receive

It was a bit annoying to have to abi encode a function just to send Ether. Luckily, Solidity has a nice way to handle this.

contract TakeMoney {

    receive() external payable {

    }

}

A couple of things:

- `receive()` is a special function, just like `constructor()` it doesn't have a _function_ keyword.
- We used `external` instead of `public`; `external` means it can **only** be called from outside the contract.
- So `receive()` **must** be `external` and `payable`.

Now let's try this code in Remix:

```solidity
contract TakeMoney {

    receive() external payable {

    }

    function viewBalance() public view returns (uint256) {
        return address(this).balance;
    }
}

contract ForwardMoney {
    function payMe() public payable {

    }

    function sendMoney(address luckyAddress) public payable {
        uint256 myBalance = viewBalance();
        luckyAddress.call{value: myBalance}("");
    }

    function viewBalance() public view returns (uint256) {
        return address(this).balance;
    }
}
```

What is happening in the code above?

- The `receive()` function allows the contract to accept money.
- The `sendMoney()` function takes an address as a parameter and _forwards_ all its balance to that address.

This construction is also how we send money to wallets.

Now here's a contract that allows only one address to withdraw ether:

```solidity
contract SaveMoney {
    function withdrawMoney()
        public
        payable {
            require(msg.sender ==
                0x5B38Da6a701c568545dCfcB03FcB875f56beddC4,
                "not the first remix address");
            msg.sender.call{value: viewBalance()}("");
    }

    function viewBalance()
        public
        view
        returns (uint256) {
            return address(this).balance;
    }

    // anyone can send
    receive()
        external
        payable {

    }
}
```

It might be confusing to do a _function call_ on a wallet that has no functions.

What could be even more confusing from the guide, is that there is no easy way to add ETH to the contract so that we can actually test that only the 1st Remix address can withdraw the money. This is very easy to solve, as we just need to add a payable `constructor()` and we will be able to send money from any address at deploy time:

```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

contract SaveMoney {
    constructor() payable {

    }

    // anyone can send
    receive() external payable {

    }

    function withdrawMoney()
        public
        payable {
            require(msg.sender ==
                0x5B38Da6a701c568545dCfcB03FcB875f56beddC4,
                "not the first remix address");
            msg.sender.call{value: viewBalance()}("");
    }

    function viewBalance()
        public
        view
        returns (uint256) {
            return address(this).balance;
    }
}
```
