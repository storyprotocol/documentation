---
title: Условия PIL
excerpt: ""
deprecated: false
hidden: false
metadata:
  title: ""
  description: ""
  robots: index
next:
  description: ""
---

> 👍 Упрощенный режим: У нас есть готовые условия PIL
>
> [Готовые варианты условий PIL](https://docs.story.foundation/docs/pil-flavors-preset-policy).

PIL (Программируемая Лицензия IP) — это первая лицензия для медийных лицензий, разработанная Story Protocol и вдохновленная [Token Bound License](https://james.grimmelmann.net/files/articles/token-bound-nft-license.pdf). Если вы еще не знакомы, прочитайте обзор:[Программируемая Лицензия IP (PIL💊)](doc:programmable-ip-license).

> 📘 Текст PIL
>
> Ознакомьтесь с полным текстом PIL [здесь](https://github.com/storyprotocol/protocol-core-v1/blob/main/PIL_Testnet.pdf). Он написан простым и понятным языком как для юридического документа!

# Условия на блокчейне

Большинство условий PIL реализованы на блокчейне. Они включены в контракт `IPILicenseTemplate` как структура `PILTerms`. Ознакомьтесь с исходным кодом [здесь](https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/interfaces/modules/licensing/IPILicenseTemplate.sol).

```sol IPILicenseTemplate.sol
/// @notice Эта структура определяет условия для PIL.
/// Эти условия могут быть применены к IP-активам.
struct PILTerms {
  bool transferable;
  address royaltyPolicy;
  uint256 defaultMintingFee;
  uint256 expiration;
  bool commercialUse;
  bool commercialAttribution;
  address commercializerChecker;
  bytes commercializerCheckerData;
  uint32 commercialRevShare;
  uint256 commercialRevCeiling;
  bool derivativesAllowed;
  bool derivativesAttribution;
  bool derivativesApproval;
  bool derivativesReciprocal;
  uint256 derivativeRevCeiling;
  address currency;
  string uri;
}
```

## Описание параметров

<Table align={[null,"left",null]}>
  <thead>
    <tr>
      <th>
        Параметр
      </th>

      <th style={{ textAlign: "left" }}>
        Значения
      </th>

      <th>
        Описание
      </th>
    </tr>

  </thead>

  <tbody>
    <tr>
      <td>
        `transferable`
      </td>

      <td style={{ textAlign: "left" }}>
        True/False
      </td>

      <td>
        Если False, токен лицензии нельзя будет передать другому адресу.
      </td>
    </tr>

    <tr>
      <td>
        `royaltyPolicy`
      </td>

      <td style={{ textAlign: "left" }}>
        Адрес
      </td>

      <td>
        Адрес контракта политики роялти, который должен быть предварительно одобрен Story Protocol.
      </td>
    </tr>

    <tr>
      <td>
        `defaultMintingFee`
      </td>

      <td style={{ textAlign: "left" }}>
        \#
      </td>

      <td>
        Комиссия за создание лицензии.
      </td>
    </tr>

    <tr>
      <td>
        `expiration`
      </td>

      <td style={{ textAlign: "left" }}>
        \#
      </td>

      <td>
        Срок действия лицензии (в блоках или секундах).
      </td>
    </tr>

    <tr>
      <td>
        `commercialUse`
      </td>

      <td style={{ textAlign: "left" }}>
        True/False
      </td>

      <td>
        Можно ли зарабатывать деньги, используя оригинал.
      </td>
    </tr>

    <tr>
      <td>
        `commercialAttribution`
      </td>

      <td style={{ textAlign: "left" }}>
        True/False
      </td>

      <td>
        Требуется ли указание авторства при коммерческом использовании.
      </td>
    </tr>

    <tr>
      <td>
        `commercializerChecker`
      </td>

      <td style={{ textAlign: "left" }}>
        Адрес
      </td>

      <td>
        Контракт, определяющий, кто может коммерчески использовать актив. Если адрес пустой, ограничений нет
      </td>
    </tr>

    <tr>
      <td>
        `commercializerCheckerData`
      </td>

      <td style={{ textAlign: "left" }}>
        Bytes
      </td>

      <td>
        Данные для проверки коммерциализации.
      </td>
    </tr>

    <tr>
      <td>
        `commercialRevShare`
      </td>

      <td style={{ textAlign: "left" }}>
        # [0-100,000,000]
      </td>

      <td>
        Процент дохода, который должен быть передан лицензиару (например, 10,000,000 = 10%).

        Весь доход от токенов которые внесены в белый список контракта [RoyaltyModule.sol](https://github.com/storyprotocol/protocol-core-v1/blob/e339f0671c9172a6699537285e32aa45d4c1b57b/contracts/modules/royalty/RoyaltyModule.sol#L50).
      </td>
    </tr>

    <tr>
      <td>
        `commercialRevCelling`
      </td>

      <td style={{ textAlign: "left" }}>
        \#
      </td>

      <td>
        Максимальный доход, который можно заработать на оригинале.
      </td>
    </tr>

    <tr>
      <td>
        `derivativesAllowed`
      </td>

      <td style={{ textAlign: "left" }}>
        True/False
      </td>

      <td>
        Разрешено ли создавать производные работы.
      </td>
    </tr>

    <tr>
      <td>
        `derivativesAttribution`
      </td>

      <td style={{ textAlign: "left" }}>
        True/False
      </td>

      <td>
        Требуется ли указывать авторство для производных работ.
      </td>
    </tr>

    <tr>
      <td>
        `derivativesApproval`
      </td>

      <td style={{ textAlign: "left" }}>
        True/False
      </td>

      <td>
        Требуется ли одобрение от владельца для создания производных работ.
      </td>
    </tr>

    <tr>
      <td>
        `derivativesReciprocal`
      </td>

      <td style={{ textAlign: "left" }}>
        True/False
      </td>

      <td>
       	Производные работы могут быть использованы для создания новых производных только при соблюдении тех же условий.
      </td>
    </tr>

    <tr>
      <td>
        `derivativeRevCelling`
      </td>

      <td style={{ textAlign: "left" }}>
        \#
      </td>

      <td>
        Максимальный доход от производных работ.
      </td>
    </tr>

    <tr>
      <td>
        `currency`
      </td>

      <td style={{ textAlign: "left" }}>
        Address
      </td>

      <td>
        	Токен ERC20, используемый для оплаты лицензии. Токен должен быть зарегестрирован в Story.
      </td>
    </tr>

    <tr>
      <td>
        `uri`
      </td>

      <td style={{ textAlign: "left" }}>
        String
      </td>

      <td>
       URI с дополнительными условиями [офф-чейн лицензии](https://docs.story.foundation/v1/docs/pil-for-devs-and-creators#off-chain-parameters-to-be-included-in-uri-field).
      </td>
    </tr>

  </tbody>
</Table>

# Условия вне блокчейна (включаются в uri)

Некоторые параметры PIL хранятся вне блокчейна, поскольку они могут быть более длинными или описательными.

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        Параметр
      </th>

      <th style={{ textAlign: "left" }}>
        Описание
      </th>
    </tr>

  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        Территория
      </td>

      <td style={{ textAlign: "left" }}>
        Указывает географические ограничения (по умолчанию глобально).
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Каналы дистрибуции
      </td>

      <td style={{ textAlign: "left" }}>

Ограничивает использование IP в определенных форматах и каналах (по умолчанию доступно для всех каналов). Примеры: "телевидение", "физические товары", "видеоигры" и т.д.

</td>
</tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Атрибуция
      </td>

      <td style={{ textAlign: "left" }}>
        Указывает, нужно ли упоминать автора (по умолчанию не требуется).
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Стандарты контента
      </td>

      <td style={{ textAlign: "left" }}>
       Указывает стандарты для использования контента (по умолчанию ограничений нет). Примеры: "Без ненависти", "Подходит для всех возрастов", "Без наркотиков".
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Сублицензирование
      </td>

      <td style={{ textAlign: "left" }}>
        Производные работы могут передавать те же права третьим лицам без подтверждения автора оригинала (по умолчанию запрещено).
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Искусственный интеллект
      </td>

      <td style={{ textAlign: "left" }}>
        Разрешено ли использовать IP для разработки, обучения моделей ИИ (по умолчанию разрешено).
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Кроссплатформенность
      </td>

      <td style={{ textAlign: "left" }}>
       Можно ли использовать IP за пределами платформы, где она была опубликована (по умолчанию разрешено).
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Применимое законодательство
      </td>

      <td style={{ textAlign: "left" }}>
        Указывает законы, регулирующие лицензию (по умолчанию Калифорния, США).
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Альтернативное разрешение споров
      </td>

      <td style={{ textAlign: "left" }}>
       Секция 3.1 [тут](https://github.com/storyprotocol/protocol-core-v1/blob/main/PIL_Beta_Final_2024_02_Plain_English.pdf).
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Дополнительные параметры
      </td>

      <td style={{ textAlign: "left" }}>
        Лицензиар может добавлять дополнительные условия в этот раздел.
      </td>
    </tr>

  </tbody>
</Table>
