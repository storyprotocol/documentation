---
title: DataUniqueness.sol
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
Library to store data without repetition, assigning an id to it if new or reusing existing one if already stored

### addIdOrGetExisting

```solidity
function addIdOrGetExisting(bytes data, mapping(bytes32 => uint256) _hashToIds, uint256 existingIds) internal returns (uint256 id, bool isNew)
```

Stores data without repetition, assigning an id to it if new or reusing existing one if already stored

#### Parameters

| Name        | Type                        | Description                                   |
| ----------- | --------------------------- | --------------------------------------------- |
| data        | bytes                       | raw bytes, abi.encode() a value to be hashed  |
| \_hashToIds | mapping(bytes32 => uint256) | storage ref to the mapping of hash -> data id |
| existingIds | uint256                     | amount of distinct data stored.               |

#### Return Values

| Name  | Type    | Description                                                                                                                                |
| ----- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| id    | uint256 | new sequential id if new data, reused id if not new                                                                                        |
| isNew | bool    | True if a new id was generated, signaling the value was stored in \_hashToIds.               False if id is reused and data was not stored |