# Lesson 13 - Syntax and structure

An interface, a screen, a button, a form should only improve the usability with ease of use for the end-user. It should not add new functionality, it should only be there to make the interaction easier.

Matheus is showing that web apps are interfaces for users to interact in the browser with backends or smart contracts. They are made using HTML, CSS and JavaScript, he gives a small demo of:

- How you can change the content of a page in your dev tools but this will only show locally
- How you can add interactivity to a page by manipulating its DOM using JavaScript
- You can also use JavaScript to make API calls
- He tries to call Etherscan API to display latest block number but he's missing the API key

Sometimes for creating a simple application, writing the contracts will take us 1hr and writing the frontend will take us 20+ hrs which we just don't have for a quick and raw prototype. That's the perfect use case for heavily opinionated frameworks such as scaffold-ETH-2.

So we will all together jump from React and Next to a framework called Scaffold-ETH-2 that contains all that we need to build our web3 dapp: It comes with Next's capabilities plus hooks for integration with Wagmi, beautiful styling out of the box, etc.

The whole tech stack:

- Hardhat or Foundry (user's choice)
- Wagmi for React Hooks
- Viem as low-level interface that provides primitives to interact with Ethereum. The alternative to ethers.js and web3.js.
- NextJS for building a frontend
- RainbowKit for adding wallet connection
- DaisyUI for pre-built Tailwind CSS components

Current installation:

`npx create-eth@latest`

- Select project name
- Select Hardhat or Foundry

`cd your-project`

And then in 3 different terminals:

```bash
yarn chain
yarn deploy
yarn start
```

Then Matheus gives a quick demo of Scaffold-ETH:

- Using the faucet
- Using `yarn-deploy` and the 'Debug contracts' section
- The mini block explorer that you have out of the box with scaffold-eth

Finally, Matheus gives a small example of how changing the `app/page.tsx` markup is used to change the content of the frontend itself.

## ❓ Questions and 💪 Exercises

## 🛠️ Links and Resources

- [Notes](https://github.com/Encode-Club-Solidity-Bootcamp/Lesson-13)
- [Video](https://www.youtube.com/watch?v=jfVj-ui8nSY)
- [Scaffold-ETH-2](https://docs.scaffoldeth.io/)
