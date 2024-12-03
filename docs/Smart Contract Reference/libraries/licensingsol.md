---
title: Licensing.sol
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
Types and constants used by the licensing related contracts

### Policy

A particular configuration (flavor) of a Policy Framework, setting values for the licensing\
terms (parameters) of the framework.

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |

```solidity
struct Policy {
  bool isLicenseTransferable;
  address policyFramework;
  bytes frameworkData;
  address royaltyPolicy;
  bytes royaltyData;
  uint256 mintingFee;
  address mintingFeeToken;
}
```

### License

Data that define a License Agreement NFT

#### Parameters

| Name | Type | Description |
| ---- | ---- | ----------- |

```solidity
struct License {
  uint256 policyId;
  address licensorIpId;
  bool transferable;
}
```
