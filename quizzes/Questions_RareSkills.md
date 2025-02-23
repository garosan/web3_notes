# Over 150 interview questions for Ethereum Developers

[Original Article](https://www.rareskills.io/post/solidity-interview-questions)

## Easy

1. What is the difference between private, internal, public, and external functions?

<details>
<summary>Answer</summary>

Private functions can be called by the same contract and contracts that inherit it, internal functions can only be called by the contract, public functions can be called by anyone and external functions can only be called from outside the contract.

</details>

---

2. Approximately, how large can a smart contract be?

<details>
<summary>Answer</summary>

Approximately 24kb. This limitation was added in [EIP-170](https://eips.ethereum.org/EIPS/eip-170) as part of the Spurious Dragon fork to avoid DoS attacks.

</details>

---

3. What is the difference between create and create2?

<details>
<summary>Answer</summary>

IDK yet.

</details>

---

4. What major change with arithmetic happened with Solidity 0.8.0?

<details>
<summary>Answer</summary>

Starting with Solidity 0.8.0, all arithmetic operations revert on underflow and overflow by default, making the use of libraries like SafeMath unnecessary.

</details>

---

5. What special CALL is required for proxies to work?

<details>
<summary>Answer</summary>

IDK yet.

</details>
