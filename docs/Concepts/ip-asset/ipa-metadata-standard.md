---
title: IPA Metadata Standard
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
> 🚧 Warning: Still Under Discussion
>
> We are still figuring out the best way to define an IPA Metadata Standard. For the sake of transparency, the following document is our thoughts so far but is subject to change as we progress towards releasing our public Mainnet.

This is the JSON metadata that is associated with an IP Asset, and gets stored inside of an IP Account. You must call `setMetadata(...)` inside of the IP Account in order to set the metadata, and then call `metadata()` to read it.

# Attributes & Structure

<Table align={["left","left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        Property Name
      </th>

      <th style={{ textAlign: "left" }}>
        Type
      </th>

      <th style={{ textAlign: "left" }}>
        Description
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
        Title of the IP
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
        Description of the IP
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
        Type of the IP Asset, can be defined arbitrarily by the creator. I.e. “character”, “chapter”, “location”, “items”, "music", etc
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
        The detailed relationship info with the IPA’s direct parent asset, such as `APPEARS_IN`, `FINETUNED_FROM`, etc. See more examples [here](https://docs.story.foundation/docs/ipa-metadata-standard#relationship-types).
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
        Date/Time that the IP was created (either ISO8601 or unix format).

        This dateCreated field can be used to specify historical dates that aren’t on-chain. For example, Harry Potter was published on June 26.
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
        Supporting image. Could be used as a “wrapper” image for things related to branding or watermarks.
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
        An array of information about the creators. Creator type defined below.
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
        An array of supporting media. Media type defined below.
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
        An array of key-value pairs that can be used for arbitrary mappings. Attribute type defined below.
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
        This is assigned to verified application from Story Protocol directly (on a request basis so far). We will map each App ID to a name
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
        Any tags that can help surface this IPA
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
        Allows you to set Do Not Train for a specific agent
      </td>
    </tr>
  </tbody>
</Table>

## Type Definitions

```typescript IpCreator
type IpCreator = {
  name: string;
  address: Address;
  contributionPercent: number; // add up to 100
  description?: string;
  image?: string;
  socialMedia?: IpCreatorSocial[];
  role?: string;
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

## Relationship Types

The different relationship types that can be used for the `relationships` attribute.

### Story Relationships

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

### AI Relationships

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

# Example Use Cases

```json Harry Potter
{
  title: "Harry Potter and the Philosopher's Stone",
  createdAt: "1997-06-26T00:00:00", // June 26 1997
  ipType: "literature",
  creators: [
    {
      name: "JK Rowling",
      address: "0x0000000000000000000000000000000000000000",
      description: "Author",
      contributionPercent: "80",
      socialMedia: [
        {
          platform: "Wikipedia",
          url: "https://en.wikipedia.org/wiki/J._K._Rowling"
        }
      ]
    },
    {
      name: "Thomas Taylor",
      address: "0x0000000000000000000000000000000000000000",
      description: "Illustrator",
      contributionPercent: "15",
    },
    {
      name: "Bloomsbury Publishing",
      address: "0x0000000000000000000000000000000000000000",
      description: "Publisher",
      contributionPercent: "5",
      socialMedia: [
        {
          platform: "Website",
          url: "https://www.bloomsbury.com/"
        }
      ]
    }
  ],
  media: [
    {
      name: "ePub",
      url: "link_to_epub",
      mimeType: "application/epub+zip"
    },
    {
      name: "Book Summary PDF",
      url: "link_to_book_summary_pdf",
      mimeType: "application/pdf"
    }
  ],
  attributes: [
    {
      key: "ISBN",
      value: "978-0-7475-3269-0"
    },
    {
      key: "Genre",
      value: "Fantasy"
    }
  ]
}
```
```json Simple IP Character
{
  title: "Kenta the Samurai Azuki",
  creators: [
    {
      name: "Kaiser",
      address: "0x0000000000000000000000000000000000000000",
      contributionPercent: "80",
      description : "Kaiser is an anime enthusiast.",
      socialMedia: [
        {
          platform: "Twitter",
          url: "https://twitter.com/kentathesamurai"
        }
      ]
    },
    {
      name: "Azuki",
      contributionPercent: "20",
      description: "Creator of Azuki collection",
    }
  ]
}
```
```json Physical Painting (RWA)
{
  title: "CICLOPIROX OLAMINE, 2004",
  creators: [
    {
      name: "Damien Hirst",
      contributionPercent: '100',
      address: '0x0000000000000000000000000000000000000000',
      description: "Damien Hirst, a poster boy for the Young British Artists who rose to prominence in late 1980s London, is one of the most notorious artists of his generation. He has pushed the limits of fine art and good taste with sculptures that comprise dead animals submerged in formaldehyde; innumerable spot paintings that appear mass-produced and can sell for millions of dollars; and the exuberantly tacky For the Love of God (2007), a human skull studded with 8,601 diamonds. Through his installations, sculptures, drawings, and paintings, Hirst explores themes including religion, mortality, and desire. Since 1988, when the artist developed and curated “Freeze,” a groundbreaking exhibition of his work and that of his Goldsmiths College peers, he has been the subject of major shows at Tate Modern in London, the National Gallery of Art in Washington, D.C., and the Rijksmuseum in Amsterdam. In 2008, Hirst controversially staged “Beautiful Inside my Head Forever,” an auction in which he sold his work directly to the public and raked in around $200 million for himself. His individual works have sold for more than $10 million at auction.      ",
      socialMedia: [
        {
          platform: "Instagram",
          url: "https://www.instagram.com/damienhirst/"
        },
        {
          platform: "Wikipedia",
          url: "https://en.wikipedia.org/wiki/Damien_Hirst"
        }
      ]
    }
  ],
  attributes: [
    {
      key: "Materials",
      value: "Etching on Hahnemühle paper"
    },
    {
      key: "Size",
      value: "45 1/10 x 44 3/10 in | 114.5 x 112.5 cm"
    },
    {
      key: "Edition",
      value: "Edition of 68"
    },
    {
      key: "Signature",
      value: "Hand-signed by artist, Signed and numbered by the artist"
    },
    {
      key: "Certificate of authenticity",
      value: "link_to_certificate"
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