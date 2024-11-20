---
title: ArbitrationPolicySP.sol
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
The Story Protocol arbitration policy is a simple policy that requires the dispute initiator to pay a fixed  
        amount of tokens to raise a dispute and refunds that amount if the dispute initiator wins the dispute.

### DISPUTE_MODULE

```solidity
address DISPUTE_MODULE
```

Returns the dispute module address

### PAYMENT_TOKEN

```solidity
address PAYMENT_TOKEN
```

Returns the payment token address

### ARBITRATION_PRICE

```solidity
uint256 ARBITRATION_PRICE
```

Returns the arbitration price

### onlyDisputeModule

```solidity
modifier onlyDisputeModule()
```

Restricts the calls to the DisputeModule

### constructor

```solidity
constructor(address _disputeModule, address _paymentToken, uint256 _arbitrationPrice, address _governable) public
```

### onRaiseDispute

```solidity
function onRaiseDispute(address caller, bytes data) external
```

Executes custom logic on raising dispute.

_Enforced to be only callable by the DisputeModule._

#### Parameters

| Name   | Type    | Description                                  |
| ------ | ------- | -------------------------------------------- |
| caller | address | Address of the caller                        |
| data   | bytes   | The arbitrary data used to raise the dispute |

### onDisputeJudgement

```solidity
function onDisputeJudgement(uint256 disputeId, bool decision, bytes data) external
```

Executes custom logic on disputing judgement.

_Enforced to be only callable by the DisputeModule._

#### Parameters

| Name      | Type    | Description                                          |
| --------- | ------- | ---------------------------------------------------- |
| disputeId | uint256 | The dispute id                                       |
| decision  | bool    | The decision of the dispute                          |
| data      | bytes   | The arbitrary data used to set the dispute judgement |

### onDisputeCancel

```solidity
function onDisputeCancel(address caller, uint256 disputeId, bytes data) external
```

Executes custom logic on disputing cancel.

_Enforced to be only callable by the DisputeModule._

#### Parameters

| Name      | Type    | Description                                   |
| --------- | ------- | --------------------------------------------- |
| caller    | address | Address of the caller                         |
| disputeId | uint256 | The dispute id                                |
| data      | bytes   | The arbitrary data used to cancel the dispute |

### governanceWithdraw

```solidity
function governanceWithdraw() external
```

Allows governance address to withdraw

_Enforced to be only callable by the governance protocol admin._