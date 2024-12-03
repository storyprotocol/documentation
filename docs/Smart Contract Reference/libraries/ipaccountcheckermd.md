---
title: IPAccountChecker.sol
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
*This library provides utility functions to check the registration and validity of IP Accounts.\
It uses the ERC165 standard for contract introspection and the IIPAccountRegistry interface\
for account registration checks.*

### isRegistered

```solidity
function isRegistered(contract IIPAccountRegistry ipAccountRegistry_, uint256 chainId_, address tokenContract_, uint256 tokenId_) external view returns (bool)
```

Returns true if the IPAccount is registered.

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
        ipAccountRegistry

        \_
      </td>

      <td>
        contract IIPAccountRegistry
      </td>

      <td>

      </td>
    </tr>

    <tr>
      <td>
        chainId

        \_
      </td>

      <td>
        uint256
      </td>

      <td>
        The chain ID where the IP Account is located.
      </td>
    </tr>

    <tr>
      <td>
        tokenContract

        \_
      </td>

      <td>
        address
      </td>

      <td>
        The address of the token contract associated with the IP Account.
      </td>
    </tr>

    <tr>
      <td>
        tokenId

        \_
      </td>

      <td>
        uint256
      </td>

      <td>
        The ID of the token associated with the IP Account.
      </td>
    </tr>
  </tbody>
</Table>

#### Return Values

| Name | Type | Description                                            |
| ---- | ---- | ------------------------------------------------------ |
| \[0] | bool | True if the IP Account is registered, false otherwise. |

### isIpAccount

```solidity
function isIpAccount(contract IIPAccountRegistry ipAccountRegistry_, address ipAccountAddress_) external view returns (bool)
```

Checks if the given address is a valid IP Account.

#### Parameters

| Name                | Type                        | Description                       |
| ------------------- | --------------------------- | --------------------------------- |
| ipAccountRegistry\_ | contract IIPAccountRegistry | The IP Account registry contract. |
| ipAccountAddress\_  | address                     | The address to check.             |

#### Return Values

| Name | Type | Description                                                 |
| ---- | ---- | ----------------------------------------------------------- |
| \[0] | bool | True if the address is a valid IP Account, false otherwise. |
