---
title: AccessController.sol
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
This contract is used to control access permissions for different function calls in the protocol.\
It allows setting permissions for specific function calls, checking permissions, and initializing the contract.\
The contract uses a mapping to store policies, which are represented as a nested mapping structure.\
The contract also interacts with other contracts such as IIPAccountRegistry, IModuleRegistry, and IIPAccount.

Each policy is represented as a mapping from an IP account address to a signer address to a recipient\
address to a function selector to a permission level.\
The permission level can be 0 (ABSTAIN), 1 (ALLOW), or 2 (DENY).

The contract includes the following functions:

* initialize: Sets the addresses of the IP account registry and the module registry.
* setPermission: Sets the permission for a specific function call.
* getPermission: Returns the permission level for a specific function call.
* checkPermission: Checks if a specific function call is allowed.\_

### IP\_ACCOUNT\_REGISTRY

```solidity
address IP_ACCOUNT_REGISTRY
```

### MODULE\_REGISTRY

```solidity
address MODULE_REGISTRY
```

### encodedPermissions

```solidity
mapping(bytes32 => uint8) encodedPermissions
```

*Tracks the permission granted to an encoded permission path, where the\
encoded permission path = keccak256(abi.encodePacked(ipAccount, signer, to, func))*

### constructor

```solidity
constructor(address governance) public
```

### initialize

```solidity
function initialize(address ipAccountRegistry, address moduleRegistry) external
```

*Initialize the Access Controller with the IP Account Registry and Module Registry addresses.\
These are separated from the constructor, because we need to deploy the AccessController first for\
to deploy many registry and module contracts, including the IP Account Registry and Module Registry.\
Enforced to be only callable by the protocol admin in governance.*

#### Parameters

| Name              | Type    | Description                             |
| ----------------- | ------- | --------------------------------------- |
| ipAccountRegistry | address | The address of the IP Account Registry. |
| moduleRegistry    | address | The address of the Module Registry.     |

### setBatchPermissions

```solidity
function setBatchPermissions(struct AccessPermission.Permission[] permissions) external
```

Sets a batch of permissions in a single transaction.

*This function allows setting multiple permissions at once. Pausable.*

#### Parameters

| Name        | Type                                  | Description                                                                   |
| ----------- | ------------------------------------- | ----------------------------------------------------------------------------- |
| permissions | struct AccessPermission.Permission\[] | An array of `Permission` structs, each representing the permission to be set. |

### setGlobalPermission

```solidity
function setGlobalPermission(address signer, address to, bytes4 func, uint8 permission) external
```

Sets the permission for all IPAccounts

*Enforced to be only callable by the protocol admin in governance.*

#### Parameters

| Name       | Type    | Description                                                                                   |
| ---------- | ------- | --------------------------------------------------------------------------------------------- |
| signer     | address | The address that can call `to` on behalf of the IP account                                    |
| to         | address | The address that can be called by the `signer` (currently only modules can be `to`)           |
| func       | bytes4  | The function selector of `to` that can be called by the `signer` on behalf of the `ipAccount` |
| permission | uint8   | The new permission level                                                                      |

### setPermission

```solidity
function setPermission(address ipAccount, address signer, address to, bytes4 func, uint8 permission) public
```

Sets the permission for a specific function call

*Each policy is represented as a mapping from an IP account address to a signer address to a recipient\
address to a function selector to a permission level. The permission level can be 0 (ABSTAIN), 1 (ALLOW), or\
2 (DENY).\
By default, all policies are set to 0 (ABSTAIN), which means that the permission is not set.\
The owner of ipAccount by default has all permission.\
address(0) => wildcard\
bytes4(0) => wildcard\
Specific permission overrides wildcard permission.*

#### Parameters

| Name       | Type    | Description                                                                                   |
| ---------- | ------- | --------------------------------------------------------------------------------------------- |
| ipAccount  | address | The address of the IP account that grants the permission for `signer`                         |
| signer     | address | The address that can call `to` on behalf of the `ipAccount`                                   |
| to         | address | The address that can be called by the `signer` (currently only modules can be `to`)           |
| func       | bytes4  | The function selector of `to` that can be called by the `signer` on behalf of the `ipAccount` |
| permission | uint8   | The new permission level                                                                      |

### checkPermission

```solidity
function checkPermission(address ipAccount, address signer, address to, bytes4 func) external view
```

Checks the permission level for a specific function call. Reverts if permission is not granted.\
Otherwise, the function is a noop.

*This function checks the permission level for a specific function call.\
If a specific permission is set, it overrides the general (wildcard) permission.\
If the current level permission is ABSTAIN, the final permission is determined by the upper level.*

#### Parameters

| Name      | Type    | Description                                                                                   |
| --------- | ------- | --------------------------------------------------------------------------------------------- |
| ipAccount | address | The address of the IP account that grants the permission for `signer`                         |
| signer    | address | The address that can call `to` on behalf of the `ipAccount`                                   |
| to        | address | The address that can be called by the `signer` (currently only modules can be `to`)           |
| func      | bytes4  | The function selector of `to` that can be called by the `signer` on behalf of the `ipAccount` |

### getPermission

```solidity
function getPermission(address ipAccount, address signer, address to, bytes4 func) public view returns (uint8)
```

Returns the permission level for a specific function call.

#### Parameters

| Name      | Type    | Description                                                                                   |
| --------- | ------- | --------------------------------------------------------------------------------------------- |
| ipAccount | address | The address of the IP account that grants the permission for `signer`                         |
| signer    | address | The address that can call `to` on behalf of the `ipAccount`                                   |
| to        | address | The address that can be called by the `signer` (currently only modules can be `to`)           |
| func      | bytes4  | The function selector of `to` that can be called by the `signer` on behalf of the `ipAccount` |

#### Return Values

| Name | Type  | Description                                                                                           |
| ---- | ----- | ----------------------------------------------------------------------------------------------------- |
| \[0] | uint8 | permission The current permission level for the function call on `to` by the `signer` for `ipAccount` |

### \_setPermission

```solidity
function _setPermission(address ipAccount, address signer, address to, bytes4 func, uint8 permission) internal
```

*The permission parameters will be encoded into bytes32 as key in the permissions mapping to save storage*

### \_encodePermission

```solidity
function _encodePermission(address ipAccount, address signer, address to, bytes4 func) internal view returns (bytes32)
```

*encode permission to hash (bytes32)*
