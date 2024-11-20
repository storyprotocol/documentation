---
title: Register an IPA as a Derivative in React
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
This section demonstrates how to register an IP Asset as a derivative of another. There are generally three methods of registering an IPA remix:

1. Link an existing IPA as a derivative of a "parent" IPA using a License Token.
2. Link an existing IPA as a derivative of a "parent" IPA directly.
3. Register an IPA and link it as a derivative of a "parent" IPA directly.

## Prerequisites

- [React SDK Setup](doc:react-sdk-setup)
- The "parent" IPA must be registered.
- The "child" IPA must be registered.

# Register Derivative using License Token

## Additional Prerequisites

- An already minted License Token from the "parent" IPA. Learn how to mint a License Token [here](doc:mint-license).

If you already have a License Token, it is easier to register a derivative this way. We can register a child IPA as a derivative of a parent IPA by calling `client.ipAsset.registerDerivativeWithLicenseTokens()` like so:

```jsx RegisterDerivativeIPA.tsx
import { useIpAsset } from "@story-protocol/react-sdk";

export default async function RegisterDerivativeIPA() {
  const { registerDerivativeWithLicenseTokens } = useIpAsset();
  
  const response = await registerDerivativeWithLicenseTokens({
    childIpId: "0xC92EC2f4c86458AFee7DD9EB5d8c57920BfCD0Ba",
    licenseTokenIds: ["5"], // array of license ids relevant to the creation of the derivative, minted from the parent IPA
    txOptions: { waitForTransaction: true }
  });

  console.log(`Derivative IPA linked to parent at transaction hash ${response.txHash}`)
  
  return (
    {/* */} 
  )
}
```
```typescript Request Type
export type RegisterDerivativeWithLicenseTokensRequest = {
  childIpId: Address;
  licenseTokenIds: string[] | bigint[] | number[];
  txOptions?: TxOptions;
};
```
```typescript Response Type
export type RegisterDerivativeWithLicenseTokensResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
};
```

# Register Derivative Directly

You can also register a derivative directly, without needing a License Token. There is no real difference between `registerDerivativeWithLicenseTokens` (above) and `registerDerivative` (below) except that the former requires an already minted License Token and the latter is a convenient function that handles it for you.

> ❓ "Why would I ever use a License Token then?"
> 
> There are a few times when you would need a License Token to register a derivative:
> 
> - The License Token contains private license terms, so you would only be able to register if you had the License Token that was manually minted by the owner. More on that [here](https://docs.story.foundation/docs/license-token#private-licenses).
> - The License Token (which is an NFT) costs a `mintingFee` to mint, and you were able to buy it on a marketplace for a cheaper price. Then it makes more sense to simply register with the License Token then have to pay the more expensive `mintingFee`.

> 📘 Quick Note
> 
> Remember that once License Terms are attached to an IP Asset, it becomes public to register a derivative with those terms, which is why you don't necessarily need a License Token first. Read more on that [here](https://docs.story.foundation/docs/license-terms#license-terms-attached-to-ip-asset).

```jsx RegisterDerivativeIPA.tsx
import { useIpAsset } from "@story-protocol/react-sdk";

export default async function RegisterDerivativeIPA() {
  const { registerDerivative } = useIpAsset();
  
  const response = await registerDerivative({
    childIpId: "0xC92EC2f4c86458AFee7DD9EB5d8c57920BfCD0Ba",
    parentIpIds: ["0xC92EC2f4c86458AFee7DD9EB5d8c57920BfCD0Ba"],
    licenseTermsIds: ["5"],
    txOptions: { waitForTransaction: true }
  });

  console.log(`Derivative IPA linked to parent at transaction hash ${response.txHash}`)
  
  return (
    {/* */} 
  )
}
```
```typescript Request Type
export type RegisterDerivativeRequest = {
  childIpId: Address;
  parentIpIds: Address[];
  licenseTermsIds: string[] | bigint[] | number[];
  licenseTemplate?: Address;
  txOptions?: TxOptions;
};
```
```typescript Response Type
export type RegisterDerivativeResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  childIpId?: Address;
};
```

# Register and Make Derivative

To register an IP Asset and link it as a derivative of another IP Asset all in one transaction, you can use the `registerDerivativeIp` function provided by the SPG [here](https://docs.story.foundation/docs/spg-functions-react#register--derivative). If you also want to mint an NFT in the same transaction (you don't have one to register already), you can also use the `mintAndRegisterIpAndMakeDerivative` function provided by the SPG [here](https://docs.story.foundation/docs/spg-functions#mint--register--derivative).