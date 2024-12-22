# Section 2. Foundry Fund Me

## Introduction

We will learn:

- Putting our code on Github
- Explore storage using the FunWithStorage contract
- Professional deployment techniques with Foundry scripts
- Gas optimization techniques
- Debugging
- Creating professional tests
- Setting up a professional development environment
- How to use a price feed
- How to use Chisel

## Project Setup

```bash
mkdir foundry-fund-me
cd foundry-fund-me
forge init
```

## Finishing the setup

Delete the 3 `Counter` example files and copy the [fund me contract](https://github.com/Cyfrin/remix-fund-me-cu/blob/main/FundMe.sol), and the [PriceConverter.sol](https://github.com/Cyfrin/remix-fund-me-cu/blob/main/PriceConverter.sol)

After this if we try `forge compile` we will get an error. We need to install the chainlink dependencies with `forge install`:

`forge install smartcontractkit/chainlink-brownie-contracts@0.6.1 --no-commit`

We end the install command with `--no commit` in order to not create a git commit. More on this option later.

Go to the `lib` folder and you will see the newly installed `chainlink-brownie-contracts` library. Inside, you'll see a folder called `contracts` then a folder called `src`. Here we can find the `AggregatorV3Interface` that we are importing in `FundMe.sol`.

But now we need to indicate the correct path to our imports of where to look for these files, so in `foundry.toml` we need to add this:

`remappings = ['@chainlink/contracts/=lib/chainlink-brownie-contracts/contracts/']`

If you now try `forge compile` everything works correctly.

## Writing tests

Inside the `test` folder create a file called `FundMeTest.t.sol`. `.t.` is a naming convention of Foundry, please use it.

Let's now write our test file. We need to:

`import {Test} from "forge-std/Test.sol";`

and then, we will deploy our `FundMe` contract inside our `setUp` function. This function is always the first to execute whenever we run our tests. Here we will perform all the prerequisite actions that are required before doing the actual testing, things like:

- Deployments
- User addresses
- Balances
- Approvals

Let's also create a function called `testDemo`. Our file should look like this:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

import {Test} from "forge-std/Test.sol";

contract FundMeTest is Test {

    function setUp() external { }

    function testDemo() public { }

 }
```

Now run `forge test`. 1 empty test should pass.

Let's update the contract to the following and run `forge test` again:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

import {Test} from "forge-std/Test.sol";

contract FundMeTest is Test {
    uint256 favNumber = 0;
    bool greatCourse = false;

    function setUp() external {
        favNumber = 1337;
        greatCourse = true;
    }

    function testDemo() public view {
        assertEq(favNumber, 1337);
        assertEq(greatCourse, true);
    }
}
```

As you can see this is the order of operations:

1. We declare some state variables.
2. Next up `setUp()` is called.
3. After that forge runs all the test functions.

## Links

- [Course Repo](https://github.com/Cyfrin/foundry-fund-me-cu)
- [FunWithStorage Contract](https://github.com/Cyfrin/foundry-fund-me-cu/blob/main/src/exampleContracts/FunWithStorage.sol)
- [Forge install](https://book.getfoundry.sh/reference/cli/forge/install)
- [Forge test](https://book.getfoundry.sh/reference/cli/forge/test)
