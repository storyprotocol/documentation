---
title: IPAssetRegistry.sol
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
This contract acts as the source of truth for all IP registered in Story Protocol. An IP is identified by its contract address, token id, and coin type, meaning any NFT may be conceptualized as an IP. Once an IP is registered into the protocol, a corresponding IP asset is generated, which references an IP resolver for metadata attribution and an IP account for protocol authorization.

IMPORTANT: The IP account address, besides being used for protocol auth, is also the canonical IP identifier for the IP NFT.

### MODULE_REGISTRY

```solidity
contract IModuleRegistry MODULE_REGISTRY
```

The canonical module registry used by the protocol.

### REGISTRATION_MODULE

```solidity
contract IRegistrationModule REGISTRATION_MODULE
```

The registration module that interacts with IPAssetRegistry.

### totalSupply

```solidity
uint256 totalSupply
```

Tracks the total number of IP assets in existence.

### isApprovedForAll

```solidity
mapping(address => mapping(address => bool)) isApprovedForAll
```

Checks whether an operator is approved to register on behalf of an IP owner.

### \_records

```solidity
mapping(address => struct IIPAssetRegistry.Record) _records
```

_Maps an IP, identified by its IP ID, to an IP record._

### \_metadataProvider

```solidity
contract IMetadataProviderMigratable _metadataProvider
```

_Tracks the current metadata provider used for IP registrations._

### constructor

```solidity
constructor(address erc6551Registry, address ipAccountImpl, address moduleRegistry, address governance) public
```

TODO: Utilize module registry for fetching different modules.

### setApprovalForAll

```solidity
function setApprovalForAll(address operator, bool approved) external
```

Enables third party operators to register on behalf of an NFT owner.

#### Parameters

| Name     | Type    | Description                                               |
| -------- | ------- | --------------------------------------------------------- |
| operator | address | The address of the operator the sender authorizes.        |
| approved | bool    | Whether or not to approve that operator for registration. |

### setRegistrationModule

```solidity
function setRegistrationModule(address registrationModule) external
```

_Sets the registration module that interacts with IPAssetRegistry._

#### Parameters

| Name               | Type    | Description                             |
| ------------------ | ------- | --------------------------------------- |
| registrationModule | address | The address of the registration module. |

### setMetadataProvider

```solidity
function setMetadataProvider(address newMetadataProvider) external
```

_Sets the provider for storage of new IP metadata, while enabling existing IP assets to migrate their  
metadata to the new provider._

#### Parameters

| Name                | Type    | Description                                    |
| ------------------- | ------- | ---------------------------------------------- |
| newMetadataProvider | address | Address of the new metadata provider contract. |

### register

```solidity
function register(uint256 chainId, address tokenContract, uint256 tokenId, address resolverAddr, bool createAccount, bytes metadata_) external returns (address ipId_)
```

Registers an NFT as IP, creating a corresponding IP record.

#### Parameters

| Name          | Type    | Description                                           |
| ------------- | ------- | ----------------------------------------------------- |
| chainId       | uint256 | The chain identifier of where the NFT resides.        |
| tokenContract | address | The address of the NFT.                               |
| tokenId       | uint256 | The token identifier of the NFT.                      |
| resolverAddr  | address | The address of the resolver to associate with the IP. |
| createAccount | bool    | Whether to create an IP account when registering.     |
| metadata\_    | bytes   | Metadata in bytes to associate with the IP.           |

#### Return Values

| Name   | Type    | Description                             |
| ------ | ------- | --------------------------------------- |
| ipId\_ | address | The address of the newly registered IP. |

### register

```solidity
function register(uint256[] licenseIds, bytes royaltyContext, uint256 chainId, address tokenContract, uint256 tokenId, address resolverAddr, bool createAccount, bytes metadata_) external returns (address ipId_)
```

Registers an NFT as an IP using licenses derived from parent IP asset(s).

#### Parameters

| Name           | Type       | Description                                                   |
| -------------- | ---------- | ------------------------------------------------------------- |
| licenseIds     | uint256\[] | The parent IP asset licenses used to derive the new IP asset. |
| royaltyContext | bytes      | The context for the royalty module to process.                |
| chainId        | uint256    | The chain identifier of where the NFT resides.                |
| tokenContract  | address    | The address of the NFT.                                       |
| tokenId        | uint256    | The token identifier of the NFT.                              |
| resolverAddr   | address    | The address of the resolver to associate with the IP.         |
| createAccount  | bool       | Whether to create an IP account when registering.             |
| metadata\_     | bytes      | Metadata in bytes to associate with the IP.                   |

#### Return Values

| Name   | Type    | Description                             |
| ------ | ------- | --------------------------------------- |
| ipId\_ | address | The address of the newly registered IP. |

### ipId

```solidity
function ipId(uint256 chainId, address tokenContract, uint256 tokenId) public view returns (address)
```

Gets the canonical IP identifier associated with an IP NFT.

_This is equivalent to the address of its bound IP account._

#### Parameters

| Name          | Type    | Description                                   |
| ------------- | ------- | --------------------------------------------- |
| chainId       | uint256 | The chain identifier of where the IP resides. |
| tokenContract | address | The address of the IP.                        |
| tokenId       | uint256 | The token identifier of the IP.               |

#### Return Values

| Name | Type    | Description                                 |
| ---- | ------- | ------------------------------------------- |
| [0]  | address | ipId The IP's canonical address identifier. |

### isRegistered

```solidity
function isRegistered(address id) external view returns (bool)
```

Checks whether an IP was registered based on its ID.

#### Parameters

| Name | Type    | Description                          |
| ---- | ------- | ------------------------------------ |
| id   | address | The canonical identifier for the IP. |

#### Return Values

| Name | Type | Description                                                   |
| ---- | ---- | ------------------------------------------------------------- |
| [0]  | bool | isRegistered Whether the IP was registered into the protocol. |

### resolver

```solidity
function resolver(address id) external view returns (address)
```

Gets the resolver bound to an IP based on its ID.

#### Parameters

| Name | Type    | Description                          |
| ---- | ------- | ------------------------------------ |
| id   | address | The canonical identifier for the IP. |

#### Return Values

| Name | Type    | Description                                                            |
| ---- | ------- | ---------------------------------------------------------------------- |
| [0]  | address | resolver The IP resolver address if registered, else the zero address. |

### metadataProvider

```solidity
function metadataProvider() external view returns (address)
```

Gets the metadata provider used for new metadata registrations.

#### Return Values

| Name | Type    | Description                                                                          |
| ---- | ------- | ------------------------------------------------------------------------------------ |
| [0]  | address | metadataProvider The address of the metadata provider used for new IP registrations. |

### metadataProvider

```solidity
function metadataProvider(address id) external view returns (address)
```

Gets the metadata provider linked to an IP based on its ID.

#### Parameters

| Name | Type    | Description                          |
| ---- | ------- | ------------------------------------ |
| id   | address | The canonical identifier for the IP. |

#### Return Values

| Name | Type    | Description                                                                        |
| ---- | ------- | ---------------------------------------------------------------------------------- |
| [0]  | address | metadataProvider The metadata provider that was bound to this IP at creation time. |

### metadata

```solidity
function metadata(address id) external view returns (bytes)
```

Gets the underlying canonical metadata linked to an IP asset.

#### Parameters

| Name | Type    | Description                       |
| ---- | ------- | --------------------------------- |
| id   | address | The canonical ID of the IP asset. |

#### Return Values

| Name | Type  | Description                                                       |
| ---- | ----- | ----------------------------------------------------------------- |
| [0]  | bytes | metadata The metadata that was bound to this IP at creation time. |

### setMetadata

```solidity
function setMetadata(address id, address provider, bytes data) external
```

Sets the underlying metadata for an IP asset.

_As metadata is immutable but additive, this will only be used when an IP migrates from a new provider that  
introduces new attributes._

#### Parameters

| Name     | Type    | Description                                  |
| -------- | ------- | -------------------------------------------- |
| id       | address | The canonical ID of the IP.                  |
| provider | address |                                              |
| data     | bytes   | Canonical metadata to associate with the IP. |

### setResolver

```solidity
function setResolver(address id, address resolverAddr) public
```

Sets the resolver for an IP based on its canonical ID.

#### Parameters

| Name         | Type    | Description                            |
| ------------ | ------- | -------------------------------------- |
| id           | address | The canonical ID of the IP.            |
| resolverAddr | address | The address of the resolver being set. |

### \_register

```solidity
function _register(uint256[] licenseIds, bytes royaltyContext, uint256 chainId, address tokenContract, uint256 tokenId, address resolverAddr, bool createAccount, bytes data) internal returns (address id)
```

_Registers an NFT as an IP._

#### Parameters

| Name           | Type       | Description                                                |
| -------------- | ---------- | ---------------------------------------------------------- |
| licenseIds     | uint256\[] | IP asset licenses used to derive the new IP asset, if any. |
| royaltyContext | bytes      | The context for the royalty module to process.             |
| chainId        | uint256    | The chain identifier of where the NFT resides.             |
| tokenContract  | address    | The address of the NFT.                                    |
| tokenId        | uint256    | The token identifier of the NFT.                           |
| resolverAddr   | address    | The address of the resolver to associate with the IP.      |
| createAccount  | bool       | Whether to create an IP account when registering.          |
| data           | bytes      | Canonical metadata to associate with the IP.               |

### \_setResolver

```solidity
function _setResolver(address id, address resolverAddr) internal
```

_Sets the resolver for the specified IP._

#### Parameters

| Name         | Type    | Description                            |
| ------------ | ------- | -------------------------------------- |
| id           | address | The canonical ID of the IP.            |
| resolverAddr | address | The address of the resolver being set. |

### \_setMetadata

```solidity
function _setMetadata(address id, contract IMetadataProviderMigratable provider, bytes data) internal
```

_Sets the for the specified IP asset._

#### Parameters

| Name     | Type                                 | Description                                          |
| -------- | ------------------------------------ | ---------------------------------------------------- |
| id       | address                              | The canonical identifier for the specified IP asset. |
| provider | contract IMetadataProviderMigratable | The metadata provider hosting the data.              |
| data     | bytes                                | The metadata to set for the IP asset.                |