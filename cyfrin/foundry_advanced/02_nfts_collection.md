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

## Project Setup

```bash
mkdir nft-project
cd nft-project
forge init
```

- Remove `src/Counter.sol`, `test/Counter.t.sol` and `script/Counter.s.sol`.
- Make sure that your `.gitignore` contains `.env` and `broadcast/`.
- Create `src/BasicNft.sol`.

Install OpenZeppelin contracts:

`forge install OpenZeppelin/openzeppelin-contracts --no-commit`

Add the remapping in `foundry.toml`:

```toml
[profile.default]
src = "src"
out = "out"
libs = ["lib"]
remappings = ["@openzeppelin/contracts=lib/openzeppelin-contracts/contracts"]
```

Import and inherit the ERC721.

### TokenURI

Hard to believe, but when NFTs began, `tokenUri` was an optional parameter, despite being integral to how NFTs are used and consumed today. TokenURI basically is an endpoint that returns the metadata for a given NFT.

Example schema:

```json
{
  "title": "Asset Metadata",
  "type": "object",
  "properties": {
    "name": {
      "type": "string",
      "description": "Identifies the asset to which this NFT represents"
    },
    "description": {
      "type": "string",
      "description": "Describes the asset to which this NFT represents"
    },
    "image": {
      "type": "string",
      "description": "A URI pointing to a resource with mime type image/* representing the asset to which this NFT represents. Consider making any images at a width between 320 and 1080 pixels and aspect ratio between 1.91:1 and 4:5 inclusive."
    }
  }
}
```

As an exercise, we can check the PudgyPenguins contract on Etherscan [here](https://etherscan.io/address/0xbd3531da5cf5857e7cfaa92426877b022e612cf8#readContract).

Go to the `tokenURI` function and enter any valid `tokenId`. This will return a string like:

`ipfs://bafybeibc5sgo2plmjkq2tzmhrn54bk3crhnc23zd2msg4ea7a4pxrkgfna/1000`

To see this in Chrome replace `ipfs://` with `https://ipfs.io/ipfs/`:

`https://ipfs.io/ipfs/bafybeibc5sgo2plmjkq2tzmhrn54bk3crhnc23zd2msg4ea7a4pxrkgfna/1000`.

Enter the above URL in Chrome and you'll see the metadata for this NFT. Once again, convert the image URL that you see:

`https://ipfs.io/ipfs/QmNf1UsmdGaMbpatQ6toXSkzDpizaGmC9zfunCyoz1enD5/penguin/1000.png`

And voilá, you'll see the image for that particular NFT, it should be the same as the one listed on OpenSea.

Both the `tokenUri` and `imageUri` for this PudgyPenguin are stored in IPFS. We will also use IPFS to store our DOGI NFTs.

### Our Code

So anytime someone mints a Doggie NFT, we need to assign a TokenURI to the minted TokenID.

The OZ implementation of ERC721 that we imported has its own virtual tokenURI function which we will override.

Let's add our own `tokenURI` function, our `BasicNft.sol` is looking like this:

```solidity
// SPDX-License-Identifier: MIT

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";

pragma solidity 0.8.26;

contract BasicNft is ERC721 {
    uint256 private s_tokenCounter;

    constructor() ERC721("Doggie", "DOGI") {
        s_tokenCounter = 0;
    }

    function tokenURI(
        uint256 tokenId
    ) public view override returns (string memory) {}
}
```

## Questions and Exercises

Question ❓: What does the `--no-commit` flag do?

## 🛠️ Links and Resources

- [Repo](https://github.com/Cyfrin/foundry-nft-cu)

- [ERC-721: NFT Standard](https://eips.ethereum.org/EIPS/eip-721)
- [ERC-1155: Multi Token Standard](https://eips.ethereum.org/EIPS/eip-1155)
- [IPFS](https://ipfs.tech/)

- [OpenSea](https://opensea.io/)
- [Rarible](https://rarible.com/)
