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

[block:parameters]
{
  "data": {
    "h-0": "Property Name",
    "h-1": "Type",
    "h-2": "Description",
    "0-0": "`title`",
    "0-1": "`string`",
    "0-2": "Title of the IP",
    "1-0": "`description`",
    "1-1": "`string`",
    "1-2": "Description of the IP",
    "2-0": "`ipType`",
    "2-1": "`string`",
    "2-2": "Type of the IP Asset, can be defined arbitrarily by the creator. I.e. “character”, “chapter”, “location”, “items”, \"music\", etc",
    "3-0": "`relationships`",
    "3-1": "`IpRelationship[]`",
    "3-2": "The detailed relationship info with the IPA’s direct parent asset, such as `APPEARS_IN`, `FINETUNED_FROM`, etc. See more examples [here](https://docs.story.foundation/docs/ipa-metadata-standard#relationship-types).",
    "4-0": "`createdAt`",
    "4-1": "`string`",
    "4-2": "Date/Time that the IP was created (either ISO8601 or unix format).  \n  \nThis dateCreated field can be used to specify historical dates that aren’t on-chain. For example, Harry Potter was published on June 26.",
    "5-0": "`watermarkImage`",
    "5-1": "`string`",
    "5-2": "Supporting image. Could be used as a “wrapper” image for things related to branding or watermarks.",
    "6-0": "`creators`",
    "6-1": "`IpCreator[]`",
    "6-2": "An array of information about the creators. Creator type defined below.",
    "7-0": "`media`",
    "7-1": "`IpMedia[]`",
    "7-2": "An array of supporting media. Media type defined below.",
    "8-0": "`attributes`",
    "8-1": "`IpAttribute[]`",
    "8-2": "An array of key-value pairs that can be used for arbitrary mappings. Attribute type defined below.",
    "9-0": "`appInfo`",
    "9-1": "`StoryProtocolApp`",
    "9-2": "This is assigned to verified application from Story Protocol directly (on a request basis so far). We will map each App ID to a name",
    "10-0": "`tags`",
    "10-1": "`string[]`",
    "10-2": "Any tags that can help surface this IPA",
    "11-0": "`robotTerms`",
    "11-1": "`IPRobotTerms`",
    "11-2": "Allows you to set Do Not Train for a specific agent"
  },
  "cols": 3,
  "rows": 12,
  "align": [
    "left",
    "left",
    "left"
  ]
}
[/block]


## Type Definitions

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

## Relationship Types

The different relationship types that can be used for the `relationships` attribute.

### Story Relationships

1. **APPEARS_IN** - A character APPEARS_IN a chapter.

2. **BELONGS_TO** - A chapter BELONGS_TO a book.

3. **PART_OF** - A book is PART_OF a series.

4. **CONTINUES_FROM** - A chapter CONTINUES_FROM the previous one.

5. **LEADS_TO** - An event LEADS_TO a consequence.

6. **FORESHADOWS** - An event FORESHADOWS future developments.

7. **CONFLICTS_WITH** - A character CONFLICTS_WITH another character.

8. **RESULTS_IN** - A decision RESULTS_IN a significant change.

9. **DEPENDS_ON** - A subplot DEPENDS_ON the main plot.

10. **SETS_UP** - A prologue SETS_UP the story.

11. **FOLLOWS_FROM** - A chapter FOLLOWS_FROM the previous one.

12. **REVEALS_THAT** - A twist REVEALS_THAT something unexpected occurred.

13. **DEVELOPS_OVER** - A character DEVELOPS_OVER the course of the story.

14. **INTRODUCES** - A chapter INTRODUCES a new character or element.

15. **RESOLVES_IN** - A conflict RESOLVES_IN a particular outcome.

16. **CONNECTS_TO** - A theme CONNECTS_TO the main narrative.

17. **RELATES_TO** - A subplot RELATES_TO the central theme.

18. **TRANSITIONS_FROM** - A scene TRANSITIONS_FROM one setting to another.

19. **INTERACTED_WITH** - A character INTERACTED_WITH another character.

20. **LEADS_INTO** - An event LEADS_INTO the climax.?  
    **PARALLEL - story** happening in parallel or around the same timeframe 

### AI Relationships

1. **TRAINED_ON** - A model is TRAINED_ON a dataset.

2. **FINETUNED_FROM** - A model is FINETUNED_FROM a base model.

3. **GENERATED_FROM** - An image is GENERATED_FROM a fine-tuned model.

4. **REQUIRES_DATA** - A model REQUIRES_DATA for training.

5. **BASED_ON** - A remix is BASED_ON a specific workflow.

6. **INFLUENCES** - Sample data INFLUENCES model output.

7. **CREATES** - A pipeline CREATES a fine-tuned model.

8. **UTILIZES** - A workflow UTILIZES a base model.

9. **DERIVED_FROM** - A fine-tuned model is DERIVED_FROM a base model.

10. **PRODUCES** - A model PRODUCES generated images.

11. **MODIFIES** - A remix MODIFIES the base workflow.

12. **REFERENCES** - An AI-generated image REFERENCES original data.

13. **OPTIMIZED_BY** - A model is OPTIMIZED_BY specific algorithms.

14. **INHERITS** - A fine-tuned model INHERITS features from the base model.

15. **APPLIES_TO** - A fine-tuning process APPLIES_TO a model.

16. **COMBINES** - A remix COMBINES elements from multiple datasets.

17. **GENERATES_VARIANTS** - A model GENERATES_VARIANTS of an image.

18. **EXPANDS_ON** - A fine-tuning process EXPANDS_ON base capabilities.

19. **CONFIGURES** - A workflow CONFIGURES a model’s parameters.

20. **ADAPTS_TO** - A fine-tuned model ADAPTS_TO new data.

# Example Use Cases

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