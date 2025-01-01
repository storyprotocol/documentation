---
title: Реестр IP-активов
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
> Ознакомьтесь со смарт-контрактом [тут](https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/registries/IPAssetRegistry.sol).

Реестр IP-активов отвечает за регистрацию IP в протоколе. Он разворачивает выделенный контракт [IP-аккаунта](doc:ip-account) для каждого нового IP-актива, зарегистрированного в протоколе (*ПРИМЕЧАНИЕ: Этот реестр наследуется от Реестра IP-аккаунтов*).

### Основные функции

```sol IPAssetRegistry.sol
function register(uint256 chainid, address tokenContract, uint256 tokenId) external whenNotPaused returns (address id)
```

Эта функция регистрирует ERC-721 NFT как новый IP-актив в Story.
