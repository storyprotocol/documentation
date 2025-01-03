---
title: Хуки
excerpt: Введение в Хуки в Story Protocol
deprecated: false
hidden: false
metadata:
  title: ""
  description: ""
  robots: index
next:
  description: ""
---

# Обзор

Хуки в Story Protocol определяются как специализированный интерфейс, который наследуется от фреймворка Module. Они предназначены для того, чтобы разработчики могли создавать индивидуальные реализации, которые легко интегрируются с уже существующими модулями.

<Image align="center" src="https://files.readme.io/2ea34f7-Screenshot_2024-02-05_at_16.09.49.png" />

# Концепция и функциональность

В то время как Модули являются основой Story Protocol, выполняя действия и управляя взаимодействиями, хуки отличаются своей узкой специализацией. Они предназначены для проверки условий, что особенно важно благодаря их функции `verify()`. Этот подход делает хуки уникальным подмножеством модулей, специально разработанных для выполнения определённых задач в экосистеме.

# Философия проектирования

С точки зрения структуры, хуки не рассматриваются как отдельные сущности от модулей. Это позволяет избежать излишней сложности архитектуры. Рассматривая хуки как специализированные модули, удаётся создать простую и эффективную систему, в которой роли и взаимодействия максимально ясны.

# Пример: Хук для проверки доступа на основе токенов

```coffeescript
pragma solidity 0.8.23;

import { IERC165 } from "@openzeppelin/contracts/utils/introspection/ERC165.sol";
import { ERC165Checker } from "@openzeppelin/contracts/utils/introspection/ERC165Checker.sol";
import { IERC721 } from "@openzeppelin/contracts/token/ERC721/IERC721.sol";
import { IHookModule } from "../../../contracts/interfaces/modules/base/IHookModule.sol";
import { BaseModule } from "../../../contracts/modules/BaseModule.sol";

/// @title Token Gated Hook.
/// @notice Хук для проверки, является ли вызывающий адрес владельцем NFT.
contract TokenGatedHook is BaseModule, IHookModule {
    using ERC165Checker for address;

    string public constant override name = "TokenGatedHook";

    function verify(address caller, bytes calldata data) external view returns (bool) {
        address tokenAddress = abi.decode(data, (address));
        if (caller == address(0)) {
            return false;
        }
        if (!tokenAddress.supportsInterface(type(IERC721).interfaceId)) {
            return false;
        }
        return IERC721(tokenAddress).balanceOf(caller) > 0;
    }

    function validateConfig(bytes calldata configData) external view override {
        address tokenAddress = abi.decode(configData, (address));
        require(tokenAddress.supportsInterface(type(IERC721).interfaceId), "TokenGatedHook: Invalid token address");
    }

    function supportsInterface(bytes4 interfaceId) public view virtual override(BaseModule, IERC165) returns (bool) {
        return interfaceId == type(IHookModule).interfaceId || super.supportsInterface(interfaceId);
    }
}

```

Модуль `TokenGatedHook` может применяться в качестве `commercializerChecker` в LicensingModule (Модуль Лицензирования). В данном случае он позволяет выдавать лицензию только тем, кто владеет определённым токеном NFT.

```
        licensingModule.registerPolicyFrameworkManager(address(pilManager));

        PILPolicy memory policyData = PILPolicy({
            attribution: true,
            commercialUse: true,
            commercialAttribution: false,
            commercializerChecker: address(0),
            commercializerCheckerData: "",
            commercialRevShare: 100,
            derivativesAllowed: false,
            derivativesAttribution: false,
            derivativesApproval: false,
            derivativesReciprocal: false,
            territories: new string[](1),
            distributionChannels: new string[](1),
            contentRestrictions: emptyStringArray
        });

        gatedNftFoo.mintId(address(this), 1);

        MockTokenGatedHook tokenGatedHook = new MockTokenGatedHook();
        policyData.commercializerChecker = address(tokenGatedHook);
       // address(this) не владеет токеном из коллекции gatedNftBar, поэтому проверка не будет пройдена.
        policyData.commercializerCheckerData = abi.encode(address(gatedNftBar));
        policyData.territories[0] = "territory1";
        policyData.distributionChannels[0] = "distributionChannel1";

        uint256 policyId = pilManager.registerPolicy(
            RegisterPILPolicyParams({
                transferable: true,
                royaltyPolicy: address(mockRoyaltyPolicyLAP),
                mintingFee: 0,
                mintingFeeToken: address(0),
                policy: policyData
            })
        );

        vm.prank(ipOwner);
        licensingModule.addPolicyToIp(ipId1, policyId);
        licensingModule.mintLicense(policyId, ipId1, 1, licenseHolder, "");
```
