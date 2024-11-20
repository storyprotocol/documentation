---
title: TokenWithdrawalModule
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
Module for transferring ERC20, ERC721, and ERC1155 tokens for IP Accounts.

_SECURITY RISK: An IPAccount can delegate to a frontend contract (not a registered module) to transfer tokens  
on behalf of the IPAccount via the Token Management Module. This frontend contract can transfer any tokens that are  
approved by the IPAccount for the Token Management Module. In other words, there's no mechanism for this module to  
granularly control which token a caller (approved contract in this case) can transfer._

### name

```solidity
string name
```

Returns the string identifier associated with the module.

### constructor

```solidity
constructor(address accessController, address ipAccountRegistry) public
```

### withdrawERC20

```solidity
function withdrawERC20(address payable ipAccount, address tokenContract, uint256 amount) external
```

Withdraws ERC20 token from the IP account to the IP account owner.

_When calling this function, the caller must have the permission to call `transfer` via the IP account.  
Does not support transfer of multiple tokens at once._

#### Parameters

| Name          | Type            | Description                                     |
| ------------- | --------------- | ----------------------------------------------- |
| ipAccount     | address payable | The IP account to transfer the ERC20 token from |
| tokenContract | address         | The address of the ERC20 token contract         |
| amount        | uint256         | The amount of token to transfer                 |

### withdrawERC721

```solidity
function withdrawERC721(address payable ipAccount, address tokenContract, uint256 tokenId) external
```

Withdraws ERC721 token from the IP account to the IP account owner.

_When calling this function, the caller must have the permission to call `transferFrom` via the IP account.  
Does not support batch transfers._

#### Parameters

| Name          | Type            | Description                                      |
| ------------- | --------------- | ------------------------------------------------ |
| ipAccount     | address payable | The IP account to transfer the ERC721 token from |
| tokenContract | address         | The address of the ERC721 token contract         |
| tokenId       | uint256         | The ID of the token to transfer                  |

### withdrawERC1155

```solidity
function withdrawERC1155(address payable ipAccount, address tokenContract, uint256 tokenId, uint256 amount) external
```

Withdraws ERC1155 token from the IP account to the IP account owner.

_When calling this function, the caller must have the permission to call `safeTransferFrom` via the IP  
account.  
Does not support batch transfers._

#### Parameters

| Name          | Type            | Description                                       |
| ------------- | --------------- | ------------------------------------------------- |
| ipAccount     | address payable | The IP account to transfer the ERC1155 token from |
| tokenContract | address         | The address of the ERC1155 token contract         |
| tokenId       | uint256         | The ID of the token to transfer                   |
| amount        | uint256         | The amount of token to transfer                   |