---
title: MetadataProviderBase.sol
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
Metadata provider base contract for storing canonical IP metadata.

### IP_ASSET_REGISTRY

```solidity
contract IIPAssetRegistry IP_ASSET_REGISTRY
```

Returns the protocol-wide IP Asset registry.

### upgradeProvider

```solidity
contract IMetadataProvider upgradeProvider
```

Returns the new metadata provider IP Assets may migrate to.

### \_ipMetadata

```solidity
mapping(address => bytes) _ipMetadata
```

Maps IP Assets (via their IP ID) to their canonical metadata.

### onlyIPAssetRegistry

```solidity
modifier onlyIPAssetRegistry()
```

Restricts calls to only originate from a protocol-authorized caller.

### constructor

```solidity
constructor(address ipAssetRegistry) internal
```

### getMetadata

```solidity
function getMetadata(address ipId) external view virtual returns (bytes)
```

Gets the metadata associated with an IP Asset.

#### Parameters

| Name | Type    | Description                             |
| ---- | ------- | --------------------------------------- |
| ipId | address | The address identifier of the IP Asset. |

#### Return Values

| Name | Type  | Description                                                 |
| ---- | ----- | ----------------------------------------------------------- |
| [0]  | bytes | metadata The encoded metadata associated with the IP Asset. |

### setUpgradeProvider

```solidity
function setUpgradeProvider(address provider) external
```

Sets a upgrade provider for users to migrate their metadata to.

#### Parameters

| Name     | Type    | Description                                             |
| -------- | ------- | ------------------------------------------------------- |
| provider | address | The address of the new metadata provider to migrate to. |

### upgrade

```solidity
function upgrade(address payable ipId, bytes metadata) external
```

Updates the provider used by the IP Asset, migrating existing metadata to the new provider, and adding  
new metadata.

#### Parameters

| Name     | Type            | Description                                                     |
| -------- | --------------- | --------------------------------------------------------------- |
| ipId     | address payable | The address identifier of the IP Asset.                         |
| metadata | bytes           | Additional metadata in bytes used by the new metadata provider. |

### setMetadata

```solidity
function setMetadata(address ipId, bytes metadata) external virtual
```

Sets the metadata associated with an IP Asset.

_Enforced to be only callable by the IP Asset registry._

#### Parameters

| Name     | Type    | Description                                           |
| -------- | ------- | ----------------------------------------------------- |
| ipId     | address | The address identifier of the IP Asset.               |
| metadata | bytes   | The metadata in bytes to associate with the IP Asset. |

### \_verifyMetadata

```solidity
function _verifyMetadata(bytes metadata) internal virtual
```

_Checks that the data conforms to the canonical metadata standards._

#### Parameters

| Name     | Type  | Description                                |
| -------- | ----- | ------------------------------------------ |
| metadata | bytes | The canonical metadata in bytes to verify. |

### \_compatible

```solidity
function _compatible(bytes m1, bytes m2) internal pure virtual returns (bool)
```

_Checks whether two sets of metadata are compatible with one another._

#### Parameters

| Name | Type  | Description                                      |
| ---- | ----- | ------------------------------------------------ |
| m1   | bytes | The first set of bytes metadata being compared.  |
| m2   | bytes | The second set of bytes metadata being compared. |