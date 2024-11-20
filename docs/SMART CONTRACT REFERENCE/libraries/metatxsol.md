---
title: MetaTx.sol
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
_This library provides functions for handling meta transactions in the Story Protocol._

### EIP712_DOMAIN_VERSION

```solidity
string EIP712_DOMAIN_VERSION
```

_Version of the EIP712 domain._

### EIP712_DOMAIN_VERSION_HASH

```solidity
bytes32 EIP712_DOMAIN_VERSION_HASH
```

_Hash of the EIP712 domain version._

### EIP712_DOMAIN

```solidity
bytes32 EIP712_DOMAIN
```

_EIP712 domain type hash._

### EXECUTE

```solidity
bytes32 EXECUTE
```

_Execute type hash._

### Execute

_Structure for the Execute type._

```solidity
struct Execute {
  address to;
  uint256 value;
  bytes data;
  uint256 nonce;
  uint256 deadline;
}
```

### calculateDomainSeparator

```solidity
function calculateDomainSeparator() internal view returns (bytes32)
```

_Calculates the EIP712 domain separator for the current contract._

#### Return Values

| Name | Type    | Description                  |
| ---- | ------- | ---------------------------- |
| [0]  | bytes32 | The EIP712 domain separator. |

### calculateDomainSeparator

```solidity
function calculateDomainSeparator(address ipAccount) internal view returns (bytes32)
```

_Calculates the EIP712 domain separator for a given IP account._

#### Parameters

| Name      | Type    | Description                                                 |
| --------- | ------- | ----------------------------------------------------------- |
| ipAccount | address | The IP account for which to calculate the domain separator. |

#### Return Values

| Name | Type    | Description                  |
| ---- | ------- | ---------------------------- |
| [0]  | bytes32 | The EIP712 domain separator. |

### getExecuteStructHash

```solidity
function getExecuteStructHash(struct MetaTx.Execute execute) internal pure returns (bytes32)
```

_Calculates the EIP712 struct hash of an Execute._

#### Parameters

| Name    | Type                  | Description          |
| ------- | --------------------- | -------------------- |
| execute | struct MetaTx.Execute | The Execute to hash. |

#### Return Values

| Name | Type    | Description                            |
| ---- | ------- | -------------------------------------- |
| [0]  | bytes32 | The EIP712 struct hash of the Execute. |