---
title: How to Register IP on Story
excerpt: Learn how to add IP & NFT metadata to an IP Asset using the Typescript SDK.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
* [Use the SDK](https://docs.story.foundation/docs/how-to-register-ip-on-story#using-the-sdk)
* [Use a Smart Contract](https://docs.story.foundation/docs/how-to-register-ip-on-story#using-a-smart-contract)

# Using the SDK

:warning: This tutorial has moved [here](https://docs.story.foundation/docs/register-an-ip-asset#/).

# Using a Smart Contract

<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/story-protocol-boilerplate/blob/main/test/0_IPARegistrar.t.sol" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    See the completed, working code of how to register your IP Asset.
  </Card>
</Cards>

Let's say you have some off-chain IP (ex. a book, a character, a drawing, etc). In order to register that IP on Story, you first need to mint an NFT to represent that IP, and then register that NFT on Story, turning it into an [🧩 IP Asset](doc:ip-asset). As you can probably tell, this is two transactions: Mint NFT ▶️ Register NFT

So, we will separate this tutorial into two sections:

1. [You already have an NFT and want to register it as IP](https://docs.story.foundation/docs/how-to-register-ip-on-story#you-already-have-an-nft-and-want-to-register-it-as-ip)
2. [You want to mint + register an NFT as IP in the same transaction](https://docs.story.foundation/docs/how-to-register-ip-on-story#you-want-to-mint--register-an-nft-as-ip-in-the-same-transaction)

## You already have an NFT and want to register it as IP

If you already have an NFT minted, or you want to register IP using a custom-built ERC-721 contract, this is the section for you.

As you can see below, the registration process is relatively straightforward. We use `SimpleNFT` as an example, but you can replace it with your own ERC-721 contract.

All you have to do is call `register` on the [IP Asset Registry](doc:ip-asset-registry) with:

* `chainid` - you can simply use `block.chainid`
* `tokenContract` - the address of your NFT collection
* `tokenId` - your NFT's ID

```sol IPARegistrar.sol
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.23;

import { IPAssetRegistry } from "@storyprotocol/core/registries/IPAssetRegistry.sol";
// your own ERC-721 NFT contract
import { SimpleNFT } from "./SimpleNFT.sol";

/// @notice Register an NFT as an IP Account.
contract IPARegistrar {
    IPAssetRegistry public immutable IP_ASSET_REGISTRY;
    SimpleNFT public immutable SIMPLE_NFT;

  	// you can get the addresses for these 
  	// here: https://docs.story.foundation/docs/deployed-smart-contracts
    constructor(address ipAssetRegistry) {
        IP_ASSET_REGISTRY = IPAssetRegistry(ipAssetRegistry);
        // Create a new Simple NFT collection
        SIMPLE_NFT = new SimpleNFT("Simple IP NFT", "SIM");
    }

    /// @notice Mint an IP NFT and register it as an IP Account via Story Protocol core.
    /// @return ipId The address of the IP Account.
    /// @return tokenId The token ID of the IP NFT.
    function mintIp() external returns (address ipId, uint256 tokenId) {
        tokenId = SIMPLE_NFT.mint(msg.sender);
        ipId = IP_ASSET_REGISTRY.register(block.chainid, address(SIMPLE_NFT), tokenId);
    }
}
```

## You want to mint + register an NFT as IP in the same transaction

If you don't have your own NFT contract and want to mint + register in the same step, this is the section for you.

To achieve this, we will be using the [📦 SPG](doc:spg), which is a utility contract that allows us to combine multiple transactions into one. In this case, we'll be using the SPG's `mintAndRegisterIp` function which combines both minting an NFT and registering it as an IP Asset.

In order to use `mintAndRegisterIp`, we first have to create a new `SPGNFT` collection. We can do this simply by calling `createCollection` on the `StoryProtocolGateway` contract. Or, if you want to create your own `SPGNFT` for some reason, you can implement the [ISPGNFT](https://github.com/storyprotocol/protocol-periphery-v1/blob/main/contracts/interfaces/ISPGNFT.sol) contract interface. Follow the example below to see example parameters you can use to initialize a new SPGNFT.

Once you have your own SPGNFT, all you have to do is call `mintAndRegisterIp` with:

* `spgNftContract` - the address of your SPGNFT contract
* `recipient` - the address of who will receive the NFT and thus be the owner of the newly registered IP. *Note: remember that registering IP on Story is permissionless, so you can register an IP for someone else (by paying for the transaction) yet they can still be the owner of that IP Asset.*
* `ipMetadata` - the metadata associated with your NFT & IP. See [this](https://docs.story.foundation/docs/ip-asset#nft-vs-ip-metadata) section to better understand setting NFT & IP metadata.

```sol IPARegistrar.sol
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.23;

import { StoryProtocolGateway } from "@storyprotocol/periphery/StoryProtocolGateway.sol";
import { IStoryProtocolGateway as ISPG } from "@storyprotocol/periphery/interfaces/IStoryProtocolGateway.sol";
import { ISPGNFT } from "@storyprotocol/periphery/interfaces/ISPGNFT.sol";

/// @notice Register an NFT as an IP Account.
contract IPARegistrar {
    StoryProtocolGateway public immutable SPG;
    ISPGNFT public immutable SPG_NFT;

    // you can get the addresses for these 
    // here: https://docs.story.foundation/docs/deployed-smart-contracts
    constructor(address storyProtocolGateway) {
        SPG = StoryProtocolGateway(storyProtocolGateway);
        // Create a new NFT collection via SPG
        SPG_NFT = ISPGNFT(
            SPG.createCollection(
                ISPGNFT.InitParams({
                  name: "Test Collection",
                  symbol: "TEST",
                  baseURI: "https://test-base-uri.com/",
                  maxSupply: 1000,
                  mintFee: 0,
                  mintFeeToken: address(0),
                  mintFeeRecipient: address(this),
                  owner: address(this),
                  mintOpen: true,
                  isPublicMinting: false
                })
            )
        );
    }

  /// @notice Mint an IP NFT and register it as an IP Account via Story Protocol Gateway (periphery).
  /// @dev Requires the collection to be created via SPG (createCollection).
  function spgMintIp() external returns (address ipId, uint256 tokenId) {
        (ipId, tokenId) = SPG.mintAndRegisterIp(
            address(SPG_NFT),
            msg.sender,
            ISPG.IPMetadata({
                ipMetadataURI: "https://ipfs.io/ipfs/QmZHfQdFA2cb3ASdmeGS5K6rZjz65osUddYMURDx21bT73",
                ipMetadataHash: keccak256(
                    abi.encodePacked(
                        "{'title':'My IP Asset','description':'This is a test IP asset','ipType':'','relationships':[],'createdAt':'','watermarkImg':'https://picsum.photos/200','creators':[],'media':[],'attributes':[{'key':'Rarity','value':'Legendary'}],'tags':[]}"
                    )
                ),
                nftMetadataURI: "https://ipfs.io/ipfs/QmRL5PcK66J1mbtTZSw1nwVqrGxt98onStx6LgeHTDbEey",
                nftMetadataHash: keccak256(
                    abi.encodePacked(
                        "{'name':'Test NFT','description':'This is a test NFT','image':'https://picsum.photos/200'}"
                    )
                )
            })
        );
    }
}
```