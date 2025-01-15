---
title: IP Asset
excerpt: IPAssetClient allows you to create, get, and list IP Assets within Story.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
## IPAssetClient

### Methods

* register
* registerDerivative
* registerDerivativeWithLicenseTokens
* mintAndRegisterIpAssetWithPilTerms
* registerIpAndAttachPilTerms
* registerDerivativeIp
* mintAndRegisterIpAndMakeDerivative
* mintAndRegisterIp
* registerPilTermsAndAttach
* mintAndRegisterIpAndMakeDerivativeWithLicenseTokens
* registerIpAndMakeDerivativeWithLicenseTokens

### register

Registers an NFT as IP, creating a corresponding IP record.

| Method     | Type                                                        |
| ---------- | ----------------------------------------------------------- |
| `register` | `(request: RegisterRequest) => Promise<RegisterIpResponse>` |

Parameters:

* `request.nftContract`: The address of the NFT.
* `request.tokenId`: The token identifier of the NFT.
* `request.ipMetadata`: \[Optional] The desired metadata for the newly minted NFT and newly registered IP.
  * `request.ipMetadata.ipMetadataURI` \[Optional] The URI of the metadata for the IP.
  * `request.ipMetadata.ipMetadataHash` \[Optional] The hash of the metadata for the IP.
  * `request.ipMetadata.nftMetadataURI` \[Optional] The URI of the metadata for the NFT.
  * `request.ipMetadata.nftMetadataHash` \[Optional] The hash of the metadata for the IP NFT.
* `request.deadline`: \[Optional]The deadline for the signature in milliseconds.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type RegisterIpResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  ipId?: Address;
};
```

### registerDerivative

Registers a derivative directly with parent IP's license terms, without needing license tokens, and attaches the license terms of the parent IPs to the derivative IP.

The license terms must be attached to the parent IP before calling this function.

All IPs attached default license terms by default.

The derivative IP owner must be the caller or an authorized operator.

| Method               | Type                                                                          |
| -------------------- | ----------------------------------------------------------------------------- |
| `registerDerivative` | `(request: RegisterDerivativeRequest) => Promise<RegisterDerivativeResponse>` |

Parameters:

* `request.childIpId`: The derivative IP ID.
* `request.parentIpIds`: The parent IP IDs.
* `request.licenseTermsIds`: The IDs of the license terms that the parent IP supports.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type RegisterDerivativeResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  childIpId?: Address;
};
```

### registerDerivativeWithLicenseTokens

Registers a derivative with license tokens.

The derivative IP is registered with license tokens minted from the parent IP's license terms.

The license terms of the parent IPs issued with license tokens are attached to the derivative IP.

The caller must be the derivative IP owner or an authorized operator.

| Method                                | Type                                                                                                            |
| ------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| `registerDerivativeWithLicenseTokens` | `(request: RegisterDerivativeWithLicenseTokensRequest) => Promise<RegisterDerivativeWithLicenseTokensResponse>` |

Parameters:

* `request.childIpId`: The derivative IP ID.
* `request.licenseTokenIds`: The IDs of the license tokens.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type RegisterDerivativeWithLicenseTokensResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
};
```

### mintAndRegisterIpAssetWithPilTerms

Mint an NFT from a collection, register it as an IP, attach metadata to the IP, and atach License Terms to the IP all in one function.

| Method                               | Type                                                                                                          |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------- |
| `mintAndRegisterIpAssetWithPilTerms` | `(request: MintAndRegisterIpAssetWithPilTermsRequest) => Promise<MintAndRegisterIpAssetWithPilTermsResponse>` |

Parameters:

* `request.spgNftContract`: The address of the NFT collection.
* `request.terms[]`: The array of license terms to be attached.
  * `request.terms.transferable`: Indicates whether the license is transferable or not.
  * `request.terms.royaltyPolicy`: The address of the royalty policy contract which required to StoryProtocol in advance.
  * `request.terms.mintingFee`: The fee to be paid when minting a license.
  * `request.terms.expiration`: The expiration period of the license.
  * `request.terms.commercialUse`: Indicates whether the work can be used commercially or not.
  * `request.terms.commercialAttribution`: Whether attribution is required when reproducing the work commercially or not.
  * `request.terms.commercializerChecker`: Commercializers that are allowed to commercially exploit the work. If zero address, then no restrictions is enforced.
  * `request.terms.commercializerCheckerData`: The data to be passed to the commercializer checker contract.
  * `request.terms.commercialRevShare`: Percentage of revenue that must be shared with the licensor.
  * `request.terms.commercialRevCeiling`: The maximum revenue that can be generated from the commercial use of the work.
  * `request.terms.derivativesAllowed`: Indicates whether the licensee can create derivatives of his work or not.
  * `request.terms.derivativesAttribution`: Indicates whether attribution is required for derivatives of the work or not.
  * `request.terms.derivativesApproval`: Indicates whether the licensor must approve derivatives of the work before they can be linked to the licensor IP ID or not.
  * `request.terms.derivativesReciprocal`: Indicates whether the licensee must license derivatives of the work under the same terms or not.
  * `request.terms.derivativeRevCeiling`: The maximum revenue that can be generated from the derivative use of the work.
  * `request.terms.currency`: The ERC20 token to be used to pay the minting fee. the token must be registered in Story Protocol.
  * `request.terms.uri`: The URI of the license terms, which can be used to fetch the offchain license terms.
* `request.ipMetadata`: \[Optional] The desired metadata for the newly minted NFT and newly registered IP.
  * `request.ipMetadata.ipMetadataURI`: \[Optional] The URI of the metadata for the IP.
  * `request.ipMetadata.ipMetadataHash`: \[Optional] The hash of the metadata for the IP.
  * `request.ipMetadata.nftMetadataURI`: \[Optional] The URI of the metadata for the NFT.
  * `request.ipMetadata.nftMetadataHash`: \[Optional] The hash of the metadata for the IP NFT.
* `request.recipient`: \[Optional] The address of the recipient of the minted NFT.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type MintAndRegisterIpAssetWithPilTermsResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  ipId?: Address;
  tokenId?: bigint;
  licenseTermsIds?: bigint[];
};
```

### batchMintAndRegisterIpAssetWithPilTerms

Batch mint an NFT from a collection and register it as an IP.

| Method                                    | Type                                                                                                                    |
| ----------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| `batchMintAndRegisterIpAssetWithPilTerms` | `(request: BatchMintAndRegisterIpAssetWithPilTermsRequest) => Promise<BatchMintAndRegisterIpAssetWithPilTermsResponse>` |

Parameters:

* `request.args[]`: The array of mint and register IP requests.
  * `request.args.spgNftContract`: The address of the NFT collection.
  * `request.args.terms[]`: The array of license terms to be attached.
    * `request.args.terms.transferable`: Indicates whether the license is transferable or not.
    * `request.args.terms.royaltyPolicy`: The address of the royalty policy contract which required to StoryProtocol in advance.
    * `request.args.terms.mintingFee`: The fee to be paid when minting a license.
    * `request.args.terms.expiration`: The expiration period of the license.
    * `request.args.terms.commercialUse`: Indicates whether the work can be used commercially or not.
    * `request.args.terms.commercialAttribution`: Whether attribution is required when reproducing the work commercially or not.
    * `request.args.terms.commercializerChecker`: Commercializers that are allowed to commercially exploit the work. If zero address, then no restrictions is enforced.
    * `request.args.terms.commercializerCheckerData`: The data to be passed to the commercializer checker contract.
    * `request.args.terms.commercialRevShare`: Percentage of revenue that must be shared with the licensor.
    * `request.args.terms.commercialRevCeiling`: The maximum revenue that can be generated from the commercial use of the work.
    * `request.args.terms.derivativesAllowed`: Indicates whether the licensee can create derivatives of his work or not.
    * `request.args.terms.derivativesAttribution`: Indicates whether attribution is required for derivatives of the work or not.
    * `request.args.terms.derivativesApproval`: Indicates whether the licensor must approve derivatives of the work before they can be linked to the licensor IP ID or not.
    * `request.args.terms.derivativesReciprocal`: Indicates whether the licensee must license derivatives of the work under the same terms or not.
    * `request.args.terms.derivativeRevCeiling`: The maximum revenue that can be generated from the derivative use of the work.
    * `request.args.terms.currency`: The ERC20 token to be used to pay the minting fee. the token must be registered in story protocol.
    * `request.args.terms.uri`: The URI of the license terms, which can be used to fetch the offchain license terms.
  * `request.args.ipMetadata`: \[Optional] The desired metadata for the newly minted NFT and newly registered IP.
    * `request.args.ipMetadata.ipMetadataURI`: \[Optional] The URI of the metadata for the IP.
    * `request.args.ipMetadata.ipMetadataHash`: \[Optional] The hash of the metadata for the IP.
    * `request.args.ipMetadata.nftMetadataURI`: \[Optional] The URI of the metadata for the NFT.
    * `request.args.ipMetadata.nftMetadataHash`: \[Optional] The hash of the metadata for the IP NFT.
  * `request.args.recipient`: \[Optional] The address of the recipient of the minted NFT.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type BatchMintAndRegisterIpAssetWithPilTermsResponse = {
  txHash: Hex;
  results?: BatchMintAndRegisterIpAssetWithPilTermsResult[];
};

export type BatchMintAndRegisterIpAssetWithPilTermsResult = {
  ipId: Address;
  tokenId: bigint;
  licenseTermsIds: bigint[];
  spgNftContract: Address;
};
```

### registerIpAndAttachPilTerms

Register a given NFT as an IP, attach metadata to the IP, and attach License Terms to the IP all in one function.

| Method                        | Type                                                                                            |
| ----------------------------- | ----------------------------------------------------------------------------------------------- |
| `registerIpAndAttachPilTerms` | `(request: RegisterIpAndAttachPilTermsRequest) => Promise<RegisterIpAndAttachPilTermsResponse>` |

Parameters:

* `request.nftContract`: The address of the NFT collection.
* `request.tokenId`:  The ID of the NFT.
* `request.terms[]`: The array of license terms to be attached.
  * `request.terms.transferable`: Indicates whether the license is transferable or not.
  * `request.terms.royaltyPolicy`: The address of the royalty policy contract which required to StoryProtocol in advance.
  * `request.terms.mintingFee`: The fee to be paid when minting a license.
  * `request.terms.expiration`: The expiration period of the license.
  * `request.terms.commercialUse`: Indicates whether the work can be used commercially or not.
  * `request.terms.commercialAttribution`: Whether attribution is required when reproducing the work commercially or not.
  * `request.terms.commercializerChecker`: Commercializers that are allowed to commercially exploit the work. If zero address, then no restrictions is enforced.
  * `request.terms.commercializerCheckerData`: The data to be passed to the commercializer checker contract.
  * `request.terms.commercialRevShare`: Percentage of revenue that must be shared with the licensor.
  * `request.terms.commercialRevCeiling`: The maximum revenue that can be generated from the commercial use of the work.
  * `request.terms.derivativesAllowed`: Indicates whether the licensee can create derivatives of his work or not.
  * `request.terms.derivativesAttribution`: Indicates whether attribution is required for derivatives of the work or not.
  * `request.terms.derivativesApproval`: Indicates whether the licensor must approve derivatives of the work before they can be linked to the licensor IP ID or not.
  * `request.terms.derivativesReciprocal`: Indicates whether the licensee must license derivatives of the work under the same terms or not.
  * `request.terms.derivativeRevCeiling`: The maximum revenue that can be generated from the derivative use of the work.
  * `request.terms.currency`: The ERC20 token to be used to pay the minting fee. the token must be registered in story protocol.
  * `request.terms.uri`: The URI of the license terms, which can be used to fetch the offchain license terms.
* `request.ipMetadata`: \[Optional] The desired metadata for the newly minted NFT and newly registered IP.
  * `request.ipMetadata.ipMetadataURI`: \[Optional] The URI of the metadata for the IP.
  * `request.ipMetadata.ipMetadataHash`: \[Optional] The hash of the metadata for the IP.
  * `request.ipMetadata.nftMetadataURI`: \[Optional] The URI of the metadata for the NFT.
  * `request.ipMetadata.nftMetadataHash`: \[Optional] The hash of the metadata for the IP NFT.
* `request.deadline`: \[Optional]The deadline for the signature in milliseconds.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type RegisterIpAndAttachPilTermsResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  ipId?: Address;
  licenseTermsIds?: bigint[];
};
```

### registerDerivativeIp

Register an NFT as IP and then link it as a derivative of another IP Asset without using license tokens.

| Method                 | Type                                                                                            |
| ---------------------- | ----------------------------------------------------------------------------------------------- |
| `registerDerivativeIp` | `(request: RegisterIpAndMakeDerivativeRequest) => Promise<RegisterIpAndMakeDerivativeResponse>` |

Parameters:

* `request.nftContract`: The address of the NFT collection.
* `request.tokenId`:  The ID of the NFT.
* `request.derivData`: The derivative data to be used for registerDerivative.
  * `request.derivData.parentIpIds`: The IDs of the parent IPs to link the registered derivative IP.
  * `request.derivData.licenseTermsIds`: The IDs of the license terms to be used for the linking.
  * `request.derivData.licenseTemplate`: \[Optional] The address of the license template to be used for the linking.
* `request.ipMetadata`: \[Optional] The desired metadata for the newly minted NFT and newly registered IP.
  * `request.ipMetadata.ipMetadataURI` \[Optional] The URI of the metadata for the IP.
  * `request.ipMetadata.ipMetadataHash` \[Optional] The hash of the metadata for the IP.
  * `request.ipMetadata.nftMetadataURI` \[Optional] The URI of the metadata for the NFT.
  * `request.ipMetadata.nftMetadataHash` \[Optional] The hash of the metadata for the IP NFT.
* `request.deadline`: \[Optional]The deadline for the signature in milliseconds.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type RegisterIpAndMakeDerivativeResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  ipId?: Address;
};
```

### mintAndRegisterIpAndMakeDerivative

Mint an NFT from a collection and register it as a derivative IP without license tokens.

| Method                               | Type                                                                                          |
| ------------------------------------ | --------------------------------------------------------------------------------------------- |
| `mintAndRegisterIpAndMakeDerivative` | `(request: MintAndRegisterIpAndMakeDerivativeRequest) => Promise<RegisterDerivativeResponse>` |

Parameters:

* `request.spgNftContract`: The address of the NFT collection.
* `request.derivData`: The derivative data to be used for registerDerivative.
  * `request.derivData.parentIpIds`: The IDs of the parent IPs to link the registered derivative IP.
  * `request.derivData.licenseTermsIds`: The IDs of the license terms to be used for the linking.
  * `request.derivData.licenseTemplate`: \[Optional] The address of the license template to be used for the linking.
* `request.ipMetadata`: \[Optional] The desired metadata for the newly minted NFT and newly registered IP.
  * `request.ipMetadata.ipMetadataURI` \[Optional] The URI of the metadata for the IP.
  * `request.ipMetadata.ipMetadataHash` \[Optional] The hash of the metadata for the IP.
  * `request.ipMetadata.nftMetadataURI` \[Optional] The URI of the metadata for the NFT.
  * `request.ipMetadata.nftMetadataHash` \[Optional] The hash of the metadata for the IP NFT.
* `request.recipient`: \[Optional] The address of the recipient of the minted NFT, default value is your wallet address.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type RegisterDerivativeResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  childIpId?: Address;
  tokenId?: bigint;
};
```

### mintAndRegisterIp

Mint an NFT from a SPGNFT collection and register it with metadata as an IP.

| Method              | Type                                                                 |
| ------------------- | -------------------------------------------------------------------- |
| `mintAndRegisterIp` | `(request: MintAndRegisterIpRequest) => Promise<RegisterIpResponse>` |

Parameters:

* `request.spgNftContract`: The address of the NFT collection.
* `request.recipient`: \[Optional] The address of the recipient of the minted NFT, default value is your wallet address.
* `request.ipMetadata`: \[Optional] The desired metadata for the newly minted NFT and newly registered IP.
  * `request.ipMetadata.ipMetadataURI` \[Optional] The URI of the metadata for the IP.
  * `request.ipMetadata.ipMetadataHash` \[Optional] The hash of the metadata for the IP.
  * `request.ipMetadata.nftMetadataURI` \[Optional] The URI of the metadata for the NFT.
  * `request.ipMetadata.nftMetadataHash` \[Optional] The hash of the metadata for the IP NFT.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type RegisterIpResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  ipId?: Address;
};
```

### registerPilTermsAndAttach

Register Programmable IP License Terms (if unregistered) and attach it to IP.

| Method                      | Type                                                                                        |
| --------------------------- | ------------------------------------------------------------------------------------------- |
| `registerPilTermsAndAttach` | `(request: RegisterPilTermsAndAttachRequest) => Promise<RegisterPilTermsAndAttachResponse>` |

Parameters:

* `request.ipId`: The ID of the IP.
* `request.terms[]`: The array of license terms to be attached.
  * `request.terms.transferable`: Indicates whether the license is transferable or not.
  * `request.terms.royaltyPolicy`: The address of the royalty policy contract which required to StoryProtocol in advance.
  * `request.terms.mintingFee`: The fee to be paid when minting a license.
  * `request.terms.expiration`: The expiration period of the license.
  * `request.terms.commercialUse`: Indicates whether the work can be used commercially or not.
  * `request.terms.commercialAttribution`: Whether attribution is required when reproducing the work commercially or not.
  * `request.terms.commercializerChecker`: Commercializers that are allowed to commercially exploit the work. If zero address, then no restrictions is enforced.
  * `request.terms.commercializerCheckerData`: The data to be passed to the commercializer checker contract.
  * `request.terms.commercialRevShare`: Percentage of revenue that must be shared with the licensor.
  * `request.terms.commercialRevCeiling`: The maximum revenue that can be generated from the commercial use of the work.
  * `request.terms.derivativesAllowed`: Indicates whether the licensee can create derivatives of his work or not.
  * `request.terms.derivativesAttribution`: Indicates whether attribution is required for derivatives of the work or not.
  * `request.terms.derivativesApproval`: Indicates whether the licensor must approve derivatives of the work before they can be linked to the licensor IP ID or not.
  * `request.terms.derivativesReciprocal`: Indicates whether the licensee must license derivatives of the work under the same terms or not.
  * `request.terms.derivativeRevCeiling`: The maximum revenue that can be generated from the derivative use of the work.
  * `request.terms.currency`: The ERC20 token to be used to pay the minting fee. the token must be registered in story protocol.
  * `request.terms.uri`: The URI of the license terms, which can be used to fetch the offchain license terms.
* `request.deadline`: \[Optional] The deadline for the signature in milliseconds, default is 1000s.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type RegisterPilTermsAndAttachResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  licenseTermsIds?: bigint[];
};
```

### mintAndRegisterIpAndMakeDerivativeWithLicenseTokens

Mint an NFT from a collection and register it as a derivative IP using license tokens

| Method                                                | Type                                                                                                   |
| ----------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| `mintAndRegisterIpAndMakeDerivativeWithLicenseTokens` | `(request: MintAndRegisterIpAndMakeDerivativeWithLicenseTokensRequest) => Promise<RegisterIpResponse>` |

Parameters:

* `request.spgNftContract`: The address of the NFT collection.
* `request.licenseTokenIds`: The IDs of the license tokens to be burned for linking the IP to parent IPs.
* `request.ipMetadata`: \[Optional] The desired metadata for the newly minted NFT and newly registered IP.
  * `request.ipMetadata.ipMetadataURI` \[Optional] The URI of the metadata for the IP.
  * `request.ipMetadata.ipMetadataHash` \[Optional] The hash of the metadata for the IP.
  * `request.ipMetadata.nftMetadataURI` \[Optional] The URI of the metadata for the NFT.
  * `request.ipMetadata.nftMetadataHash` \[Optional] The hash of the metadata for the IP NFT.
* `request.recipient`: \[Optional] The address to receive the minted NFT, default value is your wallet address.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type RegisterIpResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  ipId?: Address;
};
```

### registerIpAndMakeDerivativeWithLicenseTokens

Register the given NFT as a derivative IP using license tokens.

| Method                                         | Type                                                                                            |
| ---------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| `registerIpAndMakeDerivativeWithLicenseTokens` | `(request: RegisterIpAndMakeDerivativeWithLicenseTokensRequest) => Promise<RegisterIpResponse>` |

Parameters:

* `request.nftContract`: The address of the NFT collection.
* `request.tokenId`: The ID of the NFT.
* `request.licenseTokenIds`: The IDs of the license tokens to be burned for linking the IP to parent IPs.
* `request.ipMetadata`: \[Optional] The desired metadata for the newly minted NFT and newly registered IP.
  * `request.ipMetadata.ipMetadataURI` \[Optional] The URI of the metadata for the IP.
  * `request.ipMetadata.ipMetadataHash` \[Optional] The hash of the metadata for the IP.
  * `request.ipMetadata.nftMetadataURI` \[Optional] The URI of the metadata for the NFT.
  * `request.ipMetadata.nftMetadataHash` \[Optional] The hash of the metadata for the IP NFT.
* `request.deadline`: \[Optional] The deadline for the signature in milliseconds, default is 1000ms.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type RegisterIpResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  ipId?: Address;
};
```
