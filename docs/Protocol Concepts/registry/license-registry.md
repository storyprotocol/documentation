---
title: License Registry
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
> 🗒️ Contract
>
> View the smart contract [here](https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/registries/LicenseRegistry.sol).

The License Registry stores all license-related states within the protocol, including managing global state like registering new License Templates like the [Programmable IP License (PIL💊)](doc:programmable-ip-license), attaching licenses to individual [IP Assets](doc:ipasset), registering derivatives, and the like:

```sol LicenseRegistry.sol
struct LicenseRegistryStorage {
  /// The default license template address
  address defaultLicenseTemplate;
  /// The default license terms ID
  uint256 defaultLicenseTermsId;
  /// Registered license templates
  mapping(address licenseTemplate => bool isRegistered) registeredLicenseTemplates;
  /// Mapping of parent IPs to derivative IPs
  mapping(address childIpId => EnumerableSet.AddressSet parentIpIds) parentIps;
  /// Mapping of derivative IPs to parent IPs
  mapping(address parentIpId => EnumerableSet.AddressSet childIpIds) childIps;
  /// attachedLicenseTerms Mapping of attached license terms to IP IDs
  mapping(address ipId => EnumerableSet.UintSet licenseTermsIds) attachedLicenseTerms;
  /// Mapping of license templates to IP IDs
  mapping(address ipId => address licenseTemplate) licenseTemplates;
  /// Mapping of minting license configs to a licenseTerms of an IP
  mapping(bytes32 ipLicenseHash => Licensing.LicensingConfig licensingConfig) licensingConfigs;
  /// Mapping of minting license configs to an IP, the config will apply to all licenses under the IP
  mapping(address ipId => Licensing.LicensingConfig licensingConfig) licensingConfigsForIp;
}
```

### Notable Functions

```sol LicenseRegistry.sol
function attachLicenseTermsToIp(address ipId, address licenseTemplate, uint256 licenseTermsId) external onlyLicensingModule
```

This function allows you to attach License Terms to an IP Asset.

```sol LicenseRegistry.sol
function registerDerivativeIp(address childIpId, address[] calldata parentIpIds, address licenseTemplate, uint256[] calldata licenseTermsIds, bool isUsingLicenseToken) external onlyLicensingModule
```

This function allows you to register an IP Asset as a derivative of another IP Asset, unlocking things like claimable royalty flows from the [Royalty Module](doc:royalty-module).
