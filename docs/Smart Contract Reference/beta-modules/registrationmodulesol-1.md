---
title: RegistrationModule.sol
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
## Note

**The RegistrationModule will get phased out post-beta, so we recommend using the IPAssetRegistry for registering IP assets!**

The registration module is responsible for the registration of IP into the protocol. During registration, this module will register an IP into the protocol, create a resolver, and bind to it any licenses and terms specified by the IP registrant (IP account owner).

### name

```solidity
string name
```

Returns the string identifier associated with the module.

### ipResolver

```solidity
contract IPResolver ipResolver
```

Returns the metadata resolver used by the registration module.

### constructor

```solidity
constructor(address assetRegistry, address licensingModule, address resolverAddr) public
```

### registerRootIp

```solidity
function registerRootIp(uint256 policyId, address tokenContract, uint256 tokenId, string ipName, bytes32 contentHash, string externalURL) external returns (address)
```

Registers a root-level IP into the protocol. Root-level IPs can be thought of as organizational hubs\
for encapsulating policies that actual IPs can use to register through. As such, a root-level IP is not an\
actual IP, but a container for IP policy management for their child IP assets.

#### Parameters

| Name          | Type    | Description                                               |
| ------------- | ------- | --------------------------------------------------------- |
| policyId      | uint256 | The policy that identifies the licensing terms of the IP. |
| tokenContract | address | The address of the NFT bound to the root-level IP.        |
| tokenId       | uint256 | The token id of the NFT bound to the root-level IP.       |
| ipName        | string  | The name assigned to the new IP.                          |
| contentHash   | bytes32 | The content hash of the IP being registered.              |
| externalURL   | string  | An external URI to link to the IP.                        |

### registerDerivativeIp

```solidity
function registerDerivativeIp(uint256[] licenseIds, address tokenContract, uint256 tokenId, string ipName, bytes32 contentHash, string externalURL, bytes royaltyContext) external
```

Registers derivative IPs into the protocol. Derivative IPs are IP assets that inherit policies from\
parent IPs by burning acquired license NFTs.

#### Parameters

| Name           | Type       | Description                                                                                                                                                               |
| -------------- | ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| licenseIds     | uint256\[] | The licenses to incorporate for the new IP.                                                                                                                               |
| tokenContract  | address    | The address of the NFT bound to the derivative IP.                                                                                                                        |
| tokenId        | uint256    | The token id of the NFT bound to the derivative IP.                                                                                                                       |
| ipName         | string     | The name assigned to the new IP.                                                                                                                                          |
| contentHash    | bytes32    | The content hash of the IP being registered.                                                                                                                              |
| externalURL    | string     | An external URI to link to the IP.                                                                                                                                        |
| royaltyContext | bytes      | The royalty context for the derivative IP. TODO: Replace all metadata with a generic bytes parameter type, and do       encoding on the periphery contract level instead. |
