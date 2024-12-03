---
title: Register an NFT as an IP Asset
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# IP Asset Registration

In Story Protocol, an IP is recognized as any ERC-721 compatible NFT. Yes, that means your Bored Ape or Crypto Punk is already eligible for registration into our protocol, and don't worry, there is no wrapping involved! Before going forward, it is important to distinguish the difference between an IP and an IP Asset (IPA):

*Only once an ERC-721 is registered into Story Protocol's global IP asset registry, is a corresponding IP record generated for that original IP. We refer to this globally unique record as the IP Asset, and the original ERC-721 as the IP.*

Also, one last important detail to note: Once registered, not only does every IP (ERC-721) have a IPA (record) associated with it, but also an IP Account, which is a separate contract bound to the original IP for controlling permissions around interactions with Story Protocol's modules. You can think of the IPA as the record keeper for the IP, and the IP Account as its permissions manager.

Now that we've covered what an IPA is, let's go over how you might register your IP via the IP Asset Registry.

## Registration via the IP Asset Registry

Because the IPA registry is fully permissionless, you can also register directly to it as well. The only caveat is that it won't bundle policy registration or add custom metadata for you, you will have to do that as an extra step yourself. This is because the core IP asset registry is only responsible for holding the canonical IP metadata. Policies are added via the licensing module, whereas custom metadata is added to the IP resolver that gets bound to the record. As such, we recommend direct registrations for power users who are looking to build something more customizable.

Let's see how we can perform a barebones registration using the IP Asset Registry directly:

```
pragma solidity ^0.8.23;

import { IPAssetRegistry } from "@story-protocol/protocol-core/contracts/registries/IPAssetRegistry.sol";
import { IPResolver } from "@story-protocol/protocol-core/contracts/resolvers/IPResolver.sol";
import { IP } from "@story-protocol/protocol-core/contracts/lib/IP.sol";

contract ExampleIPARegistryRegistration {

  IPResolver public resolver;
  IPAssetRegistry public immutable REGISTRY;

  constructor(address registry, address resolverAddr) {
    REGISTRY = IPAssetRegistry(registry);
    resolver = IPResolver(resolverAddr);
  }

  function register(
  	address tokenContract,
     uint256 tokenId
	) public returns (address ipId) {
      bytes memory metadata = abi.encode(
        IP.MetadataV1({
          name: "name for your IP asset",
          hash:  bytes32("your IP asset content hash"),
          registrationDate: uint64(block.timestamp),
          registrant: msg.sender,
          uri: "https://yourip.xyz/metadata-regarding-its-ip"
        })
      );

      ipId = REGISTRY.register(
        block.chainid,
        tokenContract,
        tokenId,
        address(resolver),
        true,
        metadata
      );
  }
}

```

In this case, we are doing something similar. First, we have to ABI-encode the IP metadata, and then pass similar parameters as before into the `register` function. The difference, however, as mentioned earlier, is that here we do not add any *custom* metadata, nor do we perform any policy additions.

## Registration Delegation

Now, you might ask yourself - what if I want someone else to perform a registration of my IP asset on my behalf? To understand how to go about this, it is important you first familiarize yourself with how the [Access Controller](doc:access-controller) works for IPA-based authorization. 

With that understanding, it is important to now note that IPA registration is one action that is slightly different than all other IP-based module actions, and the reason for this is that because an IPA does not yet exist prior to registration,  it does not have any ACL mechanisms. Because of this, the IP asset registry, besides permitting registration if `msg.sender` is the NFT owner, also permits the call if `msg.sender` is either a protocol-approved module, or was given registry operator permissions by the original NFT owner.

For the former case, to allow delegation to a custom contract, your contract must first be approved by the protocol, so that we ensure proper authorization is added and that your code is adequately vetted. For more information on this, please see the section [Build a periphery contract](doc:build-a-licensing-marketplace).

The latter case, however, provides a simple way of enabling delegation. A user who wishes to delegate registration to another user or contract can simply call `setApprovalForAll` on the registry for the approved operator, much like you do with existing ERC-721 contracts. Below, we show how this can be tested:

```
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.23;

import { Test } from "forge-std/Test.sol";

import { ERC721 } from "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import { IPAssetRegistry } from "@story-protocol/protocol-core/contracts/registries/IPAssetRegistry.sol";
import { IPResolver } from "@story-protocol/protocol-core/contracts/resolvers/IPResolver.sol";
import { IP } from "@story-protocol/protocol-core/contracts/lib/IP.sol";

contract MockERC721 is ERC721 {

    uint256 totalSupply = 0;

    constructor(string memory name, string memory symbol) ERC721(name, symbol) {}

    function mint() external returns (uint256 id) {
        id = totalSupply++;
        _mint(msg.sender, id);
    }
}

contract IPARegistrarTest is Test {

    address payable internal alice = payable(vm.addr(0xa1c3));
    address payable internal bob = payable(vm.addr(0xb0b));

  IPAssetRegistry public REGISTRY;

     address public constant IPA_REGISTRY_ADDR = address(0x292639452A975630802C17c9267169D93BD5a793);
    address public constant IP_RESOLVER_ADDR = address(0x3809f4128B0B33AFb17576edafD7D4F4E2ABE933);
    uint256 public constant TEST_LICENSE = 0;
  	bytes public ROYALTY_CONTEXT = "";

    MockERC721 public nft;

    function setUp() public {
        nft = new MockERC721("Story Mock NFT", "STORY");
        REGISTRY = IPAssetRegistry(IPA_REGISTRY_ADDR);
    }

    function test_IPARegistrationApproval() public {
      uint256[] memory licenses  = new uint256[](1);
      licenses[0] = TEST_LICENSE;

      bytes memory metadata = abi.encode(
        IP.MetadataV1({
          name: "name for your IP asset",
          hash:  bytes32("your IP asset content hash"),
          registrationDate: uint64(block.timestamp),
          registrant: msg.sender,
          uri: "https://yourip.xyz/metadata-regarding-its-ip"
        })
      );

      // Get one NFT as Alice
      vm.prank(alice);
      uint256 tokenId = nft.mint();

      // If bob tries to register on behalf of Alice, it will fail.
      vm.prank(bob);
      vm.expectRevert();
      REGISTRY.register(
        licenses,
        ROYALTY_CONTEXT,
        block.chainid,
        address(nft),
        tokenId,
        IP_RESOLVER_ADDR,
        true,
        metadata
      );

      // If Alice however approves bob as an operator, it will succeed.
      vm.prank(alice);
      REGISTRY.setApprovalForAll(bob, true);
      vm.prank(bob);
      REGISTRY.register(
        licenses,
        ROYALTY_CONTEXT,
        block.chainid,
        address(nft),
        tokenId,
        IP_RESOLVER_ADDR,
        true,
        metadata
      );
    }

}

```

Using these primitives around delegation can allow you to build powerful apps that simplifies the UX around user-managed NFT registration. Again, it is important to note that for all other module interactions, delegation requires a very different process, which you can read in [Delegate IPA calls](doc:granting-ip-permissions-to-a-relayer-or-3rd-party-module).
