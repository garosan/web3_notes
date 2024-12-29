# Section 02. Storage Factory

3 contracts:

- `SimpleStorage.sol`: The contract from the previous lesson with slight modifications
- `AddFiveStorage.sol`: A child contract of `SimpleStorage`
- `StorageFactory.sol`: A contract that will deploy `SimpleStorage` and interact with it

We begin with this version of `SimpleStorage.sol`:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.26;

// pragma solidity ^0.8.0;
// pragma solidity >=0.8.0 <0.9.0;

contract SimpleStorage {
    uint256 myFavoriteNumber;

    struct Person {
        uint256 favoriteNumber;
        string name;
    }
    // uint256[] public anArray;
    Person[] public listOfPeople;

    mapping(string => uint256) public nameToFavoriteNumber;

    function store(uint256 _favoriteNumber) public {
        myFavoriteNumber = _favoriteNumber;
    }

    function retrieve() public view returns (uint256) {
        return myFavoriteNumber;
    }

    function addPerson(string memory _name, uint256 _favoriteNumber) public {
        listOfPeople.push(Person(_favoriteNumber, _name));
        nameToFavoriteNumber[_name] = _favoriteNumber;
    }
}
```

This is `StorageFactory.sol` final code:

```solidity
// SPDX-License-Identifier: MIT

pragma solidity 0.8.26;

import {SimpleStorage} from "./SimpleStorage.sol";

contract StorageFactory {
    SimpleStorage[] public listOfSimpleStorageContracts;

    function createSimpleStorageContract() public {
        SimpleStorage simpleStorageContractVariable = new SimpleStorage();
        // SimpleStorage simpleStorage = new SimpleStorage();
        listOfSimpleStorageContracts.push(simpleStorageContractVariable);
    }

    function sfStore(
        uint256 _simpleStorageIndex,
        uint256 _simpleStorageNumber
    ) public {
        // Address
        // ABI
        // SimpleStorage(address(simpleStorageArray[_simpleStorageIndex])).store(_simpleStorageNumber);
        listOfSimpleStorageContracts[_simpleStorageIndex].store(
            _simpleStorageNumber
        );
    }

    function sfGet(uint256 _simpleStorageIndex) public view returns (uint256) {
        // return SimpleStorage(address(simpleStorageArray[_simpleStorageIndex])).retrieve();
        return listOfSimpleStorageContracts[_simpleStorageIndex].retrieve();
    }
}
```

- Remember that you should always try to use **named imports**.

### Interacting with a contract's ABI

Every time you want to interact with another contract, you need:

- The contract address
- The contract ABI (If you don't have the full ABI available a function selector can suffice)

### Inheritance

We finally learn about inheritance with the `virtual` and `override` keywords.

## Exercises and Questions

Exercise 💪: Create a contract `AnimalFactory` that includes a function `createAnimals`. This function must be capable of deploying the other 2 contracts `Cows` and `Birds`, which are simple contracts with just a constructor method.

## Links

- [GitHub Repo](https://github.com/Cyfrin/remix-storage-factory-cu)
- [Solidity Factory Pattern](https://betterprogramming.pub/learn-solidity-the-factory-pattern-75d11c3e7d29)
- [The `new` keyword](https://docs.soliditylang.org/en/latest/control-structures.html#creating-contracts-via-new)
- [Solidity Docs - Inheritance](https://docs.soliditylang.org/en/v0.8.28/contracts.html#inheritance)
