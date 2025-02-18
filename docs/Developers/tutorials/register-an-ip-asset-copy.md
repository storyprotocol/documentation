---
title: 'Case Study: Registering a Derivative of Ippy'
excerpt: >-
  A Case Study showing how PiPi, a generative pfp project, registers derivatives
  of Story's official Ippy mascot.
deprecated: false
hidden: true
metadata:
  image: >-
    https://files.readme.io/5bdd7e2accd0851b1462987c369c350000cb690ae901b4dc9002da02ea0808e3-Screenshot_2025-02-18_at_9.56.54_AM.png
  robots: index
---
[PiPi](https://pfp3.io/pipi/mint) is a free generative pfp project on Story that lets you mint derivative artworks of [Ippy](https://explorer.story.foundation/ipa/0xB1D831271A68Db5c18c8F0B69327446f7C8D0A42), Story's official mascot. Ippy has [Non-Commercial Social Remixing (NCSR)](https://docs.story.foundation/docs/pil-flavors#flavor-1-non-commercial-social-remixing) terms attached, which means anyone can use it or create derivative works as long as it's not used commercially and proper attribution is shown.

When a PiPi is linked as a derivative of Ippy, it automatically inherits the same license terms (NCSR) and is linked in its ancestry graph, which you can see directly on our explorer.

<Image align="center" border={false} caption="In the bottom right, you can see Ippy is the root IP of this PiPi." src="https://files.readme.io/0d7c3ca3a88a9906dac899d6a776417f1c01a07cfa01d9f732974260fd9ea469-Screenshot_2025-02-18_at_10.03.13_AM.png" />

In the following tutorial, you will learn how exactly these PiPi images were properly registered as derivatives of the official Ippy IP.

### :warning: Prerequisites

There are a few steps you have to complete before you can start the tutorial.

1. Complete the [Setup Your Own Project](doc:sc-setup)

## 1) Setup Metadata

Before we register our new PiPi IP, we need to set up its metadata. There are two types of metadata:

1. NFT Metadata
2. IP Metadata

<Cards columns={4}>
  <Card title="NFT vs. IP Metadata" href="https://docs.story.foundation/docs/ip-asset#nft-vs-ip-metadata" icon="fa-home" target="_blank">
    Learn how to properly set up NFT and IP Metadata.
  </Card>
</Cards>

Using [this PiPi](https://explorer.story.foundation/ipa/0xBB42BF2713ee736284C45B1b549a03625cc97e51) as an example, here is the NFT & IP metadata that was set:

> 📘 View Yourself
>
> You can also see these yourself by going to the [explorer](https://explorer.story.foundation/ipa/0xBB42BF2713ee736284C45B1b549a03625cc97e51) and clicking "NFT Details > NFT Metadata URI > View Metadata" and "IP Details > IP Metadata URI > View Metadata"

```json NFT Metadata
{
  "name": "PiPi NFT #1103",
  "image": "https://ipfs.io/ipfs/bafybeigsv4cgacndijwy6b7qhxbseonrybrcpbh47zrlm64gsjm4mlpb2q/nft_1103.jpeg",
  "attributes": [
    {
      "trait_type": "Bg",
      "value": "Orange"
    },
    {
      "trait_type": "Body",
      "value": "Pink"
    },
    {
      "trait_type": "Eyes",
      "value": "Cute"
    },
    {
      "trait_type": "Cloth",
      "value": "Blue"
    },
    {
      "trait_type": "Glasses",
      "value": "Neo"
    },
    {
      "trait_type": "Hat",
      "value": "Duck"
    }
  ],
  "title": "PiPi NFT",
  "ipType": "NFT",
  "description": "Pipi - The first Derivative IP Asset NFT collection on Story Protocol. Limited 2222 generative PFPs inspired by the Ippy, official Story mascot.",
  "creators": [
    {
      "name": "PFP3",
      "address": "0xF91510A17392Be6B3b6F620427051168A1e56A72",
      "description": "PFP Generator",
      "image": "https://utfs.io/f/XyGBmmuHQK18FodS0WDuqCo1LVerXR7sgm8vJnESazWcM5yB",
      "socialMedia": [
        {
          "platform": "twitter",
          "url": "https://x.com/pfp3_"
        },
        {
          "platform": "website",
          "url": "https://pfp3.io"
        },
        {
          "platform": "discord",
          "url": "https://discord.gg/pfp3"
        }
      ],
      "role": "creator",
      "contributionPercent": 100
    }
  ],
  "tags": [
    "PiPi",
    "Derivative IPA",
    "NFT",
    "PF3",
    "PFP"
  ]
}
```
```json IP Metadata
{
  "name": "PiPi NFT #1103",
  "image": "https://ipfs.io/ipfs/bafybeigsv4cgacndijwy6b7qhxbseonrybrcpbh47zrlm64gsjm4mlpb2q/nft_1103.jpeg",
  "attributes": [
    {
      "trait_type": "Bg",
      "value": "Orange"
    },
    {
      "trait_type": "Body",
      "value": "Pink"
    },
    {
      "trait_type": "Eyes",
      "value": "Cute"
    },
    {
      "trait_type": "Cloth",
      "value": "Blue"
    },
    {
      "trait_type": "Glasses",
      "value": "Neo"
    },
    {
      "trait_type": "Hat",
      "value": "Duck"
    }
  ],
  "title": "PiPi NFT",
  "ipType": "NFT",
  "description": "Pipi - The first Derivative IP Asset NFT collection on Story Protocol. Limited 2222 generative PFPs inspired by the Ippy, official Story mascot.",
  "creators": [
    {
      "name": "PFP3",
      "address": "0xF91510A17392Be6B3b6F620427051168A1e56A72",
      "description": "PFP Generator",
      "image": "https://utfs.io/f/XyGBmmuHQK18FodS0WDuqCo1LVerXR7sgm8vJnESazWcM5yB",
      "socialMedia": [
        {
          "platform": "twitter",
          "url": "https://x.com/pfp3_"
        },
        {
          "platform": "website",
          "url": "https://pfp3.io"
        },
        {
          "platform": "discord",
          "url": "https://discord.gg/pfp3"
        }
      ],
      "role": "creator",
      "contributionPercent": 100
    }
  ],
  "tags": [
    "PiPi",
    "Derivative IPA",
    "NFT",
    "PF3",
    "PFP"
  ]
}
```

Once you have metadata written, you can upload them to IPFS and will later set it when minting our NFT.

## 2. Minting an NFT

When you want to register an IP on Story, you must first mint an NFT. This NFT represents the **ownership** over the [🧩 IP Asset](doc:ip-asset).

Here is part of the `_mintNFT` function in the `PiPi.sol` contract:

```sol PiPi.sol
contract PiPi is ERC721, Ownable, IERC721Receiver {

  // ... some code here ...

  function whitelistMint() external payable returns (string memory, address) {
    require(whitelistMintEnabled, "Whitelist mint is not active");
    require(whitelist[msg.sender], "Address not whitelisted");
    require(mintedCount[msg.sender] < WHITELIST_MAX_P_WALLET, "Whitelist mint limit reached");
    require(_totalSupply < MAX_SUPPLY, "Max supply reached");

    return _mintNFT(msg.sender);
  }

  function _mintNFT(address recipient) internal returns (string memory, address) {
    uint256 newTokenId = _totalSupply + 1;
    _safeMint(address(this), newTokenId);

    address ipId = _registerAsIPAsset(newTokenId);

    string memory nftUri = tokenURI(newTokenId);
    bytes32 metadataHash = keccak256(abi.encodePacked(nftUri));
    CORE_METADATA_MODULE.setAll(ipId, nftUri, metadataHash, metadataHash);

    registerDerivativeForToken(ipId);

    _safeTransfer(address(this), recipient, newTokenId, "");

    // ... more code here

    return (nftUri, ipId);
  }
}
```

As you can see, the user calls `whitelistMint` which then calls `_mintNFT` after checking if the user is on a whitelist. On line 16, we are then minting a new NFT to the contract.

> 📘 Why do we mint an NFT to the contract and not the user?
>
> We later have to register the IP as a derivative of Ippy. Only the owner (the address holding the NFT) can register an IP as a derivative of another. So, we will mint the NFT to the contract => contract registers NFT as IP and then later as a derivative of Ippy => transfer NFT to the user.

## 3. Registering NFT as IP

Once we have minted a new NFT, we can register it as IP. On line 18 above, it calls a `_registerAsIPAsset` function:

```sol PiPi.sol
function _registerAsIPAsset(uint256 tokenId) internal returns (address) {
  try IP_ASSET_REGISTRY.register(block.chainid, address(this), tokenId) returns (address ipId) {
    require(ipId != address(0), "IP Asset registration failed");
    return ipId;
  } catch Error(string memory reason) {
    revert(reason);
  } catch {
    revert("IP Asset registration failed");
  }
}
```

All this is doing is calling the `register` function on the [IP Asset Registry](doc:ip-asset-registry), which creates a new [🧩 IP Asset](doc:ip-asset) in our protocol, and returning a `ipId`.

## 4. Set Metadata on IP

Now that we have registered a new IP Asset, we can take our metadata from before and set it on the NFT & IP with the `CoreMetadataModule.sol`. As described [here](https://docs.story.foundation/docs/ip-asset#adding-nft--ip-metadata-to-ip-asset), we need to set 4 params:

1. `nftMetadataHash`
2. `nftMetadataURI`
3. `ipMetadataHash`
4. `ipMetadataURI`

```sol PiPi.sol
// handles the NFT's `nftMetadataHash`
// handles the IP's `ipMetadataURI` and `ipMetadataHash`
function _mintNFT(address recipient) internal returns (string memory, address) {
  
  // ... some code here ...

  string memory nftUri = tokenURI(newTokenId);
  bytes32 metadataHash = keccak256(abi.encodePacked(nftUri));
  CORE_METADATA_MODULE.setAll(ipId, nftUri, metadataHash, metadataHash);
  
  // ... some code here ...

}

// handles the NFT's `nftMetadataURI`
function tokenURI(uint256 tokenId) public view override returns (string memory) {
  return string(abi.encodePacked(_baseUri, StringUtils.uint2str(tokenId), ".json"));
}
```

Note that the NFT's `tokenUri` is set through

## 5. Register as Derivative