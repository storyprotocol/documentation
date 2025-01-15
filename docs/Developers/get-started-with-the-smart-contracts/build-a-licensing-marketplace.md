---
title: Build a periphery contract
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
In this guide, we'll go over how you can create your own periphery contract for interfacing with the core protocol modules. 

**Part One: Creating the contract**

```
import { IPAssetRegistry } from "@story-protocol/core/IPAssetRegistry.sol";
import { IPAssetRegistry } from "@story-protocol/core/interfaces/modules/licensing/ILicensingModule.sol";
import { IP } from "@story-protocol/core/lib/IP.sol";
import { IPResolver } from "@story-protocol/core/resolvers/IPResolver.sol";
import { IERC721 } from "@openzeppelin/contracts/token/ERC721/IERC721.sol";

contract IPRegistrationGateway.{
  
  address public immutable IP_RESOLVER;
  IPAssetRegistry public immutable IP_ASSET_REGISTRY;
  ILicensingModule public immutable LICENSING_MODULE;
  IERC721 public immutable STORY_MASCOT;
  uint256 public immutable STORY_MASCOT_POLICY_ID;
  
  // 0. Initialize the contract.
  constructor(
    address resolver,
    address ipAssetRegistry, 
    address licensingModule,
    address storyMascot, 
    uint256 policyId
  ) {
    IP_RESOLVER = resolver;
    IP_ASSET_REGISTRY = IPAssetRegistry(ipAssetRegistry);
    ILicensingModule = ILicensingModule(licensingModule);
    STORY_MASCOT = IERC721(storyMascot);
    STORY_MASCOT_POLICY_ID = policyId;
  }
  
  // Function for IP registration
  function registerIPAsset(
  	uint256 storyMascotId,
    string memory ipName,
    bytes32 contentHash,
    string calldata externalURL
  ) external returns (address) {
      
      // 1. Create core IP metadata compatible with the IP asset registry.
      bytes memory metadata = abi.encode(
        IP.MetadataV1({
          name: ipName,
          hash: contentHash,
          registrationDate: uint64(block.timestamp),
          registrant: msg.sender,
          uri: externalURL
        })
      );

      // 2. Register IP asset.
      address ipId = IP_ASSET_REGISTRY.register(
            block.chainid,
            address(STORY_MASCOT),
            storyMascotId,
            address(IP_RESOLVER),
            true,
            metadata
      );
      
      // 3. Add the policy to the IP.
      licensingModule.addPolicyToIp(ipId, STORY_MASCOT_POLICY_ID);
  }
}
```

> 📘 Learn more about Policy Ids
>
> [Presets, templates and Solidity helper library](https://docs.storyprotocol.xyz/docs/pil-flavors-preset-policy)

Here is an explanation of the steps involved:

0. **Constructor**\
   For the frontend contract to function, it must be aware of the addresses of various Story Protocol contracts (see the Protocol Reference for addresses of deployed contracts on all chains), as well as aware of the policy ID that the user wishes to tie to the IP. A policy ID identifies the encompassing licensing framework that will be used to generate licenses for the registered IP, and enforces how royalties, rights, and licensing should work for derivatives created from it 

{/*-*/}

0. **Metadata Creation**\
   All IP registered through Story Protocol must abide by a standard metadata structure, whose latest format can be found in the `IP` struct library. As of version `v0.1-beta`, the structure requires the following elements:
   1. `string name`
   2. `bytes32 hash`
   3. `uint64 registrationDate`
   4. `address registrant`
   5. `string uri`
1. **IP Registration**\
   For the IP to be created, a call must be invoked to the IP asset registry, providing the chain ID of the originating NFT, the NFT contract address, the NFT token id, and the address of the IP resolver to use for storing custom IP metadata attributes. The default Story Protocol IP resolver address may be found in `[link to IP resolver page]`.
2. **Policy Creation**\
   Finally, for the policy to be added to the IP, a call must be made to the licensing module with the required policy id. 

**Part Two: Granting approval for your Frontend to call the protocol**

For your Frontend to interact with the protocol, it must first get approval to do so. For the case of IP registration, there are two ways of doing this:

* **Direct approval for registration by the NFT owner**
  * A user who wishes to use your contract for registration simply has to call `setApprovalForAll` on the `ipAssetRegistry` contract, which grants your contract the permission to directly register on their behalf
* **Direct approval for registration by the Story Protocol DAO**
  * This applies for all IP workloads that extend outside of the simple process of registration. To be granted approval, you must first make a proposal to XXX, which will then grant your contract permissions via the access controller.
