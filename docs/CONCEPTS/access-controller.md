---
title: 🔒 Access Controller
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
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/ff607ff-Screenshot_2024-01-23_at_14.30.19.png",
        "",
        ""
      ],
      "align": "center"
    }
  ]
}
[/block]


Access Controller manages all permission-related states and permission checks in Story Protocol. In particular, it maintains the _Permission Table_ and _Permission Engine_ to process and store permissions. IPAccount permissions are set by the IPAccount owner.

## Permission Table

### Permission Record

| IPAccount  | Signer (caller) | To (only module) | Function Sig | Permission |
| ---------- | --------------- | ---------------- | ------------ | ---------- |
| 0x123..111 | 0x789..222      | 0x790..333       | 0xAaAaAaAa   | Allow      |
| 0x123..111 | 0x789..222      | 0x790..333       | 0xBBBBBBBB   | Deny       |
| 0x123..111 | 0x789..222      | 0x790..333       | 0xCCCCCC     | Abstain    |

Each record defines a permission in the form of the **Signer** (caller) calling the **Func** of the **To** (module) on behalf of the **IPAccount**.

The permission field can be set as "Allow," "Deny," or "Abstain." Abstain indicates that the permission decision is determined by the upper-level permission.

### Wildcard

Wildcard is also supported when defining permissions; it defines a permission that applies to multiple modules and/or functions.

With wildcards, users can easily define a whitelist or blacklist of permissions.

| IPAccount  | Signer (caller) | To (module) | Func | Permission |
| ---------- | --------------- | ----------- | ---- | ---------- |
| 0x123..111 | 0x789..222      | \*          | \*   | Allow      |
| 0x123..111 | 0x789..222      | 0x790..333  | \*   | Deny       |

The above example shows that the signer (0x789...) is unable to invoke any functions of the module (0x790...) on behalf of the IPAccount (0x123...).

In other words, the IPAccount has blacklisted the signer from calling any functions on the module 0x790...333

- Supported wildcards:

| Parameter                  | Wildcard   |
| -------------------------- | ---------- |
| Func                       | bytes4(0)  |
| Addresses (IPAccount / To) | address(0) |

### Permission Prioritization

Specific permissions override general permissions.

| IPAccount  | Signer (caller) | To (module) | Func       | Permission |
| ---------- | --------------- | ----------- | ---------- | ---------- |
| 0x123..111 | 0x789..222      | \*          | \*         | Allow      |
| 0x123..111 | 0x789..222      | 0x790..333  | \*         | Deny       |
| 0x123..111 | 0x789..222      | 0x790..333  | 0xCCCCDDDD | Allow      |

The above shows that the signer (0x789...) is not allowed to call any functions of the module (0x790...) on behalf of IPAccount (0x123...), except for the function 0xCCCCDDDD

Furthermore, the signer (0x789...) is permitted to call all other modules on behalf of IPAccount (0x123...).

<br />

## Call Flows with Access Control

There exist three types of call flows expected by the Access Controller.

1. An IPAccount calls a module directly.
2. A module calls another module directly.
3. A module calls a registry directly.

### IPAccount calling a Module directly

- IPAccount performs a permission check with the Access Controller.
- The module only needs to check if the `msg.sender` is a valid IPAccount.

When calling a module from an IPAccount, the IPAccount performs an access control check with AccessController to determine if the current caller has permission to make the call. In the module, it only needs to check whether the transaction `msg.sender` is a valid IPAccount.

`AccessControlled` provide a modifier `onlyIpAccount()` helps to perform the access control check.

```solidity Solidity
contract MockModule is IModule, AccessControlled {
    function action(string memory param) external view onlyIpAccount() returns (string memory) {
            // do something
    }
}
```

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/6a835ae-Screenshot_2024-01-22_at_17.18.49.png",
        "",
        ""
      ],
      "align": "center"
    }
  ]
}
[/block]


## Module calling another Module

- The callee module needs to perform the authorization check itself.

When a module is called directly from another module, it is responsible for performing the access control check using AccessController. This check determines whether the current caller has permission to make the call to the module.

`AccessControlled` provide a modifier `verifyPermission(address ipAccount)` helps to perform the access control check.

```coffeescript Solidity
contract MockModule is IModule, AccessControlled {
    function callFromAnotherModule(address ipAccount) external verifyPermission(ipAccount) returns (string memory) {
        if (!IAccessController(accessController).checkPermission(ipAccount, msg.sender, address(this), this.callFromAnotherModule.selector)) {
		        revert Unauthorized();
        }
			  // do something
    }
}
```

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/767f852-Screenshot_2024-01-22_at_17.19.07.png",
        "",
        ""
      ],
      "align": "center"
    }
  ]
}
[/block]


## Module calling Registry

- The registry performs the authorization check by calling AccessController.
- The registry authorizes modules through set global permission

When a registry is called by a module, it can perform the access control check using AccessController. This check determines whether the callee module has permission to call the registry.

```solidity Solidity
// called by StoryProtocl Admin
IAccessController(accessController).setGlobalPermission(address(0), address(module), address(registry), bytes4(0))) {

```

```solidity Solidity
contract MockRegistry {
    function registerAction() external returns (string memory) {
        if (!IAccessController(accessController).checkPermission(address(0), msg.sender, address(this), this.registerAction.selector)) {
		        revert Unauthorized();
        }
			  // do something
    }
}
```

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/3d24a42-Screenshot_2024-01-24_at_09.45.06.png",
        "",
        ""
      ],
      "align": "center"
    }
  ]
}
[/block]


> 📘 The IPAccount's permissions will be revoked upon transfer of ownership.
> 
> The permissions associated with the IPAccount are exclusively linked to its current owner. When the ownership of the IPAccount is transferred to a new individual, the existing permissions granted to the previous owner are automatically revoked. This ensures that only the current, legitimate owner has access to these permissions. If, in the future, the IPAccount ownership is transferred back to the original owner, the permissions that were initially revoked will be reinstated, restoring the original owner's access and control.