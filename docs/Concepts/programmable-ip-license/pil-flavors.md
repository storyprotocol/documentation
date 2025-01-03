---
title: Варианты PIL (примеры)
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

[💊 Программируемая Лицензия IP (PIL)](doc:programmable-ip-license) хорошо настраивается, но для удобства использования мы поддерживаем популярные предварительно настроенные лицензионные условия (также известные как «варианты»). Мы ожидаем, что эти варианты будут наиболее востребованы:

## Вариант №1: Некоммерческий социальный ремикс

> 📘 Условия по умолчанию
>
> Этот вариант уже зарегистрирован как `licenseTermsId = 1` в Story. Кроме того, все объекты IP имеют эти условия по умолчанию.

Позвольте миру создавать и играть с вашей работой. Эта лицензия позволяет бесконечно свободно создавать ремиксы, отслеживая использование вашей работы и предоставляя вам полный кредит за оригинал. Похоже на TikTok с добавлением атрибуции.

### Что могут делать другие?

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        Могут
      </th>

      <th style={{ textAlign: "left" }}>
        Не могут
      </th>
    </tr>

  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        ✅ Делать ремиксы на эту работу
        (`derivativesAllowed == true`)
      </td>

      <td style={{ textAlign: "left" }}>
        ❌ Использовать оригинал и производные работы в коммерческих целях
        (`commercialUse == false`)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        ✅ Распространять ремиксы где угодно
      </td>

      <td style={{ textAlign: "left" }}>
        ❌ Присваивать авторство за ремиксы как за оригинал
        (`derivativesAttribution == true`)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        ✅ Указывать вас как автора оригинальной работы
        (`derivativesAttribution == true`)
      </td>

      <td style={{ textAlign: "left" }}>

      </td>
    </tr>

  </tbody>
</Table>

###  Значения параметров PIL

- **На блокчейне**:

```sol Solidity
PILTerms({
  transferable: true,
  royaltyPolicy: zeroAddress,
  defaultMintingFee: BigInt(0),
  expiration: BigInt(0),
  commercialUse: false,
  commercialAttribution: false,
  commercializerChecker: zeroAddress,
  commercializerCheckerData: "0x",
  commercialRevShare: 0,
  commercialRevCeiling: BigInt(0),
  derivativesAllowed: true,
  derivativesAttribution: true,
  derivativesApproval: false,
  derivativesReciprocal: true,
  derivativeRevCeiling: BigInt(0),
  currency: zeroAddress,
  uri: "",
});
```

- **Вне блокчейна:**

| Параметр                                          | Опции/Метки                                                                   |
| ------------------------------------------------- | ----------------------------------------------------------------------------- |
| Территория                                        | Без ограничений                                                               |
| Каналы распостранения                             | Без ограничений                                                               |
| Атрибуция                                         | Обязательно                                                                   |
| Стандарты контента                                | Без ограничений                                                               |
| Возможность сублицензирования                     | Нет                                                                           |
| Обучение ИИ                                       | Разрешено                                                                     |
| Ограничения по использованию на разных платформах | Нет                                                                           |
| Применимое право                                  | Калифорния                                                                    |
| Альтернативное разрешение споров                  | Метка: Alternative-Dispute-Resolution Ledger-Authoritative-Dispute-Resolution |
| Дополнительные параметры лицензии                 | Отсутствуют                                                                   |

## Вариант №2: Коммерческое использование

Сохраняйте контроль над использованием вашей работы, позволяя другим использовать её на условиях, которые вы устанавливаете. Похоже на Shutterstock с правилами, установленными создателем.

### Что могут делать другие?

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        Могут
      </th>

      <th style={{ textAlign: "left" }}>
        Не могут
      </th>
    </tr>

  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        ✅ Покупать право использовать вашу работу
        (`defaultMintingFee` задана)
      </td>

      <td style={{ textAlign: "left" }}>
        ❌ Присваивать авторство за оригинальную работу
        (`commercialAttribution == true`)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        ✅ Использовать оригинал и производные работы в коммерческих целях
        (`commercialUse == true`)
      </td>

      <td style={{ textAlign: "left" }}>

      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        ✅ Распространять ремиксы где угодно
      </td>

      <td style={{ textAlign: "left" }}>

      </td>
    </tr>

  </tbody>
</Table>

### Значения параметров PIL

- **На блокчейне**:

```sol Solidity
PILTerms({
  transferable: true,
  royaltyPolicy: ROYALTY_POLICY, // ex. RoyaltyPolicyLAP address
  defaultMintingFee: BigInt(100), // ex. costs 100 SUSD to mint
  expiration: BigInt(0),
  commercialUse: true,
  commercialAttribution: true,
  commercializerChecker: zeroAddress,
  commercializerCheckerData: zeroAddress,
  commercialRevShare: 0,
  commercialRevCeiling: BigInt(0),
  derivativesAllowed: true,
  derivativesAttribution: true,
  derivativesApproval: false,
  derivativesReciprocal: false,
  derivativeRevCeiling: BigInt(0),
  currency: CURRENCY, // ex. SUSD address
  uri: "",
})
```

- **Вне блокчейна**

| Параметр                                          | Опции/Метки                                                                   |
| ------------------------------------------------- | ----------------------------------------------------------------------------- |
| Территория                                        | Без ограничений                                                               |
| Каналы распостранения                             | Без ограничений                                                               |
| Атрибуция                                         | Обязательно                                                                   |
| Стандарты контента                                | Без ограничений                                                               |
| Возможность сублицензирования                     | Нет                                                                           |
| Обучение ИИ                                       | Разрешено                                                                     |
| Ограничения по использованию на разных платформах | Нет                                                                           |
| Применимое право                                  | Калифорния                                                                    |
| Альтернативное разрешение споров                  | Метка: Alternative-Dispute-Resolution Ledger-Authoritative-Dispute-Resolution |
| Дополнительные параметры лицензии                 | Отсутствуют                                                                   |

## Вариант №3: Коммерческий ремикс

Позвольте миру использовать и развивать вашу работу... и зарабатывать на этом деньги вместе с вами! Эта лицензия разрешает бесконечный бесплатный ремиксинг с отслеживанием всех случаев использования вашей работы и предоставлением вам полного признания, при этом каждая производная работа платит процент от выручки своей "родительской" IP.

### Что могут делать другие?

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        Могут
      </th>

      <th style={{ textAlign: "left" }}>
        Не могут
      </th>
    </tr>

  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        ✅ Создавать ремиксы на эту работу  
        (`derivativesAllowed == true`)
      </td>

      <td style={{ textAlign: "left" }}>
        ❌ Присваивать себе авторство ремикса как оригинальной работы  
        (`derivativesAttribution == true`)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        ✅ Распространять свои ремиксы где угодно
      </td>

      <td style={{ textAlign: "left" }}>
        ❌ Присваивать себе авторство оригинальной работы  
        (`commercialAttribution == true`)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        ✅ Указывать автора оригинала  
        (`derivativesAttribution == true`)
      </td>

      <td style={{ textAlign: "left" }}>
        ❌  Претендовать на весь доход от коммерческого использования оригинальной или производной работы
        (`commercialRevShare` is set)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        ✅ Коммерчески использовать оригинальные и производные работы  
        (`commercialUse == true`)
      </td>

      <td style={{ textAlign: "left" }}>

      </td>
    </tr>

  </tbody>
</Table>

###  Значения условий PIL

- **На блокчейне**:

```sol Solidity
PILTerms({
  transferable: true,
  royaltyPolicy: ROYALTY_POLICY, // ex. RoyaltyPolicyLAP address
  defaultMintingFee: BigInt(100), // ex. costs 100 SUSD to mint
  expiration: BigInt(0),
  commercialUse: true,
  commercialAttribution: true,
  commercializerChecker: zeroAddress,
  commercializerCheckerData: zeroAddress,
  commercialRevShare: 50, // ex. can claim 50% of derivative revenue
  commercialRevCeiling: BigInt(0),
  derivativesAllowed: true,
  derivativesAttribution: true,
  derivativesApproval: false,
  derivativesReciprocal: true,
  derivativeRevCeiling: BigInt(0),
  currency: SUSD, // ex. SUSD address
  uri: "",
});
```

- **Вне блокчейна**


| Параметр                                          | Опции/Метки                                                                   |
| ------------------------------------------------- | ----------------------------------------------------------------------------- |
| Территория                                        | Без ограничений                                                               |
| Каналы распостранения                             | Без ограничений                                                               |
| Атрибуция                                         | Обязательно                                                                   |
| Стандарты контента                                | Без ограничений                                                               |
| Возможность сублицензирования                     | Нет                                                                           |
| Обучение ИИ                                       | Разрешено                                                                     |
| Ограничения по использованию на разных платформах | Нет                                                                           |
| Применимое право                                  | Калифорния                                                                    |
| Альтернативное разрешение споров                  | Метка: Alternative-Dispute-Resolution Ledger-Authoritative-Dispute-Resolution |
| Дополнительные параметры лицензии                 | Отсутствуют                                                                   |

# Примеры

Ниже приведены примеры потока роялти. Скоро появятся новые примеры!

## Пример 1

<Image align="center" src="https://files.readme.io/574c9f3-Screenshot_2024-08-16_at_9.54.00_PM.png" />

### Объяснение

Кто-то регистрирует своего персонажа Azuki на платформе Story. По умолчанию этот IP-актив (IPA1) имеет условия "Некоммерческий социальный ремикс", которые разрешают любому создавать производные работы, но запрещают их коммерческое использование. Затем кто-то создает и регистрирует ремикс на этот актив (IPA2), унаследовав те же условия. Еще один человек создает ремикс на IPA2, регистрируя IPA3.

Владелец IPA1 затем решает разрешить коммерческое использование своей работы, но без создания производных работ. Для этого требуется уплата комиссии за выпуск лицензии в размере 10 USDC, а также передача 10% от всего полученного дохода. Например, кто-то хочет использовать IPA1 на футболке. Он платит 10 USDC за выпуск токена лицензии, представляющего право на коммерческое использование IPA1. Затем он размещает изображение на футболке и продает ее. 10% дохода от продаж должны быть отправлены владельцу IPA1 через блокчейн.

## Пример 2

<Image align="center" src="https://files.readme.io/e3c7fbf-Screenshot_2024-08-16_at_9.54.16_PM.png" />

### Объяснение

Кто-то регистрирует своего персонажа Azuki на платформе Story. По умолчанию этот IP-актив (IPA1) имеет условия "Некоммерческий социальный ремикс", которые разрешают любому создавать производные работы, но запрещают их коммерческое использование. Затем кто-то создает и регистрирует ремикс на этот актив (IPA2), унаследовав те же условия. Еще один человек создает ремикс на IPA2, регистрируя IPA3.

Владелец IPA1 затем решает, что другие могут создавать производные работы и коммерчески их использовать. Однако они должны уплатить комиссию за выпуск лицензии в размере 10 USDC и отчислять 10% от полученного дохода. Например, кто-то хочет использовать IPA1 на футболке. Он платит 10 USDC за выпуск лицензии и "сжигает" этот токен, чтобы создать собственный ремикс, изменив, например, цвет фона на красный. Затем он размещает ремиксированное изображение на футболке и продает ее. 10% дохода от продаж футболки должны быть отправлены владельцу IPA1 через блокчейн.

Третий человек хочет коммерчески использовать ремикс, разместив его в телевизионной рекламе, но при этом хочет изменить цвет волос персонажа на белый. Для этого он платит 10 USDC за выпуск лицензии (1 USDC из которых отправляется владельцу IPA1), чтобы создать свой ремикс. Затем он размещает ремиксированное изображение в телевизионной рекламе. 10% дохода от рекламы должны быть отправлены владельцу IPA2 через блокчейн, из которых 10% перераспределяются обратно владельцу IPA1.
