---
title: Claim Revenue
excerpt: Learn how to claim due revenue from a child IP Asset in TypeScript.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
This section demonstrates how to claim due revenue from an IP Asset in TypeScript.

## Prerequisites

* [Setup](doc:typescript-sdk-setup) the client object.
* Understand the [💸 Royalty Module](doc:royalty-module)

# Claiming Revenue Tokens

As you can probably tell from reading the [💸 Royalty Module](doc:royalty-module) docs, there are a few steps involved in claiming revenue from child IP Assets. Obviously, the revenue must first be paid to the child (read [Pay an IPA in TypeScript](doc:pay-ipa)). 

The Royalty Module automatically splits the revenue between the child and its ancestors based on the license terms ▶️ the revenue tokens must then be transferred from the royalty policy contract to the IPA's [IP Royalty Vault](doc:ip-royalty-vault) ▶️ a snapshot must be taken ▶️ the tokens can be claimed by whoever owns the associated Royalty Tokens.

To simplify this, the SDK supports a `transferToVaultAndSnapshotAndClaimByTokenBatch` function which does all of the above. 

```typescript TypeScript
const claimRevenue = await client.royalty.transferToVaultAndSnapshotAndClaimByTokenBatch({
  ancestorIpId: '0xDa03c4B278AD44f5a669e9b73580F91AeDE0E3B2',
  claimer: '0xDa03c4B278AD44f5a669e9b73580F91AeDE0E3B2', // whoever owns the royalty tokens (most likely the associated ipId)
  royaltyClaimDetails: [{ 
    childIpId: '0x0b825D9E5FA196e6B563C0a446e8D9885057f9B1', 
    royaltyPolicy: RoyaltyPolicyLAP, // insert RoyaltyPolicyLAP address from https://docs.story.foundation/docs/deployed-smart-contracts
    currencyToken: SUSD, // insert SUSD address from https://docs.story.foundation/docs/deployed-smart-contracts
    amount: 1 
  }],
  txOptions: { waitForTransaction: true },
})
console.log(`Claimed ${claimRevenue.amountsClaimed} revenue at snapshotId ${claimRevenue.snapshotId}`)
```
```typescript Request Type
export type TransferToVaultAndSnapshotAndClaimByTokenBatchRequest = {
  ancestorIpId: Address;
  royaltyClaimDetails: RoyaltyClaimDetail[];
  claimer?: Address;
  txOptions?: TxOptions;
};
```
```typescript Response Type
export type TransferToVaultAndSnapshotAndClaimByTokenBatchResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  snapshotId?: bigint;
  amountsClaimed?: bigint;
};
```
