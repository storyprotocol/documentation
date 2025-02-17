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
<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/registerDerivativeCommercialSpg.ts" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    All of this page is covered in this working code example.
  </Card>
</Cards>

This section demonstrates how to pay an IP Asset. There are a few reasons you would do this:

1. you simply want to "tip" an IP
2. you have to because your license terms with an ancestor IP require you to forward a certain % of payment

In either scenario, you would use the below `payRoyaltyOnBehalf` function. When this happens, the [💸 Royalty Module](doc:royalty-module) automatically handles the different payment flows such that parent IP Assets who have negotiated a certain `commercialRevShare` with the IPA being paid can claim their due share.

### :warning: Prerequisites

There are a few steps you have to complete before you can start the tutorial.

1. Complete the [TypeScript SDK Setup](doc:typescript-sdk-setup)
2. Have a basic understanding of the [💸 Royalty Module](doc:royalty-module)

## Before We Start

You can pay an IP Asset using the `payRoyaltyOnBehalf` function.

You will be paying the IP Asset with [$WIP](https://aeneid.storyscan.xyz/address/0x1514000000000000000000000000000000000000). **Note that if you don't have enough $WIP, the function will auto wrap an equivalent amount of $IP into $WIP for you.** If you don't have enough of either, it will fail.

To help with the following scenarios, let's say we have a parent IP Asset that has negotiated a 50% `commercialRevShare` with its child IP Asset.

### :coin: Whitelisted Revenue Tokens

Only tokens that are whitelisted by our protocol can be used as payment ("revenue") tokens. $WIP is one of those tokens. To see that list, go [here](https://docs.story.foundation/docs/deployed-smart-contracts#whitelisted-revenue-tokens).

> ❗️ Testing: MockERC20
>
> If you want to test paying IP Assets, you'll probably want a whitelisted revenue token you can mint freely for testing. We have provided [MockERC20](https://aeneid.storyscan.xyz/address/0xF2104833d386a2734a4eB3B8ad6FC6812F29E38E?tab=write_contract#0x40c10f19) on Aeneid testnet which you can mint and pay with.
>
> Note, however, that in order for the SDK to be able to spend MockERC20 for you, you must [approve it](https://aeneid.storyscan.xyz/address/0xF2104833d386a2734a4eB3B8ad6FC6812F29E38E?tab=write_contract#0x095ea7b3) where the `spender` is `0xD2f60c40fEbccf6311f8B47c4f2Ec6b040400086` (the address of `RoyaltyModule.sol` on Aeneid testnet) and the value is ≥ the amount you want to pay.

## Scenario #1: Tipping an IP Asset

In this scenario, you're an external 3rd-party user who wants to pay an IP Asset 2 $WIP for being cool. When you call the function below, you should make `payerIpId` a zero address because you are not paying on behalf of an IP Asset. Additionally, you would set `amount` to 2.

> Associated Docs: [royalty.payRoyaltyOnBehalf](https://docs.story.foundation/docs/sdk-royalty#payroyaltyonbehalf)

```typescript main.ts
import { WIP_TOKEN_ADDRESS } from '@story-protocol/core-sdk'
// you should already have a client set up (prerequisite)
import { client } from './utils'

async function main() {
  const payRoyalty = await client.royalty.payRoyaltyOnBehalf({
    receiverIpId: "0x0b825D9E5FA196e6B563C0a446e8D9885057f9B1", // the ip you're paying
    payerIpId: zeroAddress,
    token: WIP_TOKEN_ADDRESS,
    amount: 2,
    txOptions: { waitForTransaction: true },
  });

  console.log(`Paid royalty at transaction hash ${payRoyalty.txHash}`);
}

main();
```

Let's say the IP Asset you're paying is a derivative. And due to existing license terms with a parent that specify 50% `commercialRevShare`, 50% of the revenue (2\*0.5 = 1) would automatically be claimable by the parent thanks to the [💸 Royalty Module](doc:royalty-module), such that both the parent and child IP Assets earn 1 $WIP. We'll go over this on the next page.

## Scenario #2: Paying Due Share

In this scenario, lets say a derivative IP Asset earned 2 USD off-chain. Because the derivative owes the parent IP Asset 50% of its revenue, it could give the parent 1 USD off-chain and be ok. Or, it can send 1 $USD equivalent to the parent on-chain *(for this example, let's just assume 1 $WIP = 1 USD)*.

> Associated Docs: [royalty.payRoyaltyOnBehalf](https://docs.story.foundation/docs/sdk-royalty#payroyaltyonbehalf)

```typescript main.ts
import { WIP_TOKEN_ADDRESS } from '@story-protocol/core-sdk'
// you should already have a client set up (prerequisite)
import { client } from './utils'

async function main() {
  const payRoyalty = await client.royalty.payRoyaltyOnBehalf({
    receiverIpId: "0xDa03c4B278AD44f5a669e9b73580F91AeDE0E3B2", // parentIpId
    payerIpId: "0x0b825D9E5FA196e6B563C0a446e8D9885057f9B1", // childIpId
    token: WIP_TOKEN_ADDRESS,
    amount: 1,
    txOptions: { waitForTransaction: true },
  });

  console.log(`Paid royalty at transaction hash ${payRoyalty.txHash}`);
}

main();
```

### :globe_with_meridians: Complex Royalty Graphs

Let's say the child earned 1,000 USD off-chain, and is linked to a huge ancestor tree where each parent has a different set of complex license terms. In this scenario, you won't be able to individually calculate each payment to each parent. Instead, you would just pay *yourself* the amount you earned, and the [💸 Royalty Module](doc:royalty-module) will automate the payment, such that each ancestor gets their due share.

## :checkered_flag: View Completed Code

Congratulations, you paid an IP Asset on-chain!

<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/registerDerivativeCommercialSpg.ts" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    All of this page is covered in this working code example.
  </Card>
</Cards>

## Claiming Revenue

Now that we have paid revenue, we need to learn how to claim it! We will cover that on the next page.