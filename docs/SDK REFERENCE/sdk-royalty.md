---
title: Royalty
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
## RoyaltyClient

### Methods

* payRoyaltyOnBehalf
* claimableRevenue
* claimRevenue
* snapshot
* transferToVaultAndSnapshotAndClaimByTokenBatch
* transferToVaultAndSnapshotAndClaimBySnapshotBatch
* snapshotAndClaimByTokenBatch
* snapshotAndClaimBySnapshotBatch
* getRoyaltyVaultAddress

### payRoyaltyOnBehalf

Allows the function caller to pay royalties to the receiver IP asset on behalf of the payer IP asset.

| Method               | Type                                                                          |
| -------------------- | ----------------------------------------------------------------------------- |
| `payRoyaltyOnBehalf` | `(request: PayRoyaltyOnBehalfRequest) => Promise<PayRoyaltyOnBehalfResponse>` |

Parameters:

* `request.receiverIpId`: The ipId that receives the royalties.
* `request.payerIpId`: The ID of the IP asset that pays the royalties.
* `request.token`: The token to use to pay the royalties.
* `request.amount`: The amount to pay.
* `request.txOptions`: [Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type PayRoyaltyOnBehalfResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
};
```

### claimableRevenue

Calculates the amount of revenue token claimable by a token holder at certain snapshot.

| Method             | Type                                                                      |
| ------------------ | ------------------------------------------------------------------------- |
| `claimableRevenue` | `(request: ClaimableRevenueRequest) => Promise<ClaimableRevenueResponse>` |

Parameters:

* `request.royaltyVaultIpId`: The id of the royalty vault.
* `request.account`: The address of the token holder.
* `request.snapshotId`: The snapshot id.
* `request.token`: The revenue token to claim.
* `request.txOptions`: [Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type ClaimableRevenueResponse = bigint;
```

### claimRevenue

Allows token holders to claim by a list of snapshot ids based on the token balance at certain snapshot.

| Method         | Type                                                              |
| -------------- | ----------------------------------------------------------------- |
| `claimRevenue` | `(request: ClaimRevenueRequest) => Promise<ClaimRevenueResponse>` |

Parameters:

* `request.snapshotIds`: The list of snapshot ids.
* `request.royaltyVaultIpId`: The id of the royalty vault.
* `request.token`: The revenue token to claim.
* `request.account`: [Optional]The ipId of the IP Account you want to claim to instead of the wallet running the transaction.
* `request.txOptions`: [Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type ClaimRevenueResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  claimableToken?: bigint;
};
```

### snapshot

Snapshots the claimable revenue and royalty token amounts.

| Method     | Type                                                      |
| ---------- | --------------------------------------------------------- |
| `snapshot` | `(request: SnapshotRequest) => Promise<SnapshotResponse>` |

Parameters:

* `request`: The request object that contains all data needed to snapshot.
* `request.royaltyVaultIpId`: The id of the royalty vault.
* `request.txOptions`: [Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type SnapshotResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  snapshotId?: bigint;
};
```

### transferToVaultAndSnapshotAndClaimByTokenBatch

Transfers royalties from royalty policy to the ancestor IP's royalty vault, takes a snapshot, and claims revenue on that snapshot for each specified currency token.

| Method                                           | Type                                                                                                                                  |
| ------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------- |
| `transferToVaultAndSnapshotAndClaimByTokenBatch` | `(request: TransferToVaultAndSnapshotAndClaimByTokenBatchRequest) => Promise<TransferToVaultAndSnapshotAndClaimByTokenBatchResponse>` |

Parameters:

* `request.ancestorIpId`: The ipId of the ancestor IP.
* `request.royaltyClaimDetails[]`: The details of the royalty claim from child IPs
  * `request.royaltyClaimDetails.childIpId`: The address of the child IP.
  * `request.royaltyClaimDetails.royaltyPolicy`: The address of the royalty policy.
  * `request.royaltyClaimDetails.currencyToken`: The address of the currency (revenue) token to claim.
* `request.claimer`: [Optional] The address of the claimer of the revenue tokens (must be a royalty token holder), default value is wallet address.
* `request.txOptions`: [Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type TransferToVaultAndSnapshotAndClaimByTokenBatchResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  snapshotId?: bigint;
  amountsClaimed?: bigint;
};
```

### transferToVaultAndSnapshotAndClaimBySnapshotBatch

Transfers royalties to the ancestor IP's royalty vault, takes a snapshot, claims revenue for each

| Method                                              | Type                                                                                                                                        |
| --------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| `transferToVaultAndSnapshotAndClaimBySnapshotBatch` | `(request: TransferToVaultAndSnapshotAndClaimBySnapshotBatchRequest) => Promise<TransferToVaultAndSnapshotAndClaimBySnapshotBatchResponse>` |

Parameters:

* `request.ancestorIpId`: The ipId of the ancestor IP.
* `request.unclaimedSnapshotIds`: The IDs of the unclaimed snapshots to include in the claim.
* `request.royaltyClaimDetails[]`: The details of the royalty claim from child IPs
  * `request.royaltyClaimDetails.childIpId`: The address of the child IP.
  * `request.royaltyClaimDetails.royaltyPolicy`: The address of the royalty policy.
  * `request.royaltyClaimDetails.currencyToken`: The address of the currency (revenue) token to claim.
* `request.claimer`: [Optional] The address of the claimer of the revenue tokens (must be a royalty token holder), default value is wallet address.
* `request.txOptions`: [Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type TransferToVaultAndSnapshotAndClaimBySnapshotBatchResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  snapshotId?: bigint;
  amountsClaimed?: bigint;
};
```

### snapshotAndClaimByTokenBatch

Takes a snapshot of the IP's royalty vault and claims revenue on that snapshot for each

| Method                         | Type                                                                                              |
| ------------------------------ | ------------------------------------------------------------------------------------------------- |
| `snapshotAndClaimByTokenBatch` | `(request: SnapshotAndClaimByTokenBatchRequest) => Promise<SnapshotAndClaimByTokenBatchResponse>` |

Parameters:

* `request.royaltyVaultIpId`: The address of the IP.
* `request.currencyTokens`: The addresses of the currency (revenue) tokens to claim.
* `request.claimer`: [Optional] The address of the claimer of the revenue tokens (must be a royalty token holder), default value is wallet address.
* `request.txOptions`: [Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type SnapshotAndClaimByTokenBatchResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  snapshotId?: bigint;
  amountsClaimed?: bigint;
};
```

### snapshotAndClaimBySnapshotBatch

Takes a snapshot of the IP's royalty vault and claims revenue for each specified currency token

| Method                            | Type                                                                                                    |
| --------------------------------- | ------------------------------------------------------------------------------------------------------- |
| `snapshotAndClaimBySnapshotBatch` | `(request: SnapshotAndClaimBySnapshotBatchRequest) => Promise<SnapshotAndClaimBySnapshotBatchResponse>` |

Parameters:

* `request.royaltyVaultIpId`: The address of the IP.
* `request.unclaimedSnapshotIds`: The IDs of unclaimed snapshots to include in the claim.
* `request.currencyTokens`: The addresses of the currency (revenue) tokens to claim.
* `request.claimer`: [Optional] The address of the claimer of the revenue tokens (must be a royalty token holder), default value is wallet address.
* `request.txOptions`: [Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type SnapshotAndClaimBySnapshotBatchResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  snapshotId?: bigint;
  amountsClaimed?: bigint;
};
```

### getRoyaltyVaultAddress

Get the royalty vault proxy address of given royaltyVaultIpId.

| Method                   | Type                                          |
| ------------------------ | --------------------------------------------- |
| `getRoyaltyVaultAddress` | `(royaltyVaultIpId: Hex) => Promise<Address>` |

Parameters:

* `royaltyVaultIpId`: the `ipId` associated with the royalty vault.
