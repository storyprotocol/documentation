---
title: Register License Terms
excerpt: Learn how to register new License Terms in TypeScript.
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
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