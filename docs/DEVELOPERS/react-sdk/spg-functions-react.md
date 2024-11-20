---
title: SPG Functions in React
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
A group of functions provided by the [📦 SPG](doc:spg), which is essentially a way to combine independent operations like [Register an NFT as an IP Asset](doc:register-an-nft-as-an-ip-asset-react) and [Attach License Terms to an IP Asset](doc:attach-terms-to-an-ip-asset-react) into one transaction to make your life easier.

## Prerequisites

* [React SDK Setup](doc:react-sdk-setup)

# Mint + Register + Attach Terms

This function allows you to do all of the following: Mint an NFT :arrow_forward: [Register an NFT as an IP Asset](doc:register-an-nft-as-an-ip-asset-react) :arrow_forward: [Attach License Terms to an IP Asset](doc:attach-terms-to-an-ip-asset-react)

> 📘 Before You Use this Function
>
> The address of `nftContract` **must** implement <a href="https://github.com/storyprotocol/protocol-periphery-v1/blob/main/contracts/interfaces/ISPGNFT.sol" target="_blank">ISPGNFT</a> in order to work.
>
> An easy way to create a collection that implements ISPGNFT is to call the `createCollection` function in the <a href="https://github.com/storyprotocol/protocol-periphery-v1/blob/main/contracts/StoryProtocolGateway.sol" target="_blank">SPG contract</a>.

> 📘 NFT Metadata
>
> Note that this function will also set the underlying NFT's `tokenUri` to whatever is passed under `ipMetadata.nftMetadataURI`.

```jsx RegisterIPA.tsx
import { toHex } from 'viem';
import { useIpAsset, useNftClient, PIL_TYPE } from "@story-protocol/react-sdk";

export default async function RegisterIPA() {
  const { createNFTCollection } = useNftClient();
  const { mintAndRegisterIpAssetWithPilTerms } = useIpAsset();
  
  const newCollection = await createNFTCollection({ 
    name: 'Test NFT', 
    symbol: 'TEST', 
    txOptions: { waitForTransaction: true } 
  });
  
  const response = await mintAndRegisterIpAssetWithPilTerms({
    // an NFT contract address created by the SPG
    nftContract: newCollection.nftContract as Address,
    pilType: PIL_TYPE.NON_COMMERCIAL_REMIX,
    // https://docs.story.foundation/docs/ipa-metadata-standard
    ipMetadata: {
      ipMetadataURI: 'test-uri',
      ipMetadataHash: toHex('test-metadata-hash', { size: 32 }),
      nftMetadataHash: toHex('test-nft-metadata-hash', { size: 32 }),
      nftMetadataURI: 'test-nft-uri',
    },
    txOptions: { waitForTransaction: true }
  });
  
  console.log(`
    Completed at transaction hash ${response.txHash},
    NFT Token ID: ${response.tokenId}, 
    IPA ID: ${response.ipId}, 
    License Terms ID: ${response.licenseTermsId}
  `);
  
  return (
    {/* */} 
  )
}
```
```typescript Request Type
export type CreateIpAssetWithPilTermsRequest = {
  nftContract: Address;
  pilType: PIL_TYPE;
  currency?: Address;
  mintingFee?: string | number | bigint;
  recipient?: Address;
  commercialRevShare?: number;
  nftMetadata?: string;
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
export type CreateIpAssetWithPilTermsResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  ipId?: Address;
  tokenId?: bigint;
  licenseTermsId?: bigint;
};
```

# Register + Attach Terms

This function allows you to do all of the following: [Register an NFT as an IP Asset](doc:register-an-nft-as-an-ip-asset-react) :arrow_forward: [Attach License Terms to an IP Asset](doc:attach-terms-to-an-ip-asset-react)

```jsx RegisterIPA.tsx
import { toHex } from 'viem';
import { useIpAsset } from "@story-protocol/react-sdk";

export default async function RegisterIPA() {
  const { registerIpAndAttachPilTerms } = useIpAsset();
  
  const response = await registerIpAndAttachPilTerms({
    nftContract: "0xd516482bef63Ff19Ed40E4C6C2e626ccE04e19ED", // your NFT contract address
    tokenId: '127',
    pilType: PIL_TYPE.NON_COMMERCIAL_REMIX,
    // https://docs.story.foundation/docs/ipa-metadata-standard
    ipMetadata: {
      ipMetadataURI: 'test-uri',
      ipMetadataHash: toHex('test-metadata-hash', { size: 32 }),
      nftMetadataHash: toHex('test-nft-metadata-hash', { size: 32 }),
      nftMetadataURI: 'test-nft-uri',
    },
    txOptions: { waitForTransaction: true }
  });

  console.log(`
    Completed at transaction hash ${response.txHash},
    IPA ID: ${response.ipId},
    License Terms ID: ${response.licenseTermsId}
  `);
  
  return (
    {/* */} 
  )
}
```
```typescript Request Type
export type RegisterIpAndAttachPilTermsRequest = {
  nftContract: Address;
  tokenId: bigint | string | number;
  pilType: PIL_TYPE;
  mintingFee: string | number | bigint;
  currency: Address;
  deadline?: bigint | number | string;
  commercialRevShare?: number;
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
export type RegisterIpAndAttachPilTermsResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  ipId?: Address;
  licenseTermsId?: bigint;
};
```

# Register + Derivative

This function allows you to do all of the following: [Register an NFT as an IP Asset](doc:register-an-nft-as-an-ip-asset-react) :arrow_forward: [Register an IPA as a Derivative](doc:register-ipa-as-derivative-react)

```jsx RegisterDerivative.tsx
import { toHex } from 'viem';
import { useIpAsset } from "@story-protocol/react-sdk";

export default async function RegisterDerivative() {
  const { registerDerivativeIp } = useIpAsset();
  
  const response = await registerDerivativeIp({
    nftContract: "0xd516482bef63Ff19Ed40E4C6C2e626ccE04e19ED", // your NFT contract address
    tokenId: '127',
    derivData: {
      parentIpIds: ['0x4c1f8c1035a8cE379dd4ed666758Fb29696CF721'],
      licenseTermsIds: ['13']
    },
    // https://docs.story.foundation/docs/ipa-metadata-standard
    ipMetadata: {
      ipMetadataURI: 'test-uri',
      ipMetadataHash: toHex('test-metadata-hash', { size: 32 }),
      nftMetadataHash: toHex('test-nft-metadata-hash', { size: 32 }),
      nftMetadataURI: 'test-nft-uri',
    },
    txOptions: { waitForTransaction: true }
  });

  console.log(`Completed at transaction hash ${response.txHash}, IPA ID: ${response.ipId}`);
  
  return (
    {/* */} 
  )
}
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

# Mint + Register + Derivative

This function allows you to do all of the following: Mint an NFT :arrow_forward: [Register an NFT as an IP Asset](doc:register-an-nft-as-an-ip-asset) :arrow_forward: [Register an IPA as a Derivative](doc:register-ipa-as-derivative)

> 📘 Before You Use this Function
>
> The address of `nftContract` **must** implement <a href="https://github.com/storyprotocol/protocol-periphery-v1/blob/main/contracts/interfaces/ISPGNFT.sol" target="_blank">ISPGNFT</a> in order to work.
>
> An easy way to create a collection that implements ISPGNFT is to call the `createCollection` function in the <a href="https://github.com/storyprotocol/protocol-periphery-v1/blob/main/contracts/StoryProtocolGateway.sol" target="_blank">SPG contract</a>.

> 📘 NFT Metadata
>
> Note that this function will also set the underlying NFT's `tokenUri` to whatever is passed under `ipMetadata.nftMetadataURI`.

```jsx RegisterDerivative.tsx
import { toHex } from 'viem';
import { useIpAsset } from "@story-protocol/react-sdk";

export default async function RegisterDerivative() {
  const { mintAndRegisterIpAndMakeDerivative } = useIpAsset();
  
  const response = await mintAndRegisterIpAndMakeDerivative({
    // an NFT contract address created by the SPG
    nftContract: "0xd516482bef63Ff19Ed40E4C6C2e626ccE04e19ED",
    derivData: {
      parentIpIds: ['0x4c1f8c1035a8cE379dd4ed666758Fb29696CF721'],
      licenseTermsIds: ['13']
    },
    // https://docs.story.foundation/docs/ipa-metadata-standard
    ipMetadata: {
      ipMetadataURI: 'test-uri',
      ipMetadataHash: toHex('test-metadata-hash', { size: 32 }),
      nftMetadataHash: toHex('test-nft-metadata-hash', { size: 32 }),
      nftMetadataURI: 'test-nft-uri',
    },
    txOptions: { waitForTransaction: true }
  });

  console.log(`Completed at transaction hash ${response.txHash}, IPA ID: ${response.ipId}`);
  
  return (
    {/* */} 
  )
}
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
