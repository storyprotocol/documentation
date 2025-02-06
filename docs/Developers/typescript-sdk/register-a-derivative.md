---
title: Register a Derivative
excerpt: >-
  Learn how to register a derivative/remix IP Asset as a child of another in
  TypeScript.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
This section demonstrates how to register an IP Asset as a derivative of another. **Luckily there are many ways you can do this based on what is best for you**, and it is up to you to choose which one you need.

## Prerequisites

* [Setup](doc:typescript-sdk-setup) the client object.
* The parent IP Asset must be registered and have License Terms attached to it.

# Existing Child IP + License Token

## Additional Prerequisites

* An already minted License Token from the parent IP Asset. Learn how to mint a License Token [here](doc:mint-a-license).

If you already have a License Token, it is easier to register a derivative this way. We can register a child IPA as a derivative of a parent IPA by calling `client.ipAsset.registerDerivativeWithLicenseTokens()` like so:

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
  txOptions?: TxOptions;
};
```
```typescript Response Type
export type RegisterDerivativeWithLicenseTokensResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
};
```

# Existing Child IP + Parent IP

You can also register a derivative directly, without needing a License Token. There is no real difference between `registerDerivativeWithLicenseTokens` (above) and `registerDerivative` (below) except that the former requires an already minted License Token and the latter is a convenient function that handles it for you.

> ❓ "Why would I ever use a License Token then?"
>
> There are a few times when you would need a License Token to register a derivative:
>
> * The License Token contains private license terms, so you would only be able to register if you had the License Token that was manually minted by the owner. More on that [here](https://docs.story.foundation/docs/license-token#private-licenses).
> * The License Token (which is an NFT) costs a `mintingFee` to mint, and you were able to buy it on a marketplace for a cheaper price. Then it makes more sense to simply register with the License Token then have to pay the more expensive `mintingFee`.

> 📘 Quick Note
>
> Remember that once License Terms are attached to an IP Asset, it becomes public to register a derivative with those terms, which is why you don't necessarily need a License Token first. Read more on that [here](https://docs.story.foundation/docs/license-terms#license-terms-attached-to-ip-asset).

```typescript TypeScript
const response = await client.ipAsset.registerDerivative({
  childIpId: "0xC92EC2f4c86458AFee7DD9EB5d8c57920BfCD0Ba",
  parentIpIds: ["0xC92EC2f4c86458AFee7DD9EB5d8c57920BfCD0Ba"],
  licenseTermsIds: ["5"],
  txOptions: { waitForTransaction: true }
});

console.log(`Derivative IPA linked to parent at transaction hash ${response.txHash}`)
```
```typescript Request Type
export type RegisterDerivativeRequest = {
  childIpId: Address;
  parentIpIds: Address[];
  licenseTermsIds: string[] | bigint[] | number[];
  licenseTemplate?: Address;
  txOptions?: TxOptions;
};
```
```typescript Response Type
export type RegisterDerivativeResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  childIpId?: Address;
};
```

# Existing NFT, Register IP, and Link to Existing Parent IP

This function allows you to do all of the following: [Register an NFT as an IP Asset](doc:register-an-nft-as-an-ip-asset) :arrow_forward: [Register an IPA as a Derivative](doc:register-ipa-as-derivative)

```typescript TypeScript
import { toHex } from 'viem';

const response = await client.ipAsset.registerDerivativeIp({
  nftContract: "0x041B4F29183317Fd352AE57e331154b73F8a1D73", // your NFT contract address
  tokenId: '127',
  derivData: {
    parentIpIds: ['0x4c1f8c1035a8cE379dd4ed666758Fb29696CF721'],
    licenseTermsIds: ['13']
  },
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
  derivData: {
    parentIpIds: Address[];
    licenseTermsIds: string[] | bigint[] | number[];
    licenseTemplate?: Address;
  };
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
export type RegisterIpAndMakeDerivativeResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  ipId?: Address;
};
```

# Existing NFT, Register IP, and Link to Existing Parent IP w/ License Tokens

## Additional Prerequisites

* An already minted License Token from the parent IP Asset. Learn how to mint a License Token [here](doc:mint-a-license).

```typescript TypeScript
import { toHex } from 'viem';

const response = await client.ipAsset.registerIpAndMakeDerivativeWithLicenseTokens({
  nftContract: "0x041B4F29183317Fd352AE57e331154b73F8a1D73", // your NFT contract address
  tokenId: '127',
  licenseTokenIds: ['10'],
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

# Mint NFT, Register IP, and Link to Existing Parent IP

This function allows you to do all of the following: Mint an NFT :arrow_forward: [Register an NFT as an IP Asset](doc:register-an-nft-as-an-ip-asset) :arrow_forward: [Register an IPA as a Derivative](doc:register-ipa-as-derivative)

> 🚧 Before You Use this Function
>
> The address of `nftContract` **must** implement <a href="https://github.com/storyprotocol/protocol-periphery-v1/blob/main/contracts/interfaces/ISPGNFT.sol" target="_blank">ISPGNFT</a> in order to work.
>
> An easy way to create a collection that implements ISPGNFT is to call the `createCollection` function in the <a href="https://github.com/storyprotocol/protocol-periphery-v1/blob/main/contracts/StoryProtocolGateway.sol" target="_blank">SPG contract</a>.

> 📘 NFT Metadata
>
> Note that this function will also set the underlying NFT's `tokenUri` to whatever is passed under `ipMetadata.nftMetadataURI`.

```typescript TypeScript
import { PIL_TYPE } from '@story-protocol/core-sdk';
import { toHex } from 'viem';

const response = await client.ipAsset.mintAndRegisterIpAndMakeDerivative({
  // an NFT contract address created by the SPG
  spgNftContract: "0xfE265a91dBe911db06999019228a678b86C04959",
  derivData: {
    parentIpIds: ['0x4c1f8c1035a8cE379dd4ed666758Fb29696CF721'],
    licenseTermsIds: ['13']
  },
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
  nftContract: Address;
  derivData: {
    parentIpIds: Address[];
    licenseTermsIds: string[] | bigint[] | number[];
    licenseTemplate?: Address;
  };
  nftMetadata?: string;
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
export type RegisterDerivativeResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  childIpId?: Address;
  tokenId?: bigint;
};
```

# Mint NFT, Register IP, and Link to Existing Parent IP w/ License Tokens

## Additional Prerequisites

* An already minted License Token from the parent IP Asset. Learn how to mint a License Token [here](doc:mint-a-license).

```typescript TypeScript
import { toHex } from 'viem';

const response = await client.ipAsset.mintAndRegisterIpAndMakeDerivativeWithLicenseTokens({
  spgNftContract: "0xfE265a91dBe911db06999019228a678b86C04959", // your NFT contract address
  licenseTokenIds: ['10'],
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