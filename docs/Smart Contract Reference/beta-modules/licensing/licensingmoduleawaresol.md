---
title: LicensingModuleAware.sol
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
Base contract to be inherited by modules that need to access the licensing module.

### LICENSING\_MODULE

```solidity
contract ILicensingModule LICENSING_MODULE
```

Returns the protocol-wide licensing module.

### constructor

```solidity
constructor(address licensingModule) internal
```

### onlyLicensingModule

```solidity
modifier onlyLicensingModule()
```

Modifier for authorizing the calling entity to only the LicensingModule.
