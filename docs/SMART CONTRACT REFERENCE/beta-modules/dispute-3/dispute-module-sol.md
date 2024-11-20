---
title: DisputeModule.sol
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
The dispute module acts as an enforcement layer for IP assets that allows raising and resolving disputes  
through arbitration by judges.

### name

```solidity
string name
```

Returns the string identifier associated with the module.

### IN_DISPUTE

```solidity
bytes32 IN_DISPUTE
```

Tag to represent the dispute is in dispute state waiting for judgement

### IP_ASSET_REGISTRY

```solidity
contract IIPAssetRegistry IP_ASSET_REGISTRY
```

Protocol-wide IP asset registry

### disputeCounter

```solidity
uint256 disputeCounter
```

Dispute ID counter

### baseArbitrationPolicy

```solidity
address baseArbitrationPolicy
```

The address of the base arbitration policy

### disputes

```solidity
mapping(uint256 => struct IDisputeModule.Dispute) disputes
```

Returns the dispute information for a given dispute id

### isWhitelistedDisputeTag

```solidity
mapping(bytes32 => bool) isWhitelistedDisputeTag
```

Indicates if a dispute tag is whitelisted

### isWhitelistedArbitrationPolicy

```solidity
mapping(address => bool) isWhitelistedArbitrationPolicy
```

Indicates if an arbitration policy is whitelisted

### isWhitelistedArbitrationRelayer

```solidity
mapping(address => mapping(address => bool)) isWhitelistedArbitrationRelayer
```

Indicates if an arbitration relayer is whitelisted for a given arbitration policy

### arbitrationPolicies

```solidity
mapping(address => address) arbitrationPolicies
```

Arbitration policy for a given ipId

### constructor

```solidity
constructor(address _controller, address _assetRegistry, address _governance) public
```

### whitelistDisputeTag

```solidity
function whitelistDisputeTag(bytes32 tag, bool allowed) external
```

Whitelists a dispute tag

#### Parameters

| Name    | Type    | Description                                        |
| ------- | ------- | -------------------------------------------------- |
| tag     | bytes32 | The dispute tag                                    |
| allowed | bool    | Indicates if the dispute tag is whitelisted or not |

### whitelistArbitrationPolicy

```solidity
function whitelistArbitrationPolicy(address arbitrationPolicy, bool allowed) external
```

Whitelists an arbitration policy

#### Parameters

| Name              | Type    | Description                                               |
| ----------------- | ------- | --------------------------------------------------------- |
| arbitrationPolicy | address | The address of the arbitration policy                     |
| allowed           | bool    | Indicates if the arbitration policy is whitelisted or not |

### whitelistArbitrationRelayer

```solidity
function whitelistArbitrationRelayer(address arbitrationPolicy, address arbPolicyRelayer, bool allowed) external
```

Whitelists an arbitration relayer for a given arbitration policy

#### Parameters

| Name              | Type    | Description                                                |
| ----------------- | ------- | ---------------------------------------------------------- |
| arbitrationPolicy | address | The address of the arbitration policy                      |
| arbPolicyRelayer  | address | The address of the arbitration relayer                     |
| allowed           | bool    | Indicates if the arbitration relayer is whitelisted or not |

### setBaseArbitrationPolicy

```solidity
function setBaseArbitrationPolicy(address arbitrationPolicy) external
```

Sets the base arbitration policy

#### Parameters

| Name              | Type    | Description                           |
| ----------------- | ------- | ------------------------------------- |
| arbitrationPolicy | address | The address of the arbitration policy |

### setArbitrationPolicy

```solidity
function setArbitrationPolicy(address ipId, address arbitrationPolicy) external
```

Sets the arbitration policy for an ipId

#### Parameters

| Name              | Type    | Description                           |
| ----------------- | ------- | ------------------------------------- |
| ipId              | address | The ipId                              |
| arbitrationPolicy | address | The address of the arbitration policy |

### raiseDispute

```solidity
function raiseDispute(address targetIpId, string linkToDisputeEvidence, bytes32 targetTag, bytes data) external returns (uint256)
```

Raises a dispute on a given ipId

#### Parameters

| Name                  | Type    | Description                                |
| --------------------- | ------- | ------------------------------------------ |
| targetIpId            | address | The ipId that is the target of the dispute |
| linkToDisputeEvidence | string  | The link of the dispute evidence           |
| targetTag             | bytes32 | The target tag of the dispute              |
| data                  | bytes   | The data to initialize the policy          |

#### Return Values

| Name | Type    | Description                                  |
| ---- | ------- | -------------------------------------------- |
| [0]  | uint256 | disputeId The id of the newly raised dispute |

### setDisputeJudgement

```solidity
function setDisputeJudgement(uint256 disputeId, bool decision, bytes data) external
```

Sets the dispute judgement on a given dispute. Only whitelisted arbitration relayers can call to judge.

#### Parameters

| Name      | Type    | Description                           |
| --------- | ------- | ------------------------------------- |
| disputeId | uint256 | The dispute id                        |
| decision  | bool    | The decision of the dispute           |
| data      | bytes   | The data to set the dispute judgement |

### cancelDispute

```solidity
function cancelDispute(uint256 disputeId, bytes data) external
```

Cancels an ongoing dispute

#### Parameters

| Name      | Type    | Description                    |
| --------- | ------- | ------------------------------ |
| disputeId | uint256 | The dispute id                 |
| data      | bytes   | The data to cancel the dispute |

### resolveDispute

```solidity
function resolveDispute(uint256 disputeId) external
```

Resolves a dispute after it has been judged

#### Parameters

| Name      | Type    | Description    |
| --------- | ------- | -------------- |
| disputeId | uint256 | The dispute id |

### isIpTagged

```solidity
function isIpTagged(address ipId) external view returns (bool)
```

Returns true if the ipId is tagged with any tag (meaning at least one dispute went through)

#### Parameters

| Name | Type    | Description |
| ---- | ------- | ----------- |
| ipId | address | The ipId    |

#### Return Values

| Name | Type | Description                         |
| ---- | ---- | ----------------------------------- |
| [0]  | bool | isTagged True if the ipId is tagged |