# Development with Foundry

- [Tutorial: Setting up Foundry with Base](https://docs.base.org/tutorials/intro-to-foundry-setup/)

In order to work with Base, you need to configure a couple of settings in the configuration `foundry.toml` file.

1st thing is the Solidity version, you need to configure your config file as follows:

```toml
[profile.default]
src = 'src'
out = 'out'
libs = ['lib']
solc_version = "0.8.23"
```

run `forge build` to ensure everything works well.

Also configure the JSON RPC endpoint for Base and the API key for Basescan, check the link for instructions.

- [Tutorial: Testing smart contracts with Foundry](https://docs.base.org/tutorials/intro-to-foundry-testing/)

All the info here is also covered by Cyfrin and basically by these 2 Foundry docs:

- [Cheatcodes](https://book.getfoundry.sh/forge/cheatcodes)
- [Writing Tests](https://book.getfoundry.sh/forge/writing-tests)

Foundry includes a set of cheatcodes, which are special instructions that are accessible using the vm instance in your tests.

With cheatcodes you can:

- Manipulate the state of the blockchain
- Test reverts
- Test events
- Change block number
- Change identity
- And more!
