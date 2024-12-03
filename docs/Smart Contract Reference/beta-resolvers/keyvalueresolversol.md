---
title: KeyValueResolver.sol
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
Resolver used for returning values associated with keys. This is the\
        preferred approach for adding additional attribution to IP that the\
        IP originator thinks is beneficial to have on chain.

### \_values

```solidity
mapping(address => mapping(string => string)) _values
```

*Stores key-value pairs associated with each IP.*

### setValue

```solidity
function setValue(address ipId, string key, string val) external virtual
```

Sets the string value for a specified key of an IP ID.

*Enforced to be only callable by users with valid permission to call on behalf of the ipId.*

#### Parameters

| Name | Type    | Description                               |
| ---- | ------- | ----------------------------------------- |
| ipId | address | The canonical identifier of the IP asset. |
| key  | string  | The string parameter key to update.       |
| val  | string  | The value to set for the specified key.   |

### value

```solidity
function value(address ipId, string key) external view virtual returns (string)
```

Retrieves the string value associated with a key for an IP asset.

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
        ipId
      </td>

      <td>
        address
      </td>

      <td>

      </td>
    </tr>

    <tr>
      <td>
        key
      </td>

      <td>
        string
      </td>

      <td>
        The string parameter key to query.
      </td>
    </tr>
  </tbody>
</Table>

#### Return Values

| Name | Type   | Description                                        |
| ---- | ------ | -------------------------------------------------- |
| \[0] | string | value The value associated with the specified key. |

### supportsInterface

```solidity
function supportsInterface(bytes4 id) public view virtual returns (bool)
```

IERC165 interface support.
