---
title: Register an IP Asset
excerpt: Learn how to Register an NFT as an IP Asset in Solidity.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/story-protocol-boilerplate/blob/main/test/0_IPARegistrar.t.sol" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    Follow the completed code all the way through.
  </Card>
</Cards>

Let's say you have some off-chain IP (ex. a book, a character, a drawing, etc). In order to register that IP on Story, you first need to mint an NFT. This NFT is the **ownership** over the IP. Then you **register** that NFT on Story, turning it into an [🧩 IP Asset](doc:ip-asset). The below tutorial will walk you through how to do this.

### :warning: Prerequisites

There are a few steps you have to complete before you can start the tutorial.

1. Complete the [Setup Your Own Project](doc:sc-setup)

## 0/. Before We Start

There are two scenarios:

1. You already have a **custom** ERC-721 NFT contract and can mint from it
2. You want to create an [📦 SPG (Periphery)](doc:spg) NFT contract to do minting for you

## Scenario #1: You already have a custom ERC-721 NFT contract and can mint from it

If you already have an NFT minted, or you want to register IP using a custom-built ERC-721 contract, this is the section for you.

As you can see below, the registration process is relatively straightforward. We use `SimpleNFT` as an example, but you can replace it with your own ERC-721 contract.

All you have to do is call `register` on the [IP Asset Registry](doc:ip-asset-registry) with:

* `chainid` - you can simply use `block.chainid`
* `tokenContract` - the address of your NFT collection
* `tokenId` - your NFT's ID

Let's create a test file to see it work and verify the results:

> 📘 Contract Addresses
>
> We have filled in the addresses from the Story contracts for you. However you can also find the addresses for them here: [Deployed Smart Contracts](doc:deployed-smart-contracts)

```sol test/0_IPARegistrar.sol
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.26;

import { Test } from "forge-std/Test.sol";
import { IIPAssetRegistry } from "@storyprotocol/core/interfaces/registries/IIPAssetRegistry.sol";

// your own ERC-721 NFT contract
import { SimpleNFT } from "../src/mocks/SimpleNFT.sol";

// Run this test:
// forge test --fork-url https://aeneid.storyrpc.io/ --match-path test/0_IPARegistrar.t.sol
contract IPARegistrarTest is Test {
    address internal alice = address(0xa11ce);

    // For addresses, see https://docs.story.foundation/docs/deployed-smart-contracts
    // Protocol Core - IPAssetRegistry
    IIPAssetRegistry internal IP_ASSET_REGISTRY = IIPAssetRegistry(0x77319B4031e6eF1250907aa00018B8B1c67a244b);

    SimpleNFT public SIMPLE_NFT;

    function setUp() public {
        // Create a new Simple NFT collection
        SIMPLE_NFT = new SimpleNFT("Simple IP NFT", "SIM");
    }

    /// @notice Mint an NFT and then register it as an IP Asset.
    function test_register() public {
        uint256 expectedTokenId = SIMPLE_NFT.nextTokenId();
        address expectedIpId = IP_ASSET_REGISTRY.ipId(block.chainid, address(SIMPLE_NFT), expectedTokenId);

        uint256 tokenId = SIMPLE_NFT.mint(alice);
        address ipId = IP_ASSET_REGISTRY.register(block.chainid, address(SIMPLE_NFT), tokenId);

        assertEq(tokenId, expectedTokenId);
        assertEq(ipId, expectedIpId);
        assertEq(SIMPLE_NFT.ownerOf(tokenId), alice);
    }
}
```

## Scenario #2: You want to create an SPG NFT contract to do minting for you

If you don't have your own custom NFT contract, this is the section for you.

To achieve this, we will be using the [📦 SPG](doc:spg), which is a utility contract that allows us to combine multiple transactions into one. In this case, we'll be using the SPG's `mintAndRegisterIp` function which combines both minting an NFT and registering it as an IP Asset.

In order to use `mintAndRegisterIp`, we first have to create a new `SPGNFT` collection. We can do this simply by calling `createCollection` on the `StoryProtocolGateway` contract. Or, if you want to create your own `SPGNFT` for some reason, you can implement the [ISPGNFT](https://github.com/storyprotocol/protocol-periphery-v1/blob/main/contracts/interfaces/ISPGNFT.sol) contract interface. Follow the example below to see example parameters you can use to initialize a new SPGNFT.

Once you have your own SPGNFT, all you have to do is call `mintAndRegisterIp` with:

* `spgNftContract` - the address of your SPGNFT contract
* `recipient` - the address of who will receive the NFT and thus be the owner of the newly registered IP. *Note: remember that registering IP on Story is permissionless, so you can register an IP for someone else (by paying for the transaction) yet they can still be the owner of that IP Asset.*
* `ipMetadata` - the metadata associated with your NFT & IP. See [this](https://docs.story.foundation/docs/ip-asset#nft-vs-ip-metadata) section to better understand setting NFT & IP metadata.

Let's create a test file to see it work and verify the results:

> 📘 Contract Addresses
>
> We have filled in the addresses from the Story contracts for you. However you can also find the addresses for them here: [Deployed Smart Contracts](doc:deployed-smart-contracts)

```sol test/0_IPARegistrar.sol
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.26;

import { Test } from "forge-std/Test.sol";
import { IIPAssetRegistry } from "@storyprotocol/core/interfaces/registries/IIPAssetRegistry.sol";
import { ISPGNFT } from "@storyprotocol/periphery/interfaces/ISPGNFT.sol";
import { IRegistrationWorkflows } from "@storyprotocol/periphery/interfaces/workflows/IRegistrationWorkflows.sol";
import { WorkflowStructs } from "@storyprotocol/periphery/lib/WorkflowStructs.sol";

// Run this test:
// forge test --fork-url https://aeneid.storyrpc.io/ --match-path test/0_IPARegistrar.t.sol
contract IPARegistrarTest is Test {
    address internal alice = address(0xa11ce);

    // For addresses, see https://docs.story.foundation/docs/deployed-smart-contracts
    // Protocol Core - IPAssetRegistry
    IIPAssetRegistry internal IP_ASSET_REGISTRY = IIPAssetRegistry(0x77319B4031e6eF1250907aa00018B8B1c67a244b);
    // Protocol Periphery - RegistrationWorkflows
    IRegistrationWorkflows internal REGISTRATION_WORKFLOWS =
        IRegistrationWorkflows(0xbe39E1C756e921BD25DF86e7AAa31106d1eb0424);

    ISPGNFT public SPG_NFT;

    function setUp() public {
        // Create a new NFT collection via SPG
        SPG_NFT = ISPGNFT(
            REGISTRATION_WORKFLOWS.createCollection(
                ISPGNFT.InitParams({
                    name: "Test Collection",
                    symbol: "TEST",
                    baseURI: "",
                    contractURI: "",
                    maxSupply: 100,
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

    /// @notice Mint an NFT and register it in the same call via the Story Protocol Gateway.
    /// @dev Requires the collection address that is passed into the `mintAndRegisterIp` function
    /// to be created via SPG (createCollection), as done above. Or, a contract that
    /// implements the `ISPGNFT` interface.
    function test_mintAndRegisterIp() public {
        uint256 expectedTokenId = SPG_NFT.totalSupply() + 1;
        address expectedIpId = IP_ASSET_REGISTRY.ipId(block.chainid, address(SPG_NFT), expectedTokenId);

        // Note: The caller of this function must be the owner of the SPG NFT Collection.
        // In this case, the owner of the SPG NFT Collection is the contract itself
        // because it deployed it in the `setup` function.
        // We can make `alice` the recipient of the NFT though, which makes her the
        // owner of not only the NFT, but therefore the IP Asset.
        (address ipId, uint256 tokenId) = REGISTRATION_WORKFLOWS.mintAndRegisterIp(
            address(SPG_NFT),
            alice,
            WorkflowStructs.IPMetadata({
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
            }),
            true
        );

        assertEq(ipId, expectedIpId);
        assertEq(tokenId, expectedTokenId);
        assertEq(SPG_NFT.ownerOf(tokenId), alice);
    }
}
```

## Add License Terms to IP

Congratulations, you registered an IP!

<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/story-protocol-boilerplate/blob/main/test/0_IPARegistrar.t.sol" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    Follow the completed code all the way through.
  </Card>
</Cards>

Now that your IP is registered, you can create and attach [License Terms](doc:license-terms) to it. This will allow others to mint a license and use your IP, restricted by the terms.

We will go over this on the next page.