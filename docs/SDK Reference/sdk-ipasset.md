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

### Navigating Around the IPAssetClient

Because there are a lot of functions to interact with the [📜 Licensing Module](doc:licensing-module), we have broken them down into a helpful chart so you can identify what you're looking for, and then find the associated docs.

| **Function**                                                                                | **Mint an NFT** | **Register IPA** | **Create License Terms** | **Attach License Terms** | **Mint License Token** | **Register as Derivative** |
| ------------------------------------------------------------------------------------------- | :-------------: | :--------------: | :----------------------: | :----------------------: | :--------------------: | :------------------------: |
| <span style={{color: "#e03130"}}>register</span>                                            |                 |         ✓        |                          |                          |                        |                            |
| <span style={{color: "#e03130"}}>mintAndRegisterIp</span>                                   |        ✓        |         ✓        |                          |                          |                        |                            |
| <span style={{color: "#e03130"}}>registerIpAndAttachPilTerms</span>                         |                 |         ✓        |             ✓            |             ✓            |                        |                            |
| <span style={{color: "#e03130"}}>mintAndRegisterIpAssetWithPilTerms</span>                  |        ✓        |         ✓        |             ✓            |             ✓            |                        |                            |
| <span style={{color: "#e03130"}}>registerDerivativeIp</span>                                |                 |         ✓        |                          |                          |                        |              ✓             |
| <span style={{color: "#e03130"}}>mintAndRegisterIpAndMakeDerivativeWithLicenseTokens</span> |        ✓        |         ✓        |                          |                          |                        |              ✓             |
| <span style={{color: "#e03130"}}>registerIpAndMakeDerivativeWithLicenseTokens</span>        |                 |         ✓        |                          |                          |                        |              ✓             |
| <span style={{color: "#e03130"}}>mintAndRegisterIpAndMakeDerivative</span>                  |        ✓        |         ✓        |                          |                          |                        |              ✓             |
| <span style={{color: "#e03130"}}>registerDerivative</span>                                  |                 |                  |                          |                          |                        |              ✓             |
| <span style={{color: "#e03130"}}>registerDerivativeWithLicenseTokens</span>                 |                 |                  |                          |                          |                        |              ✓             |
| <span style={{color: "#e03130"}}>registerPilTermsAndAttach</span>                           |                 |                  |             ✓            |             ✓            |                        |                            |
| <span style={{color: "#1971c2"}}>registerPILTerms</span>                                    |                 |                  |             ✓            |                          |                        |                            |
| <span style={{color: "#1971c2"}}>attachLicenseTerms</span>                                  |                 |                  |                          |             ✓            |                        |                            |
| <span style={{color: "#1971c2"}}>mintLicenseTokens</span>                                   |                 |                  |                          |                          |            ✓           |                            |

* <span style={{color: "#e03130"}}>Red</span>: IPAssetClient (this page)
* <span style={{color: "#1971c2"}}>Blue</span>: [LicenseClient](doc:sdk-license)

## register

Registers an NFT as IP, creating a corresponding [🧩 IP Asset](doc:ip-asset). If the given NFT was already registered, this function will return the existing `ipId`.

> 📘 NFT Metadata
>
> Note that this function will also set the underlying NFT's `tokenUri` to whatever is passed under `ipMetadata.nftMetadataURI`.

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

```typescript TypeScript
import { toHex } from 'viem';

const response = await client.ipAsset.register({
  nftContract: "0x041B4F29183317Fd352AE57e331154b73F8a1D73",
  tokenId: "12",
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
```python Python
metadata = {
  'ip_metadata_uri': "test-uri",
  'ip_metadata_hash': web3.to_hex(web3.keccak(text="test-ip-metadata-hash")),
  'nft_metadata_uri': "test-uri",
  'nft_metadata_hash': web3.to_hex(web3.keccak(text="test-nft-metadata-hash"))
}

response = story_client.IPAsset.register(
  nft_contract="0x041B4F29183317Fd352AE57e331154b73F8a1D73",
  token_id="12",
  ip_metadata=metadata
)
```
```typescript Request Type
export type RegisterRequest = {
  nftContract: Address;
  tokenId: string | number | bigint;
  deadline?: string | number | bigint;
} & IpMetadataAndTxOptions;
```
```typescript Response Type
export type RegisterIpResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  ipId?: Address;
};
```

## batchRegister

Batch registers an NFT as IP, creating a corresponding IP record.

| Method          | Type                                                                |
| --------------- | ------------------------------------------------------------------- |
| `batchRegister` | `(request: BatchRegisterRequest) => Promise<BatchRegisterResponse>` |

## registerDerivative

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
* `request.maxMintingFee`: The maximum minting fee that the caller is willing to pay. If set to 0, then there is no no limit. **Recommended for simplicity: 0**
* `request.maxRevenueShare`: The maximum revenue share percentage agreed upon between a child and parent when a child is registering as derivative. Must be between 0 and 100. **Recommended for simplicity: 100**
* `request.maxRts`: The maximum number of royalty tokens that can be distributed to the external royalty policies. Must be between 0 and 100,000,000. **Recommended for simplicity: 100\_000\_000**
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript TypeScript
const response = await client.ipAsset.registerDerivative({
  childIpId: "0xC92EC2f4c86458AFee7DD9EB5d8c57920BfCD0Ba",
  parentIpIds: ["0xC92EC2f4c86458AFee7DD9EB5d8c57920BfCD0Ba"],
  licenseTermsIds: ["5"],
  maxMintingFee: BigInt(0), // disabled
  maxRts: 100_000_000, // default
  maxRevenueShare: 100, // default
  txOptions: { waitForTransaction: true }
});

console.log(`Derivative IPA linked to parent at transaction hash ${response.txHash}`)
```
```typescript Request Type
export type RegisterDerivativeRequest = {
  txOptions?: TxOptions;
  childIpId: Address;
} & DerivativeData;

export type DerivativeData = {
  parentIpIds: Address[];
  licenseTermsIds: bigint[] | string[] | number[];
  maxMintingFee: bigint | string | number;
  maxRts: number | string;
  maxRevenueShare: number | string;
  licenseTemplate?: Address;
};
```
```typescript Response Type
export type RegisterDerivativeResponse = {
  txHash?: Hex;
  encodedTxData?: EncodedTxData;
};
```

## registerDerivativeWithLicenseTokens

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
* `request.maxRts`: The maximum number of royalty tokens that can be distributed to the external royalty policies. Must be between 0 and 100,000,000. **Recommended for simplicity: 100\_000\_000**
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

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
  maxRts: number | string;
  txOptions?: TxOptions;
};
```
```typescript Response Type
export type RegisterDerivativeWithLicenseTokensResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
};
```

## mintAndRegisterIpAssetWithPilTerms

Mint an NFT from a collection, register it as an IP, attach metadata to the IP, and attach License Terms to the IP all in one function.

> 📘 NFT Metadata
>
> Note that this function will also set the underlying NFT's `tokenUri` to whatever is passed under `ipMetadata.nftMetadataURI`.

| Method                               | Type                                                                                                          |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------- |
| `mintAndRegisterIpAssetWithPilTerms` | `(request: MintAndRegisterIpAssetWithPilTermsRequest) => Promise<MintAndRegisterIpAssetWithPilTermsResponse>` |

Parameters:

* `request.spgNftContract`: The address of the NFT collection.
* `request.allowDuplicates`: Set to true to allow minting IPs with the same NFT metadata.
* `request.licenseTermsData[]`: The array of license terms to be attached. :warning: **This function will fail if you pass in an empty array.**
  * `request.licenseTermsData.terms`: See the [LicenseTerms type](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/resources/license.ts#L26).
  * `request.licenseTermsData.licensingConfig`: See the [LicensingConfig type](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/common.ts#L15).
* `request.ipMetadata`: \[Optional] The desired metadata for the newly minted NFT and newly registered IP.
  * `request.ipMetadata.ipMetadataURI`: \[Optional] The URI of the metadata for the IP.
  * `request.ipMetadata.ipMetadataHash`: \[Optional] The hash of the metadata for the IP.
  * `request.ipMetadata.nftMetadataURI`: \[Optional] The URI of the metadata for the NFT.
  * `request.ipMetadata.nftMetadataHash`: \[Optional] The hash of the metadata for the IP NFT.
* `request.recipient`: \[Optional] The address of the recipient of the minted NFT.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

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
  currency: '0x1514000000000000000000000000000000000000', // insert $WIP address from https://docs.story.foundation/docs/deployed-smart-contracts
  uri: '',
}

const licensingConfig: LicensingConfig = {
  isSet: false,
  mintingFee: BigInt(0),
  licensingHook: zeroAddress,
  hookData: zeroHash,
  commercialRevShare: 0,
  disabled: false,
  expectMinimumGroupRewardShare: 0,
  expectGroupRewardPool: zeroAddress,
};

const response = await client.ipAsset.mintAndRegisterIpAssetWithPilTerms({
  spgNftContract: '0xfE265a91dBe911db06999019228a678b86C04959',
  licenseTermsData: [{ terms: commercialRemixTerms, licensingConfig }], // IP already has non-commercial social remixing terms. You can add more here.
  // set to true to mint ip with same nft metadata
  allowDuplicates: true,
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
  allowDuplicates: boolean;
  licenseTermsData: LicenseTermsData<RegisterPILTermsRequest, LicensingConfig>[];
  recipient?: Address;
  royaltyPolicyAddress?: Address;
} & IpMetadataAndTxOptions & WithWipOptions;
```
```typescript Response Type
export type MintAndRegisterIpAssetWithPilTermsResponse = {
  txHash?: Hex;
  encodedTxData?: EncodedTxData;
  ipId?: Address;
  tokenId?: bigint;
  receipt?: TransactionReceipt;
  licenseTermsIds?: bigint[];
};
```

## batchMintAndRegisterIpAssetWithPilTerms

Batch mint an NFT from a collection and register it as an IP.

| Method                                    | Type                                                                                                                    |
| ----------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| `batchMintAndRegisterIpAssetWithPilTerms` | `(request: BatchMintAndRegisterIpAssetWithPilTermsRequest) => Promise<BatchMintAndRegisterIpAssetWithPilTermsResponse>` |

## registerIpAndAttachPilTerms

Register a given NFT as an IP, attach metadata to the IP, and attach License Terms to the IP all in one function.

> 📘 NFT Metadata
>
> Note that this function will also set the underlying NFT's `tokenUri` to whatever is passed under `ipMetadata.nftMetadataURI`.

| Method                        | Type                                                                                            |
| ----------------------------- | ----------------------------------------------------------------------------------------------- |
| `registerIpAndAttachPilTerms` | `(request: RegisterIpAndAttachPilTermsRequest) => Promise<RegisterIpAndAttachPilTermsResponse>` |

Parameters:

* `request.nftContract`: The address of the NFT collection.
* `request.tokenId`:  The ID of the NFT.
* `request.licenseTermsData[]`: The array of license terms to be attached. :warning: **This function will fail if you pass in an empty array.**
  * `request.licenseTermsData.terms`: See the [LicenseTerms type](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/resources/license.ts#L26).
  * `request.licenseTermsData.licensingConfig`: See the [LicensingConfig type](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/common.ts#L15).
* `request.ipMetadata`: \[Optional] The desired metadata for the newly minted NFT and newly registered IP.
  * `request.ipMetadata.ipMetadataURI`: \[Optional] The URI of the metadata for the IP.
  * `request.ipMetadata.ipMetadataHash`: \[Optional] The hash of the metadata for the IP.
  * `request.ipMetadata.nftMetadataURI`: \[Optional] The URI of the metadata for the NFT.
  * `request.ipMetadata.nftMetadataHash`: \[Optional] The hash of the metadata for the IP NFT.
* `request.deadline`: \[Optional]The deadline for the signature in milliseconds.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

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
  currency: '0x1514000000000000000000000000000000000000', // insert $WIP address from https://docs.story.foundation/docs/deployed-smart-contracts
  uri: '',
}

const licensingConfig: LicensingConfig = {
  isSet: false,
  mintingFee: BigInt(0),
  licensingHook: zeroAddress,
  hookData: zeroHash,
  commercialRevShare: 0,
  disabled: false,
  expectMinimumGroupRewardShare: 0,
  expectGroupRewardPool: zeroAddress,
};

const response = await client.ipAsset.registerIpAndAttachPilTerms({
  nftContract: '0x041B4F29183317Fd352AE57e331154b73F8a1D73',
  tokenId: '12',
  licenseTermsData: [{ terms: commercialRemixTerms, licensingConfig }], // IP already has non-commercial social remixing terms. You can add more here.
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
  licenseTermsData: LicenseTermsData<RegisterPILTermsRequest, LicensingConfig>[];
  deadline?: bigint | number | string;
} & IpMetadataAndTxOptions;
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

## registerDerivativeIp

Register an NFT as IP and then link it as a derivative of another IP Asset without using license tokens.

> 📘 NFT Metadata
>
> Note that this function will also set the underlying NFT's `tokenUri` to whatever is passed under `ipMetadata.nftMetadataURI`.

| Method                 | Type                                                                                            |
| ---------------------- | ----------------------------------------------------------------------------------------------- |
| `registerDerivativeIp` | `(request: RegisterIpAndMakeDerivativeRequest) => Promise<RegisterIpAndMakeDerivativeResponse>` |

Parameters:

* `request.nftContract`: The address of the NFT collection.
* `request.tokenId`:  The ID of the NFT.
* `request.derivData`: The derivative data to be used for registerDerivative.
  * `request.derivData.parentIpIds`: The IDs of the parent IPs to link the registered derivative IP.
  * `request.derivData.licenseTermsIds`: The IDs of the license terms to be used for the linking.
  * `request.derivData.maxMintingFee`: The maximum minting fee that the caller is willing to pay. If set to 0, then there is no no limit. **Recommended for simplicity: 0**
  * `request.derivData.maxRevenueShare`: The maximum revenue share percentage agreed upon between a child and parent when a child is registering as derivative. Must be between 0 and 100. **Recommended for simplicity: 100**
  * `request.derivData.maxRts`: The maximum number of royalty tokens that can be distributed to the external royalty policies. Must be between 0 and 100,000,000. **Recommended for simplicity: 100\_000\_000**
  * `request.derivData.licenseTemplate`: \[Optional] The address of the license template to be used for the linking.
* `request.ipMetadata`: \[Optional] The desired metadata for the newly minted NFT and newly registered IP.
  * `request.ipMetadata.ipMetadataURI` \[Optional] The URI of the metadata for the IP.
  * `request.ipMetadata.ipMetadataHash` \[Optional] The hash of the metadata for the IP.
  * `request.ipMetadata.nftMetadataURI` \[Optional] The URI of the metadata for the NFT.
  * `request.ipMetadata.nftMetadataHash` \[Optional] The hash of the metadata for the IP NFT.
* `request.deadline`: \[Optional]The deadline for the signature in milliseconds.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript TypeScript
import { toHex } from 'viem';

const derivData: DerivativeData = {
  parentIpIds: ["0xd142822Dc1674154EaF4DDF38bbF7EF8f0D8ECe4"],
  licenseTermsIds: ["1"],
  maxMintingFee: BitInt(0), // disabled
  maxRts: 100_000_000, // default
  maxRevenueShare: 100, // default
};

const response = await client.ipAsset.registerDerivativeIp({
  nftContract: "0x041B4F29183317Fd352AE57e331154b73F8a1D73", // your NFT contract address
  tokenId: '127',
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

console.log(`Completed at transaction hash ${response.txHash}, IPA ID: ${response.ipId}`);
```
```typescript Request Type
export type RegisterIpAndMakeDerivativeRequest = {
  nftContract: Address;
  tokenId: string | number | bigint;
  deadline?: string | number | bigint;
  derivData: DerivativeData;
  sigMetadataAndRegister?: {
    signer: Address;
    deadline: bigint | string | number;
    signature: Hex;
  };
} & IpMetadataAndTxOptions & WithWipOptions;

export type DerivativeData = {
  parentIpIds: Address[];
  licenseTermsIds: bigint[] | string[] | number[];
  maxMintingFee: bigint | string | number;
  maxRts: number | string;
  maxRevenueShare: number | string;
  licenseTemplate?: Address;
};
```
```typescript Response Type
export type RegisterIpAndMakeDerivativeResponse = {
  txHash?: Hex;
  encodedTxData?: EncodedTxData;
  ipId?: Address;
  tokenId?: bigint;
  receipt?: TransactionReceipt;
};
```

## batchRegisterDerivative

Batch registers a derivative directly with parent IP's license terms.

| Method                    | Type                                                                                    |
| ------------------------- | --------------------------------------------------------------------------------------- |
| `batchRegisterDerivative` | `(request: BatchRegisterDerivativeRequest) => Promise<BatchRegisterDerivativeResponse>` |

## mintAndRegisterIpAndMakeDerivative

Mint an NFT from a collection and register it as a derivative IP without license tokens.

> 📘 NFT Metadata
>
> Note that this function will also set the underlying NFT's `tokenUri` to whatever is passed under `ipMetadata.nftMetadataURI`.

| Method                               | Type                                                                                                          |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------- |
| `mintAndRegisterIpAndMakeDerivative` | `(request: MintAndRegisterIpAndMakeDerivativeRequest) => Promise<MintAndRegisterIpAndMakeDerivativeResponse>` |

Parameters:

* `request.spgNftContract`: The address of the NFT collection.
* `request.allowDuplicates`: Set to true to allow minting IPs with the same NFT metadata.
* `request.derivData`: The derivative data to be used for registerDerivative.
  * `request.derivData.parentIpIds`: The IDs of the parent IPs to link the registered derivative IP.
  * `request.derivData.licenseTermsIds`: The IDs of the license terms to be used for the linking.
  * `request.derivData.maxMintingFee`: The maximum minting fee that the caller is willing to pay. If set to 0, then there is no no limit. **Recommended for simplicity: 0**
  * `request.derivData.maxRevenueShare`: The maximum revenue share percentage agreed upon between a child and parent when a child is registering as derivative. Must be between 0 and 100. **Recommended for simplicity: 100**
  * `request.derivData.maxRts`: The maximum number of royalty tokens that can be distributed to the external royalty policies. Must be between 0 and 100,000,000. **Recommended for simplicity: 100\_000\_000**
  * `request.derivData.licenseTemplate`: \[Optional] The address of the license template to be used for the linking.
* `request.ipMetadata`: \[Optional] The desired metadata for the newly minted NFT and newly registered IP.
  * `request.ipMetadata.ipMetadataURI` \[Optional] The URI of the metadata for the IP.
  * `request.ipMetadata.ipMetadataHash` \[Optional] The hash of the metadata for the IP.
  * `request.ipMetadata.nftMetadataURI` \[Optional] The URI of the metadata for the NFT.
  * `request.ipMetadata.nftMetadataHash` \[Optional] The hash of the metadata for the IP NFT.
* `request.recipient`: \[Optional] The address of the recipient of the minted NFT, default value is your wallet address.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

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
  spgNftContract: Address;
  derivData: DerivativeData;
  recipient?: Address;
  allowDuplicates: boolean;
} & IpMetadataAndTxOptions & WithWipOptions;

export type DerivativeData = {
  parentIpIds: Address[];
  licenseTermsIds: bigint[] | string[] | number[];
  maxMintingFee: bigint | string | number;
  maxRts: number | string;
  maxRevenueShare: number | string;
  licenseTemplate?: Address;
};
```
```typescript Response Type
export type MintAndRegisterIpAndMakeDerivativeResponse = {
  encodedTxData?: EncodedTxData;
} & CommonRegistrationResponse;

export type CommonRegistrationResponse = {
  txHash?: Hex;
  ipId?: Address;
  tokenId?: bigint;
  receipt?: TransactionReceipt;
};
```

## batchMintAndRegisterIpAndMakeDerivative

Batch mint an NFT from a collection and register it as a derivative IP without license tokens.

| Method                                    | Type                                                                                                                    |
| ----------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| `batchMintAndRegisterIpAndMakeDerivative` | `(request: BatchMintAndRegisterIpAndMakeDerivativeRequest) => Promise<BatchMintAndRegisterIpAndMakeDerivativeResponse>` |

## mintAndRegisterIp

Mint an NFT from an SPGNFT collection and register it with metadata as an IP.

> 📘 NFT Metadata
>
> Note that this function will also set the underlying NFT's `tokenUri` to whatever is passed under `ipMetadata.nftMetadataURI`.

| Method              | Type                                                                 |
| ------------------- | -------------------------------------------------------------------- |
| `mintAndRegisterIp` | `(request: MintAndRegisterIpRequest) => Promise<RegisterIpResponse>` |

Parameters:

* `request.spgNftContract`: The address of the NFT collection.
* `request.allowDuplicates`: Set to true to allow minting IPs with the same NFT metadata.
* `request.recipient`: \[Optional] The address of the recipient of the minted NFT, default value is your wallet address.
* `request.ipMetadata`: \[Optional] The desired metadata for the newly minted NFT and newly registered IP.
  * `request.ipMetadata.ipMetadataURI` \[Optional] The URI of the metadata for the IP.
  * `request.ipMetadata.ipMetadataHash` \[Optional] The hash of the metadata for the IP.
  * `request.ipMetadata.nftMetadataURI` \[Optional] The URI of the metadata for the NFT.
  * `request.ipMetadata.nftMetadataHash` \[Optional] The hash of the metadata for the IP NFT.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

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
  // set to true to have multiple NFTs with same metadata
  allowDuplicates: true,
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
  allowDuplicates: boolean;
} & IpMetadataAndTxOptions;
```
```typescript Response Type
export type RegisterIpResponse = {
  encodedTxData?: EncodedTxData;
} & CommonRegistrationResponse;

export type CommonRegistrationResponse = {
  txHash?: Hex;
  ipId?: Address;
  tokenId?: bigint;
  receipt?: TransactionReceipt;
};
```

## registerPilTermsAndAttach

Register Programmable IP License Terms (if unregistered) and attach it to IP.

| Method                      | Type                                                                                        |
| --------------------------- | ------------------------------------------------------------------------------------------- |
| `registerPilTermsAndAttach` | `(request: RegisterPilTermsAndAttachRequest) => Promise<RegisterPilTermsAndAttachResponse>` |

Parameters:

* `request.ipId`: The ID of the IP.
* `request.licenseTermsData[]`: The array of license terms to be attached.
  * `request.licenseTermsData.terms`: See the [LicenseTerms type](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/resources/license.ts#L26).
  * `request.licenseTermsData.licensingConfig`: See the [LicensingConfig type](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/common.ts#L15).
* `request.deadline`: \[Optional] The deadline for the signature in milliseconds, default is 1000s.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

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
  currency: '0x1514000000000000000000000000000000000000', // insert $WIP address from https://docs.story.foundation/docs/deployed-smart-contracts
  uri: '',
}

const licensingConfig: LicensingConfig = {
  isSet: false,
  mintingFee: BigInt(0),
  licensingHook: zeroAddress,
  hookData: zeroHash,
  commercialRevShare: 0,
  disabled: false,
  expectMinimumGroupRewardShare: 0,
  expectGroupRewardPool: zeroAddress,
};

const response = await client.ipAsset.registerPilTermsAndAttach({
  ipId: '0x4c1f8c1035a8cE379dd4ed666758Fb29696CF721',
  licenseTermsData: [{ terms: commercialRemixTerms, licensingConfig }],
  txOptions: { waitForTransaction: true },
})
console.log(`License Terms ${response.licenseTermsId} attached to IP Asset.`)
```
```typescript Request Type
export type RegisterPilTermsAndAttachRequest = {
  ipId: Address;
  licenseTermsData: LicenseTermsData<RegisterPILTermsRequest, LicensingConfig>[];
  deadline?: string | number | bigint;
  txOptions?: TxOptions;
};
```
```typescript Response Type
export type RegisterPilTermsAndAttachResponse = {
  txHash?: Hex;
  encodedTxData?: EncodedTxData;
  licenseTermsIds?: bigint[];
};
```

## mintAndRegisterIpAndMakeDerivativeWithLicenseTokens

Mint an NFT from a collection and register it as a derivative IP using license tokens

> 📘 NFT Metadata
>
> Note that this function will also set the underlying NFT's `tokenUri` to whatever is passed under `ipMetadata.nftMetadataURI`.

| Method                                                | Type                                                                                                   |
| ----------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| `mintAndRegisterIpAndMakeDerivativeWithLicenseTokens` | `(request: MintAndRegisterIpAndMakeDerivativeWithLicenseTokensRequest) => Promise<RegisterIpResponse>` |

Parameters:

* `request.spgNftContract`: The address of the NFT collection.
* `request.allowDuplicates`: Set to true to allow minting IPs with the same NFT metadata.
* `request.maxRts`: The maximum number of royalty tokens that can be distributed to the external royalty policies. Must be between 0 and 100,000,000. **Recommended for simplicity: 100\_000\_000**
* `request.licenseTokenIds`: The IDs of the license tokens to be burned for linking the IP to parent IPs.
* `request.ipMetadata`: \[Optional] The desired metadata for the newly minted NFT and newly registered IP.
  * `request.ipMetadata.ipMetadataURI` \[Optional] The URI of the metadata for the IP.
  * `request.ipMetadata.ipMetadataHash` \[Optional] The hash of the metadata for the IP.
  * `request.ipMetadata.nftMetadataURI` \[Optional] The URI of the metadata for the NFT.
  * `request.ipMetadata.nftMetadataHash` \[Optional] The hash of the metadata for the IP NFT.
* `request.recipient`: \[Optional] The address to receive the minted NFT, default value is your wallet address.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript TypeScript
import { toHex } from 'viem';

const response = await client.ipAsset.mintAndRegisterIpAndMakeDerivativeWithLicenseTokens({
  spgNftContract: "0xfE265a91dBe911db06999019228a678b86C04959", // your NFT contract address
  licenseTokenIds: ['10'],
  maxRts: 100_000_000, // default
  // set to true to allow ip with same nft metadata
  allowDuplicates: true,
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
  maxRts: number | string;
  allowDuplicates: boolean;
} & IpMetadataAndTxOptions & WithWipOptions;
```
```typescript Response Type
export type RegisterIpResponse = {
  encodedTxData?: EncodedTxData;
} & CommonRegistrationResponse;

export type CommonRegistrationResponse = {
  txHash?: Hex;
  ipId?: Address;
  tokenId?: bigint;
  receipt?: TransactionReceipt;
};
```

## registerIpAndMakeDerivativeWithLicenseTokens

Register the given NFT as a derivative IP using license tokens.

> 📘 NFT Metadata
>
> Note that this function will also set the underlying NFT's `tokenUri` to whatever is passed under `ipMetadata.nftMetadataURI`.

| Method                                         | Type                                                                                            |
| ---------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| `registerIpAndMakeDerivativeWithLicenseTokens` | `(request: RegisterIpAndMakeDerivativeWithLicenseTokensRequest) => Promise<RegisterIpResponse>` |

Parameters:

* `request.nftContract`: The address of the NFT collection.
* `request.tokenId`: The ID of the NFT.
* `request.maxRts`: The maximum number of royalty tokens that can be distributed to the external royalty policies. Must be between 0 and 100,000,000. **Recommended for simplicity: 100\_000\_000**
* `request.licenseTokenIds`: The IDs of the license tokens to be burned for linking the IP to parent IPs.
* `request.ipMetadata`: \[Optional] The desired metadata for the newly minted NFT and newly registered IP.
  * `request.ipMetadata.ipMetadataURI` \[Optional] The URI of the metadata for the IP.
  * `request.ipMetadata.ipMetadataHash` \[Optional] The hash of the metadata for the IP.
  * `request.ipMetadata.nftMetadataURI` \[Optional] The URI of the metadata for the NFT.
  * `request.ipMetadata.nftMetadataHash` \[Optional] The hash of the metadata for the IP NFT.
* `request.deadline`: \[Optional] The deadline for the signature in milliseconds, default is 1000ms.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript TypeScript
import { toHex } from 'viem';

const response = await client.ipAsset.registerIpAndMakeDerivativeWithLicenseTokens({
  nftContract: "0x041B4F29183317Fd352AE57e331154b73F8a1D73", // your NFT contract address
  tokenId: '127',
  licenseTokenIds: ['10'],
  maxRts: 100_000_000, // default
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
  maxRts: number | string;
} & IpMetadataAndTxOptions & WithWipOptions;
```
```typescript Response Type
export type RegisterIpResponse = {
  encodedTxData?: EncodedTxData;
} & CommonRegistrationResponse;

export type CommonRegistrationResponse = {
  txHash?: Hex;
  ipId?: Address;
  tokenId?: bigint;
  receipt?: TransactionReceipt;
};
```