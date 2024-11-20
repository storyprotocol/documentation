---
title: Permissions
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
## PermissionClient

### Methods

- setPermission
- createSetPermissionSignature
- setAllPermissions
- setBatchPermissions
- createBatchPermissionSignature

### setPermission

Sets the permission for a specific function call.

Each policy is represented as a mapping from an IP account address to a signer address to a recipient  
address to a function selector to a permission level. The permission level can be 0 (ABSTAIN), 1 (ALLOW), or  
2 (DENY).

By default, all policies are set to 0 (ABSTAIN), which means that the permission is not set. The owner of IP Account by default has all permission.

| Method          | Type                                                                  |
| --------------- | --------------------------------------------------------------------- |
| `setPermission` | `(request: SetPermissionsRequest) => Promise<SetPermissionsResponse>` |

Parameters:

- `request.ipId`: The IP ID that grants the permission for `signer`.
- `request.signer`: The address that can call `to` on behalf of the `ipAccount`.
- `request.to`: The address that can be called by the `signer` (currently only modules can be `to`)
- `request.permission`: The new permission level.
- `request.func`: [Optional] The function selector string of `to` that can be called by the `signer` on behalf of the `ipAccount`. Be default, it allows all functions.
- `request.txOptions`: [Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type SetPermissionsResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  success?: boolean;
};
```

### createSetPermissionSignature

Specific permission overrides wildcard permission with signature.

| Method                         | Type                                                                                |
| ------------------------------ | ----------------------------------------------------------------------------------- |
| `createSetPermissionSignature` | `(request: CreateSetPermissionSignatureRequest) => Promise<SetPermissionsResponse>` |

Parameters:

- `request.ipId`: The IP ID that grants the permission for `signer`.
- `request.signer`: The address that can call `to` on behalf of the `ipAccount`.
- `request.to`: The address that can be called by the `signer` (currently only modules can be `to`)
- `request.permission`: The new permission level.
- `request.func`: [Optional] The function selector string of `to` that can be called by the `signer` on behalf of the `ipAccount`. Be default, it allows all functions.
- `request.deadline`: [Optional] The deadline for the signature in milliseconds, default is 1000ms.
- `request.txOptions`: [Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type SetPermissionsResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  success?: boolean;
};
```

### setAllPermissions

Sets permission to a signer for all functions across all modules.

| Method              | Type                                                                     |
| ------------------- | ------------------------------------------------------------------------ |
| `setAllPermissions` | `(request: SetAllPermissionsRequest) => Promise<SetPermissionsResponse>` |

Parameters:

- `request.ipId`: The IP ID that grants the permission for `signer`.
- `request.signer`: The address of the signer receiving the permissions.
- `request.permission`: The new permission.
- `request.txOptions`: [Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type SetPermissionsResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  success?: boolean;
};
```

### setBatchPermissions

Sets a batch of permissions in a single transaction.

| Method                | Type                                                                       |
| --------------------- | -------------------------------------------------------------------------- |
| `setBatchPermissions` | `(request: SetBatchPermissionsRequest) => Promise<SetPermissionsResponse>` |

Parameters:

- `request.permissions[]`: An array of `Permission` structure, each representing the permission to be set.
  - `request.permissions[].ipId`: The IP ID that grants the permission for `signer`.
  - `request.permissions[].signer`: The address that can call `to` on behalf of the `ipAccount`.
  - `request.permissions[].to`: The address that can be called by the `signer` (currently only modules can be `to`)
  - `request.permissions[].permission`: The new permission level.
  - `request.permissions[].func`: [Optional] The function selector string of `to` that can be called by the `signer` on behalf of the `ipAccount`. Be default, it allows all functions.
- `request.deadline`: [Optional] The deadline for the signature in milliseconds, default is 1000ms.
- `request.txOptions`: [Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type SetPermissionsResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  success?: boolean;
};
```

### createBatchPermissionSignature

Sets a batch of permissions in a single transaction with signature.

| Method                           | Type                                                                                  |
| -------------------------------- | ------------------------------------------------------------------------------------- |
| `createBatchPermissionSignature` | `(request: CreateBatchPermissionSignatureRequest) => Promise<SetPermissionsResponse>` |

Parameters:

- `request.ipId`: The IP ID that grants the permission for `signer`
- `request.permissions[]` - An array of `Permission` structure, each representing the permission to be set.
  - `request.permissions[].ipId`: The IP ID that grants the permission for `signer`.
  - `request.permissions[].signer`: The address that can call `to` on behalf of the `ipAccount`.
  - `request.permissions[].to`: The address that can be called by the `signer` (currently only modules can be `to`)
  - `request.permissions[].permission`: The new permission level.
  - `request.permissions[].func`: [Optional] The function selector string of `to` that can be called by the `signer` on behalf of the `ipAccount`. Be default, it allows all functions.
- `request.txOptions`: [Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type SetPermissionsResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  success?: boolean;
};
```