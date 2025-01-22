# Lesson 05 - Vscode setup and code quality

Lets start by cloning OpenZeppelin contracts repository:

```bash
git clone https://github.com/OpenZeppelin/openzeppelin-contracts.git
cd .\openzeppelin-contracts\
npm install

npm run compile # Everything should compile successfully
```

Let's run an example test:

```bash
npm run test ./test/token/ERC20/ERC20.test.js
...
> openzeppelin-solidity@5.0.2 test
> hardhat test ./test/token/ERC20/ERC20.test.js
...
126 passing (5s)
```

To create a breaking change, open this file:

`code ./contracts/token/ERC20/ERC20.sol`

Change line `78`:

```solidity
function decimals() public view virtual override returns (uint8) {
    return 42;
}
```

Run the tests again and they will fail.

## ❓ Questions and 💪 Exercises

## 🛠️ Links and Resources

- [Notes](https://github.com/Encode-Club-Solidity-Bootcamp/Lesson-05)
- [Video](https://youtu.be/YOf9UtGexSc)
