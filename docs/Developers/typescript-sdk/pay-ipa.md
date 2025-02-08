---
title: Pay an IPA
excerpt: Learn how to pay an IP Asset in TypeScript.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
This section demonstrates how to pay an IP Asset in TypeScript.

There are a few reasons you would pay an IP Asset:

* you simply want to "tip" an IP
* you have to because your license terms with that IP require you to forward a certain % of payment

In either scenario, you would use the below `payRoyaltyOnBehalf` function. When this happens, the Royalty Module automatically handles the different payment flows such that parent IP Assets who have negotiated a certain `commercialRevShare` with the IPA being paid can claim their due share. This is covered in ...

## Prerequisites

* [Setup](doc:typescript-sdk-setup) the client object.
* Understand the [💸 Royalty Module](doc:royalty-module)

# Paying an IP Asset

You can pay an IP Asset using the `payRoyaltyOnBehalf` function.

You will be paying the IP Asset with [$WIP](https://aeneid.storyscan.xyz/address/0x1514000000000000000000000000000000000000). **Note that if you don't have enough $WIP, the function will auto wrap an equivalent amount of $IP into $WIP for you.** If you don't have enough of either, it will fail.

> 📘 Whitelisted Revenue Tokens
>
> Only tokens that are whitelisted by our protocol can be used as payment ("revenue") tokens. $WIP is one of those tokens. To see that list, go [here](https://docs.story.foundation/docs/deployed-smart-contracts#/).

To help with the following scenarios, let's say we have a parent IP Asset that has negotiated a 50% `commercialRevShare` with its child IP Asset.

There are two situations where you'd be calling this function:

1. Tipping an IP Asset
2. Paying your due share to an ancestor

## Tipping an IP Asset

In this scenario, you're an external 3rd-party user who wants to pay the child 2 $WIP for being cool. When you call the function below, you should make `payerIpId` a zero address because you are not paying on behalf of an IP Asset. Additionally, you would set `amount` to 2.

Due to the existing license terms that specify 50% `commercialRevShare`, 50% of the revenue (2\*0.5 = 1) would automatically be claimable by the parent thanks to the [💸 Royalty Module](doc:royalty-module), such that both the parent and child IP Assets earn 1 $WIP.

```typescript TypeScript
const payRoyalty = await client.royalty.payRoyaltyOnBehalf({
  receiverIpId: "0x0b825D9E5FA196e6B563C0a446e8D9885057f9B1", // child ipId
  payerIpId: zeroAddress,
  token: "0x1514000000000000000000000000000000000000", // insert WIP address from https://docs.story.foundation/docs/deployed-smart-contracts
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
  amount: string | number | bigint;
  txOptions?: TxOptions;
};
```
```typescript Response Type
export type PayRoyaltyOnBehalfResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
};
```

## Paying Due Share

In this scenario, lets say the child IP Asset earned 2 USD off-chain. Because the child owes the parent IP Asset 50% of its revenue, it needs to send 1 $USD to the parent *(for this example, let's just assume 1 $WIP = 1 USD)*.

> 📘 Complex Royalty Graphs
>
> Let's say the child earned 1,000 USD off-chain, and is linked to huge ancestor tree where each parent has a different set of complex license terms. In this scenario, you won't be able to individually calculate each payment to each parent. Instead, you would just pay *yourself* the amount you earned, and the [💸 Royalty Module](doc:royalty-module) will automate the payment, such that each ancestor gets their due share.

```typescript TypeScript
const payRoyalty = await client.royalty.payRoyaltyOnBehalf({
  receiverIpId: "0xDa03c4B278AD44f5a669e9b73580F91AeDE0E3B2", // parentIpId
  payerIpId: "0x0b825D9E5FA196e6B563C0a446e8D9885057f9B1", // childIpId
  token: "0x1514000000000000000000000000000000000000", // insert WIP address from https://docs.story.foundation/docs/deployed-smart-contracts
  amount: 1,
  txOptions: { waitForTransaction: true },
});
console.log(`Paid royalty at transaction hash ${payRoyalty.txHash}`);
```
```typescript Request Type
export type PayRoyaltyOnBehalfRequest = {
  receiverIpId: Address;
  payerIpId: Address;
  token: Address;
  amount: string | number | bigint;
  txOptions?: TxOptions;
};
```
```typescript Response Type
export type PayRoyaltyOnBehalfResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
};
```