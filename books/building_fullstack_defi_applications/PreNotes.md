# Preface

[Github](https://github.com/PacktPublishing/Building-Full-stack-DeFi-Application)

# Ch1. Introduction to DeFi

- Objectives 👉
  - Get an overview of DeFi and understand its main concepts
  - Learn what people can do with several types of DeFi applications
  - Understand the architecture of DeFi applications
  - Discover the potential vulnerabilities in DeFi applications and their solutions

Temas cubiertos pero que nos vamos a saltar

- What is DeFi?
- Characteristics of DeFi
  - Decentralization
  - Transparency
  - Open
  - Non-custodial
  - Anonymous
  - Zero downtime
- Understanding terminologies

  - ERC-20
  - Liquidity
  - Liquidity pool
  - Swap
  - Oracle
  - Total value locked (TVL)
  - Annual percentage yield (APY) and annual percentage rate (APR)

  Most public DeFi projects provide a public API for people to access TVL
  DefiLlama (https://defillama.com/) is one such platform that uses a public API to collect data from DeFi projects.

- Overview of DeFi applications

  - Decentralized exchanges
  - Automated market maker (AMM, a type of DEX)
  - Lending and borrowing platform (should have at least 4 ops: Deposit, Borrow, Repay, and Withdraw)
  - Staking, yield farming, and liquidity mining
  - More that are not covered in this book: https://101blockchains.com/decentralized-finance-applications/

## Architecture of DeFi applications

![DeFi App Architecture](https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781837634118/files/image/B19739_01_7.jpg)

Components of the App:

- DeFi application frontend
- DeFi wallet
- RPC endpoint
- Interactions between blockchain and oracle

## Vulnerabilities of DeFi applications

- Reentrancy
- Self-destruct operation
- Gas overflow
- Random number manipulation

### Reentrancy

A reentrancy attack occurs when a function makes an external call to another untrusted contract. Then, the untrusted contract makes a recursive call back to the original function in an attempt to drain funds.

- [SolidityByExample - Reentrancy](https://solidity-by-example.org/hacks/re-entrancy/)

To prevent a reentrancy attack, we will use ReentrancyGuard from OpenZeppelin.

### Self-destruct operation

After the DAO debacle, in 2016, Ethereum introduced the selfdestruct function to serve as an exit door for smart contracts in case of an attack. Here is an example of how to use the selfdestruct function:

```solidity
contract SelfdestructExample {
  function killContract(address payable receiver) external {
    selfdestruct(receiver);
  }
}
```

Este código de arriba es muy alto nivel, no menciona nada sobre los detalles de implementación de la función. El punto es que obviamente si tu contrato tiene una 'backdoor' que puedes activar para destruir el contrato y mandar fondos a una address `receiver` que tu pongas como parámetro cuando mandes llamar `killContract`, si alguien encuentra la manera de hackearte esa función, pues con que pongan su address como parámetro `receiver` les va a caer todo el dinero a ellos.

- [SolidityByExample - Self Destruct](https://solidity-by-example.org/hacks/self-destruct/)

### Gas overflow

Este tiene qué ver con la cantidad de gas en ciertas transacciones que es indeterminada debido a la naturaleza de Solidity y Ethereum. Tratar de no usar loops que puedan convertirse en infinitas o arreglos dinámicos que no sabemos de qué tamaño se van a volver.

- [Trucos básicos for gas optimization](https://dev.to/jamiescript/gas-saving-techniques-in-solidity-324c)

### Random number manipulation

If you want to implement a random number generator with the facilities in EVM, you can use code similar to the following:

```solidity
/*
 * Returns a random number.
 * If the caller of the function gets a random number
 * that can be divided by 10000, then the caller will win.
 */
function getRandomNumber() private view returns (uint256) {
  return uint256(keccak256(abi.encodePacked(block.timestamp, msg.sender)));
}
```

Check `block.timestamp`. Because the timestamp is set by the miner, a hacker can set the block.timestamp function of the next block by being a miner to generate a random number that makes them win.

To get a true random number, we can use an oracle service such as Chainlink VRF. It relies on many nodes being on the network to generate a random number that is secure and almost impossible to manipulate by hackers.

- [Learn more about VRF](https://docs.chain.link/vrf/v2/subscription/examples/get-a-random-number/)

There are many more types of vulnerabilities in DeFi. We will discuss this in more detail when we build DeFi applications later in this book.

Read more:

- [Chainlink - Hybrid smart contracts](https://chain.link/education-hub/hybrid-smart-contracts)
- [What Is a Blockchain Oracle?](https://chain.link/education/blockchain-oracles)
- [PancakeSwap - Yield farms](https://docs.pancakeswap.finance/products/yield-farming)
- [BNB Chain Hack](https://www.coindesk.com/business/2022/10/06/binance-linked-bnb-price-falls-close-to-4-on-hack-rumors)

# Ch2. Getting Started with DeFi Application Development

you will learn how to create a full stack DeFi project with Node.js, React.js, and Hardhat. We will also guide you through the process of developing, building, deploying, debugging, and testing smart contracts with these tools.

## Creating a DeFi project

Installing Node.js
Creating a project with create-react-app
Installing and configuring Hardhat

Entonces, crear una React app (yo con Vite) y en el root folder correr:

```shell
npm install --save-dev hardhat
rm README.md
npx hardhat init
```

Options to select:

```shell
👷 Welcome to Hardhat v2.12.6 👷‍
✔ What do you want to do? · Create a JavaScript project
✔ Hardhat project root: · /Users/mymac/git/Building-Full-stack-DeFi-Application/defi-apps
✔ Do you want to add a .gitignore? (Y/n) · y
✔ Do you want to install this sample project's dependencies with npm (@nomicfoundation/hardhat-toolbox)? (Y/n) · y
```

After running npx hardhat, a new Hardhat configuration file, hardhat.config.js, is created in the project directory. We need to open this file and add the following highlighted lines to define four paths:

```javascript
require("@nomicfoundation/hardhat-toolbox");
/** @type import('hardhat/config').HardhatUserConfig */
module.exports = {
  solidity: "0.8.17",
  paths: {
    sources: "./src/backend/contracts",
    artifacts: "./src/backend/artifacts",
    cache: "./src/backend/cache",
    tests: "./src/backend/test",
  },
};
```

Then create the path folders

`$ mkdir -p src/backend/{contracts,artifacts,cache,test}`

## Writing, compiling, and deploying a smart contract in a local environment

Before writing a smart contract, we need to install the OpenZeppelin

`npm install @openzeppelin/contracts`

### My own tests, I will be creating with Vite

npm create vite@latest . -- --template react

Then:

npm install
npm run dev

npm install --save-dev hardhat
rm README.md

............ bunch of stuff

Then:

npx hardhat node


npx hardhat run scripts/deploy.cjs --network localhost

### Deploying a smart contract on Testnet

Remember to npm i dotenv

npx hardhat run scripts/deploy.cjs --network sepolia


https://sepolia.etherscan.io/address/0x5B2aE816E92bE1b8022dC13eAB9fd77e3232e9C8


npx hardhat run scripts/deploy.cjs --network baseMain
npx hardhat run scripts/deploy.cjs --network arbitrumMain



const contract = await ethers.getContractAt("SimpleDeFiToken", "0x5fbdb2315678afecb367f032d93f642f64180aa3")


## 28-NOV-2024

Mismo ejercicio pero usando create-react-app

npx hardhat run scripts/deploy.js --network localhost

npx hardhat run scripts/deploy.js --network ethereumSepolia
npx hardhat run scripts/deploy.js --network arbitrumSepolia
npx hardhat run scripts/deploy.js --network baseSepolia


https://ethereum.stackexchange.com/questions/158885/error-hh19-your-project-is-an-esm-project-you-have-type-module-set-in-you


Hay qué Downgrading "chai" from 5.00 to 4.3.7!!!!