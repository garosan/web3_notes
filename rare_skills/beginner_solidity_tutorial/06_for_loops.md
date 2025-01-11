# For Loops

Just like if statements, there is nothing surprising about for loops.

Here is the code to add up all the numbers from 1 to 99:

```solidity
contract ExampleContract {
    function addNumbers()
        public
        pure
        returns (uint256) {
            uint256 sum = 0;
            for (uint256 i = 0; i < 100; i++) {
                sum += i;
            }
            return sum;
    }
}
```

A very natural use-case for for loops is iterating over an array. But arrays will be introduced in a later chapter.

Like other languages, you can do an early return from a function inside a for loop. This code will loop from 2 to the number given until it finds a prime factor (prime factors are the prime numbers that, when multiplied together, give a particular number):

If we pass for example `49`, this should return `7`.

```solidity
contract PrimeFactorFinder {
    function findPrimeFactor(uint256 x) public pure returns (uint256) {
        // start at 2, 1 is not a prime factor
        // use <= because x might be prime
        for (uint256 i = 2; i <= x; i++) {
            if (x % i == 0) {
                return i;
            }
        }
    }
}
```

Once again, if you pass a number like `1` or `0` the function will return `0`.

Exercise:

```solidity
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.13;

contract IsPrime {
    /**
     * The goal of this exercise is to return if "number" is prime or not (true or false)
     */
    function isPrime(uint256 number) public view returns (bool) {
        // your code here
    }
}
```

Solution:

```solidity
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.13;

contract IsPrime {
    function isNumberPrime(uint256 num) public pure returns (bool) {
        uint256 result = findPrimeFactor(num);
        if (result == num) {
            return true;
        } else {
            return false;
        }
    }

    function findPrimeFactor(uint256 x) public pure returns (uint256) {
        // start at 2, 1 is not a prime factor
        // use <= because x might be prime
        for (uint256 i = 2; i <= x; i++) {
            if (x % i == 0) {
                return i;
            }
        }
    }
}
```

Just a curiosity that if you pass in even a relatively small prime number like `7877` the computation will take several seconds to return.
