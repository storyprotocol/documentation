---
title: 🧱 Модули
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
Modules are standalone contracts that adhere to the [`IModule` interface](https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/interfaces/modules/base/IModule.sol). These modules play a crucial role in taking action on IP to change the data/state around or of IP.

Модули — это независимые контракты, которые соответствуют [интерфейсу `IModule`](https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/interfaces/modules/base/IModule.sol). Эти модули играют ключевую роль в выполнении действий с интеллектуальной собственностью (IP), изменяя данные/состояние, связанные с ней или её частью.

# Существующие Модули

Существует несколько важных модулей, созданных командой Story, с которыми стоит ознакомиться:

## [📜 Модуль Лицензирования](doc:licensing-module)

Отвечает за определение и привязку лицензий к IP-активам.

## [💸 Модуль Роялти](doc:royalty-module)

Отвечает за управление потоком роялти между родительскими и дочерними IP-активами.

## [❌ Модуль Споров](doc:dispute-module)

Отвечает за обработку споров по неправомерно зарегистрированным или неправомерно использованным IP-активам.

## [👥 Модуль Группировки](doc:grouping-module)

Отвечает за управление группами IP-активов.
