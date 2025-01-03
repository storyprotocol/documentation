---
title: LicensingModule.sol
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
Licensing module is the main entry point for the licensing system. It is responsible for:

* Registering policy frameworks
* Registering policies
* Minting licenses
* Linking IP to its parent
* Verifying linking parameters
* Verifying policy parameters

### name

```solidity
string name
```

Returns the string identifier associated with the module.

### ROYALTY\_MODULE

```solidity
contract RoyaltyModule ROYALTY_MODULE
```

Returns the canonical protocol-wide RoyaltyModule

### LICENSE\_REGISTRY

```solidity
contract ILicenseRegistry LICENSE_REGISTRY
```

Returns the canonical protocol-wide LicenseRegistry

### DISPUTE\_MODULE

```solidity
contract IDisputeModule DISPUTE_MODULE
```

Returns the dispute module

### onlyLicenseRegistry

```solidity
modifier onlyLicenseRegistry()
```

Modifier to allow only LicenseRegistry as the caller

### constructor

```solidity
constructor(address accessController, address ipAccountRegistry, address royaltyModule, address registry, address disputeModule) public
```

### registerPolicyFrameworkManager

```solidity
function registerPolicyFrameworkManager(address manager) external
```

Registers a policy framework manager into the contract, so it can add policy data for licenses.

#### Parameters

| Name    | Type    | Description                                                                    |
| ------- | ------- | ------------------------------------------------------------------------------ |
| manager | address | the address of the manager. Will be ERC165 checked for IPolicyFrameworkManager |

### registerPolicy

```solidity
function registerPolicy(struct Licensing.Policy pol) external returns (uint256 policyId)
```

Registers a policy into the contract. MUST be called by a registered framework or it will revert.\
The policy data and its integrity must be verified by the policy framework manager.

#### Parameters

| Name | Type                    | Description                                                                      |
| ---- | ----------------------- | -------------------------------------------------------------------------------- |
| pol  | struct Licensing.Policy | The Licensing policy data. MUST have same policy framework as the caller address |

#### Return Values

| Name     | Type    | Description                           |
| -------- | ------- | ------------------------------------- |
| policyId | uint256 | The id of the newly registered policy |

### addPolicyToIp

```solidity
function addPolicyToIp(address ipId, uint256 polId) external returns (uint256 indexOnIpId)
```

Adds a policy to the set of policies of an IP. Reverts if policy is undefined in LicenseRegistry.

#### Parameters

| Name  | Type    | Description          |
| ----- | ------- | -------------------- |
| ipId  | address | The id of the IP     |
| polId | uint256 | The id of the policy |

#### Return Values

| Name        | Type    | Description                                     |
| ----------- | ------- | ----------------------------------------------- |
| indexOnIpId | uint256 | The index of the policy in the IP's policy list |

### mintLicense

```solidity
function mintLicense(uint256 policyId, address licensorIpId, uint256 amount, address receiver, bytes royaltyContext) external returns (uint256 licenseId)
```

Mints a license to create derivative IP. License NFTs represent a policy granted by IPs (licensors).\
Reverts if caller is not authorized by any of the licensors.

*This NFT needs to be burned in order to link a derivative IP with its parents. If this is the first\
combination of policy and licensors, a new licenseId will be created (by incrementing prev totalLicenses).\
If not, the license is fungible and an id will be reused. The licensing terms that regulate creating new\
licenses will be verified to allow minting.*

#### Parameters

| Name           | Type    | Description                                        |
| -------------- | ------- | -------------------------------------------------- |
| policyId       | uint256 | The id of the policy with the licensing parameters |
| licensorIpId   | address | The id of the licensor IP                          |
| amount         | uint256 | The amount of licenses to mint                     |
| receiver       | address | The address that will receive the license          |
| royaltyContext | bytes   | The context for the royalty module to process      |

#### Return Values

| Name      | Type    | Description                  |
| --------- | ------- | ---------------------------- |
| licenseId | uint256 | The ID of the license NFT(s) |

### linkIpToParents

```solidity
function linkIpToParents(uint256[] licenseIds, address childIpId, bytes royaltyContext) external
```

Links an IP to the licensors listed in the license NFTs, if their policies allow it. Burns the license\
NFTs in the process. The caller must be the owner of the IP asset and license NFTs.

#### Parameters

| Name           | Type       | Description                                   |
| -------------- | ---------- | --------------------------------------------- |
| licenseIds     | uint256\[] | The id of the licenses to burn                |
| childIpId      | address    | The id of the child IP to be linked           |
| royaltyContext | bytes      | The context for the royalty module to process |

### isFrameworkRegistered

```solidity
function isFrameworkRegistered(address policyFramework) external view returns (bool)
```

Returns if the framework address is registered in the LicensingModule.

#### Parameters

| Name            | Type    | Description                                 |
| --------------- | ------- | ------------------------------------------- |
| policyFramework | address | The address of the policy framework manager |

#### Return Values

| Name | Type | Description                                      |
| ---- | ---- | ------------------------------------------------ |
| \[0] | bool | isRegistered True if the framework is registered |

### totalPolicies

```solidity
function totalPolicies() external view returns (uint256)
```

Returns amount of distinct licensing policies in the LicensingModule.

#### Return Values

| Name | Type    | Description                          |
| ---- | ------- | ------------------------------------ |
| \[0] | uint256 | totalPolicies The amount of policies |

### policy

```solidity
function policy(uint256 policyId) public view returns (struct Licensing.Policy pol)
```

Returns the policy data for policyId, reverts if not found.

#### Parameters

| Name     | Type    | Description          |
| -------- | ------- | -------------------- |
| policyId | uint256 | The id of the policy |

#### Return Values

| Name | Type                    | Description     |
| ---- | ----------------------- | --------------- |
| pol  | struct Licensing.Policy | The policy data |

### getPolicyId

```solidity
function getPolicyId(struct Licensing.Policy pol) external view returns (uint256 policyId)
```

Returns the policy id for the given policy data, or 0 if not found.

#### Parameters

| Name | Type                    | Description                      |
| ---- | ----------------------- | -------------------------------- |
| pol  | struct Licensing.Policy | The policy data in Policy struct |

#### Return Values

| Name     | Type    | Description          |
| -------- | ------- | -------------------- |
| policyId | uint256 | The id of the policy |

### policyAggregatorData

```solidity
function policyAggregatorData(address framework, address ipId) external view returns (bytes)
```

Returns the policy aggregator data for the given IP ID in the framework.

#### Parameters

| Name      | Type    | Description                                 |
| --------- | ------- | ------------------------------------------- |
| framework | address | The address of the policy framework manager |
| ipId      | address | The id of the IP asset                      |

#### Return Values

| Name | Type  | Description                                                                    |
| ---- | ----- | ------------------------------------------------------------------------------ |
| \[0] | bytes | data The encoded policy aggregator data to be decoded by the framework manager |

### isPolicyDefined

```solidity
function isPolicyDefined(uint256 policyId) public view returns (bool)
```

Returns if policyId exists in the LicensingModule

#### Parameters

| Name     | Type    | Description          |
| -------- | ------- | -------------------- |
| policyId | uint256 | The id of the policy |

#### Return Values

| Name | Type | Description                             |
| ---- | ---- | --------------------------------------- |
| \[0] | bool | isDefined True if the policy is defined |

### policyIdsForIp

```solidity
function policyIdsForIp(bool isInherited, address ipId) external view returns (uint256[] policyIds)
```

Returns the policy ids attached to an IP

*Potentially gas-intensive operation, use with care.*

#### Parameters

| Name        | Type    | Description                                      |
| ----------- | ------- | ------------------------------------------------ |
| isInherited | bool    | True if the policy is inherited from a parent IP |
| ipId        | address | The id of the IP asset                           |

#### Return Values

| Name      | Type       | Description                      |
| --------- | ---------- | -------------------------------- |
| policyIds | uint256\[] | The ids of policy ids for the IP |

### totalPoliciesForIp

```solidity
function totalPoliciesForIp(bool isInherited, address ipId) public view returns (uint256)
```

Returns the total number of policies attached to an IP

#### Parameters

| Name        | Type    | Description                                      |
| ----------- | ------- | ------------------------------------------------ |
| isInherited | bool    | True if the policy is inherited from a parent IP |
| ipId        | address | The id of the IP asset                           |

#### Return Values

| Name | Type    | Description                                           |
| ---- | ------- | ----------------------------------------------------- |
| \[0] | uint256 | totalPolicies The total number of policies for the IP |

### isPolicyIdSetForIp

```solidity
function isPolicyIdSetForIp(bool isInherited, address ipId, uint256 policyId) external view returns (bool)
```

True if the given policy attached to the given IP is inherited from a parent IP.

#### Parameters

<Table>
  <thead>
    <tr>
      <th>
        Name
      </th>

      <th>
        Type
      </th>

      <th>
        Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        isInherited
      </td>

      <td>
        bool
      </td>

      <td>

      </td>
    </tr>

    <tr>
      <td>
        ipId
      </td>

      <td>
        address
      </td>

      <td>
        The id of the IP asset that has the policy attached
      </td>
    </tr>

    <tr>
      <td>
        policyId
      </td>

      <td>
        uint256
      </td>

      <td>
        The id of the policy to check if inherited
      </td>
    </tr>
  </tbody>
</Table>

#### Return Values

| Name | Type | Description                                                  |
| ---- | ---- | ------------------------------------------------------------ |
| \[0] | bool | isInherited True if the policy is inherited from a parent IP |

### policyIdForIpAtIndex

```solidity
function policyIdForIpAtIndex(bool isInherited, address ipId, uint256 index) external view returns (uint256 policyId)
```

Returns the policy ID for an IP by local index on the IP's policy set

#### Parameters

| Name        | Type    | Description                                        |
| ----------- | ------- | -------------------------------------------------- |
| isInherited | bool    | True if the policy is inherited from a parent IP   |
| ipId        | address | The id of the IP asset to check                    |
| index       | uint256 | The local index of a policy in the IP's policy set |

#### Return Values

| Name     | Type    | Description          |
| -------- | ------- | -------------------- |
| policyId | uint256 | The id of the policy |

### policyForIpAtIndex

```solidity
function policyForIpAtIndex(bool isInherited, address ipId, uint256 index) external view returns (struct Licensing.Policy)
```

Returns the policy data for an IP by the policy's local index on the IP's policy set

#### Parameters

| Name        | Type    | Description                                        |
| ----------- | ------- | -------------------------------------------------- |
| isInherited | bool    | True if the policy is inherited from a parent IP   |
| ipId        | address | The id of the IP asset to check                    |
| index       | uint256 | The local index of a policy in the IP's policy set |

#### Return Values

| Name | Type                    | Description            |
| ---- | ----------------------- | ---------------------- |
| \[0] | struct Licensing.Policy | policy The policy data |

### policyStatus

```solidity
function policyStatus(address ipId, uint256 policyId) external view returns (uint256 index, bool isInherited, bool active)
```

Returns the status of a policy in an IP's policy set

#### Parameters

| Name     | Type    | Description                     |
| -------- | ------- | ------------------------------- |
| ipId     | address | The id of the IP asset to check |
| policyId | uint256 | The id of the policy            |

#### Return Values

| Name        | Type    | Description                                          |
| ----------- | ------- | ---------------------------------------------------- |
| index       | uint256 | The local index of the policy in the IP's policy set |
| isInherited | bool    | True if the policy is inherited from a parent IP     |
| active      | bool    | True if the policy is active                         |

### isPolicyInherited

```solidity
function isPolicyInherited(address ipId, uint256 policyId) external view returns (bool)
```

Returns if the given policy attached to the given IP is inherited from a parent IP.

#### Parameters

| Name     | Type    | Description                                         |
| -------- | ------- | --------------------------------------------------- |
| ipId     | address | The id of the IP asset that has the policy attached |
| policyId | uint256 | The id of the policy to check if inherited          |

#### Return Values

| Name | Type | Description                                                  |
| ---- | ---- | ------------------------------------------------------------ |
| \[0] | bool | isInherited True if the policy is inherited from a parent IP |

### isParent

```solidity
function isParent(address parentIpId, address childIpId) external view returns (bool)
```

Returns if an IP is a derivative of another IP

#### Parameters

| Name       | Type    | Description                            |
| ---------- | ------- | -------------------------------------- |
| parentIpId | address | The id of the parent IP asset to check |
| childIpId  | address | The id of the child IP asset to check  |

#### Return Values

| Name | Type | Description                                                    |
| ---- | ---- | -------------------------------------------------------------- |
| \[0] | bool | isParent True if the child IP is a derivative of the parent IP |

### parentIpIds

```solidity
function parentIpIds(address ipId) external view returns (address[])
```

Returns the list of parent IP assets for a given child IP asset

#### Parameters

| Name | Type    | Description                           |
| ---- | ------- | ------------------------------------- |
| ipId | address | The id of the child IP asset to check |

#### Return Values

| Name | Type       | Description                                 |
| ---- | ---------- | ------------------------------------------- |
| \[0] | address\[] | parentIpIds The ids of the parent IP assets |

### totalParentsForIpId

```solidity
function totalParentsForIpId(address ipId) external view returns (uint256)
```

Returns the total number of parents for an IP asset

#### Parameters

| Name | Type    | Description                     |
| ---- | ------- | ------------------------------- |
| ipId | address | The id of the IP asset to check |

#### Return Values

| Name | Type    | Description                                       |
| ---- | ------- | ------------------------------------------------- |
| \[0] | uint256 | totalParents The total number of parent IP assets |
