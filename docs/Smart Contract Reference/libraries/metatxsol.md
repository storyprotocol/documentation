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
*This library provides functions for handling meta-transactions in the Story Protocol.*

### EIP712\_DOMAIN\_VERSION

```solidity
string EIP712_DOMAIN_VERSION
```

*Version of the EIP712 domain.*

### EIP712\_DOMAIN\_VERSION\_HASH

```solidity
bytes32 EIP712_DOMAIN_VERSION_HASH
```

*Hash of the EIP712 domain version.*

### EIP712\_DOMAIN

```solidity
bytes32 EIP712_DOMAIN
```

*EIP712 domain type hash.*

### EXECUTE

```solidity
bytes32 EXECUTE
```

*Execute type hash.*

### Execute

*Structure for the Execute type.*

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

*Calculates the EIP712 domain separator for the current contract.*

#### Return Values

| Name | Type    | Description                  |
| ---- | ------- | ---------------------------- |
| \[0] | bytes32 | The EIP712 domain separator. |

### calculateDomainSeparator

```solidity
function calculateDomainSeparator(address ipAccount) internal view returns (bytes32)
```

*Calculates the EIP712 domain separator for a given IP account.*

#### Parameters

| Name      | Type    | Description                                                 |
| --------- | ------- | ----------------------------------------------------------- |
| ipAccount | address | The IP account for which to calculate the domain separator. |

#### Return Values

| Name | Type    | Description                  |
| ---- | ------- | ---------------------------- |
| \[0] | bytes32 | The EIP712 domain separator. |

### getExecuteStructHash

```solidity
function getExecuteStructHash(struct MetaTx.Execute execute) internal pure returns (bytes32)
```

*Calculates the EIP712 struct hash of an Execute.*

#### Parameters

| Name    | Type                  | Description          |
| ------- | --------------------- | -------------------- |
| execute | struct MetaTx.Execute | The Execute to hash. |

#### Return Values

| Name | Type    | Description                            |
| ---- | ------- | -------------------------------------- |
| \[0] | bytes32 | The EIP712 struct hash of the Execute. |
