---
title: GovernanceLib.sol
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
*This library provides types for Story Protocol Governance.*

### PROTOCOL\_ADMIN

```solidity
bytes32 PROTOCOL_ADMIN
```

### ProtocolState

An enum containing the different states the protocol can be in.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |

```solidity
enum ProtocolState {
  Unpaused,
  Paused
}
```
