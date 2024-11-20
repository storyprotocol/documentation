---
title: AccessControlled.sol
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
Provides a base contract for access control functionalities.

_This abstract contract implements basic access control mechanisms with an emphasis  
on IP account verification and permission checks.  
It is designed to be used as a base contract for other contracts that require access control.  
It provides modifiers and functions to verify if the caller has the necessary permission  
and is a registered IP account._

### ACCESS_CONTROLLER

```solidity
contract IAccessController ACCESS_CONTROLLER
```

The IAccessController instance for permission checks.

### IP_ACCOUNT_REGISTRY

```solidity
contract IIPAccountRegistry IP_ACCOUNT_REGISTRY
```

The IIPAccountRegistry instance for IP account verification.

### constructor

```solidity
constructor(address accessController, address ipAccountRegistry) internal
```

_Initializes the contract by setting the ACCESS_CONTROLLER and IP_ACCOUNT_REGISTRY addresses._

#### Parameters

| Name              | Type    | Description                                    |
| ----------------- | ------- | ---------------------------------------------- |
| accessController  | address | The address of the AccessController contract.  |
| ipAccountRegistry | address | The address of the IPAccountRegistry contract. |

### verifyPermission

```solidity
modifier verifyPermission(address ipAccount)
```

Verifies that the caller has the necessary permission for the given IPAccount.

_Modifier that calls \_verifyPermission to check if the provided IP account has the required permission.  
modules can use this modifier to check if the caller has the necessary permission._

#### Parameters

| Name      | Type    | Description                              |
| --------- | ------- | ---------------------------------------- |
| ipAccount | address | The address of the IP account to verify. |

### onlyIpAccount

```solidity
modifier onlyIpAccount()
```

Ensures that the caller is a registered IP account.

_Modifier that checks if the msg.sender is a registered IP account.  
modules can use this modifier to check if the caller is a registered IP account.  
so that enforce only registered IP Account can call the functions._

### \_verifyPermission

```solidity
function _verifyPermission(address ipAccount) internal view
```

_Internal function to verify if the caller (msg.sender) has the required permission to execute  
the function on provided ipAccount._

#### Parameters

| Name      | Type    | Description                              |
| --------- | ------- | ---------------------------------------- |
| ipAccount | address | The address of the IP account to verify. |

### \_hasPermission

```solidity
function _hasPermission(address ipAccount) internal view returns (bool)
```

_Internal function to check if the caller (msg.sender) has the required permission to execute  
the function on provided ipAccount, returning a boolean._

#### Parameters

| Name      | Type    | Description                             |
| --------- | ------- | --------------------------------------- |
| ipAccount | address | The address of the IP account to check. |

#### Return Values

| Name | Type | Description                                                      |
| ---- | ---- | ---------------------------------------------------------------- |
| [0]  | bool | bool Returns true if the caller has permission, false otherwise. |