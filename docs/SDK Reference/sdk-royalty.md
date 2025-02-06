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
* claimAllRevenue
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
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

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
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type ClaimableRevenueResponse = bigint;
```

### claimAllRevenue

Allows token holders to claim by a list of snapshot ids based on the token balance at certain snapshot.

| Method            | Type                                                                    |
| ----------------- | ----------------------------------------------------------------------- |
| `claimAllRevenue` | `(request: ClaimAllRevenueRequest) => Promise<ClaimAllRevenueResponse>` |

Parameters:

* `request.ancestorIpId`: The address of the ancestor IP from which the revenue is being claimed.
* `request.claimer`: The address of the claimer of the currency (revenue) tokens. This is normally the ipId of the ancestor IP if the IP has all royalty tokens. Otherwise, this would be the address that is holding the ancestor IP royalty tokens.
* `request.childIpIds[]`: The addresses of the child IPs from which royalties are derived.
* `request.royaltyPolicies[]`: The addresses of the royalty policies, where royaltyPolicies\[i] governs the royalty flow for childIpIds\[i].
* `request.currencyTokens[]`: The addresses of the currency tokens in which royalties will be claimed.
* `request.claimOptions`: \[Optional]
  * `request.claimOptions.autoTransferAllClaimedTokensFromIp`: \[Optional]When enabled, all claimed tokens on the claimer are transferred to the wallet address if the wallet owns the IP. If the wallet is the claimer or if the claimer is not an IP owned by the wallet, then the tokens will not be transferred. Set to false to disable auto transferring claimed tokens from the claimer. **Default: true**
  * `request.claimOptions.autoUnwrapIpTokens`: \[Optional]By default all claimed WIP tokens are converted back to IP after they are transferred. Set this to false to disable this behavior. **Default: false**
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Request Type
export type ClaimAllRevenueRequest = {
  ancestorIpId: Address;
  claimer: Address;
  childIpIds: Address[];
  royaltyPolicies: Address[];
  currencyTokens: Address[];
  claimOptions?: {
    autoTransferAllClaimedTokensFromIp?: boolean;
    autoUnwrapIpTokens?: boolean;
  };
};
```
```typescript Response Type
export type ClaimAllRevenueResponse = {
  txHashes: Hash[];
  receipt?: TransactionReceipt;
  claimedTokens?: ClaimedToken[];
};

export type ClaimedToken = {
  token: Address;
  amount: bigint;
};
```

### getRoyaltyVaultAddress

Get the royalty vault proxy address of given royaltyVaultIpId.

| Method                   | Type                                          |
| ------------------------ | --------------------------------------------- |
| `getRoyaltyVaultAddress` | `(royaltyVaultIpId: Hex) => Promise<Address>` |

Parameters:

* `royaltyVaultIpId`: the `ipId` associated with the royalty vault.