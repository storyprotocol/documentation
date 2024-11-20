---
title: PILPolicyFrameworkManager.sol
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
PIL Policy Framework Manager implements the PIL Policy Framework logic for encoding and decoding PIL\
policies into the LicenseRegistry and verifying the licensing parameters for linking, minting, and transferring.

### constructor

```solidity
constructor(address accessController, address ipAccountRegistry, address licensing, string name_, string licenseUrl_) public
```

### registerPolicy

```solidity
function registerPolicy(struct RegisterPILPolicyParams params) external returns (uint256 policyId)
```

Registers a new policy to the registry

*Internally, this function must generate a Licensing.Policy struct and call registerPolicy.*

#### Parameters

| Name   | Type                           | Description                               |
| ------ | ------------------------------ | ----------------------------------------- |
| params | struct RegisterPILPolicyParams | parameters needed to register a PILPolicy |

#### Return Values

| Name     | Type    | Description                           |
| -------- | ------- | ------------------------------------- |
| policyId | uint256 | The ID of the newly registered policy |

### verifyLink

```solidity
function verifyLink(uint256 licenseId, address licensee, address ipId, address parentIpId, bytes policyData) external returns (bool)
```

Verify policy parameters for linking a child IP to a parent IP (licensor) by burning a license NFT.

*Enforced to be only callable by LicenseRegistry*

#### Parameters

| Name       | Type    | Description                                                     |
| ---------- | ------- | --------------------------------------------------------------- |
| licenseId  | uint256 | the license id to burn                                          |
| licensee   | address | the address that holds the license and is executing the linking |
| ipId       | address | the IP id of the IP being linked                                |
| parentIpId | address | the IP id of the parent IP                                      |
| policyData | bytes   | the encoded framework policy data to verify                     |

#### Return Values

| Name | Type | Description                           |
| ---- | ---- | ------------------------------------- |
| \[0] | bool | verified True if the link is verified |

### verifyMint

```solidity
function verifyMint(address licensee, bool mintingFromADerivative, address licensorIpId, address receiver, uint256 mintAmount, bytes policyData) external returns (bool)
```

Verify policy parameters for minting a license.

*Enforced to be only callable by LicenseRegistry*

#### Parameters

| Name                   | Type    | Description                                                  |
| ---------------------- | ------- | ------------------------------------------------------------ |
| licensee               | address | the address that holds the license and is executing the mint |
| mintingFromADerivative | bool    | true if the license is minting from a derivative IPA         |
| licensorIpId           | address | the IP id of the licensor                                    |
| receiver               | address | the address receiving the license                            |
| mintAmount             | uint256 | the amount of licenses to mint                               |
| policyData             | bytes   | the encoded framework policy data to verify                  |

#### Return Values

| Name | Type | Description                           |
| ---- | ---- | ------------------------------------- |
| \[0] | bool | verified True if the link is verified |

### getAggregator

```solidity
function getAggregator(address ipId) external view returns (struct PILAggregator rights)
```

Returns the aggregation data for inherited policies of an IP asset.

#### Parameters

| Name | Type    | Description                                      |
| ---- | ------- | ------------------------------------------------ |
| ipId | address | The ID of the IP asset to get the aggregator for |

#### Return Values

| Name   | Type                 | Description              |
| ------ | -------------------- | ------------------------ |
| rights | struct PILAggregator | The PILAggregator struct |

### getPILPolicy

```solidity
function getPILPolicy(uint256 policyId) external view returns (struct PILPolicy policy)
```

gets the PILPolicy for a given policy ID decoded from Licensing.Policy.frameworkData

*Do not call this function from a smart contract, it is only for off-chain*

#### Parameters

| Name     | Type    | Description                 |
| -------- | ------- | --------------------------- |
| policyId | uint256 | The ID of the policy to get |

#### Return Values

| Name   | Type             | Description          |
| ------ | ---------------- | -------------------- |
| policy | struct PILPolicy | The PILPolicy struct |

### processInheritedPolicies

```solidity
function processInheritedPolicies(bytes aggregator, uint256 policyId, bytes policy) external view returns (bool changedAgg, bytes newAggregator)
```

Verify compatibility of one or more policies when inheriting them from one or more parent IPs.

*Enforced to be only callable by LicenseRegistry\
The assumption in this method is that we can add parents later on, hence the need\
for an aggregator, if not we will do this when linking to parents directly with an\
array of policies.*

#### Parameters

| Name       | Type    | Description                             |
| ---------- | ------- | --------------------------------------- |
| aggregator | bytes   | common state of the policies for the IP |
| policyId   | uint256 | the ID of the policy being inherited    |
| policy     | bytes   | the policy to inherit                   |

#### Return Values

| Name          | Type  | Description                        |
| ------------- | ----- | ---------------------------------- |
| changedAgg    | bool  | true if the aggregator was changed |
| newAggregator | bytes | the new aggregator                 |

### policyToJson

```solidity
function policyToJson(bytes policyData) public pure returns (string)
```

Returns the stringified JSON policy data for the LicenseRegistry.uri(uint256) method.

*Must return ERC1155 OpenSea standard compliant metadata.*

#### Parameters

| Name       | Type  | Description                                                |
| ---------- | ----- | ---------------------------------------------------------- |
| policyData | bytes | The encoded licensing policy data to be decoded by the PFM |

#### Return Values

| Name | Type   | Description                                                 |
| ---- | ------ | ----------------------------------------------------------- |
| \[0] | string | jsonString The OpenSea-compliant metadata URI of the policy |

### \_policyCommercialTraitsToJson

```solidity
function _policyCommercialTraitsToJson(struct PILPolicy policy) internal pure returns (string)
```

*Encodes the commercial traits of PIL policy into a JSON string for OpenSea*

#### Parameters

| Name   | Type             | Description          |
| ------ | ---------------- | -------------------- |
| policy | struct PILPolicy | The policy to encode |

### \_policyDerivativeTraitsToJson

```solidity
function _policyDerivativeTraitsToJson(struct PILPolicy policy) internal pure returns (string)
```

*Encodes the derivative traits of PIL policy into a JSON string for OpenSea*

#### Parameters

| Name   | Type             | Description          |
| ------ | ---------------- | -------------------- |
| policy | struct PILPolicy | The policy to encode |

### \_verifyComercialUse

```solidity
function _verifyComercialUse(struct PILPolicy policy, address royaltyPolicy, uint256 mintingFee, address mintingFeeToken) internal view
```

*Checks the configuration of commercial use and throws if the policy is not compliant*

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
        policy
      </td>

      <td>
        struct PILPolicy
      </td>

      <td>
        The policy to verify
      </td>
    </tr>

    <tr>
      <td>
        royaltyPolicy
      </td>

      <td>
        address
      </td>

      <td>
        The address of the royalty policy
      </td>
    </tr>

    <tr>
      <td>
        mintingFee
      </td>

      <td>
        uint256
      </td>

      <td>

      </td>
    </tr>

    <tr>
      <td>
        mintingFeeToken
      </td>

      <td>
        address
      </td>

      <td>

      </td>
    </tr>
  </tbody>
</Table>

### \_verifyDerivatives

```solidity
function _verifyDerivatives(struct PILPolicy policy) internal pure
```

Checks the configuration of derivative parameters and throws if the policy is not compliant

#### Parameters

| Name   | Type             | Description          |
| ------ | ---------------- | -------------------- |
| policy | struct PILPolicy | The policy to verify |

### \_verifHashedParams

```solidity
function _verifHashedParams(bytes32 oldHash, bytes32 newHash, bytes32 permissive) internal pure returns (bytes32 result)
```

*Verifies compatibility for params where the valid options are either permissive value, or equal params*

#### Parameters

| Name       | Type    | Description                       |
| ---------- | ------- | --------------------------------- |
| oldHash    | bytes32 | hash of the old param             |
| newHash    | bytes32 | hash of the new param             |
| permissive | bytes32 | hash of the most permissive param |

#### Return Values

| Name   | Type    | Description                                        |
| ------ | ------- | -------------------------------------------------- |
| result | bytes32 | the hash that's different from the permissive hash |
