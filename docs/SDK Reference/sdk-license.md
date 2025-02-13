---
title: License
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
## LicenseClient

### Methods

* attachLicenseTerms
* mintLicenseTokens
* registerPILTerms
* registerNonComSocialRemixingPIL
* registerCommercialUsePIL
* registerCommercialRemixPIL
* getLicenseTerms

### attachLicenseTerms

Attaches license terms to an IP.

| Method               | Type                                                                 |
| -------------------- | -------------------------------------------------------------------- |
| `attachLicenseTerms` | `(request: AttachLicenseTermsRequest) => AttachLicenseTermsResponse` |

Parameters:

* `request.ipId`: The address of the IP to which the license terms are attached.
* `request.licenseTemplate`: The address of the license template.
* `request.licenseTermsId`: The ID of the license terms.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

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

### mintLicenseTokens

Mints license tokens for the license terms attached to an IP.

The license tokens are minted to the receiver.

The license terms must be attached to the IP before calling this function.

IP owners can mint license tokens for their IPs for arbitrary license terms without attaching the license terms to IP.

It might require the caller pay the minting fee, depending on the license terms or configured by the iP owner. The minting fee is paid in the minting fee token specified in the license terms or configured by the IP owner. IP owners can configure the minting fee of their IPs or configure the minting fee module to determine the minting fee.

| Method              | Type                                                                        |
| ------------------- | --------------------------------------------------------------------------- |
| `mintLicenseTokens` | `(request: MintLicenseTokensRequest) => Promise<MintLicenseTokensResponse>` |

Parameters:

* `request.licensorIpId`: The licensor IP ID.
* `request.licenseTemplate`: The address of the license template.
* `request.licenseTermsId`: The ID of the license terms within the license template.
* `request.amount`: The amount of license tokens to mint.
* `request.receiver`: The address of the receiver.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript TypeScript
const response = await client.license.mintLicenseTokens({
  licenseTermsId: "1", 
  licensorIpId: "0xC92EC2f4c86458AFee7DD9EB5d8c57920BfCD0Ba",
  receiver: "0x14dC79964da2C08b23698B3D3cc7Ca32193d9955", 
  amount: 1,
  maxMintingFee: BigInt(0), // disabled
  maxRevenueShare: 100, // default
  txOptions: { waitForTransaction: true }
});

console.log(`License Token minted at transaction hash ${response.txHash}, License IDs: ${response.licenseTokenIds}`)
```
```typescript Request Type
export type MintLicenseTokensRequest = {
  licensorIpId: Address;
  licenseTermsId: string | number | bigint;
  licenseTemplate?: Address;
  maxMintingFee: bigint | string | number;
  maxRevenueShare: number | string;
  amount?: number | string | bigint;
  receiver?: Address;
} & WithTxOptions & WithWipOptions;
```
```typescript Response Type
export type MintLicenseTokensResponse = {
  licenseTokenIds?: bigint[];
  receipt?: TransactionReceipt;
  txHash?: string;
  encodedTxData?: EncodedTxData;
};
```

### registerPILTerms

Registers new license terms and return the ID of the newly registered license terms.

| Method             | Type                                                                 |
| ------------------ | -------------------------------------------------------------------- |
| `registerPILTerms` | `(request: RegisterPILTermsRequest) => Promise<RegisterPILResponse>` |

Parameters:

* Expected Parameters: Instead of listing all of the expected parameters here, please see `LicenseTerms` type in [this](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/resources/license.ts) file. They all come from the [PIL Terms](doc:pil-terms).
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

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
  commercialRevShare: 10, // 10%
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
  /**Percentage of revenue that must be shared with the licensor. Must be from 0-100.*/
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

### registerNonComSocialRemixingPIL

Convenient function to register a PIL non commercial social remix license to the registry.

> 🚧 No reason to call this function
>
> Non-Commercial Social Remixing terms are already registered with `licenseTermdId = 1` in our protocol. There's no reason to register them again.

| Method                            | Type                                                                                 |
| --------------------------------- | ------------------------------------------------------------------------------------ |
| `registerNonComSocialRemixingPIL` | `(request?: RegisterNonComSocialRemixingPILRequest) => Promise<RegisterPILResponse>` |

Parameters:

* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript TypeScript
const response = await client.license.registerNonComSocialRemixingPIL({
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

### registerCommercialUsePIL

Convenient function to register a PIL commercial use license to the registry.

| Method                     | Type                                                                         |
| -------------------------- | ---------------------------------------------------------------------------- |
| `registerCommercialUsePIL` | `(request: RegisterCommercialUsePILRequest) => Promise<RegisterPILResponse>` |

Parameters:

* `request.defaultMintingFee`: The fee to be paid when minting a license.
* `request.currency`: The ERC20 token to be used to pay the minting fee and the token must be registered in story protocol.
* `request.royaltyPolicyAddress`: \[Optional] The address of the royalty policy contract, default value is LAP.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

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

### registerCommercialRemixPIL

Convenient function to register a PIL commercial Remix license to the registry.

| Method                       | Type                                                                           |
| ---------------------------- | ------------------------------------------------------------------------------ |
| `registerCommercialRemixPIL` | `(request: RegisterCommercialRemixPILRequest) => Promise<RegisterPILResponse>` |

Parameters:

* `request.defaultMintingFee`: The fee to be paid when minting a license.
* `request.commercialRevShare`: Percentage of revenue that must be shared with the licensor.
* `request.currency`: The ERC20 token to be used to pay the minting fee and the token must be registered in story protocol.
* `request.royaltyPolicyAddress`: \[Optional] The address of the royalty policy contract, default value is LAP.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

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

### getLicenseTerms

Gets License Terms of the given ID.

| Method            | Type                                 |           |                                                       |
| :---------------- | :----------------------------------- | :-------- | :---------------------------------------------------- |
| `getLicenseTerms` | \`(selectedLicenseTermsId: string \\ | number \\ | bigint) => PiLicenseTemplateGetLicenseTermsResponse\` |

Parameters:

* `selectedLicenseTermsId`: The ID of the license terms.

```typescript Response Type
export type PiLicenseTemplateGetLicenseTermsResponse = {
  terms: {
    transferable: boolean;
    royaltyPolicy: Address;
    defaultMintingFee: bigint;
    expiration: bigint;
    commercialUse: boolean;
    commercialAttribution: boolean;
    commercializerChecker: Address;
    commercializerCheckerData: Hex;
    commercialRevShare: number;
    commercialRevCeiling: bigint;
    derivativesAllowed: boolean;
    derivativesAttribution: boolean;
    derivativesApproval: boolean;
    derivativesReciprocal: boolean;
    derivativeRevCeiling: bigint;
    currency: Address;
    uri: string;
  };
};
```

### predictMintingLicenseFee

Pre-compute the minting license fee for the given IP and license terms. The function can be used to calculate the minting license fee before minting license tokens.

| Method                     | Type                                                                                            |
| -------------------------- | ----------------------------------------------------------------------------------------------- |
| `predictMintingLicenseFee` | `(request: PredictMintingLicenseFeeRequest) => LicensingModulePredictMintingLicenseFeeResponse` |

Parameters:

* `request.licensorIpId`: The IP ID of the licensor.
* `request.licenseTermsId`: The ID of the license terms.
* `request.amount`: The amount of license tokens to mint.
* `request.licenseTemplate`: \[Optional] The address of the license template, default value is Programmable IP License.
* `request.receiver`: \[Optional] The address of the receiver, default value is your wallet address.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type LicensingModulePredictMintingLicenseFeeResponse = {
  currencyToken: Address;
  tokenAmount: bigint;
};
```

### setLicensingConfig

Sets the licensing configuration for a specific license terms of an IP. If both licenseTemplate and licenseTermsId are not specified then the licensing config apply to all licenses of given IP.

| Method               | Type                                                                 |
| -------------------- | -------------------------------------------------------------------- |
| `setLicensingConfig` | `(request: SetLicensingConfigRequest) => SetLicensingConfigResponse` |

Parameters:

* `request.ipId`: The address of the IP for which the configuration is being set.
* `request.licenseTermsId`: The ID of the license terms within the license template.
* `request.licenseTemplate`: The address of the license template used, If not specified, the configuration applies to all licenses.
* `request.licensingConfig`: The licensing configuration for the license.
  * `request.licensingConfig.isSet`: Whether the configuration is set or not.
  * `request.licensingConfig.mintingFee`: The minting fee to be paid when minting license tokens.
  * `request.licensingConfig.hookData`: The data to be used by the licensing hook.
  * `request.licensingConfig.licensingHook`: The hook contract address for the licensing module, or address(0) if none.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type SetLicensingConfigResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  success?: boolean;
};
```