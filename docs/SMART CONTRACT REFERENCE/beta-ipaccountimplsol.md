---
title: IPAccountImpl.sol
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
[Git Source](https://github.com/storyprotocol/protocol-core-v1/blob/226cf1c3d7204142a7f8a7c76711c31e636c76ce/contracts/IPAccountImpl.sol)

<br />

The Story Protocol's implementation of the IPAccount.

## State Variables

### ACCESS\_CONTROLLER

```solidity
address public immutable ACCESS_CONTROLLER;
```

## Functions

### receive

```solidity
receive() external payable override(Receiver, IIPAccount);
```

### constructor

Creates a new IPAccountImpl contract instance

*Initializes the IPAccountImpl with an AccessController address which is stored\
in the implementation code's storage.\
This means that each cloned IPAccount will inherently use the same AccessController\
without the need for individual configuration.*

```solidity
constructor(address accessController, address ipAssetRegistry, address licenseRegistry, address moduleRegistry)
    IPAccountStorage(ipAssetRegistry, licenseRegistry, moduleRegistry);
```

**Parameters**

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
        `accessController`
      </td>

      <td>
        `address`
      </td>

      <td>
        The address of the AccessController contract to be used for permission checks
      </td>
    </tr>

    <tr>
      <td>
        `ipAssetRegistry`
      </td>

      <td>
        `address`
      </td>

      <td>

      </td>
    </tr>

    <tr>
      <td>
        `licenseRegistry`
      </td>

      <td>
        `address`
      </td>

      <td>

      </td>
    </tr>

    <tr>
      <td>
        `moduleRegistry`
      </td>

      <td>
        `address`
      </td>

      <td>

      </td>
    </tr>
  </tbody>
</Table>

### supportsInterface

Checks if the contract supports a specific interface

```solidity
function supportsInterface(bytes4 interfaceId)
    public
    view
    override(ERC6551, IPAccountStorage, IERC165)
    returns (bool);
```

**Parameters**

| Name          | Type     | Description                                       |
| ------------- | -------- | ------------------------------------------------- |
| `interfaceId` | `bytes4` | The interface identifier, as specified in ERC-165 |

**Returns**

| Name     | Type   | Description                                                          |
| -------- | ------ | -------------------------------------------------------------------- |
| `<none>` | `bool` | bool is true if the contract supports the interface, false otherwise |

### token

Returns the identifier of the non-fungible token which owns the account

```solidity
function token() public view override(ERC6551, IIPAccount) returns (uint256, address, uint256);
```

**Returns**

| Name     | Type      | Description                                             |
| -------- | --------- | ------------------------------------------------------- |
| `<none>` | `uint256` | chainId The EIP-155 ID of the chain the token exists on |
| `<none>` | `address` | tokenContract The contract address of the token         |
| `<none>` | `uint256` | tokenId The ID of the token                             |

### isValidSigner

Checks if the signer is valid for executing specific actions on behalf of the IP Account.

```solidity
function isValidSigner(address signer, bytes calldata data)
    public
    view
    override(ERC6551, IIPAccount)
    returns (bytes4 result);
```

**Parameters**

| Name     | Type      | Description                                                                                                                                                                                                                                                                                              |
| -------- | --------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `signer` | `address` | The signer to check                                                                                                                                                                                                                                                                                      |
| `data`   | `bytes`   | The data to be checked. The data should be encoded as `abi.encode(address to, bytes calldata)`, where `address to` is the recipient and `bytes calldata` is the calldata passed to the recipient. If `data.length == 0`, it is also considered valid, implying that the signer is valid for all actions. |

**Returns**

| Name     | Type     | Description                                               |
| -------- | -------- | --------------------------------------------------------- |
| `result` | `bytes4` | The function selector if the signer is valid, 0 otherwise |

### owner

Returns the owner of the IP Account.

```solidity
function owner() public view override(ERC6551, IIPAccount) returns (address);
```

**Returns**

| Name     | Type      | Description               |
| -------- | --------- | ------------------------- |
| `<none>` | `address` | The address of the owner. |

### state

Returns the IPAccount's internal nonce for transaction ordering.

```solidity
function state() public view override(ERC6551, IIPAccount) returns (bytes32 result);
```

### isValidSigner

*Checks if the signer is valid for the given data and recipient via the AccessController permission system.*

```solidity
function isValidSigner(address signer, address to, bytes calldata data) public view returns (bool);
```

**Parameters**

| Name     | Type      | Description                      |
| -------- | --------- | -------------------------------- |
| `signer` | `address` | The signer to check              |
| `to`     | `address` | The recipient of the transaction |
| `data`   | `bytes`   | The calldata to check against    |

**Returns**

| Name     | Type   | Description                                          |
| -------- | ------ | ---------------------------------------------------- |
| `<none>` | `bool` | bool is true if the signer is valid, false otherwise |

### executeWithSig

Executes a transaction from the IP Account on behalf of the signer.

```solidity
function executeWithSig(
    address to,
    uint256 value,
    bytes calldata data,
    address signer,
    uint256 deadline,
    bytes calldata signature
) external payable returns (bytes memory result);
```

**Parameters**

| Name        | Type      | Description                                        |
| ----------- | --------- | -------------------------------------------------- |
| `to`        | `address` | The recipient of the transaction.                  |
| `value`     | `uint256` | The amount of Ether to send.                       |
| `data`      | `bytes`   | The data to send along with the transaction.       |
| `signer`    | `address` | The signer of the transaction.                     |
| `deadline`  | `uint256` | The deadline of the transaction signature.         |
| `signature` | `bytes`   | The signature of the transaction, EIP-712 encoded. |

### execute

Executes a transaction from the IP Account.

```solidity
function execute(address to, uint256 value, bytes calldata data) external payable returns (bytes memory result);
```

**Parameters**

| Name    | Type      | Description                                  |
| ------- | --------- | -------------------------------------------- |
| `to`    | `address` | The recipient of the transaction.            |
| `value` | `uint256` | The amount of Ether to send.                 |
| `data`  | `bytes`   | The data to send along with the transaction. |

**Returns**

| Name     | Type    | Description                           |
| -------- | ------- | ------------------------------------- |
| `result` | `bytes` | The return data from the transaction. |

### execute

*Override 6551 execute function.\
Only "CALL" operation is supported.*

```solidity
function execute(address to, uint256 value, bytes calldata data, uint8 operation)
    public
    payable
    override
    returns (bytes memory result);
```

**Parameters**

| Name        | Type      | Description                                                |
| ----------- | --------- | ---------------------------------------------------------- |
| `to`        | `address` | The recipient of the transaction.                          |
| `value`     | `uint256` | The amount of Ether to send.                               |
| `data`      | `bytes`   | The data to send along with the transaction.               |
| `operation` | `uint8`   | The operation type to perform, only 0 - CALL is supported. |

**Returns**

| Name     | Type    | Description                           |
| -------- | ------- | ------------------------------------- |
| `result` | `bytes` | The return data from the transaction. |

### executeBatch

Executes a batch of transactions from the IP Account.

```solidity
function executeBatch(Call[] calldata calls, uint8 operation)
    public
    payable
    override
    returns (bytes[] memory results);
```

**Parameters**

| Name        | Type     | Description                                                |
| ----------- | -------- | ---------------------------------------------------------- |
| `calls`     | `Call[]` | The array of calls to execute.                             |
| `operation` | `uint8`  | The operation type to perform, only 0 - CALL is supported. |

**Returns**

| Name      | Type      | Description                            |
| --------- | --------- | -------------------------------------- |
| `results` | `bytes[]` | The return data from the transactions. |

### \_execute

*Executes a transaction from the IP Account.*

```solidity
function _execute(address signer, address to, uint256 value, bytes calldata data)
    internal
    returns (bytes memory result);
```

### \_updateStateForExecute

*Updates the IP Account's state all execute transactions.*

```solidity
function _updateStateForExecute(address to, uint256 value, bytes calldata data) internal;
```

**Parameters**

| Name    | Type      | Description                                  |
| ------- | --------- | -------------------------------------------- |
| `to`    | `address` | The "target" of the execute transactions.    |
| `value` | `uint256` | The amount of Ether to send.                 |
| `data`  | `bytes`   | The data to send along with the transaction. |

### \_isValidSigner

*Override Solady 6551\_isValidSigner function.*

```solidity
function _isValidSigner(address signer, bytes32 extraData, bytes calldata context)
    internal
    view
    override
    returns (bool);
```

**Parameters**

| Name        | Type      | Description                                                                             |
| ----------- | --------- | --------------------------------------------------------------------------------------- |
| `signer`    | `address` | The signer to check                                                                     |
| `extraData` | `bytes32` | The extra data to check against, it should bethe address of the recipient for IPAccount |
| `context`   | `bytes`   | The context for validating the signer                                                   |

**Returns**

| Name     | Type   | Description                                          |
| -------- | ------ | ---------------------------------------------------- |
| `<none>` | `bool` | bool is true if the signer is valid, false otherwise |

### \_domainNameAndVersion

*Override Solady EIP712 function and return EIP712 domain name for IPAccount.*

```solidity
function _domainNameAndVersion() internal view override returns (string memory name, string memory version);
```

```solidity
function onERC1155BatchReceived(address, address, uint256[], uint256[], bytes) public pure returns (bytes4)
```

### \_execute

```solidity
function _execute(address signer, address to, uint256 value, bytes data) internal returns (bytes result)
```
