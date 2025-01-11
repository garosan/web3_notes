# Payable Functions

Now we will learn how a smart contract can interact with the Ether cryptocurrency.

```solidity
contract Payable01 {
  function payMe() public payable {

  }

  function howMuchEtherIHave() public view returns (uint256) {
    return address(this).balance;
  }
}
```

After you deploy this contract in Remix, you will see that you now have a red button to which you can add ETH in the `value` input field. You can send Ether in units of Wei, Gwei, Finney, or Ether.

When we call `howMuchEtherIHave()` we actually get back `1000000000000000000`.

This is because Ether uses the same decimals strategy as the ERC20 tokens. So we are actually getting back 10^18 Wei which is equivalent to 1 Ether.

By the way you can use this little cool trick to determine how much ETH the address that called our function has:

```solidity
contract Payable02 {
    function howMuchEtherYouHave() public view returns (uint256) {
        return msg.sender.balance;
    }

    function howMuchEtherTheyHave(address them) public view returns (uint256) {
        return them.balance;
    }
}
```

If a function doesn't have the `payable` modifier, it will revert if you try to send ether to it.

By the way, solidity provides a very convenient keyword for dealing with all the zeros involved with Ether. Both of these functions do the same thing, but one is more readable.

```solidity
contract Payable03 {
    function moreThanOneEtherV1() public view returns (bool) {
        if (msg.sender.balance > 1 ether) {
            return true;
        }
        return false;
    }

        function moreThanOneEtherV2() public view returns (bool) {
        if (msg.sender.balance > 10**18) {
            return true;
        }
        return false;
    }
}
```

It is also valid to make a constructor payable, but you still need to explicitly send ether at construction time. Just because a function is payable does not mean that the person calling the function has to send Ether.

```solidity
contract Payable04 {

    constructor() payable {
        // begin life with money
    }
}
```

## Send Ether

It is pretty straight forward to send Ether when you call a function from the Remix IDE, but what if another contract wants to send Ether?

This is how you would do it:

```solidity
contract ReceiveEther {
    function takeMoney() public payable {

    }

    function myBalance() public view returns (uint256) {
        return address(this).balance;
    }
}

contract SendMoney {
    constructor() payable {

    }

    function sendMoney(address receiveEtherContract) public payable {
        uint256 amount = myBalance();
        (bool ok, ) = receiveEtherContract.call{value: amount}(
            abi.encodeWithSignature("takeMoney()")
        );
        require(ok, "transfer failed");
    }

    function myBalance() public view returns (uint256) {
        return address(this).balance;
    }
}
```

Since obviously changing the balance of a smart contract is a _state change_, `payable` functions cannot be view or pure.
