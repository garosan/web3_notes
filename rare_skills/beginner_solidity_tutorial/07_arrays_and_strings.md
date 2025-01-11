# Introduction to arrays and strings

In this section we will introduce the `array` data structure and the `string` data structure.

Let's look at a function that takes an array and returns an array.

First, this is the syntax for declaring an array of numbers is uint256[]. For now, ignore the keywords `calldata` and `memory`.

```solidity
contract Arrays01 {
    function useArrayForUint256(
        uint256[] calldata input
    ) public pure returns (uint256[] memory) {
        return input;
    }
}
```

This is the correct format to input your array in Remix: `[1,2,3,4]`

If you wanted an array of addresses or booleans, it would be the following:

```solidity
contract Arrays02 {
    function booleanArrayExample(
        bool[] calldata input
    ) public pure returns (bool[] memory) {
        return input;
    }

    function addressArrayExample(
        address[] calldata input
    ) public pure returns (address[] memory) {
        return input;
    }
}
```

## `calldata` and `memory` keywords

So what is this `calldata` and `memory` bit? First off, if you don't include them, the code won't compile.

So what are `calldata` and `memory`?

Memory in Solidity is like the heap in C, C++, or Rust. Dynamic arrays can have unlimited size, so storing them on the execution stack (don't worry if you don't know what that is), could lead to a stackoverflow error.

`Calldata` is something unique to Solidity. It is the actual "transaction data" that is sent when someone transmits a transaction to the blockchain.

`Calldata` means _refer to the data in the Ethereum transaction itself._ This is a fairly advanced concept, so don't worry if you don't fully understand it for now.

When in doubt: the function arguments for arrays and strings should be calldata and the function arguments for the return type should be memory.

There are some exceptions to using `calldata` in a function argument, but the return type for an array should always be `memory`, never `calldata`, or the code won't compile. To avoid bombarding you with information, we will talk about the exceptions to calldata later.

## More about `calldata` and `memory`

In Solidity, `calldata` and `memory` are keywords used to specify the data location for function parameters and variables, particularly when working with reference types like arrays, structs, or mappings.

Read more:

- `calldata`: is a non-modifiable, temporary location where function arguments are stored when the function is called. It's read-only and cannot be modified within the function.

- `memory`: is a modifiable, temporary location that exists only for the duration of the function call. Variables in memory can be modified within the function, but they do not persist outside the function call.

One key difference between `memory` and `calldata` is that `memory` can be modified by the function, while `calldata` cannot.

- [Alchemy Article](https://docs.alchemy.com/docs/what-is-the-difference-between-memory-and-calldata-in-solidity)

## Arrays are zero-indexed like every other language

Note that if we try to access a position in an array, and the array is empty, the transaction will revert.

Similarly, if we try to access an index that doesn't exist, the transaction will revert. Try looking for the 6th index:

```solidity
contract Arrays03 {
    uint256[] myArray = [1,2,3,4,5,6];

    function getElementAtIndex(uint256 index) public view returns (uint256) {
        return myArray[index];
    }
}
```

To get the length of an array, use `.length`.

This is also how you can loop over an array.

```solidity
contract Arrays04 {
    uint256[] myArray = [1,2,3,4,5,6];

    function productOfarray(
        uint256[] calldata myArray
    ) public pure returns (uint256) {
        uint256 product = 1;
        for (uint256 i = 0; i < myArray.length; i++) {
            product *= myArray[i];
        }
        return product;
    }
}
```

## Arrays can be declared to have a fixed length

If you want to force an array to have a fixed size, you can put the size inside of the square brackets.

```solidity
contract Arrays05 {
    function productOfarray(
        uint256[5] calldata myArray
    ) public pure returns (uint256) {
        uint256 last = myArray[4];
        return last;
    }
}
```

If the function is passed an array of any size other than 5, it will revert with an error of missing or too many arguments.

## Strings

Strings behave very similar to arrays. In fact, they are arrays under the hood (but with some differences). Here is a function that returns the string you passed it.

```solidity
contract ExampleContract {
    function echo(string calldata input)
        public
        pure
        returns (string memory) {
            return input;
    }
}
```

### Concatenating strings

Funnily enough, solidity did not support string concatenation until February 2022 when Solidity 0.8.12 was released. If you want to do string concatenation in Solidity, make sure the pragma at the top of the file is at least 0.8.12.

This how you would concatenate in Solidity:

```solidity
pragma solidity ^0.8.12;
contract ExampleContract {
    function useArrays(string calldata user)
        public
        pure
        returns(string memory) {
            return string.concat("hello ", user);
    }
}
```

There is a reason support for concatenation was added so late, smart contracts usually deal with numbers, not strings.

### Strings cannot be indexed

In languages like JavaScript or Python, you can index a string like you would an array and get a character back. Solidity cannot do this. The following code won't compile:

```solidity
pragma solidity ^0.8.12;
contract BadContract {
    function useArrays(
        string calldata input
    ) public pure returns (string memory) {
        return input[0]; // error
    }
}
```

### Strings do not support length

This is because unicode characters can make the length ambiguous, and solidity represents strings as a byte array, not a sequence of characters. So just remember you can't call `.length` on a string.

### Keep in mind for later

- Declaring arrays and strings inside a function, as opposed to in the argument or return value, has a different syntax.

[Original Article](https://www.rareskills.io/learn-solidity/arrays)
