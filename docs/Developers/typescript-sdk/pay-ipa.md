---
title: Pay an IPA
excerpt: Learn how to pay an IP Asset in TypeScript.
deprecated: false
hidden: false
metadata:
  title: ""
  description: ""
  robots: index
next:
  description: ""
---

This section demonstrates how to pay an IP Asset in TypeScript.

There are a few reasons you would pay an IP Asset:

- you simply want to "tip" an IP
- you have to because your license terms with that IP require you to forward a certain % of payment

In either scenario, you would use the below `payRoyaltyOnBehalf` function. When this happens, the Royalty Module automatically handles the different payment flows such that parent IP Assets who have negotiated a certain `commercialRevShare` with the IPA being paid can claim their due share. This is covered in ...

## Prerequisites

- [Setup](doc:typescript-sdk-setup) the client object.
- Understand the [💸 Royalty Module](doc:royalty-module)

> ❗️ Important: Before you continue
>
> In order to use the `payRoyaltyOnBehalf` function below, you'll first need to know a few things.
>
> First, the only way to make payments on Story is if the ERC20 token you're using is whitelisted. One of the only whitelisted revenue tokens is $MERC20, which can be minted for test purposes and is shown [here](https://docs.story.foundation/docs/ip-royalty-vault#whitelisted-revenue-tokens).
>
> Then, once you have $MERC20, you have to approve the Royalty Module to spend them on your behalf so it can properly distribute payment to ancestor IPs. Run the [approve transaction](https://aeneid.storyscan.xyz/address/0xF2104833d386a2734a4eB3B8ad6FC6812F29E38E?tab=write_contract#0x095ea7b3) where the `spender` is `0xD2f60c40fEbccf6311f8B47c4f2Ec6b040400086`, which is the v1.3 (current deployment supported by the SDK) address of `RoyaltyModule.sol` [here](https://docs.story.foundation/docs/deployed-smart-contracts). And the value is >= the amount you're paying with the SDK.

# Paying an IP Asset

You can pay an IP Asset using the `payRoyaltyOnBehalf` function. There are two situations where you'd be calling this function.

To help with the following scenarios, let's say we have a parent IP Asset that has negotiated a 50% `commercialRevShare` with its child IP Asset.

## Tipping an IP Asset

In this scenario, you're an external 3rd-party user who want to pay the child 2 $WIP for being cool. When you call the function below, you should make `payerIpId` a zero address because you are not paying on behalf of an IP Asset. Additionally, you would set `amount` to 2.

Due to the existing license terms that specify 50% `commercialRevShare`, 50% of the revenue (2\*0.5 = 1) would automatically be claimable by the parent thanks to the Royalty Module, such that both the parent and child IP Assets earn 1 $WIP.

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

In this scenario, lets say the child IP Asset earned 2 USD off-chain. Because the child owes the parent IP Asset 50% of its revenue, it needs to send 1 USD to the parent.

The `amount` should still be 2. This is because the Royalty Module will automate the payment, such that, like the tipping scenario, 1 $WIP would be kept for the child and 1 $WIP would be claimable by the parent.

```typescript TypeScript
const payRoyalty = await client.royalty.payRoyaltyOnBehalf({
  receiverIpId: "0xDa03c4B278AD44f5a669e9b73580F91AeDE0E3B2", //parentIpId
  payerIpId: "0x0b825D9E5FA196e6B563C0a446e8D9885057f9B1", // childIpId
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
