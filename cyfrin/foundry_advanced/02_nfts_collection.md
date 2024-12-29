# Develop an NFTs Collection

## Introduction

2 implementations:

- 1st, basic, collection of puppies stored in IPFS.
- 2nd, art will be stored on-chain, dynamic, changing on a criteria we set.

## What is an NFT

At their core, NFTs are tokens which exist on a smart contract platform, they can be viewed, bought and sold on marketplaces such as **[OpenSea](https://opensea.io/)** and **[Rarible](https://rarible.com/)**.

### ERC 721 Standard

NFTs, and the ERC721 Token Standard, differ from ERC20s in a few fundamental ways.

ERC20s handle ownership via a simple mapping of a uint256 token balance to an address.

ERC721s, by contrast, each have a unique tokenId, these tokenIds are mapped to a user's address. In addition to a tokenId, ERC721s include a tokenUri.

For art use cases of NFTs, people used to include all the metadata (like the image of the NFT) on the blockchain which was extremely expensive. So a tokenUri was added to the standard so that you could just put the URL string of the associated image and some other metadata on the blockchain, this obviously becomes a very centralized point of failure.

Often a protocol will use a service like **[IPFS](https://ipfs.tech/)** to store their images and metadata in a more decentralized way.

## Questions and Exercises

## Links

- [Repo](https://github.com/Cyfrin/foundry-nft-cu)

- [ERC-721: NFT Standard](https://eips.ethereum.org/EIPS/eip-721)
- [ERC-1155: Multi Token Standard](https://eips.ethereum.org/EIPS/eip-1155)
- [IPFS](https://ipfs.tech/)
