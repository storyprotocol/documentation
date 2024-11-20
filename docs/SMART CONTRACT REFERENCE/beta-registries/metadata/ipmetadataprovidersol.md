---
title: IPMetadataProvider.sol
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
Base contract used for customization of canonical IP metadata.

### MODULE\_REGISTRY

```solidity
contract IModuleRegistry MODULE_REGISTRY
```

Gets the protocol-wide module registry.

### \_ipMetadata

```solidity
mapping(address => bytes) _ipMetadata
```

Maps IPs to their metadata based on their IP IDs.

### constructor

```solidity
constructor(address moduleRegistry) public
```

### getMetadata

```solidity
function getMetadata(address ipId) external view virtual returns (bytes)
```

Gets the metadata associated with an IP asset.

#### Parameters

| Name | Type    | Description                             |
| ---- | ------- | --------------------------------------- |
| ipId | address | The address identifier of the IP asset. |

#### Return Values

| Name | Type  | Description                                                 |
| ---- | ----- | ----------------------------------------------------------- |
| \[0] | bytes | metadata The encoded metadata associated with the IP asset. |

### setMetadata

```solidity
function setMetadata(address ipId, bytes metadata) external
```

Sets the metadata associated with an IP asset.

#### Parameters

| Name     | Type    | Description                                           |
| -------- | ------- | ----------------------------------------------------- |
| ipId     | address | The address identifier of the IP asset.               |
| metadata | bytes   | The metadata in bytes to associate with the IP asset. |
