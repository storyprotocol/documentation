---
title: Mint a License Token
excerpt: Learn how to mint a License Token from an IP Asset in TypeScript.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
This section demonstrates how to mint a [License Token](doc:license-token) from an [🧩 IP Asset](doc:ip-asset). You can only mint a License Token from an IP Asset if the IP Asset has [License Terms](doc:license-terms) attached to it. A License Token is minted as an ERC-721.

There are two reasons you'd mint a License Token:

1. To hold the license and be able to use the underlying IP Asset as the license described (for ex. "Can use commercially as long as you provide proper attribution and share 5% of your revenue)
2. Use the license token to link another IP Asset as a derivative of it. *Note though that, as you'll see later, some SDK functions don't require you to explicitly mint a license token first in order to register a derivative, and will actually handle it for you behind the scenes.*

### :warning: Prerequisites

There are a few steps you have to complete before you can start the tutorial.

1. Complete the [TypeScript SDK Setup](doc:typescript-sdk-setup)
2. An IP Asset that has License Terms added. Learn how to add License Terms to an IPA [here](doc:attach-terms-to-an-ip-asset).

## 1. Mint License

Let's say that IP Asset (`ipId = 0x01`) has License Terms (`licenseTermdId = 10`) attached to it. We want to mint 2 License Tokens with those terms to a specific wallet address (`0x02`).

> 🚧 Paid Licenses
>
> Be mindful that some IP Assets may have license terms attached that require the user minting the license to pay a `defaultMintingFee`. You can see an example of that in the <a href="https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/registerDerivativeCommercial.ts" target="_blank">TypeScript Tutorial</a>.

```typescript TypeScript
const response = await client.license.mintLicenseTokens({
  licenseTermsId: "10", 
  licensorIpId: "0x01",
  receiver: "0x02", // optional. if not provided, it will go to the tx sender
  amount: 2,
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
  amount?: number | string | bigint;
  receiver?: Address;
  txOptions?: TxOptions;
};
```
```typescript Response Type
export type MintLicenseTokensResponse = {
  licenseTokenIds?: bigint[];
  txHash?: string;
  encodedTxData?: EncodedTxData;
};
```

### 1a. :hand: Setting Restrictions on Minting License Token

This is a note for owners of an IP Asset who want to set restrictions on who or how their license tokens are minted. You can:

* Set a max number of licenses that can be minted
* Charge dynamic fees based on who / how many are minted
* Whitelisted certain wallets to mint the tokens

... and more. Learn more by checking out the [License Config / Hook](doc:license-config-hook) section of our documentation.

## 2. Register a Derivative

Now that we have minted a License Token, we can hold it or use it to link an IP Asset as a derivative. We will go over that on the next page.

*Note though that, as you'll see later, some SDK functions don't require you to explicitly mint a license token first in order to register a derivative, and will actually handle it for you behind the scenes.*

### 2a. :question: Why would I ever use a License Token if it's not needed?

There are a few times when **you would need** a License Token to register a derivative:

* The License Token contains private license terms, so you would only be able to register as a derivative if you had the License Token that was manually minted by the owner. More on that [here](https://docs.story.foundation/docs/license-token#private-licenses).
* The License Token (which is an NFT) costs a `mintingFee` to mint, and you were able to buy it on a marketplace for a cheaper price. Then it makes more sense to simply register with the License Token then have to pay the more expensive `defaultMintingFee`.