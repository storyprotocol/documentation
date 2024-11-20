---
title: AccessPermission.sol
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
Library for IPAccount access control permissions. These permissions are used by the AccessController.

### ABSTAIN

```solidity
uint8 ABSTAIN
```

ABSTAIN means having not enough information to make decision at current level, deferred decision to up  
level permission.

### ALLOW

```solidity
uint8 ALLOW
```

ALLOW means the permission is granted to transaction signer to call the function.

### DENY

```solidity
uint8 DENY
```

DENY means the permission is denied to transaction signer to call the function.

### Permission

This struct is used to represent permissions in batch operations within the AccessController.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |

```solidity
struct Permission {
  address ipAccount;
  address signer;
  address to;
  bytes4 func;
  uint8 permission;
}
```