---
title: IPA Стандарт Метаданных
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
> 🚧 Внимание: Все еще обсуждается
>
> Мы все еще пытаемся выяснить, как лучше определить стандарт метаданных IPA. В целях прозрачности нижеприведенный документ представляет собой наши мысли на данный момент, но может быть изменен по мере продвижения к выпуску нашего публичного Mainnet.

Это JSON метаданные, которые ассоциируются с IP-активом и хранятся в IP-аккаунте. Вы должны вызвать `setMetadata(...)` внутри IP-аккаунта, чтобы установить метаданные, а затем вызвать `metadata()`, чтобы прочитать их.


# Параметры и Структура

<Table align={["left","left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        Название Параметра
      </th>

      <th style={{ textAlign: "left" }}>
        Тип
      </th>

      <th style={{ textAlign: "left" }}>
        Описание 
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        `title`
      </td>

      <td style={{ textAlign: "left" }}>
        `string`
      </td>

      <td style={{ textAlign: "left" }}>
        Заголовок IP.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `description`
      </td>

      <td style={{ textAlign: "left" }}>
        `string`
      </td>

      <td style={{ textAlign: "left" }}>
        Описание IP.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `ipType`
      </td>

      <td style={{ textAlign: "left" }}>
        `string`
      </td>

      <td style={{ textAlign: "left" }}>
        Тип IP-актива, может быть определен автором произвольно. Например, «персонаж», «глава», «локация», «предметы», «музыка» и т. д.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `relationships`
      </td>

      <td style={{ textAlign: "left" }}>
        `IpRelationship[]`
      </td>

      <td style={{ textAlign: "left" }}>
        Подробная информация об отношениях с прямым родительским активом IPA, например `APPEARS_IN`, `FINETUNED_FROM` и т.д. Дополнительные примеры смотрите [здесь] (https://docs.story.foundation/docs/ipa-metadata-standard#relationship-types).
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `createdAt`
      </td>

      <td style={{ textAlign: "left" }}>
        `string`
      </td>

      <td style={{ textAlign: "left" }}>
       Дата/Время создания IP (ISO8601 или unix формат).  

        Это поле можно использовать для указания дат, которые не содержаться в блокчейне. Например, Гарри Поттер был опубликован 26 июня.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `watermarkImage`
      </td>

      <td style={{ textAlign: "left" }}>
        `string`
      </td>

      <td style={{ textAlign: "left" }}>
        Вспомогательное изображение. Может использоваться в качестве «оберточного» изображения для брендинга или водяных знаков.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `creators`
      </td>

      <td style={{ textAlign: "left" }}>
        `IpCreator[]`
      </td>

      <td style={{ textAlign: "left" }}>
        Массив информации о создателях. Тип "создатель" определен ниже.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `media`
      </td>

      <td style={{ textAlign: "left" }}>
        `IpMedia[]`
      </td>

      <td style={{ textAlign: "left" }}>
        Массив вспомогательных носителей. Тип "носитель" определен ниже.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `attributes`
      </td>

      <td style={{ textAlign: "left" }}>
        `IpAttribute[]`
      </td>

      <td style={{ textAlign: "left" }}>
        Массив пар ключ-значение, которые могут быть использованы для произвольного сопоставления. Тип атрибута определен ниже.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `appInfo`
      </td>

      <td style={{ textAlign: "left" }}>
        `StoryProtocolApp`
      </td>

      <td style={{ textAlign: "left" }}>
        Присваивается проверенному приложению из Story Protocol напрямую (пока что на основе запроса). Мы сопоставим каждый идентификатор приложения с именем
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `tags`
      </td>

      <td style={{ textAlign: "left" }}>
        `string[]`
      </td>

      <td style={{ textAlign: "left" }}>
        Любые теги, которые могут помочь в этом IPA.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `robotTerms`
      </td>

      <td style={{ textAlign: "left" }}>
        `IPRobotTerms`
      </td>

      <td style={{ textAlign: "left" }}>
        Позволяет установить режим «Не обучать» для конкретного агента.
      </td>
    </tr>
  </tbody>
</Table>

## Определения типов

```typescript IpCreator
type IpCreator = {
  name: string;
  address: Address;
  description?: string;
  image?: string;
  socialMedia?: IpCreatorSocial[];
  role?: string;
  contributionPercent: number; // add up to 100
}

type IpCreatorSocial = {
  platform: string;
  url: string;
};
```
```typescript IpMedia
type IpMedia = {
  name: string;
  url: string;
  mimeType: string;
}
```
```typescript IpAttribute
type IpAttribute = {
  key: string;
  value: string | number;
}
```
```typescript IpRelationship
type IpRelationship = {
  parentIpId: Address;
  type: string; // see "Relationship Types" docs below
}
```
```typescript StoryProtocolApp
type StoryProtocolApp = {
  id: string;
  name: string;
  website: string;
  action?: string;
}
```
```typescript IPRobotTerms
type IPRobotTerms = {
  userAgent: string;
  allow: string;
}
```

## Типы Отношений

Различные типы отношений, которые могут быть использованы для атрибута `relationships`.

### Отношения Story

1. **APPEARS\_IN** - A character APPEARS\_IN a chapter.

2. **BELONGS\_TO** - A chapter BELONGS\_TO a book.

3. **PART\_OF** - A book is PART\_OF a series.

4. **CONTINUES\_FROM** - A chapter CONTINUES\_FROM the previous one.

5. **LEADS\_TO** - An event LEADS\_TO a consequence.

6. **FORESHADOWS** - An event FORESHADOWS future developments.

7. **CONFLICTS\_WITH** - A character CONFLICTS\_WITH another character.

8. **RESULTS\_IN** - A decision RESULTS\_IN a significant change.

9. **DEPENDS\_ON** - A subplot DEPENDS\_ON the main plot.

10. **SETS\_UP** - A prologue SETS\_UP the story.

11. **FOLLOWS\_FROM** - A chapter FOLLOWS\_FROM the previous one.

12. **REVEALS\_THAT** - A twist REVEALS\_THAT something unexpected occurred.

13. **DEVELOPS\_OVER** - A character DEVELOPS\_OVER the course of the story.

14. **INTRODUCES** - A chapter INTRODUCES a new character or element.

15. **RESOLVES\_IN** - A conflict RESOLVES\_IN a particular outcome.

16. **CONNECTS\_TO** - A theme CONNECTS\_TO the main narrative.

17. **RELATES\_TO** - A subplot RELATES\_TO the central theme.

18. **TRANSITIONS\_FROM** - A scene TRANSITIONS\_FROM one setting to another.

19. **INTERACTED\_WITH** - A character INTERACTED\_WITH another character.

20. **LEADS\_INTO** - An event LEADS\_INTO the climax.?\
    **PARALLEL - story** happening in parallel or around the same timeframe 

### Отношения AI

1. **TRAINED\_ON** - A model is TRAINED\_ON a dataset.

2. **FINETUNED\_FROM** - A model is FINETUNED\_FROM a base model.

3. **GENERATED\_FROM** - An image is GENERATED\_FROM a fine-tuned model.

4. **REQUIRES\_DATA** - A model REQUIRES\_DATA for training.

5. **BASED\_ON** - A remix is BASED\_ON a specific workflow.

6. **INFLUENCES** - Sample data INFLUENCES model output.

7. **CREATES** - A pipeline CREATES a fine-tuned model.

8. **UTILIZES** - A workflow UTILIZES a base model.

9. **DERIVED\_FROM** - A fine-tuned model is DERIVED\_FROM a base model.

10. **PRODUCES** - A model PRODUCES generated images.

11. **MODIFIES** - A remix MODIFIES the base workflow.

12. **REFERENCES** - An AI-generated image REFERENCES original data.

13. **OPTIMIZED\_BY** - A model is OPTIMIZED\_BY specific algorithms.

14. **INHERITS** - A fine-tuned model INHERITS features from the base model.

15. **APPLIES\_TO** - A fine-tuning process APPLIES\_TO a model.

16. **COMBINES** - A remix COMBINES elements from multiple datasets.

17. **GENERATES\_VARIANTS** - A model GENERATES\_VARIANTS of an image.

18. **EXPANDS\_ON** - A fine-tuning process EXPANDS\_ON base capabilities.

19. **CONFIGURES** - A workflow CONFIGURES a model’s parameters.

20. **ADAPTS\_TO** - A fine-tuned model ADAPTS\_TO new data.

# Примеры Использования

```json Harry Potter
{
  "title": "Harry Potter and the Philosopher's Stone",
  "image": "link_to_book_cover",
  "createdAt": "1997-06-26T00:00:00", // June 26 1997
  "ipType": "literature",
  "creators": [
    {
      "name": "JK Rowling",
      "description": "Author",
      "socialMedia": [
        {
          "platform": "Wikipedia",
          "url": "https://en.wikipedia.org/wiki/J._K._Rowling"
        }
      ]
    },
    {
      "name": "Thomas Taylor",
      "description": "Illustrator"
    },
    {
      "name": "Bloomsbury Publishing",
      "description": "Publisher",
      "socialMedia": [
        {
          "platform": "Website",
          "url": "https://www.bloomsbury.com/"
        }
      ]
    }
  ],
  "media": [
    {
      "name": "ePub",
      "uri": "link_to_epub",
      "mimeType": "application/epub+zip"
    },
    {
      "name": "Book Summary PDF",
      "uri": "link_to_book_summary_pdf",
      "mimeType": "application/pdf"
    }
  ],
  "attributes": [
    {
      "key": "ISBN",
      "value": "978-0-7475-3269-0"
    },
    {
      "key": "Genre",
      "value": "Fantasy"
    }
  ]
}

```
```json Simple IP Character
{
  "title": "Kenta the Samurai Azuki",
  "creators": [
    {
      "name": "Kaiser",
      "bio": "Kaiser is an anime enthusiast.",
      "socialMedia": [
        {
          "platform": "Twitter",
          "url": "https://twitter.com/kentathesamurai"
        }
      ]
    },
    {
	    "name": "Azuki",
	    "bio": "Creator of Azuki collection",
    }
  ]
}
```
```json Physical Painting (RWA)
{
  "title": "CICLOPIROX OLAMINE, 2004",
  "creators": [
    {
      "name": "Damien Hirst",
      "description": "Damien Hirst, a poster boy for the Young British Artists who rose to prominence in late 1980s London, is one of the most notorious artists of his generation. He has pushed the limits of fine art and good taste with sculptures that comprise dead animals submerged in formaldehyde; innumerable spot paintings that appear mass-produced and can sell for millions of dollars; and the exuberantly tacky For the Love of God (2007), a human skull studded with 8,601 diamonds. Through his installations, sculptures, drawings, and paintings, Hirst explores themes including religion, mortality, and desire. Since 1988, when the artist developed and curated “Freeze,” a groundbreaking exhibition of his work and that of his Goldsmiths College peers, he has been the subject of major shows at Tate Modern in London, the National Gallery of Art in Washington, D.C., and the Rijksmuseum in Amsterdam. In 2008, Hirst controversially staged “Beautiful Inside my Head Forever,” an auction in which he sold his work directly to the public and raked in around $200 million for himself. His individual works have sold for more than $10 million at auction.      ",
      "socialMedia": [
        {
          "platform": "Instagram",
          "url": "https://www.instagram.com/damienhirst/"
        },
        {
          "platform": "Wikipedia",
          "url": "https://en.wikipedia.org/wiki/Damien_Hirst"
        }
      ]
    }
  ],
  "attributes": [
    {
      "key": "Materials",
      "value": "Etching on Hahnemühle paper"
    },
    {
      "key": "Size",
      "value": "45 1/10 x 44 3/10 in | 114.5 x 112.5 cm"
    },
    {
      "key": "Edition",
      "value": "Edition of 68"
    },
    {
      "key": "Signature",
      "value": "Hand-signed by artist, Signed and numbered by the artist"
    },
    {
      "key": "Certificate of authenticity",
      "value": "link_to_certificate"
    }
  ]
}
```
```json Music
// Example: https://explorer.story.foundation/ipa/0xD3eF4f98B91B5088FB4a840f539EfA4288703af0
{
  "title": "Rise Again",
  "description": "This NFT certifies that Rise Again was created by srivatsan_qb (ID: 4123743b-8ba6-4028-a965-75b79a3ad424), with data securely fetched and verified using the Reclaim Protocol from Suno.com",
  "ipType": "Music",
  "creators": [
    {
      "name": "srivatsan_qb",
      "description": "Creator"
    }
  ],
  "media": [
    {
      "name": "Rise Again",
      "url": "https://cdn1.suno.ai/937e3060-65c0-4934-acab-7d8cc05eb9a6.mp3",
      "mimeType": "audio/mpeg"
    }
  ],
  "attributes": [
    {
      "key": "Artist",
      "value": "srivatsan_qb"
    },
    {
      "key": "Artist ID",
      "value": "4123743b-8ba6-4028-a965-75b79a3ad424"
    },
    {
      "key": "Source",
      "value": "Suno.com"
    }
  ]
}
```
