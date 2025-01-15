---
title: Mint NFTs as IP Assets using the Story Protocol Gateway (SPG)
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Bundled IP minting and registration

One common use case many developers will want is the ability to mint an IP as an ERC-721 and register or remix it in one go. This is especially relevant for registration of IP that does not yet exist on-chain, in which case you would first require an on-chain representation of your IP that is compatible with the protocol.

For these cases, we recommend using the Story Protocol SPG, which, besides offering more secure and convenient methods around remixing or registration of existing NFTs as IPAs, also offers a powerful minting engine through which you can directly create an ERC-721 representation of your IP while registering it into the protocol in a single interface.

Usually, you will likely not require your own custom ERC-721 collection for your IP, and simply want to perform ad-hoc registration of an off-chain asset. For these cases, we recommend using the **SPG default ERC-721 collection**, an ERC-721 that treats every NFT as an independent entity with its own customizable metadata, and also includes native support for rendering IP attribution within its `tokenURI()`. This means that these NFTs actually show you all the details related to IP attribution, along with its own core metadata. Let's start by examining two core use-cases:

## Bundled IP minting and registration with the SPG default ERC-721 collection

Performing standard registrations and remixing using the default SPG collection is actually very simple. Let's walk through code that contains two functions, one for minting and registering an off-chain asset as an IP, and the other for minting and remixing an off-chain asset as a derivative IP:

```
import { StoryProtocolGateway } from "@story-protocol/periphery/StoryProtocolGateway.sol";
import { Metadata } from "@story-protocol/periphery/lib/Metadata.sol";

contract ExampleSPGBundledMintAndRegistration {
  
  uint256 public immutable POLICY_ID;
  StoryProtocolGateway public immutable SPG;
  address public immutable DEFAULT_SPG_NFT;
  
  constructor(address spg, addres defaultCollection, uint256 policyId) {
    SPG = StoryProtocolGateway(spg);
    DEFAULT_SPG_NFT = defaultCollection;
    POLICY_ID = policyId;
  }
  
  function register(
  	string calldata nftName,
    string calldata nftDescription
    string calldata nftUrl,
    string calldata nftImage
	) {
    // Setup metadata attribution related to the NFT itself.
    Metadata.Attribute[] memory nftAttributes = new Metadata.Attribute[](1);
    nftAttributes[0] = Attribute({key: "Shirt-size", value: "XL"});
    bytes memory nftMetadata = abi.encode(
        Metadata.TokenData({
          name: nftName
          description: nftDescription
          externalUrl: nftUrl,
          image: nftImage,
          attributes: nftAttributes
        })
      );
      
      // Setup metadata attribution related to the IP semantics.
      Metadata.Attribute[] memory ipAttributes = new Metadata.Attribute[](1);
      attributes[0] = Attribute({key: "trademarkType", value: "merchandising"});
      Metadata.IPMetadata memory ipMetadata = Metadata.IPMetadata({
          name: "name for your IP asset",
          hash: bytes32("your IP asset content hash"),
          url:  "https://yourip.xyz/metadata-regarding-its-ip",
          customMetadata: ipAttributes
      });
      uint256 ipId = SPG.mintAndRegisterIp(
        POLICY_ID,
        DEFAULT_SPG_NFT,
        nftMetadata,
        ipMetadata
      );
    )
  }
}
```

In this example, we are building a registration function for converting a shirt that we are merchandising to an IP that exists on-chain. In the first part of the code, we prepare the metadata relevant to the NFT itself - i.e. that which you'd expect from any standard NFT collection. In the case of the default SPG NFT collection, metadata follows a custom encoding given by `Metadata.TokenData`. Then, in the latter part of the code, we perform metadata curation that is related to core IP semantics. In this case, we've provided the IPA the same name as the NFT itself, but there may be cases where these could be different.

By calling the `mintAndRegisterIp` function, the SPG will mint you an NFT representing your IP in the default SPG NFT collection, and then subsequently register it into the protocol and adding to it the configured policy type. 

Now, for minting an IP as an NFT and remixing in one shot, we would do something similar:

```
import { StoryProtocolGateway } from "@story-protocol/periphery/StoryProtocolGateway.sol";
import { Metadata } from "@story-protocol/periphery/lib/Metadata.sol";

contract ExampleSPGBundledMintAndRemixing {
  
  uint256 public constant MIN_ROYALTY = 10;/
  uint256 public immutable DEFAULT_LICENSE;
  StoryProtocolGateway public immutable SPG;
  address public immutable DEFAULT_SPG_NFT;
  
  constructor(address spg, address defaultCollection, uint256 defaultLicense) {
    SPG = StoryProtocolGateway(spg);
    DEFAULT_SPG_NFT = defaultCollection;
    DEFAULT_LICENSE = defaultLicense;
  }
  
  function remix(
  	uint256[] licenseIds,
  	string calldata nftName,
    string calldata nftDescription
    string calldata nftUrl,
    string calldata nftImage
	) {
    // Setup metadata attribution related to the NFT itself.
    Metadata.Attribute[] memory nftAttributes = new Metadata.Attribute[](1);
    nftAttributes[0] = Attribute({key: "Shirt-size", value: "XL"});
    bytes memory nftMetadata = abi.encode(
        Metadata.TokenData({
          name: nftName
          description: nftDescription
          externalUrl: nftUrl,
          image: nftImage,
          attributes: nftAttributes
        })
      );
      
      // Setup metadata attribution related to the IP semantics.
      Metadata.Attribute[] memory ipAttributes = new Metadata.Attribute[](1);
      attributes[0] = Attribute({key: "trademarkType", value: "merchandising"});
    	Metadata.IPMetadata memory ipMetadata = Metadata.IPMetadata({
          name: "name for your IP asset",
          hash: bytes32("your IP asset content hash"),
          url:  "https://yourip.xyz/metadata-regarding-its-ip",
          customMetadata: ipAttributes
      });
      uint256 ipId = SPG.mintAndRegisterDerivativeIp(
        licenseIds,
        ROYALTY_CONTEXT,
        DEFAULT_SPG_NFT,
        nftMetadata,
        ipMetadata
      );
    )
  }
}
```

Here, we call the `mintAndRegisterDerivativeIp` function of the SPG, which allows to mint our IP as an ERC-721 in the default SPG collection, and subsequently perform derivative IP registration based on the input licenses. 

## Bundled IP minting and registration with custom SPG ERC-721 collections

Because the SPG is a minting engine itself, it actually supports creation of custom IP collections out-of-the-box that allows you to perform bundled mint-backed registrations. In this section, we'll go over how you can leverage the SPG to create your own collection and use it for minting and registration of IP assets.

*Note: If you want something even more customizable, we recommend simply creating your own ERC-721 collection, and then leveraging the default`registerIp` or `registerDerivativeIp` SPG functions (or `register` if you were to call the IPA registry directly).*

Configuring your custom collection is actually very simple! All you need to do is choose one of the SPG supported collection types, and pass in settings related to your collection. Here is a sample code for IP minting and registration that utilizes a custom collection for doing so:

```
import { StoryProtocolGateway } from "@story-protocol/periphery/StoryProtocolGateway.sol";
import { IERC721MetadataProvider } from "@story-protocol/periphery/interfaces/nft/IERC721MetadataProvider.sol";
import { ERC721 } from "@story-protocol/periphery/lib/ERC721.sol";
import { Metadata } from "@story-protocol/periphery/lib/Metadata.sol";

contract ExampleCustomSPGCollectionRegistration {
  
  uint256 public immutable POLICY_ID;
  StoryProtocolGateway public immutable SPG;
  address public customCollection;
  IERC721MetadataProvider NFT_METADATA_PROVIDER;
  
  constructor(address spg, uint256 policyId, address metadataProvider) {
    SPG = StoryProtocolGateway(spg);
    POLICY_ID = policyId;
    NFT_METADATA_PROVIDER = IERC721MetadataProvider(metadataProvider);
  }
  
  function register(
  	string calldata nftName,
    string calldata nftDescription
    string calldata nftUrl,
    string calldata nftImage
	) {
    // Setup metadata attribution related to the NFT itself.
    Metadata.Attribute[] memory nftAttributes = new Metadata.Attribute[](1);
    nftAttributes[0] = Attribute({key: "Shirt-size", value: "XL"});
    bytes memory nftMetadata = abi.encode(
        Metadata.TokenData({
          name: nftName
          description: nftDescription
          externalUrl: nftUrl,
          image: nftImage,
          attributes: nftAttributes
        })
      );
      
      // Setup metadata attribution related to the IP semantics.
      Metadata.Attribute[] memory ipAttributes = new Metadata.Attribute[](1);
      attributes[0] = Attribute({key: "trademarkType", value: "merchandising"});
      Metadata.IPMetadata memory ipMetadata = Metadata.IPMetadata({
          name: "name for your IP asset",
          hash: bytes32("your IP asset content hash"),
          url:  "https://yourip.xyz/metadata-regarding-its-ip",
          customMetadata: ipAttributes
      });
      uint256 ipId = SPG.mintAndRegisterIp(
        POLICY_ID,
        DEFAULT_SPG_NFT,
        nftMetadata,
        ipMetadata
      );
    )
  }
  
  function _getOrCreateCustomCollection() returns (address) {
  	if (customCollection == address(0)) {
      customCollection = SPG.createIpCollection(
        ERC721.CollectionType.SP_DEFAULT_COLLECTION,
        ERC721.CollectionSetting({
          start: block.timestamp,
          end: 0,
          name: "My merchandising store",
          symbol: "MS",
          maxSupply: 999,
          metadataProvider: address(NFT_METADATA_PROVIDER)
        })
      );
      return customCollection;
    }

  }
}
```

Let us examine the `_getOrCreateCustomCollection()` internal function we wrote at the end. This leverages the SPG's `createIpCollection` function, which allows us to specify a collection type and a set of settings related to the collection as a whole. Here, we chose the `SP_DEFAULT_COLLECTION` type, which refers to the ERC721 variant that treats every NFT independently with its own customizable metadata. 

For the setting, `start` refers to when minting can begin for the collection, with `end` referring to the when it will terminate (0 means it will run indefinitely until the max supply is reached). In addition, we specify the name, symbol, and maximum supply. The metadata provider is an abstraction used by SPG ERC-721s for metadata rendering. You may choose to implement your own, as long as it conforms to the `IERC721MetadataProvider` interface, but we recommend simply passing in the Story Protocol default metadata provider (*refer to the deployments document for what this address is*).

*Note: For version`v0.1-beta`, the SPG only supports one type of collection, the `SP_DEFAULT_COLLECTION`, and only supports public mints by default.*
