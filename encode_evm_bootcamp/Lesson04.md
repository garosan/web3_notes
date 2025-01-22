# Lesson 04 - Interfaces and external calls

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.26;

contract HelloWorld {
    string private text;
    address public owner;

    modifier onlyOwner()
    {
        require (msg.sender == owner, "Caller is not the owner");
        _;
    }

    constructor() {
        text = "Hello World";
        owner = msg.sender;
    }

    function helloWorld() public view returns (string memory) {
        return text;
    }

    function setText(string calldata newText) public onlyOwner {
        text = newText;
    }

    function transferOwnership(address newOwner) public onlyOwner {
        owner = newOwner;
    }
}
```

## ❓ Questions and 💪 Exercises

Exercise 💪: Deploy `HelloWorld.sol` and interact with it, change message strings and change owners.

## 🛠️ Links and Resources

- [Notes](https://github.com/Encode-Club-Solidity-Bootcamp/Lesson-04)
- [Video](https://youtu.be/uuBSaLf1bv4)

- [Solidity by Example - Function Selectors](https://solidity-by-example.org/function-selector/)
- [Solidity Cheatsheet](https://docs.soliditylang.org/en/latest/cheatsheet.html)
