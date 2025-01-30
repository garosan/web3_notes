## [Ethereum Accounts](https://ethereum.org/en/developers/docs/accounts/)

Accounts can be user-controlled or deployed as smart contracts, or:

- Externally-owned account (EOA)
- Contract account

Both account types can:

- Receive, hold and send ETH and tokens
- Interact with deployed smart contracts

Ethereum accounts have four fields:

- nonce: sequential counter that tracks the number of transactions sent from that account. This prevents replay attacks and ensures that transactions are executed in the correct order.
- balance: The number of wei owned by this address.
- codeHash: This hash refers to the code of an account on the Ethereum virtual machine (EVM). Contract accounts have code fragments programmed in that can perform different operations. This EVM code gets executed if the account gets a message call. It cannot be changed, unlike the other account fields. All such code fragments are contained in the state database under their corresponding hashes for later retrieval. This hash value is known as a codeHash. For externally owned accounts, the codeHash field is the hash of an empty string.
- storageRoot: Sometimes known as a storage hash. A 256-bit hash of the root node of a Merkle Patricia trie that encodes the storage contents of the account (a mapping between 256-bit integer values), encoded into the trie as a mapping from the Keccak 256-bit hash of the 256-bit integer keys to the RLP-encoded 256-bit integer values. This trie encodes the hash of the storage contents of this account, and is empty by default.

## [Gas and Fees](https://ethereum.org/en/developers/docs/gas/)
