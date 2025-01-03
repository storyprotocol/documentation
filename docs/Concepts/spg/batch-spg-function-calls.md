---
title: Пакетные вызовы функций
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
## Предыстория

До внедрения нового подхода регистрация нескольких объектов IP или выполнение других операций, таких как выпуск токенов, добавление лицензионных условий и регистрация производных, требовали отдельной транзакции для каждой операции. Это могло быть неэффективным и дорогостоящим. Для оптимизации процесса теперь доступно два способа объединения нескольких транзакций в одну:

1. **Пакетные вызовы функций SPG:** Используйте встроенную функцию [`multicall`](https://docs.story.foundation/docs/batch-spg-function-calls#1-batch-spg-function-calls-via-built-in-multicall-function).
2. **Пакетные вызовы функций за пределами SPG:** Используйте контракт [Multicall3](https://docs.story.foundation/docs/batch-spg-function-calls#2-batch-function-calls-via-multicall3-contract).

***

## 1. Пакетные вызовы функций SPG с использованием встроенной функции `multicall`

SPG включает функцию `multicall`, которая позволяет объединить несколько операций чтения или записи в одну транзакцию.

### Определение функции

Функция `multicall` принимает массив закодированных данных вызова и возвращает массив закодированных результатов, соответствующих каждому вызову функции:

```solidity
/// @dev Выполняет пакетный вызов функций в этом контракте.
function multicall(bytes[] calldata data) external virtual returns (bytes[] memory results);
```

### Пример Использования

Допустим, вы хотите выпустить несколько NFT, зарегистрировать их как IP и связать их с производными объектами IP.

Для этого можно использовать функцию `multicall` SPG для пакетного вызова функции `mintAndRegisterIpAndMakeDerivative`.

Пример использования:

```solidity
// контракт рабочего процесса SPG: https://github.com/storyprotocol/protocol-periphery-v1/blob/main/contracts/workflows/DerivativeWorkflows.sol
contract DerivativeWorkflows {
    ...
    function mintAndRegisterIpAndMakeDerivative(
        address nftContract,
        MakeDerivative calldata derivData,
        IPMetadata calldata ipMetadata,
        address recipient
    ) external returns (address ipId, uint256 tokenId) {
        ....
    }
    ...
}
```

Пример пакетного вызова `mintAndRegisterIpAndMakeDerivative` с помощью функции `multicall`:

```javascript
// пакетное создание, регистрация и связывание производных для нескольких IP
await DerivativeWorkflows.multicall([
  DerivativeWorkflows.contract.methods.mintAndRegisterIpAndMakeDerivative(
      nftContract1,
      derivData1,
      recipient1,
      ipMetadata1,
  ).encodeABI(),
  
  DerivativeWorkflows.contract.methods.mintAndRegisterIpAndMakeDerivative(
      nftContract2,
      derivData2,
      recipient2,
      ipMetadata2,
  ).encodeABI(),
  
  DerivativeWorkflows.contract.methods.mintAndRegisterIpAndMakeDerivative(
      nftContract3,
      derivData3,
      recipient3,
      ipMetadata3,
  ).encodeABI(),
  ...
   // Добавьте больше вызовов по мере необходимости
]);
```

***

## 2. Пакетные вызовы функций через контракт Multicall3

> 🚧 Важно
>
> Контракт Multicall3 не полностью совместим с функциями SPG, связанными с выпуском SPGNFT, из-за особенностей контроля доступа и изменения контекста во время выполнения Multicall. Для таких операций используйте встроенную функцию [multicall SPG](https://docs.story.foundation/docs/batch-spg-function-calls#1-batch-spg-function-calls-via-built-in-multicall-function).

Контракт Multicall3 позволяет выполнить несколько вызовов функций в рамках одной транзакции и агрегировать результаты. Библиотека [`viem`](https://viem.sh/docs/contract/multicall#multicall) поддерживает Multicall3 на нативном уровне.

### Информация о развертывании Multicall3 в тестовой сети Story Odyssey

(Один и тот же адрес на всех сетях EVM)

```json
{
  "contractName": "Multicall3",
  "chainId": 1516,
  "contractAddress": "0xcA11bde05977b3631167028862bE2a173976CA11",
  "url": "https://odyssey.storyscan.xyz/address/0xcA11bde05977b3631167028862bE2a173976CA11"
}
```

### Основные функции

Для пакетного вызова функций используйте следующие функции:

1.**` aggregate3`**: Выполняет пакетный вызов с использованием структуры Call3.
2. **`aggregate3Value`**: Аналогична `aggregate3`, но позволяет также прикреплять значение к каждому вызову.

```solidity
/// @notice Агрегирует вызовы, обеспечивая успех каждого, если это требуется.
/// @param calls Массив структур Call3.
/// @return returnData Массив структур Result.
function aggregate3(Call3[] calldata calls) external payable returns (Result[] memory returnData);

/// @notice Агрегирует вызовы с прикрепленным значением msg.
/// @param calls Массив структур Call3Value.
/// @return returnData Массив структур Result.
function aggregate3Value(Call3Value[] calldata calls) external payable returns (Result[] memory returnData);
```

### Определения структур


* **Call3**: Используется в `aggregate3`.
* **Call3Value**: Используется в `aggregate3Value`.

```solidity
struct Call3 {
  address target;      // Адрес вызываемого контракта.
  bool allowFailure;   // Если false, вызов Multicall завершится неудачей при ошибке.
  bytes callData;      // Данные для вызова контракта.
}

struct Call3Value {
  address target;
  bool allowFailure;
  uint256 value;       // Значение (в wei), отправляемое с вызовом.
  bytes callData;      // Данные для вызова контракта.
}
```

### Тип возвращаемого значения

* **Result**: Структура возвращаемая `aggregate3` и `aggregate3Value`.

```solidity
struct Result {
  bool success;        // Успешность вызова.
  bytes returnData;    // Данные, возвращаемые функцией.
}
```

> 📘 Примеры
>
> Для подробных примеров на Solidity, TypeScript и Python, см. [репозиторий Multicall3](https://github.com/mds1/multicall/tree/main/examples).

### Ограничения

Список ограничений при использовании Multicall3 доступен в [Multicall3 README](https://github.com/mds1/multicall/blob/main/README.md#batch-contract-writes).

### Дополнительные ресурсы

* [Документация Multicall3](https://github.com/mds1/multicall/blob/main/README.md)
* [Документация Multicall от Viem](https://viem.sh/docs/contract/multicall#multicall)

### Полный интерфейс Multicall3

```solidity
interface IMulticall3 {
  struct Call {
      address target;
      bytes callData;
  }

  struct Call3 {
      address target;
      bool allowFailure;
      bytes callData;
  }

  struct Call3Value {
      address target;
      bool allowFailure;
      uint256 value;
      bytes callData;
  }

  struct Result {
      bool success;
      bytes returnData;
  }

  function aggregate(Call[] calldata calls) external payable returns (uint256 blockNumber, bytes[] memory returnData);

  function aggregate3(Call3[] calldata calls) external payable returns (Result[] memory returnData);

  function aggregate3Value(Call3Value[] calldata calls) external payable returns (Result[] memory returnData);

  function blockAndAggregate(Call[] calldata calls) external payable returns (uint256 blockNumber, bytes32 blockHash, Result[] memory returnData);

  function getBasefee() external view returns (uint256 basefee);

  function getBlockHash(uint256 blockNumber) external view returns (bytes32 blockHash);

  function getBlockNumber() external view returns (uint256 blockNumber);

  function getChainId() external view returns (uint256 chainid);

  function getCurrentBlockCoinbase() external view returns (address coinbase);

  function getCurrentBlockDifficulty() external view returns (uint256 difficulty);

  function getCurrentBlockGasLimit() external view returns (uint256 gaslimit);

  function getCurrentBlockTimestamp() external view returns (uint256 timestamp);

  function getEthBalance(address addr) external view returns (uint256 balance);

  function getLastBlockHash() external view returns (bytes32 blockHash);

  function tryAggregate(bool requireSuccess, Call[] calldata calls) external payable returns (Result[] memory returnData);

  function tryBlockAndAggregate(bool requireSuccess, Call[] calldata calls) external payable returns (uint256 blockNumber, bytes32 blockHash, Result[] memory returnData);
}
```
