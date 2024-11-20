---
title: LicenseRegistry.sol
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
Registry of License NFTs, which represent licenses granted by IP ID licensors to create derivative IPs.

### name

```solidity
string name
```

_Name of the License NFT_

### symbol

```solidity
string symbol
```

_Symbol of the License NFT_

### LICENSING_MODULE

```solidity
contract ILicensingModule LICENSING_MODULE
```

Returns the canonical protocol-wide LicensingModule

### DISPUTE_MODULE

```solidity
contract IDisputeModule DISPUTE_MODULE
```

Returns the canonical protocol-wide DisputeModule

### onlyLicensingModule

```solidity
modifier onlyLicensingModule()
```

_We have to implement this modifier instead of inheriting `LicensingModuleAware` because LicensingModule  
constructor requires the licenseRegistry address, which would create a circular dependency. Thus, we use the  
function `setLicensingModule` to set the licensing module address after deploying the module._

### constructor

```solidity
constructor(address governance) public
```

### setDisputeModule

```solidity
function setDisputeModule(address newDisputeModule) external
```

_Sets the DisputeModule address.  
Enforced to be only callable by the protocol admin_

#### Parameters

| Name             | Type    | Description                      |
| ---------------- | ------- | -------------------------------- |
| newDisputeModule | address | The address of the DisputeModule |

### setLicensingModule

```solidity
function setLicensingModule(address newLicensingModule) external
```

_Sets the LicensingModule address.  
Enforced to be only callable by the protocol admin_

#### Parameters

| Name               | Type    | Description                        |
| ------------------ | ------- | ---------------------------------- |
| newLicensingModule | address | The address of the LicensingModule |

### mintLicense

```solidity
function mintLicense(uint256 policyId, address licensorIpId_, bool transferable, uint256 amount, address receiver) external returns (uint256 licenseId)
```

Mints license NFTs representing a policy granted by a set of ipIds (licensors). This NFT needs to be  
burned in order to link a derivative IP with its parents. If this is the first combination of policy and  
licensors, a new licenseId will be created. If not, the license is fungible and an id will be reused.

_Only callable by the licensing module._

#### Parameters

| Name           | Type    | Description                                                                            |
| -------------- | ------- | -------------------------------------------------------------------------------------- |
| policyId       | uint256 | The ID of the policy to be minted                                                      |
| licensorIpId\_ | address | The ID of the IP granting the license (ie. licensor)                                   |
| transferable   | bool    | True if the license is transferable                                                    |
| amount         | uint256 | Number of licenses to mint. License NFT is fungible for same policy and same licensors |
| receiver       | address | Receiver address of the minted license NFT(s).                                         |

#### Return Values

| Name      | Type    | Description                          |
| --------- | ------- | ------------------------------------ |
| licenseId | uint256 | The ID of the minted license NFT(s). |

### burnLicenses

```solidity
function burnLicenses(address holder, uint256[] licenseIds) external
```

Burns licenses

#### Parameters

| Name       | Type       | Description                         |
| ---------- | ---------- | ----------------------------------- |
| holder     | address    | The address that holds the licenses |
| licenseIds | uint256\[] | The ids of the licenses to burn     |

### mintedLicenses

```solidity
function mintedLicenses() external view returns (uint256)
```

Returns the number of licenses registered in the protocol.

_Token ID counter total count._

#### Return Values

| Name | Type    | Description                                  |
| ---- | ------- | -------------------------------------------- |
| [0]  | uint256 | mintedLicenses The number of minted licenses |

### isLicensee

```solidity
function isLicensee(uint256 licenseId, address holder) external view returns (bool)
```

Returns true if holder has positive balance for the given license ID.

#### Return Values

| Name | Type | Description                                                                                                                                                                     |
| ---- | ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [0]  | bool | isLicensee True if holder is the licensee for the license (owner of the license NFT), or derivative IP owner if the license was added to the IP by linking (burning a license). |

### license

```solidity
function license(uint256 licenseId) external view returns (struct Licensing.License)
```

Returns the license data for the given license ID

#### Parameters

| Name      | Type    | Description           |
| --------- | ------- | --------------------- |
| licenseId | uint256 | The ID of the license |

#### Return Values

| Name | Type                     | Description                  |
| ---- | ------------------------ | ---------------------------- |
| [0]  | struct Licensing.License | licenseData The license data |

### licensorIpId

```solidity
function licensorIpId(uint256 licenseId) external view returns (address)
```

Returns the ID of the IP asset that is the licensor of the given license ID

#### Parameters

| Name      | Type    | Description           |
| --------- | ------- | --------------------- |
| licenseId | uint256 | The ID of the license |

#### Return Values

| Name | Type    | Description                         |
| ---- | ------- | ----------------------------------- |
| [0]  | address | licensorIpId The ID of the licensor |

### policyIdForLicense

```solidity
function policyIdForLicense(uint256 licenseId) external view returns (uint256)
```

Returns the policy ID for the given license ID

#### Parameters

| Name      | Type    | Description           |
| --------- | ------- | --------------------- |
| licenseId | uint256 | The ID of the license |

#### Return Values

| Name | Type    | Description                   |
| ---- | ------- | ----------------------------- |
| [0]  | uint256 | policyId The ID of the policy |

### isLicenseRevoked

```solidity
function isLicenseRevoked(uint256 licenseId) public view returns (bool)
```

Returns true if the license has been revoked (licensor tagged after a dispute in  
the dispute module). If the tag is removed, the license is not revoked anymore.

#### Parameters

| Name      | Type    | Description                    |
| --------- | ------- | ------------------------------ |
| licenseId | uint256 | The id of the license to check |

#### Return Values

| Name | Type | Description                              |
| ---- | ---- | ---------------------------------------- |
| [0]  | bool | isRevoked True if the license is revoked |

### uri

```solidity
function uri(uint256 id) public view virtual returns (string)
```

ERC1155 OpenSea metadata JSON representation of the LNFT parameters

_Expect PFM.policyToJson to return {'trait_type: 'value'},{'trait_type': 'value'},...,{...}  
(last attribute must not have a comma at the end)_

### \_update

```solidity
function _update(address from, address to, uint256[] ids, uint256[] values) internal virtual
```

_Pre-hook for ERC1155's \_update() called on transfers._