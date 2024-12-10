---
title: Mint a License Token
excerpt: Learn how to mint a License Token from an IP Asset on TypeScript.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
This section demonstrates how to mint a License Token for an IPA. You can only mint a License Token for an IPA if it has License Terms attached to it. A License Token is minted as an ERC721 token and contains the necessary licensing details.

> 💰 Paid Licenses
>
> Note that some IP Assets may have license terms attached that require the user minting the license to pay a `mintingFee`. You can see an example of that in the <a href="https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/registerDerivativeCommercial.ts" target="_blank">TypeScript Tutorial</a>.

> 📘 Max Number of Licenses
>
> If you're curious about setting the maximum number of licenses that can be created from your IP, check out the [License Config / Hook](doc:license-config-hook) section of our documentation.

## Prerequisites

* [Setup](doc:typescript-sdk-setup) the client object.
* An IPA that has License Terms added. Learn how to add License Terms to an IPA [here](doc:attach-terms-to-an-ip-asset).

# Mint License

To mint a License Token, we will need the:

* `licenseTermsId` - the id of the License Terms (ex. "1" for Non-Commercial Social Remixing)
* `licensorIpId` - the ipId of the IPA we are minting a License Token from
* `receiver` - wallet receiving the License Token, usually the wallet address that is executing the transaction
* `amount` - # of License Tokens to be minted (usually 1, unless the minter decides to mint a batch and distribute it in another way later on)

```typescript TypeScript
const response = await client.license.mintLicenseTokens({
   licenseTermsId: "1", 
   licensorIpId: "0xC92EC2f4c86458AFee7DD9EB5d8c57920BfCD0Ba",
   receiver: "0x14dC79964da2C08b23698B3D3cc7Ca32193d9955", 
   amount: 1, 
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

Setting `waitForTransaction: true` in the transaction options will return the `licenseTokenId` of the newly minted license(s).
