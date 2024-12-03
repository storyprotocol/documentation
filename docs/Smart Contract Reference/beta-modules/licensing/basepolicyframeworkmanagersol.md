---
title: BasePolicyFrameworkManager.sol
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
Base contract for policy framework managers.

### name

```solidity
string name
```

Returns the name to be show in license NFT (LNFT) metadata

### licenseTextUrl

```solidity
string licenseTextUrl
```

Returns the URL to the off chain legal agreement template text

### constructor

```solidity
constructor(address licensing, string name_, string licenseTextUrl_) internal
```

### supportsInterface

```solidity
function supportsInterface(bytes4 interfaceId) public view virtual returns (bool)
```

IERC165 interface support.
