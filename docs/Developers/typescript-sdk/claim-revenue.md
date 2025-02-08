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
* Obviously, there must be a payment to be claimed. Read [Pay an IPA in TypeScript](doc:pay-ipa)

# Claiming Revenue Tokens

There are two main ways revenue can be claimed:

1. **Scenario #1**: Someone pays my IP Asset directly, and I claim that revenue.
2. **Scenario #2**: Someone pays a derivative IP Asset of my IP, and I have the right to a % of their revenue based on the `commercialRevShare` in the license terms.

> 🚧 Quick Note
>
> The below examples show and talk about USDC. In reality, you'd be using one of the whitelisted revenue tokens [listed here](https://docs.story.foundation/docs/deployed-smart-contracts#/), which is more often than not $WIP.

## Scenario #1

In this scenario, someone pays my IP Asset directly, and I claim that revenue.

![](https://files.readme.io/a39e345a512747ff414668309a80ec1b01048c238bf1adef4d0760d830006cbd-image.png)

As we can see in the above diagram, when IP Asset 4 (it doesn't have to be an IP Asset, it can be any address) pays IP Asset 3 1M USDC, 850k USDC automatically gets deposited into IP Royalty Vault 3. Below is how IP Asset 3 would claim their revenue:

```typescript TypeScript
const claimRevenue = await client.royalty.claimAllRevenue({
  // IP Asset 3's ipId
  ancestorIpId: '0xDa03c4B278AD44f5a669e9b73580F91AeDE0E3B2',
  // whoever owns the royalty tokens associated with IP Royalty Vault 3
  // (most likely the associated ipId, which is IP Asset 3's ipId)
  claimer: '0xDa03c4B278AD44f5a669e9b73580F91AeDE0E3B2',
  // testnet address of $WIP
  currencyTokens: ['0x1514000000000000000000000000000000000000'],
  childIpIds: [],
  royaltyPolicies: []
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

## Scenario #2

In this scenario, someone pays a derivative IP Asset of my IP, and I have the right to a % of their revenue based on the `commercialRevShare` in the license terms. This is exactly the same as Scenario #1, except one extra step is added.

![](https://files.readme.io/04a92b164d8f25119881efe784aed4bb539cdd955f553d74191b513ac0623b68-image.png)

Similar to Scenario #1, as we can see in the above diagram, when IP Asset 4 (it doesn't have to be an IP Asset, it can be any address) pays IP Asset 3 1M USDC, 150k USDC automatically gets deposited to the LAP royalty policy contract to be distributed to ancestors.

![](https://files.readme.io/8f11792c3e3d2e25f74a469996b9c000c61eac167d9512f9a0f353bf62b77abf-image.png)

Then, in a second step, the USDC is transferred to the ancestors' [IP Royalty Vault](doc:ip-royalty-vault) based on the negotiated `commercialRevShare` in the license terms.

Below is how IP Asset 1 (or 2) would claim their revenue:

```typescript TypeScript
const claimRevenue = await client.royalty.claimAllRevenue({
  // IP Asset 1's ipId
  ancestorIpId: '0x089d75C9b7E441dA3115AF93FF9A855BDdbfe384',
  // whoever owns the royalty tokens associated with IP Royalty Vault 1
  // (most likely the associated ipId, which is IP Asset 1's ipId)
  claimer: '0x089d75C9b7E441dA3115AF93FF9A855BDdbfe384',
  // testnet address of $WIP
  currencyTokens: ['0x1514000000000000000000000000000000000000'],
  // IP Asset 3's ipId
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