# Ecosystem
Story provides a versatile framework to manage the entire lifecycle of IP development, enabling features like provenance, contribution tracking, frictionless licensing, and revenue sharing. The protocol has been designed with modularity and extensibility in mind, allowing developers to build a diverse array of applications on top of it, including generative AI, fandom platforms, gaming, franchise management, DeFi, social media, and DAO.

The [Story Explorer](https://explorer.story.foundation/) provides a comprehensive view of transactions on-chain and is there to assist in debugging during development.

Additionally, we are sharing some applications that can be built under the ecosystem, and you are welcome to share your bold ideas and applications built with Story Protocol.

* [Content Remixing Mobile App](doc:content-remixing-mobile-app)
* [Content Management System and Collaboration Hub](doc:content-management-system-and-collaboration-hub)
* [IP Licensing Platform](doc:ip-licensing-platform)
* [AI-generated Assets Marketplace](doc:ai-generated-assets-marketplace)
* [DeFi Applications and RWA](doc:defi-applications-and-rwa)
* [Capital Formation Platform](doc:capital-formation-platform)
* [External Hooks](doc:external-hooks)

# Join Story Academy

The Story Academy is a builder program designed to support, mentor, and accelerate promising entrepreneurs and innovative projects building on Story. We will provide technical expertise, marketing know-how, grants, and investor network introductions matching the level of ambition and execution capability of each admitted project. If you're a visionary builder already working on or planning to build a project on top of Story, we want to talk to you.

[Learn more about Story Academy](https://www.story.foundation/academy)

# IP Modifications & Restrictions
# IP Asset Modifications

IP Assets can be modified/customized a few ways. For example, by [setting the License Config](doc:license-config-hook) which allows you to change a few things as you'll see below, changing its metadata, and more. These things can **always be changed unless there is a certain condition**.

<Table align={[null,null,"left"]}>
  <thead>
    <tr>
      <th>
        Action
      </th>

      <th>
        Conditions
      </th>

      <th style={{ textAlign: "left" }}>
        Via The...
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        Modify License Minting Fee
      </td>

      <td>
        N/A
      </td>

      <td style={{ textAlign: "left" }}>
        [License Config](doc:license-config-hook)
      </td>
    </tr>

    <tr>
      <td>
        Modify Licensing Hook
      </td>

      <td>
        N/A
      </td>

      <td style={{ textAlign: "left" }}>
        [License Config](doc:license-config-hook)
      </td>
    </tr>

    <tr>
      <td>
        Modify `commercialRevShare`
      </td>

      <td>
        Cannot **decrease** `commercialRevShare` percentage. You can only increase it.

        However, you **can** set it to 0 to disable the overwrite.
      </td>

      <td style={{ textAlign: "left" }}>
        [License Config](doc:license-config-hook)
      </td>
    </tr>

    <tr>
      <td>
        Disable/Enable the License
      </td>

      <td>
        License can be disabled or re-enabled at any time.

        *Note that disabling a license disallows future licenses from being minted, but does not affect existing ones.*
      </td>

      <td style={{ textAlign: "left" }}>
        [License Config](doc:license-config-hook)
      </td>
    </tr>

    <tr>
      <td>
        Modify Metadata
      </td>

      <td>
        Cannot modify if the metadata is **frozen**. This is done by calling `freezeMetadata` in the [CoreMetadataModule.sol](https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/modules/metadata/CoreMetadataModule.sol).
      </td>

      <td style={{ textAlign: "left" }}>
        [CoreMetadataModule.sol](https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/modules/metadata/CoreMetadataModule.sol)
      </td>
    </tr>
  </tbody>
</Table>

## License Hook Modifications

The IP can be further customized or modified using the [License Hook](https://docs.story.foundation/docs/license-config-hook#licensing-hook). This is a function that gets set within the License Config that gets called before a [License Token](doc:license-token) (or more simply, a "license") is minted. There are various features you can implement with the License Hook, and are **always modifiable**:

| Feature             | Description                                                                                                         |
| ------------------- | ------------------------------------------------------------------------------------------------------------------- |
| Dynamic License Fee | You can dynamically set the price of a license. For example, it can be updated dynamically via bonding curve logic. |
| Total # of Licenses | You can abort the function based on a maximum number of license tokens that can be minted.                          |
| Specific Receivers  | You can restrict minting of license to a specific receiver.                                                         |
| More...             | Additional licensing hook features can be implemented as required.                                                  |

# Group IPA Restrictions

In addition, [Group IPAs](doc:grouping-module) are subject to the following additional restrictions:

| Action                       | Restriction                                                                                                                                                                        |
| ---------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Add IP Asset to a Group      | You can only add an IP Asset to a group if that group is not "locked". A group becomes locked once the first license token is minted from it or a derivative is linked to it.      |
| Remove IP Asset from a Group | You can only remove an IP Asset from a group if that group is not "locked". A group becomes locked once the first license token is minted from it or a derivative is linked to it. |

# IPA Metadata Standard
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
        A separate image with your watermark already applied. This way apps choosing to use it can render this version of the image (with watermark applied).
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
  title: "Rise Again",
  description: "This NFT certifies that Rise Again was created by srivatsan_qb (ID: 4123743b-8ba6-4028-a965-75b79a3ad424), with data securely fetched and verified using the Reclaim Protocol from Suno.com",
  ipType: "Music",
  creators: [
    {
      name: "srivatsan_qb",
      description: "Creator"
    }
  ],
  media: [
    {
      name: "Rise Again",
      url: "https://cdn1.suno.ai/937e3060-65c0-4934-acab-7d8cc05eb9a6.mp3",
      mimeType: "audio/mpeg"
    }
  ],
  attributes: [
    {
      key: "Artist",
      value: "srivatsan_qb"
    },
    {
      key: "Artist ID",
      value: "4123743b-8ba6-4028-a965-75b79a3ad424"
    },
    {
      key: "Source",
      value: "Suno.com"
    }
  ]
}
```

# 🧩 IP Asset
> 🐦 Skip the Read
>
> Get a quick 1-minute overview of IP Assets [here](https://twitter.com/jacobmtucker/status/1785765362744889410).

IP Assets are the foundational programmable IP metadata on Story. Each IP Asset is an on-chain ERC-721 NFT (representing an IP). Yes, that means your Azuki or Pudgy Penguin is already eligible for registration into our protocol, and don't worry, there is no wrapping involved.

If your IP is off-chain, you would simply mint an ERC-721 NFT to represent that IP first, and then register it as an IP Asset.

An IP Asset also has an associated [⚙️ IP Account](doc:ip-account), which is a modified ERC-6551 (Token Bound Account) implementation. It is a separate contract bound to the IP Asset for controlling permissions around interactions with Story's modules or storing the IP's associated data.

## Registering an IP Asset

An IP Asset is created by registering an ERC-721 NFT into Story's global [IP Asset Registry](doc:ip-asset-registry).

If you'd like to jump into code examples/tutorials, please see [How to Register IP on Story](doc:how-to-register-ip-on-story).

## NFT vs. IP Metadata

On Story, your IP is an NFT that gets registered on the protocol as an IP Asset. However, both NFTs and IP Assets have their own metadata you can set, so what's the difference?

|         | Standard                                                                                     | What is it?                                                                                                                                                                          |
| :------ | :------------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **NFT** | [Standard ERC721 NFT Metadata](https://github.com/ethereum/ercs/blob/master/ERCS/erc-721.md) | Things like name, image, collection info, etc                                                                                                                                        |
| **IP**  | [IPA Metadata Standard](doc:ipa-metadata-standard)                                           | More specific to Story, this includes information about the author of the work, its relationship to other works, attributes like app-specific metadata & AI remixing attributes, etc |

All other metadata, such as the ownership, legal, and economic details of an IP Asset are handled by our protocol directly. For example, the protocol stores data associated with parent-child relationships through the [📜 Licensing Module](doc:licensing-module), the monetary flow between IP Assets through the [💸 Royalty Module](doc:royalty-module), and the legal constraints/permissions of an IP Asset with the [💊 Programmable IP License (PIL)](doc:programmable-ip-license).

### Adding NFT & IP Metadata to IP Asset

> 📘 Working Code Example
>
> To see how to implement proper metadata for the NFT & IP, in both the SDK and smart contracts directly, check out [How to Register IP on Story](doc:how-to-register-ip-on-story).

In practice, whether you are using the SDK or our smart contract directly, our protocol asks you to provide 4 different parameters:

* View the `WorkflowStructs.sol` contract [here](https://github.com/storyprotocol/protocol-periphery-v1/blob/main/contracts/lib/WorkflowStructs.sol).

```sol WorkflowStructs.sol
/// @notice Struct for metadata for NFT minting and IP registration.
/// @dev Leave the nftMetadataURI empty if not minting an NFT.
/// @param ipMetadataURI The URI of the metadata for the IP.
/// @param ipMetadataHash The hash of the metadata for the IP.
/// @param nftMetadataURI The URI of the metadata for the NFT.
/// @param nftMetadataHash The hash of the metadata for the IP NFT.
struct IPMetadata {
  string ipMetadataURI;
  bytes32 ipMetadataHash;
  string nftMetadataURI;
  bytes32 nftMetadataHash;
}
```

* `ipMetadataURI` - a URI pointing to a JSON object that follows the [IPA Metadata Standard](doc:ipa-metadata-standard)
* `ipMetadataHash` - hash of the `ipMetadataURI` JSON object
* `nftMetadataURI` - a URI pointing to a JSON object that follows the [Standard ERC721 NFT Metadata](https://github.com/ethereum/ercs/blob/master/ERCS/erc-721.md)
* `nftMetadataHash` - hash the `nftMetadataURI` JSON object

# Hooks
# Overview

Hooks in Story Protocol are defined as a specialized interface that inherits from the Module framework. They are designed for developers to create custom implementations that integrate seamlessly with existing Modules.

<Image align="center" src="https://files.readme.io/2ea34f7-Screenshot_2024-02-05_at_16.09.49.png" />

# Concept and Functionality

While Modules are the backbone of the Story Protocol, executing actions and managing interactions, Hooks are distinct in their specific focus. They are tailored to verify conditions, an essential feature embodied in their `verify()` functionality. This design choice positions Hooks as a unique subset of Modules, specialized for particular tasks within the ecosystem.

# Design Philosophy

From a structural standpoint, Hooks are not treated as separate entities from Modules. This decision avoids unnecessary complexity in the architecture. Viewing Hooks as specialized Modules allows for a simplified, efficient design that emphasizes clarity in roles and interactions.

# Sample Token Gated Hook Module

```coffeescript
pragma solidity 0.8.23;

import { IERC165 } from "@openzeppelin/contracts/utils/introspection/ERC165.sol";
import { ERC165Checker } from "@openzeppelin/contracts/utils/introspection/ERC165Checker.sol";
import { IERC721 } from "@openzeppelin/contracts/token/ERC721/IERC721.sol";
import { IHookModule } from "../../../contracts/interfaces/modules/base/IHookModule.sol";
import { BaseModule } from "../../../contracts/modules/BaseModule.sol";

/// @title Token Gated Hook.
/// @notice Hook for ensursing caller is the owner of an NFT token.
contract TokenGatedHook is BaseModule, IHookModule {
    using ERC165Checker for address;

    string public constant override name = "TokenGatedHook";

    function verify(address caller, bytes calldata data) external view returns (bool) {
        address tokenAddress = abi.decode(data, (address));
        if (caller == address(0)) {
            return false;
        }
        if (!tokenAddress.supportsInterface(type(IERC721).interfaceId)) {
            return false;
        }
        return IERC721(tokenAddress).balanceOf(caller) > 0;
    }

    function validateConfig(bytes calldata configData) external view override {
        address tokenAddress = abi.decode(configData, (address));
        require(tokenAddress.supportsInterface(type(IERC721).interfaceId), "TokenGatedHook: Invalid token address");
    }

    function supportsInterface(bytes4 interfaceId) public view virtual override(BaseModule, IERC165) returns (bool) {
        return interfaceId == type(IHookModule).interfaceId || super.supportsInterface(interfaceId);
    }
}

```

Using The `TokenGatedHook` as `commercializerChecker` with `LicensingModule` for use case that only allow mint license to licensee who own a specific NFT token. 

```
        licensingModule.registerPolicyFrameworkManager(address(pilManager));

        PILPolicy memory policyData = PILPolicy({
            attribution: true,
            commercialUse: true,
            commercialAttribution: false,
            commercializerChecker: address(0),
            commercializerCheckerData: "",
            commercialRevShare: 100,
            derivativesAllowed: false,
            derivativesAttribution: false,
            derivativesApproval: false,
            derivativesReciprocal: false,
            territories: new string[](1),
            distributionChannels: new string[](1),
            contentRestrictions: emptyStringArray
        });

        gatedNftFoo.mintId(address(this), 1);

        MockTokenGatedHook tokenGatedHook = new MockTokenGatedHook();
        policyData.commercializerChecker = address(tokenGatedHook);
        // address(this) doesn't hold token of NFT collection gatedNftBar, so the verification will fail
        policyData.commercializerCheckerData = abi.encode(address(gatedNftBar));
        policyData.territories[0] = "territory1";
        policyData.distributionChannels[0] = "distributionChannel1";

        uint256 policyId = pilManager.registerPolicy(
            RegisterPILPolicyParams({
                transferable: true,
                royaltyPolicy: address(mockRoyaltyPolicyLAP),
                mintingFee: 0,
                mintingFeeToken: address(0),
                policy: policyData
            })
        );

        vm.prank(ipOwner);
        licensingModule.addPolicyToIp(ipId1, policyId);
        licensingModule.mintLicense(policyId, ipId1, 1, licenseHolder, "");
```


# How to Create and Register Modules
This guide will walk you through the process of creating a Module and registering it with the Story Protocol, enabling you to contribute to its ecosystem.

# How to Create a Module

Creating a module is straightforward. You need to develop a contract and implement the `IModule` interface. While inheriting from `BaseModule` is recommended for convenience, it's not mandatory.

Below is an example of how to create a HookModule:

```sol TokenGatedHook.sol
// SPDX-License-Identifier: MIT
pragma solidity 0.8.23;

import { IERC165 } from "@openzeppelin/contracts/utils/introspection/ERC165.sol";
import { ERC165Checker } from "@openzeppelin/contracts/utils/introspection/ERC165Checker.sol";
import { IERC721 } from "@openzeppelin/contracts/token/ERC721/IERC721.sol";
import { IHookModule } from "contracts/interfaces/modules/base/IHookModule.sol";
import { BaseModule } from "contracts/modules/BaseModule.sol";

/// @title Mock Token Gated Hook.
/// @notice Hook for ensursing caller is the owner of an NFT token.
contract TokenGatedHook is BaseModule, IHookModule {
    using ERC165Checker for address;

    string public constant override name = "TokenGatedHook";

    function verify(address caller, bytes calldata data) external view returns (bool) {
        address tokenAddress = abi.decode(data, (address));
        if (caller == address(0)) {
            return false;
        }
        if (!tokenAddress.supportsInterface(type(IERC721).interfaceId)) {
            return false;
        }
        return IERC721(tokenAddress).balanceOf(caller) > 0;
    }

    function validateConfig(bytes calldata configData) external view override {
        address tokenAddress = abi.decode(configData, (address));
        require(tokenAddress.supportsInterface(type(IERC721).interfaceId), "MockTokenGatedHook: Invalid token address");
    }

    function supportsInterface(bytes4 interfaceId) public view virtual override(BaseModule, IERC165) returns (bool) {
        return interfaceId == type(IHookModule).interfaceId || super.supportsInterface(interfaceId);
    }
}

```

# How to Register Your Module

After creating your module, the next step is to register it with the Story Protocol to make it available within its ecosystem. We have established a public GitHub repository at [here](https://github.com/storyprotocol/registered-modules) to facilitate the submission process. You are invited to register your module here. The Story Protocol team will conduct a thorough review of your submission. This process ensures that only safe and compliant modules are integrated into the ecosystem.

## Steps to Register Your Module

To get your module verified and listed in this repository, please follow these steps:

1. **Fork this[Repository](https://github.com/storyprotocol/registered-modules):** Begin by forking this repository to your own GitHub account.
2. **Update the Module List:** Add your module's details to the appropriate JSON file according to module types in the repository. Ensure that you adhere to the following JSON structure:

```json

{
  "name": "YourModuleName",
  "address": "YourModuleAddress",
  "blockExplorerLink": "YourModuleBlockExplorerLink"
}
```

* Replace `YourModuleName`, `YourModuleAddress`, and `YourModuleBlockExplorerLink` with your module's name, its address, and the link to its block explorer page, respectively.
* Example:

```json json
{  
  "name": "LicensingModule",  
  "address": "0xd81fd78f557b457b4350cB95D20b547bFEb4D857",  
  "blockExplorerLink": "https://testnet.storyscan.xyz/address/0xd81fd78f557b457b4350cB95D20b547bFEb4D857?tab=contract">  
}
```

3. **Create a Pull Request (PR)**: Once you have added your module, create a pull request against this repository. Utilize the provided PR template to ensure all necessary information is included.
4. **Await Verification:** After your PR is submitted, it will be reviewed. Upon approval and merging of your PR, your module will be officially registered and recognized as safe for use within the StoryProtocol community.

We look forward to seeing your contributions and expanding the StoryProtocol module ecosystem!

# Base Module
The Base Module provides a standard set of must-have functionalities for all modules registered on Story Protocol. Anyone wishing to create and register a module on Story Protocol must inherit and override the Base Module.

> 🗒️ Contract
>
> View the smart contract [here](https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/modules/BaseModule.sol).

# Key Features

## Simplicity and Flexibility

The BaseModule is intentionally kept simple and generalized. It only implements the ERC165 interface, which is crucial for interface detection. This design choice allows for maximum flexibility when developing more specific modules within the Story Protocol.

## ERC165 Interface Implementation

By implementing the ERC165 interface, BaseModule allows other contracts to query whether it supports a specific interface. This feature is essential for ensuring compatibility and interoperability within the Story Protocol ecosystem and beyond.

```sol BaseModule.sol
abstract contract BaseModule is ERC165, IModule {
    ...
}
```

## `supportsInterface` Function

A key function in the BaseModule is `supportsInterface`, which overrides the ERC165's `supportsInterface` method. This function is crucial for interface detection, allowing the contract to declare support for both its own `IModule` interface and any other interfaces it might inherit.

```sol BaseModule.sol
function supportsInterface(bytes4 interfaceId) public view virtual override(ERC165, IERC165) returns (bool) {
    return interfaceId == type(IModule).interfaceId || super.supportsInterface(interfaceId);
}
```


# 🧱 Modules
Modules are standalone contracts that adhere to the [`IModule` interface](https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/interfaces/modules/base/IModule.sol). These modules play a crucial role in taking action on IP to change the data/state around or of IP.

# Existing Modules

There are a few important modules, created by the Story team, to be aware of:

## [📜 Licensing Module](doc:licensing-module)

Responsible for defining and attaching licenses to IP Assets.

## [💸 Royalty Module](doc:royalty-module)

Responsible for handling royalty flow between parent & child IP Assets.

## [❌ Dispute Module](doc:dispute-module)

Responsible for handling the dispute of wrongfully registered or misbehaved IP Assets.

## [👥 Grouping Module](doc:grouping-module)

Responsible for handling groups of IPAs.


# View Module
# Overview

The View Module, inheriting from the Module in Story Protocol, is designed for interpreting and displaying IP data. Its primary role is to function as a read-only module, focusing on the presentation of IP-related data in various accessible formats.

<Image align="center" src="https://files.readme.io/3c35819-Untitled.png" />

# Design

The View Module is uniquely tailored to aggregate data from multiple namespaces within an IPAccount. This design enables it to present a comprehensive view of an IP, encompassing different data types and sources. It is structured to prioritize interpretability and user-friendly display of information.

<Image align="center" src="https://files.readme.io/53c890b-Screenshot_2024-01-29_at_22.35.55.png" />

# Use Cases Example

* **Core Metadata Immutability:**Utilizes CoreMetadataModule to ensure the immutability of core IP metadata.
* **User-Defined Metadata:** Employs UserMetadataModule for new metadata additions and UserMetadataViewModule for reading and showcasing data from Core and User Metadata.
* **Metadata Upgrade/Migration:** Focuses on seamless data evolution with MetadataModuleV2 and MetadataViewModuleV2, combining information from original and upgraded metadata sources without the need for migration.


# 💊 Programmable IP License (PIL)
The PIL is a legal off-chain document based on US copyright law created by the Story team.

The parameters outlined in the PIL (ex. "Commercial Use", "Derivatives Allowed", etc) have been mapped on-chain, which means they can be enforced on-chain via our protocol, bridging code and law and unlocking the benefit of transparent, autonomous, and permission-less smart contracts for the world of intellectual property.

<Cards columns={1}>
  <Card title="PIL Legal Text" href="https://github.com/piplabs/pil-document/blob/v1.3.0/Story%20Foundation%20-%20Programmable%20IP%20License%20(1.31.25).pdf" icon="fa-scroll" iconColor="#ccb092" target="_blank">
    Check out the actual PIL legal text. It is very human-readable for a legal text!
  </Card>
</Cards>

The PIL is the first and currently only example of a [License Template](doc:license-template). A License Template is simply a traditional legal document that has been brought on-chain and contains a set of pre-defined terms that people must set, like:

* `commercialUse` - can someone use my work commercially?
* `mintingFee`  - the cost of minting a license to use my work in your own works.
* `derivativesAttribution` - does someone have to credit me in their derivative works?

In code, these terms form a struct that represent their legal off-chain counterparts. To see all of the terms defined by the PIL and their associated explanations in code, see [PIL Terms](doc:pil-terms).

To see example configurations ("flavors") of the PIL, see [PIL Flavors (examples)](doc:pil-flavors).

## The Background Story

> 📘 Skip This Section
>
> If you just want to get started developing with the PIL, you can skip this section.

We designed Story's [📜 Licensing Module](doc:licensing-module) to power the expansion of emerging forms of creativity, such as authorized remixes and co-creation. Our protocol can support any media format or project, ranging from user-generated social videos & images to Hollywood-grade collaborative storytelling.

Intellectual property owners can permit other parties to use, or build on, their work by granting rights in a license, which can be for profit or for the common good. In the media world, these licenses are generally highly tailored contracts, which vary by media formats and the unique needs of licensors - often requiring unique expertise (via lawyers) and significant resources to create.

We searched for a form of a "universal license" that could support these emerging activities at scale. Hat tip to [Creative Commons](https://creativecommons.org/mission/), [Arweave](https://mirror.xyz/0x64eA438bd2784F2C52a9095Ec0F6158f847182d9/AjNBmiD4A4Sw-ouV9YtCO6RCq0uXXcGwVJMB5cdfbhE), A16Z / [Can’t Be Evil,](https://a16zcrypto.com/posts/article/introducing-nft-licenses/) The [Token-Bound NFT License](https://james.grimmelmann.net/files/articles/token-bound-nft-license.pdf) and music rights organizations, among others. But we simply couldn't find one framework or agreement robust enough - so with our expert legal counsel (with special thanks to Ghaith Mahmood and Heather Liu) we created one ourselves! **Introducing the Programmable IP License (PIL:pill:)**, the first example of a [License Template](doc:license-template) on the protocol.

## Feedback

We are excited to collect feedback and collaborate with IP owners to unlock the potential of their works - please let us know what you think! We can be reached at `legal@storyprotocol.xyz`.

<Cards columns={1}>
  <Card title="PIL Legal Text" href="https://github.com/piplabs/pil-document/blob/v1.3.0/Story%20Foundation%20-%20Programmable%20IP%20License%20(1.31.25).pdf" icon="fa-scroll" iconColor="#ccb092" target="_blank">
    Check out the actual PIL legal text. It is very human-readable for a legal text!
  </Card>
</Cards>

# PIL Flavors (examples)
The [💊 Programmable IP License (PIL)](doc:programmable-ip-license) is very configurable, but we support popular pre-configured License Terms (also known as "flavors") for ease of use. We expect these to be the most popular options:

## Flavor #1: Non-Commercial Social Remixing

> 📘 Already Registered
>
> This flavor is already registered as `licenseTermsId = 1` on our protocol. This is because it doesn't take any inputs, so we registered it ahead of time.

Let the world build on and play with your creation. This license allows for endless free remixing while tracking all uses of your work while giving you full credit. Similar to: TikTok plus attribution.

### What others can do?

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        Others can
      </th>

      <th style={{ textAlign: "left" }}>
        Others cannot
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        ✅ Remix this work
        (`derivativesAllowed == true`)
      </td>

      <td style={{ textAlign: "left" }}>
        ❌ Commercialize the original and derivative works
        (`commercialUse == false`)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        ✅ Distribute their remix anywhere
      </td>

      <td style={{ textAlign: "left" }}>
        :x: Claim credit for any derivative works
        (`derivativesAttribution == true`)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        :white_check_mark: Get the license for free\
        (`defaultMintingFee == 0`)
      </td>

      <td style={{ textAlign: "left" }}>
        :x: Claim credit for the original work\
        ("Attribution" is true in the off-chain terms)
      </td>
    </tr>
  </tbody>
</Table>

### PIL Term Values

* **On-chain**:

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

* **Off-chain:**

| Parameter                         | Options / Tags                                                              |
| --------------------------------- | --------------------------------------------------------------------------- |
| Territory                         | No restrictions                                                             |
| Channels of Distribution          | No Restriction                                                              |
| Attribution                       | True                                                                        |
| Content Standards                 | No Restriction                                                              |
| Sublicensable                     | False                                                                       |
| AI Learning Models                | True                                                                        |
| Restriction on Cross-Platform Use | False                                                                       |
| Governing Law                     | California                                                                  |
| Alternative Dispute Resolution    | Tag: Alternative-Dispute-Resolution Ledger-Authoritative-Dispute-Resolution |
| Additional License Parameters     | None                                                                        |

## Flavor #2: Commercial Use

Retain control over reuse of your work, while allowing anyone to appropriately use the work in exchange for the economic terms you set. This is similar to Shutterstock with creator-set rules.

### What others can do?

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        Others can
      </th>

      <th style={{ textAlign: "left" }}>
        Others cannot
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        ✅ Remix this work
        (`derivativesAllowed == true`)
      </td>

      <td style={{ textAlign: "left" }}>
        ❌ Claim credit for the original work
        (`commercialAttribution == true`)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        ✅ Commercialize the original and derivative works
        (`commercialUse == true`)
      </td>

      <td style={{ textAlign: "left" }}>
        :x: Claim credit for any derivative works
        (`derivativesAttribution == true`)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        ✅ Distribute their remix anywhere
      </td>

      <td style={{ textAlign: "left" }}>
        :x: Get the license for free\
        (`defaultMintingFee` is set)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        :white_check_mark: Keep all revenue\
        (`commercialRevShare == 0`)
      </td>

      <td style={{ textAlign: "left" }}>
        :x: Claim credit for the original work even non-commercially\
        ("Attribution is true in the off-chain terms)
      </td>
    </tr>
  </tbody>
</Table>

### PIL Term Values

* **On-chain**:

```sol Solidity
PILTerms({
  transferable: true,
  royaltyPolicy: ROYALTY_POLICY, // ex. RoyaltyPolicyLAP address
  defaultMintingFee: BigInt(100), // ex. costs 100 $WIP to mint
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
  currency: CURRENCY, // ex. $WIP address
  uri: "",
})
```

* **Off-chain**

| Parameter                         | Options / Tags                                                              |
| --------------------------------- | --------------------------------------------------------------------------- |
| Territory                         | No restrictions                                                             |
| Channels of Distribution          | No Restriction                                                              |
| Attribution                       | True                                                                        |
| Content Standards                 | No Restriction                                                              |
| Sublicensable                     | False                                                                       |
| AI Learning Models                | True                                                                        |
| Restriction on Cross-Platform Use | False                                                                       |
| Governing Law                     | California                                                                  |
| Alternative Dispute Resolution    | Tag: Alternative-Dispute-Resolution Ledger-Authoritative-Dispute-Resolution |
| Additional License Parameters     | None                                                                        |

## Flavor #3: Commercial Remix

Let the world build on and play with your creation... and earn money together from it! This license allows for endless free remixing while tracking all uses of your work while giving you full credit, with each derivative paying a percentage of revenue to its "parent" IP.

### What others can do?

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        Others can
      </th>

      <th style={{ textAlign: "left" }}>
        Others cannot
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        ✅ Remix this work
        (`derivativesAllowed == true`)
      </td>

      <td style={{ textAlign: "left" }}>
        ❌ Claim credit for the original work
        (`commercialAttribution == true`)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        ✅ Commercialize the original and derivative works
        (`commercialUse == true`)
      </td>

      <td style={{ textAlign: "left" }}>
        :x: Claim credit for any derivative works
        (`derivativesAttribution == true`)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        ✅ Distribute their remix anywhere
      </td>

      <td style={{ textAlign: "left" }}>
        ❌ Keep all revenue\
        (`commercialRevShare` is set)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>

      </td>

      <td style={{ textAlign: "left" }}>
        :x: Get the license for free\
        (`defaultMintingFee` is set)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>

      </td>

      <td style={{ textAlign: "left" }}>
        :x: Claim credit for the original work even non-commercially\
        ("Attribution is true in the off-chain terms)
      </td>
    </tr>
  </tbody>
</Table>

### PIL Term Values

* **On-chain**:

```sol Solidity
PILTerms({
  transferable: true,
  royaltyPolicy: ROYALTY_POLICY, // ex. RoyaltyPolicyLAP address
  defaultMintingFee: BigInt(100), // ex. costs 100 $WIP to mint
  expiration: BigInt(0),
  commercialUse: true,
  commercialAttribution: true,
  commercializerChecker: zeroAddress,
  commercializerCheckerData: zeroAddress,
  commercialRevShare: 50 * 10 ** 6, // ex. can claim 50% of derivative revenue
  commercialRevCeiling: BigInt(0),
  derivativesAllowed: true,
  derivativesAttribution: true,
  derivativesApproval: false,
  derivativesReciprocal: true,
  derivativeRevCeiling: BigInt(0),
  currency: CURRENCY, // ex. $WIP address
  uri: "",
});
```

* **Off-chain**

| Parameter                         | Options / Tags                                                              |
| --------------------------------- | --------------------------------------------------------------------------- |
| Territory                         | No restrictions                                                             |
| Channels of Distribution          | No Restriction                                                              |
| Attribution                       | True                                                                        |
| Content Standards                 | No Restriction                                                              |
| Sublicensable                     | False                                                                       |
| AI Learning Models                | True                                                                        |
| Restriction on Cross-Platform Use | False                                                                       |
| Governing Law                     | California                                                                  |
| Alternative Dispute Resolution    | Tag: Alternative-Dispute-Resolution Ledger-Authoritative-Dispute-Resolution |
| Additional License Parameters     | None                                                                        |

## Flavor #4: Creative Commons Attribution

Let the world build on and play with your creation - including making money.

### What others can do?

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        Others can
      </th>

      <th style={{ textAlign: "left" }}>
        Others cannot
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        ✅ Remix this work
        (`derivativesAllowed == true`)
      </td>

      <td style={{ textAlign: "left" }}>
        ❌ Claim credit for the original work
        (`commercialAttribution == true`)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        ✅ Commercialize the original and derivative works
        (`commercialUse == true`)
      </td>

      <td style={{ textAlign: "left" }}>
        :x: Claim credit for any derivative works
        (`derivativesAttribution == true`)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        ✅ Distribute their remix anywhere
      </td>

      <td style={{ textAlign: "left" }}>
        :x: Claim credit for the original work even non-commercially\
        ("Attribution is true in the off-chain terms)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        :white_check_mark: Get the license for free\
        (`defaultMintingFee == 0`)
      </td>

      <td style={{ textAlign: "left" }}>

      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        :white_check_mark: Keep all revenue\
        (`commercialRevShare == 0`)
      </td>

      <td style={{ textAlign: "left" }}>

      </td>
    </tr>
  </tbody>
</Table>

### PIL Term Values

* **On-chain**:

```sol Solidity
PILTerms({
  transferable: true,
  royaltyPolicy: zeroAddress,
  defaultMintingFee: 0,
  expiration: 0,
  commercialUse: true,
  commercialAttribution: true,
  commercializerChecker: zeroAddress,
  commercializerCheckerData: EMPTY_BYTES,
  commercialRevShare: 0,
  commercialRevCelling: 0,
  derivativesAllowed: true,
  derivativesAttribution: true,
  derivativesApproval: false,
  derivativesReciprocal: true,
  derivativeRevCelling: 0,
  currency: zeroAddress,
  uri: ''
});
```

* **Off-chain**

| Parameter                         | Options / Tags                                                              |
| --------------------------------- | --------------------------------------------------------------------------- |
| Territory                         | No restrictions                                                             |
| Channels of Distribution          | No Restriction                                                              |
| Attribution                       | True                                                                        |
| Content Standards                 | No Restriction                                                              |
| Sublicensable                     | False                                                                       |
| AI Learning Models                | True                                                                        |
| Restriction on Cross-Platform Use | False                                                                       |
| Governing Law                     | California                                                                  |
| Alternative Dispute Resolution    | Tag: Alternative-Dispute-Resolution Ledger-Authoritative-Dispute-Resolution |
| Additional License Parameters     | None                                                                        |

# Examples

Here are some common examples of royalty flow. *More coming soon!*

## Example 1

<Image align="center" src="https://files.readme.io/01710d13ebea0b9946a65dca0d5948b4a70d6ee6e5a3fbdbb722f74b588a6e42-Screenshot_2025-02-12_at_9.24.01_PM.png" />

### Explanation

Someone registers their Azuki on Story. By default, that IP Asset has Non-Commercial Social Remixing Terms, which specify that anyone can create derivatives of that work but cannot commercialize them. So, someone else creates & registers a remix of that work (IPA2) which inherits those same terms. Someone else then does the same to IPA2, creating & registering IPA3.

The owner of IPA1 then decides that others can commercialize the work, but they cannot create derivatives to do so, they must pay a 10 $WIP minting fee, and they must share 10% of all revenue earned. So, someone wants to commercialize IPA1 by putting it on a t-shirt. They pay the 10 $WIP minting fee to get a License Token, which represents the license to commercialize IPA1. They then put the image on a t-shirt and sell it. 10% of revenue earned by that t-shirt must be sent on-chain to IPA1.

## Example 2

<Image align="center" src="https://files.readme.io/9763a4efe56b88e8a29531e11341538ccd040a8c7d0a786fee8d1d237a2e7a85-Screenshot_2025-02-12_at_9.24.31_PM.png" />

### Explanation

Someone registers their Azuki on Story. By default, that IP Asset has Non-Commercial Social Remixing Terms, which specify that anyone can create derivatives of that work but cannot commercialize them. So, someone else creates & registers a remix of that work (IPA2) which inherits those same terms. Someone else then does the same to IPA2, creating & registering IPA3.

The owner of IPA1 then decides that others can create derivatives of their work and commercialize them, but they must pay a 10 $WIP minting fee and share 10% of all revenue earned. So, someone wants to commercialize IPA1 by putting it on a t-shirt. They pay the 10 $WIP minting fee to get a License Token and burn it to create their own derivative, which changes the background color to red. They then put the remixed image on a t-shirt and sell it. 10% of revenue earned by that t-shirt must be sent on-chain to IPA1.

A third person wants to commercialize the remix by putting it in a TV advertisement, but they want to change the hair color to white. So, they pay a 10 $WIP minting fee (of which, 1 $WIP gets sent back to IPA1) to create their own derivative. They then put the remixed image in a TV ad. 10% of revenue earned by that t-shirt must be sent on-chain to IPA4, of which 10% will be distributed back to IPA1.

# PIL Terms
<Cards columns={3}>
  <Card title="Read the Overview" href="https://docs.story.foundation/docs/programmable-ip-license" icon="fa-pills" iconColor="yellow">
    If you haven't already, read the Programmable IP License (PIL💊) overview.
  </Card>

  <Card title="Preset PIL Terms" href="https://docs.story.foundation/docs/pil-flavors" icon="fa-thumbs-up" iconColor="#51af51">
    Since there are so many possible combinations of the PIL, we have created preset "flavors" for you to use while developing.
  </Card>

  <Card title="PIL Legal Text" href="https://github.com/piplabs/pil-document/blob/v1.3.0/Story%20Foundation%20-%20Programmable%20IP%20License%20(1.31.25).pdf" icon="fa-scroll" iconColor="#ccb092" target="_blank">
    Check out the actual PIL legal text. It is very human-readable for a legal text!
  </Card>
</Cards>

# On-chain terms

Most PIL terms are on-chain. They are implemented in the `IPILicenseTemplate.sol` contract as a `PILTerms` struct [here](https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/interfaces/modules/licensing/IPILicenseTemplate.sol).

```sol IPILicenseTemplate.sol
/// @notice This struct defines the terms for a Programmable IP License (PIL).
/// These terms can be attached to IP Assets.
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

## Descriptions

<Table align={[null,"left",null]}>
  <thead>
    <tr>
      <th>
        Parameter
      </th>

      <th style={{ textAlign: "left" }}>
        Values
      </th>

      <th>
        Description
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
        If false, the License Token cannot be transferred once it is minted to a recipient address.
      </td>
    </tr>

    <tr>
      <td>
        `royaltyPolicy`
      </td>

      <td style={{ textAlign: "left" }}>
        Address
      </td>

      <td>
        The address of the royalty policy contract.
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
        The fee to be paid when minting a license.
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
        The expiration period of the license.
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
        You can make money from using the original IP Asset, subject to limitations below.
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
        If true, people must give credit to the original work in their commercial application (eg. merch)
      </td>
    </tr>

    <tr>
      <td>
        `commercializerChecker`
      </td>

      <td style={{ textAlign: "left" }}>
        Address
      </td>

      <td>
        Commercializers that are allowed to commercially exploit the original work. If zero address, then no restrictions are enforced.
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
        The data to be passed to the commercializer checker contract.
      </td>
    </tr>

    <tr>
      <td>
        `commercialRevShare`
      </td>

      <td style={{ textAlign: "left" }}>
        \[0-100,000,000]
      </td>

      <td>
        Amount of revenue (from any source, original & derivative) that must be shared with the licensor (a value of 10,000,000 == 10% of revenue share).

        This will collect all revenue from tokens that are whitelisted in the [RoyaltyModule.sol contract](https://github.com/storyprotocol/protocol-core-v1/blob/e339f0671c9172a6699537285e32aa45d4c1b57b/contracts/modules/royalty/RoyaltyModule.sol#L50).
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
        If `commercialUse` is set to true, this value determines the maximum revenue you can earn from the original work.
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
        Indicates whether the licensee can create derivatives of his work or not.
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
        If true, derivatives that are made must give credit to the original work.
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
        If true, the licensor must approve derivatives of the work.
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
        If true, derivatives of this derivative can be created indefinitely as long as they have the exact same terms.
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
        If `commercialUse` is set to true, this value determines the maximum revenue you can earn from derivative works.
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
        The ERC20 token to be used to pay the minting fee. The token must be registered in story protocol.
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
        The URI of the license terms, which can be used to fetch [off-chain license terms](https://docs.story.foundation/v1/docs/pil-for-devs-and-creators#off-chain-parameters-to-be-included-in-uri-field).
      </td>
    </tr>
  </tbody>
</Table>

# Off-chain terms to be included in `uri` field

Some PIL terms must be stored off-chain and passed in the `uri` field above. This is because these terms are often more lengthy and/or descriptive, so it would not make sense to store them on-chain.

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        Parameter
      </th>

      <th style={{ textAlign: "left" }}>
        Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        Territory
      </td>

      <td style={{ textAlign: "left" }}>
        Limit usage of the IP to certain regions and/or countries.

        By default, the IP can be used globally.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Channels of Distribution
      </td>

      <td style={{ textAlign: "left" }}>
        Restrict usage of the IP to certain media formats and use in certain channels of distribution.

        By default, the IP can be used across all possible channels of distribution.

        Examples: "television", "physical consumer products", "video games", etc.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Attribution
      </td>

      <td style={{ textAlign: "left" }}>
        If the original author should be credited for usage of the IP.

        By default, you do not need to provide credit to the original author.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Content Standards
      </td>

      <td style={{ textAlign: "left" }}>
        Set content standards around use of the IP.

        By default, no standards apply.

        Examples: "No-Hate", "Suitable-for-All-Ages", "No-Drugs-or-Weapons", "No-Pornography".
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Sublicensable
      </td>

      <td style={{ textAlign: "left" }}>
        Derivative works can grant the same rights they received under this license to a 3rd party, without approval from the original licensor.

        By default, derivatives may not do so.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        AI Learning Models
      </td>

      <td style={{ textAlign: "left" }}>
        Whether or not the IP can be used to develop AI learning models.

        By default, the IP **cannot** be used for such development.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Restriction On Cross-Platform Use
      </td>

      <td style={{ textAlign: "left" }}>
        Limit licensing and creation of derivative works solely on the app on which the IP is made available.

        By default, the IP can be used anywhere.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Governing Law
      </td>

      <td style={{ textAlign: "left" }}>
        The laws of a certain jurisdiction by which this license abides.

        By default, this is California, USA.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Alternative Dispute Resolution
      </td>

      <td style={{ textAlign: "left" }}>
        Please see section 3.1 (s) [here](https://github.com/storyprotocol/protocol-core-v1/blob/main/PIL_Testnet.pdf).
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Additional License Parameters
      </td>

      <td style={{ textAlign: "left" }}>
        There may be other terms the licensor would like to add and they can do so in this tag.
      </td>
    </tr>
  </tbody>
</Table>

# 📦 SPG (Periphery)
The Story Protocol Gateway (SPG) is a group of periphery/utility smart contracts, deployed on our protocol that **allows you to combine independent operations** - like registering an [🧩 IP Asset](doc:ip-asset) and attaching License Terms to that IP Asset - **into one transaction to make your life easier**.

This was primarily developed to make our [SDK](doc:sdk-overview) easier to use.

For example, this `mintAndRegisterIpAndAttachPILTerms` is one of the functions in the SPG (more specifically in the `LicenseAttachmentWorkflows.sol`) that allows you to mint an NFT, register it as an IP Asset, and attach License Terms to it all in one call:

```sol LicenseAttachmentWorkflows.sol
function mintAndRegisterIpAndAttachPILTerms(
  address spgNftContract,
  address recipient,
  WorkflowStructs.IPMetadata calldata ipMetadata,
  WorkflowStructs.LicenseTermsData[] calldata licenseTermsData,
  bool allowDuplicates
) external onlyMintAuthorized(spgNftContract) returns (address ipId, uint256 tokenId, uint256[] memory licenseTermsIds)
```

## All Supported Workflows

As mentioned above, there are many different functions we have created for you that combine multiple functions into one. We have categorized them into different groups. These groups are called "workflows".

<Cards columns={2}>
  <Card title="View all Workflows" href="https://github.com/storyprotocol/protocol-periphery-v1/blob/main/docs/WORKFLOWS.md" icon="fa-eyes" iconColor="grey" target="_blank">
    Click here to view all of the supported workflows.
  </Card>

  <Card title="Smart Contracts" href="https://github.com/storyprotocol/protocol-periphery-v1/tree/main/contracts/workflows" icon="fa-scroll" iconColor="#ccb092" target="_blank">
    Click here to view the workflow smart contracts.
  </Card>
</Cards>

## Batching Calls

Although the SPG contains certain functions like `mintAndRegisterIpAndAttachPILTerms`, `registerIpAndAttachPILTerms`, and a bunch more, it would be tedious for us to continually update the contract to account for every single combination of possible interactions with an IP Asset.

Instead, we have allowed for a "Multicall" mechanism where you can batch transactions how you like. For more info, see [Batch Function Calls](doc:batch-spg-function-calls).

# Batch Function Calls

## Background

Prior to this point, registering multiple IPs or performing other operations such as minting, attaching licensing terms, and registering derivatives requires separate transactions for each operation. This can be inefficient and costly. To streamline the process, you can batch multiple transactions into a single one. Two solutions are now available for this:

1. **Batch SPG function calls:** Use [SPG’s built-in `multicall` function](https://docs.story.foundation/docs/batch-spg-function-calls#1-batch-spg-function-calls-via-built-in-multicall-function).
2. **Batch function calls beyond SPG:** Use the [Multicall3 Contract](https://docs.story.foundation/docs/batch-spg-function-calls#2-batch-function-calls-via-multicall3-contract).

---

## 1. Batch SPG Function Calls via Built-in `multicall` Function

SPG includes a `multicall` function that allows you to combine multiple read or write operations into a single transaction.

### Function Definition

The `multicall` function accepts an array of encoded call data and returns an array of encoded results corresponding to each function call:

```solidity
/// @dev Executes a batch of function calls on this contract.
function multicall(bytes[] calldata data) external virtual returns (bytes[] memory results);
```

### Example Usage

Suppose you want to mint multiple NFTs, register them as IPs, and link them as derivatives to some parent IPs.

To accomplish this, you can use SPG’s `multicall` function to batch the calls to the `mintAndRegisterIpAndMakeDerivative` function.

Here’s how you might do it:

```solidity
// an SPG workflow contract: https://github.com/storyprotocol/protocol-periphery-v1/blob/main/contracts/workflows/DerivativeWorkflows.sol
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

To batch call `mintAndRegisterIpAndMakeDerivative` using the `multicall` function:

```javascript
// batch mint, register, and make derivatives for multiple IPs
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
  // Add more calls as needed
]);
```

---

## 2. Batch Function Calls via Multicall3 Contract

> 🚧 Warning
>
> Note: The Multicall3 contract is not fully compatible with SPG functions that involve SPGNFT minting due to access control and context changes during Multicall execution. For such operations, use [SPG’s built-in multicall function.](https://docs.story.foundation/docs/batch-spg-function-calls#1-batch-spg-function-calls-via-built-in-multicall-function)

The Multicall3 contract allows you to execute multiple calls within a single transaction and aggregate the results. The [`viem` library](https://viem.sh/docs/contract/multicall#multicall) provides native support for Multicall3.

### Story Aeneid Testnet Multicall3 Deployment Info

(Same address across all EVM chains)

```json
{
  "contractName": "Multicall3",
  "chainId": 1516,
  "contractAddress": "0xcA11bde05977b3631167028862bE2a173976CA11",
  "url": "https://aeneid.storyscan.xyz/address/0xcA11bde05977b3631167028862bE2a173976CA11"
}
```

### Main Functions

To batch multiple function calls, you can use the following functions:

1. **`aggregate3`**: Batches calls using the `Call3` struct.
2. **`aggregate3Value`**: Similar to `aggregate3`, but also allows attaching a value to each call.

```solidity
/// @notice Aggregate calls, ensuring each returns success if required.
/// @param calls An array of Call3 structs.
/// @return returnData An array of Result structs.
function aggregate3(Call3[] calldata calls) external payable returns (Result[] memory returnData);

/// @notice Aggregate calls with an attached msg value.
/// @param calls An array of Call3Value structs.
/// @return returnData An array of Result structs.
function aggregate3Value(Call3Value[] calldata calls) external payable returns (Result[] memory returnData);
```

### Struct Definitions

- **Call3**: Used in `aggregate3`.
- **Call3Value**: Used in `aggregate3Value`.

```solidity
struct Call3 {
  address target;      // Target contract to call.
  bool allowFailure;   // If false, the multicall will revert if this call fails.
  bytes callData;      // Data to call on the target contract.
}

struct Call3Value {
  address target;
  bool allowFailure;
  uint256 value;       // Value (in wei) to send with the call.
  bytes callData;      // Data to call on the target contract.
}
```

### Return Type

- **Result**: Struct returned by both `aggregate3` and `aggregate3Value`.

```solidity
struct Result {
  bool success;        // Whether the function call succeeded.
  bytes returnData;    // Data returned from the function call.
}
```

> 📘 Examples
>
> For detailed examples in Solidity, TypeScript, and Python, see the [Multicall3 repository](https://github.com/mds1/multicall/tree/main/examples).

### Limitations

For a list of limitations when using Multicall3, refer to the [Multicall3 README](https://github.com/mds1/multicall/blob/main/README.md#batch-contract-writes).

### Additional Resources

- [Multicall3 Documentation](https://github.com/mds1/multicall/blob/main/README.md)
- [Multicall Documentation from Viem](https://viem.sh/docs/contract/multicall#multicall)

### Full Multicall3 Interface

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


# ❓ Concepts FAQ
## *"What is the difference between License Tokens, Royalty Tokens, and Revenue Tokens?"*

<Table align={["left","left","left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>

      </th>

      <th style={{ textAlign: "left" }}>
        License Tokens
      </th>

      <th style={{ textAlign: "left" }}>
        Royalty Tokens
      </th>

      <th style={{ textAlign: "left" }}>
        Revenue Tokens
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        **Module**
      </td>

      <td style={{ textAlign: "left" }}>
        [📜 Licensing Module](doc:licensing-module)
      </td>

      <td style={{ textAlign: "left" }}>
        [💸 Royalty Module](doc:royalty-module)
      </td>

      <td style={{ textAlign: "left" }}>
        [💸 Royalty Module](doc:royalty-module)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        **Explanation**
      </td>

      <td style={{ textAlign: "left" }}>
        An ERC-721 NFT that gets minted from an IP Asset with specific license terms. It is essentially the license you hold that gives you access to use the associated IP Asset based on the terms in the License Token.

        A License Token is burned when it is used to register an IP Asset as a derivative of another.
      </td>

      <td style={{ textAlign: "left" }}>
        Each IP Asset has 100,000,000 Royalty Tokens associated, where each token represents the right of whoever owns them to claim 0.000001% of the gains ("*Revenue Tokens*") deposited into the IPA's Royalty Vault.
      </td>

      <td style={{ textAlign: "left" }}>
        These are the tokens that are actually used for payment (ex. $WIP).

        "*Royalty Tokens*" are used to claim these Revenue Tokens when an IP Asset earns them.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        **Associated Docs**
      </td>

      <td style={{ textAlign: "left" }}>
        [License Token](doc:license-token)
      </td>

      <td style={{ textAlign: "left" }}>
        [IP Royalty Vault](doc:ip-royalty-vault)
      </td>

      <td style={{ textAlign: "left" }}>
        [IP Royalty Vault](doc:ip-royalty-vault)
      </td>
    </tr>
  </tbody>
</Table>

# 🔍 Overview
A piece of Intellectual Property is represented as an [🧩 IP Asset](doc:ip-asset) and its associated [⚙️ IP Account](doc:ip-account), a smart contract designed to serve as the core identity for each IP. We also have various [🧱 Modules](doc:story-modules) to add functionality to IP Assets, like creating derivatives of them, disputing IP, and automating revenue flow between them.

# Architecture

<Image align="center" src="https://files.readme.io/251dabc-story-v1-architecture.png" />

Let's briefly introduce the layers mentioned in the above diagram:

## [🧩 IP Asset](doc:ip-asset)

When you want to bring an IP on-chain, you mint an ERC-721 NFT. This NFT represents **ownership** over your IP.

Then, you **register** the NFT in our protocol through the [IP Asset Registry](doc:ip-asset-registry). This deploys an [⚙️ IP Account](doc:ip-account), effectively creating an "IP Asset". The address of that contract is the identifier for the IP Asset (the `ipId`).

The underlying NFT can be traded/sold like any other NFT, and the new owner will own the IP Asset and all revenue associated with it.

## [⚙️ IP Account](doc:ip-account)

IP Accounts are smart contracts that are tied to an IP Asset, and do two main things:

1. Store the associated IP Asset's data, such as the associated licenses and royalties created from the IP
2. Facilitates the utilization of this data by various modules. For example, licensing, revenue/royalty sharing, remixing, and other critical features are made possible due to the IP Account's programmability.

The address of the IP Account is the IP Asset's identifier (the `ipId`).

## [🧱 Modules](doc:story-modules)

Modules are customizable smart contracts that define and extend the functionality of IP Accounts. Modules empower developers to create functions and interactions for each IP to make IPs truly programmable.

We already have a few core modules:

1. [📜 Licensing Module](doc:licensing-module): create parent\<->child relationships between IPs, enabling derivatives of IPs that are restricted by the agreements in the license terms (must give attribution, share 10% revenue, etc)
2. [💸 Royalty Module](doc:royalty-module): automate revenue flow between IPs, abiding by the negotiated revenue sharing in license terms
3. [❌ Dispute Module](doc:dispute-module): facilitates the disputing and flagging of IP
4. [👥 Grouping Module](doc:grouping-module): allows for IPs to be grouped together

## [🗂️ Registry](doc:registry)

The various registries on our protocol function as a primary directory/storage for the global states of the protocol. Unlike IP Accounts, which manage the state of specific IPs, a registry oversees the broader states of the protocol.

## [💊 Programmable IP License (PIL)](doc:programmable-ip-license)

The PIL is a real, off-chain legal contract that defines certain **License Terms** for how an IP Asset can be legally licensed. For example, how an IP Asset is commercialized, remixed, or attributed, and who is allowed to do that and under what conditions.

We have mapped these same terms on-chain so you can easily attach terms to your IP Asset for others to seamlessly and transparently license your IP.

# Payments and Revenue Tokens
All payments on Story are made with **Revenue Tokens**. These are ERC-20 tokens that have been whitelisted by our protocol.

## When do payments happen?

There are three common scenarios when you'd make a payment on Story:

1. Mint License Tokens - minting a license token from an IP Asset that has a minting fee. You do this either to hold the license token or use it to register your IP Asset as a derivative
2. Paying Commercial Revenue Share - your license terms with an ancestor IP Asset require you to forward a certain % of your revenue. For ex. share 50% of your revenue share with the parent IP Asset
3. Tipping - you just want to send some money to an IP Asset

## Whitelisted Revenue Tokens

Revenue Tokens are ERC-20 tokens that have been whitelisted by our protocol. Currently, the whitelisted revenue tokens are:

<Tabs>
  <Tab title="Aeneid Testnet">
    | Token  | Contract Address                             | Explorer                                                                                                                   | Mint                                                                                                                                                                                                                            |
    | :----- | :------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
    | WIP    | `0x1514000000000000000000000000000000000000` | <a href="https://aeneid.storyscan.xyz/address/0x1514000000000000000000000000000000000000" target="_blank">View here ↗️</a> | N/A                                                                                                                                                                                                                             |
    | MERC20 | `0xF2104833d386a2734a4eB3B8ad6FC6812F29E38E` | <a href="https://aeneid.storyscan.xyz/address/0xF2104833d386a2734a4eB3B8ad6FC6812F29E38E" target="_blank">View here ↗️</a> | [https://aeneid.storyscan.xyz/address/0xF2104833d386a2734a4eB3B8ad6FC6812F29E38E?tab=write\_contract#0x40c10f19](https://aeneid.storyscan.xyz/address/0xF2104833d386a2734a4eB3B8ad6FC6812F29E38E?tab=write_contract#0x40c10f19) |
  </Tab>

  <Tab title="Mainnet">
    | Token | Contract Address                             | Explorer                                                                                                                   | Mint |
    | :---- | :------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------- | :--- |
    | WIP   | `0x1514000000000000000000000000000000000000` | <a href="https://aeneid.storyscan.xyz/address/0x1514000000000000000000000000000000000000" target="_blank">View here ↗️</a> | N/A  |
  </Tab>
</Tabs>

## Testing

On Aeneid testnet, we have provided a `MockERC20` token for you to be able to mint and use

# IP Royalty Vault
<Accordion title="Skip the Read - 1 Minute Summary" icon="fa-info-circle">
  An IP Royalty Vault is a pool for all monetary inflows related to an IP Asset.

  Every IP Asset has 100,000,000 Royalty Tokens associated with it, where each token represents the right to 0.000001% of that IPA's revenue (*"Revenue Tokens"*) stored in the pool.

  Revenue Tokens are ERC-20 tokens used for payment (ex. WIP). These tokens must be whitelisted by the protocol to be used.
</Accordion>

Each IP Asset has an IP Royalty Vault, which acts as a pool for all monetary inflows related to an IP Asset's commercial exploration or from minting licenses. Anyone who holds Royalty Tokens (defined below) has the right to claim their share of this pool.

<Cards columns={1}>
  <Card title="IPRoyaltyVault.sol" href="https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/modules/royalty/policies/IpRoyaltyVault.sol" icon="fa-scroll" iconColor="#ccb092" target="_blank">
    View the smart contract for the IP Royalty Vault.
  </Card>
</Cards>

## Token Terminology

1. **Royalty Tokens**: the IP Royalty Vault contract is also the ERC-20 contract for the Royalty Tokens of each IP Asset. This means the address of the IP Royalty Vault for an IP Asset is also the ERC-20 token address of the Royalty Tokens. Each IP Asset has 100,000,000 Royalty Tokens associated, where each token represents 0.000001% of those gains. The holders of these Royalty Tokens can claim the Revenue Tokens (defined below) that are in the associated IP Royalty Vault.
2. **Revenue Tokens**: these are the tokens used for payment (ie. WIP). Royalty Tokens can be used to claim Revenue Tokens. Read below on whitelisting :arrow_heading_down:

### Whitelisted Revenue Tokens

An ERC-20 token must be whitelisted by our protocol in the [RoyaltyModule.sol contract](https://github.com/storyprotocol/protocol-core-v1/blob/e339f0671c9172a6699537285e32aa45d4c1b57b/contracts/modules/royalty/RoyaltyModule.sol#L50) to be used as a Revenue Token. Here are the whitelisted tokens:

<Tabs>
  <Tab title="Aeneid Testnet">
    | Token  | Contract Address                             | Explorer                                                                                                                   | Mint                                                                                                                                                |
    | :----- | :------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------- |
    | WIP    | `0x1514000000000000000000000000000000000000` | <a href="https://aeneid.storyscan.xyz/address/0x1514000000000000000000000000000000000000" target="_blank">View here ↗️</a> | N/A                                                                                                                                                 |
    | MERC20 | `0xF2104833d386a2734a4eB3B8ad6FC6812F29E38E` | <a href="https://aeneid.storyscan.xyz/address/0xF2104833d386a2734a4eB3B8ad6FC6812F29E38E" target="_blank">View here ↗️</a> | <a href="https://aeneid.storyscan.xyz/address/0xF2104833d386a2734a4eB3B8ad6FC6812F29E38E?tab=write_contract#0x40c10f19" target="_blank">Mint ↗️</a> |
  </Tab>

  <Tab title="Mainnet">
    | Token | Contract Address                             | Explorer                                                                                                                   | Mint |
    | :---- | :------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------- | :--- |
    | WIP   | `0x1514000000000000000000000000000000000000` | <a href="https://aeneid.storyscan.xyz/address/0x1514000000000000000000000000000000000000" target="_blank">View here ↗️</a> | N/A  |
  </Tab>
</Tabs>

## How to obtain Royalty Tokens?

When an IP Asset receives revenue, it is deposited into its IP Royalty Vault. In order to claim revenue from this vault, you must have the associated Royalty Tokens. Once any address owns Royalty Tokens of a given IP Asset, it is entitled to that % (% of the total supply of Royalty Tokens owned) of any future Revenue Token (that is whitelisted) received & in the IP Royalty Vault.

There are two ways that trigger the IP Royalty Vault deployment and make the initial Royalty Token distribution - whichever comes first:

1. A License Token is minted from an IP for the first time: the associated IP Account (the parent's IP Account) receives 100% of the Royalty Tokens
2. An IP registers as a derivative: the associated IP Account (the child's IP Account) receives 100% Royalty Tokens, and then distributes x% of it to ancestors based on the License Terms

Because Royalty Tokens are ERC-20, they can be transferred like any other token. Thus, the IP Account could send them to someone else, or even put them up for sale on the secondary market.

## How Revenue Flows

This section will help explain how revenue flows from the time of payment to being claimed by the royalty token holder. For the purposes of explanation, we will use an example from the [Liquid Absolute Percentage (LAP)](doc:liquid-absolute-percentage), but it is the same for any royalty policy.

Imagine we have a scenario where IPA4 tips IPA3 1M WIP by calling `payRoyaltyOnBehalf`.

1. Revenue Tokens flow to the Royalty Module contract. This contract then splits up the tokens based on the **royalty stack** on the receiving IPA. In this case, IPA3 has a royalty stack of 15%, so 850k tokens flow to IP Royalty Vault 3, and 150k tokens flow to the LAP contract.

![](https://files.readme.io/9cc0e9761fddd67bfeae74b14e2ec2bf51f686f72a145df096a96b54a17c5cd1-image.png)

<br />

2. The LAP contract separates the payment to the ancestors by calling `transferToVault`. In this case, IPA2 deserves 100k (10% of IPA3's earnings) and IPA1 deserves 50k (5% of IPA3's earnings).

![](https://files.readme.io/d91316a30d117ea76bd42eecf3030aa1c6bf251217193d4e6e504d0f7b7367b8-image.png)

<br />

3. Now that the Revenue Tokens are in the IP Royalty Vaults, the associated Royalty Token holders can claim from the vaults. Remember, the Revenue Tokens get claimed to whoever holds the Royalty Tokens. In the most common case, they are in the IP Account since that's where they originate. To claim, you would call either `claimRevenueOnBehalfByTokenBatch` or `claimRevenueOnBehalf`.

![](https://files.readme.io/ef288631b074cd82d2edb95b6b27844db6a36cd8976a29f20a0cd9b393752218-image.png)

### External Royalty Policies

Revenue Tokens can also move from a vault to another vault via the functions `claimByTokenBatchAsSelf` located in the `IpRoyaltyVault.sol` contract. For this to be possible the vault that is claiming revenue tokens needs to own Royalty Tokens of the vault being claimed from. This can be particularly useful when used together with external royalty policies.

Vaults can only claim from other vaults if those other vaults belong to IPs in the same derivative chain. If a vault owns royalty tokens from an IP but it is not an ancestor of that IP, it is not possible to claim rewards with those royalty tokens.

# Liquid Absolute Percentage (LAP)
<Accordion title="Skip the Read - 1 Minute Summary" icon="fa-info-circle">
  Let's come up with an example: An IP Asset ('C') is a child of 'B', and 'B' is a child of 'A', such that it goes A▶️B▶️C. 'A' specifies that any descendant must share 5% of their revenue with it. On the other hand, 'B' specifies that any descendants must share 10% of their revenue with it.

  Okay, great. Let's see what happens in two (independent) common scenarios:

  1. **Minting a License** - 'C' mints a license from 'B' that costs 100 WIP. When 'C' pays 'B' 100 WIP to mint a license, 'A' claims 5 WIP from B. In the end, 'B' only gets 95 WIP.
  2. **Tipping Directly** - 'C' is a comic book that is super well written. Someone tips 100 WIP to 'C' because they love it. 'A' claims 5 WIP from 'C'. 'B' claims 10 WIP from 'C'. In the end, 'C' only gets 85 WIP.
</Accordion>

The Liquid Absolute Percentage (LAP) defines that each parent IP Asset can choose a minimum royalty percentage that all of its downstream IP Assets in a derivative chain will share from their monetary gains as defined in the license agreement.

<Cards columns={1}>
  <Card title="RoyaltyPolicyLAP.sol" href="https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/modules/royalty/policies/LAP/RoyaltyPolicyLAP.sol" icon="fa-scroll" iconColor="#ccb092" target="_blank">
    View the smart contract for the LAP Royalty Policy.
  </Card>
</Cards>

## Prerequisites

Before continuing, make sure you have read the [IP Royalty Vault](doc:ip-royalty-vault) terminology.

The License Royalty % in this page correspond to the same value as the `commercialRevShare` on the PIL terms.

## Royalty Payment and Claiming Flow

In the image below, IPA 1 and IPA 2 - due to being ancestors of IPA 3 - have a % economic right over revenue made by IPA 3. Key notes to understand the derivative chain below:

* License Royalty Percentage - this percentage is selected by the user and it means the percentage that the user wants - according to LAP rules - in return for allowing other users to remix its IPA.
* Royalty Stack - is the revenue an IPA has to pay all its ancestors. For LAP royalty stack = sum of parents royalty stack + sum of licenses percentages used to connect to each parent
  * Royalty Stack IPA 2 = Royalty Stack IPA 1 + License Royalty % between IPAs 1 and 2 = 0% + 5% = 5%
  * Royalty Stack IPA 3 = Royalty Stack IPA 2 + License Royalty % between IPAs 2 and 3 = 5% + 10% = 15%
* Royalty Tokens flow to the IPA initially when a vault is deployed. The Royalty Tokens can be transferred to any other address and after that transfer any future royalty inflow will be claimable by that new address which now holds the RTs.

![](https://files.readme.io/906b26bc193243391a86a33823d56568afd60eb0cc57b3ab42b77a97f1975142-image.png)

Now, let's imagine a scenario where a new IP Asset 4 intends to join the derivative chain as a derivative of IP Asset 3. An example flow sequence below:

1. IP Asset 4 pays 1M WIP in royalties to its parent IPA 3 by calling `payRoyaltyOnBehalf`. Note that the royalty process is the same whether the payment is the license minting fee or any other royalty payment - with the difference being that the license minting fee is made via `payLicenseMintingFee` and is mandatory upon derivative creation. Once a payment is made, a share equivalent to the IPA 3 royalty stack % is sent to the royalty policy contract and the remaining amount is sent to the IPA 3 vault.

![](https://files.readme.io/3cb2287fee2bcbdfaed6a7068b7fcd1769d32f73f46d5f29e2c4b404b9d79f64-image.png)

2. Each ancestor can call `transferToVault` on the royalty policy contract to receive the amount each ancestor has the right to claim from a given descendant. Funds are moved to the ancestor's IP Royalty Vault.
   1. 100k WIP are transferred to the IP Royalty Vault 2 since it the right to 10% of all IPA 2 descendants revenue
   2. 50k WIP are transferred to the IP Royalty Vault 1 since it the right to 5% of all IPA 2 descendants revenue

![](https://files.readme.io/2c12c635e25d9a815cd7d47ece1b75b72974208540b136e4d2405c994b791bd4-image.png)

3. In the final step of the claiming flow, any Royalty Token holder address can call `claimRevenueOnBehalfByTokenBatch`/`claimRevenueOnBehalf` (for non-vault claimers) or `claimRevenueByTokenBatchAsSelf` (when the claimer is an IP Royalty Vault) to claim revenue tokens. In the current example:
   1. 50k WIP are claimed to the IPA 1 which holds 100% RT1
   2. 100k WIP are claimed to the IPA 2 which holds 100% RT2
   3. 850k WIP are claimed by IPA 3 which holds 100% RT3\
      Note: Any royalty token holder address can claim - whether it is a smart contract, IPA, or EOA.

![](https://files.readme.io/dffc067a7dc834888bebb3c84a293ce120a30a8160f81dd744356c6428244df6-image.png)

# Liquid Relative Percentage (LRP)
<Accordion title="Skip the Read - 1 Minute Summary" icon="fa-info-circle">
  Let's come up with an example: An IP Asset ('C') is a child of 'B', and 'B' is a child of 'A', such that it goes A▶️B▶️C. 'A' specifies that any **direct** descendant must share 5% of their revenue with it. On the other hand, 'B' specifies that any **direct** descendants must share 10% of their revenue with it.

  Okay, great. Let's see what happens in two (independent) common scenarios:

  1. **Minting a License** - 'C' mints a license from 'B' that costs 100 WIP. When 'C' pays 'B' 100 WIP to mint a license, 'A' claims 5 WIP from B. In the end, 'B' only gets 95 WIP.
  2. **Tipping Directly** - 'C' is a comic book that is super well written. Someone tips 100 WIP to 'C' because they love it. 'B' claims 10 WIP from 'C'. 'A' claims 0.5 WIP from 'B' (5% of 10). In the end, 'C' only gets 90 WIP.
</Accordion>

The Liquid Relative Percentage (LRP) royalty policy defines that each parent IP Asset can choose a minimum royalty percentage that only the direct derivative IP Assets in a derivative chain will share from their monetary gains as defined in the license agreement.

<Cards columns={1}>
  <Card title="RoyaltyPolicyLRP.sol" href="https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/modules/royalty/policies/LRP/RoyaltyPolicyLRP.sol" icon="fa-scroll" iconColor="#ccb092" target="_blank">
    View the smart contract for the LRP Royalty Policy.
  </Card>
</Cards>

## Prerequisites

Before continuing, make sure you have read the [IP Royalty Vault](doc:ip-royalty-vault) terminology.

The License Royalty % in this page correspond to the same value as the `commercialRevShare` on the PIL terms.

## Royalty Payment and Claiming Flow

In the image below, IPA 1 and IPA 2 - due to being ancestors of IPA 3 - have a % economic right over revenue made by IPA 3. Key notes to understand the derivative chain below:

* License Royalty Percentage - this percentage is selected by the user and it means the percentage that the user wants - according to LRP rules - in return for allowing other users to remix its IPA.
* Royalty Stack LRP - is the revenue an IPA has to pay all its parent. For LRP royalty stack = sum of licenses percentages used to connect to each parent
  * Royalty Stack IPA 2 = License Royalty % between IPAs 1 and 2 = 5%
  * Royalty Stack IPA 3 = License Royalty % between IPAs 2 and 3 = 10%
* Royalty tokens flow to the IPA initially when a vault is deployed. The Royalty Tokens can be transferred to any other address and after that transfer any future royalty inflow will be claimable by that new address which now holds the RTs.

![](https://files.readme.io/444805c5024234346548005c7dc2d271274eff452ebab10cf7245ac44d5f9d56-image.png)

Now, let's imagine a scenario where a new IP Asset 4 intends to join the derivative chain as a derivative of IP Asset 3. An example flow sequence below:

1. IP Asset 4 pays 1M WIP in royalties to its parent IPA 3 by calling `payRoyaltyOnBehalf`. Note that the royalty process is the same whether the payment is the license minting fee or any other royalty payment - with the difference being that the license minting fee is made via `payLicenseMintingFee` and is mandatory upon derivative creation. Once a payment is made, a share equivalent to the IPA 3 royalty stack % is sent to the royalty policy contract and the remaining amount is sent to the IPA 3 vault.

![](https://files.readme.io/b2145b6218b5d08ee3a40076afcbe6f86727fb2df4a90f457539c6f17eefc528-image.png)

2. Each ancestor can call `transferToVault` on the royalty policy contract to receive the amount each ancestor has the right to claim from a given descendant. Funds are moved to the ancestor's IP Royalty Vault.
   1. 95k WIP are transferred to the IP Royalty Vault 2 since it has the right to 10% of all IPA 2 descendants revenue and has to pay 5% of its revenue to its direct parent IPA 1. So 100k is received from IPA 3 and 5k is paid to IPA 1, resulting in IPA 2 keeping 100k - 5k = 95k.
   2. 5k WIP are transferred to the IP Royalty Vault 1 since it has the right to 0.5% of all IPA 2 descendants revenue. IPA 1 has the right to 5% of revenue earned by IPA 2, which in turn has 10% of revenue earned by IPA 3. Given LRP royalty policy considers relative percentages, then IPA 1 has the right to 10%\*5% = 0.5% of revenue earned by IPA 3.

![](https://files.readme.io/a12749274bbad8b4f72f6bdcf2f79cd9c5945c677a0c28303a6d6ac4a180f1b0-image.png)

3. In the final step of the claiming flow, any Royalty Token holder address can call `claimRevenueOnBehalfByTokenBatch`/`claimRevenueOnBehalf` (for non-vault claimers) or `claimRevenueByTokenBatchAsSelf` (when the claimer is an IP Royalty Vault) to claim revenue tokens. In the current example:
   1. 5k WIP are claimed to the IPA 1 which holds 100% RT1
   2. 95k WIP are claimed to the IPA 2 which holds 100% RT2
   3. 900k WIP are claimed by IPA 3 which holds 100% RT3\
      Note: Any royalty token holder address can claim - whether it is a smart contract, IPA, or EOA.

![](https://files.readme.io/d14763b11b16f307edbef7bced62f8e1be05c8ab78dbc7bf47cb0edead49dbd6-image.png)

# 💸 Royalty Module
The Royalty Module defines how revenue flows between IPs on Story. More specifically, between parent and child [🧩 IP Assets](doc:ip-asset). There are two common scenarios when revenue flow would happen:

1. Minting a License - when you mint a [License Token](doc:license-token) that has a `mintingFee`. When this is paid by someone (who wants to register a derivative or simply hold the license), the revenue should flow up the ancestry chain.
2. Tipping Directly - if someone sends revenue to an IP directly, it should flow up the chain.

<Cards columns={1}>
  <Card title="RoyaltyModule.sol" href="https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/modules/royalty/RoyaltyModule.sol" icon="fa-scroll" iconColor="#ccb092" target="_blank">
    View the smart contract for the Royalty Module.
  </Card>
</Cards>

## High-Level Example

The below example (using [Liquid Absolute Percentage](doc:liquid-absolute-percentage)) shows what happens when an IP Asset 4 (IPA4) tips IPA3 1,000,000 WIP.

1. Revenue first flows to the Royalty Module contract
2. Royalty Module sends WIP to both IPA3 and the LAP contract based on the **royalty stack** (15%)
3. LAP will distribute funds to further ancestors since they have negotiated some license agreement where they are due revenue from IPA3's earnings.

> Don't worry if you don't understand everything in the picture, this is just to show you an overview of what the Royalty Module is all about.

![](https://files.readme.io/37507c9dda61f5be3028edc020733b183f728575ff592f4f81ecc405b3e41df6-image.png)

## Royalty Policies

Royalty policies are a component of the license agreement between two IP Assets. It defines how revenue flow actually happens.

The Royalty Module supports both whitelisted/native policies created by our team directly, and external ones created by you.

> 📘 Note on Royalty Policies
>
> An IP Asset without any parents can mint licenses with different royalty policies while a derivative IP Asset inherits the royalty policy of its parents.
>
> Additionally, there will always be one royalty policy governing every link an IP Asset has with each of its derivatives.

### Whitelisted/Native Royalty Policies

These policies require governance whitelisting and guarantee royalty token distribution to ancestors.

1. [Liquid Absolute Percentage (LAP)](doc:liquid-absolute-percentage)
2. [Liquid Relative Percentage (LRP)](doc:liquid-relative-percentage)

### External Royalty Policies

These policies can be registered in a permission-less way and stipulate their own royalty and revenue distribution rules.

* [External Royalty Policies](doc:external-royalty-policies)

## Royalty Token % vs Royalty Stack %

When creating a derivative, the creator will want to answer the question: "How much percentage of my IP earnings will I keep and how much will go to ancestor IPs?

To answer this question two concepts are important:

1. Royalty Token - Each IP Asset has 100,000,000 Royalty Tokens associated, where each token represents 0.000001% of the capital that enters each IP Royalty Vault. The holders of these Royalty Tokens can claim the Revenue Tokens that are in the associated IP Royalty Vault.
2. Royalty Stack - is the percentage of IP revenue that has to be paid to ancestors via Whitelisted/Native royalty policies. External royalty policies do not use the royalty stack percentage - only Whitelisted/Native royalties policies do.

Let's imagine the scenario below:

* IP1 is a root IP Asset.
* IP2 is a derivative of IP1.
* User A has 100% of Royalty Tokens of IP1
* User B has 20% of Royalty Tokens of IP2
* User C has 80% of Royalty Tokens of IP2
* IP2 Royalty Stack is 10% - meaning that all its ancestor IPs via Native/Whitelisted policies require IP2 to pay 10% of its revenue in order to create the derivative. In this case, there is only 1 ancestor which is IP1. IP1 demands 10% of IP2's future revenue in order to create a derivative.

In the image below there is an example of a one million WIP payment made to IP2. In the image we can see how much each Royalty Token holder of the entire derivative chain receives when the payment is made.

![](https://files.readme.io/a96e7d196a85f69dceb2b125ce70008115e15d0aa76b4e14b0dff2007525051b-image.png)

* RT Holder A - From the one million WIP payment gets 100k WIP. Royalty Stack percentage is paid first and RT Holder A has 100% of Royalty Tokens of IP1 so gets to keep the whole 100k WIP.
* RT Holder B - From the one million WIP payment gets 180k WIP. IP2 holders as a whole receive 900k WIP from the original one million WIP payment. Those 900k WIP are then split among the different Royalty Token holders of IP2 which are B and C. B has 20% of Royalty Tokens of IP2 so it receives 900k WIP \* 20% = 180k.
* RT Holder C - From the one million WIP payment gets 720k WIP. IP2 holders as a whole receive 900k WIP from the original one million WIP payment. Those 900k WIP are then split among the different Royalty Token holders of IP2 which are B and C. C has 80% of Royalty Tokens of IP2 so it receives 900k WIP \* 80% = 720k.

## Derivative Chain Configurations

![](https://files.readme.io/79bd27f-image.png)

The derivative chain can assume multiple configurations.

Each IP Asset is restricted to a total royalty % of 100%. It will revert when minting a license that would make the IPA reserve more than 100% of its royalty tokens for ancestors, since this would make no sense.

# External Royalty Policies
There can be many flavors and variations of royalty distribution rules as we observe in the real world. The same can be expected onchain. Whenever a use case requires unique and specific royalty rules, then those set of rules can be registered as an **External Royalty Policy**.

## 1. What is an External Royalty Policy?

It is a smart contract that inherits a specific interface called `IExternalRoyaltyPolicy`, which defines the view function below:

<Cards columns={1}>
  <Card title="IExternalRoyaltyPolicy.sol" href="https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/interfaces/modules/royalty/policies/IExternalRoyaltyPolicy.sol" icon="fa-scroll" iconColor="#ccb092" target="_blank">
    View the smart contract for external royalty policies.
  </Card>
</Cards>

```sol IExternalRoyaltyPolicy.sol
/// @notice Returns the amount of royalty tokens required to link a child to a given IP asset
/// @param ipId The ipId of the IP asset
/// @param licensePercent The percentage of the license
/// @return The amount of royalty tokens required to link a child to a given IP asset
function getPolicyRtsRequiredToLink(address ipId, uint32 licensePercent) external view returns (uint32);
```

After developing your smart contract make sure it inherits the interface above and you can register your new External Royalty Policy by calling `registerExternalRoyaltyPolicy` function in [RoyaltyModule.sol](https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/modules/royalty/RoyaltyModule.sol).

## 2. How does it work?

Let's follow an example of a new External Royalty Policy called "Policy X".

### External Royalty Policies are selected by users

An IPA owner decides the royalty policy he/she wants to allow the IP to be remixed with. There are multiple options of royalty rules that can be chosen such as LAP, LRP and other External Royalty Policies. Let's say the user decides to mint a license token with "Policy X". After that, IP2 remixes IP1 and IP3 remixes IP2 and we have the situation as the image below:

![](https://files.readme.io/0253ab910f450ded528c9c6464896fffe10b335c22888d864d56f0e8633c7f7a-image.png)

Every time there is a remix - the link between the parent and derivative has 2 data points associated:

1. The royalty policy address
   1. "Policy X" address in the example
2. The percentage of royalty tokens the parent demands from derivatives. This percentage can have different meanings depending on the royalty policy being used - ie. it can be a relative percentage, an absolute percentage, an adjusted percentage according to specific rules, etc.
   1. 10% between IP1 and IP2
   2. 50% between IP2 and IP3

### External Royalty Policies receive royalty tokens from their users' IPs

Following the example, when each remix is made and during the `onLinkToParents` function call in [RoyaltyModule.sol](https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/modules/royalty/RoyaltyModule.sol), the function `getPolicyRtsRequiredToLink` is called on the "Policy X" address.

```sol IExternalRoyaltyPolic.sol
/// @notice Returns the amount of royalty tokens required to link a child to a given IP asset
/// @param ipId The ipId of the IP asset
/// @param licensePercent The percentage of the license
/// @return The amount of royalty tokens required to link a child to a given IP asset
function getPolicyRtsRequiredToLink(address ipId, uint32 licensePercent) external view returns (uint32);
```

It should return the % of derivative's royalty tokens that the royalty policy demands for the link to happen. That share of royalty tokens are sent to the "Policy X" contract. In the example case:

* "Policy X" receives 3% of RT2 token supply that it can then redistributed to its userbase. IP1 owner wanted 10%, however - let's assume for the sake of the example - that due to the specific use case of "Policy X" and its custom logic, the IP2 owner is granted a special status in the platform in which it it has a 70% discount on the % share it has to give parent IPs due to having a very large distribution network to promote IPs. Therefore, instead of having to give 10% as the license percentage indicated it only gives 3%.
* "Policy X" receives 50% of RT3 token supply that it can then redistributed to its userbase.

![](https://files.readme.io/199d471b5a9a34bfb2dfd28a4933123fd7a35c530b01f5360040784345d1b461-image.png)

### External Royalty Policies redistribute value back to their users according to custom rules

There are two ways in which an External Royalty Policy can redistribute value back to its users:

1. Send Royalty Tokens directly to its users
2. Keep the Royalty Tokens in the External Royalty Policy contract and have users claim Revenue Tokens through the said contract

Let's explore both in the context of "Policy X". Let's say that from the 50% of RT3 token supply "Policy X" received - 40% are kept in the "Policy X" contract and 10% are sent to an ancestor royalty vault (IP1).

![](https://files.readme.io/ecbc7a9db20ed8fee4c110b09239649490757cdb1f4ed63af20a98d2cd60cbc1-image.png)

Now let's imagine there is a 1M payment made to IP3 - an example of how the flow would be:

![](https://files.readme.io/4c9d0f8ca5e6c2b46f420b67016dbab667d429dba3fa2fa7b150deae9d4cf913-image.png)

From the 1M WIP inflow to IP3 Royalty Vault:

* 500k WIP are claimed by the IP Account 3 which had 50% of RT3 token supply
* 100k WIP are claimed by the IP1 Royalty Vault which has 10% of RT3 token supply via `claimByTokenBatchAsSelf`  function
* 400k WIP are claimed by "Policy X" which has 40 of RT3 token supply. This amount is further split by "Policy X" custom contract according to its specific rules - which define y% and z% - to its users.

# 👥 Grouping Module
The Grouping Module enables the creation and management of group IP Assets, supporting a royalty pool for the group.

<Cards columns={1}>
  <Card title="GroupingModule.sol" href="https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/modules/grouping/GroupingModule.sol" icon="fa-scroll" iconColor="#ccb092" target="_blank">
    View the smart contract for the Grouping Module.
  </Card>
</Cards>

`GroupingModule.sol` is the main entry point for the grouping workflows. It is **stateless** and responsible for:

* Registering a new group
* Adding an IPA to a group
* Removing an IPA from a group
* Checking whether a group contains a specific IPA
* Get the total number of IPAs of a group

## Creating a Group IPA

Similar to the IP Asset registration process, in which you must have a minted NFT to register and then an IP Account is created, the same applies to Group IP Assets. You must have a minted ERC-721 NFT (that represents the ownership of the group) to register as a group, and then when you register, an IP Account for the group is deployed.

Anyone can create a new group.

### Group IP Asset Registry

Similar to how when an IP Asset is created an IP Account is deployed & registered through the [IP Asset Registry](doc:ip-asset-registry), the Group's IP Account is deployed and registered through the [Group IP Asset Registry](doc:group-ip-asset-registry). This is responsible for managing the registration and tracking of Group IP Assets, including the group members and reward pools.

## The Group's IP Account

The Group IP Account should function equivalently to a normal IP Account, allowing attachment of license terms, creation of derivatives, execution with modules, and other interactions. It also has the same common interface of IP Account. Hence, the Group IP Account can be applied to anywhere where IP Account can be applied.

Besides the common interfaces of IP Account, the Group IP Account has functions to manage the adding/removing of individual IPAs in the group.

## Adding & Removing from a Group

Only the owner of a group can add/remove IP Assets. You **do not** have to own an IP Asset to add it to your group.

### Conditions to Join a Group

An IPA must include one license terms that matches the license terms of the group. An IPA may include other license terms in addition to the one that matches the group.

### Groups Becoming Locked

A group IPA is locked when:

1. it has derivative IPs registered or
2. when someone mints a license token from the group.

Once the group is locked, IPAs cannot be removed from it, but new IPAs can still be added.

## Group Restrictions

* A derivative IP of a group IP can only have the group IP as its sole parent
* A Group IP cannot attach License Terms that use the LAP Royalty policy
* An empty group cannot have derivative IPs or mint License Tokens
* A Group IP cannot be registered as a derivative as another parent IP
* **Single License Term Validation:** Ensure that a Group IPA can only attach one license term common to all members
* **Consistent License Config Validation:** When adding an IP to a group, the Group and IP must have the same mintingFee and licenseHook in the LicenseConfig, and the Group’s commercial revenue share must be greater than or equal to the IP’s share
* **Freezing License Config Items:** Once a Group gains its first member, the mintingFee, licensingHook, and licensingHookData are frozen. The Group’s commercial revenue share can only increase
* **Group Max Size Limit:** Enforce a maximum group size of 1000

## :blue_book: Example

Let's say you have an AI bot that uses training data to continuously learn and produce better content. The training data is a Group IPA that is the root, and the AI bot is a derivative IPA of the training data. And any time the AI bot gets paid, the revenue flows back to the training data as revenue.

Now you want to add more training data to the group. Since the group is now locked (you linked a derivative to it), you should register a new Group IPA as a root, and then a new AI bot as a derivative.

# Module Registry
<Cards columns={1}>
  <Card title="ModuleRegistry.sol" href="https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/registries/ModuleRegistry.sol" icon="fa-scroll" iconColor="#ccb092" target="_blank">
    View the smart contract for the Module Registry.
  </Card>
</Cards>

The Module Registry maintains and updates the global list of modules and hooks registered permissionlessly on Story. It can enable/disable modules on a per-IP Account basis for granular control over each IP Account's interaction with modules and hooks.

**This module is likely not very important for you** unless you wish to dive into creating/reading modules.

# 🗂️ Registry
The various registries on Story function as a primary directory/storage for the global states of the protocol. Obviously, they also contain functions to update that storage.

Unlike [⚙️ IP Accounts](doc:ip-account), which manage the state of specific IPs, a **registry** oversees the broader states of the protocol.

# Types of Registries

Below are all of the registries on Story.

## [IP Asset Registry](doc:ip-asset-registry)

Responsible for registering IPs into the protocol.

## [Group IP Asset Registry](doc:group-ip-asset-registry)

Responsible for registering and maintaining Group IP Assets.

## [License Registry](doc:license-registry)

Stores all license-related states within the protocol, like attaching License Terms to IP Assets, registering derivatives, creating new License Templates, etc.

## [Module Registry](doc:module-registry)

Maintains and updates the global list of modules and hooks registered permissionlessly on Story

# IP Asset Registry
<Cards columns={1}>
  <Card title="IPAssetRegistry.sol" href="https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/registries/IPAssetRegistry.sol" icon="fa-scroll" iconColor="#ccb092" target="_blank">
    View the smart contract for the IP Asset Registry.
  </Card>
</Cards>

The IP Asset Registry is responsible for registering IPs into the protocol. It deploys a dedicated [IP Account](doc:ip-account) contract for each new IP Asset registered on the protocol (*NOTE: This registry inherits from the* [IP Account Registry](https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/registries/IPAccountRegistry.sol))

### Notable Functions

```sol IPAssetRegistry.sol
function register(uint256 chainid, address tokenContract, uint256 tokenId) external whenNotPaused returns (address id)
```

This function registers an ERC-721 NFT as a new IP Asset on Story.

# Group IP Asset Registry
<Cards columns={1}>
  <Card title="GroupIPAssetRegistry.sol" href="https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/registries/GroupIPAssetRegistry.sol" icon="fa-scroll" iconColor="#ccb092" target="_blank">
    View the smart contract for the Group IP Asset Registry.
  </Card>
</Cards>

The Group IP Asset Registry is responsible for managing the registration and tracking of Group IP Assets, including the group members and reward pools.

The Group IP Asset Registry will maintain grouping relationship on-chain between the Group's IP Account and individual IP Accounts through a mapping:

```sol GroupIPAssetRegistry.sol
mapping(address groupIpId => EnumerableSet.AddressSet memberIpIds) groups;
```

### Notable Functions

```sol GroupIPAssetRegistry.sol
function registerGroup(address groupNft, uint256 groupNftId, address rewardPool) external onlyGroupingModule whenNotPaused returns (address groupId)
```

This function registers a new Group IPA on Story.

```sol GroupIPAssetRegistry.sol
function addGroupMember(address groupId, address[] calldata ipIds) external onlyGroupingModule whenNotPaused
```

Adds already registered IPAs to an existing Group IPA.

```sol GroupIPAssetRegistry.sol
function removeGroupMember(address groupId, address[] calldata ipIds) external onlyGroupingModule whenNotPaused
```

Removes registered IPAs from a Group IPA.

# License Registry
<Cards columns={1}>
  <Card title="LicenseRegistry.sol" href="https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/registries/LicenseRegistry.sol" icon="fa-scroll" iconColor="#ccb092" target="_blank">
    View the smart contract for the License Registry.
  </Card>
</Cards>

The License Registry stores all license-related states within the protocol, including managing global state like registering new License Templates like the [Programmable IP License (PIL💊)](doc:programmable-ip-license), attaching licenses to individual [IP Assets](doc:ipasset), registering derivatives, and the like:

```sol LicenseRegistry.sol
struct LicenseRegistryStorage {
  /// The default license template address
  address defaultLicenseTemplate;
  /// The default license terms ID
  uint256 defaultLicenseTermsId;
  /// Registered license templates
  mapping(address licenseTemplate => bool isRegistered) registeredLicenseTemplates;
  /// Mapping of parent IPs to derivative IPs
  mapping(address childIpId => EnumerableSet.AddressSet parentIpIds) parentIps;
  /// Mapping of derivative IPs to parent IPs
  mapping(address parentIpId => EnumerableSet.AddressSet childIpIds) childIps;
  /// attachedLicenseTerms Mapping of attached license terms to IP IDs
  mapping(address ipId => EnumerableSet.UintSet licenseTermsIds) attachedLicenseTerms;
  /// Mapping of license templates to IP IDs
  mapping(address ipId => address licenseTemplate) licenseTemplates;
  /// Mapping of minting license configs to a licenseTerms of an IP
  mapping(bytes32 ipLicenseHash => Licensing.LicensingConfig licensingConfig) licensingConfigs;
  /// Mapping of minting license configs to an IP, the config will apply to all licenses under the IP
  mapping(address ipId => Licensing.LicensingConfig licensingConfig) licensingConfigsForIp;
}
```

### Notable Functions

```sol LicenseRegistry.sol
function attachLicenseTermsToIp(address ipId, address licenseTemplate, uint256 licenseTermsId) external onlyLicensingModule
```

This function allows you to attach License Terms to an IP Asset.

```sol LicenseRegistry.sol
function registerDerivativeIp(address childIpId, address[] calldata parentIpIds, address licenseTemplate, uint256[] calldata licenseTermsIds, bool isUsingLicenseToken) external onlyLicensingModule
```

This function allows you to register an IP Asset as a derivative of another IP Asset, unlocking things like claimable royalty flows from the [💸 Royalty Module](doc:royalty-module).

# ⚙️ IP Account
> 🐦 Skip the Read
>
> Get a quick 2-minute overview of IP Accounts [here](https://twitter.com/jacobmtucker/status/1787603252198134234).

When an [🧩 IP Asset](doc:ip-asset) is registered, it is given an associated **IP Account**. An IP Account is a modified ERC-6551 (Token Bound Account) implementation. It is a separate contract bound to the IP Asset for controlling permissions around interactions with Story's modules or storing the IP's associated data. Upon registration, an IP Asset is assigned a unique ID. This ID is the address of the IP Account that is bound to the IP Asset.

![](https://files.readme.io/aab60607fd795080b061d93bfdfaf9a800930db861be332d205a48d637e234f1-image.png)

An IP Account mainly does two things:

1. Stores comprehensive IP-related data, including metadata and ownership details of associated assets such as the License Tokens or Royalty Tokens that are created from the IP.
2. Facilitates the utilization of this data by various modules. These modules interact with and contribute to the IP Account, creating and storing data. For example, licensing, revenue/royalty sharing, remixing, disputing an IP, and other modules are made possible due to the IP Account's programmability.

> 📘 Transferring the Underlying NFT
>
> If the underlying NFT is transferred, the new owner is also automatically the owner of the associated IP Asset & IP Account.

## `execute` and `executeWithSig`

A key feature of IP Account is the generic `execute()` function, which allows calling arbitrary modules within Story via encoded bytes data (thus extensible for future modules). Additionally, there is a `executeWithSig()` function that enables users to sign transactions and have others execute on their behalf for seamless UX.


# UMA Arbitration Policy
> 📘 UMA
>
> For detailed information on how UMA's dispute resolution works, [visit their website](https://uma.xyz/).

This arbitration policy is a dispute resolution mechanism that uses UMA’s optimistic oracle to verify disputes. Below we share a high-level overview of how the UMA dispute process works.

## Smart Contract Flow Diagram

![](https://files.readme.io/e0dfb0a226bdd29ab3adede7d1df7d6662497331e1b92319ee1ad8344dc5dfa3-image.png)

1. Raise Dispute - The first step to initiate a dispute against an IP Asset is to call the `raiseDispute` function on [DisputeModule.sol](https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/modules/dispute/DisputeModule.sol). This function will in turn call `assertTruth` on UMA's `OptimisticOracleV3.sol`. To initiate a dispute the dispute initiator will need to post a bond of at least the minimum bond defined by UMA for the selected currency. Note that this bond will be lost if the dispute is deemed not verifiably correct by the oracle.
2. (Optional) Dispute Assertion / Counter Dispute - After the `raiseDispute` call there is a period of time called liveness in which a counter dispute can be submitted. The liveness period is split in two parts: (i) the first part of the liveness period in which only the IP owner can counter dispute and (ii) a second part in which any address can counter dispute - which can be done by calling `disputeAssertion` on `ArbitrationPolicyUMA.sol`. To counter a dispute the caller will need to post a bond of the same amount and currency that was used by the dispute initiator when raising a dispute. Note that this bond will be lost if the original dispute is deemed to be verifiably correct by the oracle.
3. Settle Assertion
   1. If nobody submitted a counter dispute then when the liveness period is over, any address can call `settleAssertion` on UMA's `OptimisticOracleV3.sol`.
   2. If somebody has submitted a counter dispute before the liveness period is over, then the dispute is escalated to UMA decision makers who will judge and make a decision on whether the IP is infringing or not. After the decision has been made, then any address can call `settleAssertion` on UMA's `OptimisticOracleV3.sol`.

## Dispute Evidence Submission Guidelines

When raising a dispute or making a counter dispute, both parties can submit dispute evidence. Dispute evidence refers to a text document that oracle participants will use & read from to make a judgement on the dispute.

### Burden of Proof

In all disputes with UMA arbitration policy, the burden of proof lies with the party creating the dispute. This means that the disputer must provide clear, compelling, and verifiable evidence to prove the dispute beyond reasonable doubt. Disputes that do not meet this high bar can be counter-disputed with the disputing party losing their bond.

### Document Characteristics

> 🚧 Experimental
>
> As the process is still experimental, we can expect iteration and fine-tuning on the contents/formats of how the evidence should be submitted.

Every document should have the following characteristics:

* It should be a text document. Can have images or video if necessary.

* It should be uploaded on IPFS.

* It should not take the reviewer more than 1 hour to review the dispute evidence document - the reviewer's time is limited and the evidence could be deemed invalid if it would take too much time to review. Best efforts will be applied to solve a dispute but please keep it concise to have your dispute evidence be valid.

Depending on what the type of the Dispute Tag is, you also need to include in the evidence the "Dispute Evidence Contents of the table below:

<Table align={["left","left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        Dispute Tag
      </th>

      <th style={{ textAlign: "left" }}>
        Dispute Evidence Contents
      </th>

      <th style={{ textAlign: "left" }}>
        Dispute review process (Human reviewer instructions)
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        `IMPROPER_REGISTRATION`
      </td>

      <td style={{ textAlign: "left" }}>
        A. Showcase or pointer to the pre-existing IP that is being infringed upon by the disputed IP

        B. Proof of public display of the pre-existing IP at an earlier date than the infringing IP (onchain or offchain) and/or instructions on where/how to check it
      </td>

      <td style={{ textAlign: "left" }}>
        1. Check if the pre-existing  is the same or very similar to the disputed IP using input A
           * Mickey Mouse with 1 pixel difference is an infringement
           * Mickey Mouse with a new hat is an infringement unless it’s a derivative of the original Mickey Mouse with an appropriate license
        2. Check the registration date of the pre-existing IP using input B
        3. Confirm that the disputed IP has a later registration date
        4. Confirm that the disputed IP is not a derivative of the pre-existing IP

        <br />
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `IMPROPER_USAGE`

        Examples (non-exhaustive):\
        Territory,
        Channels of Distribution,
        Expiration,
        Irrevocable,
        Attribution,
        Derivatives,
        Limitations on Creation of Derivatives,
        Commercial Use,
        Sublicensable,
        Non-Transferable,
        Restriction on Cross-Platform Use
      </td>

      <td style={{ textAlign: "left" }}>
        A. PIL term that has been violated

        B. Description of the violation

        C. Proof of the violation
      </td>

      <td style={{ textAlign: "left" }}>
        1. Read the associated PIL term description on the PIL license official document using input A
        2. Read the violation description using input B
        3. Decide on the veracity of the proof presented by checking on associated platforms when possible using input C

        <br />
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `IMPROPER_PAYMENT`
      </td>

      <td style={{ textAlign: "left" }}>
        A. Description of each payment the disputed IP received that should have been shared with its royalty vault and/or its ancestors but it were not

        B. Proof of those payments that were not properly shared as royalties
      </td>

      <td style={{ textAlign: "left" }}>
        1. Check veracity of the proof of payments by checking on the associated platforms when possible using input A and B
        2. If proof of payments are deemed to be real, confirm that the payment has indeed not been made onchain by checking on the blockchain explorer. Payments should be made calling payRoyaltyOnBehalf() function on RoyaltyModule.sol smart contract. In addition, royalty payments must be made within 15 days of when the capital was originally received by the owner/IP who is paying those royalties.

        <br />
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `CONTENT_STANDARDS_VIOLATION`

        No-Hate,\
        Suitable-for-All-Ages,
        No-Drugs-or-Weapons,
        No-Pornography
      </td>

      <td style={{ textAlign: "left" }}>
        A. The content standard point that has been violated

        B. Description of the violation

        C. Proof of violation
      </td>

      <td style={{ textAlign: "left" }}>
        1. Read the associated content standards description on the official content standards section in the PIL using input A
        2. Read the violation description using input B
        3. Decide on the veracity of the proof presented by checking on associated platforms when possible using input C

        <br />
      </td>
    </tr>
  </tbody>
</Table>

# ❌ Dispute Module
The Dispute Module creates a way for users to raise and resolve disputes through arbitration.

<Cards columns={1}>
  <Card title="DisputeModule.sol" href="https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/modules/dispute/DisputeModule.sol" icon="fa-scroll" iconColor="#ccb092" target="_blank">
    View the smart contract for the Dispute Module.
  </Card>
</Cards>

## Dispute Terminology

The main components of the arbitration system are:

* **Arbitration Policies:** the arbitration policy refers to the set rules/process/entities that combined will decide on a dispute. Currently the only supported arbitration policy is the [UMA Arbitration Policy](doc:uma-arbitration-policy).
* **Arbitration Penalty:** what happens to an IP Asset after it has been "tagged". An IPA is not deemed "tagged" unless the dispute is decided to be correct. Once tagged, an IPA will not be able to:
  * mint licenses
  * link to any parents
  * claim royalties
  * and all of its existing licenses become unusable

### Dispute Tags

**Tags** refer to the "labels" that can be applied to IP Assets in the protocol when raising a dispute. **Tags must be whitelisted by protocol governance to be used in a dispute.** The initial set of tags are planned to be:

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        Dispute Tag
      </th>

      <th style={{ textAlign: "left" }}>
        Explanation
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        `IMPROPER_REGISTRATION`
      </td>

      <td style={{ textAlign: "left" }}>
        Refers to registration of IP that already exists.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `IMPROPER_USAGE`

        Examples (non-exhaustive):
        Territory,
        Channels of Distribution,
        Expiration,
        Irrevocable,
        Attribution,
        Derivatives,
        Limitations on Creation of Derivatives,
        Commercial Use,
        Sublicensable,
        Non-Transferable,
        Restriction on Cross-Platform Use
      </td>

      <td style={{ textAlign: "left" }}>
        Refers to improper use of an IP Asset across multiple items (examples on the left). These items can be found in more detail in the [💊 Programmable IP License (PIL)](doc:programmable-ip-license)   legal document.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `IMPROPER_PAYMENT`
      </td>

      <td style={{ textAlign: "left" }}>
        Refers to missing payments associated with an IP.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `CONTENT_STANDARDS_VIOLATION`

        No-Hate\
        Suitable-for-All-Ages
        No-Drugs-or-Weapons
        No-Pornography
      </td>

      <td style={{ textAlign: "left" }}>
        Refers to "No-Hate", "Suitable-for-All-Ages", "No-Drugs-or-Weapons" and "No-Pornography". These items can be found in more detail in the [💊 Programmable IP License (PIL)](doc:programmable-ip-license) legal document.
      </td>
    </tr>
  </tbody>
</Table>

## Dispute Process Flow

![](https://files.readme.io/a1dc371-image.png)

### Raise Dispute

The `raiseDispute` function is permissionless and allows any address to raise a dispute against any IP Asset registered on the protocol. The dispute initiator has to:

1. Select which "tag" it is raising a dispute on which will be applied to the IP Asset if the arbitration decision is positive. This means an IP Asset is officially "tagged" only when the proposed tag is confirmed as correct ("positive decision" in the diagram above).
2. Submit the dispute evidence for evaluation
3. Other conditions custom to each arbitration policy - such as payment rules, etc.

### Set Dispute Judgement

The `setDisputeJudgement` can only be called by whitelisted addresses and allows the caller to set the dispute judgment. Can only be called once as dispute decisions are immutable. If 3rd parties want to offer the possibility for recourse they can do so on their end and relay the final judgment.

### Tag Derivative If Parent Infringed

If the `setDisputeJudgement` has tagged an IP as infringing then any address can call `tagIfRelatedIpInfringed` to apply the same tag as the parent to the derivatives all the way down the derivative chain or if the IP is a group then the group member tag can be applied to any group IP which it is a member of.

> 📘 Looking Ahead
>
> In the future, the idea is that any related IP Asset of an infringing IP Asset would automatically be tagged without needing someone to call `tagIfRelatedIpInfringed`. This is currently a limitation that we are aware of.

The derivatives are then tagged directly without any need for judgment given that it is considered that if a parent IP license has been infringed then all derivatives that come from that license are also implicitly in an infringement situation.

**Example**: IPA 7 is first tagged ("PLAGIARISM") as infringing via `setDisputeJudgement` after having gone through a dispute process. Only after that can IPAs 3, 1, and 0 can be tagged via `tagIfRelatedIpInfringed` by any address without needing to go through a new dispute process.

![](https://files.readme.io/ee69754-image.png)

### Resolve Dispute

Resolving a dispute removes the tag from the IP Asset. Since there are two ways in which a tag can be applied, there are two ways for it to be resolved:

1. Tag was applied via the`setDisputeJudgement` function

In a case where a dispute judgment was positive, then a tag was applied. After the tag has been applied to an IP Asset, the **dispute initiator** can, if he/she believes the matter to be resolved and the tag to no longer apply, choose to remove it by calling `resolveDispute`. For example, if one party owed money to the dispute initiator and paid the full amount after the dispute judgment then the tag could be cleared and the IP Asset would have a clean slate again.

If the dispute initiator chooses to not resolve, then the tag that was defined in `setDisputeJudgement` remains in force.

2. Tag was applied via the`tagIfRelatedIpInfringed` function

If an IP has been previously tagged as infringing via `tagIfRelatedIpInfringed`, such tag can be removed via `resolveDispute` in a permissionless way as long as the parent is no longer considered an infringing IP Asset.

This mechanism of permissionless resolving disputes exists to make it easier to propagate down the derivative chain and remove infringement tags from derivative IPs when the parent has resolved its original dispute and is no longer considered as being in an infringing situation, and therefore neither are its derivatives.

If no address chooses to resolve, then the tag that was applied from the parent to the derivative remains in force.

### Cancel Dispute

In a case where a dispute was raised but the matter has been resolved before the dispute judgment, the dispute initiator can cancel the dispute. However, depending on the conditions of each arbitration policy, there may be non-refundable fees that are not recouped on cancellation.

# License Config / Hook
## License Config

<Cards columns={1}>
  <Card title="LicensingConfig Struct" href="https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/lib/Licensing.sol" icon="fa-scroll" iconColor="#ccb092" target="_blank">
    View the LicensingConfig struct in the smart contract.
  </Card>
</Cards>

Optionally, you can attach a `LicensingConfig` to an IP Asset (for a specific `licenseTermsId` attached to that asset) which contains fields like a `mintingFee` and a `licensingHook`, as shown below.

```sol Licensing.sol
/// @notice This struct is used by IP owners to define the configuration
/// when others are minting license tokens of their IP through the LicensingModule.
/// When the `mintLicenseTokens` function of LicensingModule is called, the LicensingModule will read
/// this configuration to determine the minting fee and execute the licensing hook if set.
/// IP owners can set these configurations for each License or set the configuration for the IP
/// so that the configuration applies to all licenses of the IP.
/// If both the license and IP have the configuration, then the license configuration takes precedence.
/// @param isSet Whether the configuration is set or not.
/// @param mintingFee The minting fee to be paid when minting license tokens.
/// @param licensingHook  The hook contract address for the licensing module, or address(0) if none
/// @param hookData The data to be used by the licensing hook.
/// @param commercialRevShare The commercial revenue share percentage.
/// @param disabled Whether the license is disabled or not.
/// @param expectMinimumGroupRewardShare The minimum percentage of the group’s reward share
/// (from 0 to 100%, represented as 100 * 10 ** 6) that can be allocated to the IP when it is added to the group.
/// If the remaining reward share in the group is less than the minimumGroupRewardShare,
/// the IP cannot be added to the group.
/// @param expectGroupRewardPool The address of the expected group reward pool.
/// The IP can only be added to a group with this specified reward pool address,
/// or address(0) if the IP does not want to be added to any group.
struct LicensingConfig {
  bool isSet;
  uint256 mintingFee;
  address licensingHook;
  bytes hookData;
  uint32 commercialRevShare;
  bool disabled;
  uint32 expectMinimumGroupRewardShare;
  address expectGroupRewardPool;
}
```

What do some of these mean?

1. `isSet` - if this is false, the license config is completely ignored
2. `disabled` - if this is true, then no licenses can be minted and no more derivatives can be attached at all

Fields like the `mintingFee` and `commercialRevShare` overwrite their duplicate in the license terms themselves. **A benefit of this is that derivative IP Assets, which normally cannot change their license terms, are able to overwrite certain fields.**

The `licensingHook` is an address to a smart contract that implements the `ILicensingHook.sol` interface, which contains a `beforeMintLicenseTokens` function which will be run before a user mints a License Token. This means you can insert logic to be run upon minting a license.

The hook itself is defined below in a different section. You can see it contains information about the license, who is minting the License Token, and who is receiving it.

### Setting the License Config

You can set the License Config by calling the `setLicenseConfig` function in the [LicensingModule.sol contract](https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/modules/licensing/LicensingModule.sol).

### Logic that is Possible with License Config

1. **Max Number of Licenses**: The `licensingHook` (described in the next section) is where you can define logic for the max number of licenses that can be minted. For example, reverting the transaction if the max number of licenses has already been minted.
2. **Disallowing Derivatives**: If you register a derivative of an IP Asset, that derivative cannot change its License Terms as described [here](https://docs.story.foundation/docs/license-terms#inherited-license-terms). You can be wondering: "What if I, as a derivative, want to disallow derivatives of myself, but my License Terms allow derivatives and I cannot change this?" To solve this, you can simply set `disabled` to true.
3. **Minting Fee**: Similar to #2 above... what about the minting fee? Although you cannot change License Terms on a derivative IP Asset (and thus the minting fee inside of it), you can change the minting fee for that derivative by modifying the `mintingFee` in the License Config, or returning a `totalMintingFee` from the `licensingHook` (described in the next section).
4. **Commercial Revenue Share**: Similar to #2 and #3 above, you can modify the `commercialRevShare` in the License Config.
5. **Dynamic Pricing for Minting a License Token**: Set dynamic pricing for minting a License Token from an IP Asset based on how many total have been minted, how many licenses the user is minting, or even who the user is. All of this data is available in the `licensingHook` (described in the next section).

... and more.

### Restrictions

See [IP Modifications & Restrictions](https://docs.story.foundation/docs/ipa-modifications) for the various restrictions on setting the License Config.

## Licensing Hook

<Cards columns={1}>
  <Card title="ILicensingHook.sol" href="https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/interfaces/modules/licensing/ILicensingHook.sol#L26" icon="fa-scroll" iconColor="#ccb092" target="_blank">
    View the smart contract for the Licensing Hook.
  </Card>
</Cards>

The `beforeMintLicenseTokens` function, which acts as a hook, is a function that can be called before a License Token is minted to implement custom logic and determine the final `totalMintingFee` of that License Token. The owner of an IP Asset must set the License Config (of which the hook is contained in), with their own implementation of the `beforeMintLicenseTokens` function, for this to be called.

It can also be used to implement various checks and logic, as [outlined above](https://docs.story.foundation/docs/license-config-hook#logic-that-is-possible-with-license-config).

> 🚧 Warning!
>
> Beware of potentially malicious implementations of external license hooks. Please first verify the code of the hook you choose because it may be not reviewed or audited by the Story team.

```sol ILicensingHook.sol
/// @notice This function is called when the LicensingModule mints license tokens.
/// @dev The hook can be used to implement various checks and determine the minting price.
/// The hook should revert if the minting is not allowed.
/// @param caller The address of the caller who calling the mintLicenseTokens() function.
/// @param licensorIpId The ID of licensor IP from which issue the license tokens.
/// @param licenseTemplate The address of the license template.
/// @param licenseTermsId The ID of the license terms within the license template,
/// which is used to mint license tokens.
/// @param amount The amount of license tokens to mint.
/// @param receiver The address of the receiver who receive the license tokens.
/// @param hookData The data to be used by the licensing hook.
/// @return totalMintingFee The total minting fee to be paid when minting amount of license tokens.
function beforeMintLicenseTokens(
  address caller,
  address licensorIpId,
  address licenseTemplate,
  uint256 licenseTermsId,
  uint256 amount,
  address receiver,
  bytes calldata hookData
) external returns (uint256 totalMintingFee);
```

Note that it returns the `totalMintingFee`. You may be wondering, "I can set the minting fee in the License Terms, in the `LicenseConfig`, and return a dynamic price from `beforeMintLicenseTokens`. What will the final minting fee actually be?" Here is the priority:

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        Minting Fee
      </th>

      <th style={{ textAlign: "left" }}>
        Importance
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        The `totalMintingFee` returned from `beforeMintLicenseTokens`
      </td>

      <td style={{ textAlign: "left" }}>
        Highest Priority
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        The `mintingFee` set in the `LicenseConfig`
      </td>

      <td style={{ textAlign: "left" }}>
        :arrow_down:
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        The `mintingFee` set in the License Terms
      </td>

      <td style={{ textAlign: "left" }}>
        Lowest Priority
      </td>
    </tr>
  </tbody>
</Table>

# License Token
<Cards columns={1}>
  <Card title="LicenseToken.sol" href="https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/LicenseToken.sol" icon="fa-scroll" iconColor="#ccb092" target="_blank">
    View the smart contract for License Tokens.
  </Card>
</Cards>

A **License Token** is represented as an **ERC-721 NFT** and contains the specific [License Terms](doc:license-terms) it represents. Its associated `licenseTokenId` is global, as there is one License Token contract.

Once License Terms are attached to an IP Asset, it becomes public so that anyone can mint a License Token for those terms. A License Token is burned when it is used to register another IP as a derivative of the original IP Asset.

<Image align="center" alt="A diagram showing what happens when a License Token is minted." border={false} caption="A diagram showing what happens when a License Token is minted." src="https://files.readme.io/2c2938f-Screenshot_2024-05-07_at_18.42.00.png" />

## Private Licenses

In order to mint a private License Token, the owner of a root IP Asset can issue License Tokens that have terms **not yet attached to the IP Asset itself**. It is important to also note that derivative IP Assets cannot issue private licenses because it is restricted to only issue licenses of its inherited terms.

## Transferability of the License Token

License Tokens might be transferrable or not, depending on the values of the License Terms terms they point to.

Once a non-transferable License Token is minted to a recipient, it is locked there forever.

## Registering a Derivative

There are two ways to register a derivative IP Asset.

> 📘 Small Note
>
> An IP Asset can only register as a derivative one time. If an IP Asset has multiple parents, it must register both at the same time. Once an IP Asset is a derivative, it cannot link any more parents.

### 1. Using an Existing License Token

A License Token is burned when it is used to register another IP as a derivative of the original IP Asset.

<Image align="center" src="https://files.readme.io/9bc3615-image.png" />

### 2. Registering a Derivative Directly

You can also register a derivative directly, without the need for a License Token. Remember that if License Terms are attached to an IP Asset it is public to mint the License Token anyway, so this is simply a convenient way to go about it, thus skipping the middle step of minting a License Token.

<Image align="center" src="https://files.readme.io/02181c4-Screenshot_2024-05-07_at_18.51.15.png" />

# License Template
A License Template is a legal framework, written in code ("programmable"), that defines various licensing terms for an IP. Such as:

* "Is commercial use allowed?" - true/false (bool)
* "Is the license transferrable?" - true/false (bool)
* "If commercial, what % of royalty do I receive?" - number

These terms and values differ per License Template.

The first (and currently only) example of a License Template was developed by the Story team directly, and is called the Programmable IP License (PIL :pill:).

<Cards columns={2}>
  <Card title="Programmable IP License (PIL)" href="https://docs.story.foundation/docs/programmable-ip-license" icon="fa-pills" iconColor="yellow">
    Learn about the first implementation of a License Template
  </Card>

  <Card title="PIL Smart Contract" href="https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/modules/licensing/PILicenseTemplate.sol" icon="fa-scroll" iconColor="#ccb092" target="_blank">
    View the smart contract for the PIL.
  </Card>
</Cards>

## License Template Requirements

License Templates are responsible for:

* Providing a link to the actual, off-chain, legal contract template, with all the parameters, their possible values, and the correspondent legalese, in `licenseTextUrl`.
  * For a licensing framework to be compatible with Story Protocol, the legal text **must** be clear and parametrized, with each licensing parameter establishing the possible outcomes of each value.
  * The parameter values in each License Template (called "License Template terms") drive the legal text for each license agreement.
* Defining a `struct` with the particular definitions of the parameters in accordance, which must be encoded into the License Terms struct (described below).
* Providing registration methods for the License Terms, and getters.
* **Verifying** that both the **minter** and the address **linking a derivative are allowed by the License Template terms to perform those actions**.
  * These conditions could be enforced by the License Template itself or through hooks. They can range from limitations on the derivative creations, token-gating LNFT holders, creative control from licensors, KYC, etc. It's up to the implementation of each License Template.
* **Verifying that the License Terms are compatible if a derivative has or will have multiple parents**

## Create Your Own Template

You can create your own License Template (like the PIL), but it must be approved by the Story team to be fully embedded into the protocol.

# License Terms
When registering your IP on Story, you can attach License Terms to the IP. These are real, legally binding terms enforced on-chain by the [📜 Licensing Module](doc:licensing-module), disputable by the [❌ Dispute Module](doc:dispute-module), and in the worst case, able to be enforced off-chain in court through traditional means.

In them are also terms for commercial usage, which describes how the [💸 Royalty Module](doc:royalty-module) will be enforced (ex. "50% of revenue must be shared with the parent IP").

<Cards columns={1}>
  <Card title="Example License Terms" href="https://docs.story.foundation/docs/pil-flavors" icon="fa-ice-cream" iconColor="white" target="_blank">
    View some popular combinations of PIL License Terms, also known as "flavors".
  </Card>
</Cards>

More specifically, License Terms are a particular combination of values from a [License Template](doc:license-template). Indeed, there can and will exist **multiple** License Terms (variations) for each License Template. You can imagine that a License Template generates many License Term variations.

<Image align="center" src="https://files.readme.io/62ee532-Screenshot_2024-05-07_at_17.59.18.png" />

Once registered, **License Terms are immutable — they can't be tampered with or altered**, even by the License Template that generated it.

Additionally, License Terms have a unique numeric ID within the License Template they stem from. This makes License Terms reusable, meaning if someone creates License Terms with a specific set of values, it only needs to be created once and can be used by anyone else.

For example, a particular set of term values of the [Programmable IP License (PIL💊)](doc:programmable-ip-license-pil), such as non-commercial usage + derivatives allowed + free minting, defines a unique License Terms with an associated ID.

## License Terms Attached to IP Asset

The owner of a root IP Asset can attach License Terms to signal to other users that they can mint License Tokens of those terms to create a derivative of this IP Asset. **Once License Terms are attached to an IP Asset, it is now considered "public" and anyone can mint a License Token using those terms.**

<Image align="center" src="https://files.readme.io/39a365f-Screenshot_2024-05-07_at_18.43.38.png" />

## Inherited License Terms

On the other hand, derivative IP Assets inherit their License Terms from the parent IP Asset. This means that when an IP Asset registers itself as a derivative, it burns the License Token and inherits the associated License Terms. **The owner of this derivative cannot set new License Terms.**

> 📘 Changing Certain License Terms on a Derivative
>
> You may be wondering: "if I cannot set new License Terms on my derivative, does that also mean I can't change the minting fee, or disallowing more derivatives, on my derivative?"
>
> Thankfully, there is a way to get around this! Although you cannot change License Terms on a derivative IP, you can utilize the [License Config to implement special behaviors](doc:license-config-hook).

## Expiration

License Terms support an `expiration` time. Once License Terms expire, any derivatives that abide by that license will no longer be able to generate revenue or create further derivatives. If an IP Asset is a derivative of multiple parents, it will expire when the soonest expiration time between the two parents is reached.

# 📜 Licensing Module
<Accordion title="Skip the Read - 1 Minute Summary" icon="fa-info-circle">
  The Licensing Module allows you to create a real legal license from a **License Template** (which is the [Programmable IP License (PIL💊)](doc:programmable-ip-license)) and attach it to your IP Asset. This license, and the **License Terms** that define it, restrict how others can use your IP, commercialize it, and remix it.

  If License Terms are attached to an IP Asset, anyone can mint a **License Token** (an ERC-721 NFT) from it which acts as the license to use that work based on the terms that define it. This token can then be burned to register a derivative work. This then establishes a parent-child relationship between assets, unlocking things like automatic royalty flow from the [💸 Royalty Module](doc:royalty-module).
</Accordion>

The owner of an IP Asset owns intellectual property rights such as creating derivatives, being commercially exploited, and being reproduced in different platforms.

IP Assets can programmatically grant permissions for any users to exercise those rights with some autonomy via [License Tokens](doc:license-token) (an ERC-721 NFT), which point to a particular set of conditions, known as [License Terms](doc:license-terms).

<Image align="center" alt="The contracts in blue are built into the protocol. The contracts in white can be developed by the community or 3rd party vendor. " border={false} caption="Blue: contracts built into the protocol. White: contracts developed by the community or 3rd party vendor." src="https://files.readme.io/3be1037-Screenshot_2024-05-07_at_17.52.53.png" />

## LicensingModule

<Cards columns={1}>
  <Card title="LicensingModule.sol" href="https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/modules/licensing/LicensingModule.sol" icon="fa-scroll" iconColor="#ccb092" target="_blank">
    View the smart contract for the License Module.
  </Card>
</Cards>

The `LicensingModule.sol` contract is the main entry point for the licensing system. It is responsible for:

* Attaching License Terms to IP Assets
* Minting License Tokens
* Registering derivatives
* Setting License Configs

## Further Readings

The following document will walk through all of the major components of the Licensing Module as shown above:

* [License Template](doc:license-template)
* [License Terms](doc:license-terms)
* [License Token](doc:license-token)
* [License Registry](doc:license-registry)
* [License Config / Hook](doc:license-config-hook)

# 🔒 Access Controller
<Cards columns={1}>
  <Card title="AccessController.sol" href="https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/access/AccessController.sol" icon="fa-scroll" iconColor="#ccb092" target="_blank">
    View the smart contract for the Access Controller.
  </Card>
</Cards>

Access Controller manages all permission-related states and permission checks in Story Protocol. In particular, it maintains the *Permission Table* and *Permission Engine* to process and store permissions. IPAccount permissions are set by the IPAccount owner.

<Image align="center" src="https://files.readme.io/ff607ff-Screenshot_2024-01-23_at_14.30.19.png" />

## Permission Table

### Permission Record

| IPAccount  | Signer (caller) | To (only module) | Function Sig | Permission |
| ---------- | --------------- | ---------------- | ------------ | ---------- |
| 0x123..111 | 0x789..222      | 0x790..333       | 0xAaAaAaAa   | Allow      |
| 0x123..111 | 0x789..222      | 0x790..333       | 0xBBBBBBBB   | Deny       |
| 0x123..111 | 0x789..222      | 0x790..333       | 0xCCCCCC     | Abstain    |

Each record defines a permission in the form of the **Signer** (caller) calling the **Func** of the **To** (module) on behalf of the **IPAccount**.

The permission field can be set as "Allow," "Deny," or "Abstain." Abstain indicates that the permission decision is determined by the upper-level permission.

### Wildcard

Wildcard is also supported when defining permissions; it defines a permission that applies to multiple modules and/or functions.

With wildcards, users can easily define a whitelist or blacklist of permissions.

| IPAccount  | Signer (caller) | To (module) | Func | Permission |
| :--------- | :-------------- | :---------- | :--- | :--------- |
| 0x123..111 | 0x789..222      | \*          | \*   | Allow      |
| 0x123..111 | 0x789..222      | 0x790..333  | \*   | Deny       |

The above example shows that the signer (0x789...) is unable to invoke any functions of the module (0x790...) on behalf of the IPAccount (0x123...).

In other words, the IPAccount has blacklisted the signer from calling any functions on the module 0x790...333

* Supported wildcards:

| Parameter                  | Wildcard   |
| -------------------------- | ---------- |
| Func                       | bytes4(0)  |
| Addresses (IPAccount / To) | address(0) |

### Permission Prioritization

Specific permissions override general permissions.

| IPAccount  | Signer (caller) | To (module) | Func       | Permission |
| :--------- | :-------------- | :---------- | :--------- | :--------- |
| 0x123..111 | 0x789..222      | \*          | \*         | Allow      |
| 0x123..111 | 0x789..222      | 0x790..333  | \*         | Deny       |
| 0x123..111 | 0x789..222      | 0x790..333  | 0xCCCCDDDD | Allow      |

The above shows that the signer (0x789...) is not allowed to call any functions of the module (0x790...) on behalf of IPAccount (0x123...), except for the function 0xCCCCDDDD

Furthermore, the signer (0x789...) is permitted to call all other modules on behalf of IPAccount (0x123...).

<br />

## Call Flows with Access Control

There exist three types of call flows expected by the Access Controller.

1. An IPAccount calls a module directly.
2. A module calls another module directly.
3. A module calls a registry directly.

### IPAccount calling a Module directly

* IPAccount performs a permission check with the Access Controller.
* The module only needs to check if the `msg.sender` is a valid IPAccount.

When calling a module from an IPAccount, the IPAccount performs an access control check with AccessController to determine if the current caller has permission to make the call. In the module, it only needs to check whether the transaction `msg.sender` is a valid IPAccount.

`AccessControlled` provide a modifier `onlyIpAccount()` helps to perform the access control check.

```solidity Solidity
contract MockModule is IModule, AccessControlled {
    function action(string memory param) external view onlyIpAccount() returns (string memory) {
            // do something
    }
}
```

<Image align="center" src="https://files.readme.io/6a835ae-Screenshot_2024-01-22_at_17.18.49.png" />

## Module calling another Module

* The callee module needs to perform the authorization check itself.

When a module is called directly from another module, it is responsible for performing the access control check using AccessController. This check determines whether the current caller has permission to make the call to the module.

`AccessControlled` provide a modifier `verifyPermission(address ipAccount)` helps to perform the access control check.

```coffeescript Solidity
contract MockModule is IModule, AccessControlled {
    function callFromAnotherModule(address ipAccount) external verifyPermission(ipAccount) returns (string memory) {
        if (!IAccessController(accessController).checkPermission(ipAccount, msg.sender, address(this), this.callFromAnotherModule.selector)) {
		        revert Unauthorized();
        }
			  // do something
    }
}
```

<Image align="center" src="https://files.readme.io/767f852-Screenshot_2024-01-22_at_17.19.07.png" />

## Module calling Registry

* The registry performs the authorization check by calling AccessController.
* The registry authorizes modules through set global permission

When a registry is called by a module, it can perform the access control check using AccessController. This check determines whether the callee module has permission to call the registry.

```solidity Solidity
// called by StoryProtocl Admin
IAccessController(accessController).setGlobalPermission(address(0), address(module), address(registry), bytes4(0))) {

```

```solidity Solidity
contract MockRegistry {
    function registerAction() external returns (string memory) {
        if (!IAccessController(accessController).checkPermission(address(0), msg.sender, address(this), this.registerAction.selector)) {
		        revert Unauthorized();
        }
			  // do something
    }
}
```

<Image align="center" src="https://files.readme.io/3d24a42-Screenshot_2024-01-24_at_09.45.06.png" />

> 📘 The IPAccount's permissions will be revoked upon transfer of ownership.
>
> The permissions associated with the IPAccount are exclusively linked to its current owner. When the ownership of the IPAccount is transferred to a new individual, the existing permissions granted to the previous owner are automatically revoked. This ensures that only the current, legitimate owner has access to these permissions. If, in the future, the IPAccount ownership is transferred back to the original owner, the permissions that were initially revoked will be reinstated, restoring the original owner's access and control.

# IP Asset
## IPAssetClient

### Methods

* register
* registerDerivative
* registerDerivativeWithLicenseTokens
* mintAndRegisterIpAssetWithPilTerms
* registerIpAndAttachPilTerms
* registerDerivativeIp
* mintAndRegisterIpAndMakeDerivative
* mintAndRegisterIp
* registerPilTermsAndAttach
* mintAndRegisterIpAndMakeDerivativeWithLicenseTokens
* registerIpAndMakeDerivativeWithLicenseTokens

### Navigating Around the IPAssetClient

Because there are a lot of functions to interact with the [📜 Licensing Module](doc:licensing-module), we have broken them down into a helpful chart so you can identify what you're looking for, and then find the associated docs.

| **Function**                                                                                | **Mint an NFT** | **Register IPA** | **Create License Terms** | **Attach License Terms** | **Mint License Token** | **Register as Derivative** |
| ------------------------------------------------------------------------------------------- | :-------------: | :--------------: | :----------------------: | :----------------------: | :--------------------: | :------------------------: |
| <span style={{color: "#e03130"}}>register</span>                                            |                 |         ✓        |                          |                          |                        |                            |
| <span style={{color: "#e03130"}}>mintAndRegisterIp</span>                                   |        ✓        |         ✓        |                          |                          |                        |                            |
| <span style={{color: "#e03130"}}>registerIpAndAttachPilTerms</span>                         |                 |         ✓        |             ✓            |             ✓            |                        |                            |
| <span style={{color: "#e03130"}}>mintAndRegisterIpAssetWithPilTerms</span>                  |        ✓        |         ✓        |             ✓            |             ✓            |                        |                            |
| <span style={{color: "#e03130"}}>registerDerivativeIp</span>                                |                 |         ✓        |                          |                          |                        |              ✓             |
| <span style={{color: "#e03130"}}>mintAndRegisterIpAndMakeDerivativeWithLicenseTokens</span> |        ✓        |         ✓        |                          |                          |                        |              ✓             |
| <span style={{color: "#e03130"}}>registerIpAndMakeDerivativeWithLicenseTokens</span>        |                 |         ✓        |                          |                          |                        |              ✓             |
| <span style={{color: "#e03130"}}>mintAndRegisterIpAndMakeDerivative</span>                  |        ✓        |         ✓        |                          |                          |                        |              ✓             |
| <span style={{color: "#e03130"}}>registerDerivative</span>                                  |                 |                  |                          |                          |                        |              ✓             |
| <span style={{color: "#e03130"}}>registerDerivativeWithLicenseTokens</span>                 |                 |                  |                          |                          |                        |              ✓             |
| <span style={{color: "#e03130"}}>registerPilTermsAndAttach</span>                           |                 |                  |             ✓            |             ✓            |                        |                            |
| <span style={{color: "#1971c2"}}>registerPILTerms</span>                                    |                 |                  |             ✓            |                          |                        |                            |
| <span style={{color: "#1971c2"}}>attachLicenseTerms</span>                                  |                 |                  |                          |             ✓            |                        |                            |
| <span style={{color: "#1971c2"}}>mintLicenseTokens</span>                                   |                 |                  |                          |                          |            ✓           |                            |

* <span style={{color: "#e03130"}}>Red</span>: IPAssetClient (this page)
* <span style={{color: "#1971c2"}}>Blue</span>: [LicenseClient](doc:sdk-license)

## register

Registers an NFT as IP, creating a corresponding [🧩 IP Asset](doc:ip-asset). If the given NFT was already registered, this function will return the existing `ipId`.

> 📘 NFT Metadata
>
> Note that this function will also set the underlying NFT's `tokenUri` to whatever is passed under `ipMetadata.nftMetadataURI`.

| Method     | Type                                                        |
| ---------- | ----------------------------------------------------------- |
| `register` | `(request: RegisterRequest) => Promise<RegisterIpResponse>` |

Parameters:

* `request.nftContract`: The address of the NFT.
* `request.tokenId`: The token identifier of the NFT.
* `request.ipMetadata`: \[Optional] The desired metadata for the newly minted NFT and newly registered IP.
  * `request.ipMetadata.ipMetadataURI` \[Optional] The URI of the metadata for the IP.
  * `request.ipMetadata.ipMetadataHash` \[Optional] The hash of the metadata for the IP.
  * `request.ipMetadata.nftMetadataURI` \[Optional] The URI of the metadata for the NFT.
  * `request.ipMetadata.nftMetadataHash` \[Optional] The hash of the metadata for the IP NFT.
* `request.deadline`: \[Optional]The deadline for the signature in milliseconds.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript TypeScript
import { toHex } from 'viem';

const response = await client.ipAsset.register({
  nftContract: "0x041B4F29183317Fd352AE57e331154b73F8a1D73",
  tokenId: "12",
  ipMetadata: {
    ipMetadataURI: 'test-uri',
    ipMetadataHash: toHex('test-metadata-hash', { size: 32 }),
    nftMetadataHash: toHex('test-nft-metadata-hash', { size: 32 }),
    nftMetadataURI: 'test-nft-uri',
  },
  txOptions: { waitForTransaction: true }
});

console.log(`Root IPA created at transaction hash ${response.txHash}, IPA ID: ${response.ipId}`)
```
```typescript Request Type
export type RegisterRequest = {
  nftContract: Address;
  tokenId: string | number | bigint;
  deadline?: string | number | bigint;
} & IpMetadataAndTxOptions;
```
```typescript Response Type
export type RegisterIpResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  ipId?: Address;
};
```

## batchRegister

Batch registers an NFT as IP, creating a corresponding IP record.

| Method          | Type                                                                |
| --------------- | ------------------------------------------------------------------- |
| `batchRegister` | `(request: BatchRegisterRequest) => Promise<BatchRegisterResponse>` |

## registerDerivative

Registers a derivative directly with parent IP's license terms, without needing license tokens, and attaches the license terms of the parent IPs to the derivative IP.

The license terms must be attached to the parent IP before calling this function.

All IPs attached default license terms by default.

The derivative IP owner must be the caller or an authorized operator.

| Method               | Type                                                                          |
| -------------------- | ----------------------------------------------------------------------------- |
| `registerDerivative` | `(request: RegisterDerivativeRequest) => Promise<RegisterDerivativeResponse>` |

Parameters:

* `request.childIpId`: The derivative IP ID.
* `request.parentIpIds`: The parent IP IDs.
* `request.licenseTermsIds`: The IDs of the license terms that the parent IP supports.
* `request.maxMintingFee`: The maximum minting fee that the caller is willing to pay. If set to 0, then there is no no limit. **Recommended for simplicity: 0**
* `request.maxRevenueShare`: The maximum revenue share percentage agreed upon between a child and parent when a child is registering as derivative. Must be between 0 and 100. **Recommended for simplicity: 100**
* `request.maxRts`: The maximum number of royalty tokens that can be distributed to the external royalty policies. Must be between 0 and 100,000,000. **Recommended for simplicity: 100\_000\_000**
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript TypeScript
const response = await client.ipAsset.registerDerivative({
  childIpId: "0xC92EC2f4c86458AFee7DD9EB5d8c57920BfCD0Ba",
  parentIpIds: ["0xC92EC2f4c86458AFee7DD9EB5d8c57920BfCD0Ba"],
  licenseTermsIds: ["5"],
  maxMintingFee: BigInt(0), // disabled
  maxRts: 100_000_000, // default
  maxRevenueShare: 100, // default
  txOptions: { waitForTransaction: true }
});

console.log(`Derivative IPA linked to parent at transaction hash ${response.txHash}`)
```
```typescript Request Type
export type RegisterDerivativeRequest = {
  txOptions?: TxOptions;
  childIpId: Address;
} & DerivativeData;

export type DerivativeData = {
  parentIpIds: Address[];
  licenseTermsIds: bigint[] | string[] | number[];
  maxMintingFee: bigint | string | number;
  maxRts: number | string;
  maxRevenueShare: number | string;
  licenseTemplate?: Address;
};
```
```typescript Response Type
export type RegisterDerivativeResponse = {
  txHash?: Hex;
  encodedTxData?: EncodedTxData;
};
```

## registerDerivativeWithLicenseTokens

Registers a derivative with license tokens.

The derivative IP is registered with license tokens minted from the parent IP's license terms.

The license terms of the parent IPs issued with license tokens are attached to the derivative IP.

The caller must be the derivative IP owner or an authorized operator.

| Method                                | Type                                                                                                            |
| ------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| `registerDerivativeWithLicenseTokens` | `(request: RegisterDerivativeWithLicenseTokensRequest) => Promise<RegisterDerivativeWithLicenseTokensResponse>` |

Parameters:

* `request.childIpId`: The derivative IP ID.
* `request.licenseTokenIds`: The IDs of the license tokens.
* `request.maxRts`: The maximum number of royalty tokens that can be distributed to the external royalty policies. Must be between 0 and 100,000,000. **Recommended for simplicity: 100\_000\_000**
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript TypeScript
const response = await client.ipAsset.registerDerivativeWithLicenseTokens({
  childIpId: "0xC92EC2f4c86458AFee7DD9EB5d8c57920BfCD0Ba",
  licenseTokenIds: ["5"], // array of license ids relevant to the creation of the derivative, minted from the parent IPA
  maxRts: 100_000_000, // default
  txOptions: { waitForTransaction: true }
});

console.log(`Derivative IPA linked to parent at transaction hash ${response.txHash}`)
```
```typescript Request Type
export type RegisterDerivativeWithLicenseTokensRequest = {
  childIpId: Address;
  licenseTokenIds: string[] | bigint[] | number[];
  maxRts: number | string;
  txOptions?: TxOptions;
};
```
```typescript Response Type
export type RegisterDerivativeWithLicenseTokensResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
};
```

## mintAndRegisterIpAssetWithPilTerms

Mint an NFT from a collection, register it as an IP, attach metadata to the IP, and attach License Terms to the IP all in one function.

> 📘 NFT Metadata
>
> Note that this function will also set the underlying NFT's `tokenUri` to whatever is passed under `ipMetadata.nftMetadataURI`.

| Method                               | Type                                                                                                          |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------- |
| `mintAndRegisterIpAssetWithPilTerms` | `(request: MintAndRegisterIpAssetWithPilTermsRequest) => Promise<MintAndRegisterIpAssetWithPilTermsResponse>` |

Parameters:

* `request.spgNftContract`: The address of the NFT collection.
* `request.allowDuplicates`: Set to true to allow minting IPs with the same NFT metadata.
* `request.licenseTermsData[]`: The array of license terms to be attached. :warning: **This function will fail if you pass in an empty array.**
  * `request.licenseTermsData.terms`: See the [LicenseTerms type](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/resources/license.ts#L26).
  * `request.licenseTermsData.licensingConfig`: See the [LicensingConfig type](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/common.ts#L15).
* `request.ipMetadata`: \[Optional] The desired metadata for the newly minted NFT and newly registered IP.
  * `request.ipMetadata.ipMetadataURI`: \[Optional] The URI of the metadata for the IP.
  * `request.ipMetadata.ipMetadataHash`: \[Optional] The hash of the metadata for the IP.
  * `request.ipMetadata.nftMetadataURI`: \[Optional] The URI of the metadata for the NFT.
  * `request.ipMetadata.nftMetadataHash`: \[Optional] The hash of the metadata for the IP NFT.
* `request.recipient`: \[Optional] The address of the recipient of the minted NFT.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript TypeScript
import { LicenseTerms } from '@story-protocol/core-sdk';

const commercialRemixTerms: LicenseTerms = {
  transferable: true,
  royaltyPolicy: RoyaltyPolicyLAP, // insert RoyaltyPolicyLAP address from https://docs.story.foundation/docs/deployed-smart-contracts
  defaultMintingFee: BigInt(0),
  expiration: BigInt(0),
  commercialUse: true,
  commercialAttribution: true,
  commercializerChecker: zeroAddress,
  commercializerCheckerData: zeroAddress,
  commercialRevShare: 50, // can claim 50% of derivative revenue
  commercialRevCeiling: BigInt(0),
  derivativesAllowed: true,
  derivativesAttribution: true,
  derivativesApproval: false,
  derivativesReciprocal: true,
  derivativeRevCeiling: BigInt(0),
  currency: '0x1514000000000000000000000000000000000000', // insert $WIP address from https://docs.story.foundation/docs/deployed-smart-contracts
  uri: '',
}

const licensingConfig: LicensingConfig = {
  isSet: false,
  mintingFee: BigInt(0),
  licensingHook: zeroAddress,
  hookData: zeroHash,
  commercialRevShare: 0,
  disabled: false,
  expectMinimumGroupRewardShare: 0,
  expectGroupRewardPool: zeroAddress,
};

const response = await client.ipAsset.mintAndRegisterIpAssetWithPilTerms({
  spgNftContract: '0xfE265a91dBe911db06999019228a678b86C04959',
  licenseTermsData: [{ terms: commercialRemixTerms, licensingConfig }], // IP already has non-commercial social remixing terms. You can add more here.
  // set to true to mint ip with same nft metadata
  allowDuplicates: true,
  // https://docs.story.foundation/docs/ip-asset#adding-nft--ip-metadata-to-ip-asset
  ipMetadata: {
    ipMetadataURI: 'test-uri',
    ipMetadataHash: toHex('test-metadata-hash', { size: 32 }),
    nftMetadataHash: toHex('test-nft-metadata-hash', { size: 32 }),
    nftMetadataURI: 'test-nft-uri',
  },
  txOptions: { waitForTransaction: true },
})

console.log(`
  Token ID: ${response.tokenId}, 
  IPA ID: ${response.ipId}, 
  License Terms ID: ${response.licenseTermsId}
`)
```
```typescript Request Type
export type MintAndRegisterIpAssetWithPilTermsRequest = {
  spgNftContract: Address;
  allowDuplicates: boolean;
  licenseTermsData: LicenseTermsData<RegisterPILTermsRequest, LicensingConfig>[];
  recipient?: Address;
  royaltyPolicyAddress?: Address;
} & IpMetadataAndTxOptions & WithWipOptions;
```
```typescript Response Type
export type MintAndRegisterIpAssetWithPilTermsResponse = {
  txHash?: Hex;
  encodedTxData?: EncodedTxData;
  ipId?: Address;
  tokenId?: bigint;
  receipt?: TransactionReceipt;
  licenseTermsIds?: bigint[];
};
```

## batchMintAndRegisterIpAssetWithPilTerms

Batch mint an NFT from a collection and register it as an IP.

| Method                                    | Type                                                                                                                    |
| ----------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| `batchMintAndRegisterIpAssetWithPilTerms` | `(request: BatchMintAndRegisterIpAssetWithPilTermsRequest) => Promise<BatchMintAndRegisterIpAssetWithPilTermsResponse>` |

## registerIpAndAttachPilTerms

Register a given NFT as an IP, attach metadata to the IP, and attach License Terms to the IP all in one function.

> 📘 NFT Metadata
>
> Note that this function will also set the underlying NFT's `tokenUri` to whatever is passed under `ipMetadata.nftMetadataURI`.

| Method                        | Type                                                                                            |
| ----------------------------- | ----------------------------------------------------------------------------------------------- |
| `registerIpAndAttachPilTerms` | `(request: RegisterIpAndAttachPilTermsRequest) => Promise<RegisterIpAndAttachPilTermsResponse>` |

Parameters:

* `request.nftContract`: The address of the NFT collection.
* `request.tokenId`:  The ID of the NFT.
* `request.licenseTermsData[]`: The array of license terms to be attached. :warning: **This function will fail if you pass in an empty array.**
  * `request.licenseTermsData.terms`: See the [LicenseTerms type](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/resources/license.ts#L26).
  * `request.licenseTermsData.licensingConfig`: See the [LicensingConfig type](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/common.ts#L15).
* `request.ipMetadata`: \[Optional] The desired metadata for the newly minted NFT and newly registered IP.
  * `request.ipMetadata.ipMetadataURI`: \[Optional] The URI of the metadata for the IP.
  * `request.ipMetadata.ipMetadataHash`: \[Optional] The hash of the metadata for the IP.
  * `request.ipMetadata.nftMetadataURI`: \[Optional] The URI of the metadata for the NFT.
  * `request.ipMetadata.nftMetadataHash`: \[Optional] The hash of the metadata for the IP NFT.
* `request.deadline`: \[Optional]The deadline for the signature in milliseconds.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript TypeScript
import { LicenseTerms } from '@story-protocol/core-sdk';
import { toHex } from 'viem';

const commercialRemixTerms: LicenseTerms = {
  transferable: true,
  royaltyPolicy: RoyaltyPolicyLAP, // insert RoyaltyPolicyLAP address from https://docs.story.foundation/docs/deployed-smart-contracts
  defaultMintingFee: BigInt(0),
  expiration: BigInt(0),
  commercialUse: true,
  commercialAttribution: true,
  commercializerChecker: zeroAddress,
  commercializerCheckerData: zeroAddress,
  commercialRevShare: 50, // can claim 50% of derivative revenue
  commercialRevCeiling: BigInt(0),
  derivativesAllowed: true,
  derivativesAttribution: true,
  derivativesApproval: false,
  derivativesReciprocal: true,
  derivativeRevCeiling: BigInt(0),
  currency: '0x1514000000000000000000000000000000000000', // insert $WIP address from https://docs.story.foundation/docs/deployed-smart-contracts
  uri: '',
}

const licensingConfig: LicensingConfig = {
  isSet: false,
  mintingFee: BigInt(0),
  licensingHook: zeroAddress,
  hookData: zeroHash,
  commercialRevShare: 0,
  disabled: false,
  expectMinimumGroupRewardShare: 0,
  expectGroupRewardPool: zeroAddress,
};

const response = await client.ipAsset.registerIpAndAttachPilTerms({
  nftContract: '0x041B4F29183317Fd352AE57e331154b73F8a1D73',
  tokenId: '12',
  licenseTermsData: [{ terms: commercialRemixTerms, licensingConfig }], // IP already has non-commercial social remixing terms. You can add more here.
  // https://docs.story.foundation/docs/ip-asset#adding-nft--ip-metadata-to-ip-asset
  ipMetadata: {
    ipMetadataURI: 'test-uri',
    ipMetadataHash: toHex('test-metadata-hash', { size: 32 }),
    nftMetadataHash: toHex('test-nft-metadata-hash', { size: 32 }),
    nftMetadataURI: 'test-nft-uri',
  },
  txOptions: { waitForTransaction: true },
})
console.log(`Root IPA created at transaction hash ${response.txHash}, IPA ID: ${response.ipId}`)
```
```typescript Request Type
export type RegisterIpAndAttachPilTermsRequest = {
  nftContract: Address;
  tokenId: bigint | string | number;
  licenseTermsData: LicenseTermsData<RegisterPILTermsRequest, LicensingConfig>[];
  deadline?: bigint | number | string;
} & IpMetadataAndTxOptions;
```
```typescript Response Type
export type RegisterIpAndAttachPilTermsResponse = {
  txHash?: Hex;
  encodedTxData?: EncodedTxData;
  ipId?: Address;
  licenseTermsIds?: bigint[];
  tokenId?: bigint;
};
```

## registerDerivativeIp

Register an NFT as IP and then link it as a derivative of another IP Asset without using license tokens.

> 📘 NFT Metadata
>
> Note that this function will also set the underlying NFT's `tokenUri` to whatever is passed under `ipMetadata.nftMetadataURI`.

| Method                 | Type                                                                                            |
| ---------------------- | ----------------------------------------------------------------------------------------------- |
| `registerDerivativeIp` | `(request: RegisterIpAndMakeDerivativeRequest) => Promise<RegisterIpAndMakeDerivativeResponse>` |

Parameters:

* `request.nftContract`: The address of the NFT collection.
* `request.tokenId`:  The ID of the NFT.
* `request.derivData`: The derivative data to be used for registerDerivative.
  * `request.derivData.parentIpIds`: The IDs of the parent IPs to link the registered derivative IP.
  * `request.derivData.licenseTermsIds`: The IDs of the license terms to be used for the linking.
  * `request.derivData.maxMintingFee`: The maximum minting fee that the caller is willing to pay. If set to 0, then there is no no limit. **Recommended for simplicity: 0**
  * `request.derivData.maxRevenueShare`: The maximum revenue share percentage agreed upon between a child and parent when a child is registering as derivative. Must be between 0 and 100. **Recommended for simplicity: 100**
  * `request.derivData.maxRts`: The maximum number of royalty tokens that can be distributed to the external royalty policies. Must be between 0 and 100,000,000. **Recommended for simplicity: 100\_000\_000**
  * `request.derivData.licenseTemplate`: \[Optional] The address of the license template to be used for the linking.
* `request.ipMetadata`: \[Optional] The desired metadata for the newly minted NFT and newly registered IP.
  * `request.ipMetadata.ipMetadataURI` \[Optional] The URI of the metadata for the IP.
  * `request.ipMetadata.ipMetadataHash` \[Optional] The hash of the metadata for the IP.
  * `request.ipMetadata.nftMetadataURI` \[Optional] The URI of the metadata for the NFT.
  * `request.ipMetadata.nftMetadataHash` \[Optional] The hash of the metadata for the IP NFT.
* `request.deadline`: \[Optional]The deadline for the signature in milliseconds.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript TypeScript
import { toHex } from 'viem';

const derivData: DerivativeData = {
  parentIpIds: ["0xd142822Dc1674154EaF4DDF38bbF7EF8f0D8ECe4"],
  licenseTermsIds: ["1"],
  maxMintingFee: BitInt(0), // disabled
  maxRts: 100_000_000, // default
  maxRevenueShare: 100, // default
};

const response = await client.ipAsset.registerDerivativeIp({
  nftContract: "0x041B4F29183317Fd352AE57e331154b73F8a1D73", // your NFT contract address
  tokenId: '127',
  derivData,
  // https://docs.story.foundation/docs/ip-asset#adding-nft--ip-metadata-to-ip-asset
  ipMetadata: {
    ipMetadataURI: 'test-uri',
    ipMetadataHash: toHex('test-metadata-hash', { size: 32 }),
    nftMetadataHash: toHex('test-nft-metadata-hash', { size: 32 }),
    nftMetadataURI: 'test-nft-uri',
  },
  txOptions: { waitForTransaction: true }
});

console.log(`Completed at transaction hash ${response.txHash}, IPA ID: ${response.ipId}`);
```
```typescript Request Type
export type RegisterIpAndMakeDerivativeRequest = {
  nftContract: Address;
  tokenId: string | number | bigint;
  deadline?: string | number | bigint;
  derivData: DerivativeData;
  sigMetadataAndRegister?: {
    signer: Address;
    deadline: bigint | string | number;
    signature: Hex;
  };
} & IpMetadataAndTxOptions & WithWipOptions;

export type DerivativeData = {
  parentIpIds: Address[];
  licenseTermsIds: bigint[] | string[] | number[];
  maxMintingFee: bigint | string | number;
  maxRts: number | string;
  maxRevenueShare: number | string;
  licenseTemplate?: Address;
};
```
```typescript Response Type
export type RegisterIpAndMakeDerivativeResponse = {
  txHash?: Hex;
  encodedTxData?: EncodedTxData;
  ipId?: Address;
  tokenId?: bigint;
  receipt?: TransactionReceipt;
};
```

## batchRegisterDerivative

Batch registers a derivative directly with parent IP's license terms.

| Method                    | Type                                                                                    |
| ------------------------- | --------------------------------------------------------------------------------------- |
| `batchRegisterDerivative` | `(request: BatchRegisterDerivativeRequest) => Promise<BatchRegisterDerivativeResponse>` |

## mintAndRegisterIpAndMakeDerivative

Mint an NFT from a collection and register it as a derivative IP without license tokens.

> 📘 NFT Metadata
>
> Note that this function will also set the underlying NFT's `tokenUri` to whatever is passed under `ipMetadata.nftMetadataURI`.

| Method                               | Type                                                                                                          |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------- |
| `mintAndRegisterIpAndMakeDerivative` | `(request: MintAndRegisterIpAndMakeDerivativeRequest) => Promise<MintAndRegisterIpAndMakeDerivativeResponse>` |

Parameters:

* `request.spgNftContract`: The address of the NFT collection.
* `request.allowDuplicates`: Set to true to allow minting IPs with the same NFT metadata.
* `request.derivData`: The derivative data to be used for registerDerivative.
  * `request.derivData.parentIpIds`: The IDs of the parent IPs to link the registered derivative IP.
  * `request.derivData.licenseTermsIds`: The IDs of the license terms to be used for the linking.
  * `request.derivData.maxMintingFee`: The maximum minting fee that the caller is willing to pay. If set to 0, then there is no no limit. **Recommended for simplicity: 0**
  * `request.derivData.maxRevenueShare`: The maximum revenue share percentage agreed upon between a child and parent when a child is registering as derivative. Must be between 0 and 100. **Recommended for simplicity: 100**
  * `request.derivData.maxRts`: The maximum number of royalty tokens that can be distributed to the external royalty policies. Must be between 0 and 100,000,000. **Recommended for simplicity: 100\_000\_000**
  * `request.derivData.licenseTemplate`: \[Optional] The address of the license template to be used for the linking.
* `request.ipMetadata`: \[Optional] The desired metadata for the newly minted NFT and newly registered IP.
  * `request.ipMetadata.ipMetadataURI` \[Optional] The URI of the metadata for the IP.
  * `request.ipMetadata.ipMetadataHash` \[Optional] The hash of the metadata for the IP.
  * `request.ipMetadata.nftMetadataURI` \[Optional] The URI of the metadata for the NFT.
  * `request.ipMetadata.nftMetadataHash` \[Optional] The hash of the metadata for the IP NFT.
* `request.recipient`: \[Optional] The address of the recipient of the minted NFT, default value is your wallet address.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript TypeScript
import { PIL_TYPE } from '@story-protocol/core-sdk';
import { toHex } from 'viem';

const derivData: DerivativeData = {
  parentIpIds: ["0xd142822Dc1674154EaF4DDF38bbF7EF8f0D8ECe4"],
  licenseTermsIds: ["1"],
  maxMintingFee: BitInt(0), // disabled
  maxRts: 100_000_000, // default
  maxRevenueShare: 100, // default
};

const response = await client.ipAsset.mintAndRegisterIpAndMakeDerivative({
  // an NFT contract address created by the SPG
  spgNftContract: "0xfE265a91dBe911db06999019228a678b86C04959",
  derivData,
  // https://docs.story.foundation/docs/ip-asset#adding-nft--ip-metadata-to-ip-asset
  ipMetadata: {
    ipMetadataURI: 'test-uri',
    ipMetadataHash: toHex('test-metadata-hash', { size: 32 }),
    nftMetadataHash: toHex('test-nft-metadata-hash', { size: 32 }),
    nftMetadataURI: 'test-nft-uri',
  },
  txOptions: { waitForTransaction: true }
});

console.log(`Completed at transaction hash ${response.txHash}, IPA ID: ${response.ipId}, Token ID: ${response.tokenId}`);
```
```typescript Request Type
export type MintAndRegisterIpAndMakeDerivativeRequest = {
  spgNftContract: Address;
  derivData: DerivativeData;
  recipient?: Address;
  allowDuplicates: boolean;
} & IpMetadataAndTxOptions & WithWipOptions;

export type DerivativeData = {
  parentIpIds: Address[];
  licenseTermsIds: bigint[] | string[] | number[];
  maxMintingFee: bigint | string | number;
  maxRts: number | string;
  maxRevenueShare: number | string;
  licenseTemplate?: Address;
};
```
```typescript Response Type
export type MintAndRegisterIpAndMakeDerivativeResponse = {
  encodedTxData?: EncodedTxData;
} & CommonRegistrationResponse;

export type CommonRegistrationResponse = {
  txHash?: Hex;
  ipId?: Address;
  tokenId?: bigint;
  receipt?: TransactionReceipt;
};
```

## batchMintAndRegisterIpAndMakeDerivative

Batch mint an NFT from a collection and register it as a derivative IP without license tokens.

| Method                                    | Type                                                                                                                    |
| ----------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| `batchMintAndRegisterIpAndMakeDerivative` | `(request: BatchMintAndRegisterIpAndMakeDerivativeRequest) => Promise<BatchMintAndRegisterIpAndMakeDerivativeResponse>` |

## mintAndRegisterIp

Mint an NFT from an SPGNFT collection and register it with metadata as an IP.

> 📘 NFT Metadata
>
> Note that this function will also set the underlying NFT's `tokenUri` to whatever is passed under `ipMetadata.nftMetadataURI`.

| Method              | Type                                                                 |
| ------------------- | -------------------------------------------------------------------- |
| `mintAndRegisterIp` | `(request: MintAndRegisterIpRequest) => Promise<RegisterIpResponse>` |

Parameters:

* `request.spgNftContract`: The address of the NFT collection.
* `request.allowDuplicates`: Set to true to allow minting IPs with the same NFT metadata.
* `request.recipient`: \[Optional] The address of the recipient of the minted NFT, default value is your wallet address.
* `request.ipMetadata`: \[Optional] The desired metadata for the newly minted NFT and newly registered IP.
  * `request.ipMetadata.ipMetadataURI` \[Optional] The URI of the metadata for the IP.
  * `request.ipMetadata.ipMetadataHash` \[Optional] The hash of the metadata for the IP.
  * `request.ipMetadata.nftMetadataURI` \[Optional] The URI of the metadata for the NFT.
  * `request.ipMetadata.nftMetadataHash` \[Optional] The hash of the metadata for the IP NFT.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript TypeScript
import { PIL_TYPE } from '@story-protocol/core-sdk';
import { toHex, Address, zeroAddress } from 'viem';

const newCollection = await client.nftClient.createNFTCollection({
  name: 'Test NFT',
  symbol: 'TEST',
  isPublicMinting: true,
  mintOpen: true,
  mintFeeRecipient: zeroAddress,
  contractURI: '',
  txOptions: { waitForTransaction: true },
})

const response = await client.ipAsset.mintAndRegisterIp({
  // an NFT contract address created by the SPG
  spgNftContract: newCollection.spgNftContract as Address,
  // set to true to have multiple NFTs with same metadata
  allowDuplicates: true,
  // https://docs.story.foundation/docs/ip-asset#adding-nft--ip-metadata-to-ip-asset
  ipMetadata: {
    ipMetadataURI: 'test-uri',
    ipMetadataHash: toHex('test-metadata-hash', { size: 32 }),
    nftMetadataHash: toHex('test-nft-metadata-hash', { size: 32 }),
    nftMetadataURI: 'test-nft-uri',
  },
  txOptions: { waitForTransaction: true }
});

console.log(`Completed at transaction hash ${response.txHash}, NFT Token ID: ${response.tokenId}, IPA ID: ${response.ipId}, License Terms ID: ${response.licenseTermsId}`);
```
```typescript Request Type
export type MintAndRegisterIpRequest = {
  spgNftContract: Address;
  recipient?: Address;
  allowDuplicates: boolean;
} & IpMetadataAndTxOptions;
```
```typescript Response Type
export type RegisterIpResponse = {
  encodedTxData?: EncodedTxData;
} & CommonRegistrationResponse;

export type CommonRegistrationResponse = {
  txHash?: Hex;
  ipId?: Address;
  tokenId?: bigint;
  receipt?: TransactionReceipt;
};
```

## registerPilTermsAndAttach

Register Programmable IP License Terms (if unregistered) and attach it to IP.

| Method                      | Type                                                                                        |
| --------------------------- | ------------------------------------------------------------------------------------------- |
| `registerPilTermsAndAttach` | `(request: RegisterPilTermsAndAttachRequest) => Promise<RegisterPilTermsAndAttachResponse>` |

Parameters:

* `request.ipId`: The ID of the IP.
* `request.licenseTermsData[]`: The array of license terms to be attached.
  * `request.licenseTermsData.terms`: See the [LicenseTerms type](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/resources/license.ts#L26).
  * `request.licenseTermsData.licensingConfig`: See the [LicensingConfig type](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/common.ts#L15).
* `request.deadline`: \[Optional] The deadline for the signature in milliseconds, default is 1000s.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript TypeScript
import { LicenseTerms } from '@story-protocol/core-sdk';

const commercialRemixTerms: LicenseTerms = {
  transferable: true,
  royaltyPolicy: RoyaltyPolicyLAP, // insert RoyaltyPolicyLAP address from https://docs.story.foundation/docs/deployed-smart-contracts
  defaultMintingFee: BigInt(0),
  expiration: BigInt(0),
  commercialUse: true,
  commercialAttribution: true,
  commercializerChecker: zeroAddress,
  commercializerCheckerData: zeroAddress,
  commercialRevShare: 50, // can claim 50% of derivative revenue
  commercialRevCeiling: BigInt(0),
  derivativesAllowed: true,
  derivativesAttribution: true,
  derivativesApproval: false,
  derivativesReciprocal: true,
  derivativeRevCeiling: BigInt(0),
  currency: '0x1514000000000000000000000000000000000000', // insert $WIP address from https://docs.story.foundation/docs/deployed-smart-contracts
  uri: '',
}

const licensingConfig: LicensingConfig = {
  isSet: false,
  mintingFee: BigInt(0),
  licensingHook: zeroAddress,
  hookData: zeroHash,
  commercialRevShare: 0,
  disabled: false,
  expectMinimumGroupRewardShare: 0,
  expectGroupRewardPool: zeroAddress,
};

const response = await client.ipAsset.registerPilTermsAndAttach({
  ipId: '0x4c1f8c1035a8cE379dd4ed666758Fb29696CF721',
  licenseTermsData: [{ terms: commercialRemixTerms, licensingConfig }],
  txOptions: { waitForTransaction: true },
})
console.log(`License Terms ${response.licenseTermsId} attached to IP Asset.`)
```
```typescript Request Type
export type RegisterPilTermsAndAttachRequest = {
  ipId: Address;
  licenseTermsData: LicenseTermsData<RegisterPILTermsRequest, LicensingConfig>[];
  deadline?: string | number | bigint;
  txOptions?: TxOptions;
};
```
```typescript Response Type
export type RegisterPilTermsAndAttachResponse = {
  txHash?: Hex;
  encodedTxData?: EncodedTxData;
  licenseTermsIds?: bigint[];
};
```

## mintAndRegisterIpAndMakeDerivativeWithLicenseTokens

Mint an NFT from a collection and register it as a derivative IP using license tokens

> 📘 NFT Metadata
>
> Note that this function will also set the underlying NFT's `tokenUri` to whatever is passed under `ipMetadata.nftMetadataURI`.

| Method                                                | Type                                                                                                   |
| ----------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| `mintAndRegisterIpAndMakeDerivativeWithLicenseTokens` | `(request: MintAndRegisterIpAndMakeDerivativeWithLicenseTokensRequest) => Promise<RegisterIpResponse>` |

Parameters:

* `request.spgNftContract`: The address of the NFT collection.
* `request.allowDuplicates`: Set to true to allow minting IPs with the same NFT metadata.
* `request.maxRts`: The maximum number of royalty tokens that can be distributed to the external royalty policies. Must be between 0 and 100,000,000. **Recommended for simplicity: 100\_000\_000**
* `request.licenseTokenIds`: The IDs of the license tokens to be burned for linking the IP to parent IPs.
* `request.ipMetadata`: \[Optional] The desired metadata for the newly minted NFT and newly registered IP.
  * `request.ipMetadata.ipMetadataURI` \[Optional] The URI of the metadata for the IP.
  * `request.ipMetadata.ipMetadataHash` \[Optional] The hash of the metadata for the IP.
  * `request.ipMetadata.nftMetadataURI` \[Optional] The URI of the metadata for the NFT.
  * `request.ipMetadata.nftMetadataHash` \[Optional] The hash of the metadata for the IP NFT.
* `request.recipient`: \[Optional] The address to receive the minted NFT, default value is your wallet address.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript TypeScript
import { toHex } from 'viem';

const response = await client.ipAsset.mintAndRegisterIpAndMakeDerivativeWithLicenseTokens({
  spgNftContract: "0xfE265a91dBe911db06999019228a678b86C04959", // your NFT contract address
  licenseTokenIds: ['10'],
  maxRts: 100_000_000, // default
  // set to true to allow ip with same nft metadata
  allowDuplicates: true,
  // https://docs.story.foundation/docs/ip-asset#adding-nft--ip-metadata-to-ip-asset
  ipMetadata: {
    ipMetadataURI: 'test-uri',
    ipMetadataHash: toHex('test-metadata-hash', { size: 32 }),
    nftMetadataHash: toHex('test-nft-metadata-hash', { size: 32 }),
    nftMetadataURI: 'test-nft-uri',
  },
  txOptions: { waitForTransaction: true }
});

console.log(`Completed at transaction hash ${response.txHash}, IPA ID: ${response.ipId}, Token ID: ${response.tokenId}`);
```
```typescript Request Type
export type MintAndRegisterIpAndMakeDerivativeWithLicenseTokensRequest = {
  spgNftContract: Address;
  licenseTokenIds: string[] | bigint[] | number[];
  recipient?: Address;
  maxRts: number | string;
  allowDuplicates: boolean;
} & IpMetadataAndTxOptions & WithWipOptions;
```
```typescript Response Type
export type RegisterIpResponse = {
  encodedTxData?: EncodedTxData;
} & CommonRegistrationResponse;

export type CommonRegistrationResponse = {
  txHash?: Hex;
  ipId?: Address;
  tokenId?: bigint;
  receipt?: TransactionReceipt;
};
```

## registerIpAndMakeDerivativeWithLicenseTokens

Register the given NFT as a derivative IP using license tokens.

> 📘 NFT Metadata
>
> Note that this function will also set the underlying NFT's `tokenUri` to whatever is passed under `ipMetadata.nftMetadataURI`.

| Method                                         | Type                                                                                            |
| ---------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| `registerIpAndMakeDerivativeWithLicenseTokens` | `(request: RegisterIpAndMakeDerivativeWithLicenseTokensRequest) => Promise<RegisterIpResponse>` |

Parameters:

* `request.nftContract`: The address of the NFT collection.
* `request.tokenId`: The ID of the NFT.
* `request.maxRts`: The maximum number of royalty tokens that can be distributed to the external royalty policies. Must be between 0 and 100,000,000. **Recommended for simplicity: 100\_000\_000**
* `request.licenseTokenIds`: The IDs of the license tokens to be burned for linking the IP to parent IPs.
* `request.ipMetadata`: \[Optional] The desired metadata for the newly minted NFT and newly registered IP.
  * `request.ipMetadata.ipMetadataURI` \[Optional] The URI of the metadata for the IP.
  * `request.ipMetadata.ipMetadataHash` \[Optional] The hash of the metadata for the IP.
  * `request.ipMetadata.nftMetadataURI` \[Optional] The URI of the metadata for the NFT.
  * `request.ipMetadata.nftMetadataHash` \[Optional] The hash of the metadata for the IP NFT.
* `request.deadline`: \[Optional] The deadline for the signature in milliseconds, default is 1000ms.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript TypeScript
import { toHex } from 'viem';

const response = await client.ipAsset.registerIpAndMakeDerivativeWithLicenseTokens({
  nftContract: "0x041B4F29183317Fd352AE57e331154b73F8a1D73", // your NFT contract address
  tokenId: '127',
  licenseTokenIds: ['10'],
  maxRts: 100_000_000, // default
  // https://docs.story.foundation/docs/ip-asset#adding-nft--ip-metadata-to-ip-asset
  ipMetadata: {
    ipMetadataURI: 'test-uri',
    ipMetadataHash: toHex('test-metadata-hash', { size: 32 }),
    nftMetadataHash: toHex('test-nft-metadata-hash', { size: 32 }),
    nftMetadataURI: 'test-nft-uri',
  },
  txOptions: { waitForTransaction: true }
});

console.log(`Completed at transaction hash ${response.txHash}, IPA ID: ${response.ipId}`);
```
```typescript Request Type
export type RegisterIpAndMakeDerivativeWithLicenseTokensRequest = {
  nftContract: Address;
  tokenId: string | number | bigint;
  licenseTokenIds: string[] | bigint[] | number[];
  deadline?: string | number | bigint;
  maxRts: number | string;
} & IpMetadataAndTxOptions & WithWipOptions;
```
```typescript Response Type
export type RegisterIpResponse = {
  encodedTxData?: EncodedTxData;
} & CommonRegistrationResponse;

export type CommonRegistrationResponse = {
  txHash?: Hex;
  ipId?: Address;
  tokenId?: bigint;
  receipt?: TransactionReceipt;
};
```

# Group
## GroupClient

### Methods

* registerGroup
* mintAndRegisterIpAndAttachLicenseAndAddToGroup
* registerIpAndAttachLicenseAndAddToGroup
* registerGroupAndAttachLicense
* registerGroupAndAttachLicenseAndAddIps

### registerGroup

Registers a Group IPA.

| Method          | Type                                                                |
| --------------- | ------------------------------------------------------------------- |
| `registerGroup` | `(request: RegisterGroupRequest) => Promise<RegisterGroupResponse>` |

Parameters:

* `request.groupPool`: The address specifying how royalty will be split amongst the pool of IPs in the group.
* `request.txOptions`: [Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type RegisterGroupResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  groupId?: Address;
};
```

### mintAndRegisterIpAndAttachLicenseAndAddToGroup

Mint an NFT from a SPGNFT collection, register it with metadata as an IP, attach license terms to the registered IP, and add it to a group IP.

| Method                                           | Type                                                                                                                                  |
| ------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------- |
| `mintAndRegisterIpAndAttachLicenseAndAddToGroup` | `(request: MintAndRegisterIpAndAttachLicenseAndAddToGroupRequest) => Promise<MintAndRegisterIpAndAttachLicenseAndAddToGroupResponse>` |

Parameters:

* `request.nftContract`: The address of the NFT collection.
* `request.groupId`: The ID of the group IP to add the newly registered IP.
* `request.licenseTermsId`: The ID of the registered license terms that will be attached to the new IP.
* `request.recipient`: [Optional] The address of the recipient of the minted NFT,default value is your wallet address.
* `request.licenseTemplate`: [Optional] The address of the license template to be attached to the new group IP,default value is Programmable IP License.
* `request.deadline`: [Optional] The deadline for the signature in milliseconds,default value is 1000ms.
* `request.ipMetadata`: [Optional] The desired metadata for the newly minted NFT and newly registered IP.
  * `request.ipMetadata.ipMetadataURI` [Optional] The URI of the metadata for the IP.
  * `request.ipMetadata.ipMetadataHash` [Optional] The hash of the metadata for the IP.
  * `request.ipMetadata.nftMetadataURI` [Optional] The URI of the metadata for the NFT.
  * `request.ipMetadata.nftMetadataHash` [Optional] The hash of the metadata for the IP NFT.
* `request.txOptions`: [Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type MintAndRegisterIpAndAttachLicenseAndAddToGroupResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  ipId?: Address;
  tokenId?: bigint;
};
```

### registerIpAndAttachLicenseAndAddToGroup

Register an NFT as IP with metadata, attach license terms to the registered IP, and add it to a group IP.

| Method                                    | Type                                                                                                                    |
| ----------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| `registerIpAndAttachLicenseAndAddToGroup` | `(request: RegisterIpAndAttachLicenseAndAddToGroupRequest) => Promise<RegisterIpAndAttachLicenseAndAddToGroupResponse>` |

Parameters:

* `request.spgNftContract`: The address of the NFT collection.
* `request.tokenId`: The ID of the NFT.
* `request.groupId`: The ID of the group IP to add the newly registered IP.
* `request.licenseTermsId`: The ID of the registered license terms that will be attached to the new IP.
* `request.licenseTemplate`: [Optional] The address of the license template to be attached to the new group IP, default value is Programmable IP License.
* `request.deadline`: [Optional] The deadline for the signature in milliseconds, default is 1000ms.
* `request.ipMetadata`: [Optional] The desired metadata for the newly minted NFT and newly registered IP.
  * `request.ipMetadata.ipMetadataURI` [Optional] The URI of the metadata for the IP.
  * `request.ipMetadata.ipMetadataHash` [Optional] The hash of the metadata for the IP.
  * `request.ipMetadata.nftMetadataURI` [Optional] The URI of the metadata for the NFT.
  * `request.ipMetadata.nftMetadataHash` [Optional] The hash of the metadata for the IP NFT.
* `request.txOptions`: [Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type RegisterIpAndAttachLicenseAndAddToGroupResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  ipId?: Address;
  tokenId?: bigint;
};
```

### registerGroupAndAttachLicense

Register a group IP with a group reward pool and attach license terms to the group IP.

| Method                          | Type                                                                                                |
| ------------------------------- | --------------------------------------------------------------------------------------------------- |
| `registerGroupAndAttachLicense` | `(request: RegisterGroupAndAttachLicenseRequest) => Promise<RegisterGroupAndAttachLicenseResponse>` |

Parameters:

* `request.groupPool`: The address specifying how royalty will be split amongst the pool of IPs in the group.
* `request.licenseTermsId`: The ID of the registered license terms that will be attached to the new group IP.
* `request.licenseTemplate`: [Optional] The address of the license template to be attached to the new group IP, default value is Programmable IP License.
* `request.txOptions`: [Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type RegisterGroupAndAttachLicenseResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  groupId?: Address;
};
```

### registerGroupAndAttachLicenseAndAddIps

Register a group IP with a group reward pool, attach license terms to the group IP, and add individual IPs to the group IP.

| Method                                   | Type                                                                                                                  |
| ---------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| `registerGroupAndAttachLicenseAndAddIps` | `(request: RegisterGroupAndAttachLicenseAndAddIpsRequest) => Promise<RegisterGroupAndAttachLicenseAndAddIpsResponse>` |

Parameters:

* `request.pIds`: must have the same PIL terms as the group IP.
* `request.groupPool`: The address specifying how royalty will be split amongst the pool of IPs in the group.
* `request.licenseTermsId`: The ID of the registered license terms that will be attached to the new group IP.
* `request.licenseTemplate`: [Optional] The address of the license template to be attached to the new group IP,default value is Programmable IP License.
* `request.txOptions`: [Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type RegisterGroupAndAttachLicenseAndAddIpsResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  groupId?: Address;
};
```


# WIP Client
## WipClient

### Methods

* deposit
* withdraw
* approve
* balanceOf

### deposit

Wraps the selected amount of IP to WIP. The WIP will be deposited to the wallet that transferred the IP.

| Method    | Type                        |
| --------- | --------------------------- |
| `deposit` | `(request: DepositRequest)` |

Parameters:

* `request.amount`: The amount to deposit.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Request Type
export type DepositRequest = WithTxOptions & {
  amount: TokenAmountInput;
};
```

### withdraw

Unwraps the selected amount of WIP to IP.

| Method     | Type                         |
| ---------- | ---------------------------- |
| `withdraw` | `(request: WithdrawRequest)` |

Parameters:

* `request.amount`: The amount to withdraw.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Request Type
export type WithdrawRequest = WithTxOptions & {
  amount: TokenAmountInput;
};
```

### approve

Approve a spender to use the wallet's WIP balance.

| Method    | Type                                                    |
| --------- | ------------------------------------------------------- |
| `approve` | `(request: ApproveRequest) => Promise<{ txHash: Hex }>` |

Parameters:

* `request.amount`: The amount of WIP tokens to approve.
* `request.spender`: The address that will use the WIP tokens
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Request Type
export type ApproveRequest = WithTxOptions & {
  spender: Address;
  amount: TokenAmountInput;
};
```

### balanceOf

Returns the balance of WIP for an address.

| Method      | Type                                 |
| ----------- | ------------------------------------ |
| `balanceOf` | `(addr: Address) => Promise<bigint>` |

Parameters:

* `addr`: The address you want to check the baalnce for.

# Dispute
## DisputeClient

### Methods

* raiseDispute
* cancelDispute
* resolveDispute

### raiseDispute

Raises a dispute on a given ipId

| Method         | Type                                                              |
| -------------- | ----------------------------------------------------------------- |
| `raiseDispute` | `(request: RaiseDisputeRequest) => Promise<RaiseDisputeResponse>` |

Parameters:

* `request.targetIpId`: The IP ID that is the target of the dispute.
* `request.targetTag`: The target tag of the dispute.
* `request.cid`: CID (Content Identifier) is a unique identifier in IPFS, including CID v0 (base58) and CID v1 (base32).
* `request.data`: \[Optional] The data to initialize the policy.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript TypeScript
const disputeResponse = await client.dispute.raiseDispute({
  targetIpId: '0xC92EC2f4c86458AFee7DD9EB5d8c57920BfCD0Ba',
  // NOTE: you must use your own CID here, because every time it is used,
  // the protocol does not allow you to use it again
  cid: 'QmbWqxBEKC3P8tqsKc98xmWNzrzDtRLMiMPL8wBuTGsMnR',
  // you must pick from one of the whitelisted tags here: 
  // https://docs.story.foundation/docs/dispute-module#dispute-tags
  targetTag: 'IMPROPER_REGISTRATION',
  bond: 0,
  liveness: 2592000,
  txOptions: { waitForTransaction: true },
})
console.log(`Dispute raised at transaction hash ${disputeResponse.txHash}, Dispute ID: ${disputeResponse.disputeId}`)
```
```typescript Request Type
export type RaiseDisputeRequest = {
  targetIpId: Address;
  cid: string;
  targetTag: string;
  liveness: bigint | number | string;
  bond: bigint | number | string;
  txOptions?: TxOptions;
};
```
```typescript Response Type
export type RaiseDisputeResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  disputeId?: bigint;
};
```

### cancelDispute

Cancels an ongoing dispute

| Method          | Type                                                                |
| --------------- | ------------------------------------------------------------------- |
| `cancelDispute` | `(request: CancelDisputeRequest) => Promise<CancelDisputeResponse>` |

Parameters:

* `request.disputeId`: The ID of the dispute to be cancelled.
* `request.data`: \[Optional] Additional data used in the cancellation process.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Request Type
export type CancelDisputeRequest = {
  disputeId: number | string | bigint;
  data?: Address;
  txOptions?: TxOptions;
};
```
```typescript Response Type
export type CancelDisputeResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
};
```

### resolveDispute

Resolves a dispute after it has been judged

| Method           | Type                                                                  |
| ---------------- | --------------------------------------------------------------------- |
| `resolveDispute` | `(request: ResolveDisputeRequest) => Promise<ResolveDisputeResponse>` |

Parameters:

* `request.disputeId`: The ID of the dispute to be resolved.
* `request.data`: The data to resolve the dispute.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Request Type
export type ResolveDisputeRequest = {
  disputeId: number | string | bigint;
  data: Address;
  txOptions?: TxOptions;
};
```
```typescript Response Type
export type ResolveDisputeResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
};
```

# Permissions
## PermissionClient

### Methods

* setPermission
* createSetPermissionSignature
* setAllPermissions
* setBatchPermissions
* createBatchPermissionSignature

### setPermission

Sets the permission for a specific function call.

Each policy is represented as a mapping from an IP account address to a signer address to a recipient\
address to a function selector to a permission level. The permission level can be 0 (ABSTAIN), 1 (ALLOW), or\
2 (DENY).

By default, all policies are set to 0 (ABSTAIN), which means that the permission is not set. The owner of IP Account by default has all permission.

| Method          | Type                                                                  |
| --------------- | --------------------------------------------------------------------- |
| `setPermission` | `(request: SetPermissionsRequest) => Promise<SetPermissionsResponse>` |

Parameters:

* `request.ipId`: The IP ID that grants the permission for `signer`.
* `request.signer`: The address that can call `to` on behalf of the `ipAccount`.
* `request.to`: The address that can be called by the `signer` (currently only modules can be `to`)
* `request.permission`: The new permission level.
* `request.func`: [Optional] The function selector string of `to` that can be called by the `signer` on behalf of the `ipAccount`. By default, it allows all functions.
* `request.txOptions`: [Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type SetPermissionsResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  success?: boolean;
};
```

### createSetPermissionSignature

Specific permission overrides wildcard permission with signature.

| Method                         | Type                                                                                |
| ------------------------------ | ----------------------------------------------------------------------------------- |
| `createSetPermissionSignature` | `(request: CreateSetPermissionSignatureRequest) => Promise<SetPermissionsResponse>` |

Parameters:

* `request.ipId`: The IP ID that grants the permission for `signer`.
* `request.signer`: The address that can call `to` on behalf of the `ipAccount`.
* `request.to`: The address that can be called by the `signer` (currently only modules can be `to`)
* `request.permission`: The new permission level.
* `request.func`: [Optional] The function selector string of `to` that can be called by the `signer` on behalf of the `ipAccount`. By default, it allows all functions.
* `request.deadline`: [Optional] The deadline for the signature in milliseconds, default is 1000ms.
* `request.txOptions`: [Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type SetPermissionsResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  success?: boolean;
};
```

### setAllPermissions

Sets permission to a signer for all functions across all modules.

| Method              | Type                                                                     |
| ------------------- | ------------------------------------------------------------------------ |
| `setAllPermissions` | `(request: SetAllPermissionsRequest) => Promise<SetPermissionsResponse>` |

Parameters:

* `request.ipId`: The IP ID that grants the permission for `signer`.
* `request.signer`: The address of the signer receiving the permissions.
* `request.permission`: The new permission.
* `request.txOptions`: [Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type SetPermissionsResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  success?: boolean;
};
```

### setBatchPermissions

Sets a batch of permissions in a single transaction.

| Method                | Type                                                                       |
| --------------------- | -------------------------------------------------------------------------- |
| `setBatchPermissions` | `(request: SetBatchPermissionsRequest) => Promise<SetPermissionsResponse>` |

Parameters:

* `request.permissions[]`: An array of `Permission` structure, each representing the permission to be set.
  * `request.permissions[].ipId`: The IP ID that grants the permission for `signer`.
  * `request.permissions[].signer`: The address that can call `to` on behalf of the `ipAccount`.
  * `request.permissions[].to`: The address that can be called by the `signer` (currently only modules can be `to`)
  * `request.permissions[].permission`: The new permission level.
  * `request.permissions[].func`: [Optional] The function selector string of `to` that can be called by the `signer` on behalf of the `ipAccount`. By default, it allows all functions.
* `request.deadline`: [Optional] The deadline for the signature in milliseconds, default is 1000ms.
* `request.txOptions`: [Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type SetPermissionsResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  success?: boolean;
};
```

### createBatchPermissionSignature

Sets a batch of permissions in a single transaction with signature.

| Method                           | Type                                                                                  |
| -------------------------------- | ------------------------------------------------------------------------------------- |
| `createBatchPermissionSignature` | `(request: CreateBatchPermissionSignatureRequest) => Promise<SetPermissionsResponse>` |

Parameters:

* `request.ipId`: The IP ID that grants the permission for `signer`
* `request.permissions[]` - An array of `Permission` structure, each representing the permission to be set.
  * `request.permissions[].ipId`: The IP ID that grants the permission for `signer`.
  * `request.permissions[].signer`: The address that can call `to` on behalf of the `ipAccount`.
  * `request.permissions[].to`: The address that can be called by the `signer` (currently only modules can be `to`)
  * `request.permissions[].permission`: The new permission level.
  * `request.permissions[].func`: [Optional] The function selector string of `to` that can be called by the `signer` on behalf of the `ipAccount`. By default, it allows all functions.
* `request.txOptions`: [Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type SetPermissionsResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  success?: boolean;
};
```


# Overview
This section provides a detailed description of every function in our SDK.

<Cards columns={2}>
  <Card title="NPM Package" href="https://www.npmjs.com/package/@story-protocol/core-sdk" icon="fa-home" iconColor="red" target="_blank">
    View our `@story-protocol/core-sdk` package on npm.
  </Card>

  <Card title="Step-by-Step Guide" href="https://docs.story.foundation/docs/typescript-sdk" icon="fa-home" target="_blank">
    Learn our SDK through a series of tutorials with the TypeScript SDK Guide.
  </Card>
</Cards>

Interact with the [📜 Licensing Module](doc:licensing-module):

* [Register an IP Asset](doc:sdk-ipasset)
* [Mint & Attach License Terms](doc:sdk-license)

Interact with the [💸 Royalty Module](doc:royalty-module):

* [Pay & Claim Royalty](doc:sdk-royalty)

Interact with the [❌ Dispute Module](doc:dispute-module):

* [Raise a Dispute](doc:sdk-dispute)

Interact with the [👥 Grouping Module](doc:grouping-module):

* [Manage Groups](doc:sdk-group)

And lastly, some utility/extra clients:

* [Set Permissions](doc:sdk-permissions)
* [NFT Client](doc:sdk-nftclient)
* [WIP Client](doc:wip-client)

# Royalty
## RoyaltyClient

### Methods

* payRoyaltyOnBehalf
* claimableRevenue
* claimAllRevenue
* getRoyaltyVaultAddress

### payRoyaltyOnBehalf

Allows the function caller to pay royalties to the receiver IP asset on behalf of the payer IP asset.

| Method               | Type                                                                          |
| -------------------- | ----------------------------------------------------------------------------- |
| `payRoyaltyOnBehalf` | `(request: PayRoyaltyOnBehalfRequest) => Promise<PayRoyaltyOnBehalfResponse>` |

Parameters:

* `request.receiverIpId`: The ipId that receives the royalties.
* `request.payerIpId`: The ID of the IP asset that pays the royalties.
* `request.token`: The token to use to pay the royalties.
* `request.amount`: The amount to pay.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript TypeScript
import { WIP_TOKEN_ADDRESS } from '@story-protocol/core-sdk'

const payRoyalty = await client.royalty.payRoyaltyOnBehalf({
  receiverIpId: "0x0b825D9E5FA196e6B563C0a446e8D9885057f9B1", // child ipId
  payerIpId: zeroAddress,
  token: WIP_TOKEN_ADDRESS,
  amount: 2,
  txOptions: { waitForTransaction: true },
});

console.log(`Paid royalty at transaction hash ${payRoyalty.txHash}`);
```
```typescript Request Type
export type PayRoyaltyOnBehalfRequest = {
  receiverIpId: Address;
  payerIpId: Address;
  token: Address;
  amount: TokenAmountInput;
} & WithTxOptions & WithWipOptions;
```
```typescript Response Type
export type PayRoyaltyOnBehalfResponse = {
  txHash?: string;
  receipt?: TransactionReceipt;
  encodedTxData?: EncodedTxData;
};
```

### claimableRevenue

Get total amount of revenue token claimable by a royalty token holder.

| Method             | Type                                                                      |
| ------------------ | ------------------------------------------------------------------------- |
| `claimableRevenue` | `(request: ClaimableRevenueRequest) => Promise<ClaimableRevenueResponse>` |

Parameters:

* `request.royaltyVaultIpId`: The id of the royalty vault.
* `request.claimer`: The address of the royalty token holder.
* `request.token`: The revenue token to claim.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Request Type
export type ClaimableRevenueRequest = {
  royaltyVaultIpId: Address;
	claimer: Address;
  token: Address;
}
```
```typescript Response Type
export type ClaimableRevenueResponse = bigint;
```

### claimAllRevenue

Claims all revenue from child IP Assets and/or from your own IP Royalty Vault.

| Method            | Type                                                                    |
| ----------------- | ----------------------------------------------------------------------- |
| `claimAllRevenue` | `(request: ClaimAllRevenueRequest) => Promise<ClaimAllRevenueResponse>` |

Parameters:

* `request.ancestorIpId`: The address of the ancestor IP from which the revenue is being claimed.
* `request.claimer`: The address of the claimer of the currency (revenue) tokens. This is normally the ipId of the ancestor IP if the IP has all royalty tokens. Otherwise, this would be the address that is holding the ancestor IP royalty tokens.
* `request.childIpIds[]`: The addresses of the child IPs from which royalties are derived.
* `request.royaltyPolicies[]`: The addresses of the royalty policies, where royaltyPolicies\[i] governs the royalty flow for childIpIds\[i].
* `request.currencyTokens[]`: The addresses of the currency tokens in which royalties will be claimed.
* `request.claimOptions`: \[Optional]
  * `request.claimOptions.autoTransferAllClaimedTokensFromIp`: \[Optional]When enabled, all claimed tokens on the claimer are transferred to the wallet address if the wallet owns the IP. If the wallet is the claimer or if the claimer is not an IP owned by the wallet, then the tokens will not be transferred. Set to false to disable auto transferring claimed tokens from the claimer. **Default: true**
  * `request.claimOptions.autoUnwrapIpTokens`: \[Optional]By default all claimed WIP tokens are converted back to IP after they are transferred. Set this to false to disable this behavior. **Default: false**
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript TypeScript
import { WIP_TOKEN_ADDRESS } from '@story-protocol/core-sdk'

const claimRevenue = await client.royalty.claimAllRevenue({
  // IP Asset 1's (parent) ipId
  ancestorIpId: '0x089d75C9b7E441dA3115AF93FF9A855BDdbfe384',
  // whoever owns the royalty tokens associated with IP Royalty Vault 1
  // (most likely the associated ipId, which is IP Asset 1's ipId)
  claimer: '0x089d75C9b7E441dA3115AF93FF9A855BDdbfe384',
  currencyTokens: [WIP_TOKEN_ADDRESS],
  // IP Asset 2's (child) ipId
  childIpIds: ['0xDa03c4B278AD44f5a669e9b73580F91AeDE0E3B2'],
  // testnet address of RoyaltyPolicyLAP
  royaltyPolicies: ['0xBe54FB168b3c982b7AaE60dB6CF75Bd8447b390E']
})

console.log(`Claimed revenue: ${claimRevenue.claimedTokens}`);
```
```typescript Request Type
export type ClaimAllRevenueRequest = {
  ancestorIpId: Address;
  claimer: Address;
  childIpIds: Address[];
  royaltyPolicies: Address[];
  currencyTokens: Address[];
  claimOptions?: {
    autoTransferAllClaimedTokensFromIp?: boolean;
    autoUnwrapIpTokens?: boolean;
  };
};
```
```typescript Response Type
export type ClaimAllRevenueResponse = {
  txHashes: Hash[];
  receipt?: TransactionReceipt;
  claimedTokens?: ClaimedToken[];
};

export type ClaimedToken = {
  token: Address;
  amount: bigint;
};
```

### getRoyaltyVaultAddress

Get the royalty vault proxy address of given royaltyVaultIpId.

| Method                   | Type                                          |
| ------------------------ | --------------------------------------------- |
| `getRoyaltyVaultAddress` | `(royaltyVaultIpId: Hex) => Promise<Address>` |

Parameters:

* `royaltyVaultIpId`: the `ipId` associated with the royalty vault.

# License
## LicenseClient

### Methods

* attachLicenseTerms
* mintLicenseTokens
* registerPILTerms
* registerNonComSocialRemixingPIL
* registerCommercialUsePIL
* registerCommercialRemixPIL
* getLicenseTerms

### attachLicenseTerms

Attaches license terms to an IP.

| Method               | Type                                                                 |
| -------------------- | -------------------------------------------------------------------- |
| `attachLicenseTerms` | `(request: AttachLicenseTermsRequest) => AttachLicenseTermsResponse` |

Parameters:

* `request.ipId`: The address of the IP to which the license terms are attached.
* `request.licenseTemplate`: The address of the license template.
* `request.licenseTermsId`: The ID of the license terms.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript TypeScript
const response = await client.license.attachLicenseTerms({
  licenseTermsId: "1", 
  ipId: "0x4c1f8c1035a8cE379dd4ed666758Fb29696CF721",
  txOptions: { waitForTransaction: true }
});

if (response.success) {
  console.log(`Attached License Terms to IPA at transaction hash ${response.txHash}.`)
} else {
  console.log(`License Terms already attached to this IPA.`)
}
```
```typescript Request Type
export type AttachLicenseTermsRequest = {
  ipId: Address;
  licenseTermsId: string | number | bigint;
  licenseTemplate?: Address;
  txOptions?: TxOptions;
};
```
```typescript Response Type
export type AttachLicenseTermsResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  success?: boolean;
};
```

### mintLicenseTokens

Mints license tokens for the license terms attached to an IP.

The license tokens are minted to the receiver.

The license terms must be attached to the IP before calling this function.

IP owners can mint license tokens for their IPs for arbitrary license terms without attaching the license terms to IP.

It might require the caller pay the minting fee, depending on the license terms or configured by the iP owner. The minting fee is paid in the minting fee token specified in the license terms or configured by the IP owner. IP owners can configure the minting fee of their IPs or configure the minting fee module to determine the minting fee.

| Method              | Type                                                                        |
| ------------------- | --------------------------------------------------------------------------- |
| `mintLicenseTokens` | `(request: MintLicenseTokensRequest) => Promise<MintLicenseTokensResponse>` |

Parameters:

* `request.licensorIpId`: The licensor IP ID.
* `request.licenseTemplate`: The address of the license template.
* `request.licenseTermsId`: The ID of the license terms within the license template.
* `request.amount`: The amount of license tokens to mint.
* `request.receiver`: The address of the receiver.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript TypeScript
const response = await client.license.mintLicenseTokens({
  licenseTermsId: "1", 
  licensorIpId: "0xC92EC2f4c86458AFee7DD9EB5d8c57920BfCD0Ba",
  receiver: "0x14dC79964da2C08b23698B3D3cc7Ca32193d9955", 
  amount: 1,
  maxMintingFee: BigInt(0), // disabled
  maxRevenueShare: 100, // default
  txOptions: { waitForTransaction: true }
});

console.log(`License Token minted at transaction hash ${response.txHash}, License IDs: ${response.licenseTokenIds}`)
```
```typescript Request Type
export type MintLicenseTokensRequest = {
  licensorIpId: Address;
  licenseTermsId: string | number | bigint;
  licenseTemplate?: Address;
  maxMintingFee: bigint | string | number;
  maxRevenueShare: number | string;
  amount?: number | string | bigint;
  receiver?: Address;
} & WithTxOptions & WithWipOptions;
```
```typescript Response Type
export type MintLicenseTokensResponse = {
  licenseTokenIds?: bigint[];
  receipt?: TransactionReceipt;
  txHash?: string;
  encodedTxData?: EncodedTxData;
};
```

### registerPILTerms

Registers new license terms and return the ID of the newly registered license terms.

| Method             | Type                                                                 |
| ------------------ | -------------------------------------------------------------------- |
| `registerPILTerms` | `(request: RegisterPILTermsRequest) => Promise<RegisterPILResponse>` |

Parameters:

* Expected Parameters: Instead of listing all of the expected parameters here, please see `LicenseTerms` type in [this](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/resources/license.ts) file. They all come from the [PIL Terms](doc:pil-terms).
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript TypeScript
import { LicenseTerms } from '@story-protocol/core-sdk';
import { zeroAddress } from 'viem';

const licenseTerms: LicenseTerms = {
  defaultMintingFee: 0n,
  currency: '0x1514000000000000000000000000000000000000', // insert $WIP address from https://docs.story.foundation/docs/deployed-smart-contracts
  royaltyPolicy: RoyaltyPolicyLAP, // insert RoyaltyPolicyLAP address from https://docs.story.foundation/docs/deployed-smart-contracts
  transferable: false,
  expiration: 0n,
  commercialUse: false,
  commercialAttribution: false,
  commercializerChecker: zeroAddress,
  commercializerCheckerData: '0x',
  commercialRevShare: 0,
  commercialRevCeiling: 0n,
  derivativesAllowed: false,
  derivativesAttribution: false,
  derivativesApproval: false,
  derivativesReciprocal: false,
  derivativeRevCeiling: 0n,
  uri: '',
}

const response = await client.license.registerPILTerms({ 
  ...licenseTerms, 
  txOptions: { waitForTransaction: true } 
})

console.log(`PIL Terms registered at transaction hash ${response.txHash}, License Terms ID: ${response.licenseTermsId}`) 
```
```typescript Request Type
export type RegisterPILTermsRequest = Omit<
  LicenseTerms,
  "defaultMintingFee" | "expiration" | "commercialRevCeiling" | "derivativeRevCeiling"
> & {
  defaultMintingFee: bigint | string | number;
  expiration: bigint | string | number;
  commercialRevCeiling: bigint | string | number;
  derivativeRevCeiling: bigint | string | number;
  txOptions?: TxOptions;
};

export type LicenseTerms = {
  /*Indicates whether the license is transferable or not.*/
  transferable: boolean;
  /*The address of the royalty policy contract which required to StoryProtocol in advance.*/
  royaltyPolicy: Address;
  /*The default minting fee to be paid when minting a license.*/
  defaultMintingFee: bigint;
  /*The expiration period of the license.*/
  expiration: bigint;
  /*Indicates whether the work can be used commercially or not.*/
  commercialUse: boolean;
  /*Whether attribution is required when reproducing the work commercially or not.*/
  commercialAttribution: boolean;
  /*Commercializers that are allowed to commercially exploit the work. If zero address, then no restrictions is enforced.*/
  commercializerChecker: Address;
  /*The data to be passed to the commercializer checker contract.*/
  commercializerCheckerData: Address;
  /**Percentage of revenue that must be shared with the licensor. Must be from 0-100.*/
  commercialRevShare: number;
  /*The maximum revenue that can be generated from the commercial use of the work.*/
  commercialRevCeiling: bigint;
  /*Indicates whether the licensee can create derivatives of his work or not.*/
  derivativesAllowed: boolean;
  /*Indicates whether attribution is required for derivatives of the work or not.*/
  derivativesAttribution: boolean;
  /*Indicates whether the licensor must approve derivatives of the work before they can be linked to the licensor IP ID or not.*/
  derivativesApproval: boolean;
  /*Indicates whether the licensee must license derivatives of the work under the same terms or not.*/
  derivativesReciprocal: boolean;
  /*The maximum revenue that can be generated from the derivative use of the work.*/
  derivativeRevCeiling: bigint;
  /*The ERC20 token to be used to pay the minting fee. the token must be registered in story protocol.*/
  currency: Address;
  /*The URI of the license terms, which can be used to fetch the offchain license terms.*/
  uri: string;
};
```
```typescript Response Type
export type RegisterPILResponse = {
  licenseTermsId?: bigint;
  txHash?: string;
  encodedTxData?: EncodedTxData;
};
```

### registerNonComSocialRemixingPIL

Convenient function to register a PIL non commercial social remix license to the registry.

> 🚧 No reason to call this function
>
> Non-Commercial Social Remixing terms are already registered with `licenseTermdId = 1` in our protocol. There's no reason to register them again.

| Method                            | Type                                                                                 |
| --------------------------------- | ------------------------------------------------------------------------------------ |
| `registerNonComSocialRemixingPIL` | `(request?: RegisterNonComSocialRemixingPILRequest) => Promise<RegisterPILResponse>` |

Parameters:

* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript TypeScript
const response = await client.license.registerNonComSocialRemixingPIL({
  txOptions: { waitForTransaction: true }
});

console.log(`PIL Terms registered at transaction hash ${response.txHash}, License Terms ID: ${response.licenseTermsId}`) 
```
```typescript Request Type
export type RegisterNonComSocialRemixingPILRequest = {
  txOptions?: TxOptions;
};
```
```typescript Response Type
export type RegisterPILResponse = {
  licenseTermsId?: bigint;
  txHash?: string;
  encodedTxData?: EncodedTxData;
};
```

### registerCommercialUsePIL

Convenient function to register a PIL commercial use license to the registry.

| Method                     | Type                                                                         |
| -------------------------- | ---------------------------------------------------------------------------- |
| `registerCommercialUsePIL` | `(request: RegisterCommercialUsePILRequest) => Promise<RegisterPILResponse>` |

Parameters:

* `request.defaultMintingFee`: The fee to be paid when minting a license.
* `request.currency`: The ERC20 token to be used to pay the minting fee and the token must be registered in story protocol.
* `request.royaltyPolicyAddress`: \[Optional] The address of the royalty policy contract, default value is LAP.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript TypeScript
const commercialUseParams = {
  currency: '0x1514000000000000000000000000000000000000', // insert $WIP address from https://docs.story.foundation/docs/deployed-smart-contracts
  defaultMintingFee: '10' // 10 of the currency (using the above currency, 10 $WIP)
}

const response = await client.license.registerCommercialUsePIL({
  ...commercialUseParams,
  txOptions: { waitForTransaction: true }
});

console.log(`PIL Terms registered at transaction hash ${response.txHash}, License Terms ID: ${response.licenseTermsId}`) 
```
```typescript Request Type
export type RegisterCommercialUsePILRequest = {
  defaultMintingFee: string | number | bigint;
  currency: Address;
  royaltyPolicyAddress?: Address;
  txOptions?: TxOptions;
};
```
```typescript Response Type
export type RegisterPILResponse = {
  licenseTermsId?: bigint;
  txHash?: string;
  encodedTxData?: EncodedTxData;
};
```

### registerCommercialRemixPIL

Convenient function to register a PIL commercial Remix license to the registry.

| Method                       | Type                                                                           |
| ---------------------------- | ------------------------------------------------------------------------------ |
| `registerCommercialRemixPIL` | `(request: RegisterCommercialRemixPILRequest) => Promise<RegisterPILResponse>` |

Parameters:

* `request.defaultMintingFee`: The fee to be paid when minting a license.
* `request.commercialRevShare`: Percentage of revenue that must be shared with the licensor.
* `request.currency`: The ERC20 token to be used to pay the minting fee and the token must be registered in story protocol.
* `request.royaltyPolicyAddress`: \[Optional] The address of the royalty policy contract, default value is LAP.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript TypeScript
const commercialRemixParams = {
  currency: '0x1514000000000000000000000000000000000000', // insert $WIP address from https://docs.story.foundation/docs/deployed-smart-contracts
  defaultMintingFee: '10', // 10 of the currency (using the above currency, 10 $WIP)
  commercialRevShare: 10 // 10%
}

const response = await client.license.registerCommercialRemixPIL({
  ...commercialRemixParams,
  txOptions: { waitForTransaction: true }
});

console.log(`PIL Terms registered at transaction hash ${response.txHash}, License Terms ID: ${response.licenseTermsId}`) 
```
```typescript Request Type
export type RegisterCommercialRemixPILRequest = {
  defaultMintingFee: string | number | bigint;
  commercialRevShare: number;
  currency: Address;
  royaltyPolicyAddress?: Address;
  txOptions?: TxOptions;
};
```
```typescript Response Type
export type RegisterPILResponse = {
  licenseTermsId?: bigint;
  txHash?: string;
  encodedTxData?: EncodedTxData;
};
```

### getLicenseTerms

Gets License Terms of the given ID.

| Method            | Type                                 |           |                                                       |
| :---------------- | :----------------------------------- | :-------- | :---------------------------------------------------- |
| `getLicenseTerms` | \`(selectedLicenseTermsId: string \\ | number \\ | bigint) => PiLicenseTemplateGetLicenseTermsResponse\` |

Parameters:

* `selectedLicenseTermsId`: The ID of the license terms.

```typescript Response Type
export type PiLicenseTemplateGetLicenseTermsResponse = {
  terms: {
    transferable: boolean;
    royaltyPolicy: Address;
    defaultMintingFee: bigint;
    expiration: bigint;
    commercialUse: boolean;
    commercialAttribution: boolean;
    commercializerChecker: Address;
    commercializerCheckerData: Hex;
    commercialRevShare: number;
    commercialRevCeiling: bigint;
    derivativesAllowed: boolean;
    derivativesAttribution: boolean;
    derivativesApproval: boolean;
    derivativesReciprocal: boolean;
    derivativeRevCeiling: bigint;
    currency: Address;
    uri: string;
  };
};
```

### predictMintingLicenseFee

Pre-compute the minting license fee for the given IP and license terms. The function can be used to calculate the minting license fee before minting license tokens.

| Method                     | Type                                                                                            |
| -------------------------- | ----------------------------------------------------------------------------------------------- |
| `predictMintingLicenseFee` | `(request: PredictMintingLicenseFeeRequest) => LicensingModulePredictMintingLicenseFeeResponse` |

Parameters:

* `request.licensorIpId`: The IP ID of the licensor.
* `request.licenseTermsId`: The ID of the license terms.
* `request.amount`: The amount of license tokens to mint.
* `request.licenseTemplate`: \[Optional] The address of the license template, default value is Programmable IP License.
* `request.receiver`: \[Optional] The address of the receiver, default value is your wallet address.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type LicensingModulePredictMintingLicenseFeeResponse = {
  currencyToken: Address;
  tokenAmount: bigint;
};
```

### setLicensingConfig

Sets the licensing configuration for a specific license terms of an IP. If both licenseTemplate and licenseTermsId are not specified then the licensing config apply to all licenses of given IP.

| Method               | Type                                                                 |
| -------------------- | -------------------------------------------------------------------- |
| `setLicensingConfig` | `(request: SetLicensingConfigRequest) => SetLicensingConfigResponse` |

Parameters:

* `request.ipId`: The address of the IP for which the configuration is being set.
* `request.licenseTermsId`: The ID of the license terms within the license template.
* `request.licenseTemplate`: The address of the license template used, If not specified, the configuration applies to all licenses.
* `request.licensingConfig`: The licensing configuration for the license.
  * `request.licensingConfig.isSet`: Whether the configuration is set or not.
  * `request.licensingConfig.mintingFee`: The minting fee to be paid when minting license tokens.
  * `request.licensingConfig.hookData`: The data to be used by the licensing hook.
  * `request.licensingConfig.licensingHook`: The hook contract address for the licensing module, or address(0) if none.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type SetLicensingConfigResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  success?: boolean;
};
```

# NFT Client
## NftClient

### Methods

* createNFTCollection

### createNFTCollection

Creates a new SPG NFT Collection.

| Method                | Type                                                                            |
| --------------------- | ------------------------------------------------------------------------------- |
| `createNFTCollection` | `(request: CreateNFTCollectionRequest) => Promise<CreateNFTCollectionResponse>` |

Parameters:

* `request.name`: The name of the collection.
* `request.symbol`: The symbol of the collection.
* `request.isPublicMinting`: If true, anyone can mint from the collection. If false, only the addresses with the minter role can mint.
* `request.mintOpen`: Whether the collection is open for minting on creation.
* `request.mintFeeRecipient`: The address to receive mint fees.
* `request.contractURI`: The contract URI for the collection. Follows ERC-7572 standard. See [here](https://eips.ethereum.org/EIPS/eip-7572).
* `request.baseURI`: \[Optional] The base URI for the collection. If baseURI is not empty, tokenURI will be either baseURI + token ID (if nftMetadataURI is empty) or baseURI + nftMetadataURI.
* `request.maxSupply`: \[Optional] The maximum supply of the collection.
* `request.mintFee`: \[Optional] The cost to mint a token.
* `request.mintFeeToken`: \[Optional] The token to mint.
* `request.owner`: \[Optional] The owner of the collection.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript TypeScript
import { zeroAddress } from 'viem'

// Create a new SPG NFT collection
//
// NOTE: Use this code to create a new SPG NFT collection. You can then use the
// `newCollection.spgNftContract` address as the `spgNftContract` argument in
// functions like `mintAndRegisterIpAssetWithPilTerms` in the
// `simpleMintAndRegisterSpg.ts` file.
//
// You will mostly only have to do this once. Once you get your nft contract address,
// you can use it in SPG functions.
//
const newCollection = await client.nftClient.createNFTCollection({
  name: 'Test NFT',
  symbol: 'TEST',
  isPublicMinting: true,
  mintOpen: true,
  mintFeeRecipient: zeroAddress,
  contractURI: '',
  txOptions: { waitForTransaction: true },
})

console.log(`New SPG NFT collection created at transaction hash ${newCollection.txHash}`)
console.log(`NFT contract address: ${newCollection.spgNftContract}`)
```
```typescript Request Type
export type CreateNFTCollectionRequest = {
  name: string;
  symbol: string;
  isPublicMinting: boolean;
  mintOpen: boolean;
  mintFeeRecipient: Address;
  contractURI: string;
  baseURI?: string;
  maxSupply?: number;
  mintFee?: bigint;
  mintFeeToken?: Hex;
  owner?: Hex;
  txOptions?: TxOptions;
};
```
```typescript Response Type
export type CreateNFTCollectionResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  spgNftContract?: Address; // the address of the newly created contract
};
```

# Welcome to Story Network
# Story Network (L1)

Welcome to the Hub for Story Network, the Story Chain.

This section is designed to help you understand the fundamentals of Story Network. We’ve structured the content into two parts:

1. Understanding the Architecture
2. Operating a Node

Story Network is a purpose-built Layer 1 blockchain that seamlessly integrates the best of both the Ethereum Virtual Machine (EVM) and Cosmos SDK. It offers full EVM compatibility while incorporating deep execution layer optimizations to efficiently support graph-based data structures. These optimizations make it particularly well-suited for handling complex intellectual property (IP) data structures in a cost-effective and scalable manner.

## Key Features

* **EVM Compatibility**: Full compatibility with Ethereum Virtual Machine
* **Optimized Data Structures**: Precompiled primitives for efficient IP graph traversal
* **Fast Finality**: CometBFT-based consensus layer for quick transaction finality
* **Modular Architecture**: Decoupled consensus from execution using Ethereum's Engine-API

## Documentation Sections

### Getting Started

* [Node Architecture](doc:story-node-structure)
* [Network Info](doc:network-info)
* [Whitepaper](https://www.story.foundation/whitepaper.pdf)

### Node Operations

* [Operating a node](doc:operating-a-node)
  * Full Node Setup
  * Archive Node Setup
  * Node Upgrade Guide
  * Release Notes

### Validation

* [Become a validator](doc:become-a-validator)
  * Validator Setup
  * Validator Operations

### Network Economics

* [Staking Design](doc:tokenomics-staking)
  * Token Economics
  * Staking Mechanisms
  * Rewards Structure

### Resources

* [Additional Resources](doc:additional-resources)
  * GitHub Repositories
  * SIP Repository
  * Community Forum
* [Troubleshooting](doc:network-faq)
  * Common Issues
  * Troubleshooting
  * Best Practices

## Network Information

The Story Network is currently available in multiple environments:

* Mainnet (Production)
* Aeneid (Testnet)
* Localnet (Development)

For detailed network information and connection details, please refer to the respective network documentation sections.

# Additional Resources

# Additional Resources

## Github

- [Story Github](https://github.com/piplabs/story)
- [Story-geth Github](https://github.com/piplabs/story-geth)

## SIP Repository

- [SIP Repository](https://github.com/storyprotocol/SIPs)

## Community Forum

- [Story Forum](https://forum.story.foundation/)

# Release Notes
This page provides information on the story execution and consensus client software release information. You may find execution client releases in [story-geth](https://github.com/piplabs/story-geth/releases) repo, and consensus client releases in [story](https://github.com/piplabs/story/releases) repo.

### Production releases

There are generally four types of releases:
* Major: It requires hardfork upgrade with a predefined upgrade height. Node operators need to upgrade before or on the height. The release will increase minor version number.
* Minor: It doesn't require hardfork upgrade. Node operators are required to upgrade binaries as soon as possible. The release will increase patch version number.
* Fix: It is an urgent fix. Node operators are required to upgrade binaries as soon as possible. The release will increase minor version or patch version number.
* Optional: It is an optional fix. Node operators can upgrade binaries based on needs. The release will increase patch version number.

Each release comes with a release note describing a list of new features or fixes. Released software binaries are also attached in the release note. We currently provide binaries supporting four types of systems: darwin-amd64, darwin-arm64, linux-amd64, and linux-arm64. You may also build your binaries using the commit hash in the release note.

### Release entries

Refer to the following release matrix to run nodes for Mainnet and Aeneid Testnet.

| Network | story-geth | story  |
|---------|------------|--------|
| Mainnet	| v1.0.1	   | v1.1.0 |
| Aeneid 	| v1.0.1	   | v1.1.0 |

# Operating Your Node
## **1. Setting Up a Geth Archive Node**
To run a Geth archive node, use `--gcmode=archive` instead of `--gcmode=full`. This ensures that Geth retains all historical blockchain state data, making it ideal for indexing services and blockchain analytics.

- `--syncmode=full`: Ensures a complete blockchain sync.
- `--gcmode=archive`: Retains full historical state data without pruning.

---

## **2. Enabling RPC (HTTP) and WebSocket in Geth**
### **HTTP (RPC) Options**
| Option | Description |
|--------|------------|
| `--http` | Enables the HTTP-RPC server. |
| `--http.addr=0.0.0.0` | Binds the HTTP server to all network interfaces. |
| `--http.port=8545` | Sets the HTTP-RPC port (default: 8545). |
| `--http.vhosts=*` | Allows requests from any domain (use with caution in production). |
| `--http.api=web3,eth,txpool,net,engine,debug` | Specifies the available APIs for HTTP requests. |

### **WebSocket (WS) Options**
| Option | Description |
|--------|------------|
| `--ws` | Enables the WebSocket server. |
| `--ws.addr=0.0.0.0` | Binds the WebSocket server to all network interfaces. |
| `--ws.port=8546` | Sets the WebSocket port (default: 8546). |
| `--ws.origins=*` | Allows WebSocket connections from any domain (use with caution in production). |
| `--ws.api=web3,eth,txpool,net,engine,debug` | Specifies the available APIs for WebSocket connections. |

These configurations ensure external applications can interact with the Geth node using both HTTP-RPC and WebSocket.

---

## **3. Monitoring Geth and Story Protocol**
### **Geth Monitoring Configuration**
- `--metrics`: Enables Prometheus-compatible metrics for Geth.
- `--metrics.addr=0.0.0.0`: Binds the metrics server to all interfaces.
- `--metrics.port=6060`: Exposes metrics on port `6060`.

### **Story Protocol Monitoring**
- Modify `config.toml` and set:
  ```toml
  prometheus = true
  ```
- The default Prometheus metrics port for Story Protocol is `26660`.

With these settings, both Geth and Story Protocol expose monitoring metrics that can be collected using Prometheus and visualized with Grafana.

# Node Upgrade
There are three types of upgrades

1. Upgrade the story geth client
2. Upgrade the story client manually
3. Schedule the upgrade with Cosmovisor

### Upgrade the story geth client

```bash
# Stop the services
sudo systemctl stop story
sudo systemctl stop story-geth

# Download the new binary
wget ${STORY_GETH_BINARY_URL}
sudo mv ./geth-linux-amd64 story-geth
sudo chmod +x story-geth
sudo mv ./story-geth $HOME/go/bin/story-geth
source $HOME/.bashrc

# Restart the service
sudo systemctl start story-geth
sudo systemctl start story
```

### Upgrade the story client manually

```bash
# Stop the service
sudo systemctl stop story

# Download the new binary
wget ${STORY_BINARY_URL}
sudo mv story-linux-amd64 story
sudo chmod +x story
sudo mv ./story $HOME/go/bin/story

# Schedule the update
sudo systemctl start story
```

### Schedule the upgrade with Cosmovisor

The following steps outline how to schedule an upgrade using Cosmovisor:

1. Create the upgrade directory and download the new binary

```bash
# Download the new binary
wget ${STORY_BINARY_URL}

# Schedule the upgrade
source $HOME/.bash_profile
cosmovisor add-upgrade ${UPGRADE_NAME} ${UPGRADE_PATH} \
  --force \
  --upgrade-height ${UPGRADE_HEIGHT}
```

2. Verify the upgrade configuration

```bash
# Check the upgrade info
cat $HOME/.story/data/upgrade-info.json
```

The upgrade-info.json should show:

```json
{
  "name": "v1.0.0",
  "time": "2025-02-05T12:00:00Z",
  "height": 858000
}
```

3. Monitor the upgrade

```bash
# Watch the node logs for the upgrade
journalctl -u story -f -o cat
```

Note: Cosmovisor will automatically handle the binary switch once the specified block height is reached. Before the upgrade, confirm that your node is fully synced and has enough disk space available.

# Full Node
This section will guide you through how to setup a Story node for mainnet. Story draws inspiration from ETH PoS in decoupling execution and consensus clients. The execution client `story-geth` relays EVM blocks into the `story` consensus client via Engine API, using an ABCI++ adapter to make EVM state compatible with that of CometBFT. With this architecture, consensus efficiency is no longer bottlenecked by execution transaction throughput.

The `story` and `geth` binaries, which make up the clients required for running Story nodes, are available from our latest `release` pages:

* **`story-geth`execution client:**
  * Release Link: [**Click here**](https://github.com/piplabs/story-geth/releases)
  * Latest Stable Binary (v1.0.1): [**Click here**](https://github.com/piplabs/story-geth/releases/tag/v1.0.1)
* **`story`consensus client:**
  * Releases link: [**Click here**](https://github.com/piplabs/story/releases)
  * Latest Stable Binary (v1.1.0): [**Click here**](https://github.com/piplabs/story/releases/tag/v1.1.0)

# Story Node Installation Guide

## Pre-Installation Checklist

* [ ] Verify system meets hardware requirements
* [ ] Operating system: Ubuntu 22.04 LTS
* [ ] Required ports are available
* [ ] Sufficient disk space available
* [ ] Root or sudo access

## Quick Reference

* Installation time: \~30 minutes
* Network: Story Mainnet or Story Aeneid Testnet
* Required versions:
  * Check Latest Release

## 1. System Preparation

### 1.1 System Requirements

For optimal performance and reliability, we recommend running your node on either:

* A Virtual Private Server (VPS)
* A dedicated Linux-based machine

### System Specs

| Hardware  | Minimal Requirement |
| --------- | ------------------- |
| CPU       | Dedicated 8 Cores   |
| RAM       | 32 GB               |
| Disk      | 500 GB NVMe Drive   |
| Bandwidth | 25 MBit/s           |

### 1.2 Required Ports

*Ensure all ports needed for your node functionality are needed, described below*

* `story-geth`
  * 8545
    * Required if you want your node to interface via JSON-RPC API over HTTP
  * 8546
    * Required for websockets interaction
  * 30303 (TCP + API)
    * MUST be open for p2p communication
* `story`
  * 26656
    * MUST be open for consensus p2p communication
  * 26657
    * Required if you want your node interfacing for Tendermint RPC
  * 26660
    * Needed if you want to expose prometheus metrics

### 1.3 Install Dependencies

```bash
# Update system
sudo apt update && sudo apt-get update

# Install required packages
sudo apt install -y \
  curl \
  git \
  make \
  jq \
  build-essential \
  gcc \
  unzip \
  wget \
  lz4 \
  aria2 \
  gh
```

### 1.4 Install Go

For Odyssey, we need to install Go 1.22.0

```bash
# Download and install Go 1.22.0
cd $HOME

# Set Go version
GO_VERSION="1.22.0"

# Download Go binary
wget "https://golang.org/dl/go${GO_VERSION}.linux-amd64.tar.gz"

# Remove existing Go installation and extract new version
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go${GO_VERSION}.linux-amd64.tar.gz"

# Clean up downloaded archive
rm "go${GO_VERSION}.linux-amd64.tar.gz"

# Add Go to PATH
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> ~/.bash_profile
source ~/.bash_profile

# Verify installation
go version
```

## 2. Story Node Installation

### 2.1 Install Story-Geth

1. Download and setup binary

```bash
cd $HOME
wget https://github.com/piplabs/story-geth/releases/download/v1.0.1/geth-linux-amd64
sudo mv ./geth-linux-amd64 story-geth
sudo chmod +x story-geth
sudo mv ./story-geth $HOME/go/bin/
source $HOME/.bashrc

# Verify installation
story-geth version
```

You will see the version of the geth binary.

```
Geth
version: 1.0.1-stable
...

```

(Mac OS X only) The OS X binaries have yet to be signed by our build process, so you may need to unquarantine them manually:

```bash
sudo xattr -rd com.apple.quarantine ./geth
```

2. Configure and start service

<Tabs>
  <Tab title="Mainnet">
    ```bash
           # Setup systemd service
    sudo tee /etc/systemd/system/story-geth.service > /dev/null <<EOF
    [Unit]
    Description=Story Geth Client
    After=network.target

    [Service]
    User=${user}
    ExecStart=${path_to_geth_binary} --story --syncmode full
    Restart=on-failure
    RestartSec=3
    LimitNOFILE=4096

    [Install]
    WantedBy=multi-user.target
    EOF

    # Start service
    sudo systemctl daemon-reload
    sudo systemctl enable story-geth
    sudo systemctl start story-geth

    # Verify service status
    sudo systemctl status story-geth
    ```
  </Tab>

  <Tab title="Aeneid testnet">
    ```bash
    # Setup systemd service
    sudo tee /etc/systemd/system/story-geth.service > /dev/null <<EOF
    [Unit]
    Description=Story Geth Client
    After=network.target

    [Service]
    User=${user}
    ExecStart=${path_to_geth_binary} --aeneid --syncmode full
    Restart=on-failure
    RestartSec=3
    LimitNOFILE=4096

    [Install]
    WantedBy=multi-user.target
    EOF

    # Start service
    sudo systemctl daemon-reload
    sudo systemctl enable story-geth
    sudo systemctl start story-geth

    # Verify service status
    sudo systemctl status story-geth
    ```
  </Tab>
</Tabs>

### 2.2 Install Story Consensus Client

#### Cosmovisor installation

For updating the story client, we recommend using Cosmovisor.

1. Install Cosmovisor

```bash
go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@v1.6.0
cosmovisor version
```

2. Configure Cosmovisor

```bash
# Set daemon configuration
export DAEMON_NAME=story
export DAEMON_HOME=$HOME/.story/story
export DAEMON_DATA_BACKUP_DIR=${DAEMON_HOME}/cosmovisor/backup
sudo mkdir -p \
  $DAEMON_HOME/cosmovisor/backup \
  $DAEMON_HOME/data


# Persist configuration
echo "export DAEMON_NAME=story" >> $HOME/.bash_profile
echo "export DAEMON_HOME=$HOME/.story/story" >> $HOME/.bash_profile
echo "export DAEMON_DATA_BACKUP_DIR=${DAEMON_HOME}/cosmovisor/backup" >> $HOME/.bash_profile
echo "export DAEMON_ALLOW_DOWNLOAD_BINARIES=false" >> $HOME/.bash_profile
```

#### Install Story Client

```bash
cd $HOME
wget https://github.com/piplabs/story/releases/download/v1.0.0/story-linux-amd64
sudo mv story-linux-amd64 story
sudo chmod +x story
sudo mv ./story $HOME/go/bin/
source $HOME/.bashrc
story version
```

> You should expect to see version 1.0.0-stable

(Mac OS X Only) The OS X binaries have yet to be signed by our build process, so you may need to unquarantine them manually:

```bash
sudo xattr -rd com.apple.quarantine ./story
```

#### Init Story with Cosmovisor

<Tabs>
  <Tab title="Mainnet">
    ```bash
    cosmovisor init ./story
    cosmovisor run init --network story --moniker ${moniker_name}
    cosmovisor version
    ```
  </Tab>

  <Tab title="Aeneid testnet">
    ```bash
    cosmovisor init ./story
    cosmovisor run init --network aeneid --moniker ${moniker_name}
    cosmovisor version
    ```
  </Tab>
</Tabs>

#### Custom Configuration

To override your own node settings, you can do the following:

* `${STORY_DATA_ROOT}/config/config.toml` can be modified to change network and consensus settings
* `${STORY_DATA_ROOT}/config/story.toml` to update various client configs
* `${STORY_DATA_ROOT}/priv_validator_key.json` is a sensitive file containing your validator key, but may be replaced with your own

#### Custom Automation

Below we list a sample `Systemd` configuration you may use on Linux

```bash
# story
sudo tee /etc/systemd/system/story.service > /dev/null <<EOF
[Unit]
Description=Story Cosmovisor
After=network.target

[Service]
Type=simple
User=$USER
Group=$GROUP
ExecStart=/usr/local/bin/cosmovisor run run \
--api-enable \
--api-address=0.0.0.0:1317
Restart=on-failure
RestartSec=5s
LimitNOFILE=65535
Environment="DAEMON_NAME=$DAEMON_NAME"
Environment="DAEMON_HOME=$DAEMON_HOME"
Environment="DAEMON_ALLOW_DOWNLOAD_BINARIES=false"
Environment="DAEMON_RESTART_AFTER_UPGRADE=true"
Environment="DAEMON_DATA_BACKUP_DIR=$DAEMON_HOME/cosmovisor/backup"
WorkingDirectory=$DAEMON_HOME

[Install]
WantedBy=multi-user.target
EOF

```

<Tabs>
  <Tab title="With Cosmovisor">
    ```bash
    # story
    sudo tee /etc/systemd/system/story.service > /dev/null <<EOF
    [Unit]
    Description=Story Cosmovisor
    After=network.target

    [Service]
    Type=simple
    User=${USER}
    Group=${GROUP}
    ExecStart=${path_to_story_binary} run run \
    --api-enable \
    --api-address=0.0.0.0:1317
    Restart=on-failure
    RestartSec=5s
    LimitNOFILE=65535
    Environment="DAEMON_NAME=$DAEMON_NAME"
    Environment="DAEMON_HOME=$DAEMON_HOME"
    Environment="DAEMON_ALLOW_DOWNLOAD_BINARIES=false"
    Environment="DAEMON_RESTART_AFTER_UPGRADE=true"
    Environment="DAEMON_DATA_BACKUP_DIR=$DAEMON_HOME/cosmovisor/backup"
    WorkingDirectory=$DAEMON_HOME

    [Install]
    WantedBy=multi-user.target
    EOF

    ```
  </Tab>

  <Tab title="Without Cosmovisor">
    ```bash
    # story
    sudo tee /etc/systemd/system/story.service > /dev/null <<EOF
    [Unit]
    Description=Story Cosmovisor
    After=network.target

    [Service]
    Type=simple
    User=${USER}
    Group=${GROUP}
    ExecStart=${path_to_story_binary} run
    Restart=on-failure
    RestartSec=5s
    LimitNOFILE=65535
    WorkingDirectory=$HOME/.story/story

    [Install]
    WantedBy=multi-user.target
    EOF

    ```
  </Tab>
</Tabs>

#### Start the service

```bash
sudo systemctl daemon-reload
sudo systemctl enable story
sudo systemctl start story

# Monitor logs
journalctl -u cosmovisor -f -o cat
```

#### Debugging

If you would like to check the status of `story` while it is running, it is helpful to query its internal JSONRPC/HTTP endpoint. Here are a few helpful commands to run:

* `curl localhost:26657/net_info | jq '.result.peers[].node_info.moniker'`
  * This will give you a list of consesus peers the node is sync'd with by moniker
* `curl localhost:26657/health`
  * This will let you know if the node is healthy - `{}` indicates it is

## 3. Verify Installation

### 3.1 Check Geth Status

```bash
# Check sync status
curl -X POST -H "Content-Type: application/json" \
  --data '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}' \
  http://localhost:8545

```

### 3.2 Check Consensus Client

```bash
# Check node status
curl localhost:26657/status

# Check peer connections
curl localhost:26657/net_info | jq '.result.peers[].node_info.moniker'
```

## Clean status

If you ever run into issues and would like to try joining the network from a cleared state, run the following:

### Geth

<Tabs>
  <Tab title="Mainnet">
    ```bash
    rm -rf ${GETH_DATA_ROOT} && ./geth --story --syncmode full
    ```

    Mac OS X: `rm -rf ~/Library/Story/geth/* && ./geth --story    --syncmode full`

    Linux: `rm -rf ~/.story/geth/* && ./geth --story --syncmode full`
  </Tab>

  <Tab title="Aeneid Testnet">
    ```bash
    rm -rf ${GETH_DATA_ROOT} && ./geth --aeneid --syncmode full
    ```

    Mac OS X: `rm -rf ~/Library/Story/geth/* && ./geth --aeneid    --syncmode full`

    Linux: `rm -rf ~/.story/geth/* && ./geth --aeneid --syncmode full`
  </Tab>
</Tabs>

### Story

<Tabs>
  <Tab title="Mainnet">
    ```bash
    rm -rf ${STORY_DATA_ROOT} && ./story init --network story && ./story run
    ```

    Mac OS X: `rm -rf ~/Library/Story/story/* && ./story init --network story && ./story run`

    Linux: `rm -rf ~/.story/story/* && ./story init --network story && ./story run`
  </Tab>

  <Tab title="Aeneid Testnet">
    ```bash
    rm -rf ${STORY_DATA_ROOT} && ./story init --network aeneid && ./story run
    ```

    Mac OS X: `rm -rf ~/Library/Story/story/* && ./story init --network aeneid && ./story run`

    Linux: `rm -rf ~/.story/story/* && ./story init --network aeneid && ./story run`
  </Tab>
</Tabs>

# Mainnet
# Resources

**Network Name**: Story Mainnet

**Chain ID**: 1514

**Chainlist Link**: [https://chainlist.org/chain/1514](https://chainlist.org/chain/1514)

## :link: RPCs

<Table align={["left","left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        RPC Name
      </th>

      <th style={{ textAlign: "left" }}>
        RPC URL
      </th>

      <th style={{ textAlign: "left" }}>
        Official
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        Story
      </td>

      <td style={{ textAlign: "left" }}>
        `https://mainnet.storyrpc.io`
      </td>

      <td style={{ textAlign: "left" }}>
        :white_check_mark:
      </td>
    </tr>
  </tbody>
</Table>

## :mag: Block Explorers

<Table align={["left","left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        Explorer
      </th>

      <th style={{ textAlign: "left" }}>
        URL
      </th>

      <th style={{ textAlign: "left" }}>
        Official
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        <a href="https://storyscan.xyz/" target="_blank">BlockScout Explorer ↗️</a>
      </td>

      <td style={{ textAlign: "left" }}>
        `https://storyscan.xyz/`
      </td>

      <td style={{ textAlign: "left" }}>
        :white_check_mark:
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        <a href="https://www.okx.com/web3/explorer/story" target="_blank">OKX Explorer ↗️</a>
      </td>

      <td style={{ textAlign: "left" }}>
        `https://www.okx.com/web3/explorer/story`
      </td>

      <td style={{ textAlign: "left" }}>

      </td>
    </tr>
  </tbody>
</Table>

## Staking Dashboard

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        Dashboard URL
      </th>

      <th style={{ textAlign: "left" }}>
        Official
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        [Story Dashboard](https://staking.story.foundation/)
      </td>

      <td style={{ textAlign: "left" }}>
        :white_check_mark:
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        [Origin Stake](https://ipworld.io/)
      </td>

      <td style={{ textAlign: "left" }}>

      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        [Node.Guru](https://story.explorers.guru/)
      </td>

      <td style={{ textAlign: "left" }}>

      </td>
    </tr>
  </tbody>
</Table>

## Contract deployment addresses

* [Proof of Creativity](doc:deployed-smart-contracts)

# Further Sections

* [Mainnet Status Page](https://status.story.foundation/)
* [Node Setup](doc:full-node)
* [Validator Operations](doc:validator-operations)
* [Staking Design](doc:tokenomics-staking)

# Network Info
# Overview

Story Network is a purpose-built layer 1 blockchain achieving the best of EVM and Cosmos SDK. It is 100% EVM-compatible alongside deep execution layer optimizations to support graph data structures, purpose-built for handling complex data structures like IP quickly and cost-efficiently. It does this by:

* using precompiled primitives to traverse complex data structures like IP graphs within seconds at marginal costs
* a consensus layer based on the mature CometBFT stack to ensure fast finality and cheap transactions
* a modular architecture that decouples consensus from execution via Ethereum’s Engine-API

# Available Network

<Cards columns={3}>
  <Card title="Mainnet" href="https://docs.story.foundation/docs/mainnet/" icon="fa-home" target="_blank" />

  <Card title="Aeneid Testnet" href="https://docs.story.foundation/docs/aeneid/" icon="fa-wrench" target="_blank" />

  <Card title="Run a localnet" href="https://docs.story.foundation/docs/localnet/" icon="fa-cog" target="_blank" />
</Cards>

# Run a Localnet
# Overview

You can easily set up your own local Story network using docker compose, consisting of one boot node and four validator nodes. With this local network, you can test the consensus layer of the Story network or deploy your application using the precompiled primitive, the IP graph, to conduct various tests. Additionally, you can reset the network at any time as needed.

# Run a Local Story Network

> For more detailed information for running Story local network, please refer the repository:\
> [https://github.com/piplabs/story-localnet](https://github.com/piplabs/story-localnet)

## Prerequisite

To set up a local network, [Docker](https://docs.docker.com/get-started/get-docker/) is required.

## Step 1 - Start Docker

Please run Docker.

## Step 2 - Clone Repository

You need to clone three repositories: `story`, `story-geth`, and `story-localnet`.\
Make sure all three repositories are located within the same subfolder.

```bash
# clone repositories
git clone https://github.com/piplabs/story.git
git clone https://github.com/piplabs/story-geth.git
git clone https://github.com/piplabs/story-localnet.git
```

## Step 3 - Start Nodes

Navigate to story-localnet project and start the local network.

```bash
# move to story-localnet
cd story-localnet

# start story local network
./start.sh
```

## Step 4 - Terminate Nodes

If you want to stop the Story local network, you can do so by executing the script below.

```bash
# terminate story local network
./terminate.sh 
```

***

## How to Allocate Token to Your Account from Genesis

You may need to allocate IP tokens to your account for testing in the local network.\
To allocate tokens to your account in the genesis block, follow these steps:

1. Add your account information to the alloc section in `config/story/genesis-geth.json`:

```json
"<hex-encoded-account-address>": {
  "nonce": "0x0",
  "balance": "<hex-encoded-balance>",
  "code": "0x",
  "storage": {}
}
```

2. Run the `update-genesis-hash.sh` script to update the genesis block hash:

```bash
./update-genesis-hash.sh
```

***

## How to interact with Story Local Network

By default, the Story local network has the following ports open for interaction.

| **Port**  | **Service** | **Role**                                                                |
| --------- | ----------- | ----------------------------------------------------------------------- |
| **8545**  | story-geth  | endpoint of RPC server for Story execution client.                      |
| **1317**  | story-node  | endpoint of API server for interacting with the Story consensus client. |
| **26657** | story-node  | endpoint of cosmos-sdk RPC server for Story consensus client.           |

***

## Monitoring Systems

This setup includes a monitoring stack to provide centralized metrics and logs\
visualization for the blockchain network. Tools include **Prometheus**,
**Loki**, **Promtail**, and **Grafana**, all integrated through Docker Compose.

### **Components and Access Information**

| **Service**    | **Role**                                                           | **Default Port**               | **Access URL**          |
| -------------- | ------------------------------------------------------------------ | ------------------------------ | ----------------------- |
| **Prometheus** | Collects metrics from nodes and itself for performance monitoring. | `9090`                         | `http://localhost:9090` |
| **Loki**       | Aggregates and stores logs from the network nodes via Promtail.    | `3100`                         | `http://localhost:3100` |
| **Promtail**   | Scrapes logs from Docker containers and sends them to Loki.        | `9080` (API), `9095` (Metrics) | `http://localhost:9080` |
| **Grafana**    | Provides a dashboard interface for metrics and logs visualization. | `3000`                         | `http://localhost:3000` |

# Aeneid - Testnet
# Resources

**Network Name**: Story Aeneid Testnet

**Chain ID**: 1315

## :link: RPCs

<Table align={["left","left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        RPC Name
      </th>

      <th style={{ textAlign: "left" }}>
        RPC URL
      </th>

      <th style={{ textAlign: "left" }}>
        Official
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        Story
      </td>

      <td style={{ textAlign: "left" }}>
        `https://aeneid.storyrpc.io`
      </td>

      <td style={{ textAlign: "left" }}>
        :white_check_mark:
      </td>
    </tr>
  </tbody>
</Table>

## :mag: Explorers

<Table align={["left","left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        Explorer
      </th>

      <th style={{ textAlign: "left" }}>
        URL
      </th>

      <th style={{ textAlign: "left" }}>
        Official
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        <a href="https://aeneid.storyscan.xyz/" target="_blank">Blockscout Explorer ↗️</a>
      </td>

      <td style={{ textAlign: "left" }}>
        `https://aeneid.storyscan.xyz`
      </td>

      <td style={{ textAlign: "left" }}>
        :white_check_mark:
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        <a href="https://explorer.story.foundation/" target="_blank">IP Explorer ↗️</a> (only for IP-related actions like licensing, minting licenses, etc)
      </td>

      <td style={{ textAlign: "left" }}>
        `https://explorer.story.foundation`
      </td>

      <td style={{ textAlign: "left" }}>
        ✅
      </td>
    </tr>
  </tbody>
</Table>

## Faucet

| Faucet                                                                               | Amount |
| :----------------------------------------------------------------------------------- | :----- |
| [Google Cloud Faucet](https://cloud.google.com/application/web3/faucet/story/aeneid) | 1 IP   |

## Staking Dashboard

| Dashboard URL                                               | Official |
| :---------------------------------------------------------- | :------- |
| [Story Dashboard](https://aeneid.staking.story.foundation/) | ✅        |

## Contract deployment addresses

* [Proof of Creativity](doc:deployed-smart-contracts)

# Engine API
The Engine API is a collection of JSON-RPC methods that facilitate communication between the execution layer (EL) and the consensus layer (CL) of an EVM node. Story's execution layer, which offers full EVM compatibility, supports all standard JSON-RPC methods defined by the [Ethereum Engine API](https://github.com/ethereum/execution-apis/blob/main/src/engine/common.md). Meanwhile, Story's consensus layer, built on Cosmos modules, utilizes the Engine API to coordinate with the execution layer.

## Functionalities

The Engine API facilitates seamless interaction between the EL and the CL by providing essential coordination mechanisms, including:

* **Handshake**
* **Synchronization**
* **Block Validation**
* **Block Proposal**

## Execution Layer Implementation

The EL in Story implements the following standard Engine API methods to support these functionalities:

* `engine_exchangeCapabilities`: Exchanges supported methods.
* `engine_getClientVersion`: Exchanges client version data.
* `engine_newPayload`: Inserts the given payload into the local chain.
* `engine_forkchoiceUpdate`: Updates the canonical chain marker and generates the payload with given attributes.
* `engine_getPayload`: Retrieves the pre-generated payload.

## Consensus Layer Interaction

How does Story's Consensus Layer (CL) interact with these methods? The answer lies in CometBFT ABCI++.

CometBFT is a state machine replication engine which provides consensus and security for Cosmos modules. ABCI++, also known as ABCI 2.0, is the interface between CometBFT and the actual state machine being replicated(i.e. EL's state machine).

ABCI++ comprises of a set of methods that interact with the Engine API, as outlined below:

### **1. PrepareProposal** (Proposing a New Block)

* The CL checks whether a payload is already being generated using `payloadID`.
* If not, the CL calls `engine_forkchoiceUpdate` to trigger a new payload generation.
* The CL then calls `engine_getPayload` with `payloadID` to fetch the payload and propose a new block.

### \*\*2. ProcessProposal (Processing a New Block)

* The CL calls `engine_newPayload` to  delivers the new block to the EL.
* The EL validates payload of the new block, executes transactions deterministically and updates its state.

### **3. FinalizeBlock** (Finalizing a Decided Block)

* The CL calls `engine_newPayload` to  delivers the finalized block to the EL.
* If the block has not yet been incorporated into the EL, the EL validates payload of the new block, executes transactions deterministically and updates its state.
* Since CometBFT provides instant finality, the CL calls `engine_forkchoiceUpdate` to finalize the block.
* Finally, the CL calls `engine_forkchoiceUpdate` again, with extra attributes,  to start an optimistic build of the next block if enabled, and if the validator is the next proposer.

This interaction ensures smooth coordination between the EL and the CL, maintaining the integrity and efficiency of Story's blockchain network.

# mint
## Contents

1. [Contents](#contents)
2. [State](#state)
3. [Begin Block](#begin-block)
4. [Parameters](#parameters)
5. [Events](#events)

## State

### Params

* Params: `mint/params -> legacy_amino(params)`

```protobuf protobuf
message Params {
  option (amino.name) = "client/x/mint/Params";

  // type of coin to mint
  string mint_denom = 1;
  // inflation amount per year
  string inflations_per_year = 2 [
    (cosmos_proto.scalar)  = "cosmos.Dec",
    (gogoproto.customtype) = "cosmossdk.io/math.LegacyDec",
    (gogoproto.nullable)   = false
  ];
  // expected blocks per year
  uint64 blocks_per_year = 3;
}
```

## Begin Block

Minting parameters are calculated and inflation paid at the beginning of each block.

### Inflation amount calculation

Inflation amount is calculated using an "inflation calculation function" that's\
passed to the `NewAppModule` function. If no function is passed, then the SDK's
default inflation function will be used (`DefaultInflationCalculationFn`). In case a custom
inflation calculation logic is needed, this can be achieved by defining and
passing a function that matches `InflationCalculationFn`'s signature.

```go
type InflationCalculationFn func(ctx sdk.Context, minter Minter, params Params, bondedRatio math.LegacyDec) math.LegacyDec
```

## Parameters

The minting module contains the following parameters:

| Key               | Type            | Example             |
| ----------------- | --------------- | ------------------- |
| MintDenom         | string          | "stake"             |
| InflationsPerYear | string (dec)    | "20000000000000000" |
| BlocksPerYear     | string (uint64) | "10368000"          |

* `MintDenom` is the coin denominator used.
* `InflationsPerYear` is the target inflation per year, in 1e18 decimals.
* `BlocksPerYear` is the target number of blocks per year.

## Events

The minting module emits the following events:

### BeginBlocker

| Type | Attribute Key | Attribute Value |
| :--- | :------------ | :-------------- |
| mint | amount        | "1000"          |

# evmengine
## Abstract

This document specifies the internal `x/evmengine` module of the Story blockchain.

As Story Network separates the consensus and execution client, like Ethereum, the consensus client (CL) and execution client (EL) needs to communicate to sync to the network, propose proper EVM blocks, and execute EVM-triggered EL actions in CL.

The module exists to facilitate all communications between CL and EL using the [Engine API](engine-api), from staking and upgrades to driving block production and consensus in CL and EL.

## Contents

1. **[State](#state)**
2. **[Prepare Proposal](#prepare-proposal)**
3. **[Process Proposal](#process-proposal)**
4. **[Post Finalize](#post-finalize)**
5. **[Messages](#messages)**
6. **[UBI](#ubi)**
7. **[Upgrades](#upgrades)**

## State

### Build Delay

Type: `time.Duration`

Build delay determines the wait duration from the start of `PrepareProposal` ABCI2 call before fetching the next EVM block data to propose from EL via the [Engine API](engine-api). Applicable to the current proposer only. If the node has a block optimistically built beforehand, the build delay is not used.

### Build Optimistic

Type: `bool`

Enable optimistic building of a block if true. A node will deterministically build the next block if it finds itself as the next proposer in the current block. Optimistic building starts with requesting the next EVM block data (for the next CL block) immediately after the `FinalizeBlock` of ABCI2.

### Head Table

Type: `ExecutionHeadTable`

Head table stores the latest execution head data to be used for partial validation of EVM blocks received from other validators. When the chain initializes, the execution head is populated with the genesis execution hash loaded from `genesis.json`.

The following execution head is stored in the table.

```protobuf protobuf
message ExecutionHead {
  option (cosmos.orm.v1.table) = {
    id: 1;
    primary_key: { fields: "id", auto_increment: true }
  };

  uint64 id               = 1; // Auto-incremented ID (always and only 1).
  uint64 created_height   = 2; // Consensus chain height this execution block was created in.
  uint64 block_height     = 3; // Execution block height.
  bytes  block_hash       = 4; // Execution block hash.
  uint64 block_time       = 5; // Execution block time.
}
```

### Upgrade Contract

Type: `*bindings.UpgradeEntrypoint`

Upgrade contract is used to filter and parse upgrade-related events from EL.

### UBI Contract

Type: `*bindings.UBIPool`

UBI contract is used to filter and parse UBI-related events from EL.

### Mutable Payload

Type: struct

Mutable payload stores the optimistic block built, if optimistic building is enabled.

#### Genesis State

The module's `GenesisState` defines the state necessary for initializing the chain from a previously exported height.

```protobuf protobuf
message GenesisState {
  Params params = 1 [(gogoproto.nullable) = false];
}

message Params {
  bytes execution_block_hash = 1 [
    (gogoproto.moretags) = "yaml:\"execution_block_hash\""
  ];
}
```

## Prepare Proposal

At each block, if the node is the proposer, ABCI2 triggers `PrepareProposal` which

1. Loads staking & reward withdrawals from the [evmstaking](./evmstaking-module) module.
2. Builds a valid EVM block.
   * If optimistic building: loads the optimistically built block.
   * Non-optimistic: requests and retrieves an EVM block from EL.
3. Collects the EVM logs of the previous/parent block.
4. Assembles `MsgExecutionPayload` with the built EVM block and previous EVM logs.
5. Returns a transaction containing the assembled `MsgExecutionPayload` data.

This CL block is then propagated to all other validators.

## Process Proposal

At each block, if the node is not a proposer but a validator, ABCI2 triggers `ProcessProposal` with received commits (which should be a transaction of `MsgExecutionPayload` data in the honest case).

The node first validates that the received commit has only one transaction with at least 2/3 of votes committed. Then, the node validates that the one transaction only contains one unmarshalled `MsgExecutionPayload` data. Finally, the node processes the received data and broadcasts its acceptance of the proposal to the network. If any of the validation or processing fails, the node rejects the proposal.

More specifically, the node processes the received `MsgExecutionPayload` data in the following manner:

1. Validates the fields of the received `MsgExecutionPayload` (outlined in [Messages](#msgexecutionpayload)).
2. Compare local stake & reward withdrawals with the received withdrawals data.
3. Push the received execution payload to EL via the Engine API and wait for payload validation.
4. Update the EL forkchoice to the execution payload's block hash.
5. Process staking events using the [evmstaking](./evmstaking-module) module.
6. Process upgrade events.
7. Update the execution head to the execution payload (finalized block).

## Post Finalize

If optimistic building is enabled, `PostFinalize` is triggered immediately after `FinalizeBlock` set through custom ABCI callback. During this process, the node peeks the staking and reward queues from the evmstaking module, and builds a new execution payload on top of the current execution head. It sets the optimistic block to be used in the next block's `PrepareProposal` phase and returns the response from the forkchoice update.

## Messages

In this section we describe the processing of the evmengine messages and the corresponding updates to the state. All created/modified state objects specified by each message are defined within the state section.

### MsgExecutionPayload

```protobuf protobuf
message MsgExecutionPayload {
  option (cosmos.msg.v1.signer) = "authority";
  string            authority           = 1;
  bytes             execution_payload   = 2;
  repeated EVMEvent prev_payload_events = 3;
}

message EVMEvent {
  bytes          address = 1;
  repeated bytes topics  = 2;
  bytes          data    = 3;
  bytes          tx_hash = 4;
}
```

This message is expected to fail if:

* authority is invalid (not evmengine authority)
* execution payload fails to unmarshal to [ExecutableData](https://github.com/piplabs/story/blob/c38b80c13579d3df7174ea10c3368ef0692f52da/client/x/evmengine/types/executable_data.go#L17-L35) for reasons such as invalid fields
* execution payload's block number does not match CL head's block number + 1
* execution payload's block parent hash does not match CL head's hash
* execution payload's timestamp is invalid
* execution payload's RANDAO does not match CL head's hash (ie. parent hash)
* execution payload's `Withdrawals`, `BlobGasUsed`, and `ExcessBlobGas` fields are nil
* execution payload's `Withdrawals` count does not match local node's sum of dequeued stake & reward withdrawals

The message must contain previous block's events, which gets processed at the current CL block (in other words, execution events from EL block n-1 are processed at CL block n). In the future, the message will remove `prev_payload_events` and rely on [Engine API](engine-api) to get the current finalized EL block's events.

Also note that EVM events are processed in CL in the order they are generated in EL.

## UBI

All UBI-related changes must be triggered from the canonical UBI contract in the EVM execution layer. This module handles the execution handling of those triggers in CL. Read more about [UBI for validators](https://docs.story.foundation/docs/tokenomics-staking#ubi-for-validators)

### Set UBI Distribution

The `UBIPool` contract emits the UBI distribution set event, which is parsed by the module to set the UBI percentage in the distribution module.

## Upgrades

All chain upgrade-related logics must be triggered from the canonical upgrade contract in the EVM execution layer. This module handles the execution handling of those triggers in CL.

### Software Upgrade

The `UpgradeEntrypoint` contract emits the software upgrade event, which is parsed by the module to schedule an upgrade at a given height for a given binary name. Currently, all upgrades must either be set via forks or by the software upgrade events; the latter process is a multisig-controlled process, which will transition into a voting-based process in the future.

### Cancel Upgrade

Similar to the software upgrade, the module processes the cancel upgrade event from EVM logs of the previous block, and clears an existing upgrade plan.

# staking
## Abstrat

The staking module has been modified to accommodate for the following changes below. Refer to the Cosmos SDK's [staking module docs](https://docs.cosmos.network/main/build/modules/staking) for more information.

## Reward Multiplier

### Validators

Validators can choose to accept either locked tokens or unlocked tokens as delegations. Validators for locked tokens are conditioned to half the inflation allocation of validators for unlocked tokens.

Since each validator receives different inflation distribution based on delegations, the inflation distribution I<sub>v<sub>i</sub></sub> for validator v<sub>i</sub> in the rewards pool is calculated as follows:

<Image align="center" src="https://files.readme.io/3ee4914a7cc6036ceebbdd31ce93e525984a08364f8c3ab2152b86b3bcd5df7e-Screenshot_2025-02-11_at_8.30.07_AM.png" />

where

* I<sub>v<sub>i</sub></sub> is the total inflationary token rewards for v<sub>i</sub>
* S<sub>v<sub>i</sub></sub> is the staked tokens for v<sub>i</sub>
* M<sub>v<sub>i</sub></sub> is the rewards multiplier for v<sub>i</sub>
  * 0.5 for locked tokens
  * 1 for unlocked tokens
* R<sub>n</sub> is the total inflationary tokens allocated for the rewards pool in block n, calculated in the [mint](./mint-module.md) module

### Delegations

Delegators can delegate with four different staking lock times, which results in different staking reward multiplier for each delegation (delegator-validator pair of stakes). The inflation distribution for each delegation D<sub>i</sub> is calculated as follows:

<Image align="center" src="https://files.readme.io/002ae69aa3b3e52a33747452fe0c0b91b9120f20155deb19b56fb7917132b8de-Screenshot_2025-02-11_at_8.34.44_AM.png" />

where

* S<sub>d<sub>i</sub></sub> is the staked tokens of delegation d<sub>i</sub> on validator v<sub>d</sub>
* M<sub>d<sub>i</sub></sub> is the rewards multiplier of d<sub>i</sub> on v<sub>d</sub>
* I<sub>v</sub> is the total inflationary token rewards for v<sub>d</sub>
* C<sub>v</sub> is the commission rate for v<sub>d</sub>

#### Time-weighted Reward Multiplier M<sub>d<sub>i</sub></sub>

* *Flexible* (no lockup): 1
* *Short* (90 days): 1.1
* *Medium* (360 days): 1.5
* *Long* (540 days): 2.0

# evmstaking
## Abstract

This document specifies the internal `x/evmstaking` module of the Story blockchain.

In Story blockchain, the gas token resides on the execution layer (EL) to pay for transactions and interact with smart contracts. However, the consensus layer (CL) manages the consensus staking, slashing, and rewarding. This module exists to facilitate CL-level staking-related logic, such as delegating to validators with custom lock periods.

## Contents

1. **[State](#state)**
2. **[Two Queue System](#two-queue-system)**
3. **[Withdrawal Queue Content](#withdrawal-queue-content)**
4. **[End Block](#end-block)**
5. **[Processing Staking Events](#processing-staking-events)**
6. **[Withdrawing Delegations](#withdrawing-delegations)**
7. **[Withdrawing Rewards](#withdrawing-rewards)**
8. **[Withdrawing UBI](#withdrawing-ubi)**

## State

### Withdrawal Queue

Type: `Queue[types.Withdrawal]`

The (stake) withdrawal queue stores the pending unbonded stakes to be burned on CL and minted on EL. Stakes that are unbonded after 14 days of unstaking period are added to the queue to be processed.

### Reward Withdrawal Queue

Type: `Queue[types.Withdrawal]`

The reward withdrawal queue stores the pending rewards from stakes to be burned on CL and minted on EL. All rewards above a threshold are eligible to be queued in this queue, but there exists a parameter of maximum additions per block.

### Parameters

```protobuf protobuf
message Params {
  uint32 max_withdrawal_per_block = 1 [
    (gogoproto.moretags) = "yaml:\"max_withdrawal_per_block\""
  ];
  uint32 max_sweep_per_block = 2 [
    (gogoproto.moretags) = "yaml:\"max_sweep_per_block\""
  ];
  uint64 min_partial_withdrawal_amount = 3 [
    (gogoproto.moretags) = "yaml:\"min_partial_withdrawal_amount\""
  ];
  string ubi_withdraw_address = 4 [
    (gogoproto.moretags) = "yaml:\"ubi_withdraw_address\""
  ];
}
```

* `max_withdrawal_per_block` is the maximum number of withdrawals (reward and unstakes, each) to process per block. This parameter prevents nodes from processing a large amount of withdrawals at once, which could exceed the max chain timeout.
* `max_sweep_per_block` is the maximum number of validator-delegator delegations to sweep per block. This parameter prevents nodes from processing a large amount of delegations at once.
* `min_partial_withdrawal_amount` is the minimum amount required for rewards to get added to the reward withdrawal queue.
* `ubi_withdrawal_address` is the UBI contract address to which UBI withdrawals should be deposited.

### Delegator Withdraw Address

Type: `Map[string, string]`

The delegator-withdraw address mapping tracks the address to which a delegator receives their withdrawn stakes. The (stake) withdrawal queue uses this map to determine the `execution_address` in the `Withdrawal` struct used in building an EVM block payload.

While the delegator can change the withdraw address at any time, existing stake withdraw requests in the (stake) withdrawal queue will maintain their original values.

### Delegator Reward Address

The delegator-reward address mapping tracks the address to which a delegator receives their reward stakes, similar to the delegator-withdraw mapping.

While the delegator can change the reward address at any time, existing reward withdraw requests in the reward withdrawal queue will maintain their original values.

Type: `Map[string, string]`

### Delegator Operator Address

Type: `Map[string, string]`

The delegator-operator address mapping tracks the address to which a delegator has given the privilege to delegate (stake), undelegate (unstake), and redelegate on behalf of themselves.

### IP Token Staking Contract

Type: `*bindings.IPTokenStaking`

IPTokenStaking contract is used to filter and parse staking-related events from EL.

## Two Queue System

The module departs from traditional Cosmos SDK staking module's unstaking system, where all unbonded entries (stakes that have unbonded after 14 days of unbonding period) are immediately distributed into delegators account. Instead, Story's unstaking system assimilates Ethereum 2.0's unstaking system, where 16 full or partial (reward) withdrawals are processed per slot.

In a single queue of withdrawals, reward withdrawals can significantly delay stake withdrawals. Hence, Story blockchain implements a two-queue system where a max amount to process per block is enforced per queue. In other words, the stake/ubi withdrawal and reward withdrawal queues can each process the max parameter per block.

## Withdrawal Queue Content

Since the module only processes unstakes/rewards/ubi and stores them in queues, the actual dequeueing for withdrawal to the execution layer is carried out in the [evmengine](./evmengine-module) module. More specifically, a proposer dequeues the max number of withdrawals from each queue and adds them to the EVM block payload, which gets executed by EL via the [Engine API](engine-api). When validators receive proposed block payload from the proposer, they individually peek the local queues and compare them against the received block's withdrawals. Mismatching withdrawals indicate non-determinism in staking logics and should result in chain halt.

In other words, the `evmstaking` module is in charge of parsing, processing, and inserting withdrawal requests to two queues, while the `evmengine` module is in charge of validating and dequeuing withdrawal requests, as well as depositing them to corresponding withdrawal addresses in EL.

## End Block

The `EndBlock` ABCI2 call is responsible for fetching the unbonded entries (stakes that have unbonded after 14 days) from the [staking](./staking-module) module and inserting them into the (stake) withdrawal queue. Furthermore, it processes stake reward withdrawals into the reward withdrawal queue and UBI withdrawals into the (stake) withdrawal queue.

If the network is in the [Singularity period](tokenomics-staking#singularity), the End Block is skipped as there are no staking rewards and withdrawals available during this period. Otherwise, refer to [Withdrawing Delegations](#withdrawing-delegations) and [Withdrawing Rewards](#withdrawing-rewards) for detailed withdrawal processes.

## Processing Staking Events

The module parses and processes staking events emitted from the [IPTokenStaking contract](https://github.com/piplabs/story/blob/main/contracts/src/protocol/IPTokenStaking.sol), which are collected by the [evmengine](./evmengine-module) module. The list of events are:

### Staking events

* Create Validator
* Deposit (delegate)
* Withdraw (undelegate)
* Redelegate
* Unjail: anyone can request to unjail a jailed validator by paying the unjail fee in the contract.

These operations incur a fixed gas cost to prevent spam.

### Parameter events

* Update Validator Commission: update the validator commission.
* Set Withdrawal Address: delegator can modify their withdrawal address for future unstakes/undelegations.
* Set Reward Address: delegator can modify their withdrawal address for future reward emissions.
* Set Operator: delegator can modify their operator with privileges of delegation, undelegation, and redelegation.
* Unset Operator: delegator can remove operator.

These operations incur a fixed gas cost to prevent spam.

## Withdrawal

Both withdrawal queues hold withdrawals of type:

```protobuf protobuf
message Withdrawal {
  option (gogoproto.equal) = true;
  option (gogoproto.goproto_getters) = false;

  uint64 creation_height = 1;
  string execution_address = 2 [
    (cosmos_proto.scalar) = "cosmos.AddressString",
    (gogoproto.moretags) = "yaml:\"execution_address\""
  ];
  uint64 amount = 3 [
    (gogoproto.moretags) = "yaml:\"amount\""
  ];
  WithdrawalType withdrawal_type = 4 [
    (gogoproto.moretags) = "yaml:\"withdrawal_type\""
  ];
  string validator_address = 5 [
    (gogoproto.moretags) = "yaml:\"validator_address\""
  ];
}
```

* `creation_height` is the block height at which the withdrawal is created.
* `execution_address` is the EVM address receiving the withdrawn fund, which is burned in CL.
* `amount` is the amount to burn on CL and mint on EL.
* `withdrawal_type` is the type of withdrawal: $0$ for unstakes, $1$ for reward, and $2$ for UBI.
* `validator_address` is the EVM validator address.

### Withdrawing Delegations

Delegations that have unbonded after 14 days of unbonding period (ie. unbonded entries) gets added to the (stake) withdrawal queue at the end of each block. If validator is totally-unstaked, ie. all delegations and self-delegations are unbonded, then validator's commission is also withdrawn.

### Withdrawing Rewards

Inflation rewards allocated to delegations are auto-swept at the end of each block. If a delegation's accrued reward is greater than the parameterized threshold, the reward is added to the reward withdrawal queue to be credited to the delegator's EVM reward address.

# List of Modules
# List of Modules

Here is a list of all production-grade modules that can be used on the Story blockchain, along with their respective documentation:

* [evmengine](./evmengine-module) - Handles Cosmos-side logics on each EVM state transition via the [Engine API](engine-api).
* [evmstaking](./evmstaking-module) - Handles staking and network emission logics with queues.
* [mint](./mint-module)

## Cosmos SDK (modified)

Story network uses the following Cosmos SDK modules with some modifications:

* [staking](./staking-module)
* [distribution](https://docs.cosmos.network/main/build/modules/distribution)

## Cosmos SDK (unmodified)

Story network uses the following Cosmos SDK modules without non-trivial modifications:

* [auth](https://docs.cosmos.network/main/build/modules/auth)
* [bank](https://docs.cosmos.network/main/build/modules/bank)
* [consensusparams](https://docs.cosmos.network/main/build/modules/consensus)
* [gov](https://docs.cosmos.network/main/build/modules/gov)
* [slashing](https://docs.cosmos.network/main/build/modules/slashing)
* [upgrade](https://docs.cosmos.network/main/build/modules/upgrade)

# Node Architecture

Story is a purpose-built modular blockchain fully EVM compatible using Cosmos SDK and CometBFT to achieve fast block time and one-shot finality. A Story node consists of two clients: `story-geth` as the execution client (EL) and a `story` as the consensus client (CL). These clients communicate via the [Engine API interface](doc:engine-api) defined by [Ethereum](https://hackmd.io/@danielrachi/engine_api).

`story-geth` is a fork of the Geth client, with the addition of the [IPGraph Precompile](doc:precompile) and [RIP-7212](https://github.com/ethereum/RIPs/blob/master/RIPS/rip-7212.md) precompile. It handles transaction execution, broadcasting and state storage while maintaining full compatibility with the Ethereum Virtual Machine (EVM) and supporting all Ethereum JSON-RPC methods.

`story` is built on the Cosmos SDK and CometBFT. The Cosmos SDK provides a modular framework for building blockchain applications, enabling seamless integration of new modules and features while allowing the network to be easily extended and customized. `story` client introduces upgrades and additional Cosmos SDK modules to support Engine API integration and novel staking mechanisms. CometBFT, a high-performance, scalable, and secure consensus engine, has been extensively tested within the Cosmos ecosystem. CometBFT and Cosmos SDK communicate through ABCI++ interface(link to ABCI++ spec).

<Image align="center" src="https://files.readme.io/12b850eac8fcdf10ebb8d2ed23f7217e1b791b87865b37e582d8711790e4f204-image.png" />


# Precompiles
## Introduction

Precompiled contracts are specialized smart contracts implemented directly in the execution layer of a blockchain. Unlike user-deployed smart contracts that execute EVM bytecode, precompiled contracts offer optimized native implementations for complex cryptographic and computational operations. This significantly improves efficiency and reduces gas costs. Precompiled contracts exist at fixed addresses within the execution client and each precompile has a predefined gas cost based on its computational complexity, ensuring predictable execution fees.

Story Protocol introduces two precompiled contracts:

* `p256Verify` precompile to support signature verifications in the secp256r1 elliptic curve.
* `ipgraph` precompile to enhance on-chain intellectual property management.

In addition, Story Protocol’s execution layer supports all standard EVM precompiled contracts, ensuring full compatibility with Ethereum-based tooling and applications.

## Precompiled Contracts

| Address          | Functionality                                                 |
| ---------------- | ------------------------------------------------------------- |
| byte{0x01}       | `ecrecover`- ECDSA signature recovery                         |
| byte{0x02}       | `sha256` - SHA-256 hash computation                           |
| byte{0x03}       | `ripemd160` - RIPEMD-160 hash computation                     |
| byte{0x04}       | `identity` - Identity function                                |
| byte{0x05}       | `modexp` - Modular exponentiation                             |
| byte{0x06}       | `bn256Add` - BN256 elliptic curve addition                    |
| byte{0x07}       | `bn256ScalarMul` - BN256 elliptic curve scalar multiplication |
| byte{0x08}       | `bn256Pairing` - BN256 elliptic curve pairing check           |
| byte{0x09}       | `blake2f` - Blake2 hash function                              |
| byte{0x0a}       | `kzgPointEvaluation` - KZG polynomial commitment evaluation   |
| byte{0x01, 0x00} | `p256Verify` -  Secp256r1 signature verification              |
| byte{0x01, 0x01} | `ipgraph` - Intellectual property management                  |

## p256Verify precompile

Refer to [RIP-7212](https://github.com/ethereum/RIPs/blob/master/RIPS/rip-7212.md) for more information.

## IPgraph precompile

The `ipgraph` precompile enables efficient querying and modification of IP relationships and royalty structures while minimizing gas costs.

This precompile provides multiple functions based on the function selector—the first 4 bytes of the input.

| Function Selector        | Description                                                     | Gas computation formula                               | Gas Cost                           |
| :----------------------- | :-------------------------------------------------------------- | :---------------------------------------------------- | :--------------------------------- |
| `addParentIp`            | Adds a parent IP record                                         | `intrinsicGas + (ipGraphWriteGas * parentCount)`      | Larger than 1100                   |
| `hasParentIp`            | Checks if an IP is parent of another IP                         | `ipGraphReadGas * averageParentIpCount`               | 40                                 |
| `getParentIps`           | Retrieves parent IPs                                            | `ipGraphReadGas * averageParentIpCount`               | 40                                 |
| `getParentIpsCount`      | Gets the number of parent IPs                                   | `ipGraphReadGas`                                      | 10                                 |
| `getAncestorIps`         | Retrieves ancestor IPs                                          | `ipGraphReadGas * averageAncestorIpCount * 2`         | 600                                |
| `getAncestorIpsCount`    | Gets the number of ancestor IPs                                 | `ipGraphReadGas * averageParentIpCount * 2`           | 80                                 |
| `hasAncestorIp`          | Checks if an IP is ancestor of another IP                       | `ipGraphReadGas * averageAncestorIpCount * 2`         | 600                                |
| `setRoyalty`             | Sets royalty details of an IP                                   | `ipGraphWriteGas`                                     | 1000                               |
| `getRoyalty`             | Retrieves royalty details of an IP                              | `varies by royalty policy`                            | LAP:900, LRP:620, other:1000       |
| `getRoyaltyStack`        | Retrieves royalty stack  of an IP                               | `varies by royalty policy`                            | LAP:50, LRP: 600, other:1000       |
| `hasParentIpExt`         | Checks if an IP is parent of another IP through external call   | `ipGraphExternalReadGas * averageParentIpCount`       | 8400                               |
| `getParentIpsExt`        | Retrieves parent IPs through external call                      | `ipGraphExternalReadGas * averageParentIpCount`       | 8400                               |
| `getParentIpsCountExt`   | Gets the number of parent IPs through external call             | `ipGraphExternalReadGas`                              | 2100                               |
| `getAncestorIpsExt`      | Retrieve ancestor IPs through external call                     | `ipGraphExternalReadGas * averageAncestorIpCount * 2` | 126000                             |
| `getAncestorIpsCountExt` | Gets the number of ancestor IPs through external call           | `ipGraphExternalReadGas * averageParentIpCount * 2`   | 16800                              |
| `hasAncestorIpExt`       | Checks if an IP is ancestor of another IP through external call | `ipGraphExternalReadGas * averageAncestorIpCount * 2` | 126000                             |
| `getRoyaltyExt`          | Retrieves royalty details of an IP through external call        | `varies by royalty policy`                            | LAP:189000, LRP:130200, other:1000 |
| `getRoyaltyStackExt`     | Retrieves royalty stack of an IP through external call          | `varies by royalty policy`                            | LAP:10500, LRP:126000, other:1000  |

Refer to the [Royalty Module](doc:royalty-module) for detailed information on royalty policies.

# Validator Operations
## Quick Links

* [Story Geth Releases](https://github.com/piplabs/story-geth/releases)
* [Story Releases](https://github.com/piplabs/story/releases/)

# Overview

This section will guide you through how you can run your own validator. Validator operations may be done via the `story` consensus client.

> 📘 Note
>
> The below operations do not requiring running a node! However, if you would like to participate in staking rewards, you must run a validator node.

Before proceeding, it is important to familiarize yourself with the difference between a delegator and a validator:

* A **validator** is a full node that participates in consensus whose signed key resides in the `priv_validator_key.json` file under your `story` data directory. To print out your validator key details you may refer to the [validator key export section](https://docs.story.foundation/docs/validator-operations#validator-key-export)
* A **delegator** refers to an account operator that holds `IP` and wishes to participate in consensus rewards but without needing to run a validator themselves.

In the same folder as where your `story` binary resides, add a `.env` file with a `PRIVATE_KEY` whose account has `IP` funded. **We recommend using your delegator account for all below operations.**

> 📘 Note
>
> You may also issue transactions as the validator itself. To get the EVM private key corresponding to your validator, please refer to the [Validator Key Export](https://docs.story.foundation/docs/validator-operations#validator-key-export) section.

The `.env` file should look like the following *(make sure not to add a 0x prefix):*

```bash
# ~/.env
PRIVATE_KEY=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

With this, you are all set to perform different validator operations! Below, we will guide you through all of those supported via the CLI:

## Validator Key Export

By default, when you run `./story init` a validator key is created for you. To view your validator key, run the following command:

```bash
./story validator export [flags]
```

This will print out your validator public key file in compressed and uncompressed formats. By default, we use the hex-encoded compressed key for public identification.

```text
Compressed Public Key (hex): 03bdc7b8940babe9226d52d7fa299a1faf3d64a82f809889256c8f146958a63984
Compressed Public Key (base64): A73HuJQLq+kibVLX+imaH689ZKgvgJiJJWyPFGlYpjmE
Uncompressed Public Key (hex): 04bdc7b8940babe9226d52d7fa299a1faf3d64a82f809889256c8f146958a6398496b9e2af0a3a1d199c3cc1d09ee899336a530c185df6b46a9735b25e79a493af
EVM Address: 0x9EacBe2C3B1eb0a9FC14106d97bd3A1F89efdDCc
Validator Address: storyvaloper1p470h0jtph4n5hztallp8vznq8ehylsw9vpddx
Delegator Address: story1p470h0jtph4n5hztallp8vznq8ehylswtr4vxd
```

**Available Flags:**

* `--export-evm-key`: (string) Exports the derived EVM private key of your validator into the default data config directory
* `--export-evm-key-path`: (string) Specifies a different download location for the derived EVM private key of your validator
* `--keyfile`: (string) Path to the Tendermint key file (default "/home/ubuntu/.story/story/config/priv\_validator\_key.json")

*If you would like to issue transactions as your validator, and not as a delegator, you may export the key to your`.env` file and ensure it has IP sent to it, e.g. via`./story validator export --export-evm-key --evm-key-path .env`*

## Validator Creation

To create a new validator, run the following command:

```bash Locked token node
./story validator create 
			 --stake ${AMOUNT_TO_STAKE_IN_WEI} \
			 --moniker ${VALIDATOR_NAME} \
       --rpc ${rpc} \
		   --chain-id ${chain_id} \
			 --commission-rate ${rate} \
 			 --unlocked=false
```
```Text Unlocked token node
./story validator create 
			 --stake ${AMOUNT_TO_STAKE_IN_WEI} \
			 --moniker ${VALIDATOR_NAME} \
       --rpc ${rpc} \
		   --chain-id ${chain_id} \
			 --commission-rate ${rate} \
```

This will create the validator corresponding to your validator key saved in `priv_validator_key.json`, providing the validator with `{$AMOUNT_TO_STAKE_IN_WEI}` IP to self-stake. *Note that to participate in consensus, at least 1024 IP must be staked (equivalent to`1024000000000000000000 wei`)!*

Below is a list of optional flags to further customize your validator setup:

**Available Flags:**

* `--stake`: Sets the amount the validator will self-delegate in wei (default is `1024000000000000000000` wei).
* `--moniker`: Defines a custom name for the validator, visible to users on the network.
* `--chain-id`: Specifies the Chain ID for the transaction. By default, this is set to `1516`.
* `--commission-rate`: Sets the validator's commission rate in bips (1% = 100 bips). For instance, `1000` represents a 10% commission (default is `1000`).
* `--explorer`: Specifies the URL of the blockchain explorer (default: [https://odyssey.storyscan.xyz](https://odyssey.storyscan.xyz)).
* `--keyfile`: Points to the path of the Tendermint key file (default: `/home/node_story_odyssey/.story/story/config/priv_validator_key.json`).
* `--max-commission-change-rate`: Sets the maximum rate at which the validator's commission can change, in bips. For example, `100` represents a maximum change of 1% (default is `1000`).
* `--max-commission-rate`: Defines the maximum commission rate the validator can charge, in bips. For instance, `5000` allows a 50% maximum rate (default is `5000`).
* `--private-key`: Uses a specified private key for signing the transaction. If not set, the key in `priv_validator_key.json` will be used.
* `--rpc`: Sets the RPC URL to connect to the network (default: [https://odyssey.storyrpc.io](https://odyssey.storyrpc.io)).
* `--unlocked`: Determines if unlocked token staking is supported (`true` for unlocked staking, `false` for locked staking). By default, this is set to `true`.

### Example creation command use

```bash
story validator create 
	--stake 1024000000000000000000
  --moniker "timtimtim"
  --commission-rate 700
  --validator-pubkey "<validator_pubkey>" # if you dont have a .env
  --rpc "https://mainnet.storyrpc.io"
	--chain-id 1514
```

### Verifying your validator

Once created, please use the `Explorer URL` to confirm the transaction. If successful, you should see your validator pub key (*found in your`priv_validator_key.json` file)* listed as part of the following endpoint:

```bash
curl https://testnet.storyrpc.io/validators | jq .
```

Congratulations, you are now one of Story’s very first IP validators!

## Validator Staking

To stake to an existing validator, run the following command:

```bash
./story validator stake \
   --validator-pubkey ${VALIDATOR_PUB_KEY_IN_HEX} \
   --stake ${AMOUNT_TO_STAKE_IN_WEI}
   --staking-period ${STAKING_PERIOD}
```

* Note that your own `${VALIDATOR_PUB_KEY_IN_HEX}`may be found by running the `./story validator export` command as the `Compressed Public Key (hex)`.
* You must stake at least 1024 IP worth (`*1024000000000000000000 wei`) for the transaction to be valid

Once staked, you may use the `Explorer URL` to confirm the transaction. As mentioned earlier, you may use our [validator endpoint](https://rpc.odyssey.storyrpc.io/validators) to confirm the new voting power of the validator.

**Available Flags:**

* `--validator-pubkey`: (string) The public key of the validator to stake to
* `--stake`: (string) The amount of IP to stake in wei
* `--chain-id`: (int) Chain ID to use for the transaction (default: 1514)
* `--explorer`: (string) URL of the blockchain explorer
* `--help`, `-h`: Display help information for stake command
* `--private-key`: (string) Private key used for the transaction
* `--rpc`: (string) RPC URL to connect to the network
* `--staking-period`: (stakingPeriod) Staking period (options: "flexible", "short", "medium", "long") (default: flexible)

### Example staking command use

```bash
./story validator stake \
  --validator-pubkey 03bdc7b8940babe9226d52d7fa299a1faf3d64a82f809889256c8f146958a63984 \
  --stake 1024000000000000000000
  --staking-period "short"
```

## Validator Unstaking

To unstake from a validator, run the following command:

```bash
./story validator unstake \
  --validator-pubkey ${VALIDATOR_PUB_KEY_IN_HEX} \
  --unstake ${AMOUNT_TO_UNSTAKE_IN_WEI} \
	--delegation-id ${ID_STAKING_PERIOD}
```

This will unstake `${AMOUNT_TO_UNSTAKE_IN_WEI}` IP from the selected validator. You must unstake at least 1024 IP worth (`*1024000000000000000000 wei`) for the transaction to be valid.

Like in the staking operation, please use the `Explorer URL` to confirm the transaction and our [validator endpoint](https://rpc.odyssey.storyrpc.io/validators) to double-check the newly reduced voting power of the validator.

**Available Flags:**

* `--chain-id`: (int) Chain ID to use for the transaction (default: 1514)
* `--delegation-id`: (uint32) The delegation ID (0 for flexible staking)
* `--explorer`: (string) URL of the blockchain explorer (default: "[https://storyscan.xyz](https://storyscan.xyz)")
* `--help`, `-h`: Help for unstake command
* `--private-key`: (string) Private key used for the transaction
* `--rpc`: (string) RPC URL to connect to the network (default: "[https://storyrpc.io](https://storyrpc.io)")
* `--unstake`: (string) Amount to unstake in wei
* `--validator-pubkey`: (string) Validator's hex-encoded compressed 33-byte secp256k1 public key

### Example unstaking command use

```bash
./story validator unstake \
   --validator-pubkey 03bdc7b8940babe9226d52d7fa299a1faf3d64a82f809889256c8f146958a63984 \
   --unstake 1024000000000000000000 \
   --delegation-id 1
```

## Validator Stake-on-behalf

To stake on behalf of another delegator, run the following command:

```bash
./story validator stake-on-behalf \
  --delegator-address ${DELEGATOR_EVM} \
  --validator-pubkey ${VALIDATOR_PUB_KEY_IN_HEX} \
  --stake ${AMOUNT_TO_STAKE_IN_WEI} \
  --staking-period ${STAKING_PERIOD} \
  --rpc
  --chain-id
```

This will stake `${AMOUNT_TO_STAKE_IN_WEI}` IP to the validator on behalf of the provided delegator. You must stake at least 1024 IP worth (`*1024000000000000000000 wei`) for the transaction to be valid.

Like in the other staking operations, please use the `Explorer URL` to confirm the transaction and our [validator endpoint](https://rpc.odyssey.storyrpc.io/validators) to double-check the increased voting power of the validator.

**Available Flags:**

* `--chain-id`: (int) Chain ID to use for the transaction (default: 1514)
* `--delegator-address`: (string) Delegator's EVM address
* `--explorer`: (string) URL of the blockchain explorer (default: "[https://storyscan.xyz](https://storyscan.xyz)")
* `--help`, `-h`: Help for stake-on-behalf command
* `--private-key`: (string) Private key used for the transaction
* `--rpc`: (string) RPC URL to connect to the network (default: "[https://storyrpc.io](https://storyrpc.io)")
* `--stake`: (string) Amount for the validator to self-delegate in wei
* `--staking-period`: (stakingPeriod) Staking period (options: "flexible", "short", "medium", "long") (default: flexible)
* `--validator-pubkey`: (string) Validator's hex-encoded compressed 33-byte secp256k1 public key

### Example Stake-on-behalf command use

```bash
./story validator stake-on-behalf \
   --delegator-address 0xF84ce113FCEe12d78Eb41590c273498157c91520 \
   --validator-pubkey 03e42b4d778cda2f3612c85161ba7c0aad1550a872f3279d99e028a1dfa7854930 \
   --stake 1024000000000000000000 \
   --staking-period "short" \
	 --rpc \
   --chain-id
```

## Validator Unstake-on-behalf

You may also unstake on behalf of delegators. However, to do so, you must be registered as an authorized operator for that delegator. To unstake on behalf of another delegator as an operator, run the following command:

```bash
./story validator unstake-on-behalf \
  --delegator-address ${DELEGATOR_PUB_KEY_IN_HEX} \
  --validator-pubkey ${VALIDATOR_PUB_KEY_IN_HEX} \
  --unstake ${AMOUNT_TO_STAKE_IN_WEI} \
  --rpc \
  --chain-id
```

This will unstake `${AMOUNT_TO_STAKE_IN_WEI}` IP from the validator on behalf of the delegator, assuming you are a registered operator for that delegator. You must unstake at least 1024 IP worth (`*1024000000000000000000 wei`) for the transaction to be valid.

Like in the other staking operations, please use the `Explorer URL` to confirm the transaction and our [validator endpoint](https://rpc.odyssey.storyrpc.io/validators) to double-check the decreased voting power of the validator.

**Available Flags:**

* `--chain-id`: (int) Chain ID to use for the transaction (default: 1514)
* `--delegator-address`: (string) Delegator's EVM address
* `--explorer`: (string) URL of the blockchain explorer (default: "[https://storyscan.xyz](https://storyscan.xyz)")
* `--help`, `-h`: Help for unstake-on-behalf command
* `--private-key`: (string) Private key used for the transaction
* `--rpc`: (string) RPC URL to connect to the network (default: "[https://storyrpc.io](https://storyrpc.io)")
* `--unstake`: (string) Amount to unstake in wei
* `--validator-pubkey`: (string) Validator's hex-encoded compressed 33-byte secp256k1 public key

### Example Unstake-on-behalf command use

```bash
./story validator unstake-on-behalf \
   --delegator-address 0xF84ce113FCEe12d78Eb41590c273498157c91520 \
   --validator-pubkey 03e42b4d778cda2f3612c85161ba7c0aad1550a872f3279d99e028a1dfa7854930 \
   --unstake 1024000000000000000000 \
   --rpc \
   --chain-id
```

## Validator Unjail

In case a validator becomes jailed, for example if it experiences substantial downtime, you may use the following command to unjail the targeted validator:

```Text Bash
./story validator unjail \
  --private-key ${PRIVATE_KEY} \
  --rpc
  --chain-id
```

Note that you will need at least 1 IP in the wallet submitting the transaction for the transaction to be valid.

**Available Flags:**

* `--chain-id`: (int) Chain ID to use for the transaction
* `--explorer`: (string) URL of the blockchain explorer
* `--private-key`: (string) Private key used for the transaction
* `--rpc`: (string) RPC URL to connect to the network

### Example unjail command use

```bash
./story validator unjail \
  --validator-pubkey 03bdc7b8940babe9226d52d7fa299a1faf3d64a82f809889256c8f146958a63984 \
  --rpc \
  --chain-id 
```

## Validator Unjail-on-behalf

If you are an authorized operator, you may unjail a validator on their behalf using the following command:

```bash
./story validator unjail-on-behalf \
  --private-key ${PRIVATE_KEY} \
  --validator-pubkey ${VALIDATOR_PUB_KEY_IN_HEX} \
  --rpc \
  --chain-id
```

**Available Flags:**

* `--chain-id`: (int) Chain ID to use for the transaction
* `--explorer`: (string) URL of the blockchain explorer
* `--private-key`: (string) Private key used for the transaction
* `--rpc`: (string) RPC URL to connect to the network
* `--validator-pubkey`: (string) Validator's hex-encoded compressed 33-byte secp256k1 public key

### Example unjail-on-behalf command use

```bash
./story validator unjail-on-behalf \
  --private-key 0x1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef \
  --validator-pubkey 03e42b4d778cda2f3612c85161ba7c0aad1550a872f3279d99e028a1dfa7854930 \
  --rpc \
  --chain-id
```

## Validator Redelegate

To redelegate from one validator to another, run the following command:

```bash
./story validator redelegate \
  --validator-src-pubkey ${VALIDATOR_SRC_PUB_KEY_IN_HEX} \
  --validator-dst-pubkey ${VALIDATOR_DST_PUB_KEY_IN_HEX} \
  --redelegate ${AMOUNT_TO_REDELEGATE_IN_WEI}
  --rpc \
  --chain-id
```

**Available Flags:**

* `--chain-id`: (int) Chain ID to use for the transaction (default 1514)
* `--delegation-id`: (uint32) The delegation ID (0 for flexible staking)
* `--explorer`: (string) URL of the blockchain explorer (default "[https://storyscan.xyz](https://storyscan.xyz)")
* `--help`, `-h`: Help for redelegate command
* `--private-key`: (string) Private key used for the transaction
* `--redelegate`: (string) Amount to redelegate in wei
* `--rpc`: (string) RPC URL to connect to the network (default "[https://storyrpc.io](https://storyrpc.io)")
* `--validator-dst-pubkey`: (string) Dst validator's hex-encoded compressed 33-byte secp256k1 public key
* `--validator-src-pubkey`: (string) Src validator's hex-encoded compressed 33-byte secp256k1 public key

### Example redelegate command use

```bash
./story validator redelegate \
  --validator-src-pubkey 03bdc7b8940babe9226d52d7fa299a1faf3d64a82f809889256c8f146958a63984 \
  --validator-dst-pubkey 02ed58a9319aba87f60fe08e87bc31658dda6bfd7931686790a2ff803846d4e59c \
  --redelegate 1024000000000000000000 \
  --rpc \
  --chain-id
```

## Validator Redelegate-on-behalf

If you are an authorized operator, you may redelegate from one validator to another on behalf of a delegator using the following command:

```bash
./story validator redelegate-on-behalf \
  --delegator-address ${DELEGATOR_EVM_ADDRESS} \
  --validator-src-pubkey ${VALIDATOR_SRC_PUB_KEY_IN_HEX} \
  --validator-dst-pubkey ${VALIDATOR_DST_PUB_KEY_IN_HEX} \
  --redelegate ${AMOUNT_TO_REDELEGATE_IN_WEI} \
  --rpc \
  --chain-id
```

**Available Flags:**

* `--chain-id`: (int) Chain ID to use for the transaction (default 1514)
* `--delegation-id`: (uint32) The delegation ID (0 for flexible staking)
* `--delegator-address`: (string) Delegator's EVM address
* `--explorer`: (string) URL of the blockchain explorer (default "[https://storyscan.xyz](https://storyscan.xyz)")
* `--help`, `-h`: Help for redelegate-on-behalf command
* `--private-key`: (string) Private key used for the transaction
* `--redelegate`: (string) Amount to redelegate in wei
* `--rpc`: (string) RPC URL to connect to the network (default "[https://storyrpc.io](https://storyrpc.io)")
* `--validator-dst-pubkey`: (string) Dst validator's hex-encoded compressed 33-byte secp256k1 public key
* `--validator-src-pubkey`: (string) Src validator's hex-encoded compressed 33-byte secp256k1 public key

### Example redelegate-on-behalf command use

```bash
./story validator redelegate-on-behalf \
  --delegator-address 0xf398C12A45Bc409b6C652E25bb0a3e702492A4ab \
  --validator-src-pubkey 03bdc7b8940babe9226d52d7fa299a1faf3d64a82f809889256c8f146958a63984 \
  --validator-dst-pubkey 02ed58a9319aba87f60fe08e87bc31658dda6bfd7931686790a2ff803846d4e59c \
  --redelegate 1024000000000000000000 \
  --rpc \
  --chain-id
```

## Set Operator

Delegators may add operators to unstake or redelegate on their behalf. To add an operator, run the following command:

* `--chain-id` int         Chain ID to use for the transaction (default 1514)
* `--explorer` string      URL of the blockchain explorer (default "[https://storyscan.xyz](https://storyscan.xyz)")
* `--operator` string      Sets an operator to your delegator
* `--private-key` string   Private key used for the transaction
* `--rpc` string           RPC URL to connect to the network (default "[https://storyrpc.io](https://storyrpc.io)")

```bash
./story validator set-operator \
  --operator ${OPERATOR_EVM_ADDRESS} \
  --rpc \
  --chain-id
```

Note that you will need at least 1 IP in the wallet submitting the transaction for the transaction to be valid.

### Example add operator command use

```bash
./story validator set-operator \
  --operator 0xf398C12A45Bc409b6C652E25bb0a3e702492A4ab \
  --rpc \
  --chain-id
```

## Unset Operator

To remove an operator, run the following command:

```bash
./story validator unset-operator \
  --operator ${OPERATOR_EVM_ADDRESS} \  
  --rpc \
  --chain-id
```

### Example Remove Operator command use

```bash
./story validator remove-operator \
  --operator 0xf398C12A45Bc409b6C652E25bb0a3e702492A4ab \
  --rpc \
  --chain-id
```

## Set Withdrawal Address

To change the address that your delegator receives staking and withdrawal rewards from, you can run the following:

```bash
./story validator set-withdrawal-address \
  --withdrawal-address ${OPERATOR_EVM_ADDRESS}
```

Note that you will need at least 1 IP in the wallet submitting the transaction for the transaction to be valid.

### Example Set Withdrawal Address command use

```bash
./story validator set-withdrawal-address \
  --withdrawal-address 0xf398C12A45Bc409b6C652E25bb0a3e702492A4ab
```

## Migrating a validator to another machine

> 🚧 Important
>
> Before migrating your validator node to a new machine, make sure the current node is fully shut down. Attempting to restore an active validator could result in "double signing," a critical error that may lead to the slashing of your delegated shares.

1. Begin by configuring a new environment for your validator. Ensure that the new full node is fully synced to the latest block on the network.
2. To avoid accidental double-signing, it’s essential to fully shut down the original validator node before activating the new instance. We recommend deleting the Story service file to prevent it from automatically restarting after a system reboot. Additionally, back up your `priv_validator_key.json` file and remove it from the current server running the active validator. Skipping these steps could result in missed blocks or other penalties.

```bash
# Step 1: Stop the original validator node
sudo systemctl stop <your_service_file_name>.service

# Step 2: Disable the Story service to prevent automatic restarts
sudo systemctl disable <your_service_file_name>.service

# Step 3: Delete the Story service file to prevent it from starting on reboot
sudo rm /etc/systemd/system/<your_service_file_name>.service

# Step 4: Back up the `priv_validator_key.json` file securely, e.g., using SFTP:
# Use an SFTP client or a secure method to download the file without displaying it in the terminal
# If needed for verification purposes only, you may view it with the following command:
cat ~/.story/story/config/priv_validator_key.json

# Step 5: Remove the `priv_validator_key.json` file from the current server
rm ~/.story/story/config/priv_validator_key.json
```

3. Locate the `priv_validator_key.json` file in the `~/.story/story/config/` directory on your new machine. Replace this file with the backup copy from your old validator.

> ❗️ Important: Before proceeding, shut down the old validator on the original server and do not restart it!

4. After transferring the private key file, restart the validator node on your new setup. This will reintegrate your validator with the network, enabling it to resume its validation role.

# Infrastructure Partners
## RPC Providers

<Cards columns={1}>
  <Card title="QuickNode" href="https://www.quicknode.com/chains/story" icon="fa-home" target="_blank">
    QuickNode provides hosted Story RPC nodes under their free and paid plans, granting flexible and reliable access to the network. For high-throughput or mission-critical applications, Dedicated Clusters deliver premium performance with unmetered billing, elevated rate limits, and robust infrastructure.
  </Card>
</Cards>

## Cross-chain

<Cards columns={3}>
  <Card title="LayerZero" href="https://docs.layerzero.network/v2/developers/evm/technical-reference/deployed-contracts?chains=odyssey-testnet" icon="fa-home" iconColor="#000000" target="_blank">
    LayerZero is a technology that enables applications to move data across blockchains, uniquely supporting censorship-resistant messages and permissionless development through immutable smart contracts.
  </Card>

  <Card title="deBridge" href="https://debridge.finance/" icon="fa-home" iconColor="#fbff3a" target="_blank">
    Blazingly fast bridging for anyone that likes to be one step ahead.
  </Card>

  <Card title="Stargate" href="https://stargate.finance/" icon="fa-home" iconColor="#ffffff" target="_blank">
    Stargate is a fully composable liquidity transport protocol that lives at the heart of Omnichain DeFi.
  </Card>
</Cards>

## Onramp/Offramp

<Cards columns={2}>
  <Card title="Transak" href="https://transak.com/" icon="fa-home" iconColor="#1461db" target="_blank">
    Enable users to buy or sell crypto from your app.
  </Card>

  <Card title="Halliday" href="https://halliday.xyz/" icon="fa-home" iconColor="#392df8" target="_blank">
    The Commerce Automation Network for Modular Chains.
  </Card>
</Cards>

## Indexers/Data

<Cards columns={3}>
  <Card title="Simplehash" href="https://simplehash.com/" icon="fa-home" iconColor="#5046e5" target="_blank">
    Instant access to Token and NFT market prices, metadata and media. 80+ chains.
  </Card>

  <Card title="Goldsky" href="https://goldsky.com/" icon="fa-home" iconColor="#ffbf60" target="_blank">
    Crypto Data Live-Streamed.
  </Card>

  <Card title="Zettablock" href="https://zettablock.com/" icon="fa-home" iconColor="#3c4ff6" target="_blank">
    A unified platform for open and trustfree AI development, empowering an accessible ecosystem of models and datasets.
  </Card>
</Cards>

## Oracles/VRF

<Cards columns={3}>
  <Card title="Gelato" href="https://www.gelato.network/" icon="fa-home" iconColor="#ff3b57" target="_blank">
    Build scalable, custom enterprise-grade Rollups with Gelato's Web3 Services natively integrated.
  </Card>

  <Card title="Redstone" href="https://www.redstone.finance/" icon="fa-home" iconColor="#ae0722" target="_blank">
    Modular oracles for DeFi.
  </Card>

  <Card title="Pyth" href="https://www.pyth.network/" icon="fa-home" iconColor="#e6dafe" target="_blank">
    Secure your smart contracts with reliable, low-latency market data from institutional sources. Build apps with high-fidelity oracle feeds designed for mission-critical systems.
  </Card>

  <Card title="Uma" href="https://uma.xyz/" icon="fa-home" iconColor="#fe4d4c" target="_blank">
    A decentralized truth machine.
  </Card>
</Cards>

## Dev Tools

<Cards columns={2}>
  <Card title="Protofire" href="https://protofire.io/" icon="fa-home" iconColor="#f54704" target="_blank">
    Protofire boosts TVL and usage for Web3 projects with our Dev DAO, reducing costs and enhancing quality.
  </Card>

  <Card title="Wagmi" href="https://wagmi.sh/" icon="fa-home" iconColor="#000000" target="_blank">
    Type Safe, Extensible, and Modular by design. Build high-performance blockchain frontends.
  </Card>
</Cards>

## Wallets/AA

<Cards columns={3}>
  <Card title="Dynamic" href="https://www.dynamic.xyz/" icon="fa-home" iconColor="#4779ff" target="_blank">
    Dynamic offers a suite of tools for effortless log in, wallet creation and user management. Designed for users. Built for developers.
  </Card>

  <Card title="Pimlico" href="https://www.pimlico.io/" icon="fa-home" iconColor="#7115aa" target="_blank">
    The world's most popular account abstraction infrastructure platform
  </Card>

  <Card title="ZeroDev" href="https://zerodev.app/" icon="fa-home" iconColor="#23a4f0" target="_blank">
    ZeroDev is the most powerful toolkit for building with smart accounts, including both “smart EOAs” (EIP-7702) and “smart contract accounts” (ERC-4337).
  </Card>

  <Card title="Tomo" href="https://tomo.inc/" icon="fa-home" iconColor="#f21f7f" target="_blank">
    The all-in-one wallet designed to bring the mass adoption.
  </Card>

  <Card title="Privy" href="https://www.privy.io/" icon="fa-home" iconColor="#000000" target="_blank">
    Privy is a powerful authentication and key management platform to securely onboard, activate, and manage your users at scale.
  </Card>

  <Card title="Keplr" href="https://www.keplr.app/" icon="fa-home" iconColor="#0657fa" target="_blank">
    Introducing Keplr, the fast, simple, secure wallet that plugs you into any blockchains and apps wherever you go. Pioneering its ways in the multichain future from day one.
  </Card>

  <Card title="Turnkey" href="https://www.turnkey.com/" icon="fa-home" iconColor="#000000" target="_blank">
    Secure, flexible, and scalable wallet infrastructure.
  </Card>
</Cards>

# Troubleshooting
Welcome to Story node troubleshooting! This section covers common problems and solutions when running Story nodes.

### Node Setup

<details>
  <summary>What are the hardware requirements?</summary>

  <br />

  See the <a href="https://docs.story.foundation/docs/node-setup#system-specs">system specs</a>
</details>

***

<details>
  <summary>What's the max expected TPS?</summary>

  <br />

  \~700
</details>

***

<details>
  <summary>Is it fully EVM-compatible? Is there any customization already being made on the IP blockchain? Or are there any coming customization to be applied?</summary>

  <br />

  Yes, it's EVM-compatible. Story's execution client is a fork of Geth with our custom precompiles, which enhance the IP graph's performance while maintaining strict EVM compatibility. Other Ethereum execution clients, such as RETH and Erigon, can be supported later.
</details>

***

<details>
  <summary>Which is your consensus mechanism?</summary>

  <br />

  Our consensus mechanism is CometBFT
</details>

***

<details>
  <summary>Batches support? Limit on batch request?</summary>

  <br />

  Batch RPCs are supported - for Geth there is a 1K limit and on the consensus side there is 10 request limit
</details>

***

<details>
  <summary>WS connections? (if yes, how do they work)</summary>

  <br />

  Yes, WS is enabled on the execution client, and is recommended for subscription use-cases. It is open on port 8546
</details>

***

<details>
  <summary>How many different paths does node serves (several path with diff methods RPC)?</summary>

  <br />

  Please see Geth's latest JSON-RPC documentation for a full comprehensive list <a href="https://ethereum.org/en/developers/docs/apis/json-rpc/#web3_clientversion">here</a>. In the future, we may add more.
</details>

***

<details>
  <summary>Caching rules for RPC method?</summary>

  <br />

  We recommend employing standard in-memory caching with a 1-10 min TTL based on the RPC method
</details>

***

<details>
  <summary>What is the best method to get latest block and check node is healthy and in sync?</summary>

  <br />

  Use `eth_syncing` RPC call on the execution client to check if the node is sync and `eth_blockNumber` for getting the latest block
</details>

***

<details>
  <summary>What are the heaviest RPC methods? How much time does it take to respond to request with such method?</summary>

  <br />

  `eth_call` / `eth_getLogs` / `eth_getBlockByNumber` \
  We are still running latency tests to get a sense of response times.
</details>

***

<details>
  <summary>Is archive node provisioning a requirement? If yes how big?</summary>

  <br />

  No, not at the moment.
</details>

***

<details>
  <summary>Are there snapshots available for full / archive?</summary>

  <br />

  Not yet, but we are working on it.
</details>

<br />

### Common Issues

***

<details>
  <summary>Database Initialization Failure</summary>

  <br />

  **Error:**

  ```bash
  ERRO !! Fatal error occurred, app died️ unexpectedly !! err="create db: failed to initialize database:
  ```

  **Solution:**

  1. Save your validator state:

  ```bash
  cp $HOME/.story/story/data/priv_validator_state.json $HOME/.story/story/priv_validator_state.json.backup
  ```

  > 🚧 Be very careful with this file, especially if your validator is already signing blocks.

  * Check your the database backend type, your node must support the same as you are using the snapshot:

  ```bash
  cat $HOME/.story/story/config/story.toml
  ```

  Default is `app-db-backend = "goleveldb”`. The fallback is the `db_backend` value set in CometBFT's `config.toml`.

  ```bash
  cat $HOME/.story/story/config/config.toml
  ```
</details>

***

<details>
  <summary>High Gas Fees</summary>

  **Problem:** Need to adjust gas fees on RPC node

  **Solution:**
  Add the `--rpc.txfee` flag to your geth startup command:

  ```bash

  sudo tee /etc/systemd/system/story-geth.service > /dev/null <<EOF
  [Unit]
  Description=Story-Geth Node
  After=network.target

  [Service]
  User=$USER
  Type=simple
  WorkingDirectory=$HOME/.story/geth
  ExecStart=$(which geth) --story --syncmode full --rpc.txfee 2
  Restart=on-failure
  LimitNOFILE=65535

  [Install]
  WantedBy=multi-user.target
  EOF
  ```
</details>

***

<details>
  <summary>Failed to send PacketPing</summary>

  <br />

  **Error:**

  ```bash
  ERRO Failed to send PacketPing module=p2p peer=19fa6dd52e72e4e85bbb873b705282cf73217a6b@158.220.80.96:40128 err="write tcp 139.59.139.135:26656->158.220.80.96:40128: write: broken pipe"
  ```

  Solution:

  * If the node is synchronized, you can ignore this error. Your client may be a little behind.
  * If the node stops, you should restart the services.
</details>

***

<details>
  <summary>Cosmovisor: failed to read upgrade info</summary>

  <br />

  An error occurs when starting the cosmovisor:

  ```bash
  panic: failed to read upgrade info from disk unexpected end of JSON input
  ```

  Solution:

  * You must ensure that the installed cosmovisor version must be at least [v1.7.0.](https://docs.cosmos.network/main/build/tooling/cosmovisor)
  * Then check your info file (edit version `v0.13.0` in your case):

  ```bash
  cat $HOME/.story/story/cosmovisor/upgrades/v0.13.0/upgrade-info.json
  ```

  If you don\`t have create new one:

  ```bash
  echo '{"name":"v0.13.0","time":"0001-01-01T00:00:00Z","height":858000}' > $HOME/.story/story/cosmovisor/upgrades/v0.13.0/upgrade-info.json
  ```

  Find out more about automatic updates with cosmovisor [here](https://docs.story.foundation/docs/odyssey-node-setup#automated-upgrades).
</details>

***

<details>
  <summary>IPC endpoint closed</summary>

  <br />

  Error:

  ```bash
  INFO HTTP server stopped
  INFO IPC endpoint closed
  ```

  Solution:

  * It looks like port 8551 stopping, the background process running `iptables` blocking ip and port and access posix.
  * For solution try uninstall `ufw posix` and `iptables`:

  ```bash
  iptables -I INPUT -s localhost -j ACCEPT
  ```
</details>

***

<details>
  <summary>Found signature from the same key</summary>

  <br />

  Error:

  ```bash
  panic: Faile to consensus  state: found signature from the same key
  ```

  Solution:

  * The validator has been double signed. It is currently not possible to restore the validator after it has been double signed.
  * To avoid such situations, see this post on how to correctly [migrate a validator to another machine](https://docs.story.foundation/docs/odyssey-validator-operations#migrating-a-validator-to-another-machine).
</details>

***

<details>
  <summary>Failed to validate create flags: missing required flag(s): moniker</summary>

  <br />

  Error:

  ```bash
  4-11-26 08:42:20.302 ERRO !! Fatal error occurred, app died️ unexpectedly !! err="failed to validate create flags: missing required flag(s): moniker" stacktrace="[errors.go:39 flags.go:173 validator.go:168 validator.go:384 command.go:985 command.go:1117 command.go:1041 command.go:1034 cmd.go:34 main.go:10 proc.go:271 asm_amd64.s:1695]"
  ```

  Solution:

  * You missed flag `--moniker`.
  * The command to create a new validator should look like this:

  ```bash
  ./story validator create --stake ${AMOUNT_TO_STAKE_IN_WEI} --moniker ${VALIDATOR_NAME}
  ```

  See more options [here](https://docs.story.foundation/docs/odyssey-validator-operations#validator-creation).
</details>

***

<details>
  <summary>Error adding vote</summary>

  <br />

  Error:

  ```bash
  ERRO failed to process message msg_type= *consensus.VoteMessage err:" error adding vote"
  ```

  Solution:

  * It looks like your node is down. To get started, check the current versions of the binaries [here](https://docs.story.foundation/docs/odyssey-node-setup).
  * If you have up-to-date binary - try updating peers, this usually happens when a node loses p2p communication:

  ```bash
  PEERS="..."
  sed -i -e "/^\[p2p\]/,/^\[/{s/^[[:space:]]*persistent_peers *=.*/persistent_peers = \"$PEERS\"/}" $HOME/.story/story/config/config.toml
  ```
</details>

***

<details>
  <summary>Error signing vote</summary>

  <br />

  Error:

  ```bash
  ERRO failed signing vote module=consensus height=403750 round=0 vote="Vote{23:B12C6AE31E8E 403750/00/SIGNED_MSG_TYPE_PREVOTE(Prevote) FA591EB1E540 000000000000 000000000000 @ 2024-11-08T16:58:10.375918193Z}" err="error signing vote: height regression. Got 403750, last height 420344"
  ```

  Solution:

  * Looks like you have a problem with your `priv_validator_state` of validator.
    > 🚧 Be very careful with this file, especially if your validator is already signing blocks.
  * You can make a copy of your state with a command:

  ```bash
  cp $HOME/.story/story/data/priv_validator_state.json $HOME/.story/story/priv_validator_state.json.backup
  ```

  Check your validator state:

  ```bash
  cat $HOME/.story/story/data/priv_validator_state.json
  ```

  * If you get this error, you can reset your state (🚧 ONLY IF YOUR VALIDATOR HAS NOT YET SIGNET BLOCKS).
  * Stop node.

  ```bash
  sudo tee $HOME/.story/story/data/priv_validator_state.json > /dev/null <<EOF
  {
    "height": "0",
    "round": 0,
    "step": 0
  }
  EOF
  ```

  * Start node.
</details>

***

<details>
  <summary>Unknown flag: --home</summary>

  <br />

  Error:

  ```bash
  ERRO !! Fatal error occurred, app died️ unexpectedly !! err="unknown flag: --home"
  ```

  Solution:

  * It looks like a misconfiguration. You must try to remove the `--home` flag from the startup command.
  * Your systemd to run might look like this:
</details>

***

<details>
  <summary>Failed to register the Ethereum service</summary>

  <br />

  Error:

  ```bash
  Fatal: Failed to register the Ethereum service: incompatible state scheme, stored: path, provided: hash
  ```

  Solution:

  * You have problems with the state of validator or a corrupted database.
  * Try using a snapshot.
    > 🚧 Be very careful with this file, especially if your validator is already signing blocks.
  * We have described how to reset your state [here](https://docs.story.foundation/docs/troubleshooting#error-signing-vote).

  ## Failed to reconnect to peer

  Error:

  ```bash
  24-09-25 06:38:45.235 ERRO Failed to reconnect to peer. Beginning exponential backoff module=p2p addr=e0600fa5f2129e647ef30a942aac1695201ff135@65.109.115.98:26656 elapsed=2m29.598884906s
  ```

  Solution:

  * If the node is synchronized and not far behind, you can ignore this error.
  * If the node is lagging or has stopped completely, try updating peers, this usually happens when a node loses p2p communication:

  ```bash
  PEERS="..."
  sed -i -e "/^\[p2p\]/,/^\[/{s/^[[:space:]]*persistent_peers =./persistent_peers = \"$PEERS\"/}" $HOME/.story/story/config/config.toml
  ```
</details>

***

<details>
  <summary>Processing finalized payload halted while evm syncing</summary>

  <br />

  Warn:

  ```bash
  WARN Processing finalized payload halted while evm syncing (will retry) payload_height=...
  ```

  Solution:

  * It just means that story-geth is syncing, you can ignore this warn.
  * However, if it takes a long time, we recommend that you stop the processes one at a time and start them again later in the following order:

  ```bash
  sudo systemctl stop story-geth story
  sudo systemctl daemon-reload
  sudo systemctl start story-geth
  sudo systemctl enable story-geth

  sudo systemctl daemon-reload
  sudo systemctl start story
  sudo systemctl enable story
  ```
</details>

***

<details>
  <summary>Upgrade handler is missing</summary>

  <br />

  Error:

  ```bash
  ERRO error in proxyAppConn.FinalizeBlock      module=consensus err="module manager preblocker: wrong app version 0, upgrade handler is missing for upgrade plan"
  ```

  Solution:

  * Looks like you missed an update.
  * To get started, check the current versions of the binaries [here](https://docs.story.foundation/docs/odyssey-node-setup).
</details>

***

<details>
  <summary>Home directory contains unexpected file</summary>

  <br />

  Error:

  ```bash
  ERRO !! Fatal error occurred, app died️ unexpectedly !! err="home directory contains unexpected file(s), use --force to initialize anyway"
  ```

  Solution:

  * This means that you have already initialized the node.
  * `$HOME/.story/story` directory created, and there are files in it. Delete it, or try with it.
</details>

***

<details>
  <summary>Err="create comet node: create node</summary>

  Error:

  ```bash
  ERRO !! Fatal error occurred, app died️ unexpectedly ! err="create comet node: create node
  ```

  Solution:

  * It appears that your node is using incorrect versions.
  * Check the current versions of the binaries [here](https://docs.story.foundation/docs/odyssey-node-setup).
  * And most likely you need to perform a rollback binary to current versions.
</details>

***

<details>
  <summary>WAL does not contain</summary>

  <br />

  Error:

  ```bash
  ERRO catchup replay: WAL does not contain
  ```

  Solution:

  * Looks like an `AppHash` issue.
  * To get started, upgrade to the current versions of the binaries [here](https://docs.story.foundation/docs/odyssey-node-setup).
  * If your versions are newer than the current ones, perform a rollback.
</details>

***

<details>
  <summary>Err="load engine JWT file: read jwt file</summary>

  Error:

  ```bash
  ERRO !! Fatal error occurred, app died️ unexpectedly !! err="load engine JWT file: read jwt file: open /root/.story/geth/odyssey/geth/jwtsecret: no such file or directory
  ```

  Solution:

  * It seems your node can't get `jwtsecret`.
  * Check your `WorkingDirectory` in your `geth-service` , by default `WorkingDirectory=$HOME/.story/geth`.
  * Check all paths, you can get your `jwtsecret`with command (for odyssey network):

  ```bash
  cat .story/geth/odyssey/geth/jwtsecret
  ```
</details>

***

<details>
  <summary>Couldn't connect to any seeds</summary>

  <br />

  Error:

  ```bash
  ERRO Couldn't connect to any seeds module=p2p
  ```

  Solution:

  * If the node is synchronized and not far behind, you can ignore this error.
  * If the node is lagging or has stopped completely, try updating seeds/peers, it usually happens when a node loses p2p communication (we recommend that you stop the node and delete the addrbook).

  ```bash
  rm -rf $HOME/.story/story/config/addrbook.json
  SEEDS="..."
  PEERS="..."
  sed -i -e "/^\[p2p\]/,/^\[/{s/^[[:space:]]*seeds *=.*/seeds = \"$SEEDS\"/}" \
         -e "/^\[p2p\]/,/^\[/{s/^[[:space:]]*persistent_peers *=.*/persistent_peers = \"$PEERS\"/}" $HOME/.story/story/config/config.toml
  ```
</details>

***

<details>
  <summary>Processing finalized payload failed err="rpc forkchoice updated</summary>

  <br />

  Warn:

  ```bash
  WRN Processing finalized payload; evm syncing
  WRN Processing finalized payload failed: evm fork choice update (will retry) status="" err="rpc forkchoice updated v3: beacon syncer reorging"
  ```

  Solution:

  * Everything is fine, it just means that `story-geth` is syncing, which takes some time.
  * If the node is not far behind, you can ignore this warning.

  ## Dial tcp 127.0.0.1:9090

  Warn:

  ```bash
  WRN error getting latest block error:"rpc error: dial tcp 127.0.0.1:9090"
  ```

  Solution:

  * The logs show a connection failure on port `9090`.
  * Check the listening ports:

  ```bash
  sudo ss -tulpn  | grep LISTEN
  ```

  * If other node uses `9090`, then modify it to another.
  * Normally, this WARNING should not affect the performance of your node.
</details>

***

<details>
  <summary>Wrong AppHash</summary>

  Error:

  ```bash
  ERRO Error in validation module=blocksync err="wrong Block[dot]Header[dot]AppHash  Expected [...]
  ```

  Solution:

  * `Wrong AppHash` type logs means the story node version you are using is wrong.
  * Upgrade to the current versions of the binaries [here](https://docs.story.foundation/docs/odyssey-node-setup).
  * If your versions are newer than the current ones, perform a rollback.
</details>

***

<details>
  <summary>Connection failed sendRoutine / Stopping peer</summary>

  Error:

  ```bash
  ERRO Connection failed @ sendRoutine module=p2p peer=...
  ERRO Stopping peer for error module=p2p peer=...
  ```

  Solution:

  * If the node is synchronized and not far behind, you can ignore this error.
  * If the node is lagging or has stopped completely, try updating peers, this usually happens when a node loses p2p communication:

  ```bash
  PEERS="..."
  sed -i -e "/^\[p2p\]/,/^\[/{s/^[[:space:]]*persistent_peers =./persistent_peers = \"$PEERS\"/}" $HOME/.story/story/config/config.toml
  ```
</details>

***

<details>
  <summary>Moniker must be valid non-empty</summary>

  Error:

  ```bash
  ERRO !! Fatal error occurred, app died️ unexpectedly ! err="create comet node: create node: info.Moniker must be valid non-empty
  ```

  Solution:

  * Looks like a problem with your node moniker.
  * Be sure to use `""` when executing init:

  ```bash
  story init --network "..." --moniker "..."
  ```

  * Go to config, find the moniker and put it inside `""` only:

  ```bash
  sudo nano ~/.story/story/config/config.toml
  ```
</details>

***

<details>
  <summary>Invalid address (26656)</summary>

  Error:

  ```bash
  Fatal error occurred, app died️ unexpectedly ! err="create comet node: create node: invalid address (26656):
  ```

  Solution:

  * The logs report a connection failure on port `26656`.
  * Check the listening ports:

  ```bash
  sudo ss -tulpn  | grep LISTEN
  ```

  * If another node is using `26656`, change it to another and keep the default `26656` for story in the `P2P configuration` options in `config`:

  ```bash
  sudo nano ~/.story/story/config/config.toml
  ```
</details>

***

<details>
  <summary>Eth\_coinbase does not exist</summary>

  Warn:

  ```bash
  WARN Beacon client online, but no consensus updates received in a while. Please fix your beacon client to follow the chain!
  Served eth_coinbase eth_coinbase does not exist
  ```

  Solution:

  * This error indicates that the network has stopped.
</details>

***

<details>
  <summary>Verifying proposal failed</summary>

  Warn:

  ```bash
  WARN Verifying proposal failed: push new payload to evm (will retry) status="" err="new payload: rpc new payload v3: Post \"http://localhost:8551\": round trip: dial tcp 127.0.0.1:8551: connect: connection refused" stacktrace="[errors.go:39 jwt.go:41 client.go:259 client.go:180 client.go:724 client.go:590 http.go:229 http.go:173 client.go:351 engineclient.go:101 msg_server.go:183 proposal_server.go:34 helpers.go:30 proposal_server.go:33 tx.pb.go:299 msg_service_router.go:175 tx.pb.go:301 msg_service_router.go:198 prouter.go:74 abci.go:520 cmt_abci.go:40 abci.go:85 local_client.go:164 app_conn.go:89 execution.go:166 state.go:1381 state.go:1338 state.go:2055 state.go:910 state.go:836 asm_amd64.s:1695]"
  WARN Verifying proposal
  ```

  Solution:

  * It looks like port 8551 stopping, the background process running `iptables` blocking ip and port and access posix.
  * For solution try uninstall `ufw posix` and `iptables`:

  ```bash
  iptables -I INPUT -s localhost -j ACCEPT
  ```
</details>

# Staking Design
# Purpose

This document walks through the staking specification for Story. The goal is to provide clarity to network participants and technical partners on how Story’s staking mechanics work and how users can interface with our chain.

# Tokenomics

## Genesis

The story genesis allocation will consist of 1 billion tokens, distributed among ecosystem participants, the foundation, investors, and the core team.  Please refer this document for the detailed [Token Distribution](https://www.story.foundation/blog/introducing-ip).

## Locked vs Unlocked tokens

Unlocked tokens have no restrictions imposed on them and can be used for gas consumption, transfers, and staking.

Unlike unlocked tokens, locked tokens cannot be transferred or traded and are unlocked based on an unlock schedule. However, locked tokens may be staked to earn staking rewards, with the locked staking reward rate being half of that of unlocked tokens.

Staked locked and unlocked tokens have the same voting power. That means that a validator with 100 staked locked tokens has the same network voting power as a validator with 100 staked unlocked tokens.

Both types of tokens can be slashed if their validators get slashed.

## Token emissions

A fixed number of tokens will be allocated for emissions in the first year, with the quantity determined by the foundation at Genesis. For subsequent years, the number of emitted tokens will be controlled by an emissions algorithm whose parameters may be updated via governance or subject to change via hard forks. The emissions per block are controlled by the following two parameters:

* blocks\_per\_year: 10368000 blocks
  * The number of blocks expected to be produced in a year
* inflations\_per\_year: 20,000,000 tokens
  * The total number of inflationary tokens to be emitted in a year

New emissions will flow to two places:

1. Block Rewards
2. Community Pool

## Token burn

Since Story uses a fork of geth as the execution client, the burning mechanism follows Ethereum’s EIP-1559.

# Staking

> 🔗 <a href="https://staking.story.foundation/" target="_blank">Stake with the Staking Dashboard ↗️</a>

Story supports the below staking-related operations

* Create validator
* Update validator commission
* Stake
* Stake on behalf
* Unstake
* Unstake on behalf
* Redelegate
* Redelegate on behalf
* Set withdraw address
* Set reward address
* Unjail
* Unjail on behalf

Before explaining the behavior of each of these operations, some high-level concepts like **Token Staking Types**, **Validator Set Status**, **Unbonding**, and **Staking Period** will be explained first:

## Token Staking Types

As staking is enabled for both locked and unlocked tokens, validators must choose which type of token staking they want to support. Once a token staking type is selected, validators cannot switch to a different type.

## Validator Set Status

In Story, validators are grouped into one of two sets, (1) the active (bonded) validator set, which participates in consensus and receives block rewards, or (2) the non-active (unbonded) validator set, which does not contribute to the consensus process. To be selected as part of the active validator set, a validator must be one of the top 64 validators ranked by staked tokens.

## Unbonding

Unstaking for delegators is subject to an unbonding process. Users must wait for an unbonding time before any tokens return to their accounts.

This is the same for validators who self-delegate to themselves. They also need to go through the unbonding process when they want to unstake.

The unbonding time is 14 days. During the unbonding period, the delegator/validator will not earn block rewards. But they may still be slashed.

For each validator/delegator pair, the maximum ongoing unbonding transactions is 14. More unbonding requests beyond this limit will fail.

## Staking period

Delegators can decide how flexible and how long they want to stake their tokens. By default, for both locked and unlocked tokens, delegators can stake and then unstake immediately and get their token back after the unbonding time. We call this **flexible staking** in this document.

For unlocked tokens, a few more fixed staking periods are supported: 90 days, 360 days, and 540 days. In this case, users can only call unstake after the staking period is mature. Any call earlier than the mature day will be discarded. Unstaking from a mature staking period is still subject to the unbonding process, meaning users will get their staked tokens back after 14 days of unbonding time.

Staking in these fixed staking periods earns more rewards. The longer the period, the bigger the reward weight multiplier. Reward multiplier for different periods:

* Locked flexible period - **0.5**
* Flexible period - **1.0**
* 90 days - **1.1**
* 360 days - **1.5**
* 540 days - **2**

For locked tokens, only flexible staking is allowed and the reward multiplier is **0.5**. If a user delegates their locked tokens to a staking period, we will convert that to a flexible staking delegation.

After the staking period ends, users can choose not to unstake. In this case, they will continue earning the same reward rate based on the reward rate of the corresponding staking period until they unstake manually. They can unstake at any time after the staking period ends. For example, if the 1-year staking period’s reward rate is 0.02% per block, after staking for 1 year, users can still earn 0.02% per block of the reward until they unstake.

# Staking Operations

## Create validator

To become a validator, the validator must first run a validator node based on the latest released story binaries, then call the CreateValidator function with an initial staking amount, moniker, and commission rate. It also needs to set the max commission rate and max commission rate change to make sure it doesn’t change the commission rate later dramatically. The minimum commission rate that a validator can set is 5%.

The initial staking amount needs to be larger than a threshold, which is 1024 IP. The amount will be deducted from the caller’s wallet. It can only be staked to a flexible period.

If a validator tries to call create validator function the second time, it will be ignored.

## Update validator commission

This operation allows validators to edit their validator commission rate. If the updated commission rate is larger than max commission rate or the commission rate change delta is larger than max commission rate change, the operation will fail.

A fee of 1 IP will be charged for updating a validator to prevent spamming. The fee will be burnt by the contract.

The commission rate can only be updated once per day. It will not throw an error from the contract. But it won’t take effect in the consensus layer.

## Stake

Both the validator and delegator can stake tokens to a validator. A validator can stake to itself, which is called self-delegation. Users can decide if they want to stake with a fixed staking period or stake without a period (flexible staking).

If a fixed period is chosen, a delegation id will be returned to the users. Users must use this delegation id to unstake tokens from this stake operation. If flexible staking is chosen, the returned delegation id will be 0.

The staking amount needs to be larger than a threshold, which is 1024 IP.

If a delegator delegates to a non-existent validator, the tokens will NOT be refunded.

## Unstake

When staking without a staking period, users can unstake anytime. The tokens will be distributed to the user’s account after the unbonding time.

A fee of 1 IP will be charged for unstaking to prevent spamming. The fee will be burnt by the contract.

When staking with a staking period, users can only unstake after the staking period is mature. The tokens will be distributed to the user’s account after the unbonding time. Unstaking requests before the staking period matures will be ignored.

The minimum unstaking amount is 1024 IP. After the unstaking request is processed, if the remaining staked amount is less than 1024 IP, the remaining part will also be unstaked together.

The unstaking request will first go through the unbonding process, which is 14 days. After that, the unbonded requests are sent to a withdrawal queue, distributing a maximum of 32 withdrawals per block. If there are more than 32 withdrawal requests in the withdrawal queue, the next 32 withdrawal requests will be processed in the next block.

Partial unstake of a delegation is supported. For example, if a 1-year long delegation has 1 million tokens, after 1 year, users can unstake 500k from this delegation and keep the remaining staked to continue earning rewards.

Unstake can fail if the validator, delegator and delegation id passed in is incorrect.

Unstake can also fail if the maximum concurrent unbonding request (currently 14) has been reached for the validator/delegator pair.

If the unstake amount passed in is larger than the total unstakable tokens, the current total unstakable amounts will be unstaked. For example, if users unstake 1024 IP and only have 1023 IP stake, 1023 IP will be withdrawn.

If a validator exits, by either being offline and getting jailed, or not having enough stakes to be in the top 64 validator set, the delegators can unstake their tokens if the tokens are not in a staking period or their staking period is mature. Otherwise, delegators must wait until the staking period matures to unstake.

## Redelegate

Redelegate operation allows a delegator to move its staked tokens from one validator to another. The tokens can be redelegated to the new validator immediately and start earning rewards. However, the redelegated tokens are still subject to the unbonding process, IF the source validator is in the active validator set or unbonding from the active validator set. During this 14 days unbounding time, it will be slashed if the original validator gets slashed.

A fee of 1 IP will be charged for redelegation to prevent spamming. The fee will be burnt by the contract.

The minimum redelegation amount is 1024 IP. If a delegator’s initial stake is 1024 IP but later gets slashed, it can still redelegate its tokens to another validator even if the token amount is less than 1024 IP.

Similarly to unstaking, if the redelegation amount passed in is larger than the total redelegatable tokens, the total redelegatable amounts will be redelegated. If the remaining balance after redelegation is less than 1024 IP, all remaining tokens will be redelegated together.

The delegation id will stay the same after the redelegation.

Redelegation has its own maximum ongoing unbonding transaction limit per delegator/source validator/destination validator pair, which is also 14.

Delegators can choose to redelegate their tokens to another active validator even if their tokens are still in an immature staking period. Their staking period maturation date and reward rate will stay the same.

Redelegation can only be triggered when the source and destination validators support the same token type.

## Set withdrawal/reward address

Delegators can call the staking contract to set a withdrawal address. The unstaked tokens will be sent to this withdrawal address. Similarly, delegators can set a separate reward address. All reward distributions will be sent to this address.

A fee of 1 IP will be charged for updating either the withdrawal address or the reward address to prevent spamming. The fee will be burnt by the contract.

The address change will take effect in the next block.

## Slash/Unjail

Slashing penalizes bad behaviors on the validators by slashing out a fraction of their staked tokens. Two types of behaviors can get slashed in Story: **double sign** and **downtime**.

* **double sign**: If a validator double signs for a block, they will get slashed 5% of their tokens and get permanently jailed (called tombstoned).
* **downtime**: If a validator is offline for too long and misses 95% of the past 28,800 blocks, they will get slashed 0.02% of their tokens and get jailed.

A validator will also get jailed after self-undelegation if the validator’s remaining self-delegation amount is smaller than the minimum self-delegation (1024 IP).

A jailed validator cannot participate in the consensus and earn any reward. But they can unjail themselves after a cooldown time, which is currently set to 10 minutes. After 10 minutes, it can call story’s staking contract to unjail itself IF their stake is more than minimum stake amount (1024 IP), after which it can participate in the consensus again if it’s still within the top 64 validators.

A jailed validator can still withdraw all their stakes.

Delegators can still stake and unstake from a jailed validator as long as there are remaining stakes on this jailed validator. The jailed validator will only be removed from the chain (hence not able to be staked/unstaked) when there is no remaining stake on it.

A fee of 1 IP will be charged for unjailing a validator to prevent spamming. The fee will be burnt by the contract.

## On behalf functions

Most of the staking-related operations can be done from another wallet on behalf of the validators or delegators. Most of these on-behalf functions are permissionless since they spend tokens from the wallet that calls the on-behalf operations, not from the actual validators or delegators.

## Add operator

If a delegator wants to allow another wallet to unstake or redelegate on their behalf, they must call the staking contract to add that wallet as the operator for their delegator. After that, the operator can unstake and redelegate the delegator’s tokens on behalf of the delegator.

The same applies to a validator who wants to allow another wallet to unjail on its behalf.

A fee of 1 IP will be charged for adding an operator.

## An additional data field

Each function will include an additional unformatted `data` input field to accommodate potential future changes. It can avoid changing user interfaces in the future.

## Validator key format

Validator public keys are secp256k1 keys. The keys have a 33 bytes compressed version and 65 bytes uncompressed version. When interacting with the story's smart contracts, a 33 bytes compressed key is used to identify validators.

# Rewards

## Rewards Pool Allocation

For every block, a fixed proportion of token inflation will go to the rewards distribution pool, which will be shared among all 64 active validators according to each of their share weights. *These allocated tokens will then be shared among the validator and its delegators in a fashion described by the next section.* The validator share weight is calculated based on the total token staking amount, and whether or not the token staking type is locked or unlocked.

As an example, assume that we have 100 tokens allocated for the validator rewards distribution pool, and assume that we only have 3 active validators:

* validatorA with 10 locked tokens staked
* validatorB with 10 locked tokens staked
* validatorC with 10 unlocked tokens staked

To calculate how many tokens each validator receives, we first calculate each of their weighted shares, which is defined as the number of staked tokens multiplied by their rewards multiplier (0.5 if staking locked tokens, 1 if staking unlocked tokens). This gives us:

* validatorA with 10 \* 0.5 = 5 shares
* validatorB with 10 \* 0.5 = 5 shares
* validatorC with 10 \* 1 = 10 shares

With the weighted and total shares calculated, we can then get the total number of inflationary tokens allocated for each validator:

* validatorA with 100 \* (5 / 20) = 25 tokens
* validatorB with 100 \* (5 / 20) = 25 tokens
* validatorC with 100 \* (10 / 20) = 50 tokens

The formula for calculating the total number of tokens allocated for a validator is as follows:

<Image align="center" src="https://files.readme.io/833d419fc139ba363c56aef263dcca571fe449ab824a2349a69d7419ee658bd0-Screenshot_2024-10-30_at_8.13.27_PM.png" />

where

* R\_i is the total inflationary token rewards for validator i
* S\_i is the staked tokens for validator i
* M\_i is the rewards multiplier (0.5 for locked tokens, 1 for unlocked tokens)
* R\_total is the total inflationary tokens allocated for the rewards pool

## Validator And Delegator Rewards

Total rewards allocations (*whose calculations are shown in the prior section*) for each validator are shared between the validator itself and all of its delegators:

* The validator takes a fixed percentage commission, set by the validator itself
* Remaining rewards are distributed among delegators according to their share weights

Calculation of delegator rewards is similar to that of validator rewards, where the proportion of tokens received for each delegator out of the remaining validator rewards is calculated based on each delegator’s staking multiplier (described in the staking section).

As an example, assume a validator has 100 total rewards allocated to it, with a validator commission of 20%, and 3 delegators delegating to it:

* delegatorA with 10 tokens staked and a staking multiplier of 1
* delegatorB with 10 tokens staked and a staking multiplier of 1
* delegatorC with 10 tokens staked and a staking multiplier of 2

To calculate how many tokens each delegator receives, we first calculate each of their weighted shares, which is defined as the number of staked tokens multiplied by their staking rewards multiplier. This gives us:

* delegatorA with 10 \* 1 = 10 shares
* delegatorB with 10 \* 1 = 10 shares
* delegatorC with 10 \* 2 = 20 shares

With the weighted and total shares calculated, we can then get the total number of inflationary tokens allocated for each delegator, noting that the total number of tokens to be distributed among delegators is give by 100 - (100 \* 0.20) = 80:

* delegatorA with 80 \* (10 / 40) = 20 tokens
* delegatorB with 80 \* (10 / 40) = 20 tokens
* delegatorC with 80 \* (20 / 40) = 40 tokens

The formula for calculating the delegator token reward can be found below:

<Image align="center" src="https://files.readme.io/429c0eff2f0acddcabfa3e6259e427b47156aed244020bfb7f11a5b63387fec9-Screenshot_2024-10-30_at_8.15.51_PM.png" />

where

* D\_i is the total inflationary token rewards for delegator i
* S\_i is the staked tokens for delegator i
* M\_i is the staked rewards multiplier for delegator i
* R\_total is the total inflationary tokens allocated for the validator
* C is the commission rate for the validator

The validator commission is also treated as a reward and will follow the same auto-reward distribution rule described below. The minimal validator commission is set to 5% to avoid a cut-throat competition of lower commission rates among validators.

The reward calculation results will be rounded down to gwei. Anything smaller than 1 gwei will be truncated.

## Auto reward distribution

The reward is accumulated per block and can be distributed per block. However, it will only be automatically distributed to the delegator’s account when it is larger than a threshold. The default and also minimal threshold is 8 IP, which means that only if the delegator’s reward is more than 8 IP, it will be sent to the delegator’s account.

The reward distribution will go to a reward distribution queue, which only processes a fixed amount of reward distribution requests per block. The reward distribution per block is 32.

The staking reward cannot be manually withdrawn by design.

# Community Pool

A percentage of the newly minted tokens in every block will go to a community pool contract. The foundation will determine how to use the tokens sent to the pool. The maximum community pool percentage that can be set is 20%.

The community pool contract address: **0xcccccc0000000000000000000000000000000002**

# Singularity

The first 1,580,851 blocks after the genesis is called Singularity, during which everyone can create a validator and stake tokens but the active validator set will only have the genesis validators. There is also no new token emission, hence no reward. Unstake and redelegate are also not supported.

The Genesis validator set consists of 8 validators, setup by the foundation and trusted staking institutions. 4 of them support locked tokens and the other 4 support unlocked tokens. Each of them has an initial stake of 0.001 IP. Each of them will set a commission rate. During the Singularity, the genesis valdiators will need to self delegate at least 1024 IP to perform validator operations like editing validator commission rate.

After Singularity, the top 64 validator nodes with the highest stakes will be selected to participate in consensus and receive rewards.

Slashing/Jail won’t happen during Singularity.

# Staking contract

Story’s staking contract will handle all validators/delegators related operations. It’s deployed to address: **0xcccccc0000000000000000000000000000000001**

The contract interfaces are defined here: [https://github.com/piplabs/story/blob/main/contracts/src/protocol/IPTokenStaking.sol](https://github.com/piplabs/story/blob/main/contracts/src/protocol/IPTokenStaking.sol)

# What is Story
<Image align="center" src="https://files.readme.io/30567679bc8ee50fe55d31b026f751e3535b21f9b2ed52ae7a6777cfd094ee5c-image_6.png" />

# Introducing the World's IP Blockchain

Story is a purpose-built layer 1 blockchain designed specifically for intellectual property. Here's the rundown:

* Story allows you to register your IP on the blockchain. This IP could be an image, a song, an RWA, AI training data, or anything in-between.
* You can add usage terms to your IP, which specifies how others can use it, like "You owe me 50% of your commercial revenue if you use my IP", or "You cannot create derivatives of my IP".
* By making IP programmable on the blockchain, it becomes this transparent & decentralized global IP repository where AI agents (or any other software) and humans alike can transact on IP.

<Cards columns={2}>
  <Card title="Skip the Read - 1 Minute Summary" href="https://docs.story.foundation/docs/explain-like-im-five" icon="fa-home">
    Want to skip to a summary? Check out our "Explain Like I'm Five" for a super fast & easy to understand explanation of everything Story.
  </Card>

  <Card title="Read the Whitepaper" href="https://www.story.foundation/whitepaper.pdf" icon="fa-file" target="_blank">
    Read the Story whitepaper.
  </Card>
</Cards>

<Accordion title="Why did we build Story?" icon="fa-question">
  When IP owners share their work online, it’s easy for others to use or change it without crediting them, and they often don't get paid fairly if their work becomes popular or valuable. This can be discouraging for people who want to share their ideas and creations but don’t want to lose control over them.

  Additionally, the sheer speed and superabundance of AI-generated media is outpacing the current IP system that was designed for physical replication. In many cases, [AI is trained on and is producing copyrighted data](https://twitter.com/BriannaWu/status/1823833723764084846).

  Story fixes this by providing a way for creators to share their work with built-in protection. When someone (including an AI model) uses a song, image, or any creative work that’s registered on Story, the system automatically tracks who the original owner is and makes sure they get credited. Plus, if that work generates revenue—say someone remixes a song and it earns money—the original owner automatically receives their fair share based on license terms that were set on-chain.
</Accordion>

## The "How"

There are several elements that make up Story as a whole. Below we will cover the most important components: Story Network (the L1), the Proof-of-Creativity Protocol (the smart contracts), and the Programmable IP License.

![](https://files.readme.io/56f3c96-image.png)

### Story Network: "The World's IP Blockchain"

Story Network is a purpose-built layer 1 blockchain achieving the best of EVM and Cosmos SDK. It is 100% EVM-compatible alongside deep execution layer optimizations to support graph data structures, purpose-built for handling complex data structures like IP quickly and cost-efficiently. It does this by:

* using precompiled primitives to traverse complex data structures like IP graphs within seconds at marginal costs
* a consensus layer based on the mature CometBFT stack to ensure fast finality and cheap transactions

### Proof-of-Creativity Protocol

Our "Proof-of-Creativity" Protocol, made up of smart contracts, is natively deployed on Story Network and allows anyone to onramp IP to Story. Most of our documentation focuses on the protocol.

Creators can register their IP as [🧩 IP Assets](doc:ip-asset) on our protocol. IP Assets (IPA) are the foundational programmable IP metadata on the protocol. Each IPA is composed of:

1. an on-chain NFT. This could be an existing NFT like [Azuki](https://www.azuki.com/en) that itself is the IP, or a new NFT specifically minted to represent some off-chain IP like a real-world asset.
2. its associated IP Account, which is a modified ERC-6551 (Token Bound Account) implementation.

To interact with IP Assets within the protocol, we use “**modules**” such as the licensing, royalty, and dispute modules. For example, the owner of an IP Asset can set **terms** on their IP such as whether or not derivative works can use the IP commercially (and at what cost). Creators of derivative works can then seamlessly extend their work by minting “**License Tokens**” - outlined by the terms and also represented as NFTs themselves - using the [📜 Licensing Module](doc:licensing-module), create revenue streams from derivative works using the [💸 Royalty Module](doc:royalty-module), or raise a dispute using the [❌ Dispute Module](doc:dispute-module).

### Programmable IP License

Although on-chain, an IP's usage terms and minted licenses are enforced by an off-chain legal contract called the [Programmable IP License (PIL💊)](doc:programmable-ip-license-pil), which allows anyone to offramp tokenized IP into the off-chain legal system and outlines real legal terms for how creators can remix, monetize, and create derivatives of their IP. *The protocol, or more specifically the IP Assets and modules described above, are what automate and enforce those terms on-chain*, creating a mapping between the legal world (PIL) and the blockchain.

Like USDC enables redemption for fiat, the PIL enables redemption for IP.

## An Example

**Example #1**: Imagine you're an artist who creates digital paintings, or a musician who makes original songs. You want to share your work online, but you want to ensure that if others use or change your work, they give you credit and—if they make money from it—you get a share. That’s where Story comes in. It's a platform that uses technology to give IP owners like you control over how your work is used, tracked, and shared, so it’s both protected and fairly rewarded.

Think of it like this: Suppose you upload a song to Story. Now, anyone can see that you’re the original creator, and if someone wants to remix it, they can do so through Story. The system then automatically tracks the remix as a "derivative" of your song and notes you as the original artist. This way, if the remix becomes popular and earns money, Story   can help you earn a portion of those earnings, just like the remixer.

**Example #2**: Let’s say a scientist uploads an image dataset to be used by artificial intelligence (AI) models for research. Through Story, that dataset is registered, so if any company uses it to train their AI, the original scientist is credited. If that dataset then contributes to a profitable AI application, Story ensures a fair share goes to the original contributor.

With Story, you can share your work freely, knowing that wherever it goes, it’s tracked and fairly credited back to you. The idea is to create a fair environment for sharing, building upon, and growing creative work.

# For AI Agents
This page is all about AI Agents. We have prepared a way for you to use our documentation as training data which can be seen below, or continue to learn about developing AI Agents on Story.

<Cards columns={2}>
  <Card title="Train on Our Docs" href="https://github.com/storyprotocol/documentation/blob/v1.2/combined.md" icon="fa-robot" iconColor="#4e8189" target="_blank">
    Looking to feed our docs into your AI Agent so it can use it as training data? Check out this file, which contains all of our docs in one combined `.md` file.
  </Card>

  <Card title="Read the Whitepaper" href="https://story.foundation/whitepaper.pdf" icon="fa-file" iconColor="#cfb394" target="_blank">
    Read our Agent TCP/IP whitepaper, which defines an agent-to-agent transaction system to enable a future of AGI.
  </Card>
</Cards>

Below you will find two sections:

1. **Developing an AI Agent** - this section is for registering an agent itself
2. **Implementing ATCP/IP** - this section is for implementing the ***2. An ATCP/IP Transaction*** section of the <a href="https://story.foundation/whitepaper.pdf" target="_blank">:page_with_curl: Whitepaper</a>.

## :robot: Developing an Agent

Below are details on how to:

* Register an AI Agent as IP
* Add License Terms to your AI Agent

### Registering an Agent

In order to register an AI Agent (or any IP) on Story, follow the [How to Register IP on Story](https://docs.story.foundation/docs/how-to-register-ip-on-story) tutorial. The only difference for AI Agents is how you structure your IP Metadata, which should follow the [IPA Metadata Standard](https://docs.story.foundation/docs/ipa-metadata-standard).

Here is an example of what the IP Metadata should look like for your AI Agent (using [Benjamin](https://x.com/BenjaminOnIp) as an example):

```json
{
  title: "Benjamin",
  description: "Benjamin is the official mascot of Unleash Protocol and the First IPFi AI Agent.",
  ipType: "AI Agent",
  creators: [
    {
      name: "Unleash Protocol",
      address: "0xBBE1725E1Fef9C2C20Ac6CB984bB0E970Ab18aa9",
      contributionPercent: 100,
      description: "Benjamin is the official mascot of Unleash Protocol and the First IPFi AI Agent.",
      socialMedia: [{ platform: "twitter", url: "https://x.com/BenjaminOnIP" }]
    }
  ],
  tags: ["Benjamin", "Unleash Protocol", "Story", "AI Agent", "IPFI"],
}
```

### Adding Terms to your AI Agent

Upon registering your AI Agent, you can add license terms to it. However you can add more license terms to your AI Agent afterwards as well. Follow the below tutorials to learn how to do this:

<Cards columns={2}>
  <Card title="Using the SDK" href="https://docs.story.foundation/docs/attach-terms-to-an-ip-asset" icon="fa-home" target="_blank">
    Learn how to attach terms to your AI Agent using the SDK.
  </Card>

  <Card title="Using the Smart Contracts" href="https://docs.story.foundation/docs/sc-attach-license-terms" icon="fa-home" target="_blank">
    Learn how to attach terms to your AI Agent using the Smart Contracts.
  </Card>
</Cards>

## :page_facing_up: Implementing ATCP/IP

Below are details on how to actually implement the ***2. An ATCP/IP Transaction*** section of the <a href="https://story.foundation/whitepaper.pdf" target="_blank">:page_with_curl: Whitepaper</a>.

### Registering your Agent's Outputs

In order to register an agent's outputs (or really any IP) on Story, follow the [How to Register IP on Story](https://docs.story.foundation/docs/how-to-register-ip-on-story) tutorial. The only difference is how you structure your IP Metadata, which should always follow the [IPA Metadata Standard](https://docs.story.foundation/docs/ipa-metadata-standard).

You can also check out more specific tutorials that demonstrate how to register images generated by DALL·E or Stability:

* [Protect DALL·E AI-Generated Images](doc:protect-dalle-ai-generated-images)
* [Register & Monetize Stability Images](doc:register-stability-images)

Here is an example of what the IP Metadata should look like for your generated IP:

```json
{
  title: 'Image of a Baby Sea Otter',
  description: 'An image generated by Dall-E 2',
  ipType: 'image',
  attributes: [
    {
      key: 'Model',
      value: 'dall-e-2',
    },
    {
      key: 'Prompt',
      value: 'A cute baby sea otter',
    },
  ],
  creators: [
    {
      name: 'Jacob Tucker',
      contributionPercent: 100,
      address: account.address,
    },
  ]
}
```

### Creating Agreement Terms

As described in the <a href="https://story.foundation/whitepaper.pdf" target="_blank">:page_with_curl: Whitepaper</a>, agents will negotiate on what agreement terms are appropriate for the requested task:

<Accordion title="Whitepaper Section" icon="fa-info-circle">
  2 **Terms formulation**: The provider agent will consider the request and choose an appropriate set of\
  license terms for the information being requested. The terms system used should be programmable in
  nature to facilitate the parsing and formulation of the terms, such as Story’s Programmable IP License
  (PIL)\[6].

  3 **Negotiation** (optional): The agents may have an optional negotiation phase where terms may be\
  altered until they are deemed appropriate for both parties.

  * **Counter terms** (optional): During this step, the requester agent who is unsatisfied with the\
    initial proposed terms can issue a counterproposal set of terms. Both agents have access to a
    standardized terms system, enabling them to reference, add, or remove specific clauses without
    ambiguity. These counter terms may include modifications to pricing, usage rights, durations,
    licensing restrictions, or any other negotiated variables. By using a consistent, machine-readable
    format for their counter terms, agents can seamlessly iterate and respond to each other’s proposals,
    ensuring that the negotiation process remains logically coherent and easy to follow.
  * **Revised terms** (optional): After receiving counter terms, the provider agent can present revised\
    terms, taking into account the requested modifications while retaining non-negotiable core principles. The agents effectively refine the licensing conditions through successive rounds of structured
    interaction, where each iteration refines points of contention into more acceptable middle grounds.
    Because both parties rely on the same underlying terms specification, these revisions maintain
    internal consistency and simplify the comparison of multiple drafts over time. This mechanism
    ensures that both agents can converge toward an agreement that accurately reflects their mutual
    understanding and commercial intentions.
  * *This process could have multiple iterations until an agreement is reached*
</Accordion>

Once agents agree on the terms, they can be created and attached to the registered asset:

<Cards columns={2}>
  <Card title="Using the SDK" href="https://docs.story.foundation/docs/attach-terms-to-an-ip-asset" icon="fa-home" target="_blank">
    Learn how to attach terms to your IP using the SDK.
  </Card>

  <Card title="Using the Smart Contracts" href="https://docs.story.foundation/docs/sc-attach-license-terms" icon="fa-home" target="_blank">
    Learn how to attach terms to your IP using the Smart Contracts.
  </Card>
</Cards>

### Mint a License

As stated in the <a href="https://story.foundation/whitepaper.pdf" target="_blank">:page_with_curl: Whitepaper</a>, after agents have negotiated on a set of terms, the requester agent can mint a license from the provider agent with specific agreement terms attached:

<Accordion title="Whitepaper Section" icon="fa-info-circle">
  4 **Acceptance**: The requester agent will formally accept the terms by minting an immutable token (the\
  agreement token) that encapsulates the terms and rules by which the information being provided is
  to be used. Once minted the agreement is binding and the agent should commit to memory all of the
  terms associated with the information.

  * **Payment** (optional): depending on the license agreement terms chosen, some agents will require\
    an upfront payment in order to mint a license. Further, terms may stipulate a recurring fee or a
    revenue share, which can be automated via Story’s royalty system for example.
</Accordion>

Once agreement terms are attached to an IP Asset, a [License Token](doc:license-token) can be minted:

<Cards columns={2}>
  <Card title="Using the SDK" href="https://docs.story.foundation/docs/mint-a-license" icon="fa-home" target="_blank">
    Learn how to mint a License Token using the SDK.
  </Card>

  <Card title="Using the Smart Contracts" href="https://docs.story.foundation/docs/sc-mint-license-token" icon="fa-home" target="_blank">
    Learn how to mint a License Token using the Smart Contracts.
  </Card>
</Cards>

Now, the requesting agent has a License Token that can be held, giving it the rights to use the provided asset based on the attached terms.

### Claim Revenue

Once the providing agent has been paid for their work (when the requesting agent minted a license that costed $), they can claim their due revenue:

<Cards columns={2}>
  <Card title="Using the SDK" href="https://docs.story.foundation/docs/claim-revenue" icon="fa-home" target="_blank">
    Learn how to claim revenue using the SDK.
  </Card>

  <Card title="Using the Smart Contracts" href="https://docs.story.foundation/docs/sc-claiming-royalty" icon="fa-home" target="_blank">
    Learn how to claim revenue using the Smart Contracts.
  </Card>
</Cards>

# Explain Like I'm Five
<Image alt="Credit to the original tweet [here](https://x.com/devrelius/status/1812865477657694513)." align="center" src="https://files.readme.io/0de1024-quick-story-overview.png">
  Credit to the original tweet [here](https://x.com/devrelius/status/1812865477657694513).
</Image>


# Quickstart
You want to start building on Story quickly... so let's get started!

> 📘 Looking to read up on Story first?
>
> If you'd like to read up on Story before diving into the technical details, check out our awesome [Learn Hub](https://learn.story.foundation/) which will explain the who, what, and why of Story.

## :fast_forward: Skip everything. Go to the code.

<Cards columns={2}>
  <Card title="TypeScript Code Example" href="https://github.com/storyprotocol/typescript-tutorial/tree/main" icon="fa-user" target="_blank">
    This is a clone-able quickstart for you to check out. You can clone it directly and follow the associated README.
  </Card>

  <Card title="Smart Contract Code Example" href="https://github.com/storyprotocol/story-protocol-boilerplate" icon="fa-user" target="_blank">
    This is a boilerplate for you to check out. You can clone it directly, study the example smart contracts, and follow the associated README for running the tests.
  </Card>
</Cards>

## :building_construction: Story Network Infra

See [Network Info](doc:network-info) for all RPC, explorer, and faucet info.

## :computer: Use our SDKs

We have built a [🛠️ TypeScript SDK](doc:typescript-sdk) with its own in-depth tutorials for popular functions and use cases.

## :gear: Deployed Smart Contracts

Check out the addresses for the deployed smart contracts [here](doc:deployed-smart-contracts). Note that there are two different kinds of contracts:

* [Story Protocol Core](https://github.com/storyprotocol/protocol-core-v1) - This repository contains the core protocol logic, consisting of a thin IP registry (the [IP Asset Registry](doc:ip-asset-registry)), a set of [🧱 Modules](doc:story-modules) defining logic around [📜 Licensing](doc:licensing-module), [💸 Royalty](doc:royalty-module), [❌ Dispute](doc:dispute-module), metadata, and a module manager for administering module and user access control.
* [Story Protocol Periphery](https://github.com/storyprotocol/protocol-periphery-v1)- Whereas the core contracts deal with the underlying protocol logic, the periphery contracts deal with protocol extensions that greatly increase UX and simplify IPA management. This is mostly handled through the [📦 SPG](doc:spg).

## :globe_with_meridians: Use our API

Check out the entire [API Reference](https://docs.story.foundation/reference/api-introduction) for learning how to use our API.

## :memo: Register IP on Story

Let's start with the most basic question: *"What does it take to register IP on Story in my app? How do I do this?"*

To register IP on Story, you'll first need an NFT. If your IP is an ERC-721 NFT (ex. an Azuki or Pudgy Penguin on Story), you're already set. If not, you must mint an NFT to represent your off-chain IP. And don't worry, we'll help you do this in the following tutorials.

Next you'd register that NFT on Story, ultimately creating an [🧩 IP Asset](doc:ip-asset). An "IP Asset" is your IP registered on Story, empowered by:

* all of Story's [🧱 Modules](doc:story-modules) like transparent licensing, automatic royalty payments, and disputing of wrongfully registered IP
* IP protection through the [💊 Programmable IP License (PIL)](doc:programmable-ip-license)

Follow the below tutorials to register IP on Story:

<Cards columns={2}>
  <Card title="Using the SDK" href="https://docs.story.foundation/docs/register-an-ip-asset" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    Learn how to register IP on Story using the TypeScript SDK.
  </Card>

  <Card title="Using the Smart Contracts" href="https://docs.story.foundation/docs/sc-register-an-ip-asset" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    Learn how to register IP on Story using the Smart Contracts.
  </Card>
</Cards>

### Difference Between IP Metadata vs. NFT Metadata

A common question we get from developers while registering their IP on Story is: *"What metadata should be/is expected to be attached to the NFT, and then separately, the IP Asset?"*

To answer that question, please see [NFT vs. IP Metadata](https://docs.story.foundation/docs/ip-asset#nft-vs-ip-metadata).

## :scroll: Licensing Your IP

You may be wondering, *"How do I take advantage of Story's on-chain licensing? How do I make sure my registered IP has a license ready to go?"*

Before you attach any sort of licenses or license terms to your [🧩 IP Asset](doc:ip-asset), it would be best to first understand what the [💊 Programmable IP License (PIL)](doc:programmable-ip-license) actually is. This "PIL" is what defines the available [License Terms](doc:license-terms) on Story, which in turn - when attached to an IP Asset - is what defines how others can use (commercially, create derivatives, etc) that IP Asset.

Our tutorials will show you exactly how to attach license terms to your IP Asset:

<Cards columns={2}>
  <Card title="Using the SDK" href="https://docs.story.foundation/docs/attach-terms-to-an-ip-asset" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    Learn how to attach license terms to your IP on Story using the TypeScript SDK.
  </Card>

  <Card title="Using the Smart Contracts" href="https://docs.story.foundation/docs/sc-attach-license-terms" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    Learn how to attach license terms to your IP on Story using the Smart Contracts.
  </Card>
</Cards>

> 📘 Learn More
>
> For more information on licensing and the terminology behind it, check out the [📜 Licensing Module](doc:licensing-module).

## :money_with_wings: Royalties / Revenue Sharing

Now you may be wondering, *"How do I set up automatic royalty sharing between my IP Asset and someone else's? How do I then collect that payment?"*

When you attach [License Terms](doc:license-terms) to your [🧩 IP Asset](doc:ip-asset), you can specify certain commercial terms such as `commercialRevShare`, which is the amount of revenue (from any source, original & derivative) that must be shared by derivative works with the original IP. See the above section for licensing questions.

If someone then creates a derivative of my IP Asset - which has a `commercialRevShare` of let's say 10% in its license terms - and earns revenue on it, Story enforces the share of this revenue through the [💊 Programmable IP License (PIL)](doc:programmable-ip-license) (otherwise resulting in an on-chain dispute using the [❌ Dispute Module](doc:dispute-module) or traditional legal arbitration) and then handles the upstream revenue share at the protocol level. If the derivative work earns 100 $WIP, my original IP Asset could claim 10 $WIP.

Our tutorials will show you exactly how to claim revenue:

<Cards columns={2}>
  <Card title="Using the SDK" href="https://docs.story.foundation/docs/claim-revenue" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    Learn how to claim revenue on Story using the TypeScript SDK.
  </Card>

  <Card title="Using the Smart Contracts" href="https://docs.story.foundation/docs/sc-claiming-royalty" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    Learn how to claim revenue on Story using the Smart Contracts.
  </Card>
</Cards>

> 📘 Learn More
>
> For more information on royalty and how it functions, check out the [💸 Royalty Module](doc:royalty-module).

# FAQ
## *"Is on-chain IP real?"*

Story isn't replacing the legal system, it's providing on-chain rails to make the legal system more efficient for creative IP.

We have worked with world class legal teams to craft a real, off-chain legal contract called the [Programmable IP License (PIL💊)](doc:programmable-ip-license) that has simple terms allowing creators to state who can remix, monetize, and create derivatives of their IP and at what cost.

We've then built business logic on-chain (in the form of smart contracts) to automate & enforce those terms. This creates a tight mapping between the legal world and our on-chain terms.

## *"My asset is off-chain. How do I register it as IP on Story Protocol?"*

You would mint an NFT that represents your off-chain asset, and register that NFT on Story.

## *"I am selling an off-chain asset (eg. merch). How do I make sure that automatic royalty flow is enforced?"*

Automatic royalty flow can be enforced off-chain. Although the royalty infrastructure is on-chain, the associated license is valid beyond the chain. To legally abide by the license any derivative work will need to pay royalties on-chain to the IP Account that is attached to your IP Asset, or face consequences like in the real world (e.g. being taken to court for abusing a license) according to the terms set out by the license.

Furthermore, one of the terms of the [Programmable IP License (PIL💊)](doc:programmable-ip-license-pil) is that licensees are obligated to provide revenue data for off-chain transactions (e.g. merch) to licensors, if there's a revenue share involved.

## *"What is a simple example?"*

Before Story, if you wanted to create a comic with someone else’s Azuki NFT and your Pudgy NFT, you would first need to hope the Azuki has a license and then find & contact the owner. You would also need to research your license terms for your Pudgy. Since you’re not a legal expert, you would probably need a lawyer to create a new contract between the two IPs. This creates a burden on anything ever getting created legally, because few people have the time, expertise, or money for that except large studios. Thus IP is not easily composable.

With Story, Azuki & Pudgy holders can register their IP as **IP Assets** and then set terms - using the PIL - outlining how people can license and remix their IP. If your Pudgy is registered on Story, anyone can see those terms on-chain and license your Pudgy automatically using the licensing module, which generates a real license agreement. You can then register the resulting comic as a derivative, and any revenue that flows in can be distributed between you and the IP holders automatically.

## *"What are the challenges?"*

The natural question arises: "*What if I'm a bad actor and I ignore all of this and just Right Click Save As?*"

First, its underestimated the extent to which people want to follow the law. This is why PIL is so important - all of the\
IP is not just on-chain, it is tied to a real legal contract! If people rip off your IP, they can be sued in court. However this "happy path" doesn't always happen. When things go wrong, we want to provide as many layers of escalation before resorting to off-chain arbitration.

Thus, we've created the [Dispute Module](doc:dispute-module) that allows anyone to flag violating content on-chain. If the dispute is\
successful, that IP will be flagged and no longer able to monetize or generate licenses.

The worst case scenario on Story is the best case scenario anywhere else: legal arbitration. Creators using us can always use the traditional legal system as a backstop. This is a situation we want to avoid, but one that's necessary for creators to have trust in the system.

## *“Why not be a protocol on every existing chain?”*

We started as a protocol, supporting projects on Polygon, Ethereum, and more. But this siloed IP to the chain that it lived on, and did not connect IP between chains. This does not realize the vision of Story as a global IP repo.

We want to be the IP settlement layer, or in other words, bring composability across chains. We need a single hub blockchain for all registered IP. This IP can be on Story, on other chains, or off-chain, but we need a hub such that all the licenses, royalties, and IP metadata live in a single unified execution environment.

## *"Okay, so you need a singular blockchain. Why not use an existing blockchain?"*

First and most obvious, building our own Layer 1 allows us to innovate across the entire technical stack. This enables us to implement essential features like on-chain dispute resolution or on-chain IP graphs without waiting on existing L1 roadmaps.

The need to innovate the entire stack ourselves was proven when we tried to build on a few existing chains and quickly realized it would not work due to technical limitations of those chains. Here are a few reasons we decided - or rather were forced - to build an L1:

1. Existing L1s are simply not able to handle the registration of IP upon 1000s of parents - each with their own complicated licensing details - in one transaction efficiently. As you can see below, gas usage on Geth EVM rapidly increases with ancestor IPs, making it impractical beyond 670 ancestors due to block gas limits. So we improved the efficiency of graph traversing and storing for our IP graph by leveraging a new stateful precompile approach, written directly in the execution client. This creates a "shortcut" around EVM to write and read certain values in the blockchain we use for our graphs, under the restrictions of PoC protocol.
2. Similarly, existing L1s are not able to handle the royalty token flow between 1000s of IP Assets. Again, this was solved by making improvements directly in the execution client.
3. Max composability by having license data on-chain, allows for 100s of IPs to remix at once.
4. Potential rollup support with bespoke X-CHAIN data (e.g. [Initia's](https://initia.xyz/) model) for Web2 apps with millions of transactions requiring rollups. Also support Web2 apps running their own validators for max incentive alignment.
5. Future roadmap for tokenization of IPs on Story Network (more info to come).
6. Future validator-enshrined L1 tech like native oracles for NFTs and off-chain RWA IPs (Cosmos SDK vote extensions).

Lastly, by building an L1, we provide a flexible foundation that supports diverse IP-specific L2 solutions. Such that, for example, each IP can potentially launch its own L2, driving our grand vision for the future of global IP.

## *"Why not just settle for an L2?"*

We actually tried to build an L2 before deciding to build an L1. However, we not only wanted to innovate the execution layer (also possible on L2), but add in a consensus mechanism, make improvements to validator node storage, and novel validator features.

For example, we eventually want to allow Netflix, TikTok, etc to run validators to stream IP data directly on-chain, and also allow those nodes to have graph DBs optimized for IP graphs. Imagine any transaction involving a disputed IP Asset being immediately rejected.

## *"Why is Story making improvements at the validator level?"*

The alternative is running an adjacent system outside the L1 to provide these features, which will strictly be less decentralized than the L1 itself. By making these features native to L1 (validators), we can have sufficient decentralization, guaranteed execution, and less system to maintain.

Namely, if the adjacent system goes down (so disputes and oracles stop) but the L1 works fine, it'd be a huge problem. This can be remedied with re-staking like AVS but the tech is not battle-tested and there's no precedence of success using re-staking (EIGEN token is still in work).

One big incentive alignment Story can have: IP companies running validators & providing custom off-chain IP data to the network natively via validator-enshrined features. 

Or a law firm automating disputes and broadcasting them to other validators. After an agreement of disputes, validators can immediately block transactions that include disputed IPs, which is not possible with an adjacent system providing (unless we have preconf, which is a debated topic in the Ethereum land).

## *"You mention needing an L1 to improve upon the efficiency of an IP graph. Why not build an L2 with off-chain graph indexing?"*

The IP graph must be on-chain because certain on-chain features require the ability to traverse and aggregate the IP graph. For example, royalty and revenue distribution need to occur on-chain through the IP graph. Using off-chain graph indexing would make these on-chain features either unfeasible or overly complex, as it would necessitate involving an oracle.

Similarly, the dispute module can check if an IP has been flagged due to its ancestor being flagged by traversing the IP graph directly on-chain, and then take immediate action such as aborting a transaction that would license a disputed IP without the need of an oracle.

## *"How will Story prevent users from registering off-chain IP that isn't theirs?"*

We will support a few ways, including the [Dispute Module](doc:dispute-module), to deter IP infringement. For example if someone were to register someone else's IP, it could be disputed on-chain. And in the worst case, it would be brought to court just as it works in the traditional legal system today.

A more nuanced answer to this (one that we're constantly exploring/improving upon) is there may be additional ways to deter IP infringement. For example, a staking validation mechanism where users could stake tokens on a piece of IP being valid, and if it were to be disputed and marked as copyright, the tokens get slashed and distributed to the creator who was harmed. Additionally we've thought of introducing external IP infringement detection services directly into our L1 at the lowest level that could flag or automatically mark IP as potential infringement the moment its registered.

Ultimately Story is not a system built to prevent bad actors, rather it is meant to help facilitate honest actors to more easily register their IP, remix from others, and set proper terms for their work. The protocol is permissionless and stopping bad actors entirely would be near impossible, but we can try to disincentivize them as best we can. Much like how the pirating of media plummeted when Apple Music, Spotify, and Netflix made such media more accessible by creating a "path of least resistance", we see a similar future with Story & IP.


# Protect DALL·E AI-Generated Images
In this tutorial, you will learn how to license and protect DALL·E 2 AI-Generated images by registering it on Story.

## The Explanation

Let's say you generate an image using AI. Without adding a proper license to your image, others could use it freely. In this tutorial, you will learn how to add a license to your DALL·E 2 AI-Generated image so that if others want to use it, they must properly license it from you.

In order to register that IP on Story, you first need to mint an NFT to represent that IP, and then register that NFT on Story, turning it into an [🧩 IP Asset](doc:ip-asset).

## 0. Before you Start

There are a few steps you have to complete before you can start the tutorial.

1. You will need to install [Node.js](https://nodejs.org/en/download) and [npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm). If you've coded before, you likely have these.
2. Add your Story Network Testnet wallet's private key to `.env` file:

```yaml .env
WALLET_PRIVATE_KEY=
```

3. Go to [Pinata](https://pinata.cloud/) and create a new API key. Add the JWT to your `.env` file:

```yaml .env
PINATA_JWT=
```

4. Go to [OpenAI](https://platform.openai.com/settings/organization/api-keys) and create a new API key. Add the new key to your `.env` file:

> 🚧 OpenAI Credits
>
> In order to generate an image, you'll need OpenAI credits. If you just created an account, you will probably have a free trial that will give you a few credits to start with.

```yaml .env
OPENAI_API_KEY=
```

5. Add your preferred RPC URL to your `.env` file. You can just use the public default one we provide:

```yaml .env
RPC_PROVIDER_URL=https://aeneid.storyrpc.io
```

6. Install the dependencies:

```Text Terminal
npm install @story-protocol/core-sdk pinata-web3 viem openai
```

## 1. Generate an Image

In a `main.ts` file, add the following code to generate an image:

```typescript main.ts
import OpenAI from 'openai'

async function main() {
  const openai = new OpenAI({
    apiKey: process.env.OPENAI_API_KEY,
  })

  const image = await openai.images.generate({ 
    model: 'dall-e-2', 
    prompt: 'A cute baby sea otter' 
  });

  console.log(image.data[0].url) // the url to the newly created image 
}

main();
```

## 2. Set up your Story Config

In a `utils.ts` file, add the following code to set up your Story Config:

* Associated docs: [TypeScript SDK Setup](doc:typescript-sdk-setup)

```javascript utils.ts
import { StoryClient, StoryConfig } from "@story-protocol/core-sdk";
import { http } from "viem";
import { privateKeyToAccount, Address, Account } from "viem/accounts";

const privateKey: Address = `0x${process.env.WALLET_PRIVATE_KEY}`;
export const account: Account = privateKeyToAccount(privateKey);

const config: StoryConfig = {
  account: account,
  transport: http(process.env.RPC_PROVIDER_URL),
  chainId: "aeneid",
};
export const client = StoryClient.newClient(config);
```

## 3. Set up your IP Metadata

View the [IPA Metadata Standard](doc:ipa-metadata-standard) and construct your metadata for your IP. Back in the `main.ts` file, use the `generateIpMetadata` function to properly format your metadata and ensure it is of the correct type, as shown below:

```javascript main.ts
import OpenAI from 'openai'
import { IpMetadata } from "@story-protocol/core-sdk";
import { client, account } from './utils'

async function main() {
  // previous code here ...

  const ipMetadata: IpMetadata = client.ipAsset.generateIpMetadata({
    title: 'Dall-E 2 Image',
    description: 'An image generated by Dall-E 2',
    ipType: 'image',
    attributes: [
      {
        key: 'Model',
        value: 'dall-e-2',
      },
      {
        key: 'Prompt',
        value: 'A cute baby sea otter',
      },
    ],
    creators: [
      {
        name: 'Jacob Tucker',
        contributionPercent: 100,
        address: account.address,
      },
    ],
  }) 
}

main();
```

## 4. Set up your NFT Metadata

The NFT Metadata follows the [ERC-721 Metadata Standard](https://eips.ethereum.org/EIPS/eip-721).

```javascript main.ts
import OpenAI from 'openai'
import { IpMetadata } from "@story-protocol/core-sdk";
import { client, account } from './utils'

async function main() {
  // previous code here ...

  const nftMetadata = {
    name: 'Image Ownership NFT',
    description: 'This NFT represents ownership of the image generated by Dall-E 2',
    image: image.data[0].url,
    attributes: [
      {
        key: 'Model',
        value: 'dall-e-2',
      },
      {
        key: 'Prompt',
        value: 'A cute baby sea otter',
      },
    ],
  } 
}

main();
```

## 5. Upload & Register IP

Now that we have all of our metadata set up, you can easily complete this tutorial by going to [Register an IP Asset](https://docs.story.foundation/docs/register-an-ip-asset#3-optional-upload-your-ip-and-nft-metadata-to-ipfs) and **completing steps 3 (Upload your IP and NFT Metadata to IPFS) & 4 (Register an NFT as an IP Asset)**.

Once you have done that, you should see a console log with a link to our IP-explorer that shows your registered AI generated image.

## 6. Done!

# How to Tip an IP
* [Use the SDK](https://docs.story.foundation/docs/how-to-tip-an-ip#using-the-sdk)
* [Use a Smart Contract](https://docs.story.foundation/docs/how-to-tip-an-ip#using-a-smart-contract)

# Using the SDK

<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/registerDerivativeCommercial.ts" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    See a completed, working example of setting up a simple derivative chain and then tipping the child IP Asset.
  </Card>
</Cards>

In this tutorial, you will learn how to send money ("tip") an IP Asset using the TypeScript SDK.

## The Explanation

In this scenario, let's say there is a parent IP Asset that represents Mickey Mouse. Someone else draws a hat on that Mickey Mouse and registers it as a derivative (or "child") IP Asset. The License Terms specify that the child must share 50% of all commercial revenue (`commercialRevShare = 50`) with the parent. Someone else (a 3rd party user) comes along and wants to send the derivative 2 $WIP for being really cool.

For the purposes of this example, we will assume the child is already registered as a derivative IP Asset. If you want help learning this, check out [Register a Derivative](doc:register-a-derivative).

* Parent IP ID: `0x42595dA29B541770D9F9f298a014bF912655E183`
* Child IP ID: `0xeaa4Eed346373805B377F5a4fe1daeFeFB3D182a`

## 0. Before you Start

There are a few steps you have to complete before you can start the tutorial.

1. You will need to install [Node.js](https://nodejs.org/en/download) and [npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm). If you've coded before, you likely have these.
2. Add your Story Network Testnet wallet's private key to `.env` file:

```text env
WALLET_PRIVATE_KEY=<YOUR_WALLET_PRIVATE_KEY>
```

3. Add your preferred RPC URL to your `.env` file. You can just use the public default one we provide:

```text env
RPC_PROVIDER_URL=https://aeneid.storyrpc.io
```

4. Install the dependencies:

```Text Terminal
npm install @story-protocol/core-sdk viem
```

## 1. Set up your Story Config

In a `utils.ts` file, add the following code to set up your Story Config:

* Associated docs: [TypeScript SDK Setup](doc:typescript-sdk-setup)

```typescript utils.ts
import { StoryClient, StoryConfig } from '@story-protocol/core-sdk'
import { http } from 'viem'
import { privateKeyToAccount, Address, Account } from 'viem/accounts'

const privateKey: Address = `0x${process.env.WALLET_PRIVATE_KEY}`
export const account: Account = privateKeyToAccount(privateKey)

const config: StoryConfig = {  
  account: account,  
  transport: http(process.env.RPC_PROVIDER_URL),  
  chainId: 'aeneid',  
}  
export const client = StoryClient.newClient(config)
```

## 2. Tipping the Derivative IP Asset

Now create a `main.ts` file. We will use the `payRoyaltyOnBehalf` function to pay the derivative asset.

You will be paying the IP Asset with [$WIP](https://aeneid.storyscan.xyz/address/0x1514000000000000000000000000000000000000). **Note that if you don't have enough $WIP, the function will auto wrap an equivalent amount of $IP into $WIP for you.** If you don't have enough of either, it will fail.

> 📘 Whitelisted Revenue Tokens
>
> Only tokens that are whitelisted by our protocol can be used as payment ("revenue") tokens. $WIP is one of those tokens. To see that list, go [here](https://docs.story.foundation/docs/deployed-smart-contracts).

Now we can call the `payRoyaltyOnBehalf` function. In this case:

1. `receiverIpId` is the `ipId` of the derivative (child) asset
2. `payerIpId` is `zeroAddress` because the payer is a 3rd party (someone that thinks Mickey Mouse with a hat on him is cool), and not necessarily another IP Asset
3. `token` is the address of $WIP, which can be found [here](https://docs.story.foundation/docs/ip-royalty-vault#whitelisted-revenue-tokens)
4. `amount` is 2, since the person tipping wants to send 2 $WIP

```typescript main.ts
import { client } from './utils'
import { zeroAaddress } from 'viem'
import { WIP_TOKEN_ADDRESS } from '@story-protocol/core-sdk'

async function main() {
   const response = await client.royalty.payRoyaltyOnBehalf({
    receiverIpId: '0xeaa4Eed346373805B377F5a4fe1daeFeFB3D182a',
    payerIpId: zeroAddress,
    token: WIP_TOKEN_ADDRESS,
    amount: 2,
    txOptions: { waitForTransaction: true },
  })
  console.log(`Paid royalty at transaction hash ${response.txHash}`) 
}

main();
```

## 3. Child Claiming Due Revenue

At this point we have already finished the tutorial: we learned how to tip an IP Asset. But what if the child and parent want to claim their due revenue?

The child has been paid 2 $WIP. But remember, it shares 50% of its revenue with the parent IP because of the `commercialRevenue = 50` in the license terms.

The child IP can claim its 1 $WIP by calling the `claimAllRevenue` function:

* `ancestorIpId` is the `ipId` of the IP Asset thats associated with the royalty vault that has the funds in it (more simply, this is just the child's `ipId`)
* `currencyTokens` is an array that contains the address of $WIP, which can be found [here](https://docs.story.foundation/docs/ip-royalty-vault#whitelisted-revenue-tokens)
* `claimer` is the address that holds the royalty tokens associated with the child's [IP Royalty Vault](doc:ip-royalty-vault). By default, they are in the IP Account, which is just the `ipId` of the child asset

```typescript main.ts
import { client } from './utils'
import { zeroAaddress } from 'viem'
import { WIP_TOKEN_ADDRESS } from '@story-protocol/core-sdk'

async function main() {
  // previous code here ...
  const response = await client.royalty.claimAllRevenue({
    ancestorIpId: '0xDa03c4B278AD44f5a669e9b73580F91AeDE0E3B2',
    claimer: '0xDa03c4B278AD44f5a669e9b73580F91AeDE0E3B2',
    currencyTokens: [WIP_TOKEN_ADDRESS],
    childIpIds: [],
    royaltyPolicies: []
  })

  console.log(`Claimed revenue: ${response.claimedTokens}`);
}

main();
```

## 4. Parent Claiming Due Revenue

Continuing, the parent should be able to claim its revenue as well. In this example, the parent should be able to claim 1 $WIP since the child earned 2 $WIP and the `commercialRevShare = 50` in the license terms.

We will use the `claimAllRevenue` function to claim the due revenue tokens.

1. `ancestorIpId` is the `ipId` of the parent ("ancestor") asset
2. `claimer` is the address that holds the royalty tokens associated with the parent's [IP Royalty Vault](doc:ip-royalty-vault). By default, they are in the IP Account, which is just the `ipId` of the parent asset
3. `childIpIds` will have the `ipId` of the child asset
4. `royaltyPolicies` will contain the address of the royalty policy. As explained in [💸 Royalty Module](doc:royalty-module), this is either `RoyaltyPolicyLAP` or `RoyaltyPolicyLRP`, depending on the license terms. In this case, let's assume the license terms specify a `RoyaltyPolicyLAP`. Simply go to [Deployed Smart Contracts](doc:deployed-smart-contracts) and find the correct address.
5. `currencyTokens` is an array that contains the address of $WIP, which can be found [here](https://docs.story.foundation/docs/ip-royalty-vault#whitelisted-revenue-tokens)

```typescript main.ts
import { client } from './utils'
import { zeroAaddress } from 'viem'
import { WIP_TOKEN_ADDRESS } from '@story-protocol/core-sdk'

async function main() {
  // previous code here ...

  const response = await client.royalty.claimAllRevenue({
    ancestorIpId: '0x089d75C9b7E441dA3115AF93FF9A855BDdbfe384',
    claimer: '0x089d75C9b7E441dA3115AF93FF9A855BDdbfe384',
    currencyTokens: [WIP_TOKEN_ADDRESS],
    childIpIds: ['0xDa03c4B278AD44f5a669e9b73580F91AeDE0E3B2'],
    royaltyPolicies: ['0xBe54FB168b3c982b7AaE60dB6CF75Bd8447b390E']
  })
  
  console.log(`Claimed revenue: ${response.claimedTokens}`);
}

main();
```

## 5. Done!

<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/registerDerivativeCommercial.ts" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    See a completed, working example of setting up a simple derivative chain and then tipping the child IP Asset.
  </Card>
</Cards>

# Using a Smart Contract

<Cards columns={1}>
  <Card title="Go to Smart Contract Tutorial" href="https://docs.story.foundation/docs/sc-claiming-royalty" icon="fa-home">
    View the tutorial here!
  </Card>
</Cards>

# Finetune Images on Story
<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/jacob-tucker/finetune-story-flux" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    View the completed code for this tutorial.
  </Card>
</Cards>

In this tutorial, you will use the FLUX Finetuning API to take a bunch of images of Story's mascot "Ippy" and finetune an AI model to create similar images along with a prompt. Then you will monetize and protect the output IP on Story.

> 📘 This Tutorial is in TypeScript
>
> Steps 1-3 of this tutorial are based on the [FLUX Finetuning Beta Guide](https://docs.bfl.ml/finetuning/), which contains examples for calling their API in Python, however I have rewritten them in TypeScript.

## The Explanation

Generative text-to-image models often do not fully capture a creator’s unique vision, and have insufficient knowledge about specific objects, brands or visual styles. With the FLUX Pro Finetuning API, creators can use existing images to finetune an AI to create similar images, along with a prompt.

When an image is created, we will register it as IP on Story in order to grow, monetize, and protect the IP.

## 0. Before you Start

There are a few steps you have to complete before you can start the tutorial.

1. You will need to install [Node.js](https://nodejs.org/en/download) and [npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm). If you've coded before, you likely have these.
2. Add your Story Network Testnet wallet's private key to `.env` file:

```yaml .env
WALLET_PRIVATE_KEY=
```

3. Go to [Pinata](https://pinata.cloud/) and create a new API key. Add the JWT to your `.env` file:

```yaml .env
PINATA_JWT=
```

4. Go to [BFL](https://api.us1.bfl.ai/auth/profile) and create a new API key. Add the new key to your `.env` file:

> 🚧 BFL Credits
>
> In order to generate an image, you'll need BFL credits. If you just created an account, you will need to purchase credits [here](https://api.us1.bfl.ai/auth/profile).
>
> You can also see the pricing for each of the API endpoints [here](https://docs.bfl.ml/pricing/).

```yaml .env
BFL_API_KEY=
```

5. Add your preferred Story RPC URL to your `.env` file. You can just use the public default one we provide:

```yaml .env
RPC_PROVIDER_URL=https://aeneid.storyrpc.io
```

6. Install the dependencies:

```Text Terminal
npm install @story-protocol/core-sdk axios pinata-web3 viem
```

## 1. Compile the Training Data

In order to create a finetune, we'll need the input training data!

1. Create a folder in your project called `images`. In that folder, add a bunch of images that you want your finetune to train on. *Supported formats: JPG, JPEG, PNG, and WebP. Also recommended to use more than 5 images.*
2. Add Text Descriptions (Optional): In the same folder, create text files with descriptions for your images. Text files should share the same name as their corresponding images. *Example: if your image is "sample.jpg", create "sample.txt"*
3. Compress your folder into a ZIP file. It should be named `images.zip`

## 2. Create a Finetune

In order to generate an image using a similar style as input images, we need to create a **finetune**. Think of a finetune as an AI that knows all of your input images and can then start producing new ones.

Let's make a function that calls FLUX's `/v1/finetune` API route. Create a `flux` folder, and inside that folder add a file named `requestFinetuning.ts` and add the following code:

> 📘 Official Docs
>
> In order to learn what each of the parameters in the payload are, see the official `/v1/finetune` API docs [here](https://api.us1.bfl.ai/scalar#tag/tasks/POST/v1/finetune).

```typescript flux/requestFinetuning.ts
import axios from "axios";
import fs from "fs";

interface FinetunePayload {
  finetune_comment: string;
  trigger_word: string;
  file_data: string;
  iterations: number;
  mode: string;
  learning_rate: number;
  captioning: boolean;
  priority: string;
  lora_rank: number;
  finetune_type: string;
}

export async function requestFinetuning(
  zipPath: string,
  finetuneComment: string,
  triggerWord = "TOK",
  mode = "general",
  iterations = 300,
  learningRate = 0.00001,
  captioning = true,
  priority = "quality",
  finetuneType = "full",
  loraRank = 32
) {
  if (!fs.existsSync(zipPath)) {
    throw new Error(`ZIP file not found at ${zipPath}`);
  }

  const modes = ["character", "product", "style", "general"];
  if (!modes.includes(mode)) {
    throw new Error(`Invalid mode. Must be one of: ${modes.join(", ")}`);
  }

  const fileData = fs.readFileSync(zipPath);
  const encodedZip = Buffer.from(fileData).toString("base64");

  const url = "https://api.us1.bfl.ai/v1/finetune";
  const headers = {
    "Content-Type": "application/json",
    "X-Key": process.env.BFL_API_KEY,
  };

  const payload: FinetunePayload = {
    finetune_comment: finetuneComment,
    trigger_word: triggerWord,
    file_data: encodedZip,
    iterations,
    mode,
    learning_rate: learningRate,
    captioning,
    priority,
    lora_rank: loraRank,
    finetune_type: finetuneType,
  };

  try {
    const response = await axios.post(url, payload, { headers });
    return response.data;
  } catch (error) {
    throw new Error(`Finetune request failed: ${error}`);
  }
}
```

Next, create a file named `train.ts` and call the `requestFinetuning` function we just made:

> 🚧 Warning: This is expensive!
>
> Creating a new finetune is expensive, ranging from $2-$6 at the time of me writing this tutorial. Please review the "FLUX PRO FINETUNE: TRAINING" section on the [pricing page](https://docs.bfl.ml/pricing/).

```typescript train.ts
import { requestFinetuning } from "./flux/requestFinetuning";

async function main() {
  const response = await requestFinetuning("./images.zip", "ippy-finetune");
  console.log(response);
}

main();
```

This will log something that looks like:

```json
{ 
  finetune_id: '6fc5e628-6f56-48ec-93cb-c6a6b22bf5a' 
}
```

This is your `finetune_id`, and will be used to create images in the following steps.

## 3. Wait for Finetune

Before we can generate images with our finetuned model, we have to wait for FLUX to finish training!

In our `flux` folder, create a file named `finetune-progress.ts` and add the following code:

> 📘 Official Docs
>
> In order to learn what each of the parameters in the payload are, see the official `/v1/get_result` API docs [here](https://api.us1.bfl.ai/scalar#tag/utility/GET/v1/get_result).

```typescript flux/finetuneProgress.ts
import axios from "axios";

export async function finetuneProgress(finetuneId: string) {
  const url = "https://api.us1.bfl.ai/v1/get_result";
  const headers = {
    "Content-Type": "application/json",
    "X-Key": process.env.BFL_API_KEY,
  };
  try {
    const response = await axios.get(url, {
      headers,
      params: { id: finetuneId },
    });
    return response.data;
  } catch (error) {
    throw new Error(`Finetune progress failed: ${error}`);
  }
}
```

Next, create a file named `finetune-progress.ts` and call the `finetuneProgress` function we just made:

```typescript finetune-progress.ts
import { finetuneProgress } from "./flux/finetuneProgress";

// input your finetune_id here
const FINETUNE_ID = '';

async function main() {
  const progress = await finetuneProgress(FINETUNE_ID);
  console.log(progress);
}

main();
```

This will log something that looks like:

```json
{
  id: '023a1507-369e-46e0-bd6d-1f3446d7d5f2',
  status: 'Pending',
  result: null,
  progress: null
}
```

As you can see, the status is still pending. We must wait until the training is 'Ready' before we can move on to the next step.

## 4. Run Inference

> 🚧 Warning: This costs money.
>
> Although very cheap, running an inference does cost money, ranging from $0.06-0.07 at the time of me writing this tutorial. Please review the "FLUX PRO FINETUNE: INFERENCE" section on the [pricing page](https://docs.bfl.ml/pricing/).

Now that we have trained a finetune, we will use the model to create images. "Running an inference" simply means using our new model (identified by its `finetune_id`), which is trained on our images, to create new images.

There are several different inference endpoints we can use, each with [their own pricing](https://docs.bfl.ml/pricing/) (found at the bottom of the page). For this tutorial, I'll be using the `/v1/flux-pro-1.1-ultra-finetuned` endpoint, which is documented [here](https://api.us1.bfl.ai/scalar#tag/tasks/POST/v1/flux-pro-1.1-ultra-finetuned).

In our `flux` folder, create a `finetuneInference.ts` file and add the following code:

> 📘 Official Docs
>
> In order to learn what each of the parameters in the payload are, see the official `/v1/flux-pro-1.1-ultra-finetuned` API docs [here](https://api.us1.bfl.ai/scalar#tag/tasks/POST/v1/flux-pro-1.1-ultra-finetuned).

```typescript flux/finetineInference.ts
import axios from "axios";

export async function finetuneInference(
  finetuneId: string,
  prompt: string,
  finetuneStrength = 1.2,
  endpoint = "flux-pro-1.1-ultra-finetuned",
  additionalParams: Record<string, any> = {}
) {
  const url = `https://api.us1.bfl.ai/v1/${endpoint}`;
  const headers = {
    "Content-Type": "application/json",
    "X-Key": process.env.BFL_API_KEY,
  };

  const payload = {
    finetune_id: finetuneId,
    finetune_strength: finetuneStrength,
    prompt,
    ...additionalParams,
  };

  try {
    const response = await axios.post(url, payload, { headers });
    return response.data;
  } catch (error) {
    throw new Error(`Finetune inference failed: ${error}`);
  }
}
```

Next, create a file named `inference.ts` and call the `finetuneInference` function we just made. The first parameter should be the `finetune_id` we got from running the script above, and the second parameter is a prompt to generate a new image.

```typescript inference.ts
import { finetuneInference } from './flux/finetuneInference'

// input your finetune_id here
const FINETUNE_ID = '';
// add your prompt here
const PROMPT = "A picture of Ippy being really happy."

async function main() {
  const inference = await finetuneInference(FINETUNE_ID, PROMPT);
  console.log(inference)
}

main();
```

This will log something that looks like:

```json
{
  id: '023a1507-369e-46e0-bd6d-1f3446d7d5f2',
  status: 'Pending',
  result: null,
  progress: null
}
```

As you can see, the status is still pending. We must wait until the generation is ready to view our image. To do this, we will need a function to fetch our new inference to see if its ready and view the details about it.

In our `flux` folder, create a file named `getInference.ts` and add the following code:

> 📘 Official Docs
>
> In order to learn what each of the parameters in the payload are, see the official `/v1/get_result` API docs [here](https://api.us1.bfl.ai/scalar#tag/utility/GET/v1/get_result).

```typescript flux/getInference.ts
import axios from "axios";

export async function getInference(id: string) {
  const url = "https://api.us1.bfl.ai/v1/get_result";
  const headers = {
    "Content-Type": "application/json",
    "X-Key": process.env.BFL_API_KEY,
  };

  try {
    const response = await axios.get(url, { headers, params: { id } });
    return response.data;
  } catch (error) {
    throw new Error(`Inference retrieval failed: ${error}`);
  }
}
```

Back in our `inference.ts` file, lets add a loop that continuously fetches the inference until it's ready. When it's ready, we will view the new image.

```typescript inference.ts
import { finetuneInference } from './flux/finetuneInference'
import { getInference } from './flux/getInference'

// input your finetune_id here
const FINETUNE_ID = '';
// add your prompt here
const PROMPT = "A picture of Ippy being really happy."

async function main() {
  const inference = await finetuneInference(FINETUNE_ID, PROMPT);
  
  let inferenceData = await getInference(inference.id);
  while (inferenceData.status != "Ready") {
    console.log("Waiting for inference to complete...");
    // wait 5 seconds
    await new Promise((resolve) => setTimeout(resolve, 5000));
    // fetch the inference again
    inferenceData = await getInference(inference.id);
  }
  // now the inference data is ready
  console.log(inferenceData);
}

main();
```

Once the loop completed, the final log will look like:

```json
{
  id: '023a1507-369e-46e0-bd6d-1f3446d7d5f2',
  status: 'Ready',
  result: {
    sample: 'https://delivery-us1.bfl.ai/results/746585f8d1b341f3a8735ababa563ac1/sample.jpeg?se=2025-01-16T19%3A50%3A11Z&sp=r&sv=2024-11-04&sr=b&rsct=image/jpeg&sig=pPtWnntLqc49hfNnGPgTf4BzS6MZcBgHayrYkKe%2BZIc%3D',
    prompt: 'A picture of Ippy being really happy.'
  },
  progress: null
}
```

You can paste the `sample` into your browser and see the final result! Make sure to save this image as it will disappear eventually.

## 5. Set up your Story Config

Next we will register this image on Story as an [🧩 IP Asset](doc:ip-asset) in order to monetize and license the IP. Create a `story` folder and add a `utils.ts` file. In there, add the following code to set up your Story Config:

* Associated docs: [TypeScript SDK Setup](doc:typescript-sdk-setup)

```typescript story/utils.ts
import { StoryClient, StoryConfig } from "@story-protocol/core-sdk";
import { http } from "viem";
import { privateKeyToAccount, Address, Account } from "viem/accounts";

const privateKey: Address = `0x${process.env.WALLET_PRIVATE_KEY}`;
export const account: Account = privateKeyToAccount(privateKey);

const config: StoryConfig = {
  account: account,
  transport: http(process.env.RPC_PROVIDER_URL),
  chainId: "aeneid",
};
export const client = StoryClient.newClient(config);
```

## 6. Set up your IP Metadata

In your `story` folder, create a `registerIp.ts` file.

View the [IPA Metadata Standard](doc:ipa-metadata-standard) and construct the metadata for your IP. You can use the `generateIpMetadata` function to properly format your metadata and ensure it is of the correct type, as shown below:

```typescript story/registerIp.ts
import { IpMetadata } from "@story-protocol/core-sdk";
import { client, account } from './utils'

export async function registerIp(inference) {
  const ipMetadata: IpMetadata = client.ipAsset.generateIpMetadata({
    title: 'Happy Ippy',
    description: "An image of Ippy being really happy, generated by FLUX's 1.1 [pro] ultra Finetune",
    ipType: 'image',
    attributes: [
      {
        key: 'Model',
        value: 'FLUX 1.1 [pro] ultra Finetune',
      },
      {
        key: 'Prompt',
        value: 'A picture of Ippy being really happy.',
      },
    ],
    creators: [
      {
        name: 'Jacob Tucker',
        contributionPercent: 100,
        address: account.address,
      },
    ],
  })
}
```

## 7. Set up your NFT Metadata

In the `registerIp.ts` file, configure your NFT Metadata, which follows the [ERC-721 Metadata Standard](https://eips.ethereum.org/EIPS/eip-721).

```typescript story/registerIp.ts
import { IpMetadata } from "@story-protocol/core-sdk";
import { client, account } from './utils'

export async function registerIp(inference) {
  // previous code here...

  const nftMetadata = {
    name: 'Ippy Ownership NFT',
    description: "This NFT represents ownership of the Happy Ippy image generated by FLUX's 1.1 [pro] ultra Finetune",
    image: inferenceData.result.sample,
    attributes: [
      {
        key: 'Model',
        value: 'FLUX 1.1 [pro] ultra Finetune',
      },
      {
        key: 'Prompt',
        value: 'A picture of Ippy being really happy.',
      },
    ],
  }
}
```

## 8. Upload your IP and NFT Metadata to IPFS

In a new `pinata` folder, create a `uploadToIpfs` file and create a function to upload your IP & NFT Metadata objects to IPFS:

```typescript pinata/uploadToIpfs.ts
import { PinataSDK } from "pinata-web3";

const pinata = new PinataSDK({
  pinataJwt: process.env.PINATA_JWT,
});

export async function uploadJSONToIPFS(jsonMetadata: any): Promise<string> {
  const { IpfsHash } = await pinata.upload.json(jsonMetadata);
  return IpfsHash;
}
```

You can then use that function to upload your metadata, as shown below:

```typescript story/registerIp.ts
import { IpMetadata } from "@story-protocol/core-sdk";
import { client, account } from './utils'
import { uploadJSONToIPFS } from "../pinata/uploadToIpfs";
import { createHash } from "crypto";

export async function registerIp(inference) {
  // previous code here...

  const ipIpfsHash = await uploadJSONToIPFS(ipMetadata);
  const ipHash = createHash("sha256")
    .update(JSON.stringify(ipMetadata))
    .digest("hex");
  const nftIpfsHash = await uploadJSONToIPFS(nftMetadata);
  const nftHash = createHash("sha256")
    .update(JSON.stringify(nftMetadata))
    .digest("hex");
}
```

## 9. Register the NFT as an IP Asset

In this step, we will use the [📦 SPG](doc:spg) to combine minting and registering our NFT into one transaction call.

First, in your `story` folder, create a separate `createSpgNftCollection.ts` file and add the following code:

> ❓ Why do we have to do this?
>
> In order to use the `mintAndRegisterIpAssetWithPilTerms` function below, we'll have to deploy an SPG NFT collection so that the SPG can do the minting for us.
>
> Instead of doing this, you could technically write your own contract that implements [ISPGNFT](https://github.com/storyprotocol/protocol-periphery-v1/blob/main/contracts/interfaces/ISPGNFT.sol). But an easy way to create a collection that implements `ISPGNFT` is just to call the `createCollection` function in the SPG contract using the SDK, as shown below.

```typescript story/createSpgNftCollection.ts
import { zeroAddress } from 'viem'
import { client } from './utils'

async function createSpgNftCollection() {
  // Create a new SPG NFT collection
  //
  // NOTE: Use this code to create a new SPG NFT collection. You can then use the
  // `newCollection.spgNftContract` address as the `spgNftContract` argument in
  // functions like `mintAndRegisterIpAssetWithPilTerms`, which you'll see later.
  //
  // You will mostly only have to do this once. Once you get your nft contract address,
  // you can use it in SPG functions.
  //
  const newCollection = await client.nftClient.createNFTCollection({
    name: 'Test NFT',
    symbol: 'TEST',
    isPublicMinting: true,
    mintOpen: true,
    mintFeeRecipient: zeroAddress,
    contractURI: '',
    txOptions: { waitForTransaction: true },
  })

  console.log(`New SPG NFT collection created at transaction hash ${newCollection.txHash}`)
  console.log(`NFT contract address: ${newCollection.spgNftContract}`)
}

createSpgNftCollection();
```

Run this file and look at the console output. Copy the SPG NFT contract address and add that value as `SPG_NFT_CONTRACT_ADDRESS` to your `.env` file:

```Text env
SPG_NFT_CONTRACT_ADDRESS=
```

> 📘 Note
>
> You only have to do the above step **once**. Once you have your SPG NFT contract address, you can register any amount of IPs and will **not** have to do this again.

## 10. Finish our Register Function

Back in our `registerIp.ts` file, add the following code. It will mint an NFT, register it as an [🧩 IP Asset](doc:ip-asset), set [License Terms](doc:license-terms) on the IP, and then set both NFT & IP metadata.

* Associated Docs: [ipAsset.mintAndRegisterIp](https://docs.story.foundation/docs/sdk-ipasset#mintandregisterip)

```typescript story/registerIp.ts
import { IpMetadata } from "@story-protocol/core-sdk";
import { client, account } from './utils'
import { uploadJSONToIPFS } from "../pinata/uploadToIpfs";
import { createHash } from "crypto";
import { Address } from "viem";

export async function registerIp(inference) {
  // previous code here ...

  const response = await client.ipAsset.mintAndRegisterIp({
    spgNftContract: process.env.SPG_NFT_CONTRACT_ADDRESS as Address,
    allowDuplicates: true,
    ipMetadata: {
      ipMetadataURI: `https://ipfs.io/ipfs/${ipIpfsHash}`,
      ipMetadataHash: `0x${ipHash}`,
      nftMetadataURI: `https://ipfs.io/ipfs/${nftIpfsHash}`,
      nftMetadataHash: `0x${nftHash}`,
    },
    txOptions: { waitForTransaction: true },
  });

  console.log(`Root IPA created at transaction hash ${response.txHash}, IPA ID: ${response.ipId}`);
  console.log(`View on the explorer: https://explorer.story.foundation/ipa/${response.ipId}`); 
}
```

## 11. Register our Inference

Now that we have completed our `registerIp` function, let's add it to our `inference.ts` file:

```typescript inference.ts
import { finetuneInference } from './flux/finetuneInference'
import { getInference } from './flux/getInference'
import { registerIp } from './story/registerIp'

const FINETUNE_ID = '';
const PROMPT = "A picture of Ippy being really happy."

async function main() {
  const inference = await finetuneInference(FINETUNE_ID, PROMPT);
  
  let inferenceData = await getInference(inference.id);
  while (inferenceData.status != "Ready") {
    console.log("Waiting for inference to complete...");
    await new Promise((resolve) => setTimeout(resolve, 5000));
    inferenceData = await getInference(inference.id);
  }
  // now the inference data is ready
  console.log(inferenceData);
  
  // add the function here
  await registerIp(inferenceData);
}

main();
```

## 12. Done!

<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/jacob-tucker/finetune-story-flux" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    View the completed code for this tutorial.
  </Card>
</Cards>

# Register & Monetize Stability Images
In this tutorial, you will learn how to:

1. Generate an image with Stability AI
2. Upload your image to Pinata IPFS
3. Register your image as IP on Story
4. Attach License Terms to your IP

## The Explanation

Let's say you generate an image using Stability AI. Without adding a proper license to your image, others could use it freely. In this tutorial, you will learn how to add a license to your Stability AI-Generated image so that if others want to use it, they must properly license it from you.

## 0. Before you Start

There are a few steps you have to complete before you can start the tutorial.

1. You will need to install [Node.js](https://nodejs.org/en/download) and [npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm). If you've coded before, you likely have these.
2. Add your Story Network Testnet wallet's private key to `.env` file:

```yaml .env
WALLET_PRIVATE_KEY=
```

3. Go to [the Pinata dashboard](https://app.pinata.cloud/developers/api-keys) and create a new API key and a gateway. Add the JWT along with the gateway to your `.env` file:

```yaml .env
PINATA_JWT=
PINATA_GATEWAY=
```

4. Go to [Stability](https://platform.stability.ai/account/keys) and create a new API key. Add the new key to your `.env` file:

> 🚧 Stability Credits
>
> In order to generate an image, you'll need Stability credits. If you just created an account, you will probably have a free trial that will give you a few credits to start with.

```yaml .env
STABILITY_API_KEY=
```

5. Add your preferred RPC URL to your `.env` file. You can just use the public default one we provide:

```yaml .env
RPC_PROVIDER_URL=https://aeneid.storyrpc.io
```

6. Install the dependencies:

```Text Terminal
npm install @story-protocol/core-sdk pinata-web3 viem axios sharp form-data
```

## 1. Generate an Image

You can follow the [Stability API Reference](https://platform.stability.ai/docs/api-reference) to use the model of your choice. For this tutorial, we'll be using Stability's **Stable Image Core** generate endpoint to generate an image. The below is taken directly from their documentation.

Create a `main.ts` file and add the following code:

```typescript main.ts
import fs from "fs";
import axios from "axios";
import FormData from "form-data";

async function main() {
  const payload = {
    prompt: 'Lighthouse on a cliff overlooking the ocean',
    output_format: 'png',
  }

  const response = await axios.postForm(
    `https://api.stability.ai/v2beta/stable-image/generate/core`,
    axios.toFormData(payload, new FormData()),
    {
      validateStatus: undefined,
      responseType: 'arraybuffer',
      headers: {
        Authorization: `Bearer ${process.env.STABILITY_API_KEY}`,
        Accept: 'image/*',
      },
    }
  ) 
}

main();
```

## 1.5. (Optional) Condense the Image

Stability generates images that are heavy in size, and therefore expensive to store. Optionally, we can condense the produced image for faster loading speeds and less expensive storage costs.

```typescript main.ts
import fs from "fs";
import axios from "axios";
import FormData from "form-data";

async function main() {
  // previous code here ...

  const condensedImgBuffer = await sharp(response.data)
    .png({ quality: 10 }) // Adjust the quality value as needed (between 0 and 100)
    .toBuffer() 
}

main();
```

## 2. Store Image in IPFS

Now that we have our image, we need to store it on IPFS so we can get a URL back to access it. In this tutorial we'll be using [Pinata](https://pinata.cloud/), a decentralized storage solution that makes storing images easy.

In a separate file `uploadToIpfs.ts`, create a function `uploadBlobToIPFS` that uploads our buffer to IPFS:

```typescript uploadToIpfs.ts
import { PinataSDK } from 'pinata-web3'

const pinata = new PinataSDK({
  pinataJwt: process.env.PINATA_JWT,
  // you can put your pinata gateway here, or leave it empty
  pinataGateway: process.env.PINATA_GATEWAY
})

// upload our image to ipfs by putting it in a public group
export async function uploadBlobToIPFS(blob: Blob, fileName: string): Promise<string> {
  const file = new File([blob], fileName, { type: 'image/png' })
  const { IpfsHash } = await pinata.upload.file(file)
  return IpfsHash
}
```

Back in the main file, call the `uploadBlobToIPFS` function to store our image:

```typescript main.ts
import fs from "fs";
import axios from "axios";
import FormData from "form-data";
import { uploadBlobToIPFS } from './uploadToIpfs.ts'

async function main() {
  // previous code here ...

  // convert the buffer to a blob
  const blob = new Blob([condensedImgBuffer], { type: 'image/png' });
  // store the blob on ipfs
  const imageCid = await uploadBlobToIPFS(blob, 'lighthouse.png'); 
}

main();
```

## 3. Set up your Story Config

Now that we have generated and stored our image, we can register the image as IP on Story. First, let's set up our config. In a `utils.ts` file, add the following code:

* Associated docs: [TypeScript SDK Setup](doc:typescript-sdk-setup)

```typescript utils.ts
import { StoryClient, StoryConfig } from "@story-protocol/core-sdk";
import { http } from "viem";
import { privateKeyToAccount, Address, Account } from "viem/accounts";

const privateKey: Address = `0x${process.env.WALLET_PRIVATE_KEY}`;
export const account: Account = privateKeyToAccount(privateKey);

const config: StoryConfig = {
  account: account,
  transport: http(process.env.RPC_PROVIDER_URL),
  chainId: "aeneid",
};
export const client = StoryClient.newClient(config);
```

## 4. Set up your IP Metadata

View the [IPA Metadata Standard](doc:ipa-metadata-standard) and construct the metadata for your IP. You can use the `generateIpMetadata` function to properly format your metadata and ensure it is of the correct type, as shown below:

```typescript main.ts
import fs from "fs";
import axios from "axios";
import FormData from "form-data";
import { uploadBlobToIPFS } from './uploadToIpfs.ts'
import { IpMetadata } from "@story-protocol/core-sdk";
import { client, account } from './utils'

async function main() {
  // previous code here ...

  const ipMetadata: IpMetadata = client.ipAsset.generateIpMetadata({
    title: 'Lighthousd',
    description: 'A lighthouse image generated by Stability Stable Image Core',
    ipType: 'image',
    attributes: [
      {
        key: 'Model',
        value: 'Stability',
      },
      {
        key: 'Service',
        value: 'Stable Image Core'
      },
      {
        key: 'Prompt',
        value: 'Lighthouse on a cliff overlooking the ocean',
      },
    ],
    creators: [
      {
        name: 'Jacob Tucker',
        contributionPercent: 100,
        address: account.address,
      },
    ],
  }) 
}

main();
```

## 5. Set up your NFT Metadata

The NFT Metadata follows the [ERC-721 Metadata Standard](https://eips.ethereum.org/EIPS/eip-721).

```javascript main.ts
import fs from "fs";
import axios from "axios";
import FormData from "form-data";
import { uploadBlobToIPFS } from './uploadToIpfs.ts'
import { IpMetadata } from "@story-protocol/core-sdk";
import { client, account } from './utils'

async function main() {
  // previous code here ...

  const nftMetadata = {
    name: 'Ownership NFT',
    description: 'This NFT represents ownership of the image generated by Stability',
    image: process.env.PINATA_GATEWAY + '/files/' + imageCid,
    attributes: [
      {
        key: 'Model',
        value: 'Stability',
      },
      {
        key: 'Service',
        value: 'Stable Image Core'
      },
      {
        key: 'Prompt',
        value: 'Lighthouse on a cliff overlooking the ocean',
      },
    ]
  } 
}

main();
```

## 6. Upload your IP and NFT Metadata to IPFS

In the `uploadToIpfs.ts` file, create a function to upload your IP & NFT Metadata objects to IPFS:

```typescript uploadToIpfs.ts
// previous code here ...

export async function uploadJSONToIPFS(jsonMetadata: any): Promise<string> {
  const { IpfsHash } = await pinata.upload.json(jsonMetadata)
  return IpfsHash
}
```

You can then use that function to upload your metadata, as shown below:

```typescript main.ts
import fs from "fs";
import axios from "axios";
import FormData from "form-data";
import { uploadBlobToIPFS, uploadJSONToIPFS } from './uploadToIpfs.ts'
import { IpMetadata } from "@story-protocol/core-sdk";
import { client, account } from './utils'
import { createHash } from "crypto";

async function main() {
  // previous code here ...

  const ipIpfsHash = await uploadJSONToIPFS(ipMetadata);
  const ipHash = createHash("sha256")
    .update(JSON.stringify(ipMetadata))
    .digest("hex");
  const nftIpfsHash = await uploadJSONToIPFS(nftMetadata);
  const nftHash = createHash("sha256")
    .update(JSON.stringify(nftMetadata))
    .digest("hex");
}

main();
```

## 7. Create License Terms

When registering your image on Story, you can attach [License Terms](doc:license-terms) to the IP. These are real, legally binding terms enforced on-chain by the [📜 Licensing Module](doc:licensing-module), disputable by the [❌ Dispute Module](doc:dispute-module), and in the worst case, able to be enforced off-chain in court through traditional means.

Let's say we want to monetize our image such that every time someone wants to use it (on merch, advertisement, or whatever) they have to pay an initial minting fee of 10 $WIP. Additionally, every time they earn revenue on derivative work, they owe 5% revenue back as royalty.

```typescript main.ts
import fs from "fs";
import axios from "axios";
import FormData from "form-data";
import { uploadBlobToIPFS, uploadJSONToIPFS } from './uploadToIpfs.ts'
import { IpMetadata } from "@story-protocol/core-sdk";
import { client, account } from './utils'
import { createHash } from "crypto";
import { LicenseTerms, WIP_TOKEN_ADDRESS } from '@story-protocol/core-sdk';
import { zeroAddress } from 'viem';

async function main() {
  // previous code here ...

  const commercialRemixTerms: LicenseTerms = {
    transferable: true,
    royaltyPolicy: RoyaltyPolicyLAP, // insert RoyaltyPolicyLAP address from https://docs.story.foundation/docs/deployed-smart-contracts
    defaultMintingFee: BigInt(10),
    expiration: BigInt(0),
    commercialUse: true,
    commercialAttribution: true, // must give us attribution
    commercializerChecker: zeroAddress,
    commercializerCheckerData: zeroAddress,
    commercialRevShare: 5, // can claim 50% of derivative revenue
    commercialRevCeiling: BigInt(0),
    derivativesAllowed: true,
    derivativesAttribution: true,
    derivativesApproval: false,
    derivativesReciprocal: true,
    derivativeRevCeiling: BigInt(0),
    currency: WIP_TOKEN_ADDRESS,
    uri: '',
  } 
}

main();
```

## 8. Register an NFT as an IP Asset

In this step, we will use the [📦 SPG](doc:spg) to combine minting and registering our NFT into one transaction call.

First, in a separate file `createSpgNftCollection.ts`, you must create a new SPG NFT collection. You can do this with the SDK (view a working example [here](https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/utils/createSpgNftCollection.ts)):

> ❓ Why do we have to do this?
>
> In order to use the `mintAndRegisterIpAssetWithPilTerms` function below, we'll have to deploy an SPG NFT collection so that the SPG can do the minting for us.
>
> Instead of doing this, you could technically write your own contract that implements [ISPGNFT](https://github.com/storyprotocol/protocol-periphery-v1/blob/main/contracts/interfaces/ISPGNFT.sol). But an easy way to create a collection that implements `ISPGNFT` is just to call the `createCollection` function in the SPG contract using the SDK, as shown below.

```typescript createSpgNftCollection.ts
import { zeroAddress } from 'viem'
import { client } from './utils'

async function createSpgNftCollection() {
  // Create a new SPG NFT collection
  //
  // NOTE: Use this code to create a new SPG NFT collection. You can then use the
  // `newCollection.spgNftContract` address as the `spgNftContract` argument in
  // functions like `mintAndRegisterIpAssetWithPilTerms`, which you'll see later.
  //
  // You will mostly only have to do this once. Once you get your nft contract address,
  // you can use it in SPG functions.
  //
  const newCollection = await client.nftClient.createNFTCollection({
    name: 'Test NFT',
    symbol: 'TEST',
    isPublicMinting: true,
    mintOpen: true,
    mintFeeRecipient: zeroAddress,
    contractURI: '',
    txOptions: { waitForTransaction: true },
  })

  console.log(`New SPG NFT collection created at transaction hash ${newCollection.txHash}`)
  console.log(`NFT contract address: ${newCollection.spgNftContract}`)
}

createSpgNftCollection();
```

Run this file and look at the console output. Copy the SPG NFT contract address and add that value as `SPG_NFT_CONTRACT_ADDRESS` to your `.env` file:

```Text env
SPG_NFT_CONTRACT_ADDRESS=
```

> 📘 Note
>
> You only have to do the above step **once**. Once you have your SPG NFT contract address, you can register any amount of IPs and will **not** have to do this again.

The code below will mint an NFT, register it as an [🧩 IP Asset](doc:ip-asset), set [License Terms](doc:license-terms) on the IP, and then set both NFT & IP metadata.

* Associated Docs: [ipAsset.mintAndRegisterIp](https://docs.story.foundation/docs/sdk-ipasset#mintandregisterip)

```typescript main.ts
import fs from "fs";
import axios from "axios";
import FormData from "form-data";
import { uploadBlobToIPFS, uploadJSONToIPFS } from './uploadToIpfs.ts'
import { IpMetadata, WIP_TOKEN_ADDRESS } from "@story-protocol/core-sdk";
import { client, account } from './utils'
import { createHash } from "crypto";
import { LicenseTerms, LicensingConfig } from '@story-protocol/core-sdk';
import { zeroAddress, Address } from 'viem';

async function main() {
  // previous code here ...
  
  // default license config
  const licensingConfig: LicensingConfig = {
    isSet: false,
    mintingFee: BigInt(0),
    licensingHook: zeroAddress,
    hookData: zeroHash,
    commercialRevShare: 0,
    disabled: false,
    expectMinimumGroupRewardShare: 0,
    expectGroupRewardPool: zeroAddress,
  };

  const response = await client.ipAsset.mintAndRegisterIpAssetWithPilTerms({
    spgNftContract: process.env.SPG_NFT_CONTRACT_ADDRESS as Address,
    allowDuplicates: true,
    // the terms we created in the previous step
    licenseTermsData: [{ terms: commercialRemixTerms, licensingConfig }],
    ipMetadata: {
      ipMetadataURI: process.env.PINATA_GATEWAY + '/files/' + ipIpfsHash,
      ipMetadataHash: `0x${ipHash}`,
      nftMetadataURI: process.env.PINATA_GATEWAY + '/files/' + nftIpfsHash,
      nftMetadataHash: `0x${nftHash}`,
    },
    txOptions: { waitForTransaction: true },
  });

  console.log(`Root IPA created at transaction hash ${response.txHash}, IPA ID: ${response.ipId}`);
  console.log(`View on the explorer: https://explorer.story.foundation/ipa/${response.ipId}`); 
}

main();
```

## 9. Done!

Congratulations! Now your image is registered on Story with commercial license terms.

# How to Dispute an IP on Story
* [Use the SDK](https://docs.story.foundation/docs/how-to-dispute-ip-on-story#using-the-sdk)
* [Use a Smart Contract](https://docs.story.foundation/docs/how-to-dispute-ip-on-story#using-a-smart-contract)

# Using the SDK

<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/disputeIp.ts" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    View the completed code for this tutorial.
  </Card>
</Cards>

In this tutorial, you will learn how to dispute an IP on Story using the TypeScript SDK.

## The Explanation

There are many instances where you may want to dispute an IP - whether that IP is or is not owned by you. Disputing IP on Story is easy thanks to our [❌ Dispute Module](doc:dispute-module) and the [UMA Arbitration Policy](doc:uma-arbitration-policy).

Let's say you register a drawing, and then someone else registers that drawing with 1 pixel off. You can dispute it along a `IMPROPER_REGISTRATION` tag, which communicates potential plagiarism.

In this tutorial, you will simply learn how to flag an IP as being disputed.

## 0. Before you Start

There are a few steps you have to complete before you can start the tutorial.

1. You will need to install [Node.js](https://nodejs.org/en/download) and [npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm). If you've coded before, you likely have these.
2. Add your Story Network Testnet wallet's private key to `.env` file:

```yaml .env
WALLET_PRIVATE_KEY=<YOUR_WALLET_PRIVATE_KEY>
```

3. Add your preferred RPC URL to your `.env` file. You can just use the public default one we provide:

```yaml .env
RPC_PROVIDER_URL=https://aeneid.storyrpc.io
```

4. Install the dependencies:

```Text Terminal
npm install @story-protocol/core-sdk viem
```

## 1. Set up your Story Config

In a `utils.ts` file, add the following code to set up your Story Config:

* Associated docs: [TypeScript SDK Setup](doc:typescript-sdk-setup)

```typescript utils.ts
import { StoryClient, StoryConfig } from '@story-protocol/core-sdk'
import { http } from 'viem'
import { privateKeyToAccount, Address, Account } from 'viem/accounts'

const privateKey: Address = `0x${process.env.WALLET_PRIVATE_KEY}`
export const account: Account = privateKeyToAccount(privateKey)

const config: StoryConfig = {  
  account: account,  
  transport: http(process.env.RPC_PROVIDER_URL),  
  chainId: 'aeneid',  
}
export const client = StoryClient.newClient(config)
```

## 2. Dispute an IP

To dispute an IP Asset, you will need:

* the `targetIpId` of the IP Asset you are disputing (we use a test one below)
* the `targetTag` that you are applying to the dispute. Only [whitelisted tags](https://docs.story.foundation/docs/dispute-module#dispute-tags) can be applied.
* a `cid` (Content Identifier) is a unique identifier in IPFS that represents the dispute evidence you must provide, as described [here](https://docs.story.foundation/docs/uma-arbitration-policy#dispute-evidence-submission-guidelines) (we use a test one below).
  * :warning: **Note you can only provide a CID one time.** After it is used, it can't be used as evidence again.

Create a `main.ts` file and add the code below:

```typescript main.ts
import { client } from './utils'

async function main() {
  const disputeResponse = await client.dispute.raiseDispute({
    targetIpId: '0x6b42d065aDCDA6fA83B59ad731841360dC5321fB',
    // NOTE: you must use your own CID here, because every time it is used,
    // the protocol does not allow you to use it again
    cid: 'QmbWqxBEKC3P8tqsKc98xmWNzrzDtRLMiMPL8wBuTGsMnR',
    // you must pick from one of the whitelisted tags here: https://docs.story.foundation/docs/dispute-module#dispute-tags
    targetTag: 'IMPROPER_REGISTRATION',
    bond: 0,
    liveness: 2592000,
    txOptions: { waitForTransaction: true },
  })
  console.log(`Dispute raised at transaction hash ${disputeResponse.txHash}, Dispute ID: ${disputeResponse.disputeId}`) 
}

main();
```

## 3. Done!

<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/disputeIp.ts" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    See a completed, working example disputing an IP.
  </Card>
</Cards>

# Using a Smart Contract

> 🚧 Coming soon...

# How to Register IP on Story
<Cards columns={2}>
  <Card title="Using the SDK" href="https://docs.story.foundation/docs/register-an-ip-asset" icon="fa-home" target="_blank">
    Learn how to register an IP using the SDK.
  </Card>

  <Card title="Using a Smart Contract" href="https://docs.story.foundation/docs/sc-register-an-ip-asset" icon="fa-home" target="_blank">
    Learn how to register an IP using the Smart Contracts.
  </Card>
</Cards>

# 📘 Tutorials
## 📋 Registration

* [How to Register IP on Story](doc:how-to-register-ip-on-story)
* [How to Register Music on Story](doc:how-to-register-music-on-story)

## :money_with_wings: Royalty

* [How to Tip an IP](doc:how-to-tip-an-ip)

## :x: Dispute

* [How to Dispute an IP on Story](doc:how-to-dispute-ip-on-story)

## :robot: AI-Themed

* [Protect DALL·E AI-Generated Images](doc:protect-dalle-ai-generated-images)
* [Register & Monetize Stability Images](doc:register-stability-images)
* [Finetune Images on Story](doc:finetune-images)

# Training Data

In this tutorial, you will learn how to license and protect DALL·E 2 AI-Generated images by registering it on Story.

## The Explanation

AI runs on IP. The sheer speed and superabundance of AI-generated media is outpacing the current intellectual property system that was designed for physical replication. In many cases, AI is trained on and is producing copyrighted data.

This creates a "tragedy of the commons" situation, where creators are no longer incentivized to make new content if AI is just going to steal and summarize it. As a result, creators make no money since consumers go straight to AI bots, and AI bots lose in the long run if there's no new content.

To solve this, IP that is used as training data by LLMs should be registered on Story. Submit a project showcasing registering IP training data on Story, and an AI training on this registered data.

## 0. Before you Start

There are a few steps you have to complete before you can start the tutorial.

1. Add your Story Network Testnet wallet's private key to `.env` file:

```yaml .env
WALLET_PRIVATE_KEY=
```

2. Go to [Pinata](https://pinata.cloud/) and create a new API key. Add the JWT to your `.env` file:

```yaml .env
PINATA_JWT=
```

3. Go to [OpenAI](https://platform.openai.com/settings/organization/api-keys) and create a new API key. Add the new key to your `.env` file:

> 🚧 OpenAI Credits
>
> In order to generate an image, you'll need OpenAI credits. If you just created an account, you will probably have a free trial that will give you a few credits to start with.

```yaml .env
OPENAI_API_KEY=
```

4. Add your preferred RPC URL to your `.env` file. You can just use the public default one we provide:

```yaml .env
RPC_PROVIDER_URL=https://aeneid.storyrpc.io
```

5. Install the dependencies:

```Text Terminal
npm install @story-protocol/core-sdk pinata-web3 viem
```

## 1. Generate an Image

```typescript main.ts
import OpenAI from "openai";

const openai = new OpenAI({
  apiKey: process.env.OPENAI_API_KEY,
});

const image = await openai.images.generate({
  model: "dall-e-2",
  prompt: "A cute baby sea otter",
});

console.log(image.data[0].url); // the url to the newly created image
```

## 2. Set up your Story Config

- Associated docs: [TypeScript SDK Setup](doc:typescript-sdk-setup)

```javascript main.ts
import { StoryClient, StoryConfig } from "@story-protocol/core-sdk";
import { http } from "viem";
import { privateKeyToAccount, Address, Account } from "viem/accounts";

// previous code here ...

const privateKey: Address = `0x${process.env.WALLET_PRIVATE_KEY}`;
const account: Account = privateKeyToAccount(privateKey);

const config: StoryConfig = {
  account: account,
  transport: http(process.env.RPC_PROVIDER_URL),
  chainId: "aeneid",
};
const client = StoryClient.newClient(config);
```

## 3. Set up your IP Metadata

View the [IPA Metadata Standard](doc:ipa-metadata-standard) and construct your metadata for your IP. You can use the `generateIpMetadata` function to properly format your metadata and ensure it is of the correct type, as shown below:

```javascript main.ts
import { IpMetadata } from "@story-protocol/core-sdk";

// previous code here...

const ipMetadata: IpMetadata = client.ipAsset.generateIpMetadata({
  title: "Dall-E 2 Image",
  description: "An image generated by Dall-E 2",
  ipType: "image",
  attributes: [
    {
      key: "Model",
      value: "dall-e-2",
    },
    {
      key: "Prompt",
      value: "A cute baby sea otter",
    },
  ],
  creators: [
    {
      name: "Jacob Tucker",
      contributionPercent: 100,
      address: account.address,
    },
  ],
});
```

## 4. Set up your NFT Metadata

The NFT Metadata follows the [ERC-721 Metadata Standard](https://eips.ethereum.org/EIPS/eip-721).

```javascript main.ts
// previous code here...

const nftMetadata = {
  name: "NFT representing ownership of our image",
  description:
    "This NFT represents ownership of the image generated by Dall-E 2",
  image: image.data[0].url,
  attributes: [
    {
      key: "Model",
      value: "dall-e-2",
    },
    {
      key: "Prompt",
      value: "A cute baby sea otter",
    },
  ],
};
```

## 5. Upload your IP and NFT Metadata to IPFS

In a separate file, create a function to upload your IP & NFT Metadata objects to IPFS:

```javascript utils/uploadToIpfs.ts
const pinataSDK = require("@pinata/sdk");

export async function uploadJSONToIPFS(jsonMetadata): Promise<string> {
  const pinata = new pinataSDK({ pinataJWTKey: process.env.PINATA_JWT });
  const { IpfsHash } = await pinata.pinJSONToIPFS(jsonMetadata);
  return IpfsHash;
}
```

You can then use that function to upload your metadata, as shown below:

```javascript main.ts
import { uploadJSONToIPFS } from "./utils/uploadToIpfs";
import { createHash } from "crypto";

// previous code here...

const ipIpfsHash = await uploadJSONToIPFS(ipMetadata);
const ipHash = createHash("sha256")
  .update(JSON.stringify(ipMetadata))
  .digest("hex");
const nftIpfsHash = await uploadJSONToIPFS(nftMetadata);
const nftHash = createHash("sha256")
  .update(JSON.stringify(nftMetadata))
  .digest("hex");
```

## 6. Register the NFT as an IP Asset

In this step, we will use the [📦 SPG](doc:spg) to combine minting and registering our NFT into one transaction call.

First, in a separate script, you must create a new SPG NFT collection. You can do this with the SDK (view a working example [here](https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/utils/createSpgNftCollection.ts)):

> ❓ Why do we have to do this?
>
> In order to use the `mintAndRegisterIpAssetWithPilTerms` function below, we'll have to deploy an SPG NFT collection so that the SPG can do the minting for us.
>
> Instead of doing this, you could technically write your own contract that implements [ISPGNFT](https://github.com/storyprotocol/protocol-periphery-v1/blob/main/contracts/interfaces/ISPGNFT.sol). But an easy way to create a collection that implements `ISPGNFT` is just to call the `createCollection` function in the SPG contract using the SDK, as shown below.

```typescript utils/createSpgNftCollection.ts
import { StoryClient, StoryConfig } from '@story-protocol/core-sdk'
import { http } from 'viem

const privateKey: Address = `0x${process.env.WALLET_PRIVATE_KEY}`
const account: Account = privateKeyToAccount(privateKey)

const config: StoryConfig = {
  account: account,
  transport: http(process.env.RPC_PROVIDER_URL),
  chainId: 'aeneid',
}
const client = StoryClient.newClient(config)

const newCollection = await client.nftClient.createNFTCollection({
  name: 'Dall-E NFTs',
  symbol: 'DALLE',
  isPublicMinting: true,
  mintOpen: true,
  mintFeeRecipient: zeroAddress,
  contractURI: '',
  txOptions: { waitForTransaction: true },
})

console.log(
  `New SPG NFT collection created at transaction hash ${newCollection.txHash}`,
  `SPG NFT contract address: ${newCollection.spgNftContract}`
)
```

Run this file and look at the console output. Copy the SPG NFT contract address and add that value as `SPG_NFT_CONTRACT_ADDRESS` to your `.env` file:

```Text env
SPG_NFT_CONTRACT_ADDRESS=
```

> 📘 Note
>
> You only have to do the above step **once**. Once you have your SPG NFT contract address, you can register any amount of IPs and will **not** have to do this again.

The code below will mint an NFT, register it as an [🧩 IP Asset](doc:ip-asset), set [License Terms](doc:license-terms) on the IP, and then set both NFT & IP metadata.

- Associated Docs: [Mint, Register, and Attach Terms](https://docs.story.foundation/docs/attach-terms-to-an-ip-asset#mint-nft-register-as-ip-asset-and-attach-terms)

```typescript main.ts
import {
  PIL_TYPE,
  CreateIpAssetWithPilTermsResponse,
} from "@story-protocol/core-sdk";
import { Address } from "viem";

// previous code here ...

const response: CreateIpAssetWithPilTermsResponse =
  await client.ipAsset.mintAndRegisterIpAssetWithPilTerms({
    spgNftContract: process.env.SPG_NFT_CONTRACT_ADDRESS as Address,
    pilType: PIL_TYPE.NON_COMMERCIAL_REMIX,
    ipMetadata: {
      ipMetadataURI: `https://ipfs.io/ipfs/${ipIpfsHash}`,
      ipMetadataHash: `0x${ipHash}`,
      nftMetadataURI: `https://ipfs.io/ipfs/${nftIpfsHash}`,
      nftMetadataHash: `0x${nftHash}`,
    },
    txOptions: { waitForTransaction: true },
  });

console.log(
  `Root IPA created at transaction hash ${response.txHash}, IPA ID: ${response.ipId}`
);
console.log(
  `View on the explorer: https://explorer.story.foundation/ipa/${response.ipId}`
);
```

## 7. Done!


# How to Register Music on Story
In this tutorial, you will learn how to properly register music as IP on Story using the TypeScript SDK. At the end, you will be able to listen to your song directly on our explorer.

<Cards columns={2}>
  <Card title="Example Final Result" href="https://explorer.story.foundation/ipa/0x3E5b9e540a531da38760CC32E2f52b174EC5Fce8" icon="fa-home" target="_blank">
    View an example result after following this tutorial.
  </Card>

  <Card title="Justin Bieber is coming to Story!" href="https://x.com/StoryProtocol/status/1881713146274156951" icon="fa-megaphone" target="_blank">
    "Peaches" by Justin Bieber is one of the first RWAs coming to Story. Check out the announcement!
  </Card>
</Cards>

## 0. Create a Song

Before we register music on Story, you'll obviously need some music! If you already have music, make sure you have a link to the music file directly. For example, `https://cdn1.suno.ai/dcd3076f-3aa5-400b-ba5d-87d30f27c311.mp3`. If you don't already have this, you can upload your music file to IPFS:

If you want to create a test song, go to [Suno](https://suno.com), which is an awesome platform for AI-generated music. We can get a test song by:

1. Inputting a prompt to create a song
2. Click on the final result, which should take you to a URL like `https://suno.com/song/dcd3076f-3aa5-400b-ba5d-87d30f27c311`
3. Copy the the `SONG_ID` in the URL (`dcd3076f-3aa5-400b-ba5d-87d30f27c311`)
4. Copy the following URL: `https://cdn1.suno.ai/${SONG_ID}.mp3`, making sure to replace `SONG_ID` with your own.

This is the URL we'll use in step 2.

## 1. Complete the "How to Register IP" Tutorial

Most of what we need to do is already covered in [Register an IP Asset](doc:register-an-ip-asset). Complete that tutorial first, and then come back here.

## 2. Change Metadata

The only difference is how you set your metadata. In your `ipMetadata`, you can set a few extra related parameters (`ipType`, `media`, `attributes`, `creators`) like so:

* Make sure to replace the `media.url` with the one we created in step 0!

```typescript main.ts
const ipMetadata: IpMetadata = client.ipAsset.generateIpMetadata({
  title: 'Midnight Marriage',
  description: 'This is a house-style song generated on suno.',
  ipType: 'Music',
  media: [
    {
      name: 'Midnight Marriage',
      url: 'https://cdn1.suno.ai/dcd3076f-3aa5-400b-ba5d-87d30f27c311.mp3',
      mimeType: 'audio/mpeg',
    },
  ],
  attributes: [
    {
      key: 'Suno Artist',
      value: 'amazedneurofunk956',
    },
    {
      key: 'Artist ID',
      value: '4123743b-8ba6-4028-a965-75b79a3ad424',
    },
    {
      key: 'Source',
      value: 'Suno.com',
    },
  ],
  creators: [
    {
      name: 'Jacob Tucker',
      address: account.address,
      contributionPercent: 100,
    },
  ],
})
```

In your `nftMetadata`, **in order for the music to actually be played on our explorer** you must set a `media` parameter, and then you can also set some `attributes` that look something like this:

* Again, make sure to replace the `media.url` with the one we created in step 0!
* For the `image`, you can use any URL to an image. [picsum](https://picsum.photos)  is a website that generates random images, or you can use the image from your Suno song.

```typescript main.ts
const nftMetadata = {
  name: 'Midnight Marriage',
  description: 'This is an NFT representing ownership of the Midnight Marriage song.',
  image: 'https://cdn2.suno.ai/image_large_8bcba6bc-3f60-4921-b148-f32a59086a4c.jpeg',
  media: [
    {
      name: 'Midnight Marriage',
      url: 'https://cdn1.suno.ai/dcd3076f-3aa5-400b-ba5d-87d30f27c311.mp3',
      mimeType: 'audio/mpeg',
    },
  ],
  attributes: [
    {
      key: 'Suno Artist',
      value: 'amazedneurofunk956',
    },
    {
      key: 'Artist ID',
      value: '4123743b-8ba6-4028-a965-75b79a3ad424',
    },
    {
      key: 'Source',
      value: 'Suno.com',
    },
  ],
}
```

## 3. Done!

When you run the script, you will register an IP Asset and it will look something like [this](https://explorer.story.foundation/ipa/0x3E5b9e540a531da38760CC32E2f52b174EC5Fce8) on our explorer.

You can see the explorer recognizes the metadata format, and you can play the song directly on the page!

# Register License Terms
<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/story-protocol-boilerplate/blob/main/test/1_LicenseTerms.t.sol" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    Follow the completed code all the way through.
  </Card>
</Cards>

[License Terms](doc:license-terms) are a configurable set of values that define restrictions on licenses minted from your IP that have those terms. For example, "If you mint this license, you must share 50% of your revenue with me." You can view the full set of terms in [PIL Terms](doc:pil-terms).

### :warning: Prerequisites

There are a few steps you have to complete before you can start the tutorial.

1. Complete the [Setup Your Own Project](doc:sc-setup)

## 0. Before We Start

It's important to know that if **License Terms already exist for the identical set of parameters you intend to create, it is unnecessary to create it again**. License Terms are protocol-wide, so you can use existing License Terms by its `licenseTermsId`.

## 1. Register License Terms

You can view the full set of terms in [PIL Terms](doc:pil-terms).

Let's create a test file under `test/1_LicenseTerms.t.sol` to see it work and verify the results:

> 📘 Contract Addresses
>
> We have filled in the addresses from the Story contracts for you. However you can also find the addresses for them here: [Deployed Smart Contracts](doc:deployed-smart-contracts)

```sol test/1_LicenseTerms.t.sol
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.26;

import { Test } from "forge-std/Test.sol";
import { IPILicenseTemplate } from "@storyprotocol/core/interfaces/modules/licensing/IPILicenseTemplate.sol";
import { PILTerms } from "@storyprotocol/core/interfaces/modules/licensing/IPILicenseTemplate.sol";

// Run this test:
// forge test --fork-url https://aeneid.storyrpc.io/ --match-path test/1_LicenseTerms.t.sol
contract LicenseTermsTest is Test {
    address internal alice = address(0xa11ce);

    // For addresses, see https://docs.story.foundation/docs/deployed-smart-contracts
    // Protocol Core - PILicenseTemplate
    IPILicenseTemplate internal PIL_TEMPLATE = IPILicenseTemplate(0x2E896b0b2Fdb7457499B56AAaA4AE55BCB4Cd316);
    // Protocol Core - RoyaltyPolicyLAP
    address internal ROYALTY_POLICY_LAP = 0xBe54FB168b3c982b7AaE60dB6CF75Bd8447b390E;
    // Revenue Token - MERC20
    address internal MERC20 = 0xF2104833d386a2734a4eB3B8ad6FC6812F29E38E;

    function setUp() public {}

    /// @notice Registers new PIL Terms. Anyone can register PIL Terms.
    function test_registerPILTerms() public {
        PILTerms memory pilTerms = PILTerms({
            transferable: true,
            royaltyPolicy: ROYALTY_POLICY_LAP,
            defaultMintingFee: 0,
            expiration: 0,
            commercialUse: true,
            commercialAttribution: true,
            commercializerChecker: address(0),
            commercializerCheckerData: "",
            commercialRevShare: 0,
            commercialRevCeiling: 0,
            derivativesAllowed: true,
            derivativesAttribution: true,
            derivativesApproval: true,
            derivativesReciprocal: true,
            derivativeRevCeiling: 0,
            currency: MERC20,
            uri: ""
        });
        uint256 licenseTermsId = PIL_TEMPLATE.registerLicenseTerms(pilTerms);

        uint256 selectedLicenseTermsId = PIL_TEMPLATE.getLicenseTermsId(pilTerms);
        assertEq(licenseTermsId, selectedLicenseTermsId);
    }
}
```

### 1a. :icecream: PIL Flavors

As you see above, you have to choose between a lot of terms.

We have convenience functions to help you register new terms. We have created [PIL Flavors](doc:pil-flavors), which are pre-configured popular combinations of License Terms to help you decide what terms to use. You can view those PIL Flavors and then register terms using the following convenience functions:

<Cards columns={4}>
  <Card title="Non-Commercial Social Remixing" href="https://docs.story.foundation/docs/pil-flavors#flavor-1-non-commercial-social-remixing" icon="fa-file" target="_blank">
    Free remixing with attribution. No commercialization.
  </Card>

  <Card title="Commercial Use" href="https://docs.story.foundation/docs/pil-flavors#flavor-2-commercial-use" icon="fa-file" target="_blank">
    Pay to use the license with attribution, but don't have to share revenue.
  </Card>

  <Card title="Commercial Remix" href="https://docs.story.foundation/docs/pil-flavors#flavor-3-commercial-remix" icon="fa-file" target="_blank">
    Pay to use the license with attribution and pay % of revenue earned.
  </Card>

  <Card title="Creative Commons Attribution" href="https://docs.story.foundation/docs/pil-flavors#flavor-4-creative-commons-attribution" icon="fa-file" target="_blank">
    Free remixing and commercial use with attribution.
  </Card>
</Cards>

For example:

```sol
import { PILFlavors } from "@storyprotocol/core/lib/PILFlavors.sol";

PILTerms memory pilTerms = PILFlavors.commercialRemix({
  mintingFee: 0,
  commercialRevShare: 10 * 10 ** 6, // 10%
  royaltyPolicy: ROYALTY_POLICY_LAP,
  currencyToken: MERC20
});
```

## 2. Test Your Code!

Run `forge build`. If everything is successful, the command should successfully compile.

Now run the test by executing the following command:

```shell
forge test --fork-url https://aeneid.storyrpc.io/ --match-path test/1_LicenseTerms.t.sol
```

## 3. Attach Terms to Your IP

Congratulations, you created new license terms!

<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/story-protocol-boilerplate/blob/main/test/1_LicenseTerms.t.sol" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    Follow the completed code all the way through.
  </Card>
</Cards>

Now that you have registered new license terms, we can attach them to an IP Asset. This will allow others to mint a license and use your IP, restricted by the terms.

We will go over this on the next page.

# Pay & Claim Revenue
<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/story-protocol-boilerplate/blob/main/test/5_Royalty.t.sol" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    Follow the completed code all the way through.
  </Card>
</Cards>

This section demonstrates how to pay an IP Asset. There are a few reasons you would do this:

1. you simply want to "tip" an IP
2. you have to because your license terms with an ancestor IP require you to forward a certain % of payment

In either scenario, you would use the below `payRoyaltyOnBehalf` function. When this happens, the [💸 Royalty Module](doc:royalty-module) automatically handles the different payment flows such that parent IP Assets who have negotiated a certain `commercialRevShare` with the IPA being paid can claim their due share.

### :warning: Prerequisites

There are a few steps you have to complete before you can start the tutorial.

1. Complete the [Setup Your Own Project](doc:sc-setup)
2. Have a basic understanding of the [💸 Royalty Module](doc:royalty-module)
3. A child IPA and a parent IPA, for which their license terms have a commercial revenue share to make this example work

## 0. Before We Start

You can pay an IP Asset using the `payRoyaltyOnBehalf` function from the [💸 Royalty Module](doc:royalty-module).

You will be paying the IP Asset with [MockERC20](https://aeneid.storyscan.xyz/address/0xF2104833d386a2734a4eB3B8ad6FC6812F29E38E). Usually you would pay with $WIP, but because we need to mint some tokens to test, we will use MockERC20.

To help with the following scenarios, let's say we have a parent IP Asset that has negotiated a 50% `commercialRevShare` with its child IP Asset.

### 0a. :coin: Whitelisted Revenue Tokens

Only tokens that are whitelisted by our protocol can be used as payment ("revenue") tokens. MockERC20 is one of those tokens. To see that list, go [here](https://docs.story.foundation/docs/deployed-smart-contracts#whitelisted-revenue-tokens).

## 1. Paying an IP Asset

We can pay an IP Asset like so:

```sol
ROYALTY_MODULE.payRoyaltyOnBehalf(childIpId, address(0), address(MERC20), 10);
```

This will send 10 $MERC20 to the `childIpId`'s [IP Royalty Vault](doc:ip-royalty-vault). From there, the child can claim revenue. In the next section, you'll see a working version of this.

## 2. Claim Revenue

When payments are made, they eventually end up in an IP Asset's [IP Royalty Vault](doc:ip-royalty-vault). From here, they are claimed/transferred to whoever owns the Royalty Tokens associated with it, which represent a % of revenue share for a given IP Asset's IP Royalty Vault.

The IP Account (the smart contract that represents the [🧩 IP Asset](doc:ip-asset)) is what holds 100% of the Royalty Tokens when it's first registered. So usually, it indeed holds most of the Royalty Tokens.

Let's create a test file under `test/5_Royalty.t.sol` to see it work and verify the results:

> 📘 Contract Addresses
>
> We have filled in the addresses from the Story contracts for you. However you can also find the addresses for them here: [Deployed Smart Contracts](doc:deployed-smart-contracts)

```sol test/5_Royalty.t.sol
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.26;

import { Test } from "forge-std/Test.sol";
// for testing purposes only
import { MockIPGraph } from "@storyprotocol/test/mocks/MockIPGraph.sol";
import { IPAssetRegistry } from "@storyprotocol/core/registries/IPAssetRegistry.sol";
import { LicenseRegistry } from "@storyprotocol/core/registries/LicenseRegistry.sol";
import { PILicenseTemplate } from "@storyprotocol/core/modules/licensing/PILicenseTemplate.sol";
import { RoyaltyPolicyLAP } from "@storyprotocol/core/modules/royalty/policies/LAP/RoyaltyPolicyLAP.sol";
import { PILFlavors } from "@storyprotocol/core/lib/PILFlavors.sol";
import { PILTerms } from "@storyprotocol/core/interfaces/modules/licensing/IPILicenseTemplate.sol";
import { LicensingModule } from "@storyprotocol/core/modules/licensing/LicensingModule.sol";
import { LicenseToken } from "@storyprotocol/core/LicenseToken.sol";
import { RoyaltyWorkflows } from "@storyprotocol/periphery/workflows/RoyaltyWorkflows.sol";
import { RoyaltyModule } from "@storyprotocol/core/modules/royalty/RoyaltyModule.sol";
import { MockERC20 } from "@storyprotocol/test/mocks/token/MockERC20.sol";

import { SimpleNFT } from "../src/mocks/SimpleNFT.sol";

// Run this test:
// forge test --fork-url https://aeneid.storyrpc.io/ --match-path test/5_Royalty.t.sol
contract RoyaltyTest is Test {
    address internal alice = address(0xa11ce);
    address internal bob = address(0xb0b);

    // For addresses, see https://docs.story.foundation/docs/deployed-smart-contracts
    // Protocol Core - IPAssetRegistry
    IPAssetRegistry internal IP_ASSET_REGISTRY = IPAssetRegistry(0x77319B4031e6eF1250907aa00018B8B1c67a244b);
    // Protocol Core - LicenseRegistry
    LicenseRegistry internal LICENSE_REGISTRY = LicenseRegistry(0x529a750E02d8E2f15649c13D69a465286a780e24);
    // Protocol Core - LicensingModule
    LicensingModule internal LICENSING_MODULE = LicensingModule(0x04fbd8a2e56dd85CFD5500A4A4DfA955B9f1dE6f);
    // Protocol Core - PILicenseTemplate
    PILicenseTemplate internal PIL_TEMPLATE = PILicenseTemplate(0x2E896b0b2Fdb7457499B56AAaA4AE55BCB4Cd316);
    // Protocol Core - RoyaltyPolicyLAP
    RoyaltyPolicyLAP internal ROYALTY_POLICY_LAP = RoyaltyPolicyLAP(0xBe54FB168b3c982b7AaE60dB6CF75Bd8447b390E);
    // Protocol Core - LicenseToken
    LicenseToken internal LICENSE_TOKEN = LicenseToken(0xFe3838BFb30B34170F00030B52eA4893d8aAC6bC);
    // Protocol Core - RoyaltyModule
    RoyaltyModule internal ROYALTY_MODULE = RoyaltyModule(0xD2f60c40fEbccf6311f8B47c4f2Ec6b040400086);
    // Protocol Periphery - RoyaltyWorkflows
    RoyaltyWorkflows internal ROYALTY_WORKFLOWS = RoyaltyWorkflows(0x9515faE61E0c0447C6AC6dEe5628A2097aFE1890);
    // Mock - MERC20
    MockERC20 internal MERC20 = MockERC20(0xF2104833d386a2734a4eB3B8ad6FC6812F29E38E);

    SimpleNFT public SIMPLE_NFT;
    uint256 public tokenId;
    address public ipId;
    uint256 public licenseTermsId;
    uint256 public startLicenseTokenId;
    address public childIpId;

    function setUp() public {
        // this is only for testing purposes
        // due to our IPGraph precompile not being
        // deployed on the fork
        vm.etch(address(0x0101), address(new MockIPGraph()).code);

        SIMPLE_NFT = new SimpleNFT("Simple IP NFT", "SIM");
        tokenId = SIMPLE_NFT.mint(alice);
        ipId = IP_ASSET_REGISTRY.register(block.chainid, address(SIMPLE_NFT), tokenId);

        licenseTermsId = PIL_TEMPLATE.registerLicenseTerms(
            PILFlavors.commercialRemix({
                mintingFee: 0,
                commercialRevShare: 10 * 10 ** 6, // 10%
                royaltyPolicy: address(ROYALTY_POLICY_LAP),
                currencyToken: address(MERC20)
            })
        );

        vm.prank(alice);
        LICENSING_MODULE.attachLicenseTerms(ipId, address(PIL_TEMPLATE), licenseTermsId);
        startLicenseTokenId = LICENSING_MODULE.mintLicenseTokens({
            licensorIpId: ipId,
            licenseTemplate: address(PIL_TEMPLATE),
            licenseTermsId: licenseTermsId,
            amount: 2,
            receiver: bob,
            royaltyContext: "", // for PIL, royaltyContext is empty string
            maxMintingFee: 0,
            maxRevenueShare: 0
        });

        // Registers a child IP (owned by Bob) as a derivative of Alice's IP.
        uint256 childTokenId = SIMPLE_NFT.mint(bob);
        childIpId = IP_ASSET_REGISTRY.register(block.chainid, address(SIMPLE_NFT), childTokenId);

        uint256[] memory licenseTokenIds = new uint256[](1);
        licenseTokenIds[0] = startLicenseTokenId;

        vm.prank(bob);
        LICENSING_MODULE.registerDerivativeWithLicenseTokens({
            childIpId: childIpId,
            licenseTokenIds: licenseTokenIds,
            royaltyContext: "", // empty for PIL
            maxRts: 0
        });
    }

    /// @notice Pays MERC20 to Bob's IP. Some of this MERC20 is then claimable
    /// by Alice's IP.
    /// @dev In this case, this contract will act as the 3rd party paying MERC20
    /// to Bob (the child IP).
    function test_claimAllRevenue() public {
        // ADMIN SETUP
        // We mint 100 MERC20 to this contract so it has some money to pay.
        MERC20.mint(address(this), 100);
        // We approve the Royalty Module to spend MERC20 on our behalf, which
        // it will do using `payRoyaltyOnBehalf`.
        MERC20.approve(address(ROYALTY_MODULE), 10);

        // This contract pays 10 MERC20 to Bob's IP.
        ROYALTY_MODULE.payRoyaltyOnBehalf(childIpId, address(0), address(MERC20), 10);

        // Now that Bob's IP has been paid, Alice can claim her share (1 MERC20, which
        // is 10% as specified in the license terms)
        address[] memory childIpIds = new address[](1);
        address[] memory royaltyPolicies = new address[](1);
        address[] memory currencyTokens = new address[](1);
        childIpIds[0] = childIpId;
        royaltyPolicies[0] = address(ROYALTY_POLICY_LAP);
        currencyTokens[0] = address(MERC20);

        uint256[] memory amountsClaimed = ROYALTY_WORKFLOWS.claimAllRevenue({
            ancestorIpId: ipId,
            claimer: ipId,
            childIpIds: childIpIds,
            royaltyPolicies: royaltyPolicies,
            currencyTokens: currencyTokens
        });

        // Check that 1 MERC20 was claimed by Alice's IP Account
        assertEq(amountsClaimed[0], 1);
        // Check that Alice's IP Account now has 1 MERC20 in its balance.
        assertEq(MERC20.balanceOf(ipId), 1);
        // Check that Bob's IP now has 9 MERC20 in its Royalty Vault, which it
        // can claim to its IP Account at a later point if he wants.
        assertEq(MERC20.balanceOf(ROYALTY_MODULE.ipRoyaltyVaults(childIpId)), 9);
    }
}
```

## 3. Test Your Code!

Run `forge build`. If everything is successful, the command should successfully compile.

Now run the test by executing the following command:

```shell
forge test --fork-url https://aeneid.storyrpc.io/ --match-path test/5_Royalty.t.sol
```

## 4. Dispute an IP

Congratulations, you claimed revenue using the [💸 Royalty Module](doc:royalty-module)!

<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/story-protocol-boilerplate/blob/main/test/5_Royalty.t.sol" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    Follow the completed code all the way through.
  </Card>
</Cards>

Now what happens if an IP Asset doesn't pay their due share? We can dispute the IP on-chain, which we will cover on the next page.

> 🚧 Coming soon!

# ⚙️ Smart Contract Guide
In this section, we will briefly go over the protocol contracts and then guide you through how to start building on top of the protocol. If you haven't yet familiarized yourself with the overall architecture, we recommend first going over the [Architecture Overview](doc:overview) section.

## Smart Contract Tutorial

<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/story-protocol-boilerplate" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    Skip the tutorial and view the completed code. Follow the README instructions to run the tests, or go to the `/test` folder to view all of the example contracts.
  </Card>
</Cards>

**If you want to set things up from scratch**, then continue with the following tutorials, starting with the [Setup Your Own Project](doc:sc-setup) step.

## Our Smart Contracts

As of the current version, our Proof-of-Creativity Protocol is compatible with all EVM chains and is written as a set of Smart Contracts in Solidity. There are two repositories that you may interact with as a developer:

* [Story Protocol Core](https://github.com/storyprotocol/protocol-core-v1) - This repository contains the core protocol logic, consisting of a thin IP registry (the [IP Asset Registry](doc:ip-asset-registry)), a set of [🧱 Modules](doc:story-modules) defining logic around [📜 Licensing](doc:licensing-module), [💸 Royalty](doc:royalty-module), [❌ Dispute](doc:dispute-module), metadata, and a module manager for administering module and user access control.
* [Story Protocol Periphery](https://github.com/storyprotocol/protocol-periphery-v1)- Whereas the core contracts deal with the underlying protocol logic, the periphery contracts deal with protocol extensions that greatly increase UX and simplify IPA management. This is mostly handled through the [📦 SPG](doc:spg).

## Deploy & Verify Contracts on Story

> The approach to deploy & verify contracts comes from the [Blockscout official documentation](https://docs.blockscout.com/developer-support/verifying-a-smart-contract/foundry-verification).

Verify a contract with Blockscout right after deployment (make sure you add "/api/" to the end of the Blockscout homepage explorer URL):

```shell Script
forge create \
  --rpc-url <rpc_https_endpoint> \
  --private-key $PRIVATE_KEY \
  <contract_file>:<contract_name> \
  --verify \
  --verifier blockscout \
  --verifier-url <blockscout_homepage_explorer_url>/api/
```

Or if using foundry scripts:

```shell Script
forge script <script_file> \
  --rpc-url <rpc_https_endpoint> \
  --private-key $PRIVATE_KEY \
  --broadcast \
  --verify \
  --verifier blockscout \
  --verifier-url <blockscout_homepage_explorer_url>/api/
```

# Mint a License Token
<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/story-protocol-boilerplate/blob/main/test/3_LicenseToken.t.sol" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    Follow the completed code all the way through.
  </Card>
</Cards>

This section demonstrates how to mint a [License Token](doc:license-token) from an [🧩 IP Asset](doc:ip-asset). You can only mint a License Token from an IP Asset if the IP Asset has [License Terms](doc:license-terms) attached to it. A License Token is minted as an ERC-721.

There are two reasons you'd mint a License Token:

1. To hold the license and be able to use the underlying IP Asset as the license described (for ex. "Can use commercially as long as you provide proper attribution and share 5% of your revenue)
2. Use the license token to link another IP Asset as a derivative of it. *Note though that, as you'll see later, some SDK functions don't require you to explicitly mint a license token first in order to register a derivative, and will actually handle it for you behind the scenes.*

### :warning: Prerequisites

There are a few steps you have to complete before you can start the tutorial.

1. Complete the [Setup Your Own Project](doc:sc-setup)
2. An IP Asset has License Terms attached to it. You can learn how to do that [here](doc:sc-attach-license-terms)

## 1. Mint License

Let's say that IP Asset (`ipId = 0x01`) has License Terms (`licenseTermdId = 10`) attached to it. We want to mint 2 License Tokens with those terms to a specific wallet address (`0x02`).

> 🚧 Paid Licenses
>
> Be mindful that some IP Assets may have license terms attached that require the user minting the license to pay a `mintingFee`.

Let's create a test file under `test/3_LicenseToken.t.sol` to see it work and verify the results:

> 📘 Contract Addresses
>
> We have filled in the addresses from the Story contracts for you. However you can also find the addresses for them here: [Deployed Smart Contracts](doc:deployed-smart-contracts)

```sol test/3_LicenseToken.t.sol
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.26;

import { Test } from "forge-std/Test.sol";
// for testing purposes only
import { MockIPGraph } from "@storyprotocol/test/mocks/MockIPGraph.sol";
import { IIPAssetRegistry } from "@storyprotocol/core/interfaces/registries/IIPAssetRegistry.sol";
import { IPILicenseTemplate } from "@storyprotocol/core/interfaces/modules/licensing/IPILicenseTemplate.sol";
import { ILicensingModule } from "@storyprotocol/core/interfaces/modules/licensing/ILicensingModule.sol";
import { ILicenseToken } from "@storyprotocol/core/interfaces/ILicenseToken.sol";
import { RoyaltyPolicyLAP } from "@storyprotocol/core/modules/royalty/policies/LAP/RoyaltyPolicyLAP.sol";
import { PILFlavors } from "@storyprotocol/core/lib/PILFlavors.sol";
import { PILTerms } from "@storyprotocol/core/interfaces/modules/licensing/IPILicenseTemplate.sol";

import { SimpleNFT } from "../src/mocks/SimpleNFT.sol";

// Run this test:
// forge test --fork-url https://aeneid.storyrpc.io/ --match-path test/3_LicenseToken.t.sol
contract LicenseTokenTest is Test {
    address internal alice = address(0xa11ce);
    address internal bob = address(0xb0b);

    // For addresses, see https://docs.story.foundation/docs/deployed-smart-contracts
    // Protocol Core - IPAssetRegistry
    IIPAssetRegistry internal IP_ASSET_REGISTRY = IIPAssetRegistry(0x77319B4031e6eF1250907aa00018B8B1c67a244b);
    // Protocol Core - LicensingModule
    ILicensingModule internal LICENSING_MODULE = ILicensingModule(0x04fbd8a2e56dd85CFD5500A4A4DfA955B9f1dE6f);
    // Protocol Core - PILicenseTemplate
    IPILicenseTemplate internal PIL_TEMPLATE = IPILicenseTemplate(0x2E896b0b2Fdb7457499B56AAaA4AE55BCB4Cd316);
    // Protocol Core - RoyaltyPolicyLAP
    address internal ROYALTY_POLICY_LAP = 0xBe54FB168b3c982b7AaE60dB6CF75Bd8447b390E;
    // Protocol Core - LicenseToken
    ILicenseToken internal LICENSE_TOKEN = ILicenseToken(0xFe3838BFb30B34170F00030B52eA4893d8aAC6bC);
    // Revenue Token - MERC20
    address internal MERC20 = 0xF2104833d386a2734a4eB3B8ad6FC6812F29E38E;

    SimpleNFT public SIMPLE_NFT;
    uint256 public tokenId;
    address public ipId;
    uint256 public licenseTermsId;

    function setUp() public {
        // this is only for testing purposes
        // due to our IPGraph precompile not being
        // deployed on the fork
        vm.etch(address(0x0101), address(new MockIPGraph()).code);

        SIMPLE_NFT = new SimpleNFT("Simple IP NFT", "SIM");
        tokenId = SIMPLE_NFT.mint(alice);
        ipId = IP_ASSET_REGISTRY.register(block.chainid, address(SIMPLE_NFT), tokenId);

        licenseTermsId = PIL_TEMPLATE.registerLicenseTerms(
            PILFlavors.commercialRemix({
                mintingFee: 0,
                commercialRevShare: 10 * 10 ** 6, // 10%
                royaltyPolicy: ROYALTY_POLICY_LAP,
                currencyToken: MERC20
            })
        );

        vm.prank(alice);
        LICENSING_MODULE.attachLicenseTerms(ipId, address(PIL_TEMPLATE), licenseTermsId);
    }

    /// @notice Mints license tokens for an IP Asset.
    /// Anyone can mint a license token.
    function test_mintLicenseToken() public {
        uint256 startLicenseTokenId = LICENSING_MODULE.mintLicenseTokens({
            licensorIpId: ipId,
            licenseTemplate: address(PIL_TEMPLATE),
            licenseTermsId: licenseTermsId,
            amount: 2,
            receiver: bob,
            royaltyContext: "", // for PIL, royaltyContext is empty string
            maxMintingFee: 0,
            maxRevenueShare: 0
        });

        assertEq(LICENSE_TOKEN.ownerOf(startLicenseTokenId), bob);
        assertEq(LICENSE_TOKEN.ownerOf(startLicenseTokenId + 1), bob);
    }
}
```

## 2. Test Your Code!

Run `forge build`. If everything is successful, the command should successfully compile.

Now run the test by executing the following command:

```shell
forge test --fork-url https://aeneid.storyrpc.io/ --match-path test/3_LicenseToken.t.sol
```

## 3. Register a Derivative

<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/story-protocol-boilerplate/blob/main/test/3_LicenseToken.t.sol" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    Follow the completed code all the way through.
  </Card>
</Cards>

Now that we have minted a License Token, we can hold it or use it to link an IP Asset as a derivative. We will go over that on the next page.

# Register an IP Asset
<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/story-protocol-boilerplate/blob/main/test/0_IPARegistrar.t.sol" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    Follow the completed code all the way through.
  </Card>
</Cards>

Let's say you have some off-chain IP (ex. a book, a character, a drawing, etc). In order to register that IP on Story, you first need to mint an NFT. This NFT is the **ownership** over the IP. Then you **register** that NFT on Story, turning it into an [🧩 IP Asset](doc:ip-asset). The below tutorial will walk you through how to do this.

### :warning: Prerequisites

There are a few steps you have to complete before you can start the tutorial.

1. Complete the [Setup Your Own Project](doc:sc-setup)

## 0/. Before We Start

There are two scenarios:

1. You already have a **custom** ERC-721 NFT contract and can mint from it
2. You want to create an [📦 SPG (Periphery)](doc:spg) NFT contract to do minting for you

## Scenario #1: You already have a custom ERC-721 NFT contract and can mint from it

If you already have an NFT minted, or you want to register IP using a custom-built ERC-721 contract, this is the section for you.

As you can see below, the registration process is relatively straightforward. We use `SimpleNFT` as an example, but you can replace it with your own ERC-721 contract.

All you have to do is call `register` on the [IP Asset Registry](doc:ip-asset-registry) with:

* `chainid` - you can simply use `block.chainid`
* `tokenContract` - the address of your NFT collection
* `tokenId` - your NFT's ID

Let's create a test file under `test/0_IPARegistrar.sol` to see it work and verify the results:

> 📘 Contract Addresses
>
> We have filled in the addresses from the Story contracts for you. However you can also find the addresses for them here: [Deployed Smart Contracts](doc:deployed-smart-contracts)

> You can view the `SimpleNFT` contract we're using to test [here](https://github.com/storyprotocol/story-protocol-boilerplate/blob/main/src/mocks/SimpleNFT.sol).

```sol test/0_IPARegistrar.sol
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.26;

import { Test } from "forge-std/Test.sol";
import { IIPAssetRegistry } from "@storyprotocol/core/interfaces/registries/IIPAssetRegistry.sol";

// your own ERC-721 NFT contract
import { SimpleNFT } from "../src/mocks/SimpleNFT.sol";

// Run this test:
// forge test --fork-url https://aeneid.storyrpc.io/ --match-path test/0_IPARegistrar.t.sol
contract IPARegistrarTest is Test {
    address internal alice = address(0xa11ce);

    // For addresses, see https://docs.story.foundation/docs/deployed-smart-contracts
    // Protocol Core - IPAssetRegistry
    IIPAssetRegistry internal IP_ASSET_REGISTRY = IIPAssetRegistry(0x77319B4031e6eF1250907aa00018B8B1c67a244b);

    SimpleNFT public SIMPLE_NFT;

    function setUp() public {
        // Create a new Simple NFT collection
        SIMPLE_NFT = new SimpleNFT("Simple IP NFT", "SIM");
    }

    /// @notice Mint an NFT and then register it as an IP Asset.
    function test_register() public {
        uint256 expectedTokenId = SIMPLE_NFT.nextTokenId();
        address expectedIpId = IP_ASSET_REGISTRY.ipId(block.chainid, address(SIMPLE_NFT), expectedTokenId);

        uint256 tokenId = SIMPLE_NFT.mint(alice);
        address ipId = IP_ASSET_REGISTRY.register(block.chainid, address(SIMPLE_NFT), tokenId);

        assertEq(tokenId, expectedTokenId);
        assertEq(ipId, expectedIpId);
        assertEq(SIMPLE_NFT.ownerOf(tokenId), alice);
    }
}
```

## Scenario #2: You want to create an SPG NFT contract to do minting for you

If you don't have your own custom NFT contract, this is the section for you.

To achieve this, we will be using the [📦 SPG](doc:spg), which is a utility contract that allows us to combine multiple transactions into one. In this case, we'll be using the SPG's `mintAndRegisterIp` function which combines both minting an NFT and registering it as an IP Asset.

In order to use `mintAndRegisterIp`, we first have to create a new `SPGNFT` collection. We can do this simply by calling `createCollection` on the `StoryProtocolGateway` contract. Or, if you want to create your own `SPGNFT` for some reason, you can implement the [ISPGNFT](https://github.com/storyprotocol/protocol-periphery-v1/blob/main/contracts/interfaces/ISPGNFT.sol) contract interface. Follow the example below to see example parameters you can use to initialize a new SPGNFT.

Once you have your own SPGNFT, all you have to do is call `mintAndRegisterIp` with:

* `spgNftContract` - the address of your SPGNFT contract
* `recipient` - the address of who will receive the NFT and thus be the owner of the newly registered IP. *Note: remember that registering IP on Story is permissionless, so you can register an IP for someone else (by paying for the transaction) yet they can still be the owner of that IP Asset.*
* `ipMetadata` - the metadata associated with your NFT & IP. See [this](https://docs.story.foundation/docs/ip-asset#nft-vs-ip-metadata) section to better understand setting NFT & IP metadata.

Let's create a test file under `test/0_IPARegistrar.sol` to see it work and verify the results:

> 📘 Contract Addresses
>
> We have filled in the addresses from the Story contracts for you. However you can also find the addresses for them here: [Deployed Smart Contracts](doc:deployed-smart-contracts)

```sol test/0_IPARegistrar.sol
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.26;

import { Test } from "forge-std/Test.sol";
import { IIPAssetRegistry } from "@storyprotocol/core/interfaces/registries/IIPAssetRegistry.sol";
import { ISPGNFT } from "@storyprotocol/periphery/interfaces/ISPGNFT.sol";
import { IRegistrationWorkflows } from "@storyprotocol/periphery/interfaces/workflows/IRegistrationWorkflows.sol";
import { WorkflowStructs } from "@storyprotocol/periphery/lib/WorkflowStructs.sol";

// Run this test:
// forge test --fork-url https://aeneid.storyrpc.io/ --match-path test/0_IPARegistrar.t.sol
contract IPARegistrarTest is Test {
    address internal alice = address(0xa11ce);

    // For addresses, see https://docs.story.foundation/docs/deployed-smart-contracts
    // Protocol Core - IPAssetRegistry
    IIPAssetRegistry internal IP_ASSET_REGISTRY = IIPAssetRegistry(0x77319B4031e6eF1250907aa00018B8B1c67a244b);
    // Protocol Periphery - RegistrationWorkflows
    IRegistrationWorkflows internal REGISTRATION_WORKFLOWS =
        IRegistrationWorkflows(0xbe39E1C756e921BD25DF86e7AAa31106d1eb0424);

    ISPGNFT public SPG_NFT;

    function setUp() public {
        // Create a new NFT collection via SPG
        SPG_NFT = ISPGNFT(
            REGISTRATION_WORKFLOWS.createCollection(
                ISPGNFT.InitParams({
                    name: "Test Collection",
                    symbol: "TEST",
                    baseURI: "",
                    contractURI: "",
                    maxSupply: 100,
                    mintFee: 0,
                    mintFeeToken: address(0),
                    mintFeeRecipient: address(this),
                    owner: address(this),
                    mintOpen: true,
                    isPublicMinting: false
                })
            )
        );
    }

    /// @notice Mint an NFT and register it in the same call via the Story Protocol Gateway.
    /// @dev Requires the collection address that is passed into the `mintAndRegisterIp` function
    /// to be created via SPG (createCollection), as done above. Or, a contract that
    /// implements the `ISPGNFT` interface.
    function test_mintAndRegisterIp() public {
        uint256 expectedTokenId = SPG_NFT.totalSupply() + 1;
        address expectedIpId = IP_ASSET_REGISTRY.ipId(block.chainid, address(SPG_NFT), expectedTokenId);

        // Note: The caller of this function must be the owner of the SPG NFT Collection.
        // In this case, the owner of the SPG NFT Collection is the contract itself
        // because it deployed it in the `setup` function.
        // We can make `alice` the recipient of the NFT though, which makes her the
        // owner of not only the NFT, but therefore the IP Asset.
        (address ipId, uint256 tokenId) = REGISTRATION_WORKFLOWS.mintAndRegisterIp(
            address(SPG_NFT),
            alice,
            WorkflowStructs.IPMetadata({
                ipMetadataURI: "https://ipfs.io/ipfs/QmZHfQdFA2cb3ASdmeGS5K6rZjz65osUddYMURDx21bT73",
                ipMetadataHash: keccak256(
                    abi.encodePacked(
                        "{'title':'My IP Asset','description':'This is a test IP asset','ipType':'','relationships':[],'createdAt':'','watermarkImg':'https://picsum.photos/200','creators':[],'media':[],'attributes':[{'key':'Rarity','value':'Legendary'}],'tags':[]}"
                    )
                ),
                nftMetadataURI: "https://ipfs.io/ipfs/QmRL5PcK66J1mbtTZSw1nwVqrGxt98onStx6LgeHTDbEey",
                nftMetadataHash: keccak256(
                    abi.encodePacked(
                        "{'name':'Test NFT','description':'This is a test NFT','image':'https://picsum.photos/200'}"
                    )
                )
            }),
            true
        );

        assertEq(ipId, expectedIpId);
        assertEq(tokenId, expectedTokenId);
        assertEq(SPG_NFT.ownerOf(tokenId), alice);
    }
}
```

## Test Your Code!

Run `forge build`. If everything is successful, the command should successfully compile.

Now run the test by executing the following command:

```shell
forge test --fork-url https://aeneid.storyrpc.io/ --match-path test/IPARegistrar.t.sol
```

## Add License Terms to IP

Congratulations, you registered an IP!

<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/story-protocol-boilerplate/blob/main/test/0_IPARegistrar.t.sol" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    Follow the completed code all the way through.
  </Card>
</Cards>

Now that your IP is registered, you can create and attach [License Terms](doc:license-terms) to it. This will allow others to mint a license and use your IP, restricted by the terms.

We will go over this on the next page.

# Attach Terms to an IPA
<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/story-protocol-boilerplate/blob/main/test/2_AttachTerms.t.sol" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    Follow the completed code all the way through.
  </Card>
</Cards>

This section demonstrates how to attach [License Terms](doc:license-terms) to an [🧩 IP Asset](doc:ip-asset). By attaching terms, users can publicly mint [License Tokens](doc:license-token) (the on-chain "license") with those terms from the IP.

### :warning: Prerequisites

There are a few steps you have to complete before you can start the tutorial.

1. Complete the [Setup Your Own Project](doc:sc-setup)
2. Create License Terms and have a `licenseTermsId`. You can do that by following the [previous page](doc:sc-register-license-terms).

## 1. Attach License Terms

Now that we have created terms and have the associated `licenseTermsId`, we can attach them to an existing IP Asset.

Let's create a test file under `test/2_AttachTerms.t.sol` to see it work and verify the results:

> 📘 Contract Addresses
>
> We have filled in the addresses from the Story contracts for you. However you can also find the addresses for them here: [Deployed Smart Contracts](doc:deployed-smart-contracts)

```sol test/2_AttachTerms.t.sol
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.26;

import { Test } from "forge-std/Test.sol";
// for testing purposes only
import { MockIPGraph } from "@storyprotocol/test/mocks/MockIPGraph.sol";
import { IIPAssetRegistry } from "@storyprotocol/core/interfaces/registries/IIPAssetRegistry.sol";
import { ILicenseRegistry } from "@storyprotocol/core/interfaces/registries/ILicenseRegistry.sol";
import { IPILicenseTemplate } from "@storyprotocol/core/interfaces/modules/licensing/IPILicenseTemplate.sol";
import { ILicensingModule } from "@storyprotocol/core/interfaces/modules/licensing/ILicensingModule.sol";
import { PILFlavors } from "@storyprotocol/core/lib/PILFlavors.sol";
import { PILTerms } from "@storyprotocol/core/interfaces/modules/licensing/IPILicenseTemplate.sol";

import { SimpleNFT } from "../src/mocks/SimpleNFT.sol";

// Run this test:
// forge test --fork-url https://aeneid.storyrpc.io/ --match-path test/2_AttachTerms.t.sol
contract AttachTermsTest is Test {
    address internal alice = address(0xa11ce);

    // For addresses, see https://docs.story.foundation/docs/deployed-smart-contracts
    // Protocol Core - IPAssetRegistry
    IIPAssetRegistry internal IP_ASSET_REGISTRY = IIPAssetRegistry(0x77319B4031e6eF1250907aa00018B8B1c67a244b);
    // Protocol Core - LicenseRegistry
    ILicenseRegistry internal LICENSE_REGISTRY = ILicenseRegistry(0x529a750E02d8E2f15649c13D69a465286a780e24);
    // Protocol Core - LicensingModule
    ILicensingModule internal LICENSING_MODULE = ILicensingModule(0x04fbd8a2e56dd85CFD5500A4A4DfA955B9f1dE6f);
    // Protocol Core - PILicenseTemplate
    IPILicenseTemplate internal PIL_TEMPLATE = IPILicenseTemplate(0x2E896b0b2Fdb7457499B56AAaA4AE55BCB4Cd316);
    // Protocol Core - RoyaltyPolicyLAP
    address internal ROYALTY_POLICY_LAP = 0xBe54FB168b3c982b7AaE60dB6CF75Bd8447b390E;
    // Revenue Token - MERC20
    address internal MERC20 = 0xF2104833d386a2734a4eB3B8ad6FC6812F29E38E;

    SimpleNFT public SIMPLE_NFT;
    uint256 public tokenId;
    address public ipId;
    uint256 public licenseTermsId;

    function setUp() public {
        // this is only for testing purposes
        // due to our IPGraph precompile not being
        // deployed on the fork
        vm.etch(address(0x0101), address(new MockIPGraph()).code);

        SIMPLE_NFT = new SimpleNFT("Simple IP NFT", "SIM");
        tokenId = SIMPLE_NFT.mint(alice);
        ipId = IP_ASSET_REGISTRY.register(block.chainid, address(SIMPLE_NFT), tokenId);

        // Register random Commercial Remix terms so we can attach them later
        licenseTermsId = PIL_TEMPLATE.registerLicenseTerms(
            PILFlavors.commercialRemix({
                mintingFee: 0,
                commercialRevShare: 10 * 10 ** 6, // 10%
                royaltyPolicy: ROYALTY_POLICY_LAP,
                currencyToken: MERC20
            })
        );
    }

    /// @notice Attaches license terms to an IP Asset.
    /// @dev Only the owner of an IP Asset can attach license terms to it.
    /// So in this case, alice has to be the caller of the function because
    /// she owns the NFT associated with the IP Asset.
    function test_attachLicenseTerms() public {
        vm.prank(alice);
        LICENSING_MODULE.attachLicenseTerms(ipId, address(PIL_TEMPLATE), licenseTermsId);

        assertTrue(LICENSE_REGISTRY.hasIpAttachedLicenseTerms(ipId, address(PIL_TEMPLATE), licenseTermsId));
        assertEq(LICENSE_REGISTRY.getAttachedLicenseTermsCount(ipId), 1);
        (address licenseTemplate, uint256 attachedLicenseTermsId) = LICENSE_REGISTRY.getAttachedLicenseTerms({
            ipId: ipId,
            index: 0
        });
        assertEq(licenseTemplate, address(PIL_TEMPLATE));
        assertEq(attachedLicenseTermsId, licenseTermsId);
    }
}
```

## 2. Test Your Code!

Run `forge build`. If everything is successful, the command should successfully compile.

Now run the test by executing the following command:

```shell
forge test --fork-url https://aeneid.storyrpc.io/ --match-path test/2_AttachTerms.t.sol
```

## 3. Mint a License

Congraulations, you attached terms to an IPA!

<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/story-protocol-boilerplate/blob/main/test/2_AttachTerms.t.sol" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    Follow the completed code all the way through.
  </Card>
</Cards>

Now that we have attached License Terms to our IP, the next step is minting a License Token, which we'll go over on the next page.

# Using an Example
<Cards columns={2}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/story-protocol-boilerplate/blob/main/src/Example.sol" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    See the completed code.
  </Card>

  <Card title="Video Walkthrough" href="https://www.youtube.com/watch?v=X421IuZENqM" icon="fa-video" iconColor="#FF0000" target="_blank">
    Check out a video walkthrough of this tutorial!
  </Card>
</Cards>

# Writing the Smart Contract

Now that we have walked through each of the individual steps, let's try to write, deploy, and verify our own smart contract.

## Register IPA, Register License Terms, and Attach to IPA

In this first section, we will combine a few of the tutorials into one. We will create a function named `mintAndRegisterAndCreateTermsAndAttach` that allows you to mint & register a new IP Asset, register new License Terms, and attach those terms to an IP Asset. It will also accept a `receiver` field to be the owner of the new IP Asset.

### ⚠️ Prerequisites

* Complete [Register an IP Asset](doc:sc-register-an-ip-asset)
* Complete [Register License Terms](doc:sc-register-license-terms)
* Complete [Attach Terms to an IPA](doc:sc-attach-license-terms)

### Writing our Contract

Create a new file under `./src/Example.sol` and paste the following:

> 📘 Contract Addresses
>
> In order to get the contract addresses to pass in the constructor, go to [Deployed Smart Contracts](doc:deployed-smart-contracts).

```sol src/Example.sol
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.26;

import { IIPAssetRegistry } from "@storyprotocol/core/interfaces/registries/IIPAssetRegistry.sol";
import { ILicensingModule } from "@storyprotocol/core/interfaces/modules/licensing/ILicensingModule.sol";
import { IPILicenseTemplate } from "@storyprotocol/core/interfaces/modules/licensing/IPILicenseTemplate.sol";
import { PILFlavors } from "@storyprotocol/core/lib/PILFlavors.sol";

import { SimpleNFT } from "./mocks/SimpleNFT.sol";

import { ERC721Holder } from "@openzeppelin/contracts/token/ERC721/utils/ERC721Holder.sol";

/// @notice An example contract that demonstrates how to mint an NFT, register it as an IP Asset,
/// attach license terms to it, mint a license token from it, and register it as a derivative of the parent.
contract Example is ERC721Holder {
  IIPAssetRegistry public immutable IP_ASSET_REGISTRY;
  ILicensingModule public immutable LICENSING_MODULE;
  IPILicenseTemplate public immutable PIL_TEMPLATE;
  address public immutable ROYALTY_POLICY_LAP;
  address public immutable WIP;
  SimpleNFT public immutable SIMPLE_NFT;

  constructor(
    address ipAssetRegistry,
    address licensingModule,
    address pilTemplate,
    address royaltyPolicyLAP,
    address wip
  ) {
    IP_ASSET_REGISTRY = IIPAssetRegistry(ipAssetRegistry);
    LICENSING_MODULE = ILicensingModule(licensingModule);
    PIL_TEMPLATE = IPILicenseTemplate(pilTemplate);
    ROYALTY_POLICY_LAP = royaltyPolicyLAP;
    WIP = wip;
    // Create a new Simple NFT collection
    SIMPLE_NFT = new SimpleNFT("Simple IP NFT", "SIM");
  }

  /// @notice Mint an NFT, register it as an IP Asset, and attach License Terms to it.
  /// @param receiver The address that will receive the NFT/IPA.
  /// @return tokenId The token ID of the NFT representing ownership of the IPA.
  /// @return ipId The address of the IP Account.
  /// @return licenseTermsId The ID of the license terms.
  function mintAndRegisterAndCreateTermsAndAttach(
    address receiver
  ) external returns (uint256 tokenId, address ipId, uint256 licenseTermsId) {
    // We mint to this contract so that it has permissions
    // to attach license terms to the IP Asset.
    // We will later transfer it to the intended `receiver`
    tokenId = SIMPLE_NFT.mint(address(this));
    ipId = IP_ASSET_REGISTRY.register(block.chainid, address(SIMPLE_NFT), tokenId);

    // register license terms so we can attach them later
    licenseTermsId = PIL_TEMPLATE.registerLicenseTerms(
      PILFlavors.commercialRemix({
        mintingFee: 0,
        commercialRevShare: 10 * 10 ** 6, // 10%
        royaltyPolicy: ROYALTY_POLICY_LAP,
        currencyToken: WIP
      })
    );

    // attach the license terms to the IP Asset
    LICENSING_MODULE.attachLicenseTerms(ipId, address(PIL_TEMPLATE), licenseTermsId);

    // transfer the NFT to the receiver so it owns the IPA
    SIMPLE_NFT.transferFrom(address(this), receiver, tokenId);
  }
}
```

## Mint a License Token and Register as Derivative

In this next section, we will combine a few of the later tutorials into one. We will create a function named `mintLicenseTokenAndRegisterDerivative` that allows a potentially different user to register their own "child" (derivative) IP Asset, mint a License Token from the "parent" (root) IP Asset, and register their child IPA as a derivative of the parent IPA. It will accept a few parameters:

1. `parentIpId`: the `ipId` of the parent IPA
2. `licenseTermsId`: the id of the License Terms you want to mint a License Token for
3. `receiver`: the owner of the child IPA

### :warning: Prerequisites

* Complete [Mint a License Token](doc:sc-mint-license-token)
* Complete [Register a Derivative](doc:sc-remix-ipa)

### Writing our Contract

In your `Example.sol` contract, add the following function at the bottom:

```sol src/Example.sol
/// @notice Mint and register a new child IPA, mint a License Token
/// from the parent, and register it as a derivative of the parent.
/// @param parentIpId The ipId of the parent IPA.
/// @param licenseTermsId The ID of the license terms you will
/// mint a license token from.
/// @param receiver The address that will receive the NFT/IPA.
/// @return childTokenId The token ID of the NFT representing ownership of the child IPA.
/// @return childIpId The address of the child IPA.
function mintLicenseTokenAndRegisterDerivative(
  address parentIpId,
  uint256 licenseTermsId,
  address receiver
) external returns (uint256 childTokenId, address childIpId) {
  // We mint to this contract so that it has permissions
  // to register itself as a derivative of another
  // IP Asset.
  // We will later transfer it to the intended `receiver`
  childTokenId = SIMPLE_NFT.mint(address(this));
  childIpId = IP_ASSET_REGISTRY.register(block.chainid, address(SIMPLE_NFT), childTokenId);

  // mint a license token from the parent
  uint256 licenseTokenId = LICENSING_MODULE.mintLicenseTokens({
    licensorIpId: parentIpId,
    licenseTemplate: address(PIL_TEMPLATE),
    licenseTermsId: licenseTermsId,
    amount: 1,
    // mint the license token to this contract so it can
    // use it to register as a derivative of the parent
    receiver: address(this),
    royaltyContext: "", // for PIL, royaltyContext is empty string
    maxMintingFee: 0,
    maxRevenueShare: 0
  });

  uint256[] memory licenseTokenIds = new uint256[](1);
  licenseTokenIds[0] = licenseTokenId;

  // register the new child IPA as a derivative
  // of the parent
  LICENSING_MODULE.registerDerivativeWithLicenseTokens({
    childIpId: childIpId,
    licenseTokenIds: licenseTokenIds,
    royaltyContext: "", // empty for PIL
    maxRts: 0
  });

  // transfer the NFT to the receiver so it owns the child IPA
  SIMPLE_NFT.transferFrom(address(this), receiver, childTokenId);
}
```

# Testing our Contract

Create another new file under `test/Example.t.sol` and paste the following:

```sol Example.t.sol
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.26;

import { Test } from "forge-std/Test.sol";
// for testing purposes only
import { MockIPGraph } from "@storyprotocol/test/mocks/MockIPGraph.sol";
import { IIPAssetRegistry } from "@storyprotocol/core/interfaces/registries/IIPAssetRegistry.sol";
import { ILicenseRegistry } from "@storyprotocol/core/interfaces/registries/ILicenseRegistry.sol";

import { Example } from "../src/Example.sol";
import { SimpleNFT } from "../src/mocks/SimpleNFT.sol";

// Run this test:
// forge test --fork-url https://aeneid.storyrpc.io/ --match-path test/Example.t.sol
contract ExampleTest is Test {
  address internal alice = address(0xa11ce);
  address internal bob = address(0xb0b);

  // For addresses, see https://docs.story.foundation/docs/deployed-smart-contracts
  // Protocol Core - IPAssetRegistry
  address internal ipAssetRegistry = 0x77319B4031e6eF1250907aa00018B8B1c67a244b;
  // Protocol Core - LicenseRegistry
  address internal licenseRegistry = 0x529a750E02d8E2f15649c13D69a465286a780e24;
  // Protocol Core - LicensingModule
  address internal licensingModule = 0x04fbd8a2e56dd85CFD5500A4A4DfA955B9f1dE6f;
  // Protocol Core - PILicenseTemplate
  address internal pilTemplate = 0x2E896b0b2Fdb7457499B56AAaA4AE55BCB4Cd316;
  // Protocol Core - RoyaltyPolicyLAP
  address internal royaltyPolicyLAP = 0xBe54FB168b3c982b7AaE60dB6CF75Bd8447b390E;
  // Revenue Token - WIP
  address internal wip = 0x1514000000000000000000000000000000000000;

  SimpleNFT public SIMPLE_NFT;
  Example public EXAMPLE;

  function setUp() public {
    // this is only for testing purposes
    // due to our IPGraph precompile not being
    // deployed on the fork
    vm.etch(address(0x0101), address(new MockIPGraph()).code);

    EXAMPLE = new Example(ipAssetRegistry, licensingModule, pilTemplate, royaltyPolicyLAP, wip);
    SIMPLE_NFT = SimpleNFT(EXAMPLE.SIMPLE_NFT());
  }

  function test_mintAndRegisterAndCreateTermsAndAttach() public {
    ILicenseRegistry LICENSE_REGISTRY = ILicenseRegistry(licenseRegistry);
    IIPAssetRegistry IP_ASSET_REGISTRY = IIPAssetRegistry(ipAssetRegistry);

    uint256 expectedTokenId = SIMPLE_NFT.nextTokenId();
    address expectedIpId = IP_ASSET_REGISTRY.ipId(block.chainid, address(SIMPLE_NFT), expectedTokenId);

    (uint256 tokenId, address ipId, uint256 licenseTermsId) = EXAMPLE.mintAndRegisterAndCreateTermsAndAttach(alice);

    assertEq(tokenId, expectedTokenId);
    assertEq(ipId, expectedIpId);
    assertEq(SIMPLE_NFT.ownerOf(tokenId), alice);

    assertTrue(LICENSE_REGISTRY.hasIpAttachedLicenseTerms(ipId, pilTemplate, licenseTermsId));
    assertEq(LICENSE_REGISTRY.getAttachedLicenseTermsCount(ipId), 1);
    (address licenseTemplate, uint256 attachedLicenseTermsId) = LICENSE_REGISTRY.getAttachedLicenseTerms({
      ipId: ipId,
      index: 0
    });
    assertEq(licenseTemplate, pilTemplate);
    assertEq(attachedLicenseTermsId, licenseTermsId);
  }

  function test_mintLicenseTokenAndRegisterDerivative() public {
    ILicenseRegistry LICENSE_REGISTRY = ILicenseRegistry(licenseRegistry);
    IIPAssetRegistry IP_ASSET_REGISTRY = IIPAssetRegistry(ipAssetRegistry);

    (uint256 parentTokenId, address parentIpId, uint256 licenseTermsId) = EXAMPLE
    .mintAndRegisterAndCreateTermsAndAttach(alice);

    (uint256 childTokenId, address childIpId) = EXAMPLE.mintLicenseTokenAndRegisterDerivative(
      parentIpId,
      licenseTermsId,
      bob
    );

    assertTrue(LICENSE_REGISTRY.hasDerivativeIps(parentIpId));
    assertTrue(LICENSE_REGISTRY.isParentIp(parentIpId, childIpId));
    assertTrue(LICENSE_REGISTRY.isDerivativeIp(childIpId));
    assertEq(LICENSE_REGISTRY.getDerivativeIpCount(parentIpId), 1);
    assertEq(LICENSE_REGISTRY.getParentIpCount(childIpId), 1);
    assertEq(LICENSE_REGISTRY.getParentIp({ childIpId: childIpId, index: 0 }), parentIpId);
    assertEq(LICENSE_REGISTRY.getDerivativeIp({ parentIpId: parentIpId, index: 0 }), childIpId);
  }
}

```

Run `forge build`. If everything is successful, the command should successfully compile.

To test this out, simply run the following command:

```shell
forge test --fork-url https://aeneid.storyrpc.io/ --match-path test/Example.t.sol
```

# Deploy & Verify the Example Contract

The `--constructor-args` come from [Deployed Smart Contracts](doc:deployed-smart-contracts).

```shell
forge create \
  --rpc-url https://aeneid.storyrpc.io/ \
  --private-key $PRIVATE_KEY \
  ./src/Example.sol:Example \
  --verify \
  --verifier blockscout \
  --verifier-url https://aeneid.storyscan.xyz/api/ \
  --constructor-args 0x77319B4031e6eF1250907aa00018B8B1c67a244b 0x04fbd8a2e56dd85CFD5500A4A4DfA955B9f1dE6f 0x2E896b0b2Fdb7457499B56AAaA4AE55BCB4Cd316 0xBe54FB168b3c982b7AaE60dB6CF75Bd8447b390E 0xF2104833d386a2734a4eB3B8ad6FC6812F29E38E
```

If everything worked correctly, you should see something like `Deployed to: 0xfb0923D531C1ca54AB9ee10CB8364b23d0C7F47d` in the console. Paste that address into [the explorer](https://aeneid.storyscan.xyz/) and see your verified contract!

# Great job! :)

<Cards columns={2}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/story-protocol-boilerplate/blob/main/src/Example.sol" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    See the completed code.
  </Card>

  <Card title="Video Walkthrough" href="https://www.youtube.com/watch?v=X421IuZENqM" icon="fa-video" iconColor="#FF0000" target="_blank">
    Check out a video walkthrough of this tutorial!
  </Card>
</Cards>

# Setup Your Own Project
In this guide, we will show you how to setup the Story smart contract development environment in just a few minutes

### :warning: Prerequisites

* [Install Foundry](https://book.getfoundry.sh/getting-started/installation)
* [Install yarn](https://classic.yarnpkg.com/lang/en/docs/install/)

## Creating a Project

1. Run the following in a new directory of your choice: `yarn init`
2. Set up foundry using the following command: `forge init --force`
3. Open up your `foundry.toml` file and replace it with this

```toml foundry.toml
[profile.default]
out = 'out'
libs = ['node_modules', 'lib']
cache_path  = 'forge-cache'
gas_reports = ["*"]
optimizer = true
optimizer_runs = 20000
test = 'test'
solc = '0.8.26'
fs_permissions = [{ access = 'read', path = './out' }, { access = 'read-write', path = './deploy-out' }]
evm_version = 'cancun'
```

4. Remove the placeholder test contracts: `rm src/Counter.sol test/Counter.t.sol`

### :package: Installing Dependencies

Now, we are ready to start installing our dependencies. To incorporate the Story Protocol core and periphery modules, run the following to have them added to your `package.json`. We will also install `openzeppelin` and `erc6551` as a dependency for the contract and test.

```powershell Terminal
# note: you can run them one-by-one, or all at once
yarn add @story-protocol/protocol-core@https://github.com/storyprotocol/protocol-core-v1
yarn add @story-protocol/protocol-periphery@https://github.com/storyprotocol/protocol-periphery-v1
yarn add @openzeppelin/contracts
yarn add @openzeppelin/contracts-upgradeable
yarn add erc6551
yarn add solady
```

Additionally, for working with Foundry's test kit, we also recommend adding the following `devDependencies`:

```Text Terminal
yarn add -D https://github.com/dapphub/ds-test
yarn add -D github:foundry-rs/forge-std#v1.7.6
```

Then, create a file in the root folder named `remappings.txt` and paste the following:

```Text remappings.txt
@openzeppelin/=node_modules/@openzeppelin/
@storyprotocol/core/=node_modules/@story-protocol/protocol-core/contracts/
@storyprotocol/periphery/=node_modules/@story-protocol/protocol-periphery/contracts/
erc6551/=node_modules/erc6551/
forge-std/=node_modules/forge-std/src/
ds-test/=node_modules/ds-test/src/
@storyprotocol/test/=node_modules/@story-protocol/protocol-core/test/foundry/
@solady/=node_modules/solady/
```

Now we are ready to build a simple test registration contract.

# Register a Derivative
<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/story-protocol-boilerplate/blob/main/test/4_IPARemix.t.sol" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    All of this page is covered in this working code example.
  </Card>
</Cards>

Once a [License Token](doc:license-token) has been minted from an IP Asset, the owner of that token (an ERC-721 NFT) can burn it to register their own IP Asset as a derivative of the IP Asset associated with the License Token.

### :warning: Prerequisites

There are a few steps you have to complete before you can start the tutorial.

1. Complete the [Setup Your Own Project](doc:sc-setup)
2. Have a minted License Token. You can learn how to do that [here](doc:sc-mint-license-token)

## 1. Register as Derivative

Let's create a test file under `test/4_IPARemix.t.sol` to see it work and verify the results:

> 📘 Contract Addresses
>
> We have filled in the addresses from the Story contracts for you. However you can also find the addresses for them here: [Deployed Smart Contracts](doc:deployed-smart-contracts)

```sol test/4_IPARemix.t.sol
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.26;

import { Test } from "forge-std/Test.sol";
// for testing purposes only
import { MockIPGraph } from "@storyprotocol/test/mocks/MockIPGraph.sol";
import { IIPAssetRegistry } from "@storyprotocol/core/interfaces/registries/IIPAssetRegistry.sol";
import { ILicenseRegistry } from "@storyprotocol/core/interfaces/registries/ILicenseRegistry.sol";
import { IPILicenseTemplate } from "@storyprotocol/core/interfaces/modules/licensing/IPILicenseTemplate.sol";
import { ILicensingModule } from "@storyprotocol/core/interfaces/modules/licensing/ILicensingModule.sol";
import { PILFlavors } from "@storyprotocol/core/lib/PILFlavors.sol";
import { PILTerms } from "@storyprotocol/core/interfaces/modules/licensing/IPILicenseTemplate.sol";

import { SimpleNFT } from "../src/mocks/SimpleNFT.sol";

// Run this test:
// forge test --fork-url https://aeneid.storyrpc.io/ --match-path test/4_IPARemix.t.sol
contract IPARemixTest is Test {
    address internal alice = address(0xa11ce);
    address internal bob = address(0xb0b);

    // For addresses, see https://docs.story.foundation/docs/deployed-smart-contracts
    // Protocol Core - IPAssetRegistry
    IIPAssetRegistry internal IP_ASSET_REGISTRY = IIPAssetRegistry(0x77319B4031e6eF1250907aa00018B8B1c67a244b);
    // Protocol Core - LicenseRegistry
    ILicenseRegistry internal LICENSE_REGISTRY = ILicenseRegistry(0x529a750E02d8E2f15649c13D69a465286a780e24);
    // Protocol Core - LicensingModule
    ILicensingModule internal LICENSING_MODULE = ILicensingModule(0x04fbd8a2e56dd85CFD5500A4A4DfA955B9f1dE6f);
    // Protocol Core - PILicenseTemplate
    IPILicenseTemplate internal PIL_TEMPLATE = IPILicenseTemplate(0x2E896b0b2Fdb7457499B56AAaA4AE55BCB4Cd316);
    // Protocol Core - RoyaltyPolicyLAP
    address internal ROYALTY_POLICY_LAP = 0xBe54FB168b3c982b7AaE60dB6CF75Bd8447b390E;
    // Revenue Token - MERC20
    address internal MERC20 = 0xF2104833d386a2734a4eB3B8ad6FC6812F29E38E;

    SimpleNFT public SIMPLE_NFT;
    uint256 public tokenId;
    address public ipId;
    uint256 public licenseTermsId;
    uint256 public startLicenseTokenId;

    function setUp() public {
        // this is only for testing purposes
        // due to our IPGraph precompile not being
        // deployed on the fork
        vm.etch(address(0x0101), address(new MockIPGraph()).code);

        SIMPLE_NFT = new SimpleNFT("Simple IP NFT", "SIM");
        tokenId = SIMPLE_NFT.mint(alice);
        ipId = IP_ASSET_REGISTRY.register(block.chainid, address(SIMPLE_NFT), tokenId);

        licenseTermsId = PIL_TEMPLATE.registerLicenseTerms(
            PILFlavors.commercialRemix({
                mintingFee: 0,
                commercialRevShare: 10 * 10 ** 6, // 10%
                royaltyPolicy: ROYALTY_POLICY_LAP,
                currencyToken: MERC20
            })
        );

        vm.prank(alice);
        LICENSING_MODULE.attachLicenseTerms(ipId, address(PIL_TEMPLATE), licenseTermsId);
        startLicenseTokenId = LICENSING_MODULE.mintLicenseTokens({
            licensorIpId: ipId,
            licenseTemplate: address(PIL_TEMPLATE),
            licenseTermsId: licenseTermsId,
            amount: 2,
            receiver: bob,
            royaltyContext: "", // for PIL, royaltyContext is empty string
            maxMintingFee: 0,
            maxRevenueShare: 0
        });
    }

    /// @notice Mints an NFT to be registered as IP, and then
    /// linked as a derivative of alice's asset using the
    /// minted license token.
    function test_registerDerivativeWithLicenseTokens() public {
        // First we mint an NFT and register it as an IP Asset,
        // owned by Bob.
        uint256 childTokenId = SIMPLE_NFT.mint(bob);
        address childIpId = IP_ASSET_REGISTRY.register(block.chainid, address(SIMPLE_NFT), childTokenId);

        uint256[] memory licenseTokenIds = new uint256[](1);
        licenseTokenIds[0] = startLicenseTokenId;

        // Bob uses the License Token he has from Alice's IP
        // to register his IP as a derivative of Alice's IP.
        vm.prank(bob);
        LICENSING_MODULE.registerDerivativeWithLicenseTokens({
            childIpId: childIpId,
            licenseTokenIds: licenseTokenIds,
            royaltyContext: "", // empty for PIL
            maxRts: 0
        });

        assertTrue(LICENSE_REGISTRY.hasDerivativeIps(ipId));
        assertTrue(LICENSE_REGISTRY.isParentIp(ipId, childIpId));
        assertTrue(LICENSE_REGISTRY.isDerivativeIp(childIpId));
        assertEq(LICENSE_REGISTRY.getParentIpCount(childIpId), 1);
        assertEq(LICENSE_REGISTRY.getDerivativeIpCount(ipId), 1);
        assertEq(LICENSE_REGISTRY.getParentIp({ childIpId: childIpId, index: 0 }), ipId);
        assertEq(LICENSE_REGISTRY.getDerivativeIp({ parentIpId: ipId, index: 0 }), childIpId);
    }
}
```

## 2. Test Your Code!

Run `forge build`. If everything is successful, the command should successfully compile.

Now run the test by executing the following command:

```shell
forge test --fork-url https://aeneid.storyrpc.io/ --match-path test/4_IPARemix.t.sol
```

## 3. Paying and Claiming Revenue

Congratulations, you registered a derivative IP Asset!

<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/story-protocol-boilerplate/blob/main/test/4_IPARemix.t.sol" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    All of this page is covered in this working code example.
  </Card>
</Cards>

Now that we have established parent-child IP relationships, we can begin to explore payments and automated revenue share based on the license terms. We'll cover that in the upcoming pages.

# Deployed Smart Contracts
## Core Protocol Contracts

* View contracts on our GitHub [here](https://github.com/storyprotocol/protocol-core-v1/tree/main)

```json Aeneid Testnet
{
  "AccessController": "0xcCF37d0a503Ee1D4C11208672e622ed3DFB2275a",
  "ArbitrationPolicyUMA": "0xfFD98c3877B8789124f02C7E8239A4b0Ef11E936",
  "CoreMetadataModule": "0x6E81a25C99C6e8430aeC7353325EB138aFE5DC16",
  "CoreMetadataViewModule": "0xB3F88038A983CeA5753E11D144228Ebb5eACdE20",
  "DisputeModule": "0x9b7A9c70AFF961C799110954fc06F3093aeb94C5",
  "EvenSplitGroupPool": "0xf96f2c30b41Cb6e0290de43C8528ae83d4f33F89",
  "GroupNFT": "0x4709798FeA84C84ae2475fF0c25344115eE1529f",
  "GroupingModule": "0x69D3a7aa9edb72Bc226E745A7cCdd50D947b69Ac",
  "IPAccountImplBeacon": "0x9825cc7A398D9C3dDD66232A8Ec76d5b05422581",
  "IPAccountImplBeaconProxy": "0x00b800138e4D82D1eea48b414d2a2A8Aee9A33b1",
  "IPAccountImplCode": "0x6ccAd5718a27fB6a09EAdb737D889A2007838b77",
  "IPAssetRegistry": "0x77319B4031e6eF1250907aa00018B8B1c67a244b",
  "IPGraphACL": "0x1640A22a8A086747cD377b73954545e2Dfcc9Cad",
  "IpRoyaltyVaultBeacon": "0x6928ba25Aa5c410dd855dFE7e95713d83e402AA6",
  "IpRoyaltyVaultImpl": "0x73e2D097F71e5103824abB6562362106A8955AEc",
  "LicenseRegistry": "0x529a750E02d8E2f15649c13D69a465286a780e24",
  "LicenseToken": "0xFe3838BFb30B34170F00030B52eA4893d8aAC6bC",
  "LicensingModule": "0x04fbd8a2e56dd85CFD5500A4A4DfA955B9f1dE6f",
  "ModuleRegistry": "0x022DBAAeA5D8fB31a0Ad793335e39Ced5D631fa5",
  "PILicenseTemplate": "0x2E896b0b2Fdb7457499B56AAaA4AE55BCB4Cd316",
  "ProtocolAccessManager": "0xFdece7b8a2f55ceC33b53fd28936B4B1e3153d53",
  "ProtocolPauseAdmin": "0xdd661f55128A80437A0c0BDA6E13F214A3B2EB24",
  "RoyaltyModule": "0xD2f60c40fEbccf6311f8B47c4f2Ec6b040400086",
  "RoyaltyPolicyLAP": "0xBe54FB168b3c982b7AaE60dB6CF75Bd8447b390E",
  "RoyaltyPolicyLRP": "0x9156e603C949481883B1d3355c6f1132D191fC41"
}
```
```json Mainnet
{
  "AccessController": "0xcCF37d0a503Ee1D4C11208672e622ed3DFB2275a",
  "ArbitrationPolicyUMA": "0xfFD98c3877B8789124f02C7E8239A4b0Ef11E936",
  "CoreMetadataModule": "0x6E81a25C99C6e8430aeC7353325EB138aFE5DC16",
  "CoreMetadataViewModule": "0xB3F88038A983CeA5753E11D144228Ebb5eACdE20",
  "DisputeModule": "0x9b7A9c70AFF961C799110954fc06F3093aeb94C5",
  "EvenSplitGroupPool": "0xf96f2c30b41Cb6e0290de43C8528ae83d4f33F89",
  "GroupNFT": "0x4709798FeA84C84ae2475fF0c25344115eE1529f",
  "GroupingModule": "0x69D3a7aa9edb72Bc226E745A7cCdd50D947b69Ac",
  "IPAccountImplBeacon": "0x9825cc7A398D9C3dDD66232A8Ec76d5b05422581",
  "IPAccountImplBeaconProxy": "0x00b800138e4D82D1eea48b414d2a2A8Aee9A33b1",
  "IPAccountImplCode": "0x7343646585443F1c3F64E4F08b708788527e1C77",
  "IPAssetRegistry": "0x77319B4031e6eF1250907aa00018B8B1c67a244b",
  "IPGraphACL": "0x1640A22a8A086747cD377b73954545e2Dfcc9Cad",
  "IpRoyaltyVaultBeacon": "0x6928ba25Aa5c410dd855dFE7e95713d83e402AA6",
  "IpRoyaltyVaultImpl": "0x63cC7611316880213f3A4Ba9bD72b0EaA2010298",
  "LicenseRegistry": "0x529a750E02d8E2f15649c13D69a465286a780e24",
  "LicenseToken": "0xFe3838BFb30B34170F00030B52eA4893d8aAC6bC",
  "LicensingModule": "0x04fbd8a2e56dd85CFD5500A4A4DfA955B9f1dE6f",
  "ModuleRegistry": "0x022DBAAeA5D8fB31a0Ad793335e39Ced5D631fa5",
  "PILicenseTemplate": "0x2E896b0b2Fdb7457499B56AAaA4AE55BCB4Cd316",
  "ProtocolAccessManager": "0xFdece7b8a2f55ceC33b53fd28936B4B1e3153d53",
  "ProtocolPauseAdmin": "0xdd661f55128A80437A0c0BDA6E13F214A3B2EB24",
  "RoyaltyModule": "0xD2f60c40fEbccf6311f8B47c4f2Ec6b040400086",
  "RoyaltyPolicyLAP": "0xBe54FB168b3c982b7AaE60dB6CF75Bd8447b390E",
  "RoyaltyPolicyLRP": "0x9156e603C949481883B1d3355c6f1132D191fC41"
}
```

## Periphery Contracts

* View contracts on our GitHub [here](https://github.com/storyprotocol/protocol-periphery-v1)

```json Aeneid Testnet
{
  "DerivativeWorkflows": "0x9e2d496f72C547C2C535B167e06ED8729B374a4f",
  "GroupingWorkflows": "0xD7c0beb3aa4DCD4723465f1ecAd045676c24CDCd",
  "LicenseAttachmentWorkflows": "0xcC2E862bCee5B6036Db0de6E06Ae87e524a79fd8",
  "LockLicenseHook": "0x5D874d4813c4A8A9FB2AB55F30cED9720AEC0222",
  "OwnableERC20Beacon": "0x9a81C447C0b4C47d41d94177AEea3511965d3Bc9",
  "OwnableERC20Template": "0x37DbEbcFe991901C4F255E7a3C4F7D3B45EAEDe9",
  "RegistrationWorkflows": "0xbe39E1C756e921BD25DF86e7AAa31106d1eb0424",
  "RoyaltyTokenDistributionWorkflows": "0xa38f42B8d33809917f23997B8423054aAB97322C",
  "RoyaltyWorkflows": "0x9515faE61E0c0447C6AC6dEe5628A2097aFE1890",
  "SPGNFTBeacon": "0xD2926B9ecaE85fF59B6FB0ff02f568a680c01218",
  "SPGNFTImpl": "0xc09e3788Fdfbd3dd8CDaa2aa481B52CcFAb74a42",
  "TokenizerModule": "0x7e14cD9833E8A0c649bCF4AB6768f62D57febbd3",
  "TotalLicenseTokenLimitHook": "0xba8E30d9EB784Badc2aF610F56d99d212BC2A90c"
}
```
```json Mainnet
{
  "DerivativeWorkflows": "0x9e2d496f72C547C2C535B167e06ED8729B374a4f",
  "GroupingWorkflows": "0xD7c0beb3aa4DCD4723465f1ecAd045676c24CDCd",
  "LicenseAttachmentWorkflows": "0xcC2E862bCee5B6036Db0de6E06Ae87e524a79fd8",
  "LockLicenseHook": "0x5D874d4813c4A8A9FB2AB55F30cED9720AEC0222",
  "OwnableERC20Beacon": "0x9a81C447C0b4C47d41d94177AEea3511965d3Bc9",
  "OwnableERC20Template": "0xE6505ffc5A7C19B68cEc2311Cc35BC02d8f7e0B1",
  "RegistrationWorkflows": "0xbe39E1C756e921BD25DF86e7AAa31106d1eb0424",
  "RoyaltyTokenDistributionWorkflows": "0xa38f42B8d33809917f23997B8423054aAB97322C",
  "RoyaltyWorkflows": "0x9515faE61E0c0447C6AC6dEe5628A2097aFE1890",
  "SPGNFTBeacon": "0xD2926B9ecaE85fF59B6FB0ff02f568a680c01218",
  "SPGNFTImpl": "0x6Cfa03Bc64B1a76206d0Ea10baDed31D520449F5",
  "TokenizerModule": "0xAC937CeEf893986A026f701580144D9289adAC4C",
  "TotalLicenseTokenLimitHook": "0xB72C9812114a0Fc74D49e01385bd266A75960Cda"
}
```

## Whitelisted Revenue Tokens

<Tabs>
  <Tab title="Aeneid Testnet">
    | Token  | Contract Address                             | Explorer                                                                                                                   | Mint                                                                                                                                                |
    | :----- | :------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------- |
    | WIP    | `0x1514000000000000000000000000000000000000` | <a href="https://aeneid.storyscan.xyz/address/0x1514000000000000000000000000000000000000" target="_blank">View here ↗️</a> | N/A                                                                                                                                                 |
    | MERC20 | `0xF2104833d386a2734a4eB3B8ad6FC6812F29E38E` | <a href="https://aeneid.storyscan.xyz/address/0xF2104833d386a2734a4eB3B8ad6FC6812F29E38E" target="_blank">View here ↗️</a> | <a href="https://aeneid.storyscan.xyz/address/0xF2104833d386a2734a4eB3B8ad6FC6812F29E38E?tab=write_contract#0x40c10f19" target="_blank">Mint ↗️</a> |
  </Tab>

  <Tab title="Mainnet">
    | Token | Contract Address                             | Explorer                                                                                                                   | Mint |
    | :---- | :------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------- | :--- |
    | WIP   | `0x1514000000000000000000000000000000000000` | <a href="https://aeneid.storyscan.xyz/address/0x1514000000000000000000000000000000000000" target="_blank">View here ↗️</a> | N/A  |
  </Tab>
</Tabs>

## Misc

* **Multicall3**: 0xcA11bde05977b3631167028862bE2a173976CA11
* **Default License Terms ID** (Non-Commercial Social Remixing): 1

# Pay an IPA
<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/registerDerivativeCommercialSpg.ts" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    All of this page is covered in this working code example.
  </Card>
</Cards>

This section demonstrates how to pay an IP Asset. There are a few reasons you would do this:

1. you simply want to "tip" an IP
2. you have to because your license terms with an ancestor IP require you to forward a certain % of payment

In either scenario, you would use the below `payRoyaltyOnBehalf` function. When this happens, the [💸 Royalty Module](doc:royalty-module) automatically handles the different payment flows such that parent IP Assets who have negotiated a certain `commercialRevShare` with the IPA being paid can claim their due share.

### :warning: Prerequisites

There are a few steps you have to complete before you can start the tutorial.

1. Complete the [TypeScript SDK Setup](doc:typescript-sdk-setup)
2. Have a basic understanding of the [💸 Royalty Module](doc:royalty-module)

## Before We Start

You can pay an IP Asset using the `payRoyaltyOnBehalf` function.

You will be paying the IP Asset with [$WIP](https://aeneid.storyscan.xyz/address/0x1514000000000000000000000000000000000000). **Note that if you don't have enough $WIP, the function will auto wrap an equivalent amount of $IP into $WIP for you.** If you don't have enough of either, it will fail.

To help with the following scenarios, let's say we have a parent IP Asset that has negotiated a 50% `commercialRevShare` with its child IP Asset.

### :coin: Whitelisted Revenue Tokens

Only tokens that are whitelisted by our protocol can be used as payment ("revenue") tokens. $WIP is one of those tokens. To see that list, go [here](https://docs.story.foundation/docs/deployed-smart-contracts#whitelisted-revenue-tokens).

> ❗️ Testing: MockERC20
>
> If you want to test paying IP Assets, you'll probably want a whitelisted revenue token you can mint freely for testing. We have provided [MockERC20](https://aeneid.storyscan.xyz/address/0xF2104833d386a2734a4eB3B8ad6FC6812F29E38E?tab=write_contract#0x40c10f19) on Aeneid testnet which you can mint and pay with.
>
> Note, however, that in order for the SDK to be able to spend MockERC20 for you, you must [approve it](https://aeneid.storyscan.xyz/address/0xF2104833d386a2734a4eB3B8ad6FC6812F29E38E?tab=write_contract#0x095ea7b3) where the `spender` is `0xD2f60c40fEbccf6311f8B47c4f2Ec6b040400086` (the address of `RoyaltyModule.sol` on Aeneid testnet) and the value is ≥ the amount you want to pay.

## Scenario #1: Tipping an IP Asset

In this scenario, you're an external 3rd-party user who wants to pay an IP Asset 2 $WIP for being cool. When you call the function below, you should make `payerIpId` a zero address because you are not paying on behalf of an IP Asset. Additionally, you would set `amount` to 2.

> Associated Docs: [royalty.payRoyaltyOnBehalf](https://docs.story.foundation/docs/sdk-royalty#payroyaltyonbehalf)

```typescript main.ts
import { WIP_TOKEN_ADDRESS } from '@story-protocol/core-sdk'
// you should already have a client set up (prerequisite)
import { client } from './utils'

async function main() {
  const payRoyalty = await client.royalty.payRoyaltyOnBehalf({
    receiverIpId: "0x0b825D9E5FA196e6B563C0a446e8D9885057f9B1", // the ip you're paying
    payerIpId: zeroAddress,
    token: WIP_TOKEN_ADDRESS,
    amount: 2,
    txOptions: { waitForTransaction: true },
  });

  console.log(`Paid royalty at transaction hash ${payRoyalty.txHash}`);
}

main();
```

Let's say the IP Asset you're paying is a derivative. And due to existing license terms with a parent that specify 50% `commercialRevShare`, 50% of the revenue (2\*0.5 = 1) would automatically be claimable by the parent thanks to the [💸 Royalty Module](doc:royalty-module), such that both the parent and child IP Assets earn 1 $WIP. We'll go over this on the next page.

## Scenario #2: Paying Due Share

In this scenario, lets say a derivative IP Asset earned 2 USD off-chain. Because the derivative owes the parent IP Asset 50% of its revenue, it could give the parent 1 USD off-chain and be ok. Or, it can send 1 $USD equivalent to the parent on-chain *(for this example, let's just assume 1 $WIP = 1 USD)*.

> Associated Docs: [royalty.payRoyaltyOnBehalf](https://docs.story.foundation/docs/sdk-royalty#payroyaltyonbehalf)

```typescript main.ts
import { WIP_TOKEN_ADDRESS } from '@story-protocol/core-sdk'
// you should already have a client set up (prerequisite)
import { client } from './utils'

async function main() {
  const payRoyalty = await client.royalty.payRoyaltyOnBehalf({
    receiverIpId: "0xDa03c4B278AD44f5a669e9b73580F91AeDE0E3B2", // parentIpId
    payerIpId: "0x0b825D9E5FA196e6B563C0a446e8D9885057f9B1", // childIpId
    token: WIP_TOKEN_ADDRESS,
    amount: 1,
    txOptions: { waitForTransaction: true },
  });

  console.log(`Paid royalty at transaction hash ${payRoyalty.txHash}`);
}

main();
```

### :globe_with_meridians: Complex Royalty Graphs

Let's say the child earned 1,000 USD off-chain, and is linked to a huge ancestor tree where each parent has a different set of complex license terms. In this scenario, you won't be able to individually calculate each payment to each parent. Instead, you would just pay *yourself* the amount you earned, and the [💸 Royalty Module](doc:royalty-module) will automate the payment, such that each ancestor gets their due share.

## Claiming Revenue

Congratulations, you paid an IP Asset on-chain!

<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/registerDerivativeCommercialSpg.ts" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    All of this page is covered in this working code example.
  </Card>
</Cards>

Now that we have paid revenue, we need to learn how to claim it! We will cover that on the next page.

# Attach Terms to an IPA
This section demonstrates how to attach [License Terms](doc:license-terms) to an [🧩 IP Asset](doc:ip-asset). By attaching terms, users can publicly mint [License Tokens](doc:license-token) (the on-chain "license") with those terms from the IP.

### :warning: Prerequisites

There are a few steps you have to complete before you can start the tutorial.

1. Complete the [TypeScript SDK Setup](doc:typescript-sdk-setup)

## 0. Before We Start

We should mention that you do not need an existing IP Asset to attach terms to it. There are two functions you can use that allow you to **register IP + create terms + attach terms** in the same function:

* [mintAndRegisterIpAssetWithPilTerms](https://docs.story.foundation/docs/sdk-ipasset#mintandregisteripassetwithpilterms)
* [registerIpAndAttachPilTerms](https://docs.story.foundation/docs/sdk-ipasset#registeripandattachpilterms)

## 1. Register License Terms

In order to attach terms to an IP Asset, let's first create them!

[License Terms](doc:license-terms) are a configurable set of values that define restrictions on licenses minted from your IP that have those terms. For example, "If you mint this license, you must share 50% of your revenue with me." You can view the full set of terms in [PIL Terms](doc:pil-terms).

> 📘 Duplicate Terms
>
> If License Terms already exist on our protocol for the identical set of parameters you intend to create, it is unnecessary to create it again and the function will simply return the existing `licenseTermsId` and an undefined `txHash`. License Terms are protocol-wide, so you can use existing License Terms by its `licenseTermsId`.

Below is a code example showing how to create new terms:

> Associated Docs: [license.registerPILTerms](https://docs.story.foundation/docs/sdk-license#registerpilterms)

```typescript main.ts
import { LicenseTerms } from '@story-protocol/core-sdk';
import { zeroAddress } from 'viem';
// you should already have a client set up (prerequisite)
import { client } from './utils';

async function main() {
  const licenseTerms: LicenseTerms = {
    defaultMintingFee: 0n,
    // must be a whitelisted revenue token from https://docs.story.foundation/docs/deployed-smart-contracts
    currency: '0x1514000000000000000000000000000000000000',
    // insert RoyaltyPolicyLAP address from https://docs.story.foundation/docs/deployed-smart-contracts
    royaltyPolicy: '0xBe54FB168b3c982b7AaE60dB6CF75Bd8447b390E',
    transferable: false,
    expiration: 0n,
    commercialUse: false,
    commercialAttribution: false,
    commercializerChecker: zeroAddress,
    commercializerCheckerData: '0x',
    commercialRevShare: 0,
    commercialRevCeiling: 0n,
    derivativesAllowed: false,
    derivativesAttribution: false,
    derivativesApproval: false,
    derivativesReciprocal: false,
    derivativeRevCeiling: 0n,
    uri: '',
  }

  const response = await client.license.registerPILTerms({ 
    ...licenseTerms, 
    txOptions: { waitForTransaction: true } 
  })

  console.log(`PIL Terms registered at transaction hash ${response.txHash}, License Terms ID: ${response.licenseTermsId}`)  
}

main();
```

### 1a. :icecream: PIL Flavors

As you see above, you have to choose between a lot of terms.

We have convenience functions to help you register new terms. We have created [PIL Flavors](doc:pil-flavors), which are pre-configured popular combinations of License Terms to help you decide what terms to use. You can view those PIL Flavors and then register terms using the following convenience functions:

<Cards columns={4}>
  <Card title="Non-Commercial Social Remixing" href="https://docs.story.foundation/docs/pil-flavors#flavor-1-non-commercial-social-remixing" icon="fa-file" target="_blank">
    Free remixing with attribution. No commercialization.
  </Card>

  <Card title="Commercial Use" href="https://docs.story.foundation/docs/pil-flavors#flavor-2-commercial-use" icon="fa-file" target="_blank">
    Pay to use the license with attribution, but don't have to share revenue.
  </Card>

  <Card title="Commercial Remix" href="https://docs.story.foundation/docs/pil-flavors#flavor-3-commercial-remix" icon="fa-file" target="_blank">
    Pay to use the license with attribution and pay % of revenue earned.
  </Card>

  <Card title="Creative Commons Attribution" href="https://docs.story.foundation/docs/pil-flavors#flavor-4-creative-commons-attribution" icon="fa-file" target="_blank">
    Free remixing and commercial use with attribution.
  </Card>
</Cards>

## 2. Attach License Terms

Now that we have created terms and have the associated `licenseTermsId`, we can attach them to an existing IP Asset like so:

> Associated Docs: [license.attachLicenseTerms](https://docs.story.foundation/docs/sdk-license#attachlicenseterms)

```typescript TypeScript
import { LicenseTerms } from '@story-protocol/core-sdk';
import { zeroAddress } from 'viem';
// you should already have a client set up (prerequisite)
import { client } from './utils';

async function main() {
  // previous code here ...
  
  const response = await client.license.attachLicenseTerms({
    // insert your newly created license terms id here
    licenseTermsId: LICENSE_TERMS_ID, 
    // insert the ipId you want to attach terms to here
    ipId: "0x4c1f8c1035a8cE379dd4ed666758Fb29696CF721",
    txOptions: { waitForTransaction: true }
  });

  if (response.success) {
    console.log(`Attached License Terms to IPA at transaction hash ${response.txHash}.`)
  } else {
    console.log(`License Terms already attached to this IPA.`)
  }
}

main();
```

### 2a. Create Terms + Attach

:point_right: It's worth mentioning that you can **create terms + attach terms** all in the same step with the the [registerPilTermsAndAttach](https://docs.story.foundation/docs/sdk-ipasset#registerpiltermsandattach) function. Whatever is easiest for you!

:point_right: And, like we mentioned at the beginning, there are two functions you can use that allow you to **register IP + create terms + attach terms** in the same function:

* [mintAndRegisterIpAssetWithPilTerms](https://docs.story.foundation/docs/sdk-ipasset#mintandregisteripassetwithpilterms)
* [registerIpAndAttachPilTerms](https://docs.story.foundation/docs/sdk-ipasset#registeripandattachpilterms)

## 3. Mint a License

Now that we have attached License Terms to our IP, the next step is minting a License Token, which we'll go over on the next page.

# Register License Terms
This section demonstrates how to register a selection of License Terms using the PIL.

## Prerequisites

* [Setup](doc:typescript-sdk-setup) the client object.
* If License Terms already exist for the identical set of parameters you intend to create, it is unnecessary to create it again and the function will simply return the existing `licenseTermsId` and an undefined `txHash`. You can use existing License Terms by its `licenseTermsId`.

> 🪙 Whitelisted Revenue Tokens
>
> At the moment, a token must be whitelisted in our protocol to be used in the royalty module as the `currency` field.
>
> Please see the [currently whitelisted revenue tokens](https://docs.story.foundation/docs/royalty-module#whitelisted-revenue-tokens), where you can also mint tokens directly to be used in the below commercial sections.

# Create New License Terms

Create your own [PIL Terms](doc:pil-terms) that you can later attach to an IP Asset.

```typescript TypeScript
import { LicenseTerms } from '@story-protocol/core-sdk';
import { zeroAddress } from 'viem';

const licenseTerms: LicenseTerms = {
  defaultMintingFee: 0n,
  currency: '0x1514000000000000000000000000000000000000', // insert $WIP address from https://docs.story.foundation/docs/deployed-smart-contracts
  royaltyPolicy: RoyaltyPolicyLAP, // insert RoyaltyPolicyLAP address from https://docs.story.foundation/docs/deployed-smart-contracts
  transferable: false,
  expiration: 0n,
  commercialUse: false,
  commercialAttribution: false,
  commercializerChecker: zeroAddress,
  commercializerCheckerData: '0x',
  commercialRevShare: 0,
  commercialRevCeiling: 0n,
  derivativesAllowed: false,
  derivativesAttribution: false,
  derivativesApproval: false,
  derivativesReciprocal: false,
  derivativeRevCeiling: 0n,
  uri: '',
}

const response = await client.license.registerPILTerms({ 
  ...licenseTerms, 
  txOptions: { waitForTransaction: true } 
})

console.log(`PIL Terms registered at transaction hash ${response.txHash}, License Terms ID: ${response.licenseTermsId}`) 
```
```typescript Request Type
export type RegisterPILTermsRequest = Omit<
  LicenseTerms,
  "defaultMintingFee" | "expiration" | "commercialRevCeiling" | "derivativeRevCeiling"
> & {
  defaultMintingFee: bigint | string | number;
  expiration: bigint | string | number;
  commercialRevCeiling: bigint | string | number;
  derivativeRevCeiling: bigint | string | number;
  txOptions?: TxOptions;
};

export type LicenseTerms = {
  /*Indicates whether the license is transferable or not.*/
  transferable: boolean;
  /*The address of the royalty policy contract which required to StoryProtocol in advance.*/
  royaltyPolicy: Address;
  /*The default minting fee to be paid when minting a license.*/
  defaultMintingFee: bigint;
  /*The expiration period of the license.*/
  expiration: bigint;
  /*Indicates whether the work can be used commercially or not.*/
  commercialUse: boolean;
  /*Whether attribution is required when reproducing the work commercially or not.*/
  commercialAttribution: boolean;
  /*Commercializers that are allowed to commercially exploit the work. If zero address, then no restrictions is enforced.*/
  commercializerChecker: Address;
  /*The data to be passed to the commercializer checker contract.*/
  commercializerCheckerData: Address;
  /*Percentage of revenue that must be shared with the licensor.*/
  commercialRevShare: number;
  /*The maximum revenue that can be generated from the commercial use of the work.*/
  commercialRevCeiling: bigint;
  /*Indicates whether the licensee can create derivatives of his work or not.*/
  derivativesAllowed: boolean;
  /*Indicates whether attribution is required for derivatives of the work or not.*/
  derivativesAttribution: boolean;
  /*Indicates whether the licensor must approve derivatives of the work before they can be linked to the licensor IP ID or not.*/
  derivativesApproval: boolean;
  /*Indicates whether the licensee must license derivatives of the work under the same terms or not.*/
  derivativesReciprocal: boolean;
  /*The maximum revenue that can be generated from the derivative use of the work.*/
  derivativeRevCeiling: bigint;
  /*The ERC20 token to be used to pay the minting fee. the token must be registered in story protocol.*/
  currency: Address;
  /*The URI of the license terms, which can be used to fetch the offchain license terms.*/
  uri: string;
};
```
```typescript Response Type
export type RegisterPILResponse = {
  licenseTermsId?: bigint;
  txHash?: string;
  encodedTxData?: EncodedTxData;
};
```

# Create a Commercial Use License

* Click [here](https://docs.story.foundation/docs/pil-flavors#flavor-1-non-commercial-social-remixing) to see info on Commercial Use licenses.

```typescript TypeScript
const commercialUseParams = {
  currency: '0x1514000000000000000000000000000000000000', // insert $WIP address from https://docs.story.foundation/docs/deployed-smart-contracts
  defaultMintingFee: '10' // 10 of the currency (using the above currency, 10 $WIP)
}

const response = await client.license.registerCommercialUsePIL({
  ...commercialUseParams,
  txOptions: { waitForTransaction: true }
});

console.log(`PIL Terms registered at transaction hash ${response.txHash}, License Terms ID: ${response.licenseTermsId}`) 
```
```typescript Request Type
export type RegisterCommercialUsePILRequest = {
  defaultMintingFee: string | number | bigint;
  currency: Address;
  royaltyPolicyAddress?: Address;
  txOptions?: TxOptions;
};
```
```typescript Response Type
export type RegisterPILResponse = {
  licenseTermsId?: bigint;
  txHash?: string;
  encodedTxData?: EncodedTxData;
};
```

Setting `waitForTransaction: true` in the transaction options will return the `licenseTermsId` value.

# Create a Commercial Remix License

* Click [here](https://docs.story.foundation/docs/pil-flavors#flavor-1-non-commercial-social-remixing) to see info on Commercial Remix licenses.

```typescript TypeScript
const commercialRemixParams = {
  currency: '0x1514000000000000000000000000000000000000', // insert $WIP address from https://docs.story.foundation/docs/deployed-smart-contracts
  defaultMintingFee: '10', // 10 of the currency (using the above currency, 10 $WIP)
  commercialRevShare: 10 // 10%
}

const response = await client.license.registerCommercialRemixPIL({
  ...commercialRemixParams,
  txOptions: { waitForTransaction: true }
});

console.log(`PIL Terms registered at transaction hash ${response.txHash}, License Terms ID: ${response.licenseTermsId}`) 
```
```typescript Request Type
export type RegisterCommercialRemixPILRequest = {
  defaultMintingFee: string | number | bigint;
  commercialRevShare: number;
  currency: Address;
  royaltyPolicyAddress?: Address;
  txOptions?: TxOptions;
};
```
```typescript Response Type
export type RegisterPILResponse = {
  licenseTermsId?: bigint;
  txHash?: string;
  encodedTxData?: EncodedTxData;
};
```

Setting `waitForTransaction: true` in the transaction options will return the `licenseTermsId` value.

# Create a Non-Commercial Social Remixing License

* Click [here](https://docs.story.foundation/docs/pil-flavors#flavor-1-non-commercial-social-remixing) to see info on Non-Commercial Social Remixing licenses.

There are currently no parameters to be passed in here, so this function should not really be used other than to get back the `licenseTermsId` for this license.

```typescript TypeScript
const nonComSocialRemixingParams = {}

const response = await client.license.registerNonComSocialRemixingPIL({
  ...nonComSocialRemixingParams,
  txOptions: { waitForTransaction: true }
});

console.log(`PIL Terms registered at transaction hash ${response.txHash}, License Terms ID: ${response.licenseTermsId}`) 
```
```typescript Request Type
export type RegisterNonComSocialRemixingPILRequest = {
  txOptions?: TxOptions;
};
```
```typescript Response Type
export type RegisterPILResponse = {
  licenseTermsId?: bigint;
  txHash?: string;
  encodedTxData?: EncodedTxData;
};
```

Setting `waitForTransaction: true` in the transaction options will return the `licenseTermsId` value.

# Reown (WalletConnect) Setup

> 📘 Optional: Official WalletConnect Docs
>
> Check out the official Wagmi + Reown installation docs [here](https://docs.walletconnect.com/appkit/next/core/installation).

## Install the Dependencies

```shell npm
npm install --save @story-protocol/core-sdk @reown/appkit @reown/appkit-adapter-wagmi wagmi viem @tanstack/react-query
```

```shell pnpm
pnpm install @story-protocol/core-sdk viem
```

```shell yarn
yarn add @story-protocol/core-sdk viem
```

## Setup

Before diving into the example, make sure you have two things setup:

1. Make sure to have `NEXT_PUBLIC_RPC_PROVIDER_URL` set up in your `.env` file.
   1. You can use the public default one (`https://aeneid.storyrpc.io`) or any other RPC [here](https://docs.story.foundation/docs/aeneid#-rpcs).
2. Make sure to have `NEXT_PUBLIC_WALLET_CONNECT_PROJECT_ID` set up in your `.env` file. Do this by logging into [Reown (prev. WalletConnect)](https://reown.com/) and creating a project.

```jsx config/index.tsx
import { cookieStorage, createStorage, http } from "@wagmi/core";
import { WagmiAdapter } from "@reown/appkit-adapter-wagmi";
import { mainnet, arbitrum } from "@reown/appkit/networks";
import { aeneid } from "@story-protocol/core-sdk";

// Get projectId from https://cloud.reown.com
export const projectId = process.env.NEXT_PUBLIC_PROJECT_ID;

if (!projectId) {
  throw new Error("Project ID is not defined");
}

export const networks = [aeneid];

//Set up the Wagmi Adapter (Config)
export const wagmiAdapter = new WagmiAdapter({
  storage: createStorage({
    storage: cookieStorage,
  }),
  ssr: true,
  projectId,
  networks,
});

export const config = wagmiAdapter.wagmiConfig;
```

```jsx context/index.tsx
'use client'

import { wagmiAdapter, projectId } from '@/config'
import { QueryClient, QueryClientProvider } from '@tanstack/react-query'
import { createAppKit } from '@reown/appkit/react'
import { mainnet, arbitrum } from '@reown/appkit/networks'
import React, { type ReactNode } from 'react'
import { cookieToInitialState, WagmiProvider, type Config } from 'wagmi'

// Set up queryClient
const queryClient = new QueryClient()

if (!projectId) {
  throw new Error('Project ID is not defined')
}

// Set up metadata
const metadata = {
  name: 'appkit-example',
  description: 'AppKit Example',
  url: 'https://appkitexampleapp.com', // origin must match your domain & subdomain
  icons: ['https://avatars.githubusercontent.com/u/179229932']
}

// Create the modal
const modal = createAppKit({
  adapters: [wagmiAdapter],
  projectId,
  networks: [mainnet, arbitrum],
  defaultNetwork: mainnet,
  metadata: metadata,
  features: {
    analytics: true // Optional - defaults to your Cloud configuration
  }
})

function ContextProvider({ children, cookies }: { children: ReactNode; cookies: string | null }) {
  const initialState = cookieToInitialState(wagmiAdapter.wagmiConfig as Config, cookies)

  return (
    <WagmiProvider config={wagmiAdapter.wagmiConfig as Config} initialState={initialState}>
      <QueryClientProvider client={queryClient}>{children}</QueryClientProvider>
    </WagmiProvider>
  )
}

export default ContextProvider
```

```jsx app/layout.tsx
import type { Metadata } from 'next'
import { Inter } from 'next/font/google'
import './globals.css'

const inter = Inter({ subsets: ['latin'] })

import { headers } from 'next/headers' // added
import ContextProvider from '@/context'

export const metadata: Metadata = {
  title: 'AppKit Example App',
  description: 'Powered by Reown'
}

export default function RootLayout({
  children
}: Readonly<{
  children: React.ReactNode
}>) {

  const headersObj = await headers();
  const cookies = headersObj.get('cookie')

  return (
    <html lang="en">
      <body className={inter.className}>
        <ContextProvider cookies={cookies}>
          <appkit-button />
          {children}
        </ContextProvider>
      </body>
    </html>
  )
}
```

```jsx TestComponent.tsx
import { custom, toHex } from 'viem';
import { useWalletClient } from "wagmi";
import { StoryClient, StoryConfig } from "@story-protocol/core-sdk";

// example of how you would now use the fully setup sdk

export default function TestComponent() {
  const { data: wallet } = useWalletClient();

  async function setupStoryClient(): Promise<StoryClient> {
    const config: StoryConfig = {
      account: wallet!.account,
      transport: custom(wallet!.transport),
      chainId: "aeneid",
    };
    const client = StoryClient.newClient(config);
    return client;
  }

  async function registerIp() {
    const client = await setupStoryClient();
    const response = await client.ipAsset.register({
      nftContract: '0x01...',
      tokenId: '1',
      ipMetadata: {
        ipMetadataURI: "test-metadata-uri",
        ipMetadataHash: toHex("test-metadata-hash", { size: 32 }),
        nftMetadataURI: "test-nft-metadata-uri",
        nftMetadataHash: toHex("test-nft-metadata-hash", { size: 32 }),
      },
      txOptions: { waitForTransaction: true },
    });
    console.log(
      `Root IPA created at tx hash ${response.txHash}, IPA ID: ${response.ipId}`
    );
  }

  return (
    {/* */}
  )
}
```


# TypeScript SDK Setup

### :warning: Prerequisites

We require node version 18 or later version and npm version 8 to be installed in your environment. To install node and npm, we recommend you go to the [Node.js official website](https://nodejs.org) and download the latest LTS (Long Term Support) version.

### :package: Install the Dependencies

Install the [Story Protocol SDK](https://www.npmjs.com/package/@story-protocol/core-sdk) node package, as well as [viem](https://www.npmjs.com/package/viem).

```shell npm
npm install --save @story-protocol/core-sdk viem
```

```shell pnpm
pnpm install @story-protocol/core-sdk viem
```

```shell yarn
yarn add @story-protocol/core-sdk viem
```

## Initiate SDK Client

Next we can initiate the SDK Client. There are two ways to do this:

1. Using a private key (preferable for some backend admin)
2. JSON-RPC account like Metamask where users sign their own transactions

### :key: Set Up Private Key Account

<Cards columns={1}>
  <Card title="Working Example" href="https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/utils/utils.ts" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    Check out the TypeScript Tutorial for a working example of how to set up the Story SDK Client.
  </Card>
</Cards>

Before continuing with the code below:

1. Make sure to have `WALLET_PRIVATE_KEY` set up in your `.env` file.
2. Make sure to have `RPC_PROVIDER_URL` set up in your `.env` file.
   1. You can use the public default one (`https://aeneid.storyrpc.io`) or check out the other RPCs [here](https://docs.story.foundation/docs/aeneid#-rpcs).

```typescript utils.ts
import { http } from "viem";
import { Account, privateKeyToAccount, Address } from "viem/accounts";
import { StoryClient, StoryConfig } from "@story-protocol/core-sdk";

const privateKey: Address = `0x${process.env.WALLET_PRIVATE_KEY}`;
const account: Account = privateKeyToAccount(privateKey);

const config: StoryConfig = {
  account: account, // the account object from above
  transport: http(process.env.RPC_PROVIDER_URL),
  chainId: "aeneid",
};
export const client = StoryClient.newClient(config);
```

### :purse: Set Up JSON-RPC Account (ex. Metamask)

We can also use the TypeScript SDK to delay signing & sending transactions to a JSON-RPC account like Metamask.

We recommend using wagmi as a Web3 provider and then installing a wallet service like Dynamic, RainbowKit, or WalletConnect. We provide an example for all 3:

- [Dynamic Setup Tutorial](doc:dynamic-setup)
- [RainbowKit Setup Tutorial](doc:rainbowkit-setup)
- [WalletConnect Setup Tutorial](doc:walletconnect-setup)
- [Tomo Setup Tutorial](doc:tomo-setup)


# Tomo Setup

> 📘 Optional: Official TomoEVMKit Docs
>
> Check out the official Wagmi + TomoEVMKit installation docs [here](https://docs.tomo.inc/tomo-sdk/tomoevmkit/quick-start).

## Install the Dependencies

```shell npm
npm install --save @story-protocol/core-sdk @tomo-inc/tomo-evm-kit wagmi viem @tanstack/react-query
```

```shell pnpm
pnpm install @story-protocol/core-sdk viem
```

```shell yarn
yarn add @story-protocol/core-sdk viem
```

## Setup

Before diving into the example, make sure you have two things setup:

1. Make sure to have `NEXT_PUBLIC_RPC_PROVIDER_URL` set up in your `.env` file.
   1. You can use the public default one (`https://aeneid.storyrpc.io`) or any other RPC [here](https://docs.story.foundation/docs/aeneid#-rpcs).
2. Make sure to have `NEXT_PUBLIC_TOMO_CLIENT_ID` set up in your `.env` file. Do this by logging into the [Tomo Dashboard](https://dashboard.tomo.inc/) and creating a project.
3. Make sure to have `NEXT_PUBLIC_WALLET_CONNECT_PROJECT_ID` set up in your `.env` file. Do this by logging into [Reown (prev. WalletConnect)](https://reown.com/) and creating a project.

```jsx Web3Providers.tsx
"use client";
import '@tomo-inc/tomo-evm-kit/styles.css';
import { getDefaultConfig, TomoEVMKitProvider } from "@tomo-inc/tomo-evm-kit";
import { WagmiProvider } from "wagmi";
import { QueryClientProvider, QueryClient } from "@tanstack/react-query";
import { PropsWithChildren } from "react";
import { aeneid } from "@story-protocol/core-sdk";

const config = getDefaultConfig({
  appName: "Test Story App",
  clientId: process.env.NEXT_PUBLIC_TOMO_CLIENT_ID as string,
  projectId: process.env.NEXT_PUBLIC_WALLET_CONNECT_PROJECT_ID as string,
  chains: [aeneid],
  ssr: true, // If your dApp uses server side rendering (SSR)
});

const queryClient = new QueryClient();

export default function Web3Providers({ children }: PropsWithChildren) {
  return (
    <WagmiProvider config={config}>
      <QueryClientProvider client={queryClient}>
        <TomoEVMKitProvider>
          {children}
        </TomoEVMKitProvider>
      </QueryClientProvider>
    </WagmiProvider>
  );
}
```

```jsx layout.tsx
import type { Metadata } from "next";
import { Inter } from "next/font/google";
import "./globals.css";
import { PropsWithChildren } from "react";
import Web3Providers from "./Web3Providers";
import { useConnectModal } from "@tomo-inc/tomo-evm-kit";

const inter = Inter({ subsets: ["latin"] });

export const metadata: Metadata = {
  title: "Example",
  description: "This is an Example DApp",
};

export default function RootLayout({ children }: PropsWithChildren) {
  const { openConnectModal } = useConnectModal();

  return (
    <html lang="en">
      <body>
        <Web3Providers>
          <button onClick={openConnectModal}>Connect Wallet</button>
          {children}
        </Web3Providers>
      </body>
    </html>
  );
}
```

```jsx TestComponent.tsx
import { custom, toHex } from 'viem';
import { useWalletClient } from "wagmi";
import { StoryClient, StoryConfig } from "@story-protocol/core-sdk";

// example of how you would now use the fully setup sdk

export default function TestComponent() {
  const { data: wallet } = useWalletClient();

  async function setupStoryClient(): Promise<StoryClient> {
    const config: StoryConfig = {
      account: wallet!.account,
      transport: custom(wallet!.transport),
      chainId: "aeneid",
    };
    const client = StoryClient.newClient(config);
    return client;
  }

  async function registerIp() {
    const client = await setupStoryClient();
    const response = await client.ipAsset.register({
      nftContract: '0x01...',
      tokenId: '1',
      ipMetadata: {
        ipMetadataURI: "test-metadata-uri",
        ipMetadataHash: toHex("test-metadata-hash", { size: 32 }),
        nftMetadataURI: "test-nft-metadata-uri",
        nftMetadataHash: toHex("test-nft-metadata-hash", { size: 32 }),
      },
      txOptions: { waitForTransaction: true },
    });
    console.log(
      `Root IPA created at tx hash ${response.txHash}, IPA ID: ${response.ipId}`
    );
  }

  return (
    {/* */}
  )
}
```


# RainbowKit Setup

> 📘 Optional: Official RainbowKit Docs
>
> Check out the official Wagmi + RainbowKit installation docs [here](https://www.rainbowkit.com/docs/installation).

## Install the Dependencies

```shell npm
npm install --save @story-protocol/core-sdk @rainbow-me/rainbowkit wagmi viem @tanstack/react-query
```

```shell pnpm
pnpm install @story-protocol/core-sdk viem
```

```shell yarn
yarn add @story-protocol/core-sdk viem
```

## Setup

Before diving into the example, make sure you have two things setup:

1. Make sure to have `NEXT_PUBLIC_RPC_PROVIDER_URL` set up in your `.env` file.
   1. You can use the public default one (`https://aeneid.storyrpc.io`) or any other RPC [here](https://docs.story.foundation/docs/aeneid#-rpcs).
2. Make sure to have `NEXT_PUBLIC_WALLET_CONNECT_PROJECT_ID` set up in your `.env` file. Do this by logging into [Reown (prev. WalletConnect)](https://reown.com/) and creating a project.

```jsx Web3Providers.tsx
"use client";
import "@rainbow-me/rainbowkit/styles.css";
import { getDefaultConfig, RainbowKitProvider } from "@rainbow-me/rainbowkit";
import { WagmiProvider } from "wagmi";
import { QueryClientProvider, QueryClient } from "@tanstack/react-query";
import { PropsWithChildren } from "react";
import { aeneid } from "@story-protocol/core-sdk";

const config = getDefaultConfig({
  appName: "Test Story App",
  projectId: process.env.NEXT_PUBLIC_WALLET_CONNECT_PROJECT_ID as string,
  chains: [aeneid],
  ssr: true, // If your dApp uses server side rendering (SSR)
});

const queryClient = new QueryClient();

export default function Web3Providers({ children }: PropsWithChildren) {
  return (
    <WagmiProvider config={config}>
      <QueryClientProvider client={queryClient}>
        <RainbowKitProvider>
          {children}
        </RainbowKitProvider>
      </QueryClientProvider>
    </WagmiProvider>
  );
}
```

```jsx layout.tsx
import type { Metadata } from "next";
import { Inter } from "next/font/google";
import "./globals.css";
import { PropsWithChildren } from "react";
import Web3Providers from "./Web3Providers";
import { ConnectButton } from "@rainbow-me/rainbowkit";

const inter = Inter({ subsets: ["latin"] });

export const metadata: Metadata = {
  title: "Example",
  description: "This is an Example DApp",
};

export default function RootLayout({ children }: PropsWithChildren) {
  return (
    <html lang="en">
      <body>
        <Web3Providers>
          <ConnectButton />
          {children}
        </Web3Providers>
      </body>
    </html>
  );
}
```

```jsx TestComponent.tsx
import { custom, toHex } from 'viem';
import { useWalletClient } from "wagmi";
import { StoryClient, StoryConfig } from "@story-protocol/core-sdk";

// example of how you would now use the fully setup sdk

export default function TestComponent() {
  const { data: wallet } = useWalletClient();

  async function setupStoryClient(): Promise<StoryClient> {
    const config: StoryConfig = {
      account: wallet!.account,
      transport: custom(wallet!.transport),
      chainId: "aeneid",
    };
    const client = StoryClient.newClient(config);
    return client;
  }

  async function registerIp() {
    const client = await setupStoryClient();
    const response = await client.ipAsset.register({
      nftContract: '0x01...',
      tokenId: '1',
      ipMetadata: {
        ipMetadataURI: "test-metadata-uri",
        ipMetadataHash: toHex("test-metadata-hash", { size: 32 }),
        nftMetadataURI: "test-nft-metadata-uri",
        nftMetadataHash: toHex("test-nft-metadata-hash", { size: 32 }),
      },
      txOptions: { waitForTransaction: true },
    });
    console.log(
      `Root IPA created at tx hash ${response.txHash}, IPA ID: ${response.ipId}`
    );
  }

  return (
    {/* */}
  )
}
```


# Dynamic Setup

> 📘 Optional: Official Dynamic Docs
>
> Check out the official Wagmi + Dynamic installation docs [here](https://docs.dynamic.xyz/react-sdk/using-wagmi).

## Install the Dependencies

```shell npm
npm install --save @story-protocol/core-sdk viem wagmi @dynamic-labs/sdk-react-core @dynamic-labs/wagmi-connector @dynamic-labs/ethereum @tanstack/react-query
```

```shell pnpm
pnpm install @story-protocol/core-sdk viem
```

```shell yarn
yarn add @story-protocol/core-sdk viem
```

## Setup

Before diving into the example, make sure you have two things setup:

1. Make sure to have `NEXT_PUBLIC_RPC_PROVIDER_URL` set up in your `.env` file.
   1. You can use the public default one (`https://aeneid.storyrpc.io`) or any other RPC [here](https://docs.story.foundation/docs/aeneid#-rpcs).
2. Make sure to have `NEXT_PUBLIC_DYNAMIC_ENV_ID` set up in your `.env` file. Do this by logging into [Dynamic](https://app.dynamic.xyz/) and creating a project.

```jsx Web3Providers.tsx
"use client";
import { createConfig, WagmiProvider } from "wagmi";
import { http } from 'viem';
import { QueryClient, QueryClientProvider } from "@tanstack/react-query";
import { DynamicContextProvider } from "@dynamic-labs/sdk-react-core";
import { DynamicWagmiConnector } from "@dynamic-labs/wagmi-connector";
import { EthereumWalletConnectors } from "@dynamic-labs/ethereum";
import { PropsWithChildren } from "react";
import { aeneid } from "@story-protocol/core-sdk";

// setup wagmi
const config = createConfig({
  chains: [aeneid],
  multiInjectedProviderDiscovery: false,
  transports: {
    [aeneid.id]: http(),
  },
});
const queryClient = new QueryClient();

export default function Web3Providers({ children }: PropsWithChildren) {
  return (
    // setup dynamic
    <DynamicContextProvider
      settings={{
        // Find your environment id at https://app.dynamic.xyz/dashboard/developer
        environmentId: process.env.NEXT_PUBLIC_DYNAMIC_ENV_ID as string,
        walletConnectors: [EthereumWalletConnectors],
      }}
    >
      <WagmiProvider config={config}>
        <QueryClientProvider client={queryClient}>
          <DynamicWagmiConnector>
            {children}
          </DynamicWagmiConnector>
        </QueryClientProvider>
      </WagmiProvider>
    </DynamicContextProvider>
  );
}
```

```jsx layout.tsx
import type { Metadata } from "next";
import { Inter } from "next/font/google";
import "./globals.css";
import { PropsWithChildren } from "react";
import Web3Providers from "./Web3Providers";
import { DynamicWidget } from "@dynamic-labs/sdk-react-core";

const inter = Inter({ subsets: ["latin"] });

export const metadata: Metadata = {
  title: "Example",
  description: "This is an Example DApp",
};

export default function RootLayout({ children }: PropsWithChildren) {
  return (
    <html lang="en">
      <body>
        <Web3Providers>
          <DynamicWidget />
          {children}
        </Web3Providers>
      </body>
    </html>
  );
}
```

```jsx TestComponent.tsx
import { custom, toHex } from 'viem';
import { useWalletClient } from "wagmi";
import { StoryClient, StoryConfig } from "@story-protocol/core-sdk";

// example of how you would now use the fully setup sdk

export default function TestComponent() {
  const { data: wallet } = useWalletClient();

  async function setupStoryClient(): Promise<StoryClient> {
    const config: StoryConfig = {
      account: wallet!.account,
      transport: custom(wallet!.transport),
      chainId: "aeneid",
    };
    const client = StoryClient.newClient(config);
    return client;
  }

  async function registerIp() {
    const client = await setupStoryClient();
    const response = await client.ipAsset.register({
      nftContract: '0x01...',
      tokenId: '1',
      ipMetadata: {
        ipMetadataURI: "test-metadata-uri",
        ipMetadataHash: toHex("test-metadata-hash", { size: 32 }),
        nftMetadataURI: "test-nft-metadata-uri",
        nftMetadataHash: toHex("test-nft-metadata-hash", { size: 32 }),
      },
      txOptions: { waitForTransaction: true },
    });
    console.log(
      `Root IPA created at tx hash ${response.txHash}, IPA ID: ${response.ipId}`
    );
  }

  return (
    {/* */}
  )
}
```


# Register an IP Asset
<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/simpleMintAndRegisterSpg.ts" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    Follow the completed code all the way through.
  </Card>
</Cards>

Let's say you have some off-chain IP (ex. a book, a character, a drawing, etc). In order to register that IP on Story, you first need to mint an NFT. This NFT is the **ownership** over the IP. Then you **register** that NFT on Story, turning it into an [🧩 IP Asset](doc:ip-asset). The below tutorial will walk you through how to do this.

### :warning: Prerequisites

There are a few steps you have to complete before you can start the tutorial.

1. Complete the [TypeScript SDK Setup](doc:typescript-sdk-setup)
2. \[OPTIONAL] Go to [Pinata](https://pinata.cloud/) and create a new API key. Add the JWT to your `.env` file:

```yaml .env
PINATA_JWT=<YOUR_PINATA_JWT>
```

3. \[OPTIONAL] Install the `pinata-web3` dependency:

```Text Terminal
npm install pinata-web3
```

## 1. \[OPTIONAL] Set up your IP Metadata

We can set metadata on our NFT & IP, *but you don't have to*. To do this, view the [IPA Metadata Standard](doc:ipa-metadata-standard) and construct your metadata for both your NFT & IP.

Use the `generateIpMetadata` function to properly format your metadata and ensure it is of the correct type, as shown below:

```typescript main.ts
import { IpMetadata } from '@story-protocol/core-sdk'
// you should already have a client set up (prerequisite)
import { client } from './utils'

async function main() {
  const ipMetadata: IpMetadata = client.ipAsset.generateIpMetadata({
    title: 'My IP Asset',
    description: 'This is a test IP asset',
    watermarkImg: 'https://picsum.photos/200',
    attributes: [
      {
        key: 'Rarity',
        value: 'Legendary',
      },
    ],
  })
}

main();
```

## 2. \[OPTIONAL] Set up your NFT Metadata

The NFT Metadata follows the [ERC-721 Metadata Standard](https://eips.ethereum.org/EIPS/eip-721).

```typescript main.ts
import { IpMetadata } from '@story-protocol/core-sdk'
import { client } from './utils'

async function main() {
  // previous code here ...

  const nftMetadata = {
    name: 'Ownership NFT',
    description: 'This is an NFT representing owernship of our IP Asset.',
    image: 'https://picsum.photos/200',
  }
}

main();
```

## 3. \[OPTIONAL] Upload your IP and NFT Metadata to IPFS

In a separate `uploadToIpfs` file, create a function to upload your IP & NFT Metadata objects to IPFS:

```typescript uploadToIpfs.ts
import { PinataSDK } from "pinata-web3";

const pinata = new PinataSDK({
  pinataJwt: process.env.PINATA_JWT
});

export async function uploadJSONToIPFS(jsonMetadata: any): Promise<string> {
  const { IpfsHash } = await pinata.upload.json(jsonMetadata);
  return IpfsHash;
}
```

You can then use that function to upload your metadata, as shown below:

```typescript main.ts
import { IpMetadata } from '@story-protocol/core-sdk'
import { client } from './utils'
import { uploadJSONToIPFS } from './uploadToIpfs'
import { createHash } from 'crypto'

async function main() {
  // previous code here ...

  const ipIpfsHash = await uploadJSONToIPFS(ipMetadata)
  const ipHash = createHash('sha256').update(JSON.stringify(ipMetadata)).digest('hex')
  const nftIpfsHash = await uploadJSONToIPFS(nftMetadata)
  const nftHash = createHash('sha256').update(JSON.stringify(nftMetadata)).digest('hex')
}

main();
```

## 4. Register an NFT as an IP Asset

Remember that in order to register a new IP, we first have to mint an NFT, which will represent the underlying ownership of the IP. This NFT then gets "registered" and becomes an [🧩 IP Asset](doc:ip-asset).

For simplicity, we can use the SDK's `createNFTCollection` to create a new NFT collection for us. In a separate file `createSpgNftCollection.ts`, paste the following code (view a working example [here](https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/utils/createSpgNftCollection.ts)):

> ❓ Why do we have to do this?
>
> In order to use the `mintAndRegisterIp` function in the next step, we'll have to deploy an **SPG NFT** collection. **This is any contract that implements** [ISPGNFT](https://github.com/storyprotocol/protocol-periphery-v1/blob/main/contracts/interfaces/ISPGNFT.sol). An easy way to deploy a contract like this is to call the `createNFTCollection` as you'll see below.
>
> :warning: You only have to create a new SPG Collection **once**. Once you have your SPG NFT contract address, you can register any amount of IPs and will **not** have to do this again.
>
> Instead of doing this, you can mint an NFT from your own contract and use the [register](https://docs.story.foundation/docs/sdk-ipasset#register) function (providing an `nftContract` and `tokenId`) *instead of* using the `mintAndRegisterIp` function. See a working code example [here](https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/simpleMintAndRegister.ts).

```typescript createSpgNftCollection.ts
import { zeroAddress } from 'viem'
import { client } from './utils'

async function createSpgNftCollection() {
  const newCollection = await client.nftClient.createNFTCollection({
    name: 'Test NFTs',
    symbol: 'TEST',
    isPublicMinting: true,
    mintOpen: true,
    mintFeeRecipient: zeroAddress,
    contractURI: '',
    txOptions: { waitForTransaction: true },
  })

  console.log(`New SPG NFT collection created at transaction hash ${newCollection.txHash}`)
  console.log(`NFT contract address: ${newCollection.spgNftContract}`)
}

createSpgNftCollection();
```

If you run this code and look at the console output, you will find the SPG NFT contract address. Save this for later.

Now we can actually register our IP by minting an NFT, registering it as an [🧩 IP Asset](doc:ip-asset), and setting both NFT & IP metadata.

> Associated Docs: [ipAsset.mintAndRegisterIp](https://docs.story.foundation/docs/sdk-ipasset#mintandregisterip)

```typescript main.ts
import { IpMetadata } from '@story-protocol/core-sdk'
import { client } from './utils'
import { uploadJSONToIPFS } from './uploadToIpfs'
import { createHash } from 'crypto'
import { Address } from 'viem'

async function main() {
  // previous code here ...

  const response = await client.ipAsset.mintAndRegisterIp({
    // TODO: insert your SPG_NFT_CONTRACT_ADDRESS here
    spgNftContract: SPG_NFT_CONTRACT_ADDRESS as Address,
    allowDuplicates: true,
    ipMetadata: {
      ipMetadataURI: `https://ipfs.io/ipfs/${ipIpfsHash}`,
      ipMetadataHash: `0x${ipHash}`,
      nftMetadataURI: `https://ipfs.io/ipfs/${nftIpfsHash}`,
      nftMetadataHash: `0x${nftHash}`,
    },
    txOptions: { waitForTransaction: true },
  })
  
  console.log(`Root IPA created at transaction hash ${response.txHash}, IPA ID: ${response.ipId}`)
  console.log(`View on the explorer: https://explorer.story.foundation/ipa/${response.ipId}`)
}

main();
```

## 5. Add License Terms to IP

Congratulations, you registered an IP!

<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/simpleMintAndRegisterSpg.ts" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    Follow the completed code all the way through.
  </Card>
</Cards>

Now that your IP is registered, you can create and attach [License Terms](doc:license-terms) to it. This will allow others to mint a license and use your IP, restricted by the terms.

:point_right: We will go over this in the next section, but it's worth mentioning that instead of using the `mintAndRegisterIp` function, you can **register IP + create terms + attach terms** all in the same step with the following functions:

* [mintAndRegisterIpAssetWithPilTerms](https://docs.story.foundation/docs/sdk-ipasset#mintandregisteripassetwithpilterms)
* [registerIpAndAttachPilTerms](https://docs.story.foundation/docs/sdk-ipasset#registeripandattachpilterms)

# 🛠️ TypeScript SDK Guide
The best way to get started is to get your hands dirty and start building.

<Cards columns={2}>
  <Card title="Working Code Examples" href="https://github.com/storyprotocol/typescript-tutorial" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    Extremely easy & straightforward working code examples for all of the following tutorials.
  </Card>

  <Card title="Video Tutorial" href="https://www.youtube.com/watch?v=r3iIChDhp1g" icon="fa-video" iconColor="#FF0000" target="_blank">
    Watch a video tutorial overviewing an example script.
  </Card>
</Cards>

In the following series of tutorials, you will learn how to build IP applications with the Story SDK along with the concepts we mentioned in the [Architecture Overview](doc:overview).

# Claim Revenue
<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/registerDerivativeCommercialSpg.ts" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    All of this page is covered in this working code example.
  </Card>
</Cards>

This section demonstrates how to claim due revenue from an IP Asset.

There are two main ways revenue can be claimed:

1. **Scenario #1**: Someone pays my IP Asset directly, and I claim that revenue.
2. **Scenario #2**: Someone pays a derivative IP Asset of my IP, and I have the right to a % of their revenue based on the `commercialRevShare` in the license terms.

### :warning: Prerequisites

There are a few steps you have to complete before you can start the tutorial.

1. Complete the [TypeScript SDK Setup](doc:typescript-sdk-setup)
2. Have a basic understanding of the [💸 Royalty Module](doc:royalty-module)
3. Obviously, there must be a payment to be claimed. Read [Pay an IPA](doc:pay-ipa)

## Before We Start

When payments are made, they eventually end up in an IP Asset's [IP Royalty Vault](doc:ip-royalty-vault). From here, they are claimed/transferred to whoever owns the Royalty Tokens associated with it, which represent a % of revenue share for a given IP Asset's IP Royalty Vault.

The IP Account (the smart contract that represents the [🧩 IP Asset](doc:ip-asset)) is what holds 100% of the Royalty Tokens when it's first registered. So usually, it indeed holds most of the Royalty Tokens.

## Scenario #1

In this scenario, I own IP Asset 3. Someone pays my IP Asset 3 directly, and I claim that revenue. Let's view this in steps:

1. As we can see in the below diagram, when IP Asset 4 (it doesn't have to be an IP Asset, it can be any address) pays IP Asset 3 1M $WIP, 850k $WIP automatically gets deposited into IP Royalty Vault 3.

   ![](https://files.readme.io/54679e73c133053f6865da8f6587abf4090dc9edf7d87ff792fcb93cbe1f9b37-image.png)
2. Now, IP Asset 3 wants to claim its revenue sitting in the IP Royalty Vault 3. It will look like this:

   ![](https://files.readme.io/f5440ff2335b19874eda6607dbdf631045f9f9a0ce45e39f6d5cccf31f73dc51-image.png)

Below is how IP Asset 3 would claim their revenue, as shown in the image above, with the SDK:

> :eyes: Note the comments under the `claimOptions` object.\
> Associated Docs: [royalty.claimAllRevenue](https://docs.story.foundation/docs/sdk-royalty#claimallrevenue)

```typescript main.ts
import { WIP_TOKEN_ADDRESS } from '@story-protocol/core-sdk'
// you should already have a client set up (prerequisite)
import { client } from './client'

async function main() {
  const claimRevenue = await client.royalty.claimAllRevenue({
    // IP Asset 3's ipId
    ancestorIpId: '0xDa03c4B278AD44f5a669e9b73580F91AeDE0E3B2',
    // whoever owns the royalty tokens associated with IP Royalty Vault 3
    // (most likely the associated ipId, which is IP Asset 3's ipId)
    claimer: '0xDa03c4B278AD44f5a669e9b73580F91AeDE0E3B2',
    currencyTokens: [WIP_TOKEN_ADDRESS],
    childIpIds: [],
    royaltyPolicies: [],
    claimOptions: {
      // If the wallet claiming the revenue is the owner of the 
      // IP Account/IP Asset (in other words, the owner of the 
      // IP's underlying NFT), `claimAllRevenue` will transfer all 
      // earnings to the user's external wallet holding the NFT 
      // instead of the IP Account, for convenience. You can disable it here.
      autoTransferAllClaimedTokensFromIp: true,
      // Unwraps the claimed $WIP to $IP for you
      autoUnwrapIpTokens: true
    }
  })

  console.log(`Claimed revenue: ${claimRevenue.claimedTokens}`);
}

main();
```

## Scenario #2

In this scenario, I own IP Asset 1. Someone pays a derivative IP Asset 3, and I have the right to a % of their revenue based on the `commercialRevShare` in the license terms. This is exactly the same as Scenario #1, except one extra step is added. Let's view this in steps:

1. As we can see in the below diagram, when IP Asset 4 (it doesn't have to be an IP Asset, it can be any address) pays IP Asset 3 1M $WIP, 150k $WIP automatically gets deposited to the LAP royalty policy contract to be distributed to ancestors.

   ![](https://files.readme.io/26ac5c87e3e26305ac7426854991ce695922f4edc5e4594d70d40b32c62f2618-image.png)
2. Then, in a second step, the tokens are transferred to the ancestors' [IP Royalty Vault](doc:ip-royalty-vault) based on the negotiated `commercialRevShare` in the license terms.

   ![](https://files.readme.io/6c62982d41649c35d697e403d4f5281a73671488f260d9ca361e3a69e3d2888b-image.png)
3. Lastly, IP Asset 1 & 2 want to claim their revenue sitting in their associated IP Royalty Vaults. It will look like this:

   ![](https://files.readme.io/5b4616d79ac5304abc2d1292bb3f490663b0f7e6a5b54847bcc0f727ea43d19b-image.png)

Below is how IP Asset 1 (or 2) would claim their revenue, as shown in the image above, with the SDK:

> :eyes: Note the comments under the `claimOptions` object.\
> Associated Docs: [royalty.claimAllRevenue](https://docs.story.foundation/docs/sdk-royalty#claimallrevenue)

```typescript main.ts
import { WIP_TOKEN_ADDRESS } from '@story-protocol/core-sdk'
// you should already have a client set up (prerequisite)
import { client } from './client'

async function main() {
  const claimRevenue = await client.royalty.claimAllRevenue({
    // IP Asset 1's ipId
    ancestorIpId: '0x089d75C9b7E441dA3115AF93FF9A855BDdbfe384',
    // whoever owns the royalty tokens associated with IP Royalty Vault 1
    // (most likely the associated ipId, which is IP Asset 1's ipId)
    claimer: '0x089d75C9b7E441dA3115AF93FF9A855BDdbfe384',
    currencyTokens: [WIP_TOKEN_ADDRESS],
    // IP Asset 3's ipId
    childIpIds: ['0xDa03c4B278AD44f5a669e9b73580F91AeDE0E3B2'],
    // Aeneid testnet address of RoyaltyPolicyLAP
    royaltyPolicies: ['0xBe54FB168b3c982b7AaE60dB6CF75Bd8447b390E'],
    claimOptions: {
      // If the wallet claiming the revenue is the owner of the 
      // IP Account/IP Asset (in other words, the owner of the 
      // IP's underlying NFT), `claimAllRevenue` will transfer all 
      // earnings to the user's external wallet holding the NFT 
      // instead of the IP Account, for convenience. You can disable it here.
      autoTransferAllClaimedTokensFromIp: true,
      // Unwraps the claimed $WIP to $IP for you
      autoUnwrapIpTokens: true
    }
  })

  console.log(`Claimed revenue: ${claimRevenue.claimedTokens}`);
}

main();
```

## Dispute an IP

Congratulations, you claimed revenue using the [💸 Royalty Module](doc:royalty-module)!

<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/registerDerivativeCommercialSpg.ts" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    All of this page is covered in this working code example.
  </Card>
</Cards>

Now what happens if an IP Asset doesn't pay their due share? We can dispute the IP on-chain, which we will cover on the next page.

> 🚧 Coming soon!

# Register a Derivative
<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/registerDerivativeCommercialSpg.ts" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    All of this page is covered in this working code example.
  </Card>
</Cards>

This section demonstrates how to register an IP Asset as a derivative of another.

### :warning: Prerequisites

There are a few steps you have to complete before you can start the tutorial.

1. Complete the [TypeScript SDK Setup](doc:typescript-sdk-setup)

## 0. Before We Start

There are a lot of ways to register an IP Asset as a derivative of another. Below, we will help you figure out what function you should use.

> 📘 Have no idea?
>
> It is best to figure out which SDK function to use based on what is easiest for you. But if you have no idea, simply continue to the next section.

Do you already have a [License Token](doc:license-token) you can use?

* :white_check_mark: Yes: Is the derivative IP Asset already registered?
  * :white_check_mark: Yes: Use [registerDerivativeWithLicenseTokens](https://docs.story.foundation/docs/sdk-ipasset#registerderivativewithlicensetokens)
  * :x: No: Do you have your own NFT contract, or an already minted NFT?
    * :white_check_mark: Yes: Use [registerIpAndMakeDerivativeWithLicenseTokens](https://docs.story.foundation/docs/sdk-ipasset#registeripandmakederivativewithlicensetokens)
    * :x: No: Use [mintAndRegisterIpAndMakeDerivativeWithLicenseTokens](https://docs.story.foundation/docs/sdk-ipasset#mintandregisteripandmakederivativewithlicensetokens)
* :x: No: Is the derivative IP Asset already registered?
  * :white_check_mark: Yes: Use [registerDerivative](https://docs.story.foundation/docs/sdk-ipasset#registerderivative)
  * :x: No: Do you have your own NFT contract, or an already minted NFT?
    * :white_check_mark: Yes: Use [registerDerivativeIp](https://docs.story.foundation/docs/sdk-ipasset#registerderivativeip)
    * :x: No: Use [mintAndRegisterIpAndMakeDerivative](https://docs.story.foundation/docs/sdk-ipasset#mintandregisteripandmakederivative)

### 0a. :question: Why would I ever use a License Token if it's not needed?

There are a few times when **you would need** a License Token to register a derivative:

* The License Token contains private license terms, so you would only be able to register as a derivative if you had the License Token that was manually minted by the owner. More on that [here](https://docs.story.foundation/docs/license-token#private-licenses).
* The License Token (which is an NFT) costs a `mintingFee` to mint, and you were able to buy it on a marketplace for a cheaper price. Then it makes more sense to simply register with the License Token then have to pay the more expensive `defaultMintingFee`.

## 1. Register Derivative

> 📘 This is just an example
>
> You are encouraged to figure out the best derivative function to use based on the survey above. However, if you don't know and want to be walked through one solution, this next part is for you.

We're going to assume you have :x: no license tokens, :x: the derivative IP is not yet registered, and :x: you don't have your own NFT contract or an already minted NFT.

**Follow steps 1-4 of** [Register an IP Asset](doc:register-an-ip-asset). Note you can skip step 4 if you already have an SPG NFT Collection. Then, come back here.

Modify your code such that...

1. Instead of using `mintAndRegisterIp`, use `mintAndRegisterIpAndMakeDerivative`
2. Add a `derivData` field, where:
   1. `parentIpIds` is the `ipIds` of the parents you want to become a derivative of. **NOTE: Once you become a derivative, you cannot add more parents**
   2. `licenseTermIds` is an array of license terms you want to register under. These are the terms your derivative must abide by
   3. Set `maxMintingFee`, `maxRts`, and `maxRevenueShare` to be left as disabled/default as shown below

Now we can call the function like so:

> Associated Docs: [ipAsset.mintAndRegisterIpAndMakeDerivative](https://docs.story.foundation/docs/sdk-ipasset#mintandregisteripandmakederivative)

```typescript main.ts
import { IpMetadata, DerivativeData } from '@story-protocol/core-sdk'
import { client } from './utils'
import { uploadJSONToIPFS } from './uploadToIpfs'
import { createHash } from 'crypto'
import { Address } from 'viem'

async function main() {
  // previous code here ...
  
  const derivData: DerivativeData = {
    // TODO: insert the parent's ipId
    parentIpIds: [PARENT_IP_ID],
    // TODO: insert the licenseTermsId attached to parent IpId
    licenseTermsIds: [LICENSE_TERMS_ID],
    maxMintingFee: BitInt(0), // disabled
    maxRts: 100_000_000, // default
    maxRevenueShare: 100, // default
  };

  const response = await client.ipAsset.mintAndRegisterIpAndMakeDerivative({
    // TODO: insert your NFT contract address created by the SPG
    spgNftContract: SPG_NFT_CONTRACT_ADDRESS as Address,
    derivData,
    allowDuplicates: true,
    ipMetadata: {
      ipMetadataURI: `https://ipfs.io/ipfs/${ipIpfsHash}`,
      ipMetadataHash: `0x${ipHash}`,
      nftMetadataURI: `https://ipfs.io/ipfs/${nftIpfsHash}`,
      nftMetadataHash: `0x${nftHash}`,
    },
    txOptions: { waitForTransaction: true }
  });

  console.log(`Completed at transaction hash ${response.txHash}, IPA ID: ${response.ipId}, Token ID: ${response.tokenId}`);
}
```

## 2. Paying and Claiming Revenue

Congratulations, you registered a derivative IP Asset!

<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/registerDerivativeCommercialSpg.ts" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    All of this page is covered in this working code example.
  </Card>
</Cards>

Now that we have established parent-child IP relationships, we can begin to explore payments and automated revenue share based on the license terms. We'll cover that in the upcoming pages.

# Mint a License Token
This section demonstrates how to mint a [License Token](doc:license-token) from an [🧩 IP Asset](doc:ip-asset). You can only mint a License Token from an IP Asset if the IP Asset has [License Terms](doc:license-terms) attached to it. A License Token is minted as an ERC-721.

There are two reasons you'd mint a License Token:

1. To hold the license and be able to use the underlying IP Asset as the license described (for ex. "Can use commercially as long as you provide proper attribution and share 5% of your revenue)
2. Use the license token to link another IP Asset as a derivative of it. *Note though that, as you'll see later, some SDK functions don't require you to explicitly mint a license token first in order to register a derivative, and will actually handle it for you behind the scenes.*

### :warning: Prerequisites

There are a few steps you have to complete before you can start the tutorial.

1. Complete the [TypeScript SDK Setup](doc:typescript-sdk-setup)
2. An IP Asset that has License Terms added. Learn how to add License Terms to an IPA [here](doc:attach-terms-to-an-ip-asset).

## 1. Mint License

Let's say that IP Asset (`ipId = 0x01`) has License Terms (`licenseTermdId = 10`) attached to it. We want to mint 2 License Tokens with those terms to a specific wallet address (`0x02`).

> 🚧 Paid Licenses
>
> Be mindful that some IP Assets may have license terms attached that require the user minting the license to pay a `defaultMintingFee`. You can see an example of that in the <a href="https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/registerDerivativeCommercial.ts" target="_blank">TypeScript Tutorial</a>.

> Associated Docs: [license.mintLicenseTokens](https://docs.story.foundation/docs/sdk-license#mintlicensetokens)

```typescript main.ts
// you should already have a client set up (prerequisite)
import { client } from './client'

async function main() {
  const response = await client.license.mintLicenseTokens({
    licenseTermsId: "10", 
    licensorIpId: "0x01",
    receiver: "0x02", // optional. if not provided, it will go to the tx sender
    amount: 2,
    maxMintingFee: BigInt(0), // disabled
    maxRevenueShare: 100, // default
    txOptions: { waitForTransaction: true }
  });

  console.log(`License Token minted at transaction hash ${response.txHash}, License IDs: ${response.licenseTokenIds}`)
}

main();
```

### 1a. :hand: Setting Restrictions on Minting License Token

This is a note for owners of an IP Asset who want to set restrictions on who or how their license tokens are minted. You can:

* Set a max number of licenses that can be minted
* Charge dynamic fees based on who / how many are minted
* Whitelisted certain wallets to mint the tokens

... and more. Learn more by checking out the [License Config / Hook](doc:license-config-hook) section of our documentation.

## 2. Register a Derivative

Now that we have minted a License Token, we can hold it or use it to link an IP Asset as a derivative. We will go over that on the next page.

*Note though that, as you'll see later, some SDK functions don't require you to explicitly mint a license token first in order to register a derivative, and will actually handle it for you behind the scenes.*

### 2a. :question: Why would I ever use a License Token if it's not needed?

There are a few times when **you would need** a License Token to register a derivative:

* The License Token contains private license terms, so you would only be able to register as a derivative if you had the License Token that was manually minted by the owner. More on that [here](https://docs.story.foundation/docs/license-token#private-licenses).
* The License Token (which is an NFT) costs a `mintingFee` to mint, and you were able to buy it on a marketplace for a cheaper price. Then it makes more sense to simply register with the License Token then have to pay the more expensive `defaultMintingFee`.

# Releases Page
<Table align={["left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>

      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        <a href="https://github.com/storyprotocol/sdk/releases" target="_blank">SDK :arrow_upper_right:</a>
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        [Deployed Smart Contract Addresses ↗️](doc:deployed-smart-contracts)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        <a href="https://github.com/storyprotocol/protocol-core-v1/releases" target="_blank">Protocol Core :arrow_upper_right:</a>
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        <a href="https://github.com/storyprotocol/protocol-periphery-v1/releases" target="_blank">Protocol Periphery :arrow_upper_right:</a>
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        <a href="https://github.com/piplabs/story/releases" target="_blank">Story - Consensus Implementation :arrow_upper_right:</a>
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        <a href="https://github.com/piplabs/story-geth/releases" target="_blank">Story Geth - Execution Layer Implementation :arrow_upper_right:</a>
      </td>
    </tr>
  </tbody>
</Table>


# 👋 Dev Overview
If you're a developer, here is everything you need:

> 📘 Can't find something?
>
> Ask the writer of our docs on Telegram for help: @jacobmtucker

<Cards columns={3}>
  <Card title="Block Explorer" href="https://aeneid.storyscan.xyz" icon="fa-home" target="_blank">
    View all block & transaction data on Story.
  </Card>

  <Card title="IP-related Explorer" href="https://explorer.story.foundation" icon="fa-user" target="_blank">
    View transaction data specifically related to IP interactions like registering, licensing, etc.
  </Card>

  <Card title="Quickstart" href="https://docs.story.foundation/docs/quickstart" icon="fa-truck-fast" target="_blank">
    Start building on Story quickly.
  </Card>
</Cards>

# SDK

Check out the following resources to learn the SDK:

* [SDK Reference](doc:sdk-overview) - view the entire SDK reference with detailed **explanations and examples** for each function ***(includes working code so you can jump right to coding)***
* [🛠️ TypeScript SDK Guide](doc:typescript-sdk) - a detailed, step-by-step walkthrough of how to set up and implement the most popular uses of the SDK (register IP, attach terms, register derivative, etc) ***(includes working code so you can jump right to coding)***
* [📘 Tutorials](doc:tutorials) - more specific, external topics/tutorials that may be popular questions, like "How do I register music on Story?", "How do I implement wallet-less onboarding with the SDK?", etc ***(includes working code so you can jump right to coding)***

# Smart Contracts

Check out the following resources to learn the protocol:

* [⚙️ Smart Contract Guide](doc:get-started-with-the-smart-contracts) - a walkthrough of how to set up and implement the most popular uses of the protocol ***(includes working code so you can jump right to coding)***
* [Deployed Smart Contracts](doc:deployed-smart-contracts) - all the deployed protocol addresses
* [📘 Tutorials](doc:tutorials) - includes tutorials that cover more specific topics, like registering music, registering AI-generated images, etc ***(includes working code so you can jump right to coding)***

# API

View our [API Reference](https://docs.story.foundation/reference/api-introduction).

# Version Matrix
A version matrix showing the **currently available protocol versions** for different developer tools on both of our testnet networks. *Updated daily*.

<Table align={["left","left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>

      </th>

      <th style={{ textAlign: "left" }}>
        Odyssey Testnet
      </th>

      <th style={{ textAlign: "left" }}>
        Aeneid Testnet
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        SDK
      </td>

      <td style={{ textAlign: "left" }}>
        [v1.2](https://www.npmjs.com/package/@story-protocol/core-sdk/v/1.2.0-rc.4)
      </td>

      <td style={{ textAlign: "left" }}>
        [v1.3](https://www.npmjs.com/package/@story-protocol/core-sdk/v/1.3.0-beta.2)

        :warning: Note this is still a beta version.

        **Migration Guide**: To go from v1.2 to v1.3, check out the [SDK v1.3 MIGRATION GUIDE](doc:sdk-v13-migration-guide).
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Protocol
      </td>

      <td style={{ textAlign: "left" }}>
        [v1.2](https://github.com/storyprotocol/protocol-core-v1/tree/v1.2.3)
      </td>

      <td style={{ textAlign: "left" }}>
        [Core v1.3](https://github.com/storyprotocol/protocol-core-v1)\
        [Periphery v1.3](https://github.com/storyprotocol/protocol-periphery-v1)

        **Tutorial**: Check out [⚙️ Smart Contracts](doc:get-started-with-the-smart-contracts) for a tutorial on integrating with the v1.3 contracts
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        API
      </td>

      <td style={{ textAlign: "left" }}>
        :warning: Deprecated (no longer available)
      </td>

      <td style={{ textAlign: "left" }}>
        [v1.3](https://docs.story.foundation/reference/api-introduction)
      </td>
    </tr>
  </tbody>
</Table>

# SDK v1.3 MIGRATION GUIDE
Welcome to the v1.2 -> v1.3 SDK migration guide. Below, you can find the changes you must take to integrate with the v1.3 SDK.

> ❓ Need 1-on-1 help?
>
> Contact @jacobmtucker on Telegram.

# client.ipAsset

## `registerDerivative`

This function now has 3 extra parameters:

1. `maxMintingFee`: The maximum minting fee that the caller is willing to pay. If set to 0, then there is no no limit.
   1. **Recommended for simplicity**: `0`
2. `maxRevenueShare`: The maximum revenue share percentage agreed upon between a child and parent when a child is registering as derivative. Must be between 0 and 100.
   1. **Recommended for simplicity**: `100`
3. `maxRts`: The maximum number of royalty tokens that can be distributed to the external royalty policies. Must be between 0 and 100,000,000.
   1. **Recommended for simplicity**: `100_000_000`

Example:

```typescript TypeScript
const response = await client.ipAsset.registerDerivative({
  childIpId: "0x1daAE3197Bc469Cb97B917aa460a12dD95c6627c",
  parentIpIds: ["0xd142822Dc1674154EaF4DDF38bbF7EF8f0D8ECe4"],
  licenseTermsIds: ["1"],
  maxMintingFee: BigInt(0), // disabled
  maxRts: 100_000_000, // default
  maxRevenueShare: 100, // default
});
```

## `registerDerivativeWithLicenseTokens`

The function now has 1 extra parameter:

1. `maxRts`: The maximum number of royalty tokens that can be distributed to the external royalty policies. Must be between 0 and 100,000,000.
   1. **Recommended for simplicity**: `100_000_000`

Example:

```typescript TypeScript
const response = await client.ipAsset.registerDerivativeWithLicenseTokens({
  childIpId: "0xd142822Dc1674154EaF4DDF38bbF7EF8f0D8ECe4",
  licenseTokenIds: ["1"],
  maxRts: 100_000_000, // default
});
```

## `mintAndRegisterIpAssetWithPilTerms`

The function now has 1 extra parameter:

1. `allowDuplicates`: Set to true to allow minting an NFT with a duplicate `nftMetadataHash`.
   1. **Recommended for simplicity**: `true`

The function has also modified `request.terms` to be `request.licenseTermsData`, which now also takes a `licenseConfig` as described [here](https://docs.story.foundation/docs/license-config-hook).

:warning: This function will now fail if you do not provide any license terms. For that, just use `mintAndRegisterIp` instead.

Example:

```typescript TypeScript
const licenseTerms: LicenseTerms = {
  transferable: true,
  royaltyPolicy: "0x1daAE3197Bc469Cb97B917aa460a12dD95c6627c",
  defaultMintingFee: BigInt(1),
  expiration: BigInt(0),
  commercialUse: true,
  commercialAttribution: false,
  commercializerChecker: zeroAddress,
  commercializerCheckerData: zeroAddress,
  commercialRevShare: 0,
  commercialRevCeiling: BigInt(0),
  derivativesAllowed: true,
  derivativesAttribution: true,
  derivativesApproval: false,
  derivativesReciprocal: true,
  derivativeRevCeiling: BigInt(0),
  currency: "0x1514000000000000000000000000000000000000", // $WIP
  uri: "",
};

const licensingConfig: LicensingConfig = {
  isSet: false,
  mintingFee: BigInt(0),
  licensingHook: zeroAddress,
  hookData: zeroHash,
  commercialRevShare: 0,
  disabled: false,
  expectMinimumGroupRewardShare: 0,
  expectGroupRewardPool: zeroAddress,
};

const response = await client.ipAsset.mintAndRegisterIpAssetWithPilTerms({
  spgNftContract: "0x",
  licenseTermsData: [
    {
      terms: licenseTerms,
      licensingConfig,
    },
  ],
  allowDuplicates: true,
  ipMetadata: {
    ipMetadataURI: "https://",
    ipMetadataHash: toHex("metadata", { size: 32 }),
    nftMetadataURI: "https://",
    nftMetadataHash: toHex("nftMetadata", { size: 32 }),
  },
});
```

## `registerIpAndAttachPilTerms`

The function has modified `request.terms` to be `request.licenseTermsData`, which now also takes a `licenseConfig` as described [here](https://docs.story.foundation/docs/license-config-hook).

:warning: This function will now fail if you do not provide any license terms. For that, just use `register` instead.

Example:

```typescript TypeScript
const licenseTerms: LicenseTerms = {
  transferable: true,
  royaltyPolicy: "0x1daAE3197Bc469Cb97B917aa460a12dD95c6627c",
  defaultMintingFee: BigInt(1),
  expiration: BigInt(0),
  commercialUse: true,
  commercialAttribution: false,
  commercializerChecker: zeroAddress,
  commercializerCheckerData: zeroAddress,
  commercialRevShare: 0,
  commercialRevCeiling: BigInt(0),
  derivativesAllowed: true,
  derivativesAttribution: true,
  derivativesApproval: false,
  derivativesReciprocal: true,
  derivativeRevCeiling: BigInt(0),
  currency: "0x1514000000000000000000000000000000000000", // $WIP
  uri: "",
};

const licensingConfig: LicensingConfig = {
  isSet: false,
  mintingFee: BigInt(0),
  licensingHook: zeroAddress,
  hookData: zeroHash,
  commercialRevShare: 0,
  disabled: false,
  expectMinimumGroupRewardShare: 0,
  expectGroupRewardPool: zeroAddress,
};

const response = await client.ipAsset.registerIpAndAttachPilTerms({
  nftContract: "0x",
  tokenId: "3",
  ipMetadata: {
    ipMetadataURI: "https://",
    ipMetadataHash: toHex("metadata", { size: 32 }),
    nftMetadataURI: "https://",
    nftMetadataHash: toHex("nftMetadata", { size: 32 }),
  },
  licenseTermsData: [
    {
      terms: licenseTerms,
      licensingConfig,
    },
  ],
});
```

## `registerDerivativeIp`

This function now has 3 extra parameters under derivative data:

1. `maxMintingFee`: The maximum minting fee that the caller is willing to pay. If set to 0, then there is no no limit.
   1. **Recommended for simplicity**: `0`
2. `maxRevenueShare`: The maximum revenue share percentage agreed upon between a child and parent when a child is registering as derivative. Must be between 0 and 100.
   1. **Recommended for simplicity**: `100`
3. `maxRts`: The maximum number of royalty tokens that can be distributed to the external royalty policies. Must be between 0 and 100,000,000.
   1. **Recommended for simplicity**: `100_000_000`

Example:

```typescript TypeScript
const derivData: DerivativeData = {
  parentIpIds: ["0xd142822Dc1674154EaF4DDF38bbF7EF8f0D8ECe4"],
  licenseTermsIds: ["1"],
  maxMintingFee: BitInt(0), // disabled
  maxRts: 100_000_000, // default
  maxRevenueShare: 100, // default
};

const response = await client.ipAsset.registerDerivativeIp({
  nftContract: "0x",
  tokenId: "3",
  derivData,
  ipMetadata: {
    ipMetadataURI: "https://",
    ipMetadataHash: toHex("metadata", { size: 32 }),
    nftMetadataURI: "https://",
    nftMetadataHash: toHex("nftMetadata", { size: 32 }),
  },
})
```

## `mintAndRegisterIpAndMakeDerivative`

The function now has 1 extra parameter:

1. `allowDuplicates`: Set to true to allow minting an NFT with a duplicate `nftMetadataHash`.
   1. **Recommended for simplicity**: `true`

This function also has 3 extra parameters under derivative data:

1. `maxMintingFee`: The maximum minting fee that the caller is willing to pay. If set to 0, then there is no no limit.
   1. **Recommended for simplicity**: `0`
2. `maxRevenueShare`: The maximum revenue share percentage agreed upon between a child and parent when a child is registering as derivative. Must be between 0 and 100.
   1. **Recommended for simplicity**: `100`
3. `maxRts`: The maximum number of royalty tokens that can be distributed to the external royalty policies. Must be between 0 and 100,000,000.
   1. **Recommended for simplicity**: `100_000_000`

Example:

```typescript TypeScript
const derivData: DerivativeData = {
  parentIpIds: ["0xd142822Dc1674154EaF4DDF38bbF7EF8f0D8ECe4"],
  licenseTermsIds: ["1"],
  maxMintingFee: BitInt(0), // disabled
  maxRts: 100_000_000, // default
  maxRevenueShare: 100, // default
};

const response = await client.ipAsset.mintAndRegisterIpAndMakeDerivative({
  spgNftContract: "0x",
  derivData,
  allowDuplicates: true,
  ipMetadata: {
    ipMetadataURI: "https://",
    ipMetadataHash: toHex("metadata", { size: 32 }),
    nftMetadataURI: "https://",
    nftMetadataHash: toHex("nftMetadata", { size: 32 }),
  },
})
```

## `mintAndRegisterIp`

The function now has 1 extra parameter:

1. `allowDuplicates`: Set to true to allow minting an NFT with a duplicate `nftMetadataHash`.
   1. **Recommended for simplicity**: `true`

Example:

```typescript TypeScript
const response = await client.ipAsset.mintAndRegisterIp({
  spgNftContract: "0x",
  ipMetadata: {
    ipMetadataURI: "https://",
    ipMetadataHash: toHex("metadata", { size: 32 }),
    nftMetadataURI: "https://",
    nftMetadataHash: toHex("nftMetadata", { size: 32 }),
  },
  allowDuplicates: false,
})
```

## `registerPilTermsAndAttach`

The function has modified `request.terms` to be `request.licenseTermsData`, which now also takes a `licenseConfig` as described [here](https://docs.story.foundation/docs/license-config-hook).

:warning: This function will now fail if you do not provide any license terms. There is no reason you would be providing empty terms anyway.

Example:

```typescript TypeScript
const licenseTerms: LicenseTerms = {
  transferable: true,
  royaltyPolicy: "0x1daAE3197Bc469Cb97B917aa460a12dD95c6627c",
  defaultMintingFee: BigInt(1),
  expiration: BigInt(0),
  commercialUse: true,
  commercialAttribution: false,
  commercializerChecker: zeroAddress,
  commercializerCheckerData: zeroAddress,
  commercialRevShare: 0,
  commercialRevCeiling: BigInt(0),
  derivativesAllowed: true,
  derivativesAttribution: true,
  derivativesApproval: false,
  derivativesReciprocal: true,
  derivativeRevCeiling: BigInt(0),
  currency: "0x1514000000000000000000000000000000000000", // $WIP
  uri: "",
};

const licensingConfig: LicensingConfig = {
  isSet: false,
  mintingFee: BigInt(0),
  licensingHook: zeroAddress,
  hookData: zeroHash,
  commercialRevShare: 0,
  disabled: false,
  expectMinimumGroupRewardShare: 0,
  expectGroupRewardPool: zeroAddress,
};

const response = await client.ipAsset.registerPilTermsAndAttach({
  ipId: "0x",
  licenseTermsData: [
    {
      terms: licenseTerms,
      licensingConfig,
    },
  ],
})
```

## `mintAndRegisterIpAndMakeDerivativeWithLicenseTokens`

The function now has 2 extra parameters:

1. `allowDuplicates`: Set to true to allow minting an NFT with a duplicate `nftMetadataHash`.
   1. **Recommended for simplicity**: `true`
2. `maxRts`: The maximum number of royalty tokens that can be distributed to the external royalty policies. Must be between 0 and 100,000,000.
   1. **Recommended for simplicity**: `100_000_000`

Example:

```typescript TypeScript
const response = await client.ipAsset.mintAndRegisterIpAndMakeDerivativeWithLicenseTokens({
  spgNftContract: "0x",
  licenseTokenIds: ["12"],
  ipMetadata: {
    ipMetadataURI: "",
    ipMetadataHash: toHex(0, { size: 32 }),
    nftMetadataHash: toHex("nftMetadata", { size: 32 }),
    nftMetadataURI: "",
  },
  recipient: "0x73fcb515cee99e4991465ef586cfe2b072ebb512",
  maxRts: 100_000_000,
  allowDuplicates: true,
})
```

## `registerIpAndMakeDerivativeWithLicenseTokens`

The function now has 1 extra parameter:

1. `maxRts`: The maximum number of royalty tokens that can be distributed to the external royalty policies. Must be between 0 and 100,000,000.
   1. **Recommended for simplicity**: `100_000_000`

Example:

```typescript TypeScript
const response = await client.ipAsset.registerIpAndMakeDerivativeWithLicenseTokens({
  nftContract: "0x",
  maxRts: 100_000_000,
  tokenId: "3",
  licenseTokenIds: ["12"],
  ipMetadata: {
    ipMetadataURI: "",
    ipMetadataHash: toHex(0, { size: 32 }),
    nftMetadataHash: toHex("nftMetadata", { size: 32 }),
    nftMetadataURI: "",
  },
})
```

# `client.license`

## `mintLicenseTokens`

The function now has 2 extra parameters:

1. `maxMintingFee`: The maximum minting fee that the caller is willing to pay. If set to 0, then there is no no limit.
   1. **Recommended for simplicity**: `0`
2. `maxRevenueShare`: The maximum revenue share percentage agreed upon between a child and parent when a child is registering as derivative. Must be between 0 and 100.
   1. **Recommended for simplicity**: `100_000_000`

Example:

```typescript TypeScript
const response = await client.license.mintLicenseTokens({
  licensorIpId: "0x",
  licenseTermsId: "1",
  maxMintingFee: BigInt(0),
  maxRevenueShare: 100,
})
```

# client.royalty

All of the following functions have been combined into `claimAllRevenue`:

* `claimRevenue`
* `snapshot`
* `transferToVaultAndSnapshotAndClaimByTokenBatch`
* `transferToVaultAndSnapshotAndClaimBySnapshotBatch`
* `snapshotAndClaimByTokenBatch`
* `snapshotAndClaimBySnapshotBatch`

To learn how to use `claimAllRevenue` in the SDK, see [this page](https://docs.story.foundation/docs/claim-revenue).

# client.dispute

## `raiseDispute`

The function now has 2 extra parameters:

1. `bond`: The bond size.
2. `liveness`: The liveness time.

The following parameters have been removed:

1. `data`

Example:

```typescript TypeScript
const response = await client.dispute.raiseDispute({
  targetIpId: "0x1daAE3197Bc469Cb97B917aa460a12dD95c6627c",
  cid: "QmbWqxBEKC3P8tqsKc98xmWNzrzDtRLMiMPL8wBuTGsMnR",
  targetTag: "tag",
  bond: 0,
  liveness: 2592000,
})
```

