---
title: Create IP Collection
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
The `createIpCollection` function may be used for creating new IP collections whose minted NFTs may be registered as IP in Story Protocol. It is important to note that _ANY_ ERC-721 can be enrolled as an IP. However, only those registered via the SPG through this function may be natively minted and registered in one shot. On top of this, the SPG NFTs support an array of other benefits, such as native rendering of any tied IP data in the NFTs directly, as well as tooling around metadata management. If you want to register an IP and do not yet have an NFT to represent it, we still highly recommend using the global Story Protocol default IP collection, but if you would like to create something more custom then we recommend using the following function.

**Function Interface**:

```
/// @notice Creates a new Story Protocol NFT collection.
/// @param collectionType The type of ERC-721 collection to initialize.
/// @param collectionSettings Settings that apply to the collection as a whole.
/// @param mintSettings Settings that apply how the NFTs get minted.
function createIpCollection(
				SPG.CollectionType collectionType,
        SPG.CollectionSettings calldata collectionSettings,
        SPG.MintSettings calldata mintSettings
    ) external returns (address) {

```

**Applicable structs and enums** 

```
    /// @notice SPG ERC-721 configuration settings to pass in to a collection.
    struct CollectionSettings {
        string name;
        string symbol;
        uint256 maxSupply;
        bytes contractMetadata;
    }

    /// @notice SPG ERC-721 collection types usable for new IP representation.
    enum CollectionType {
        // The Story Protocol default ERC-721 collection - used when IP creators wish to tokenize
        // a single IP without the complexity of launching an entire collection an entire collection.
        SP_DEFAULT_COLLECTION,
        RFU // Reserved for future use - other collection types will be added in the future.
    }

    /// @notice Mint settings to configure for an SPG-managed collection drop.
    struct MintSettings {
        // Time at which the public mint should start.
        uint256 start;
        // Time at which the mint should end, or 0 if there is no end time.
        uint256 end;
    }


```

## Detailed Overview

The `createIpCollection` function takes three parameters

**`CollectionType`**

This enum represents the type of ERC-721 collection you want your IP NFTs to live under. Currently, for `v0.1-beta`, Story Protocol only supports a single default ERC-721 collection, the `SP_DEFAULT_COLLECTION`. This collection type assumes every NFT to be entirely independent, allowing users to supply arbitrarily custom metadata, including custom string key-value pairs. What this means is that Alice can use an NFT in this collection to represent her brand new T-Shirt print, while Stephanie can use it to represent her newest digital painting. Because of this flexibility, this is the collection type used by the global [Default IP Collection](doc:default-ip-collection) at Story Protocol. In the future, Story Protocol will add support for many more collection types.

**`CollectionSettings`**

This struct represents settings used both for NFT collection initialization, as well as management of future mints through the SPG. We will go over them one-by-one:

- `string name`
  - This represents the ERC-721 metadata name to provide for your IP collection. When your IP NFT is viewed on apps such as OpenSea, this will be the name displayed over the entire collection.
- `string symbol`
  - This represents the ERC-721 metadata symbol to provide for your collection. Like the name, when viewed on apps supporting ERC-721s, this symbol will be displayed as an abbreviated shorthand for your collection.
- `uint256 maxSupply`
  - This represents the maximum capped supply allowed for your ERC-721 collection. Supported collections should adhere to this input and ensure no more than `maxSupply` mints may be performed.
- `bytes contractMetadata`
  - This is any additional abi-encoded metadata to pass in for the collection as a whole, if the `metadataProvider` field is supported. It is dependent on the underlying provider how this data should be encoded. For the `SP_DEFAULT_COLLECTION` default provider, the metadata format should be an abi-encoding of the following struct:
    - ```
          struct ContractData {
              string description; // A description to associate with the entire IP collection.
              string image;       // A URL of an image to associate with the entire IP collection.
              string baseURI;     // A base URI to use for all tokens provided by the collection
          }
      ```





**`MintSettings`**

This struct represents settings configured for minting of a new IP collection:

- `uint256 start`
  - The time at which minting for the new IP collection should begin (for now, mints are public by default)
- `uint256 end`
  - The time at which the mint should end, or 0 if there is no end time.