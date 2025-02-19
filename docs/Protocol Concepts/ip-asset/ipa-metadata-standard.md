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
> We are still figuring out the best way to define an IPA Metadata Standard. For the sake of transparency, the following document is our thoughts so far but is subject to change as we release future versions.

<Cards columns={1}>
  <Card title="Official Ippy IP" href="https://explorer.story.foundation/ipa/0xB1D831271A68Db5c18c8F0B69327446f7C8D0A42" icon="fa-home" target="_blank">
    Check out the official Ippy IP, which has both NFT & IP metadata.
  </Card>
</Cards>

This is the JSON metadata that is associated with an IP Asset, and gets stored inside of an IP Account. You must call `setMetadata(...)` inside of the IP Account in order to set the metadata, and then call `metadata()` to read it.

# Attributes & Structure

Below are the important attributes you should provide in your IP metadata. Under the **Required For** column is what the specific field is required for:

* :mag: Story Explorer - this field will help display your IP on the Story Explorer
* :detective: Commercial Infringement Check - this field is required if your IP is **commercial** (that is, has commercial license terms attached). We will use these fields to run an infringement check on your IP.

<Table align={["left","left","left","left"]}>
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

      <th style={{ textAlign: "left" }}>
        Required For
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

      <td style={{ textAlign: "left" }}>
        :mag: Story Explorer
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

      <td style={{ textAlign: "left" }}>
        :mag: Story Explorer
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

        This field can be used to specify historical dates that aren’t on-chain. For example, Harry Potter was published on June 26.
      </td>

      <td style={{ textAlign: "left" }}>
        :mag: Story Explorer
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `image`
      </td>

      <td style={{ textAlign: "left" }}>
        `string`
      </td>

      <td style={{ textAlign: "left" }}>
        An image for your IP.

        If your IP is an image itself, you can make this the same as `mediaUrl`.
      </td>

      <td style={{ textAlign: "left" }}>
        :mag: Story Explorer
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `imageHash`
      </td>

      <td style={{ textAlign: "left" }}>
        `string`
      </td>

      <td style={{ textAlign: "left" }}>
        Hash of your `image` using SHA-256 hashing algorithm. See [here](https://docs.story.foundation/docs/ipa-metadata-standard#hashing-content)  for how that is done.

        If your IP is an image itself, you can make this the same as `mediaHash`.
      </td>

      <td style={{ textAlign: "left" }}>
        :mag: Story Explorer
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

      <td style={{ textAlign: "left" }}>
        :mag: Story Explorer
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `mediaUrl`
      </td>

      <td style={{ textAlign: "left" }}>
        `string`
      </td>

      <td style={{ textAlign: "left" }}>
        Used for infringement checking, points to the actual media (ex. image or audio)
      </td>

      <td style={{ textAlign: "left" }}>
        :detective: Commercial Infringement Check
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `mediaHash`
      </td>

      <td style={{ textAlign: "left" }}>
        `string`
      </td>

      <td style={{ textAlign: "left" }}>
        Hashed string of the media using SHA-256 hashing algorithm. See [here](https://docs.story.foundation/docs/ipa-metadata-standard#hashing-content) for how that is done.
      </td>

      <td style={{ textAlign: "left" }}>
        :detective: Commercial Infringement Check
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `mediaType`
      </td>

      <td style={{ textAlign: "left" }}>
        `string`
      </td>

      <td style={{ textAlign: "left" }}>
        Type of media (audio, video, image), based on [mimeType](https://developer.mozilla.org/en-US/docs/Web/HTTP/MIME_types/Common_types). See the allowed media types [here](https://docs.story.foundation/docs/ipa-metadata-standard#media-types).
      </td>

      <td style={{ textAlign: "left" }}>
        :detective: Commercial Infringement Check
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        N/A
      </td>

      <td style={{ textAlign: "left" }}>
        N/A
      </td>

      <td style={{ textAlign: "left" }}>
        You can include other values as well.
      </td>

      <td style={{ textAlign: "left" }}>
        N/A
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

## Media Types

For the `mediaType` field, here are the valid options:

* Audio: `audio/wav`, `audio/mpeg`, `audio/flac`, `audio/aac`, `audio/ogg`, `audio/mp4`, `audio/x-aiff`, `audio/x-ms-wma`, `audio/opus`
* Video: `video/mp4`, `video/webm`, `video/quicktime`
* Image: `image/jpeg`, `image/png`, `image/apng`, `image/avif`, `image/gif`, `image/svg+xml`, `image/webp`

## Hashing Content

For `imageHash` and `mediaHash`, here is how you would calculate the hash:

```typescript
async function getFileHash(file: File): Promise<string> {
  const arrayBuffer = await file.arrayBuffer()
  const hashBuffer = await crypto.subtle.digest('SHA-256', arrayBuffer)
  return toHex(new Uint8Array(hashBuffer), { size: 32 })
}
```

# Example Use Cases

```json Ippy Mascot
// Official Ippy Mascot: https://explorer.story.foundation/ipa/0xB1D831271A68Db5c18c8F0B69327446f7C8D0A42
{
  "title": "Ippy",
  "description": "Official mascot of Story.",
  "ipType": "Character",
  "image": "https://ipfs.io/ipfs/QmSamy4zqP91X42k6wS7kLJQVzuYJuW2EN94couPaq82A8",
  "mediaUrl": "https://ipfs.io/ipfs/QmSamy4zqP91X42k6wS7kLJQVzuYJuW2EN94couPaq82A8",
  "mediaHash": "0x21937ba9d821cb0306c7f1a1a2cc5a257509f228ea6abccc9af1a67dd754af6e",
  "mediaType": "image/png",
  "creators": [
    {
      "name": "Story Foundation",
      "address": "0x67ee74EE04A0E6d14Ca6C27428B27F3EFd5CD084",
      "description": "The World's IP Blockchain",
      "contributionPercent": 100,
      "socialMedia": [
        {
          "platform": "Twitter",
          "url": "https://twitter.com/storyprotocol"
        },
        {
          "platform": "Telegram",
          "url": "https://t.me/StoryAnnouncements"
        },
        {
          "platform": "Website",
          "url": "https://story.foundation"
        },
        {
          "platform": "Discord",
          "url": "https://discord.gg/storyprotocol"
        },
        {
          "platform": "YouTube",
          "url": "https://youtube.com/@storyFDN"
        }
      ]
    }
  ]
}
```
```json Harry Potter
{
  title: "Harry Potter and the Philosopher's Stone",
  createdAt: "1997-06-26T00:00:00", // June 26 1997
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
  ]
}
```
```json Music
// Example: https://aeneid.explorer.story.foundation/ipa/0x3E5b9e540a531da38760CC32E2f52b174EC5Fce8
{
  "title": "Midnight Marriage",
  "description": "This is a house-style song generated on suno.",
  "creators": [{
    "name": "Jacob Tucker",
    "address": "0x01",
    "contributionPercent": 100
  }]
}
```

# Experimental Fields

Below is a list of experimental fields that **are not required** but we may add later.

> ❗️ Warning: Experimental
>
> We are still figuring out the best way to define an IPA Metadata Standard. The fields below are bound to change or be removed at some point.

| Property Name    | Type               | Description                                                                                                                                                                                                            |
| :--------------- | :----------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `ipType`         | `string`           | Type of the IP Asset, can be defined arbitrarily by the creator. I.e. “character”, “chapter”, “location”, “items”, "music", etc                                                                                        |
| `relationships`  | `IpRelationship[]` | The detailed relationship info with the IPA’s direct parent asset, such as `APPEARS_IN`, `FINETUNED_FROM`, etc. See more examples [here](https://docs.story.foundation/docs/ipa-metadata-standard#relationship-types). |
| `watermarkImage` | `string`           | A separate image with your watermark already applied. This way apps choosing to use it can render this version of the image (with watermark applied).                                                                  |
| `app`            | `StoryApp`         | This is assigned to verified application from Story Protocol directly (on a request basis so far). We will map each App ID to a name                                                                                   |
| `tags`           | `string[]`         | Any tags that can help surface this IPA                                                                                                                                                                                |
| `robotTerms`     | `IPRobotTerms`     | Allows you to set Do Not Train for a specific agent                                                                                                                                                                    |
| N/A              | N/A                | You can include other values as well.                                                                                                                                                                                  |

## Type Definitions

```typescript IpRelationship
type IpRelationship = {
  parentIpId: Address;
  type: string; // see "Relationship Types" docs below
}
```
```typescript StoryApp
type StoryApp = {
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