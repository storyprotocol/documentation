---
title: Реестр групповых IP-активов
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
> Ознакомьтесь со смарт-контрактом [тут](https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/registries/GroupIPAssetRegistry.sol).

Реестр групповых IP-активов отвечает за управление регистрацией и отслеживанием групповых IP-активов, включая их участников и пулы вознаграждений.

Реестр групповых IP-активов хранит данные о связях между групповым IP-аккаунтом и индивидуальными IP-аккаунтами на блокчейне с использованием следующего отображения:

```sol GroupIPAssetRegistry.sol
mapping(address groupIpId => EnumerableSet.AddressSet memberIpIds) groups;
```

### Основные функции

```sol GroupIPAssetRegistry.sol
function registerGroup(address groupNft, uint256 groupNftId, address rewardPool) external onlyGroupingModule whenNotPaused returns (address groupId)
```

Эта функция регистрирует новый групповой IP-актив в Story.

```sol GroupIPAssetRegistry.sol
function addGroupMember(address groupId, address[] calldata ipIds) external onlyGroupingModule whenNotPaused
```

Добавляет уже зарегистрированные IP-активы в существующий групповой IP-актив.

```sol GroupIPAssetRegistry.sol
function removeGroupMember(address groupId, address[] calldata ipIds) external onlyGroupingModule whenNotPaused
```

Удаляет зарегистрированные IP-активы из группового IP-актива.
