# Arrays in Storage

Previously in our section about arrays we left off:

- Writing to indexes in the array
- Appending to an array
- Popping from an array

This is because you very rarely do that to arrays that are supplied as function arguments. However, when arrays are in storage, this operation is more common.

Example:

```solidity
contract ArraysInStorage01 {
    uint256[] public myArray;

    function setMyArray(uint256[] calldata newArray) public {
        myArray = newArray;
    }

    function addToArray(uint256 newItem) public {
        myArray.push(newItem);
    }

    function removeFromArray() public {
        myArray.pop();
    }

    function getLength() public view returns (uint256) {
        return myArray.length;
    }

    function getEntireArray() public view returns (uint256[] memory) {
        return myArray;
    }
}
```

If you copy and paste this code into Remix and interact with the functions `setMyArray`, `addToArray`, `removeFromArray`, and `getLength` you won't see any surprises.

It is worth noting that because myArray is public, Remix shows it as visible as a function. This means that the compiler will automatically generate a function called myArray() that can be called to read the values stored in myArray.

Because `myArray` is public, Remix shows it as visible as a function. But it will not return the entire array. It will ask for an index and return the value at that index.

However, the function getEntireArray() returns the entire array.

Note that pop() does not return the value.

### Removing an item

Solidity does not have a way to remove an item in the middle of a list and reduce the length by one. The following code is valid, but it does not change the length of the list.

```solidity
contract ExampleContract {

    uint256[] public myArray;

    function removeAt(uint256 index) public {
        delete myArray[index];
        // sets the value at index to be zero

        // the following code is equivalent
        // myArray[index] = 0;

        // myArray.length does not change in either circumstance
    }
}
```

If you want to remove an item and also reduce the length, you must do a "pop and swap". It removes the element at the index argument and swaps it with the last element in the array:

```solidity
contract ExampleContract {

    uint256[] public myArray;

    function popAndSwap(uint256 index) public {
        uint256 valueAtTheEnd = myArray[myArray.length - 1];
        myArray.pop(); // reduces the length;
        myArray[index] = valueAtTheEnd;
    }
}
```

Solidity cannot delete from the middle of the list and preserve the array's original order.

### Strings

Strings behave similarly to arrays, except when they are public they return the entire string, because strings cannot be indexed (confusing, isn't it?). There is no pop or length operation for strings.

```solidity
contract ExampleContract {

    string public name;

    function setName(string calldata newName)
            public {
        name = newName;
    }
}
```

[Original Article](https://github.com/RareSkills/learn-solidity/blob/prod/content/09-arrays-in-storage.md)
