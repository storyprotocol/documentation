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

1. `maxMintingFee`: The maximum minting fee that the caller is willing to pay. if set to 0 then no limit.
2. `maxRts`: The maximum number of royalty tokens that can be distributed to the external royalty policies (max: 100,000,000).
3. `maxRevenueShare`: The maximum revenue share percentage allowed for minting the License Tokens. Must be between 0 and 100,000,000 (where 100,000,000 represents 100%).

Example:

```typescript TypeScript
const response = await client.ipAsset.registerDerivative({
  childIpId: "0x1daAE3197Bc469Cb97B917aa460a12dD95c6627c",
  parentIpIds: ["0xd142822Dc1674154EaF4DDF38bbF7EF8f0D8ECe4"],
  licenseTermsIds: ["1"],
  maxMintingFee: 0n, // disabled
  maxRts: 100000000, // default
  maxRevenueShare: 100000000, // default
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
  maxRts: 100000000, // default
});
```

## `mintAndRegisterIpAssetWithPilTerms`

The function now has 1 extra parameter:

1. `allowDuplicates`: Indicates whether the license terms can be attached to the same IP ID or not.

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
  mintingFee: BigInt(1),
  licensingHook: "0x1daAE3197Bc469Cb97B917aa460a12dD95c6627c",
  hookData: zeroHash,
  commercialRevShare: 1,
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
  allowDuplicates: false,
});
```