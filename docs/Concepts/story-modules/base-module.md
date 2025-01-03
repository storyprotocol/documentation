---
title: Базовый Модуль
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
Базовый Модуль предоставляет стандартный набор обязательных функций для всех модулей, зарегистрированных в Story Protocol. Любой, кто хочет создать и зарегистрировать модуль в Story Protocol, должен унаследовать и переопределить Базовый Модуль.

> 🗒️ Контракт
>
> Ознакомьтесь со смарт-контрактом [тут](https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/modules/BaseModule.sol).

# Отличительные черты

## Простота и гибкость

BaseModule намеренно создан простым и обобщенным. Он реализует только интерфейс ERC165, который необходим для определения интерфейсов. Такой подход обеспечивает максимальную гибкость при разработке более специализированных модулей в рамках Story Protocol.

## Реализация интерфейса ERC165

Реализуя интерфейс ERC165, BaseModule позволяет другим контрактам проверять, поддерживает ли он определенный интерфейс. Эта функция важна для обеспечения совместимости и взаимодействия внутри экосистемы Story Protocol и за ее пределами.

```sol BaseModule.sol
abstract contract BaseModule is ERC165, IModule {
    ...
}
```

## Функция `supportsInterface`

Ключевая функция в BaseModule — это `supportsInterface`, которая переопределяет метод `supportsInterface` из ERC165. Эта функция играет важную роль в определении интерфейсов, позволяя контракту заявлять о поддержке как собственного интерфейса `IModule`, так и любых других интерфейсов, которые он может наследовать.

```sol BaseModule.sol
function supportsInterface(bytes4 interfaceId) public view virtual override(ERC165, IERC165) returns (bool) {
    return interfaceId == type(IModule).interfaceId || super.supportsInterface(interfaceId);
}
```
