---
title: IP.sol
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
Library for constants, structs, and helper functions used for IP.

### MetadataV1

Core metadata to associate with each IP.

```solidity
struct MetadataV1 {
  string name;
  bytes32 hash;
  uint64 registrationDate;
  address registrant;
  string uri;
}
```
