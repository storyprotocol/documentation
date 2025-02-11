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
This section demonstrates how to register an IP Asset as a derivative of another.

> ❓ "Why would I ever use a License Token then?"
>
> There are a few times when you would need a License Token to register a derivative:
>
> * The License Token contains private license terms, so you would only be able to register if you had the License Token that was manually minted by the owner. More on that [here](https://docs.story.foundation/docs/license-token#private-licenses).
> * The License Token (which is an NFT) costs a `mintingFee` to mint, and you were able to buy it on a marketplace for a cheaper price. Then it makes more sense to simply register with the License Token then have to pay the more expensive `mintingFee`.

### :warning: Prerequisites

There are a few steps you have to complete before you can start the tutorial.

1. Complete the [TypeScript SDK Setup](doc:typescript-sdk-setup)

## 0) Before We Start

There are a lot of ways to register an IP Asset as a derivative of another. Below, we will help you figure out what function you should use.

> 📘 Have no idea?
>
> It is best to figure out which SDK function to use based on what is easiest for you. But if you have no idea, simply continue to the next section.

Do you already have a [License Token](doc:license-token) you can use?

* :white_check_mark: Yes: Is the derivative IP Asset already registered?
  * :white_check_mark: Yes: Use [registerDerivativeWithLicenseTokens](https://docs.story.foundation/docs/sdk-ipasset#/registerderivativewithlicensetokens)
  * :x: No: Do you have your own NFT contract, or an already minted NFT?
    * :white_check_mark: Yes: Use [registerIpAndMakeDerivativeWithLicenseTokens](https://docs.story.foundation/docs/sdk-ipasset#/registeripandmakederivativewithlicensetokens)
    * :x: No: Use [mintAndRegisterIpAndMakeDerivativeWithLicenseTokens](https://docs.story.foundation/docs/sdk-ipasset#/mintandregisteripandmakederivativewithlicensetokens)
* :x: No: Is the derivative IP Asset already registered?
  * :white_check_mark: Yes: Use [registerDerivative](https://docs.story.foundation/docs/sdk-ipasset#/registerderivative)
  * :x: No: Do you have your own NFT contract, or an already minted NFT?
    * :white_check_mark: Yes: Use [registerDerivativeIp](https://docs.story.foundation/docs/sdk-ipasset#/registerderivativeip)
    * :x: No: Use [mintAndRegisterIpAndMakeDerivative](https://docs.story.foundation/docs/sdk-ipasset#/mintandregisteripandmakederivative)

## 1. Register Derivative

You are encouraged to figure out the best derivative function to use based on the survey above. However, if you don't know and want to be walked through one solution, this next part is for you.

We're going to assume you have :x: no license tokens, :x: the derivative IP is not yet registered, and :x: you don't have your own NFT contract or an already minted NFT.

Follow steps 1-4 of [Register an IP Asset](doc:register-an-ip-asset). Then, come back here.

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