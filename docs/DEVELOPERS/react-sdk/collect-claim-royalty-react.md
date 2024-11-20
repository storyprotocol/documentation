---
title: Collect & Claim Royalty in React
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
This section demonstrates how to collect royalty tokens and then claim actual revenue.

## Prerequisites

- [React SDK Setup](doc:react-sdk-setup)
- Understand the [Royalty Module](doc:royalty-module)

# Collect Royalty Tokens

You can collect royalty tokens from children IP Assets, which represent your share of the royalty %. This is only done one time, and anyone can call it (since it doesn't matter if someone else helps me collect my royalty tokens). The royalty tokens will be deposited to the IP Account.

This function will also claim any outstanding revenue tokens that would normally be claimed with `claimRoyalty`, but since royalty tokens were not yet collected, were not deposited to ancestors.

```jsx CollectRoyalty.tsx
import { useRoyalty } from "@story-protocol/react-sdk";

export default async function CollectRoyalty() {
  const { collectRoyaltyTokens } = useRoyalty();
  
  const response = await collectRoyaltyTokens({
    parentIpId: "0xC92EC2f4c86458AFee7DD9EB5d8c57920BfCD0Ba",
    royaltyVaultIpId: "0xC92EC2f4c86458AFee7DD9EB5d8c57920BfCD0Ba",
    txOptions: { waitForTransaction: true }
  });

  console.log(`Collected royalty token ${response.royaltyTokensCollected} at transaction hash ${response.txHash}`)
  
  return (
    {/* */} 
  )
}
```
```typescript Request Type
export type CollectRoyaltyTokensRequest = {
  parentIpId: Address;
  royaltyVaultIpId: Address;
  txOptions?: TxOptions;
};
```
```typescript Response Type
export type CollectRoyaltyTokensResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  royaltyTokensCollected?: bigint;
};
```

Setting `waitForTransaction: true` in the transaction options will return the `royaltyTokensCollected`.

# Claim Revenue

To claim your % of actual money paid to child IP Assets, otherwise known as revenue tokens, you can call `claimRoyalty` whenever you want.

However before you do this, you must have `snapshotIds` to provide to the transaction. Anyone can call this function, since it doesn't matter who snapshots. You can get that like so:

```jsx Snapshot.tsx
import { useRoyalty } from "@story-protocol/react-sdk";

export default async function Snapshot() {
  const { snapshot } = useRoyalty();
  
  const response = await snapshot({
    royaltyVaultIpId: "0xC92EC2f4c86458AFee7DD9EB5d8c57920BfCD0Ba",
    txOptions: { waitForTransaction: true }
  });

  console.log(`Took a snapshot with id ${response.snapshotId} at transaction hash ${response.txHash}`)
  
  return (
    {/* */} 
  )
}
```
```typescript Request Type
export type SnapshotRequest = {
  royaltyVaultIpId: Address;
  txOptions?: TxOptions;
};
```
```typescript Response Type
export type SnapshotResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  snapshotId?: bigint;
};
```

Setting `waitForTransaction: true` in the transaction options will return the `snapshotId`.

Now you can `claimRevenue` as shown below. **Only the owner of the royalty tokens at the time of the provided snapshot can call this function.**

> 📘 Claiming to an IP Account
> 
> Normally, `claimRevenue` claims the revenue tokens to the wallet that runs the transaction. However, you can optionally claim to the IP Account by passing an `account` parameter which is the `ipId` of the IP Account.

```jsx ClaimRevenue.tsx
import { useRoyalty } from "@story-protocol/react-sdk";

export default async function ClaimReveue() {
  const { claimRevenue } = useRoyalty();
  
  const response = await claimRevenue({
    snapshotIds: ["1", "2"],
    royaltyVaultIpId: "0xC92EC2f4c86458AFee7DD9EB5d8c57920BfCD0Ba",
    token: "0xC92EC2f4c86458AFee7DD9EB5d8c57920BfCD0Ba",
    txOptions: { waitForTransaction: true }
  });

  console.log(`Claimed revenue token ${response.claimableToken} at transaction hash ${response.txHash}`)
  
  return (
    {/* */} 
  )
}
```
```typescript Request Type
export type ClaimRevenueRequest = {
  snapshotIds: string[] | number[] | bigint[];
  token: Address;
  royaltyVaultIpId: Address;
  account?: Address;
  txOptions?: TxOptions;
};
```
```typescript Response Type
export type ClaimRevenueResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  claimableToken?: bigint;
};
```

Setting `waitForTransaction: true` in the transaction options will return the `claimableToken`.