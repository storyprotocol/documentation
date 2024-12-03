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
        \_

        array
      </td>

      <td>
        address

        \[

        ]
      </td>

      <td>
        The input array to search
      </td>
    </tr>

    <tr>
      <td>
        \_

        element
      </td>

      <td>
        address
      </td>

      <td>
        The value to find
      </td>
    </tr>
  </tbody>
</Table>

#### Return Values

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
        \[0]
      </td>

      <td>
        uint32
      </td>

      <td>
        Returns (index and isIn) for the first occurrence starting from index 0
      </td>
    </tr>

    <tr>
      <td>
        \[1]
      </td>

      <td>
        bool
      </td>

      <td>

      </td>
    </tr>
  </tbody>
</Table>
