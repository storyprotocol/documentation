---
title: IPAssetRenderer.sol
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
The IP Asset renderer is responsible for rendering canonical metadata associated with each IP Asset. This includes generation of attributes, on-chain SVGs, and external URLs. Note that the underlying data being rendered is strictly immutable.

### IP_ASSET_REGISTRY

```solidity
contract IPAssetRegistry IP_ASSET_REGISTRY
```

The global IP Asset registry.

### LICENSE_REGISTRY

```solidity
contract LicenseRegistry LICENSE_REGISTRY
```

The global licensing registry.

### TAGGING_MODULE

```solidity
contract TaggingModule TAGGING_MODULE
```

### ROYALTY_MODULE

```solidity
contract RoyaltyModule ROYALTY_MODULE
```

### constructor

```solidity
constructor(address assetRegistry, address licenseRegistry, address taggingModule, address royaltyModule) public
```

Initializes the IP asset renderer.  
TODO: Add different customization options - e.g. font, colorways, etc.  
TODO: Add an external URL for generating SP-branded links for each IP.

### name

```solidity
function name(address ipId) external view returns (string)
```

Fetches the canonical name associated with the specified IP.

#### Parameters

| Name | Type    | Description                           |
| ---- | ------- | ------------------------------------- |
| ipId | address | The canonical ID of the specified IP. |

### description

```solidity
function description(address ipId) public view returns (string)
```

Fetches the canonical description associated with the IP.

#### Parameters

| Name | Type    | Description                           |
| ---- | ------- | ------------------------------------- |
| ipId | address | The canonical ID of the specified IP. |

#### Return Values

| Name | Type   | Description                                                                                                                                                            |
| ---- | ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [0]  | string | The string descriptor of the IP. TODO: Add more information related to licensing or royalties. TODO: Update the description to an SP base URL if external URL not set. |

### hash

```solidity
function hash(address ipId) external view returns (bytes32)
```

Fetches the keccak-256 content hash associated with the specified IP.

#### Parameters

| Name | Type    | Description                           |
| ---- | ------- | ------------------------------------- |
| ipId | address | The canonical ID of the specified IP. |

#### Return Values

| Name | Type    | Description                         |
| ---- | ------- | ----------------------------------- |
| [0]  | bytes32 | The bytes32 content hash of the IP. |

### registrationDate

```solidity
function registrationDate(address ipId) external view returns (uint64)
```

Fetches the date of registration of the IP.

#### Parameters

| Name | Type    | Description                           |
| ---- | ------- | ------------------------------------- |
| ipId | address | The canonical ID of the specified IP. |

### registrant

```solidity
function registrant(address ipId) external view returns (address)
```

Fetches the initial registrant of the IP.

#### Parameters

| Name | Type    | Description                           |
| ---- | ------- | ------------------------------------- |
| ipId | address | The canonical ID of the specified IP. |

### uri

```solidity
function uri(address ipId) external view returns (string)
```

Fetches the external URL associated with the IP.

#### Parameters

| Name | Type    | Description                           |
| ---- | ------- | ------------------------------------- |
| ipId | address | The canonical ID of the specified IP. |

### owner

```solidity
function owner(address ipId) public view returns (address)
```

Fetches the current owner of the IP.

#### Parameters

| Name | Type    | Description                           |
| ---- | ------- | ------------------------------------- |
| ipId | address | The canonical ID of the specified IP. |

### tokenURI

```solidity
function tokenURI(address ipId) external view returns (string)
```

Generates a JSON of all metadata attribution related to the IP.  
TODO: Make this ERC-721 compatible, so that the IP registry may act as  
      an account-bound ERC-721 that points to this function for metadata.  
TODO: Add SVG support.  
TODO: Add licensing, royalties, and tagging information support.

### \_metadata

```solidity
function _metadata(address ipId) internal view returns (struct IP.MetadataV1 metadata)
```

_Internal function for fetching the metadata tied to an IP record._