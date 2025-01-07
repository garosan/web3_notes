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

`remappings = ['@chainlink/contracts/=lib/chainlink-brownie-contracts/contracts/']`.

Also, make sure that you're importing the correct path inside your contracts:

`import {AggregatorV3Interface} from "@chainlink/contracts/src/v0.8/interfaces/AggregatorV3Interface.sol";`

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
pragma solidity 0.8.26;

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
pragma solidity 0.8.26;

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

The `forge-std/Test.sol` offers us a `console.log` capability, let's import it:

`import {Test, console} from "forge-std/Test.sol";`

And add some some console.logs:

```Solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.26;

import {Test, console} from "forge-std/Test.sol";

contract FundMeTest is Test {
    uint256 favNumber = 0;
    bool greatCourse = false;

    function setUp() external {
        favNumber = 1337;
        greatCourse = true;
        console.log("This will get printed first!");
    }

    function testDemo() public view {
        assertEq(favNumber, 1337);
        assertEq(greatCourse, true);
        console.log("This will get printed second!");
        console.log("Updraft is changing lives!");
        console.log(
            "You can print multiple things, for example this is a uint256, followed by a bool:",
            favNumber,
            greatCourse
        );
    }
}
```

`forge test` has an option called verbosity. By controlling this option we decide how verbose should the output of the `forge test` be. The default `forge test` has a verbosity of 1. Here are the verbosity levels, choose according to your needs:

```
Verbosity levels:

- 2: Print logs for all tests
- 3: Print execution traces for failing tests
- 4: Print execution traces for all tests, and setup traces for failing tests
- 5: Print execution and setup traces for all tests
```

If we want to see the console.logs, we would run `forge test -vv` (the number of v's indicates the level).

Let's delete the logs for now and deploy the `FundMe` contract.

For that, we will first need to import it into our test file, then declare it as a state variable and deploy it in the `setUp` function.

Our test file should look like this now:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.26;

import {Test, console} from "forge-std/Test.sol";
import {FundMe} from "../src/FundMe.sol";

contract FundMeTest is Test {
    FundMe fundMe;

    function setUp() external {
        fundMe = new FundMe();
    }

    function testMinimumDollarIsFive() public view {
        assertEq(fundMe.MINIMUM_USD(), 5e18);
    }
}
```

## More tests

Let's test that the owner is recorded properly, add this:

```solidity
function testOwnerIsMsgSender() public {
    assertEq(fundMe.i_owner(), msg.sender);
}
```

Run `forge test` and you will see an error:

```bash
[FAIL: assertion failed: 0x7FA9385bE102ac3EAc297483Dd6233D62b3e1496 != 0x1804c8AB1F12E6bbf3894d4083f33e07309d1f38] testOwnerIsMsgSender()
```

Why?

Technically we are not the ones that deployed the `FundMe` contract. The `FundMe` contract was deployed by the `setUp` function, which is part of the `FundMeTest` contract. So, even though we are the ones who called `setUp` via `forge test`, the actual testing contract is the deployer of the `FundMe` contract.

To fix this, we would need to tweak our test function like this:

```solidity
    function testOwnerIsMsgSender() public {
        assertEq(fundMe.i_owner(), address(this));

    }
```

## Advanced deploy scripts

When we went straight to testing, we left behind a very important element: deploy scripts. Why is this important you ask? Because we need a certain degree of flexibility that we can't obtain in any other way, let's look through the two files `FundMe.sol` and `PriceConverter.sol`, we can see that both have an address (`0x694AA1769357215DE4FAC081bf1f309aDC325306`) hardcoded for the AggregatorV3Interface. This address is valid, it matches the AggregatorV3 on Sepolia but what if we want to test on Anvil? What if we deploy on mainnet or Arbitrum? What then?

The deploy script is the key to overcome this problem!

Create a new file called `DeployFundMe.s.sol` in `script` folder. Please use the `.s.sol` naming convention.

It should look like this:

```solidity
// SPDX-License_identifier: MIT

pragma solidity 0.8.26;

import {Script} from "forge-std/Script.sol";
import {FundMe} from "../src/FundMe.sol";

contract DeployFundMe is Script {
    function run() external{
        vm.startBroadcast();
        new FundMe();
        vm.stopBroadcast();
    }

}
```

In my case it was weirdly referencing `Counter.sol`, so I had to run `forge clean` to fix it.

## Running tests on chain forks

This course will cover 4 different types of tests:

- **Unit tests**: Focus on testing individual smart contract functions or functionalities.
- **Integration tests**: Verify how a smart contract interacts with other contracts or external systems.
- **Forking tests**: Forking refers to creating a copy of a blockchain state at a specific point in time. This copy, called a fork, is then used to run tests in a simulated environment.
- **Staging tests**: Execute tests against a deployed smart contract on a staging environment before mainnet deployment.

Coming back to our contracts, the central functionality of our protocol is the `fund` function.

For that to work, we need to be sure that AggregatorV3 runs the current version. We know from previous courses that the version returned needs to be `4`. Let's put it to the test!

Add the following code to your test file:

```solidity
function testPriceFeedVersionIsAccurate() public {
    uint256 version = fundMe.getVersion();
    assertEq(version, 4);

}
```

It ... fails. But why? Looking through the code we see this AggregatorV3 address `0x694AA1769357215DE4FAC081bf1f309aDC325306` over and over again. The address is correct, is the Sepolia deployment of the AggregatorV3 contract. But our tests use Anvil for testing purposes, so that doesn't exist.

Let's use this to run tests just for the function we care about:

`forge test --mt testPriceFeedVersionIsAccurate`

But back to our problem, how can we fix it?

Forking is the solution we need. If we run the test on an anvil instance that copies the current Sepolia state, where AggregatorV3 exists at that address, then our test function will not revert anymore. For that, we need a Sepolia RPC URL.

So we would again create a `.env` file, add it to source with our env variable:

`SEPOLIA_RPC_URL=https://eth-sepolia.g.alchemy.com/v2/YOURAPIKEYWILLGOHERE`

and then run `forge test --mt testPriceFeedVersionIsAccurate --fork-url $SEPOLIA_RPC_URL`

You can run `forge coverage --fork-url $SEPOLIA_RPC_URL` to check your total code coverage.

## Refactoring your tests

Given the hardcoded Sepolia address we cannot deploy to other chains. To fix this we can make our project more modular, which would help improve the maintainability, testing and deployment. **This is done by moving the hardcoded changing variables to the constructor**, thus regardless of the chain we deploy our contracts to, we provide the chain-specific elements once, in the constructor, and then we are good to go.

In `FundMe.sol`, create a new storage variable:

`AggregatorV3Interface private s_priceFeed;`

Add this in the constructor:

```solidity
constructor(address priceFeed){
    i_owner = msg.sender;
    s_priceFeed = AggregatorV3Interface(priceFeed);

}
```

Then this:

```solidity
function getVersion() public view returns (uint256){
    AggregatorV3Interface priceFeed = AggregatorV3Interface(s_priceFeed);
    return priceFeed.version();

}
```

Too much stuff happening here, reference:
https://updraft.cyfrin.io/courses/foundry/foundry-fund-me/refactoring-testing

## ❓ Questions and 💪 Exercises

Question ❓: What is this AggregatorV3Interface.sol file? What exactly does it do?
Question ❓: You added some console.logs to your tests, you run `forge test` and you don't see them. What is missing?

## 🛠️ Links and Resources

- [Course Repo](https://github.com/Cyfrin/foundry-fund-me-cu)
- [FunWithStorage Contract](https://github.com/Cyfrin/foundry-fund-me-cu/blob/main/src/exampleContracts/FunWithStorage.sol)
- [Forge install](https://book.getfoundry.sh/reference/cli/forge/install)
- [Forge test](https://book.getfoundry.sh/reference/cli/forge/test)
- [Forge console.log](https://book.getfoundry.sh/reference/forge-std/console-log)
- [Forge coverage](https://book.getfoundry.sh/reference/forge/forge-coverage)
