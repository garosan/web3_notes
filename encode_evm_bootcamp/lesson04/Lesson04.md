# Lesson 04 - Interfaces and external calls

- [Notes](https://github.com/Encode-Club-Solidity-Bootcamp/Lesson-04)
- [Video](https://www.youtube.com/watch?v=uuBSaLf1bv4)

Questions from last lessons:

- In Solidity, you can't directly compare a string to another string, you have to compare their hashes:

```solidity
function compareString() internal view returns (bool) {
    return keccak256(bytes("string1")) == keccack256(bytes("string1"));
}
```

- Difference between `calldata` and `memory` keywords.

## Lesson

The MethodID is the 'function selector', 4 bytes of information that allows us to specify an individual function inside the smart contract.

The selector is calculated hashing the name of the function + the argument types.

A function selector is the first 4 bytes of the Keccak-256 hash of the function signature.

If you hit a case where there's a function selector collision the Solidity compiler will handle it.

Function selectors are used internally when:

- You send a transaction with data
- You call a contract via low-level functions like `.call()`
- You write a proxy contract or work with upgradeable patterns

## Interfaces

An interface in Solidity is like a contract blueprint — it defines what functions exist in another contract, but not how they are implemented.

**Why use interfaces?**

- To interact with external contracts without needing their full source code.
- To ensure compatibility with known standards (like ERC-20, ERC-721).
- To keep your code modular and cleaner.

### What Does It Mean to "Attach" an Interface to a Contract?

It means you **declare that another contract exists** at a known address, and you’ll be **using the interface** to call its functions.

### 🧠 Example: Using an ERC-20 Token Interface

```solidity
// Define the interface
interface IERC20 {
    function transfer(address to, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
}
```

Now, in your contract, you "attach" this interface to a known token address like this:

```solidity
contract TokenSpender {
    address public tokenAddress;

    constructor(address _token) {
        tokenAddress = _token;
    }

    function sendTokens(address to, uint256 amount) external {
        // Attach interface to the contract at `tokenAddress`
        IERC20 token = IERC20(tokenAddress);
        token.transfer(to, amount); // Call transfer() using the interface
    }
}
```

💡 Here’s what’s happening:

- `IERC20` is the interface.
- `IERC20(tokenAddress)` creates a reference to the token contract.
- You can now call `transfer` and `balanceOf` on that token contract safely.

### 🛑 Interfaces Cannot:

- Have constructors.
- Have state variables.
- Implement any logic — they only declare functions and events.

### 🔥 TL;DR

| Term             | Meaning                                                               |
| ---------------- | --------------------------------------------------------------------- |
| Interface        | Defines what functions exist on a contract                            |
| Attach Interface | Create a reference to a deployed contract using that interface        |
| Use Case         | Interact with other contracts (like tokens) without copying full code |

## Fallback and Receive

Skipped

## Restricting Access

- [Solidity Docs - Restricting Access](https://docs.soliditylang.org/en/latest/common-patterns.html#restricting-access)

## Other

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
