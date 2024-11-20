---
title: MetadataProviderV1.sol
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
Storage provider for Story Protocol canonical IP metadata (v1).

### constructor

```solidity
constructor(address ipAssetRegistry) public
```

Initializes the metadata provider contract.

#### Parameters

| Name            | Type    | Description                          |
| --------------- | ------- | ------------------------------------ |
| ipAssetRegistry | address | The protocol-wide IP asset registry. |

### metadata

```solidity
function metadata(address ipId) external view returns (struct IP.MetadataV1)
```

Fetches the metadata linked to an IP asset.

#### Parameters

| Name | Type    | Description                             |
| ---- | ------- | --------------------------------------- |
| ipId | address | The address identifier of the IP asset. |

#### Return Values

| Name | Type                 | Description                                   |
| ---- | -------------------- | --------------------------------------------- |
| \[0] | struct IP.MetadataV1 | metadata The metadata linked to the IP asset. |

### name

```solidity
function name(address ipId) external view returns (string)
```

Gets the name associated with the IP asset.

#### Parameters

| Name | Type    | Description                             |
| ---- | ------- | --------------------------------------- |
| ipId | address | The address identifier of the IP asset. |

#### Return Values

| Name | Type   | Description                                 |
| ---- | ------ | ------------------------------------------- |
| \[0] | string | name The name associated with the IP asset. |

### hash

```solidity
function hash(address ipId) external view returns (bytes32)
```

Gets the hash associated with the IP asset.

#### Parameters

| Name | Type    | Description                             |
| ---- | ------- | --------------------------------------- |
| ipId | address | The address identifier of the IP asset. |

#### Return Values

| Name | Type    | Description                                 |
| ---- | ------- | ------------------------------------------- |
| \[0] | bytes32 | hash The hash associated with the IP asset. |

### registrationDate

```solidity
function registrationDate(address ipId) external view returns (uint64)
```

Gets the date in which the IP asset was registered.

#### Parameters

| Name | Type    | Description                             |
| ---- | ------- | --------------------------------------- |
| ipId | address | The address identifier of the IP asset. |

#### Return Values

| Name | Type   | Description                                                     |
| ---- | ------ | --------------------------------------------------------------- |
| \[0] | uint64 | registrationDate The date in which the IP asset was registered. |

### registrant

```solidity
function registrant(address ipId) external view returns (address)
```

Gets the initial registrant address of the IP asset.

#### Parameters

| Name | Type    | Description                             |
| ---- | ------- | --------------------------------------- |
| ipId | address | The address identifier of the IP asset. |

#### Return Values

| Name | Type    | Description                                                |
| ---- | ------- | ---------------------------------------------------------- |
| \[0] | address | registrant The initial registrant address of the IP asset. |

### uri

```solidity
function uri(address ipId) external view returns (string)
```

Gets the external URI associated with the IP asset.

#### Parameters

| Name | Type    | Description                             |
| ---- | ------- | --------------------------------------- |
| ipId | address | The address identifier of the IP asset. |

#### Return Values

| Name | Type   | Description                                        |
| ---- | ------ | -------------------------------------------------- |
| \[0] | string | uri The external URI associated with the IP asset. |

### \_verifyMetadata

```solidity
function _verifyMetadata(bytes data) internal virtual
```

*Checks that the data conforms to the canonical metadata standards.*

#### Parameters

| Name | Type  | Description                                |
| ---- | ----- | ------------------------------------------ |
| data | bytes | The canonical metadata in bytes to verify. |

### \_compatible

```solidity
function _compatible(bytes m1, bytes m2) internal pure virtual returns (bool)
```

*Checks whether two sets of metadata are compatible with one another.\
TODO: Add try-catch for ABI-decoding error handling.*

### \_hash

```solidity
function _hash(struct IP.MetadataV1 data) internal pure returns (bytes32)
```

*Gets the bytes32 hash for a MetadataV1 data struct.*

### \_metadataV1

```solidity
function _metadataV1(address ipId) internal view returns (struct IP.MetadataV1)
```

*Get the decoded canonical metadata belonging to an IP asset.*
