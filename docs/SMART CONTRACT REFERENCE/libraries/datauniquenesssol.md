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

<Table>
  <thead>
    <tr>
      <th>
        Name
      </th>

      <th>
        Type
      </th>

      <th>
        Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        data
      </td>

      <td>
        bytes
      </td>

      <td>
        raw bytes, abi.encode() a value to be hashed
      </td>
    </tr>

    <tr>
      <td>
        \_

        hashToIds
      </td>

      <td>
        mapping(bytes32 => uint256)
      </td>

      <td>
        storage ref to the mapping of hash -> data id
      </td>
    </tr>

    <tr>
      <td>
        existingIds
      </td>

      <td>
        uint256
      </td>

      <td>
        amount of distinct data stored.
      </td>
    </tr>
  </tbody>
</Table>

#### Return Values

| Name  | Type    | Description                                                                                                                                |
| ----- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| id    | uint256 | new sequential id if new data, reused id if not new                                                                                        |
| isNew | bool    | True if a new id was generated, signaling the value was stored in \_hashToIds.               False if id is reused and data was not stored |
