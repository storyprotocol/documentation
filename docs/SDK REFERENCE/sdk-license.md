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

- attachLicenseTerms
- mintLicenseTokens
- registerPILTerms
- registerNonComSocialRemixingPIL
- registerCommercialUsePIL
- registerCommercialRemixPIL
- getLicenseTerms

### attachLicenseTerms

Attaches license terms to an IP.

| Method               | Type                                                                 |
| -------------------- | -------------------------------------------------------------------- |
| `attachLicenseTerms` | `(request: AttachLicenseTermsRequest) => AttachLicenseTermsResponse` |

Parameters:

- `request.ipId`: The address of the IP to which the license terms are attached.
- `request.licenseTemplate`: The address of the license template.
- `request.licenseTermsId`: The ID of the license terms.
- `request.txOptions`: [Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

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

The license terms must be attached to the IP before calling this function. But it can mint license token of default license terms without attaching the default license terms, since it is attached to all IPs by default.

IP owners can mint license tokens for their IPs for arbitrary license terms without attaching the license terms to IP.

It might require the caller pay the minting fee, depending on the license terms or configured by the iP owner. The minting fee is paid in the minting fee token specified in the license terms or configured by the IP owner. IP owners can configure the minting fee of their IPs or configure the minting fee module to determine the minting fee.

| Method              | Type                                                                        |
| ------------------- | --------------------------------------------------------------------------- |
| `mintLicenseTokens` | `(request: MintLicenseTokensRequest) => Promise<MintLicenseTokensResponse>` |

Parameters:

- `request.licensorIpId`: The licensor IP ID.
- `request.licenseTemplate`: The address of the license template.
- `request.licenseTermsId`: The ID of the license terms within the license template.
- `request.amount`: The amount of license tokens to mint.
- `request.receiver`: The address of the receiver.
- `request.txOptions`: [Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type MintLicenseTokensResponse = {
  licenseTokenIds?: bigint[];
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

> 📘 Expected Parameters
> 
> Instead of listing all of the expected parameters here, please see `LicenseTerms` type in [this](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/resources/license.ts) file.
> 
> They all come from the [PIL Terms](doc:pil-terms).

- `request.txOptions`: [Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type RegisterPILResponse = {
  licenseTermsId?: bigint;
  txHash?: string;
  encodedTxData?: EncodedTxData;
};
```

### registerNonComSocialRemixingPIL

Convenient function to register a PIL non commercial social remix license to the registry.

| Method                            | Type                                                                                 |
| --------------------------------- | ------------------------------------------------------------------------------------ |
| `registerNonComSocialRemixingPIL` | `(request?: RegisterNonComSocialRemixingPILRequest) => Promise<RegisterPILResponse>` |

Parameters:

- `request.txOptions`: [Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

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

- `request.defaultMintingFee`: The fee to be paid when minting a license.
- `request.currency`: The ERC20 token to be used to pay the minting fee and the token must be registered in story protocol.
- `request.royaltyPolicyAddress`: [Optional] The address of the royalty policy contract, default value is LAP.
- `request.txOptions`: [Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

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

- `request.defaultMintingFee`: The fee to be paid when minting a license.
- `request.commercialRevShare`: Percentage of revenue that must be shared with the licensor.
- `request.currency`: The ERC20 token to be used to pay the minting fee and the token must be registered in story protocol.
- `request.royaltyPolicyAddress`: [Optional] The address of the royalty policy contract, default value is LAP.
- `request.txOptions`: [Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type RegisterPILResponse = {
  licenseTermsId?: bigint;
  txHash?: string;
  encodedTxData?: EncodedTxData;
};
```

### getLicenseTerms

Gets License Terms of the given ID.

| Method            | Type                                                                                               |
| ----------------- | -------------------------------------------------------------------------------------------------- |
| `getLicenseTerms` | `(selectedLicenseTermsId: string \| number \| bigint) => PiLicenseTemplateGetLicenseTermsResponse` |

Parameters:

- `selectedLicenseTermsId`: The ID of the license terms.

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

- `request.licensorIpId`: The IP ID of the licensor.
- `request.licenseTermsId`: The ID of the license terms.
- `request.amount`: The amount of license tokens to mint.
- `request.licenseTemplate`: [Optional] The address of the license template, default value is Programmable IP License.
- `request.receiver`: [Optional] The address of the receiver, default value is your wallet address.
- `request.txOptions`: [Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

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

- `request.ipId`: The address of the IP for which the configuration is being set.
- `request.licenseTermsId`: The ID of the license terms within the license template.
- `request.licenseTemplate`: The address of the license template used, If not specified, the configuration applies to all licenses.
- `request.licensingConfig`: The licensing configuration for the license.
  - `request.licensingConfig.isSet`: Whether the configuration is set or not.
  - `request.licensingConfig.mintingFee`: The minting fee to be paid when minting license tokens.
  - `request.licensingConfig.hookData`: The data to be used by the licensing hook.
  - `request.licensingConfig.licensingHook`: The hook contract address for the licensing module, or address(0) if none.
- `request.txOptions`: [Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type SetLicensingConfigResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  success?: boolean;
};
```