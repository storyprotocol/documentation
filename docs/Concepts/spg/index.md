---
title: 📦 Протоколный шлюз Story (SPG)
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
SPG (Story Protocol Gateway) — это набор периферийных/утилитарных смарт-контрактов, развернутых в рамках нашего протокола. Они позволяют **объединять независимые операции** (например, регистрацию [🧩 IP-актива](doc:ip-asset) и прикрепление лицензионных условий к этому IP-активу) **в одну транзакцию для упрощения процесса**.

> 🗒️ Контракты
>
> Ознакомьтесь со смарт-контрактами [тут](https://github.com/storyprotocol/protocol-periphery-v1/tree/main/contracts).

Функция `mintAndRegisterIpAndAttachPILTerms`, являющаяся частью SPG (в частности, "Workflow для прикрепления лицензий"), позволяет в одной транзакции создать NFT, зарегистрировать его как IP-актив и прикрепить к нему лицензионные условия.

```sol LicenseAttachmentWorkflows.sol
function mintAndRegisterIpAndAttachPILTerms(
  address nftContract,
  address recipient,
  IPMetadata calldata ipMetadata,
  PILTerms calldata terms
) external onlyCallerWithMinterRole(nftContract) returns (address ipId, uint256 tokenId, uint256 licenseTermsId)
```

## Поддерживаемые Workflow

В SPG реализовано множество функций, которые объединяют несколько операций в одну. Для удобства они разделены на категории, называемые "Workflow".

> 📘 Документация Workflow
>
> Приведенный ниже список Workflow — это копия [этого документа](https://github.com/storyprotocol/protocol-periphery-v1/blob/main/docs/WORKFLOWS.md). Мы будем стараться поддерживать его актуальность, однако для последней версии обращайтесь к оригинальному документу.

### [Workflow для регистрации](../contracts/interfaces/workflows/IRegistrationWorkflows.sol)

* `createCollection`: Создание коллекции SPGNFT.
* `registerIp`:  Регистрация IP-актива.
* `mintAndRegisterIp`: Создание NFT → Регистрация его как IP-актива.

### [Workflow для прикрепления лицензии](../contracts/interfaces/workflows/ILicenseAttachmentWorkflows.sol)

* `registerPILTermsAndAttach`: Регистрация PIL-условий → Прикрепление их к IP.
* `registerIpAndAttachPILTerms`:  Регистрация IP → Регистрация PIL-условий → Прикрепление их к IP.
* `mintAndRegisterIpAndAttachPILTerms`: Создание NFT → Регистрация его как IP → Регистрация PIL-условий → Прикрепление их к IP.

### [Workflow для производных активов](../contracts/interfaces/workflows/IDerivativeWorkflows.sol)

* `registerIpAndMakeDerivative`: Регистрация IP → Определение его как производного от другого IP.
* `mintAndRegisterIpAndMakeDerivative`: Создание NFT → Регистрация его как IP → Определение его как производного от другого IP.
* `registerIpAndMakeDerivativeWithLicenseTokens`: Регистрация IP → Определение его как производного с использованием лицензионных токенов.
* `mintAndRegisterIpAndMakeDerivativeWithLicenseTokens`: Создание NFT → Регистрация его как IP → Определение его как производного с использованием лицензионных токенов.

### [Workflow для группировки](../contracts/interfaces/workflows/IGroupingWorkflows.sol)

* `mintAndRegisterIpAndAttachLicenseAndAddToGroup`: Создание NFT → Регистрация его как IP → Прикрепление лицензионных условий → Добавление в группу IP.
* `registerIpAndAttachLicenseAndAddToGroup`: Регистрация IP → Прикрепление лицензионных условий → Добавление в группу IP.
* `registerGroupAndAttachLicense`: Регистрация группы IP → Прикрепление лицензионных условий к группе.
* `registerGroupAndAttachLicenseAndAddIps`: Registers a group IP → Attaches the given license terms to the group IP → Adds existing IPs to the group IP

### [Workflow для роялти](../contracts/interfaces/workflows/IRoyaltyWorkflows.sol)

* `transferToVaultAndSnapshotAndClaimByTokenBatch`: Перевод доходных токенов в хранилище роялти IP → Создание снимка хранилища → Вывод доходов и роялти потомков IP.
  * *Применение*: Для владельцев роялти-токенов, которые хотят вывести доходы и роялти от потомков.
* `transferToVaultAndSnapshotAndClaimBySnapshotBatch`: Перевод токенов → Создание нового снимка → Вывод текущих доходов и доходов из предыдущих снимков.
  * *Применение*: Для владельцев роялти, которые хотят получить доходы за текущий и прошлые периоды.
* `snapshotAndClaimByTokenBatch`: Создание снимка (снепшота) → Вывод текущих доходов.
  * *Применение*: Для владельцев роялти, которые хотят получить текущий доход.
* `snapshotAndClaimBySnapshotBatch`: Создание снимка → Вывод доходов за текущий и предыдущие периоды.
  * *Применение*: Для владельцев роялти, желающих получить доходы за все периоды.

## Объединение вызовов

Несмотря на наличие функций, таких как `mintAndRegisterIpAndAttachPILTerms`и `registerIpAndAttachPILTerms`, их комбинации могут быть ограничены. Чтобы обеспечить максимальную гибкость, SPG предоставляет механизм "Multicall", позволяющий группировать вызовы в одну транзакцию.

Подробнее про "Multicall" [Пакетные вызовы функций](doc:batch-spg-function-calls).
