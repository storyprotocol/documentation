---
title: SDK v1.3 MIGRATION GUIDE
excerpt: This is a migration guide to help you go from v1.2 -> v1.3 of the SDK.
deprecated: false
hidden: false
metadata:
  robots: index
---
Welcome to the v1.2 -> v1.3 SDK migration guide. Below, you can find the changes you must take to integrate with the v1.3 SDK.

# `client.ipAsset`

## `registerDerivative`

This function now has 3 extra parameters:

1. `maxMintingFee`: The maximum minting fee that the caller is willing to pay. If set to 0 then no limit.
2. `maxRevenueShare`: The maximum revenue share percentage agreed upon between a child and parent when a child is registering as derivative. Must be between 0 and 100,000,000 (where 100,000,000 represents 100%).
3. `maxRts`: The maximum number of royalty tokens that can be distributed to the external royalty policies (max: 100,000,000).

Example:

```typescript TypeScript
const response = await client.ipAsset.registerDerivative({
  childIpId: "0x1daAE3197Bc469Cb97B917aa460a12dD95c6627c",
  parentIpIds: ["0xd142822Dc1674154EaF4DDF38bbF7EF8f0D8ECe4"],
  licenseTermsIds: ["1"],
  maxMintingFee: BigInt(0), // disabled
  maxRts: 100_000_000, // default
  maxRevenueShare: 100_000_000, // default
});
```

## `registerDerivativeWithLicenseTokens`

The function now has 1 extra parameter:

1. `maxRts`: The maximum number of royalty tokens that can be distributed to the external royalty policies (max: 100,000,000).

Example:

```typescript TypeScript
const response = await client.ipAsset.registerDerivativeWithLicenseTokens({
  childIpId: "0xd142822Dc1674154EaF4DDF38bbF7EF8f0D8ECe4",
  licenseTokenIds: ["1"],
  maxRts: 100_000_000, // default
});
```

## `mintAndRegisterIpAssetWithPilTerms`

The function now has 1 extra parameter:

1. `allowDuplicates`: Set to true to allow minting an NFT with a duplicate `nftMetadataHash`.

The function has also modified `request.terms` to be `request.licenseTermsData`, which now also takes a `licenseConfig` as described [here](https://docs.story.foundation/docs/license-config-hook#/).

Example:

```typescript TypeScript
const licenseTerms: LicenseTerms = {
  transferable: true,
  royaltyPolicy: "0x1daAE3197Bc469Cb97B917aa460a12dD95c6627c",
  defaultMintingFee: BigInt(1),
  expiration: BigInt(0),
  commercialUse: true,
  commercialAttribution: false,
  commercializerChecker: zeroAddress,
  commercializerCheckerData: zeroAddress,
  commercialRevShare: 0,
  commercialRevCeiling: BigInt(0),
  derivativesAllowed: true,
  derivativesAttribution: true,
  derivativesApproval: false,
  derivativesReciprocal: true,
  derivativeRevCeiling: BigInt(0),
  currency: "0x1daAE3197Bc469Cb97B917aa460a12dD95c6627c",
  uri: "",
};

const licensingConfig: LicensingConfig = {
  isSet: true,
  mintingFee: BigInt(0),
  licensingHook: zeroAddress,
  hookData: zeroHash,
  commercialRevShare: 0,
  disabled: false,
  expectMinimumGroupRewardShare: 0,
  expectGroupRewardPool: zeroAddress,
};

const response = await client.ipAsset.mintAndRegisterIpAssetWithPilTerms({
  spgNftContract: "0x",
  licenseTermsData: [
    {
      terms: licenseTerms,
      licensingConfig,
    },
  ],
  allowDuplicates: true,
  ipMetadata: {
    ipMetadataURI: "https://",
    ipMetadataHash: toHex("metadata", { size: 32 }),
    nftMetadataURI: "https://",
    nftMetadataHash: toHex("nftMetadata", { size: 32 }),
  },
});
```

## `registerIpAndAttachPilTerms`

The function has modified `request.terms` to be `request.licenseTermsData`, which now also takes a `licenseConfig` as described [here](https://docs.story.foundation/docs/license-config-hook#/).

Example:

```typescript TypeScript
const licenseTerms: LicenseTerms = {
  transferable: true,
  royaltyPolicy: "0x1daAE3197Bc469Cb97B917aa460a12dD95c6627c",
  defaultMintingFee: BigInt(1),
  expiration: BigInt(0),
  commercialUse: true,
  commercialAttribution: false,
  commercializerChecker: zeroAddress,
  commercializerCheckerData: zeroAddress,
  commercialRevShare: 0,
  commercialRevCeiling: BigInt(0),
  derivativesAllowed: true,
  derivativesAttribution: true,
  derivativesApproval: false,
  derivativesReciprocal: true,
  derivativeRevCeiling: BigInt(0),
  currency: "0x1daAE3197Bc469Cb97B917aa460a12dD95c6627c",
  uri: "",
};

const licensingConfig: LicensingConfig = {
  isSet: true,
  mintingFee: BigInt(0),
  licensingHook: zeroAddress,
  hookData: zeroHash,
  commercialRevShare: 0,
  disabled: false,
  expectMinimumGroupRewardShare: 0,
  expectGroupRewardPool: zeroAddress,
};

const response = await client.ipAsset.registerIpAndAttachPilTerms({
  nftContract: "0x",
  tokenId: "3",
  ipMetadata: {
    ipMetadataURI: "https://",
    ipMetadataHash: toHex("metadata", { size: 32 }),
    nftMetadataURI: "https://",
    nftMetadataHash: toHex("nftMetadata", { size: 32 }),
  },
  licenseTermsData: [
    {
      terms: licenseTerms,
      licensingConfig,
    },
  ],
});
```

## `registerDerivativeIp`

This function now has 3 extra parameters under derivative data:

1. `maxMintingFee`: The maximum minting fee that the caller is willing to pay. If set to 0 then no limit.
2. `maxRevenueShare`: The maximum revenue share percentage agreed upon between a child and parent when a child is registering as derivative. Must be between 0 and 100,000,000 (where 100,000,000 represents 100%).
3. `maxRts`: The maximum number of royalty tokens that can be distributed to the external royalty policies (max: 100,000,000).

Example:

```typescript TypeScript
const derivData: DerivativeData = {
  parentIpIds: ["0xd142822Dc1674154EaF4DDF38bbF7EF8f0D8ECe4"],
  licenseTermsIds: ["1"],
  maxMintingFee: BitInt(0), // disabled
  maxRts: 100_000_000, // default
  maxRevenueShare: 100_000_000, // default
};

const response = await client.ipAsset.registerDerivativeIp({
  nftContract: "0x",
  tokenId: "3",
  derivData,
  ipMetadata: {
    ipMetadataURI: "https://",
    ipMetadataHash: toHex("metadata", { size: 32 }),
    nftMetadataURI: "https://",
    nftMetadataHash: toHex("nftMetadata", { size: 32 }),
  },
})
```

## `mintAndRegisterIpAndMakeDerivative`

The function now has 1 extra parameter:

1. `allowDuplicates`: Set to true to allow minting an NFT with a duplicate `nftMetadataHash`.

This function also has 3 extra parameters under derivative data:

1. `maxMintingFee`: The maximum minting fee that the caller is willing to pay. If set to 0 then no limit.
2. `maxRts`: The maximum number of royalty tokens that can be distributed to the external royalty policies (max: 100,000,000).
3. `maxRevenueShare`: The maximum revenue share percentage allowed for minting the License Tokens. Must be between 0 and 100,000,000 (where 100,000,000 represents 100%).

Example:

```typescript TypeScript
const derivData: DerivativeData = {
  parentIpIds: ["0xd142822Dc1674154EaF4DDF38bbF7EF8f0D8ECe4"],
  licenseTermsIds: ["1"],
  maxMintingFee: BitInt(0), // disabled
  maxRts: 100_000_000, // default
  maxRevenueShare: 100_000_000, // default
};

const response = await client.ipAsset.mintAndRegisterIpAndMakeDerivative({
  spgNftContract: "0x",
  derivData,
  allowDuplicates: true,
  ipMetadata: {
    ipMetadataURI: "https://",
    ipMetadataHash: toHex("metadata", { size: 32 }),
    nftMetadataURI: "https://",
    nftMetadataHash: toHex("nftMetadata", { size: 32 }),
  },
})
```

## `mintAndRegisterIp`

The function now has 1 extra parameter:

1. `allowDuplicates`: Set to true to allow minting an NFT with a duplicate `nftMetadataHash`.

Example:

```typescript TypeScript
const response = await client.ipAsset.mintAndRegisterIp({
  spgNftContract: "0x",
  ipMetadata: {
    ipMetadataURI: "https://",
    ipMetadataHash: toHex("metadata", { size: 32 }),
    nftMetadataURI: "https://",
    nftMetadataHash: toHex("nftMetadata", { size: 32 }),
  },
  allowDuplicates: false,
})
```

## `registerPilTermsAndAttach`

The function has modified `request.terms` to be `request.licenseTermsData`, which now also takes a `licenseConfig` as described [here](https://docs.story.foundation/docs/license-config-hook#/).

Example:

```typescript TypeScript
const licenseTerms: LicenseTerms = {
  transferable: true,
  royaltyPolicy: "0x1daAE3197Bc469Cb97B917aa460a12dD95c6627c",
  defaultMintingFee: BigInt(1),
  expiration: BigInt(0),
  commercialUse: true,
  commercialAttribution: false,
  commercializerChecker: zeroAddress,
  commercializerCheckerData: zeroAddress,
  commercialRevShare: 0,
  commercialRevCeiling: BigInt(0),
  derivativesAllowed: true,
  derivativesAttribution: true,
  derivativesApproval: false,
  derivativesReciprocal: true,
  derivativeRevCeiling: BigInt(0),
  currency: "0x1daAE3197Bc469Cb97B917aa460a12dD95c6627c",
  uri: "",
};

const licensingConfig: LicensingConfig = {
  isSet: true,
  mintingFee: BigInt(0),
  licensingHook: zeroAddress,
  hookData: zeroHash,
  commercialRevShare: 0,
  disabled: false,
  expectMinimumGroupRewardShare: 0,
  expectGroupRewardPool: zeroAddress,
};

const response = await client.ipAsset.registerPilTermsAndAttach({
  ipId: "0x",
  licenseTermsData: [
    {
      terms: licenseTerms,
      licensingConfig,
    },
  ],
})
```

## `mintAndRegisterIpAndMakeDerivativeWithLicenseTokens`

The function now has 2 extra parameters:

1. `allowDuplicates`: Set to true to allow minting an NFT with a duplicate `nftMetadataHash`.
2. `maxRts`: The maximum number of royalty tokens that can be distributed to the external royalty policies (max: 100,000,000).

Example:

```typescript TypeScript
const response = await client.ipAsset.mintAndRegisterIpAndMakeDerivativeWithLicenseTokens({
  spgNftContract: "0x",
  licenseTokenIds: ["12"],
  ipMetadata: {
    ipMetadataURI: "",
    ipMetadataHash: toHex(0, { size: 32 }),
    nftMetadataHash: toHex("nftMetadata", { size: 32 }),
    nftMetadataURI: "",
  },
  recipient: "0x73fcb515cee99e4991465ef586cfe2b072ebb512",
  maxRts: 100_000_000,
  allowDuplicates: true,
})
```

## `registerIpAndMakeDerivativeWithLicenseTokens`

The function now has 1 extra parameter:

1. `maxRts`: The maximum number of royalty tokens that can be distributed to the external royalty policies (max: 100,000,000).

Example:

```typescript TypeScript
const response = await client.ipAsset.registerIpAndMakeDerivativeWithLicenseTokens({
  nftContract: "0x",
  maxRts: 100_000_000,
  tokenId: "3",
  licenseTokenIds: ["12"],
  ipMetadata: {
    ipMetadataURI: "",
    ipMetadataHash: toHex(0, { size: 32 }),
    nftMetadataHash: toHex("nftMetadata", { size: 32 }),
    nftMetadataURI: "",
  },
})
```

# `client.license`

## `mintLicenseTokens`

The function now has 2 extra parameters:

1. `maxMintingFee`: The maximum minting fee that the caller is willing to pay. If set to 0 then no limit.
2. `maxRevenueShare`: The maximum revenue share percentage allowed for minting the License Tokens. Must be between 0 and 100,000,000 (where 100,000,000 represents 100%).

Example:

```typescript TypeScript
const response = await client.license.mintLicenseTokens({
  licensorIpId: "0x",
  licenseTermsId: "1",
  maxMintingFee: BigInt(0),
  maxRevenueShare: 100_000_000,
})
```

# `client.royalty`

Updated document coming soon!

# `client.dispute`

## `raiseDispute`

The function now has 2 extra parameters:

1. `bond`: The bond size.
2. `liveness`: The liveness time.

The following parameters have been removed:

1. `data`

Example:

```typescript TypeScript
const response = await client.dispute.raiseDispute({
  targetIpId: "0x1daAE3197Bc469Cb97B917aa460a12dD95c6627c",
  cid: "QmbWqxBEKC3P8tqsKc98xmWNzrzDtRLMiMPL8wBuTGsMnR",
  targetTag: "tag",
  bond: 0,
  liveness: 2592000,
})
```