# Section 1. Foundry Simple Storage

## Dev Environment - Windows

For Windows, it all starts with running `wsl --install` on Powershell.

## Why Foundry?

Foundry Pros:

- It **leverages Rust** for compilation, offering significantly faster build times compared to frameworks like Hardhat or Brownie.
- It's entirely **Solidity-based**, eliminating the need to learn other programming languages

## Foundry Setup

Once WSL and VS Code are installed and everything is working let's install Foundry with:

`curl -L https://foundry.paradigm.xyz | bash`

Now let's run `foundryup`.

This will install four components: forge, cast, anvil, and chisel. To confirm the successful installation, run `forge --version`, `cast --version`, `anvil --version`, `chisel --version`.

## New Foundry Project

Run `mkdir foundry-simple-storage` and `cd foundry-simple-storage`.

To start a new project, run `forge init` inside the empty directory.

Keep in mind that by default `forge init` expects an empty folder. If your folder is not empty you must run `forge init --force .`

## Understanding Foundry tree structure

- `lib` is the folder where all your dependencies are installed. The default template comes with one dependency installed: Forge Standard Library. This is the preferred testing library used for Foundry projects.
- `script` is a folder that houses all your scripts
- `src` is the folder where you put all your smart contracts
- `test` is the folder that houses all your tests
- `foundry.toml` - gives configuration parameters for Foundry

- Run `forge build` to build the project
- Run `forge test` to run the tests

You'll notice that two new directories have popped up: `out` and `cache`.

The `out` directory contains your contract artifact, such as the ABI, while the `cache` is used by forge to only recompile what is necessary.

## Continue with the exercise

Delete all the `Counter` Solidity files inside `script`, `src` and `test`. Create `src/SimpleStorage.sol` and add the contents from [this](https://github.com/Cyfrin/foundry-simple-storage-cu/blob/main/src/SimpleStorage.sol) file.

## `compile` and `build`

Difference between `forge compile` and `forge build` commands.

---

### Key Differences:

| Feature                      | `forge compile`              | `forge build`                        |
| ---------------------------- | ---------------------------- | ------------------------------------ |
| **Focus**                    | Basic compilation            | Comprehensive build process          |
| **Includes Pre-Build Steps** | No                           | Yes (if specified in `foundry.toml`) |
| **Performance**              | Faster, minimal              | Potentially slower, complete         |
| **Use Case**                 | Development and quick checks | Deployment and final builds          |

### Recommendation:

- Use `forge compile` for quick iterative development.
- Use `forge build` when preparing for deployment, running a full suite of tests, or ensuring all configurations and optimizations are included.

## Deploy a smart contract locally using Anvil

Anvil is a local testnet node shipped with Foundry. You can use it for testing your contracts from frontends or for interacting over RPC.

To run Anvil you simply have to type `anvil` in the terminal.

This testnet node always listens on `127.0.0.1:8545` this will be our `RPC_URL` parameter when we deploy smart contracts here. More on this later!

//TODO

- Read more on [Anvil](https://book.getfoundry.sh/reference/anvil/)

## Links

- [Foundry Docs](https://book.getfoundry.sh/)
- [Course Repo](https://github.com/Cyfrin/Updraft/tree/main/courses/foundry)
- [VS Code Key Bindings](https://code.visualstudio.com/docs/getstarted/keybindings)
- [FreeCodeCamp - VS Code Crash Course](https://www.youtube.com/watch?v=WPqXP_kLzpo)
- [The Linux command line for beginners](https://ubuntu.com/tutorials/command-line-for-beginners#1-overview)

Extensions

- [Solidity by Nomic Foundation](https://marketplace.visualstudio.com/items?itemName=NomicFoundation.hardhat-solidity)
- [Even Better TOML](https://marketplace.visualstudio.com/items?itemName=tamasfe.even-better-toml)
- [Inline Bookmarks](https://marketplace.visualstudio.com/items?itemName=tintinweb.vscode-inline-bookmarks)
