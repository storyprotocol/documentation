---
title: RoyaltyPolicyLAP.sol
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
Defines the logic for splitting royalties for a given ipId using a liquid absolute percentage mechanism

### LAPRoyaltyData

The state data of the LAP royalty policy

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |

```solidity
struct LAPRoyaltyData {
  bool isUnlinkableToParents;
  address splitClone;
  address ancestorsVault;
  uint32 royaltyStack;
  bytes32 ancestorsHash;
}
```

### TOTAL\_RNFT\_SUPPLY

```solidity
uint32 TOTAL_RNFT_SUPPLY
```

Returns the percentage scale - 1000 rnfts represents 100%

### MAX\_PARENTS

```solidity
uint256 MAX_PARENTS
```

Returns the maximum number of parents

### MAX\_ANCESTORS

```solidity
uint256 MAX_ANCESTORS
```

Returns the maximum number of total ancestors.

*The IP derivative tree is limited to 14 ancestors, which represents 3 levels of a binary tree 14 = 2+4+8*

### ROYALTY\_MODULE

```solidity
address ROYALTY_MODULE
```

Returns the RoyaltyModule address

### LICENSING\_MODULE

```solidity
address LICENSING_MODULE
```

Returns the LicensingModule address

### LIQUID\_SPLIT\_FACTORY

```solidity
address LIQUID_SPLIT_FACTORY
```

Returns the 0xSplits LiquidSplitFactory address

### LIQUID\_SPLIT\_MAIN

```solidity
address LIQUID_SPLIT_MAIN
```

Returns the 0xSplits LiquidSplitMain address

### ANCESTORS\_VAULT\_IMPL

```solidity
address ANCESTORS_VAULT_IMPL
```

Returns the Ancestors Vault Implementation address

### royaltyData

```solidity
mapping(address => struct RoyaltyPolicyLAP.LAPRoyaltyData) royaltyData
```

Returns the royalty data for a given IP asset

### onlyRoyaltyModule

```solidity
modifier onlyRoyaltyModule()
```

*Restricts the calls to the royalty module*

### constructor

```solidity
constructor(address royaltyModule, address licensingModule, address liquidSplitFactory, address liquidSplitMain, address governance) public
```

### receive

```solidity
receive() external payable
```

### setAncestorsVaultImplementation

```solidity
function setAncestorsVaultImplementation(address ancestorsVaultImpl) external
```

*Set the ancestors vault implementation address\
Enforced to be only callable by the protocol admin in governance*

#### Parameters

| Name               | Type    | Description                                |
| ------------------ | ------- | ------------------------------------------ |
| ancestorsVaultImpl | address | The ancestors vault implementation address |

### onLicenseMinting

```solidity
function onLicenseMinting(address ipId, bytes licenseData, bytes externalData) external
```

Executes royalty related logic on minting a license

*Enforced to be only callable by RoyaltyModule*

#### Parameters

| Name         | Type    | Description                                         |
| ------------ | ------- | --------------------------------------------------- |
| ipId         | address | The ipId whose license is being minted (licensor)   |
| licenseData  | bytes   | The license data custom to each the royalty policy  |
| externalData | bytes   | The external data custom to each the royalty policy |

### onLinkToParents

```solidity
function onLinkToParents(address ipId, address[] parentIpIds, bytes[] licenseData, bytes externalData) external
```

Executes royalty related logic on linking to parents

*Enforced to be only callable by RoyaltyModule*

#### Parameters

| Name         | Type       | Description                                                |
| ------------ | ---------- | ---------------------------------------------------------- |
| ipId         | address    | The children ipId that is being linked to parents          |
| parentIpIds  | address\[] | The parent ipIds that the children ipId is being linked to |
| licenseData  | bytes\[]   | The license data custom to each the royalty policy         |
| externalData | bytes      | The external data custom to each the royalty policy        |

### \_initPolicy

```solidity
function _initPolicy(address ipId, address[] parentIpIds, bytes[] licenseData, bytes externalData) internal
```

*Initializes the royalty policy for a given IP asset.\
Enforced to be only callable by RoyaltyModule*

#### Parameters

| Name         | Type       | Description                                                         |
| ------------ | ---------- | ------------------------------------------------------------------- |
| ipId         | address    | The to initialize the policy for                                    |
| parentIpIds  | address\[] | The parent ipIds that the children ipId is being linked to (if any) |
| licenseData  | bytes\[]   | The license data custom to each the royalty policy                  |
| externalData | bytes      | The external data custom to each the royalty policy                 |

### onRoyaltyPayment

```solidity
function onRoyaltyPayment(address caller, address ipId, address token, uint256 amount) external
```

Allows the caller to pay royalties to the given IP asset

#### Parameters

| Name   | Type    | Description                                                      |
| ------ | ------- | ---------------------------------------------------------------- |
| caller | address | The caller is the address from which funds will transferred from |
| ipId   | address | The ipId of the receiver of the royalties                        |
| token  | address | The token to pay                                                 |
| amount | uint256 | The amount to pay                                                |

### distributeIpPoolFunds

```solidity
function distributeIpPoolFunds(address ipId, address token, address[] accounts, address distributorAddress) external
```

Distributes funds internally so that accounts holding the royalty nfts at distribution moment can\
claim afterwards

*This call will revert if the caller holds all the royalty nfts of the ipId - in that case can call\
claimFromIpPoolAsTotalRnftOwner() instead*

#### Parameters

| Name               | Type       | Description                                       |
| ------------------ | ---------- | ------------------------------------------------- |
| ipId               | address    | The ipId whose received funds will be distributed |
| token              | address    | The token to distribute                           |
| accounts           | address\[] | The accounts to distribute to                     |
| distributorAddress | address    | The distributor address (if any)                  |

### claimFromIpPool

```solidity
function claimFromIpPool(address account, uint256 withdrawETH, contract ERC20[] tokens) external
```

Claims the available royalties for a given address

*If there are no funds available in split main contract but there are funds in the split clone contract\
then a distributeIpPoolFunds() call should precede this call*

#### Parameters

| Name        | Type              | Description                   |
| ----------- | ----------------- | ----------------------------- |
| account     | address           | The account to claim for      |
| withdrawETH | uint256           | The amount of ETH to withdraw |
| tokens      | contract ERC20\[] | The tokens to withdraw        |

### claimFromIpPoolAsTotalRnftOwner

```solidity
function claimFromIpPoolAsTotalRnftOwner(address ipId, uint256 withdrawETH, address token) external
```

Claims the available royalties for a given address that holds all the royalty nfts of an ipId

*This call will revert if the caller does not hold all the royalty nfts of the ipId*

#### Parameters

| Name        | Type    | Description                                       |
| ----------- | ------- | ------------------------------------------------- |
| ipId        | address | The ipId whose received funds will be distributed |
| withdrawETH | uint256 | The amount of ETH to withdraw                     |
| token       | address | The token to withdraw                             |

### claimFromAncestorsVault

```solidity
function claimFromAncestorsVault(address ipId, address claimerIpId, address[] ancestors, uint32[] ancestorsRoyalties, bool withdrawETH, contract ERC20[] tokens) external
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

### \_checkAncestorsDataIsValid

```solidity
function _checkAncestorsDataIsValid(address[] parentIpIds, uint32[] parentRoyalties, struct IRoyaltyPolicyLAP.InitParams params) internal view returns (uint32)
```

*Checks if the ancestors data is valid*

#### Parameters

| Name            | Type                                | Description          |
| --------------- | ----------------------------------- | -------------------- |
| parentIpIds     | address\[]                          | The parent ipIds     |
| parentRoyalties | uint32\[]                           | The parent royalties |
| params          | struct IRoyaltyPolicyLAP.InitParams | The init params      |

#### Return Values

| Name | Type   | Description                           |
| ---- | ------ | ------------------------------------- |
| \[0] | uint32 | newRoyaltyStack The new royalty stack |

### \_getExpectedOutputs

```solidity
function _getExpectedOutputs(address[] parentIpIds, uint32[] parentRoyalties, struct IRoyaltyPolicyLAP.InitParams params) internal view returns (address[] newAncestors, uint32[] newAncestorsRoyalty, uint32 ancestorsCount, uint32 royaltyStack)
```

*Gets the expected outputs for the ancestors and ancestors royalties*

#### Parameters

| Name            | Type                                | Description          |
| --------------- | ----------------------------------- | -------------------- |
| parentIpIds     | address\[]                          | The parent ipIds     |
| parentRoyalties | uint32\[]                           | The parent royalties |
| params          | struct IRoyaltyPolicyLAP.InitParams | The init params      |

#### Return Values

| Name                | Type       | Description               |
| ------------------- | ---------- | ------------------------- |
| newAncestors        | address\[] | The new ancestors         |
| newAncestorsRoyalty | uint32\[]  | The new ancestors royalty |
| ancestorsCount      | uint32     | The number of ancestors   |
| royaltyStack        | uint32     | The royalty stack         |

### \_deploySplitClone

```solidity
function _deploySplitClone(address ipId, address ancestorsVault, uint32 royaltyStack) internal returns (address)
```

*Deploys a liquid split clone contract*

#### Parameters

| Name           | Type    | Description                                                                      |
| -------------- | ------- | -------------------------------------------------------------------------------- |
| ipId           | address | The ipId                                                                         |
| ancestorsVault | address | The ancestors vault address                                                      |
| royaltyStack   | uint32  | The number of rnfts that the ipId has to give to its parents and/or grandparents |

#### Return Values

| Name | Type    | Description                                             |
| ---- | ------- | ------------------------------------------------------- |
| \[0] | address | The address of the deployed liquid split clone contract |

### \_safeTransferETH

```solidity
function _safeTransferETH(address to, uint256 amount) internal
```

*Allows to transfers ETH*

#### Parameters

| Name   | Type    | Description                |
| ------ | ------- | -------------------------- |
| to     | address | The address to transfer to |
| amount | uint256 | The amount to transfer     |
