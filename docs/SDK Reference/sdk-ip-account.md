---
title: IP Account
deprecated: false
hidden: false
metadata:
  robots: index
---
## IPAccountClient

### Methods

* setIpMetadata

### setIpMetadata

Sets the metadataURI for an IP asset.

| Method          | Type                                    |
| --------------- | --------------------------------------- |
| `setIpMetadata` | `(SetIpMetadataRequest) => Promis<Hex>` |

Parameters:

* `request.ipId`: The IP to set the metadata for.
* `request.metadataURI`: The metadataURI to set for the IP asset. Should be a URL pointing to metadata that fits the [📝 IPA Metadata Standard](doc:ipa-metadata-standard).
* `request.metadataHash`: The hash of metadata at metadataURI.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript TypeScript
const txHash = await client.ipAccount.setIpMetadata({
  ipId: "0x01",
  metadataURI: "https://ipfs.io/ipfs/bafkreiardkgvkejqnnkdqp4pamkx2e5bs4lzus5trrw3hgmoa7dlbb6foe",
  // example hash (not accurate)
  metadataHash: "0x129f7dd802200f096221dd89d5b086e4bd3ad6eafb378a0c75e3b04fc375f997",
});
```
```typescript Request Type
export type SetIpMetadataRequest = {
  ipId: Address;
  metadataURI: string;
  metadataHash: Hex;
  txOptions?: Omit<TxOptions, "encodedTxDataOnly">;
};
```