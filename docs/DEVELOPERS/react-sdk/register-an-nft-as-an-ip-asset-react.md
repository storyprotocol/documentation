---
title: Register an IP Asset in React
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
This section demonstrates how to register an IP Asset using the React SDK. There are generally two methods of IP registration:

1. Register an existing NFT as an IP Asset
2. Create an NFT and register it as an IP Asset in one transaction

## Prerequisites

- [React SDK Setup](doc:react-sdk-setup)

# Register an Existing NFT as an IP Asset

You can register your NFT as an IP Asset by simply calling `register()` and passing in the token's contract address and token ID, like so:

> 📘 Default License Terms
> 
> Note that every single IP Asset registered on Story automatically has [Non-Commercial Social Remixing License Terms](https://docs.story.foundation/docs/pil-flavors#flavor-1-non-commercial-social-remixing)  attached to it.
> 
> If it's a root IP Asset (meaning it has no more parents), you can attach more License Terms to it later. Please see [Attach Terms to an IP Asset in React](doc:attach-license-terms-to-an-ip-asset-react) for more info.

```jsx RegisterIPA.tsx
import { toHex } from 'viem';
import { useIpAsset } from "@story-protocol/react-sdk";

export default async function RegisterIPA() {
  const { register } = useIpAsset();
  
  const response = await register({
    nftContract: "0xd516482bef63Ff19Ed40E4C6C2e626ccE04e19ED", // your NFT contract address
    tokenId: "12", // your NFT token ID
    ipMetadata: {
      ipMetadataURI: 'test-uri',
      ipMetadataHash: toHex('test-metadata-hash', { size: 32 }),
      nftMetadataHash: toHex('test-nft-metadata-hash', { size: 32 }),
      nftMetadataURI: 'test-nft-uri',
    },
    txOptions: { waitForTransaction: true }
  });
  console.log(
    `Root IPA created at tx hash ${response.txHash}, IPA ID: ${response.ipId}`
  );
  
  return (
    {/* */} 
  )
}
```
```typescript Request Type
export type RegisterRequest = {
  nftContract: Address;
  tokenId: string | number | bigint;
  deadline?: string | number | bigint;
} & IpMetadataAndTxOption;

type IpMetadataAndTxOption = {
  ipMetadata?: {
    ipMetadataURI?: string;
    ipMetadataHash?: Hex;
    nftMetadataURI?: string;
    nftMetadataHash?: Hex;
  };
  txOptions?: TxOptions;
};
```
```typescript Response Type
export type RegisterIpResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  ipId?: Address;
};
```

> :information_source: If the provided `tokenId` from the `nftContract` has already been registered, the `response` object will contain the `ipId` of the existing IP Asset and an undefined `txHash`.

Setting `waitForTransaction: true` in the transaction options will return the `ipId` of the newly registered IP asset.

After we run the above code, the console output will look like:

```Text Console Output
Root IPA created at transaction hash 0xa3e1caa7c2124b1550d459abc739291cb1be77ac73b99c097707878ac4ef57ae,
IPA ID: 0x4c1f8c1035a8cE379dd4ed666758Fb29696CF721
```

# Create an NFT and Register it as an IP Asset

To mint an NFT and register it as an IP Asset all in one transaction, you can use the `mintAndRegisterIpAssetWithPilTerms` function provided by the SPG [here](https://docs.story.foundation/docs/spg-functions-react#mint--register--metadata--attach-terms).