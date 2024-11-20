---
title: NFT Client
excerpt: Used to mint a new SPG collection.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
## NftClient

### Methods

- createNFTCollection

### createNFTCollection

Creates a new SPG NFT Collection.

| Method                | Type                                                                            |
| --------------------- | ------------------------------------------------------------------------------- |
| `createNFTCollection` | `(request: CreateNFTCollectionRequest) => Promise<CreateNFTCollectionResponse>` |

Parameters:

- `request.name`: The name of the collection.
- `request.symbol`: The symbol of the collection.
- `request.isPublicMinting`: If true, anyone can mint from the collection. If false, only the addresses with the minter role can mint.
- `request.mintOpen`: Whether the collection is open for minting on creation.
- `request.mintFeeRecipient`: The address to receive mint fees.
- `request.contractURI`: The contract URI for the collection. Follows ERC-7572 standard. See [here](https://eips.ethereum.org/EIPS/eip-7572).
- `request.baseURI`: [Optional] The base URI for the collection. If baseURI is not empty, tokenURI will be either baseURI + token ID (if nftMetadataURI is empty) or baseURI + nftMetadataURI.
- `request.maxSupply`: [Optional] The maximum supply of the collection.
- `request.mintFee`: [Optional] The cost to mint a token.
- `request.mintFeeToken`: [Optional] The token to mint.
- `request.owner`: [Optional] The owner of the collection.
- `request.txOptions`: [Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type CreateNFTCollectionResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  spgNftContract?: Address; // the address of the newly created contract
};
```