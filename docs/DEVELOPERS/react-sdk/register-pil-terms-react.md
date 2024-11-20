---
title: Register PIL Terms in React
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
This section demonstrates how to register a selection of License Terms using the PIL.

## Prerequisites

- [React SDK Setup](doc:react-sdk-setup)
- If License Terms already exist for the identical set of parameters you intend to create, it is unnecessary to create it again and the function will simply return the existing `licenseTermsId` and an undefined `txHash`. You can use existing License Terms by its `licenseTermsId`. 

> 🪙 Whitelisted Revenue Tokens
> 
> At the moment, a token must be whitelisted in our protocol to be used in the royalty module as the `currency` field.
> 
> Please see the [currently whitelisted revenue tokens](https://docs.story.foundation/docs/royalty-module#whitelisted-revenue-tokens), where you can also mint tokens directly to be used in the below commercial sections.

# Create a Commercial Use License

- Click [here](https://docs.story.foundation/docs/pil-flavors#flavor-1-non-commercial-social-remixing) to see info on Commercial Use licenses.

```jsx RegisterTerms.tsx
import { useLicense } from "@story-protocol/react-sdk";

export default async function RegisterTerms() {
  const { registerCommercialUsePIL } = useLicense();
  
  const commercialUseParams = {
    currency: '0xB132A6B7AE652c974EE1557A3521D53d18F6739f', // see the above note on whitelisted revenue tokens
    mintingFee: '10' // 10 of the currency (using the above currency, 10 MERC20)
  }

  const response = await registerCommercialUsePIL({
    ...commercialUseParams,
    txOptions: { waitForTransaction: true }
  });

  console.log(`PIL Terms registered at transaction hash ${response.txHash}, License Terms ID: ${response.licenseTermsId}`) 
  
  return (
    {/* */} 
  )
}
```
```typescript Request Type
export type RegisterCommercialUsePILRequest = {
  mintingFee: string | number | bigint;
  currency: Address;
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

- Click [here](https://docs.story.foundation/docs/pil-flavors#flavor-1-non-commercial-social-remixing) to see info on Commercial Remix licenses.

```jsx RegisterTerms.tsx
import { useLicense } from "@story-protocol/react-sdk";

export default async function RegisterTerms() {
  const { registerCommercialRemixPIL } = useLicense();
  
  const commercialRemixParams = {
    currency: '0xB132A6B7AE652c974EE1557A3521D53d18F6739f', // see the above note on whitelisted revenue tokens
    mintingFee: '10', // 10 of the currency (using the above currency, 10 MERC20)
    commercialRevShare: 10 // 10 = 10%
  }

  const response = await registerCommercialRemixPIL({
    ...commercialRemixParams,
    txOptions: { waitForTransaction: true }
  });

  console.log(`PIL Terms registered at transaction hash ${response.txHash}, License Terms ID: ${response.licenseTermsId}`) 
  
  return (
    {/* */} 
  )
}
```
```typescript Request Type
export type RegisterCommercialRemixPILRequest = {
  mintingFee: string | number | bigint;
  commercialRevShare: number;
  currency: Address;
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

- Click [here](https://docs.story.foundation/docs/pil-flavors#flavor-1-non-commercial-social-remixing) to see info on Non-Commercial Social Remixing licenses.

There are currently no parameters to be passed in here, so this function should not really be used other than to get back the `licenseTermsId` for this license.

In addition, NCSR terms are already attached to every single IP Asset **by default**, with `licenseTermsId == 1`.

```jsx RegisterTerms.tsx
import { useLicense } from "@story-protocol/react-sdk";

export default async function RegisterTerms() {
  const { registerNonComSocialRemixingPIL } = useLicense();
  
  const nonComSocialRemixingParams = {}

  const response = await registerNonComSocialRemixingPIL({
    ...nonComSocialRemixingParams,
    txOptions: { waitForTransaction: true }
  });

  console.log(`PIL Terms registered at transaction hash ${response.txHash}, License Terms ID: ${response.licenseTermsId}`) 
  
  return (
    {/* */} 
  )
}
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