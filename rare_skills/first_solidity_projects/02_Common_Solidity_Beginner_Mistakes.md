# 20 Common Solidity Beginner Mistakes

[RareSkills Original Article](https://www.rareskills.io/post/solidity-beginner-mistakes)

## 1. Division before multiplication

## 2. Not following check-effects-interaction

Calling another contract or sending ETH to another address should be the last operation in a function. Failure to do so can leave the contract vulnerable to malicious attacks.

## 3. Using transfer or send

Solidity has two convenient functions `transfer()` and `send()` to send Ether from the contract to the destination. However, you should not use these functions.

Read [this article](https://diligence.consensys.io/blog/2019/09/stop-using-soliditys-transfer-now/) from Consensys. It's a classic that every Solidity developer must read at some point.

Why do these functions exist?

After the DAO hack, developers were very scared of re-entrancy attacks. To avoid such attacks, transfer() and send() were introduced as they limit the amount of gas available to the recipient. It results in preventing re-entrancy by starving the recipient of the gas needed to execute further code.

So don’t use transfer or send and don’t write re-entrant code. The first option is to replace transfer or send with address(receiver).call{value: amountToSend}(""). Alternatively, one can use the OpenZeppelin Address library to do the same thing

## 4. Using tx.origin instead of msg.sender

Using `tx.origin` to identify the caller opens up a security vulnerability. Suppose the user is phished into calling a malicious intermediate contract. In this situation, the malicious intermediate contract gains all the privileges of the wallet, allowing it to do any action the wallet is authorized to do — such as move funds.

Slither does not provide a warning regarding `tx.origin`.

## 5. Not using safeTransfer with ERC-20
