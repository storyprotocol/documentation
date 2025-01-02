---
title: Создание и Регистрация Модулей
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
Это руководство проведет вас через процесс создания модуля и его регистрации в Story Protocol, чтобы вы могли внести свой вклад в экосистему.


# Как создать Модуль


Создание модуля довольно простое. Вам нужно разработать смарт-контракт и реализовать интерфейс `IModule`. Рекомендуется наследоваться от `BaseModule` для удобства, но это не обязательно.

Ниже приведен пример создания HookModule (модуля хука):

```sol TokenGatedHook.sol
// SPDX-License-Identifier: MIT
pragma solidity 0.8.23;

import { IERC165 } from "@openzeppelin/contracts/utils/introspection/ERC165.sol";
import { ERC165Checker } from "@openzeppelin/contracts/utils/introspection/ERC165Checker.sol";
import { IERC721 } from "@openzeppelin/contracts/token/ERC721/IERC721.sol";
import { IHookModule } from "contracts/interfaces/modules/base/IHookModule.sol";
import { BaseModule } from "contracts/modules/BaseModule.sol";

/// @title Mock Token Gated Hook.
/// @notice Хук, проверяющий, является ли вызывающий адрес владельцем NFT.
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
        require(tokenAddress.supportsInterface(type(IERC721).interfaceId), "MockTokenGatedHook: Invalid token address");
    }

    function supportsInterface(bytes4 interfaceId) public view virtual override(BaseModule, IERC165) returns (bool) {
        return interfaceId == type(IHookModule).interfaceId || super.supportsInterface(interfaceId);
    }
}

```

# Как зарегистрировать ваш Модуль

После создания модуля следующим шагом будет его регистрация в Story Protocol, чтобы он стал доступен в экосистеме. Мы создали публичный репозиторий на GitHub для удобства подачи заявок [тут](https://github.com/storyprotocol/registered-modules). Вы можете зарегистрировать свой модуль здесь. Перед интеграцией в экосистему команда Story Protocol проведет тщательную проверку вашей заявки, чтобы убедиться, что ваш модуль безопасен и соответствует стандартам.

## Шаги для регистрации Модуля

Чтобы ваш модуль был проверен и добавлен в репозиторий, выполните следующие шаги:

1. **Сделайте форк [Репозитория](https://github.com/storyprotocol/registered-modules):** Сначала сделайте форк этого репозитория в своем аккаунте GitHub.
2. **Обновите список модулей:** Добавьте информацию о вашем модуле в соответствующий JSON-файл в репозитории, соблюдая следующую структуру:

```json

{
  "name": "YourModuleName",
  "address": "YourModuleAddress",
  "blockExplorerLink": "YourModuleBlockExplorerLink"
}
```

* Замените `YourModuleName`, `YourModuleAddress`, и `YourModuleBlockExplorerLink` на название вашего модуля, его адрес и ссылку на его страницу в блокчейн-эксплорере.
* Example:

```json json
{  
  "name": "LicensingModule",  
  "address": "0xd81fd78f557b457b4350cB95D20b547bFEb4D857",  
  "blockExplorerLink": "https://testnet.storyscan.xyz/address/0xd81fd78f557b457b4350cB95D20b547bFEb4D857?tab=contract">  
}
```

3. **Создайте Pull Request (PR)**: После того как вы добавите свой модуль, создайте pull request (PR) в этот репозиторий. Используйте предоставленный шаблон PR, чтобы включить всю необходимую информацию.
4. **Ожидайте проверки:** После того как ваш PR будет подан, его проверят. После утверждения и слияния PR, ваш модуль будет официально зарегистрирован и признан безопасным для использования в сообществе Story Protocol.

Мы с нетерпением ждем ваших вкладов и расширения экосистемы модулей Story Protocol! 😊
