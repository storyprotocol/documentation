---
title: Реестр Лицензий
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
> 🗒️ Контракт
>
> Ознакомьтесь со смарт-контрактом [тут](https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/registries/LicenseRegistry.sol).

The License Registry stores all license-related states within the protocol, including managing global state like registering new License Templates like the [Programmable IP License (PIL💊)](doc:programmable-ip-license), attaching licenses to individual [IP Assets](doc:ipasset), registering derivatives, and the like:

Реестр Лицензий хранит все состояния, связанные с лицензиями в протоколе, включая управление глобальными состояниями, такими как регистрация новых шаблонов лицензий, например, [Программируемая Лицензия IP (PIL💊)](doc:programmable-ip-license), прикрепление лицензий к отдельным [IP-активам](doc:ipasset), регистрацию производных активов и тому подобное:

```sol LicenseRegistry.sol
struct LicenseRegistryStorage {
  /// Адрес шаблона лицензии по умолчанию
  address defaultLicenseTemplate;
  /// ID условий лицензии по умолчанию
  uint256 defaultLicenseTermsId;
  /// Зарегистрированные шаблоны лицензий
  mapping(address licenseTemplate => bool isRegistered) registeredLicenseTemplates;
  /// Сопоставление родительских IP с производными IP
  mapping(address childIpId => EnumerableSet.AddressSet parentIpIds) parentIps;
  /// Сопоставление производных IP с родительскими IP
  mapping(address parentIpId => EnumerableSet.AddressSet childIpIds) childIps;
  /// Сопоставление прикрепленных условий лицензий с ID IP
  mapping(address ipId => EnumerableSet.UintSet licenseTermsIds) attachedLicenseTerms;
  /// Сопоставление шаблонов лицензий с ID IP
  mapping(address ipId => address licenseTemplate) licenseTemplates;
  /// Сопоставление конфигураций лицензий с условиями лицензии IP
  mapping(bytes32 ipLicenseHash => Licensing.LicensingConfig licensingConfig) licensingConfigs;
  /// Сопоставление конфигураций лицензий с IP, конфигурация будет применяться ко всем лицензиям данного IP
  mapping(address ipId => Licensing.LicensingConfig licensingConfig) licensingConfigsForIp;
}
```

### Основные функции

```sol LicenseRegistry.sol
function attachLicenseTermsToIp(address ipId, address licenseTemplate, uint256 licenseTermsId) external onlyLicensingModule
```

Эта функция позволяет прикрепить Условия Лицензии к IP-активу.

```sol LicenseRegistry.sol
function registerDerivativeIp(address childIpId, address[] calldata parentIpIds, address licenseTemplate, uint256[] calldata licenseTermsIds, bool isUsingLicenseToken) external onlyLicensingModule
```

Эта функция позволяет зарегистрировать IP-актив как производный от другого IP-актива, что открывает возможности, вроде получения роялти через [Модуль Роялти](doc:royalty-module).
