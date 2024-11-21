---
title: Register an IP Asset
excerpt: Learn how to Register an NFT as an IP Asset in TypeScript.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
> ✅ Written Tutorial
>
> For a full written walkthrough of registering an IP Asset on Story, check out [How to Register IP on Story](doc:how-to-register-ip-on-story).

<Cards columns={1}>
  <Card title="Written Tutorial" href="https://docs.story.foundation/docs/how-to-register-ip-on-story#/" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    Check out a full written walkthrough of registering an IP Asset on Story.
  </Card>
</Cards>

This section demonstrates how to register an IP Asset using the TypeScript SDK. There are generally two methods of IP registration:

1. Register an existing NFT as an IP Asset
2. Create an NFT and register it as an IP Asset in one transaction

## Prerequisites

* [Setup](doc:typescript-sdk-setup) the client object.

> 📘 Default License Terms
>
> Note that every single IP Asset registered on Story automatically has [Non-Commercial Social Remixing License Terms](https://docs.story.foundation/docs/pil-flavors#flavor-1-non-commercial-social-remixing)  attached to it.
>
> If it's a root IP Asset (meaning it has no more parents), you can attach more License Terms to it later. Please see [Attach Terms to an IPA](doc:attach-terms-to-an-ip-asset) for more info.

# Register an NFT as an IP Asset

You can register your NFT as an IP Asset by simply calling `client.ipAsset.register()` and passing in the token's contract address and token ID, like so:

> 📘 NFT Metadata
>
> Note that this function will also set the underlying NFT's `tokenUri` to whatever is passed under `ipMetadata.nftMetadataURI`.

```typescript TypeScript
import { toHex } from 'viem';

const response = await client.ipAsset.register({
  nftContract: "0x041B4F29183317Fd352AE57e331154b73F8a1D73", // your NFT contract address
  tokenId: "12", // your NFT token ID
  // https://docs.story.foundation/docs/ip-asset#adding-nft--ip-metadata-to-ip-asset
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
} & IpMetadataAndTxOption;

type IpMetadataAndTxOption = {
  ipMetadata?: {
    ipMetadataURI?: string;
    ipMetadataHash?: Hex;
    nftMetadataURI?: string;
    nftMetadataHash?: Hex;
  };
  txOptions?: TxOptions;
};
```
```typescript Response Type
export type RegisterIpResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  ipId?: Address;
};
```

> :information_source: If the provided `tokenId` from the `nftContract` has already been registered, the `response` object will contain the `ipId` of the existing IP Asset and an undefined `txHash`.

Setting `waitForTransaction: true` in the transaction options will return the `ipId` of the newly registered IP asset.

After we run the above code, the console output will look like:

```Text Console Output
Root IPA created at transaction hash 0xa3e1caa7c2124b1550d459abc739291cb1be77ac73b99c097707878ac4ef57ae,
IPA ID: 0x4c1f8c1035a8cE379dd4ed666758Fb29696CF721
```

# Create an NFT and Register it as an IP Asset

Instead of first minting an NFT and then registering it as an IP Asset, this function allows you to mint and register an IP Asset all in one transaction

> 📘 Before You Use this Function
>
> The address of `spgNftContract` **must** implement <a href="https://github.com/storyprotocol/protocol-periphery-v1/blob/main/contracts/interfaces/ISPGNFT.sol" target="_blank">ISPGNFT</a> in order to work.
>
> An easy way to create a collection that implements ISPGNFT is to call the `createCollection` function on the `nftClient` in the SDK, as shown below.

> 📘 NFT Metadata
>
> Note that this function will also set the underlying NFT's `tokenUri` to whatever is passed under `ipMetadata.nftMetadataURI`.

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
} & IpMetadataAndTxOption;

type IpMetadataAndTxOption = {
  ipMetadata?: {
    ipMetadataURI?: string;
    ipMetadataHash?: Hex;
    nftMetadataURI?: string;
    nftMetadataHash?: Hex;
  };
  txOptions?: TxOptions;
};
```
```typescript Response Type
export type RegisterIpResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  ipId?: Address;
  tokenId?: bigint;
};
```