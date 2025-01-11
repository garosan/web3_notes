# Section 04. Airdrop and Signatures

## Introduction

In this project, we'll delve into **Merkle Trees** and **Signatures** to create our very own _ERC20 airdrop contract_.

An airdrop occurs when a token development team distributes tokens or allows people to claim them. These tokens can be of various types, including ERC-20, ERC-1155, or ERC-721.

Tokens are typically given for free, with eligibility criteria such as contributing to the project's GitHub repository or participating in the community. This process helps to _bootstrap the project_ by distributing tokens to a **list of eligible addresses**.

Let's check the [code base](https://github.com/Cyfrin/foundry-merkle-airdrop-cu). We have a minimal Bagel token.

The [`MerkleAirdrop`](https://github.com/Cyfrin/foundry-merkle-airdrop-cu/blob/main/src/MerkleAirdrop.sol) contract is the cornerstone of this project, and it uses **Merkle Proofs** to verify address eligibility.

It includes a `claim` function that allows addresses to receive the airdrop without paying gas fees. Furthermore, it implements _signatures_ to ensure that only intended recipients can claim the tokens.

We will generate **scripts** to create Merkle Trees, Proofs, and Root Hash, as well as deploy and interact with the contract.

In this course, we will cover:

- Merkle Trees
- Merkle Proofs
- signatures
- ECDSA (Elliptical Curve Digital Signature) algorithm
- Transaction types

Furthermore:

- After initializing a ZKsync local node with Docker, we'll deploy the `Bagel` token and `MerkleAirdrop` contracts on it
- We'll then **sign a message** to allow someone else to call `claim` on your behalf so you can receive the token while not paying for gas fees
- The initial supply of tokens is created and sent to the airdrop contract
- Finally, we can claim tokens on behalf of the claiming address (so they do not have to pay gas) using a signature
- The address will now hold a balance of the airdropped tokens

## 🛠️ Links and Resources

- [GitHub Repo](https://github.com/Cyfrin/foundry-merkle-airdrop-cu)
