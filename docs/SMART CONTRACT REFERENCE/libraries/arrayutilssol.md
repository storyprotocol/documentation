---
title: ArrayUtils.sol
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
Library for address array operations

### indexOf

```solidity
function indexOf(address[] _array, address _element) internal pure returns (uint32, bool)
```

Finds the index of the first occurrence of the given element.

#### Parameters

| Name      | Type       | Description               |
| --------- | ---------- | ------------------------- |
| \_array   | address\[] | The input array to search |
| \_element | address    | The value to find         |

#### Return Values

| Name | Type   | Description                                                             |
| ---- | ------ | ----------------------------------------------------------------------- |
| [0]  | uint32 | Returns (index and isIn) for the first occurrence starting from index 0 |
| [1]  | bool   |                                                                         |