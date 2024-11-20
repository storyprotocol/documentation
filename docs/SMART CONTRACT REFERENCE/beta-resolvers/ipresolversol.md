---
title: IPResolver.sol
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
Canonical IP resolver contract used for Story Protocol.

### name

```solidity
string name
```

Returns the string identifier associated with the module.

### constructor

```solidity
constructor(address accessController, address ipAssetRegistry) public
```

### supportsInterface

```solidity
function supportsInterface(bytes4 id) public view virtual returns (bool)
```

IERC165 interface support.