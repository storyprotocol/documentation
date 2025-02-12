---
title: 🧩 IP Asset
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