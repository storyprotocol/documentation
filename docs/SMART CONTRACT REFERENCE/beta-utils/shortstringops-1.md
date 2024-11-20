---
title: ShortStringOps
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
## ShortStringOps

Library for working with Openzeppelin's ShortString data types.

### equal

```solidity
function equal(ShortString a, ShortString b) internal pure returns (bool)
```

_Compares whether two ShortStrings are equal._

### equal

```solidity
function equal(ShortString a, string b) internal pure returns (bool)
```

_Checks whether a ShortString and a regular string are equal._

### equal

```solidity
function equal(string a, ShortString b) internal pure returns (bool)
```

_Checks whether a regular string and a ShortString are equal._

### equal

```solidity
function equal(bytes32 a, ShortString b) internal pure returns (bool)
```

_Checks whether a bytes32 object and ShortString are equal._

### equal

```solidity
function equal(string a, bytes32 b) internal pure returns (bool)
```

_Checks whether a string and bytes32 object are equal._

### equal

```solidity
function equal(bytes32 a, string b) internal pure returns (bool)
```

_Checks whether a bytes32 object and string are equal._

### stringToBytes32

```solidity
function stringToBytes32(string s) internal pure returns (bytes32)
```

### bytes32ToString

```solidity
function bytes32ToString(bytes32 b) internal pure returns (string)
```