---
title: IPAccountRegistry.sol
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
This contract is responsible for managing the registration and tracking of IP Accounts.\
It leverages a public ERC6551 registry to deploy IPAccount contracts.

### IP\_ACCOUNT\_IMPL

```solidity
address IP_ACCOUNT_IMPL
```

Returns the IPAccount implementation address

### IP\_ACCOUNT\_SALT

```solidity
bytes32 IP_ACCOUNT_SALT
```

Returns the IPAccount salt

### ERC6551\_PUBLIC\_REGISTRY

```solidity
address ERC6551_PUBLIC_REGISTRY
```

Returns the public ERC6551 registry address

### constructor

```solidity
constructor(address erc6551Registry, address ipAccountImpl) public
```

### registerIpAccount

```solidity
function registerIpAccount(uint256 chainId, address tokenContract, uint256 tokenId) public returns (address ipAccountAddress)
```

Deploys an IPAccount contract with the IPAccount implementation and returns the address of the new IP

*The IPAccount deployment deltegates to public ERC6551 Registry*

#### Parameters

| Name          | Type    | Description                                                            |
| ------------- | ------- | ---------------------------------------------------------------------- |
| chainId       | uint256 | The chain ID where the IP Account will be created                      |
| tokenContract | address | The address of the token contract to be associated with the IP Account |
| tokenId       | uint256 | The ID of the token to be associated with the IP Account               |

#### Return Values

| Name             | Type    | Description                                 |
| ---------------- | ------- | ------------------------------------------- |
| ipAccountAddress | address | The address of the newly created IP Account |

### ipAccount

```solidity
function ipAccount(uint256 chainId, address tokenContract, uint256 tokenId) public view returns (address)
```

Returns the IPAccount address for the given NFT token.

#### Parameters

| Name          | Type    | Description                                                      |
| ------------- | ------- | ---------------------------------------------------------------- |
| chainId       | uint256 | The chain ID where the IP Account is located                     |
| tokenContract | address | The address of the token contract associated with the IP Account |
| tokenId       | uint256 | The ID of the token associated with the IP Account               |

#### Return Values

| Name | Type    | Description                                                                        |
| ---- | ------- | ---------------------------------------------------------------------------------- |
| \[0] | address | ipAccountAddress The address of the IP Account associated with the given NFT token |

### getIPAccountImpl

```solidity
function getIPAccountImpl() external view returns (address)
```

Returns the IPAccount implementation address.

#### Return Values

| Name | Type    | Description                                 |
| ---- | ------- | ------------------------------------------- |
| \[0] | address | The address of the IPAccount implementation |

### \_get6551AccountAddress

```solidity
function _get6551AccountAddress(uint256 chainId, address tokenContract, uint256 tokenId) internal view returns (address)
```

*Helper function to get the IPAccount address from the ERC6551 registry.*

### \_get6551AccountAddress

```solidity
function _get6551AccountAddress(uint256 chainId_, address tokenContract_, uint256 tokenId_) internal view returns (address)
```
