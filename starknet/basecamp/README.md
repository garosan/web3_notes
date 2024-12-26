# Basecamp X Edition

- [Course Link](https://www.starknet.io/tutorial-type/basecamp-x/)

- Why Cairo?
- Why Starknet?
- Smart Wallets

- Starkware is the company that developed all the products and protocols. For example, StarkEx (a permissioned L2 validium with limited functions, used by dYdX and others). StarkNet is the permisionless L2 rollup. StarkNet Foundation is the non-profit handling all the grants, marketing, etc. its mission is to grow the StarkNet ecosystem.

- As of now (oct. 2024) you can pay gas fees with ETH and Stark token.

- Why Cairo? Cairo helps provide proof that the result of a computation or execution of a program is tamper-proof, the code has not been tampered with, even if you don't have access to compute the program yourself. _A regular computer is able to keep a supercomputer honest_.

- Cairo is a general-purpose knowledge. Eli Ben-Sasson published the paper in 2018, then found blockchain to be a good place to put the tech to use. Cairo has use cases outside of blockchain.

▶️ Cairo's features:

- Enables verifiable computation
- Runs on top of the CairoVM
- Syntax inspired by Rust
- Similar ownership model
- Strongly typed, traits, macros
- Can be used outside of Starknet
- No need to know ZK

Validity Proof > Computation as Zip > File Size. In other words, Cairo is compression for computation.

▶️ Validity Proofs (VP):

- A use case of ZK Proofs
- Convinces Verifier on L1 of new L2 state without sharing txs details
- It doesn't make L2 txs private
- It makes verification efficient
- Based on STARKs, not SNARKs

More technical details: STARK proofs are bigger (proof size is 400kb vs 288 bytes for SNARKs) but proving time is 1 to 10 and they're working on making it 1 to 100, STARKs do not require a trusted setup and are quantum resistant while for SNARKs the opposite is true.

No me convenció de que sea quantum resistant haha.

▶️ What is a smart wallet?

A wallet that allows you to:

- Sign txs with TouchID or FaceID
- Execute multiple operations in a single tx
- On SN every wallet is a smart wallet
- There are no EOAs, it's built using Account Abstraction
- Providers: Argent, Braavos

When you do some operations like swapping on a DEX, that normally requires you to first approve the transaction and then do the actual swap. 2 different txns. These can be sent in a different order to the blockchain (1st the swap, then the approve), or even in different blocks, opening the gate for MEV searchers, sandwich attacks, front-running, you name it.

Starknet has multicall built-in, which allows to bundle both transactions and send them as one unit to the blockchain.

▶️ Account Abstraction

Decoupling of Signer and Account

Signer:

- Device that signs txs
- Identifies user with PK

Account:

- Smart contract on Starknet
- Holds the assets
- Verifies signatures
- Executes txs with multicall

For every wallet that you have, there's a smart contract (account contract) in Starknet connected to your wallet to allow this to work. The standard is called SNIP-6. That account contract would then interact with whatever contract you're trying to interact.

▶️ Signature Verification

Enables features like:

- Single signer
- Multisig
- Hardware signer
- Social recovery
- Session keys

Supported Elliptic Curves:

- STARK-friendly (native)
- Secp256k1 (Ethereum)
- Secp256r1 (standard)

Most interesting for me: **AA + Secure Enclave: You can turn your phone into a hardware wallet**.

Secure enclave = TEE: Trusted Execution Environment.

[TEE](https://en.wikipedia.org/wiki/Trusted_execution_environment) is the _protocol_, Secure Enclave is Apple's implementation product.

## Session 2

[Link](https://www.youtube.com/watch?v=6QgMr4sJBds)

Topics:

- Architecture 101
- Transactions
- Finality
- Full Nodes
- Recursive Proving

▶️ Cairo

Cairo source file > Sierra > Safe CASM > Validity Proof

- Cairo compiles to Sierra (Safe Intermediate Representation)
- The sequencer compiles Sierra to _Safe CASM_ (Cairo Assembly)
- The Safe CASM generates the Validity Proof
- Failed txs are included in blocks
- Sequencer always gets compensated

Sierra was introduced to eliminate the DoS vector in CairoZero (legacy version of Cairo).
