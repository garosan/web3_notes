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

Remember that as of Feb 1st, 2025, the contracts now live in:

`import {AggregatorV3Interface} from "@chainlink/contracts/src/v0.8/interfaces/AggregatorV3Interface.sol";`

Update the path accordingly.

After this if we try `forge compile` we will get an error. We need to install the chainlink dependencies with `forge install`:

`forge install smartcontractkit/chainlink-brownie-contracts@0.6.1 --no-commit`

We end the install command with `--no commit` in order to not create a git commit. More on this option later.

Go to the `lib` folder and you will see the newly installed `chainlink-brownie-contracts` library. Inside, you'll see a folder called `contracts` then a folder called `src`. Here we can find the `AggregatorV3Interface` that we are importing in `FundMe.sol`.

But now we need to indicate the correct path to our imports of where to look for these files, so in `foundry.toml` we need to add this:

`remappings = ['@chainlink/contracts/=lib/chainlink-brownie-contracts/contracts/']`.

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
3. After that, forge runs all the test functions.

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
function testOwnerIsMsgSender() public view {
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
function testPriceFeedVersionIsAccurate() public view {
    uint256 version = fundMe.getVersion();
    assertEq(version, 4);

}
```

It ... fails. But why? Looking through the code we see this AggregatorV3 address `0x694AA1769357215DE4FAC081bf1f309aDC325306` over and over again. The address is correct, its the Sepolia deployment of the AggregatorV3 contract. But our tests use Anvil for testing purposes, so that doesn't exist.

Let's use this to run tests just for the function we care about:

`forge test --mt testPriceFeedVersionIsAccurate`

As a curiosity if you had two functions named the same in different files, the above will run both functions.

But back to our problem, how can we fix it?

Forking is the solution we need. If we run the test on an anvil instance that copies the current Sepolia state, where AggregatorV3 exists at that address, then our test function will not revert anymore. For that, we need a Sepolia RPC URL.

So we would again create a `.env` file with our env variable:

`SEPOLIA_RPC_URL=https://eth-sepolia.g.alchemy.com/v2/YOURAPIKEYWILLGOHERE`

Remember to add it to source with `source .env`.

Then run `forge test --mt testPriceFeedVersionIsAccurate --fork-url $SEPOLIA_RPC_URL`

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

Ok, so basically we are changing our `FundMe.sol` and `PriceConverter.sol` files to not hardcode the address for the priceFeed, but rather provide it at deploy time.

Our files should look like this:

`FundMe.sol`:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.26;

import {AggregatorV3Interface} from "@chainlink/contracts/src/v0.8/interfaces/AggregatorV3Interface.sol";
import {PriceConverter} from "./PriceConverter.sol";

error FundMe__NotOwner();

contract FundMe {
    using PriceConverter for uint256;

    mapping(address => uint256) public addressToAmountFunded;
    address[] public funders;

    address public i_owner;
    uint256 public constant MINIMUM_USD = 5 * 10 ** 18;
    AggregatorV3Interface private s_priceFeed;

    constructor(address priceFeed) {
        i_owner = msg.sender;
        s_priceFeed = AggregatorV3Interface(priceFeed);
    }

    function fund() public payable {
        require(
            msg.value.getConversionRate(s_priceFeed) >= MINIMUM_USD,
            "You need to spend more ETH!"
        );
        // require(PriceConverter.getConversionRate(msg.value) >= MINIMUM_USD, "You need to spend more ETH!");
        addressToAmountFunded[msg.sender] += msg.value;
        funders.push(msg.sender);
    }

    function getVersion() public view returns (uint256) {
        return s_priceFeed.version();
    }

    modifier onlyOwner() {
        // require(msg.sender == owner);
        if (msg.sender != i_owner) revert FundMe__NotOwner();
        _;
    }

    function withdraw() public onlyOwner {
        for (
            uint256 funderIndex = 0;
            funderIndex < funders.length;
            funderIndex++
        ) {
            address funder = funders[funderIndex];
            addressToAmountFunded[funder] = 0;
        }
        funders = new address[](0);

        (bool callSuccess, ) = payable(msg.sender).call{
            value: address(this).balance
        }("");
        require(callSuccess, "Call failed");
    }

    fallback() external payable {
        fund();
    }

    receive() external payable {
        fund();
    }
}
```

`PriceConverter.sol`:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.26;

import {AggregatorV3Interface} from "@chainlink/contracts/src/v0.8/interfaces/AggregatorV3Interface.sol";

library PriceConverter {
    function getPrice(
        AggregatorV3Interface priceFeed
    ) internal view returns (uint256) {
        (, int256 answer, , , ) = priceFeed.latestRoundData();
        return uint256(answer * 10000000000);
    }

    function getConversionRate(
        uint256 ethAmount,
        AggregatorV3Interface priceFeed
    ) internal view returns (uint256) {
        uint256 ethPrice = getPrice(priceFeed);
        uint256 ethAmountInUsd = (ethPrice * ethAmount) / 1000000000000000000;
        return ethAmountInUsd;
    }
}
```

`//TODO. CLarify exactly why we made that change in PriceConverter.sol`

Take a moment and think if we missed updating anything in our project.

You probably know the answer, right? We are missing to provide the priceFeed address in our deploy script and in our test. Let's hardcode it for now:

`DeployFundMe.s.sol` should have this now:

```solidity
vm.startBroadcast();
new FundMe(0x694AA1769357215DE4FAC081bf1f309aDC325306);
vm.stopBroadcast();
```

Now we could do the same thing for our tests, but it would then become tedious and not a great practice to keep updating things in both files, so let's modify our deploy script so that we can use it in our test file. Your `DeployFundMe.s.sol` should look like this:

```solidity
// SPDX-License_identifier: MIT

pragma solidity 0.8.26;

import {Script} from "forge-std/Script.sol";
import {FundMe} from "../src/FundMe.sol";

contract DeployFundMe is Script {
    function run() external returns (FundMe) {
        vm.startBroadcast();
        FundMe fundMe = new FundMe(0x694AA1769357215DE4FAC081bf1f309aDC325306);
        vm.stopBroadcast();
        return fundMe;
    }
}
```

Now for our tests, let's import the deployment script into the `FundMe.t.sol`:

`import {DeployFundMe} from "../script/DeployFundMe.s.sol";`

- Create a new state variable `DeployFundMe deployFundMe;`;
- Update the `setUp` function:

```solidity
contract FundMeTest is Test {
    FundMe fundMe;
    DeployFundMe deployFundMe;

    function setUp() external {
        deployFundMe = new DeployFundMe();
        fundMe = deployFundMe.run();
    }

    function testMinimumDollarIsFive() public view {
        assertEq(fundMe.MINIMUM_USD(), 5e18);
    }
```

Let's call a `forge test --fork-url $SEPOLIA_RPC_URL` to make sure everything compiles.

Looks like the `testOwnerIsMsgSender` fails again. Take a moment and think about why.

This surely must have to do with the fact that the deployer is coming now from our deploy script and not natively from within our setUp function.

**Note**: `vm.startBroadcast` is special, it uses the address that calls the test contract or the address / private key provided as the sender.

To account for the way `vm.startBroadcast` works, perform the following modification in `FundMe.t.sol`:

```solidity
function testOwnerIsMsgSender() public {
    assertEq(fundMe.i_owner(), msg.sender);
}
```

Run `forge test --fork-url $SEPOLIA_RPC_URL` again. Everything should work now.

## Deploying a mock priceFeed

The problem: So we've come a long way but we still need to call `--fork-url` with our tests, otherwise if we just run `forge test`, our `testPriceFeedVersionIsAccurate()` test function is going to fail because such contract doesn't exist in our local Anvil blockchain. And we don't want to be calling our `SEPOLIA_RPC_URL` every time we run tests, or we're going to run out of credits very easily. Enter mocks.

We are going to write a mock priceFeed to interact with in our local Anvil blockchain for the duration of our local tests.

Create a file in `script/HelperConfig.s.sol`.

We want to achieve 2 things:

- Deploy mocks when we are on a local anvil chain
- Keep track of contract addresses across different chains

Let's begin by setting the 2 functions that we're gonna need:

```solidity
// SPDX-License-Identifier: MIT

pragma solidity 0.8.26;

import {Script} from "forge-std/Script.sol";

contract HelperConfig {
    function getSepoliaEthConfig() public pure {
        // priceFeed address
    }

    function getAnvilEthConfig() public pure {
        // priceFeed address
    }
}
```

Now instead of passing a bunch of variables with everything we need, let's create a struct for our configuration.

Our file should look like this now:

```solidity
// SPDX-License-Identifier: MIT

pragma solidity 0.8.26;

import {Script} from "forge-std/Script.sol";

contract HelperConfig {
    NetworkConfig public activeNetworkConfig;

    struct NetworkConfig {
        address priceFeed; // ETH/USD price feed address
    }

    constructor() {
        if (block.chainid == 11155111) {
            activeNetworkConfig = getSepoliaEthConfig();
        } else {
            activeNetworkConfig = getAnvilEthConfig();
        }
    }

    function getSepoliaEthConfig() public pure returns (NetworkConfig memory) {
        // priceFeed address
        NetworkConfig memory sepoliaConfig = NetworkConfig({
            priceFeed: 0x694AA1769357215DE4FAC081bf1f309aDC325306
        });
        return sepoliaConfig;
    }

    function getAnvilEthConfig() public pure returns (NetworkConfig memory) {
        // priceFeed address
    }
}
```

Now let's modify our deploy script to include our new `HelperConfig`:

```solidity
// SPDX-License_identifier: MIT

pragma solidity 0.8.26;

import {Script} from "forge-std/Script.sol";
import {FundMe} from "../src/FundMe.sol";
import {HelperConfig} from "./HelperConfig.s.sol";

contract DeployFundMe is Script {
    function run() external returns (FundMe) {
        HelperConfig helperConfig = new HelperConfig();
        address ethUsdPriceFeed = helperConfig.activeNetworkConfig();

        vm.startBroadcast();
        FundMe fundMe = new FundMe(ethUsdPriceFeed);
        vm.stopBroadcast();
        return fundMe;
    }
}
```

If we now run `forge test --fork-url $SEPOLIA_RPC_URL` the test should pass correctly.

## 💪 Exercises Time

Now let's do a little exercise. What if I want to test this on Base Sepolia? or Optimism Sepolia?

I already have my RPC_URL configured for Optimism Sepolia so let me run:

`forge test --fork-url $OPTIMISM_SEPOLIA_RPC_URL`

Failed ❌! Hmm 🤔 sounds like we have to adjust our `HelperConfig.s.sol`, hopefully it's straight forward:

We need:

- The correct `chainid` from [ChainList](https://chainlist.org/)
- We need the correct price feed address from [ChainLink](https://docs.chain.link/data-feeds/price-feeds/addresses)

In `HelperConfig.s.sol` our `constructor()` is now:

```solidity
constructor() {
    if (block.chainid == 11155111) {
        activeNetworkConfig = getSepoliaEthConfig();
    } else if (block.chainid == 11155420) {
        activeNetworkConfig = getOptimismSepoliaConfig();
    } else {
        activeNetworkConfig = getAnvilEthConfig();
    }
}
```

Our new function:

```solidity
    function getOptimismSepoliaConfig()
        public
        pure
        returns (NetworkConfig memory)
    {
        NetworkConfig memory opSepoliaConfig = NetworkConfig({
            priceFeed: 0x61Ec26aA57019C486B10502285c5A3D4A4750AD7
        });
        return opSepoliaConfig;
    }
```

## Back to our mock

We still have the problem where if we try to test in our Anvil blockchain, our test will fail because there is no price feed contract in Anvil.

We need our `getAnvilEthConfig` function in `HelperConfig` to deploy a mock contract and return the mock address.

First of all, we need to make sure we import `Script.sol` in the `HelperConfig.s.sol` contract. Also, `HelperConfig` needs to inherit from `Script`. This will give us access to the `vm.startBroadcast`/`vm.stopBroadcast` functionality. We will use this to deploy our mocks.

```solidity
import {Script} from "forge-std/Script.sol";

contract HelperConfig is Script {

}
```

In the `test` folder create a new folder called `mocks`. Inside `mocks` create a new file `MockV3Aggregator.sol`.

Creating a mock price feed from scratch is not easy so copy this content into your file:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import {AggregatorV3Interface} from "@chainlink/contracts/src/v0.8/interfaces/AggregatorV3Interface.sol";

/**
 * @title MockV3Aggregator
 * @notice Based on the FluxAggregator contract
 * @notice Use this contract when you need to test
 * other contract's ability to read data from an
 * aggregator contract, but how the aggregator got
 * its answer is unimportant
 */
contract MockV3Aggregator is AggregatorV3Interface {
    uint256 public constant version = 4;

    uint8 public decimals;
    int256 public latestAnswer;
    uint256 public latestTimestamp;
    uint256 public latestRound;

    mapping(uint256 => int256) public getAnswer;
    mapping(uint256 => uint256) public getTimestamp;
    mapping(uint256 => uint256) private getStartedAt;

    constructor(uint8 _decimals, int256 _initialAnswer) {
        decimals = _decimals;
        updateAnswer(_initialAnswer);
    }

    function updateAnswer(int256 _answer) public {
        latestAnswer = _answer;
        latestTimestamp = block.timestamp;
        latestRound++;
        getAnswer[latestRound] = _answer;
        getTimestamp[latestRound] = block.timestamp;
        getStartedAt[latestRound] = block.timestamp;
    }

    function updateRoundData(
        uint80 _roundId,
        int256 _answer,
        uint256 _timestamp,
        uint256 _startedAt
    ) public {
        latestRound = _roundId;
        latestAnswer = _answer;
        latestTimestamp = _timestamp;
        getAnswer[latestRound] = _answer;
        getTimestamp[latestRound] = _timestamp;
        getStartedAt[latestRound] = _startedAt;
    }

    function getRoundData(
        uint80 _roundId
    )
        external
        view
        returns (
            uint80 roundId,
            int256 answer,
            uint256 startedAt,
            uint256 updatedAt,
            uint80 answeredInRound
        )
    {
        return (
            _roundId,
            getAnswer[_roundId],
            getStartedAt[_roundId],
            getTimestamp[_roundId],
            _roundId
        );
    }

    function latestRoundData()
        external
        view
        returns (
            uint80 roundId,
            int256 answer,
            uint256 startedAt,
            uint256 updatedAt,
            uint80 answeredInRound
        )
    {
        return (
            uint80(latestRound),
            getAnswer[latestRound],
            getStartedAt[latestRound],
            getTimestamp[latestRound],
            uint80(latestRound)
        );
    }

    function description() external pure returns (string memory) {
        return "v0.6/test/mock/MockV3Aggregator.sol";
    }
}
```

Now let's import this in our `HelperConfig.s.sol` file and deploy it in our `getAnvilEthConfig` function, then return the address.

If everything went well, run `forge test` and you will see your tests passing. Awesome 👏👏

## ❓ Questions and 💪 Exercises

- Question ❓: What is this AggregatorV3Interface.sol file? What exactly does it do?
- Question ❓: What are the differences between using a `require` and a `revert` in our `onlyOwner()` modifier?
- Question ❓: You added some console.logs to your tests, you run `forge test` and you don't see them. What is missing?

## 🛠️ Links and Resources

- [Course Repo](https://github.com/Cyfrin/foundry-fund-me-cu)
- [FunWithStorage Contract](https://github.com/Cyfrin/foundry-fund-me-cu/blob/main/src/exampleContracts/FunWithStorage.sol)
- [Forge install](https://book.getfoundry.sh/reference/cli/forge/install)
- [Forge test](https://book.getfoundry.sh/reference/cli/forge/test)
- [Forge console.log](https://book.getfoundry.sh/reference/forge-std/console-log)
- [Forge coverage](https://book.getfoundry.sh/reference/forge/forge-coverage)
