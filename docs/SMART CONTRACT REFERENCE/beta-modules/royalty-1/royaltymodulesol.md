---
title: RoyaltyModule.sol
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
The Story Protocol royalty module allows to set royalty policies an IP asset and pay royalties as a\
        derivative IP.

### name

```solidity
string name
```

Returns the string identifier associated with the module.

### LICENSING\_MODULE

```solidity
address LICENSING_MODULE
```

Returns the licensing module address

### isWhitelistedRoyaltyPolicy

```solidity
mapping(address => bool) isWhitelistedRoyaltyPolicy
```

Indicates if a royalty policy is whitelisted

### isWhitelistedRoyaltyToken

```solidity
mapping(address => bool) isWhitelistedRoyaltyToken
```

Indicates if a royalty token is whitelisted

### royaltyPolicies

```solidity
mapping(address => address) royaltyPolicies
```

Indicates the royalty policy for a given IP asset

### constructor

```solidity
constructor(address governance) public
```

### onlyLicensingModule

```solidity
modifier onlyLicensingModule()
```

Modifier to enforce that the caller is the licensing module

### setLicensingModule

```solidity
function setLicensingModule(address licensingModule) external
```

Sets the license registry

*Enforced to be only callable by the protocol admin*

#### Parameters

| Name            | Type    | Description                         |
| --------------- | ------- | ----------------------------------- |
| licensingModule | address | The address of the license registry |

### whitelistRoyaltyPolicy

```solidity
function whitelistRoyaltyPolicy(address royaltyPolicy, bool allowed) external
```

Whitelist a royalty policy

*Enforced to be only callable by the protocol admin*

#### Parameters

| Name          | Type    | Description                                           |
| ------------- | ------- | ----------------------------------------------------- |
| royaltyPolicy | address | The address of the royalty policy                     |
| allowed       | bool    | Indicates if the royalty policy is whitelisted or not |

### whitelistRoyaltyToken

```solidity
function whitelistRoyaltyToken(address token, bool allowed) external
```

Whitelist a royalty token

*Enforced to be only callable by the protocol admin*

#### Parameters

| Name    | Type    | Description                                  |
| ------- | ------- | -------------------------------------------- |
| token   | address | The token address                            |
| allowed | bool    | Indicates if the token is whitelisted or not |

### onLicenseMinting

```solidity
function onLicenseMinting(address ipId, address royaltyPolicy, bytes licenseData, bytes externalData) external
```

Executes royalty related logic on license minting

*Enforced to be only callable by LicensingModule*

#### Parameters

| Name          | Type    | Description                                            |
| ------------- | ------- | ------------------------------------------------------ |
| ipId          | address | The ipId whose license is being minted (licensor)      |
| royaltyPolicy | address | The royalty policy address of the license being minted |
| licenseData   | bytes   | The license data custom to each the royalty policy     |
| externalData  | bytes   | The external data custom to each the royalty policy    |

### onLinkToParents

```solidity
function onLinkToParents(address ipId, address royaltyPolicy, address[] parentIpIds, bytes[] licenseData, bytes externalData) external
```

Executes royalty related logic on linking to parents

*Enforced to be only callable by LicensingModule*

#### Parameters

| Name          | Type       | Description                                                        |
| ------------- | ---------- | ------------------------------------------------------------------ |
| ipId          | address    | The children ipId that is being linked to parents                  |
| royaltyPolicy | address    | The common royalty policy address of all the licenses being burned |
| parentIpIds   | address\[] | The parent ipIds that the children ipId is being linked to         |
| licenseData   | bytes\[]   | The license data custom to each the royalty policy                 |
| externalData  | bytes      | The external data custom to each the royalty policy                |

### payRoyaltyOnBehalf

```solidity
function payRoyaltyOnBehalf(address receiverIpId, address payerIpId, address token, uint256 amount) external
```

Allows the function caller to pay royalties to the receiver IP asset on behalf of the payer IP asset.

#### Parameters

| Name         | Type    | Description                           |
| ------------ | ------- | ------------------------------------- |
| receiverIpId | address | The ipId that receives the royalties  |
| payerIpId    | address | The ipId that pays the royalties      |
| token        | address | The token to use to pay the royalties |
| amount       | uint256 | The amount to pay                     |

### payLicenseMintingFee

```solidity
function payLicenseMintingFee(address receiverIpId, address payerAddress, address licenseRoyaltyPolicy, address token, uint256 amount) external
```

Allows to pay the minting fee for a license

#### Parameters

| Name                 | Type    | Description                                    |
| -------------------- | ------- | ---------------------------------------------- |
| receiverIpId         | address | The ipId that receives the royalties           |
| payerAddress         | address | The address that pays the royalties            |
| licenseRoyaltyPolicy | address | The royalty policy of the license being minted |
| token                | address | The token to use to pay the royalties          |
| amount               | uint256 | The amount to pay                              |

### supportsInterface

```solidity
function supportsInterface(bytes4 interfaceId) public view virtual returns (bool)
```

IERC165 interface support.
