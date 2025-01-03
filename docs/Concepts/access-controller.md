---
title: 🔒 Контроллер Доступа
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
<Image align="center" src="https://files.readme.io/ff607ff-Screenshot_2024-01-23_at_14.30.19.png" />

Контроллер доступа управляет всеми состояниями и проверками разрешений в Story Protocol. В частности, он поддерживает *таблицу разрешений* и *движок разрешений* для обработки и хранения прав. Разрешения IPAccount устанавливаются владельцем IPAccount.

## Таблица разрешений

### Журнал Разрешений

| IPAccount  | Signer (caller) | To (only module) | Function Sig | Permission |
| ---------- | --------------- | ---------------- | ------------ | ---------- |
| 0x123..111 | 0x789..222      | 0x790..333       | 0xAaAaAaAa   | Allow      |
| 0x123..111 | 0x789..222      | 0x790..333       | 0xBBBBBBBB   | Deny       |
| 0x123..111 | 0x789..222      | 0x790..333       | 0xCCCCCC     | Abstain    |

Каждая запись определяет разрешение (**Permission**) в виде вызвавшего адреса (**Signer**), который вызывает функцию (**Func**) модуля (**To**) от имени IPAccount.

Поле разрешения может быть установлено как: "Allow," "Deny," или "Abstain." Abstain - воздержаться (решение о разрешении принимается на уровне выше).

### Поддержка универсальных шаблонов (Wildcard)

Шаблоны разрешений (wildcards) позволяют определить права, применимые сразу к нескольким модулям и/или функциям. Это упрощает создание списков разрешений (whitelist) или запрещений (blacklist).

<Table>
  <thead>
    <tr>
      <th>
        IPAccount
      </th>

      <th>
        Signer (caller)
      </th>

      <th>
        To (module)
      </th>

      <th>
        Func
      </th>

      <th>
        Permission
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        0x123..111
      </td>

      <td>
        0x789..222
      </td>

      <td>
        \*
      </td>

      <td>
        \*
      </td>

      <td>
        Allow
      </td>
    </tr>

    <tr>
      <td>
        0x123..111
      </td>

      <td>
        0x789..222
      </td>

      <td>
        0x790..333
      </td>

      <td>
        \*
      </td>

      <td>
        Deny
      </td>
    </tr>
  </tbody>
</Table>

В примере (0x789..222) не может вызывать функции модуля (0x790..333) от имени IPAccount (0x123..111).

Другими словами, IPAccount запретил адресу вызов функций в модуле 0x790...333


* Поддерживаемые универсальные шаблоны:

| Параметр                | Шаблон   |
| -------------------------- | ---------- |
| Func                       | bytes4(0)  |
| Addresses (IPAccount / To) | address(0) |

### Приоритет разрешений

Более конкретные разрешения имеют приоритет над общими.


<Table>
  <thead>
    <tr>
      <th>
        IPAccount
      </th>

      <th>
        Signer (caller)
      </th>

      <th>
        To (module)
      </th>

      <th>
        Func
      </th>

      <th>
        Permission
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        0x123..111
      </td>

      <td>
        0x789..222
      </td>

      <td>
        \*
      </td>

      <td>
        \*
      </td>

      <td>
        Allow
      </td>
    </tr>

    <tr>
      <td>
        0x123..111
      </td>

      <td>
        0x789..222
      </td>

      <td>
        0x790..333
      </td>

      <td>
        \*
      </td>

      <td>
        Deny
      </td>
    </tr>

    <tr>
      <td>
        0x123..111
      </td>

      <td>
        0x789..222
      </td>

      <td>
        0x790..333
      </td>

      <td>
        0xCCCCDDDD
      </td>

      <td>
        Allow
      </td>
    </tr>
  </tbody>
</Table>

Пример показывает что (0x789..222) не может вызывать функции модуля (0x790..333), кроме функции 0xCCCCDDDD.

Однако, (0x789..222) может вызывать любые функции других модулей от имени IPAccount (0x123..111).


<br />

## Потоки вызовов с контролем доступа

Существует три типа потоков вызовов, которые проверяются Контроллером доступа:

1. IPAccount вызывает модуль напрямую.
2. Модуль вызывает другой модуль.
3. Модуль вызывает реестр.

### IPAccount вызывает модуль напрямую

* IPAccount проверяет разрешения через Контроллер Доступа.
* Модуль проверяет только, является ли `msg.sender` валидным IPAccount.

When calling a module from an IPAccount, the IPAccount performs an access control check with AccessController to determine if the current caller has permission to make the call. In the module, it only needs to check whether the transaction `msg.sender` is a valid IPAccount.

`AccessControlled` предоставляет метод `onlyIpAccount()` который помогает проверить доступ.

```solidity Solidity
contract MockModule is IModule, AccessControlled {
    function action(string memory param) external view onlyIpAccount() returns (string memory) {
            // do something
    }
}
```

<Image align="center" src="https://files.readme.io/6a835ae-Screenshot_2024-01-22_at_17.18.49.png" />

## Модуль вызывает другой модуль

* Модуль, который вызывает другой модуль, должен выполнить проверку доступа самостоятельно.

Когда модуль вызывает другой модуль, он отвечает за проверку контроля доступа через Контроллер Доступа. Эта проверка определяет, имеет ли текущий вызывающий право вызывать функции другого модуля.

`AccessControlled` предоставляет метод `verifyPermission(address ipAccount)` который помогает проверить доступ.

```coffeescript Solidity
contract MockModule is IModule, AccessControlled {
    function callFromAnotherModule(address ipAccount) external verifyPermission(ipAccount) returns (string memory) {
        if (!IAccessController(accessController).checkPermission(ipAccount, msg.sender, address(this), this.callFromAnotherModule.selector)) {
		        revert Unauthorized();
        }
			  // сделать что то
    }
}
```

<Image align="center" src="https://files.readme.io/767f852-Screenshot_2024-01-22_at_17.19.07.png" />

## Модуль вызывает реестр (Registry)

* Реестр выполняет проверку авторизации через Контроллер Доступа.
* Разрешения задаются глобально через администратора.

Когда модуль вызывает реестр, реестр выполняет проверку контроля доступа через Контроллер Доступа. Эта проверка подтверждает, что модуль имеет право вызывать функции реестра.

```solidity Solidity
// called by StoryProtocl Admin
IAccessController(accessController).setGlobalPermission(address(0), address(module), address(registry), bytes4(0))) {

```

```solidity Solidity
contract MockRegistry {
    function registerAction() external returns (string memory) {
        if (!IAccessController(accessController).checkPermission(address(0), msg.sender, address(this), this.registerAction.selector)) {
		        revert Unauthorized();
        }
			  // сделать что то
    }
}
```

<Image align="center" src="https://files.readme.io/3d24a42-Screenshot_2024-01-24_at_09.45.06.png" />

> 📘 Разрешения IPAccount аннулируются при передаче права собственности.
>
> Права, связанные с IPAccount, принадлежат только текущему владельцу. При передаче IPAccount новому владельцу все ранее предоставленные разрешения автоматически аннулируются. Если право собственности возвращается прежнему владельцу, его первоначальные разрешения восстанавливаются.
