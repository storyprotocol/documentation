---
title: 📝 IPA Metadata Standard
excerpt: An overview of the IP-specific metadata standard.
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
* :robot: AI Agents - used for displaying metadata associated with AI Agents

<Table align={["left","left","left","left"]}>
  <thead>
    <tr>
      <th>
        Property Name
      </th>

      <th>
        Type
      </th>

      <th>
        Description
      </th>

      <th>
        Required For
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        `title`
      </td>

      <td>
        `string`
      </td>

      <td>
        Title of the IP
      </td>

      <td>
        :mag: Story Explorer
      </td>
    </tr>

    <tr>
      <td>
        `description`
      </td>

      <td>
        `string`
      </td>

      <td>
        Description of the IP
      </td>

      <td>
        :mag: Story Explorer
      </td>
    </tr>

    <tr>
      <td>
        `createdAt`
      </td>

      <td>
        `string`
      </td>

      <td>
        Date/Time that the IP was created (either ISO8601 or unix format).

        This field can be used to specify historical dates that aren’t on-chain. For example, Harry Potter was published on June 26.
      </td>

      <td>
        :mag: Story Explorer
      </td>
    </tr>

    <tr>
      <td>
        `image`
      </td>

      <td>
        `string`
      </td>

      <td>
        An image for your IP.
      </td>

      <td>
        :mag: Story Explorer
      </td>
    </tr>

    <tr>
      <td>
        `imageHash`
      </td>

      <td>
        `string`
      </td>

      <td>
        Hash of your `image` using SHA-256 hashing algorithm. See [here](https://docs.story.foundation/docs/ipa-metadata-standard#hashing-content) for how that is done.
      </td>

      <td>
        :mag: Story Explorer
      </td>
    </tr>

    <tr>
      <td>
        `creators`
      </td>

      <td>
        `IpCreator[]`
      </td>

      <td>
        An array of information about the creators. [See the type defined below](https://docs.story.foundation/docs/ipa-metadata-standard#type-definitions)
      </td>

      <td>
        :mag: Story Explorer
      </td>
    </tr>

    <tr>
      <td>
        `mediaUrl`
      </td>

      <td>
        `string`
      </td>

      <td>
        Used for infringement checking, points to the actual media (ex. image or audio)
      </td>

      <td>
        :detective: Commercial Infringement Check
      </td>
    </tr>

    <tr>
      <td>
        `mediaHash`
      </td>

      <td>
        `string`
      </td>

      <td>
        Hashed string of the media using SHA-256 hashing algorithm. See [here](https://docs.story.foundation/docs/ipa-metadata-standard#hashing-content) for how that is done.
      </td>

      <td>
        :detective: Commercial Infringement Check
      </td>
    </tr>

    <tr>
      <td>
        `mediaType`
      </td>

      <td>
        `string`
      </td>

      <td>
        Type of media (audio, video, image), based on [mimeType](https://developer.mozilla.org/en-US/docs/Web/HTTP/MIME_types/Common_types). See the allowed media types [here](https://docs.story.foundation/docs/ipa-metadata-standard#media-types).
      </td>

      <td>
        :detective: Commercial Infringement Check
      </td>
    </tr>

    <tr>
      <td>
        `aiMetadata`
      </td>

      <td>
        `AIMetadata`
      </td>

      <td>
        Used for registering & displaying AI Agent Metadata. [See the type defined below](https://docs.story.foundation/docs/ipa-metadata-standard#type-definitions)
      </td>

      <td>
        :robot: AI Agents
      </td>
    </tr>

    <tr>
      <td>
        N/A
      </td>

      <td>
        N/A
      </td>

      <td>
        You can include other values as well.
      </td>

      <td>
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
```typescript AIMetadata
type AIMetadata = {
  // this can be any character file you want
  // example: https://github.com/elizaOS/characterfile/blob/main/examples/example.character.json
  characterFileUrl: string;
  characterFileHash: string;
}
```

## Media Types

For the `mediaType` field, here are the valid options:

* Audio: `audio/wav`, `audio/mpeg`, `audio/flac`, `audio/aac`, `audio/ogg`, `audio/mp4`, `audio/x-aiff`, `audio/x-ms-wma`, `audio/opus`
* Video: `video/mp4`, `video/webm`, `video/quicktime`
* Image: `image/jpeg`, `image/png`, `image/apng`, `image/avif`, `image/gif`, `image/svg+xml`, `image/webp`

## Hashing Content

For `imageHash` and `mediaHash`, we use SHA-256 hashing algorithm. Here is how you would calculate the hash in code:

```typescript
import { toHex } from 'viem';

// get hash from a file
async function getFileHash(file: File): Promise<string> {
  const arrayBuffer = await file.arrayBuffer()
  const hashBuffer = await crypto.subtle.digest('SHA-256', arrayBuffer)
  return toHex(new Uint8Array(hashBuffer), { size: 32 })
}

// get hash from a url
async function getHashFromUrl(url: string): Promise<string> {
  const response = await axios.get(url, { responseType: "arraybuffer" });
  const buffer = Buffer.from(response.data);
  return "0x" + createHash("sha256").update(buffer).digest("hex");
}
```
```shell Shell
shasum -a 256 myfile.jpg
```

# Example Use Cases

<Tabs>
  <Tab title="Ippy Mascot">
    This is the official Ippy mascot that is registered on mainnet. You can view it on our protocol explorer <a href="https://explorer.story.foundation/ipa/0xB1D831271A68Db5c18c8F0B69327446f7C8D0A42" target="_blank">here</a>.

    ```json
    {
      "title": "Ippy",
      "description": "Official mascot of Story.",
      "createdAt": "1728401700",
      "image": "https://ipfs.io/ipfs/QmSamy4zqP91X42k6wS7kLJQVzuYJuW2EN94couPaq82A8",
      "imageHash": "0x21937ba9d821cb0306c7f1a1a2cc5a257509f228ea6abccc9af1a67dd754af6e",
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
      ],
      "tags": ["Ippy", "Story", "Story Mascot", "Mascot", "Official"], // experimental field
      "ipType": "Character" // experimental field
    }
    ```
  </Tab>

  <Tab title="Music">
    This is an example song generated on <a href="https://suno.com/" target="_blank">Suno</a> and registered on our testnet. View the below example <a href="https://aeneid.explorer.story.foundation/ipa/0x3E5b9e540a531da38760CC32E2f52b174EC5Fce8" target="_blank">on our protocol explorer</a>.

    ```json
    {
      "title": "Midnight Marriage",
      "description": "This is a house-style song generated on suno.",
      "createdAt": "1740005219",
      "creators": [
        {
          "name": "Jacob Tucker",
          "address": "0xA2f9Cf1E40D7b03aB81e34BC50f0A8c67B4e9112",
          "contributionPercent": 100
        }
      ],
      "image": "https://cdn2.suno.ai/image_large_8bcba6bc-3f60-4921-b148-f32a59086a4c.jpeg",
      "imageHash": "0xc404730cdcdf7e5e54e8f16bc6687f97c6578a296f4a21b452d8a6ecabd61bcc",
      "mediaUrl": "https://cdn1.suno.ai/dcd3076f-3aa5-400b-ba5d-87d30f27c311.mp3",
      "mediaHash": "0xb52a44f53b2485ba772bd4857a443e1fb942cf5dda73c870e2d2238ecd607aee",
      "mediaType": "audio/mpeg"
    }
    ```
  </Tab>

  <Tab title="AI Agent">
    The main difference here is you should supply `aiMetadata` with a character file. You can provide any character file you want, or use <a href="https://github.com/elizaOS/characterfile/blob/main/examples/example.character.json" target="_blank">this ElizaOS example</a> as a template.

    View the below example <a href="https://aeneid.explorer.story.foundation/ipa/0x49614De8b2b02C790708243F268Af50979D568d4" target="_blank">on our protocol explorer</a>.

    ```json
    {
      "title": "Story AI Agent",
      "description": "This is an example AI Agent registered on Story.",
      "createdAt": "1740005219",
      "creators": [
        {
          "name": "Jacob Tucker",
          "address": "0xA2f9Cf1E40D7b03aB81e34BC50f0A8c67B4e9112",
          "contributionPercent": 100
        }
      ],
      "image": "https://ipfs.io/ipfs/bafybeigi3k77t5h5aefwpzvx3uiomuavdvqwn5rb5uhd7i7xcq466wvute",
      "imageHash": "0x64ccc40de203f218d16bb90878ecca4338e566ab329bf7be906493ce77b1551a",
      "mediaUrl": "https://ipfs.io/ipfs/bafybeigi3k77t5h5aefwpzvx3uiomuavdvqwn5rb5uhd7i7xcq466wvute",
      "mediaHash": "0x64ccc40de203f218d16bb90878ecca4338e566ab329bf7be906493ce77b1551a",
      "mediaType": "image/webp",
      "aiMetadata": {
        "characterFileUrl": "https://ipfs.io/ipfs/bafkreic6eu4hlnwx46soib62rgkhhmlieko67dggu6bzk7bvtfusqsknfu",
        "characterFileHash": "0x5e253875b6d7e7a4e407da899473b168229def8cc6a783957c35996928494d2d"
      }
    }
    ```
  </Tab>
</Tabs>

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
| `media`          | `IpMedia[]`        | An array of supporting media. Media type defined below                                                                                                                                                                 |
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
```typescript IpMedia
type IpMedia = {
  name: string, 
  url: string, 
  mimeType: string, 
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