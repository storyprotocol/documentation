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

Allows the function caller to pay royalties to a receiver IP asset on behalf of the payer IP Asset.

| Method               | Type                                                                          |
| -------------------- | ----------------------------------------------------------------------------- |
| `payRoyaltyOnBehalf` | `(request: PayRoyaltyOnBehalfRequest) => Promise<PayRoyaltyOnBehalfResponse>` |

Parameters:

* `request.receiverIpId`: The ipId that receives the royalties.
* `request.payerIpId`: The ID of the IP asset that pays the royalties.
* `request.token`: The token to use to pay the royalties.
* `request.amount`: The amount to pay.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript TypeScript
import { WIP_TOKEN_ADDRESS } from '@story-protocol/core-sdk'

// In this case, lets say there is a root IPA 'A' and a derivative IPA 'B'.
// Someone wants to pay 'B' for whatever reason (they bought it, they want to tip it, etc).
// Since the payer is not an IP Asset (rather an external user), the `payerIpId` can
// be a zeroAddress. And the receiver is, well, the receiver's ipId which is B.
//
// It's important to note that both 'B' and its parent 'A' will be able
// to claim revenue from this based on the negotiated license terms
const payRoyalty = await client.royalty.payRoyaltyOnBehalf({
  receiverIpId: "0x0b825D9E5FA196e6B563C0a446e8D9885057f9B1", // B's ipId
  payerIpId: zeroAddress,
  token: WIP_TOKEN_ADDRESS,
  amount: 2,
  txOptions: { waitForTransaction: true },
});
console.log(`Paid royalty at transaction hash ${payRoyalty.txHash}`);

// In this case, lets say there is a root IPA 'A' and a derivative IPA 'B'.
// 'B' earns revenue off-chain, but must pay 'A' based on their negotiated license terms.
// So 'B' pays 'A' what they are due
const payRoyalty = await client.royalty.payRoyaltyOnBehalf({
  receiverIpId: "0x6B86B39F03558A8a4E9252d73F2bDeBfBedf5b68", // A's ipId
  payerIpId: "0x0b825D9E5FA196e6B563C0a446e8D9885057f9B1", // B's ipId
  token: WIP_TOKEN_ADDRESS,
  amount: 2,
  txOptions: { waitForTransaction: true },
});
console.log(`Paid royalty at transaction hash ${payRoyalty.txHash}`);
```
```typescript Request Type
export type PayRoyaltyOnBehalfRequest = {
  receiverIpId: Address;
  payerIpId: Address;
  token: Address;
  amount: TokenAmountInput;
} & WithTxOptions & WithWipOptions;
```
```typescript Response Type
export type PayRoyaltyOnBehalfResponse = {
  txHash?: string;
  receipt?: TransactionReceipt;
  encodedTxData?: EncodedTxData;
};
```

### claimableRevenue

Get total amount of revenue token claimable by a royalty token holder.

| Method             | Type                                                                      |
| ------------------ | ------------------------------------------------------------------------- |
| `claimableRevenue` | `(request: ClaimableRevenueRequest) => Promise<ClaimableRevenueResponse>` |

Parameters:

* `request.royaltyVaultIpId`: The id of the royalty vault.
* `request.claimer`: The address of the royalty token holder.
* `request.token`: The revenue token to claim.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Request Type
export type ClaimableRevenueRequest = {
  royaltyVaultIpId: Address;
  claimer: Address;
  token: Address;
}
```
```typescript Response Type
export type ClaimableRevenueResponse = bigint;
```

### claimAllRevenue

Claims all revenue from child IP Assets and/or from your own IP Royalty Vault.

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

```typescript TypeScript
import { WIP_TOKEN_ADDRESS } from '@story-protocol/core-sdk'

const claimRevenue = await client.royalty.claimAllRevenue({
  // IP Asset 1's (parent) ipId
  ancestorIpId: '0x089d75C9b7E441dA3115AF93FF9A855BDdbfe384',
  // whoever owns the royalty tokens associated with IP Royalty Vault 1
  // (most likely the associated ipId, which is IP Asset 1's ipId)
  claimer: '0x089d75C9b7E441dA3115AF93FF9A855BDdbfe384',
  currencyTokens: [WIP_TOKEN_ADDRESS],
  // IP Asset 2's (child) ipId
  childIpIds: ['0xDa03c4B278AD44f5a669e9b73580F91AeDE0E3B2'],
  // testnet address of RoyaltyPolicyLAP
  royaltyPolicies: ['0xBe54FB168b3c982b7AaE60dB6CF75Bd8447b390E']
})

console.log(`Claimed revenue: ${claimRevenue.claimedTokens}`);
```
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