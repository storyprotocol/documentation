---
title: Attach Terms to an IPA
excerpt: Learn how to Attach License Terms to an IP Asset in TypeScript.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
This section demonstrates how to add License Terms to an IPA. By attaching terms, an IPA becomes eligible for licensing creation. Users who then wish to creative derivatives of the IP may then mint licenses, which can be burned to enroll their IPs as derivative IPAs of the original work.

> 📘 Note
>
> [Non-Commercial Social Remixing](https://docs.story.foundation/docs/pil-flavors#flavor-1-non-commercial-social-remixing) License Terms are attached **by default** to every IP Asset.

There are a few different ways you can do this, depending on what works best for you:

1. Attach Existing License Terms to Existing IP Asset
2. Register New IP Asset and Attach License Terms
3. Register New Terms and Attach to an Existing IP Asset
4. Mint NFT, Register as IP Asset, and Attach Terms

## Prerequisites

* [Setup](doc:typescript-sdk-setup) the client object.

# Attach Existing License Terms to Existing IP Asset

Below is a code example to add License Terms to our IP Asset:

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

# Register New IP Asset and Attach License Terms

Below is a code example to register a new IP Asset and attach a new set of License Terms all in one transaction:

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
  currency: SUSD, // insert SUSD address from https://docs.story.foundation/docs/deployed-smart-contracts
  uri: '',
}

const response = await client.ipAsset.registerIpAndAttachPilTerms({
  nftContract: '0x041B4F29183317Fd352AE57e331154b73F8a1D73',
  tokenId: '12',
  terms: [commercialRemixTerms], // IP already has non-commercial social remixing terms. You can add more here.
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
  terms: RegisterPILTermsRequest[];
  deadline?: bigint | number | string;
} & IpMetadataAndTxOption;
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

# Register New Terms and Attach to an Existing IP Asset

Below is a code example to register terms and attach them to an IP Asset all in one transaction:

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
  currency: SUSD, // insert SUSD address from https://docs.story.foundation/docs/deployed-smart-contracts
  uri: '',
}

const response = await client.ipAsset.registerPilTermsAndAttach({
  ipId: '0x4c1f8c1035a8cE379dd4ed666758Fb29696CF721',
  terms: [commercialRemixTerms],
  txOptions: { waitForTransaction: true },
})
console.log(`License Terms ${response.licenseTermsId} attached to IP Asset.`)
```
```typescript Request Type
export type RegisterPilTermsAndAttachRequest = {
  ipId: Address;
  terms: RegisterPILTermsRequest[];
  deadline?: string | number | bigint;
  txOptions?: TxOptions;
};
```
```typescript Response Type
export type RegisterPilTermsAndAttachResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  licenseTermsIds?: bigint[];
};
```

# Mint NFT, Register as IP Asset, and Attach Terms

Below is a code example that mints a new NFT, registers it as an IP Asset, and attaches terms to it all in one transaction:

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
  currency: SUSD, // insert SUSD address from https://docs.story.foundation/docs/deployed-smart-contracts
  uri: '',
}

const response = await client.ipAsset.mintAndRegisterIpAssetWithPilTerms({
  spgNftContract: '0xfE265a91dBe911db06999019228a678b86C04959',
  terms: [commercialRemixTerms], // IP already has non-commercial social remixing terms. You can add more here.
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
  terms: RegisterPILTermsRequest[];
  recipient?: Address;
  royaltyPolicyAddress?: Address;
} & IpMetadataAndTxOption;
```
```typescript Response Type
export type MintAndRegisterIpAssetWithPilTermsResponse = {
  txHash?: Hex;
  encodedTxData?: EncodedTxData;
  ipId?: Address;
  tokenId?: bigint;
  licenseTermsIds?: bigint[];
};
```