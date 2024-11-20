---
title: AncestorVaultLAP.sol
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
The ancestors vault allows parents and grandparents to claim their share of  
        the royalty nfts of their children and grandchildren along with any accrued royalties.

### ROYALTY_POLICY_LAP

```solidity
contract IRoyaltyPolicyLAP ROYALTY_POLICY_LAP
```

The liquid split royalty policy address

### isClaimed

```solidity
mapping(address => mapping(address => bool)) isClaimed
```

Indicates if a given ancestor address has already claimed

### constructor

```solidity
constructor(address royaltyPolicyLAP) public
```

### claim

```solidity
function claim(address ipId, address claimerIpId, address[] ancestors, uint32[] ancestorsRoyalties, bool withdrawETH, contract ERC20[] tokens) external
```

Claims all available royalty nfts and accrued royalties for an ancestor of a given ipId

#### Parameters

| Name               | Type              | Description                                                  |
| ------------------ | ----------------- | ------------------------------------------------------------ |
| ipId               | address           | The ipId of the ancestors vault to claim from                |
| claimerIpId        | address           | The claimer ipId is the ancestor address that wants to claim |
| ancestors          | address\[]        | The ancestors for the selected ipId                          |
| ancestorsRoyalties | uint32\[]         | The royalties of the ancestors for the selected ipId         |
| withdrawETH        | bool              | Indicates if the claimer wants to withdraw ETH               |
| tokens             | contract ERC20\[] | The ERC20 tokens to withdraw                                 |

### \_transferRnftsAndAccruedTokens

```solidity
function _transferRnftsAndAccruedTokens(address claimerIpId, address splitClone, address[] ancestors, uint32[] ancestorsRoyalties, bool withdrawETH, contract ERC20[] tokens) internal
```

_Transfers the Royalty NFTs and accrued tokens to the claimer_

#### Parameters

| Name               | Type              | Description                                    |
| ------------------ | ----------------- | ---------------------------------------------- |
| claimerIpId        | address           | The claimer ipId                               |
| splitClone         | address           | The split clone address                        |
| ancestors          | address\[]        | The ancestors of the IP                        |
| ancestorsRoyalties | uint32\[]         | The royalties of each of the ancestors         |
| withdrawETH        | bool              | Indicates if the claimer wants to withdraw ETH |
| tokens             | contract ERC20\[] | The ERC20 tokens to withdraw                   |

### \_claimAccruedTokens

```solidity
function _claimAccruedTokens(uint256 rnftClaimAmount, uint256 totalUnclaimedRnfts, address claimerSplitClone, bool withdrawETH, contract ERC20[] tokens) internal
```

_Claims the accrued tokens (if any)_

#### Parameters

| Name                | Type              | Description                                    |
| ------------------- | ----------------- | ---------------------------------------------- |
| rnftClaimAmount     | uint256           | The amount of rnfts to claim                   |
| totalUnclaimedRnfts | uint256           | The total unclaimed rnfts                      |
| claimerSplitClone   | address           | The claimer's split clone                      |
| withdrawETH         | bool              | Indicates if the claimer wants to withdraw ETH |
| tokens              | contract ERC20\[] | The ERC20 tokens to withdraw                   |

### \_safeTransferETH

```solidity
function _safeTransferETH(address to, uint256 amount) internal
```

_Allows to transfers ETH_

#### Parameters

| Name   | Type    | Description                |
| ------ | ------- | -------------------------- |
| to     | address | The address to transfer to |
| amount | uint256 | The amount to transfer     |