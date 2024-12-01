# Section 1. Foundry Simple Storage

## Dev Environment - Windows

For Windows, it all starts with running `wsl --install` on Powershell.

## Foundry Setup

Once WSL and VS Code are installed and everything is working let's install Foundry with:

`curl -L https://foundry.paradigm.xyz | bash`

Now let's run `foundryup`.

This will install four components: forge, cast, anvil, and chisel. To confirm the successful installation, run `forge --version`, `cast --version`, `anvil --version`, `chisel --version`.

## New Foundry Project

Run `mkdir foundry-simple-storage` and `cd foundry-simple-storage`.

To start a new project, run `forge init` inside the empty directory.

## Links

- [Foundry Docs](https://book.getfoundry.sh/)
- [Course Repo](https://github.com/Cyfrin/Updraft/tree/main/courses/foundry)
- [VS Code Key Bindings](https://code.visualstudio.com/docs/getstarted/keybindings)
- [FreeCodeCamp - VS Code Crash Course](https://www.youtube.com/watch?v=WPqXP_kLzpo)
