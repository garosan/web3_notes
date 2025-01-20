# Lesson 02 - Building HelloWorld.sol in Remix

Solidity is a special language made to target the EVM, it's not supposed to have the same functionality as other general-purpose languages.

## Interfaces

Interfaces are similar to abstract contracts, but they cannot have any functions implemented. Also:

- They cannot inherit from other contracts, but they can inherit from other interfaces.
- All declared functions must be external in the interface, even if they are public in the contract.
- They cannot declare a constructor.
- They cannot declare state variables.
- They cannot declare modifiers.

Interfaces are basically limited to what the Contract ABI can represent

## ❓ Questions and 💪 Exercises

## 🛠️ Links and Resources

- [Notes](https://github.com/Encode-Club-Solidity-Bootcamp/Lesson-02)
- [Video](https://youtu.be/u59utjXGlIo)

- [Solidity Style Guide](https://docs.soliditylang.org/en/latest/style-guide.html)
- [NatSpec](https://docs.soliditylang.org/en/latest/natspec-format.html#natspec)
- [Solidity by Example - Interface](https://solidity-by-example.org/interface/)
