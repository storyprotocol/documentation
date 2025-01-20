---
title: How to Tip an IP
excerpt: Learn how to tip an IP Asset using the SDK and Smart Contracts.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
* [Use the SDK](https://docs.story.foundation/docs/how-to-tip-an-ip#using-the-sdk)
* [Use a Smart Contract](https://docs.story.foundation/docs/how-to-tip-an-ip#using-a-smart-contract)

# Using the SDK

<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/registerDerivativeCommercial.ts" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    See a completed, working example of setting up a simple derivative chain and then tipping the child IP Asset.
  </Card>
</Cards>

In this tutorial, you will learn how to send money ("tip") an IP Asset using the TypeScript SDK.

## The Explanation

In this scenario, let's say there is a parent IP Asset that represents Mickey Mouse. Someone else draws a hat on that Mickey Mouse and registers it as a derivative (or "child") IP Asset. The License Terms specify that the child must share 50% of all commercial revenue (`commercialRevShare = 50`) with the parent. Someone else (a 3rd party user) comes along and wants to send the derivative 2 SUSD for being really cool.

For the purposes of this example, we will assume the child is already registered as a derivative IP Asset. If you want help learning this, check out [Register a Derivative](doc:register-a-derivative).

* Parent IP ID: `0x42595dA29B541770D9F9f298a014bF912655E183`
* Child IP ID: `0xeaa4Eed346373805B377F5a4fe1daeFeFB3D182a`

## 0. Before you Start

There are a few steps you have to complete before you can start the tutorial.

1. Add your Story Network Testnet wallet's private key to `.env` file:

```text env
WALLET_PRIVATE_KEY=<YOUR_WALLET_PRIVATE_KEY>
```

2. Add your preferred RPC URL to your `.env` file. You can just use the public default one we provide:

```text env
RPC_PROVIDER_URL=https://rpc.odyssey.storyrpc.io
```

3. Install the dependencies:

```Text Terminal
npm install @story-protocol/core-sdk viem
```

## 1. Set up your Story Config

In a `utils.ts` file, add the following code to set up your Story Config:

* Associated docs: [TypeScript SDK Setup](doc:typescript-sdk-setup)

```typescript utils.ts
import { StoryClient, StoryConfig } from '@story-protocol/core-sdk'
import { http } from 'viem'
import { privateKeyToAccount, Address, Account } from 'viem/accounts'

const privateKey: Address = `0x${process.env.WALLET_PRIVATE_KEY}`
export const account: Account = privateKeyToAccount(privateKey)

const config: StoryConfig = {  
  account: account,  
  transport: http(process.env.RPC_PROVIDER_URL),  
  chainId: 'odyssey',  
}  
export const client = StoryClient.newClient(config)
```

## 2. Tipping the Derivative IP Asset

Now create a `main.ts` file. We will use the `payRoyaltyOnBehalf` function to pay the derivative asset. In this case:

1. `receiverIpId` is the `ipId` of the derivative asset
2. `payerIpId` is `zeroAddress` because the payer is a 3rd party (someone that thinks Mickey Mouse with a hat on him is cool), and not necessarily another IP Asset
3. `token` is the address of SUSD, which is currently the only [whitelisted revenue token](https://docs.story.foundation/docs/ip-royalty-vault#whitelisted-revenue-tokens)
4. `amount` is 2, since the person tipping wants to send 2 SUSD

```typescript main.ts
import { client } from './utils'
import { zeroAaddress } from 'viem'

async function main() {
   const response = await client.royalty.payRoyaltyOnBehalf({
    receiverIpId: '0xeaa4Eed346373805B377F5a4fe1daeFeFB3D182a',
    payerIpId: zeroAddress,
    token: '0x91f6F05B08c16769d3c85867548615d270C42fC7',
    amount: 2,
    txOptions: { waitForTransaction: true },
  })
  console.log(`Paid royalty at transaction hash ${response.txHash}`) 
}

main();
```

## 3. Parent Claiming Due Revenue

At this point we have already finished the tutorial: we learned how to tip an IP Asset. But what if the parent wants to claim their due revenue? In this example, the parent should be able to claim 1 SUSD since the child earned 2 SUSD and the `commercialRevShare = 50` in the license terms.

We will use the `transferToVaultAndSnapshotAndClaimByTokenBatch` to claim the due revenue tokens.

1. `ancestorIpId` is the `ipId` of the parent ("ancestor") asset
2. `claimer` is the address that holds the royalty tokens associated with the parent's [IP Royalty Vault](doc:ip-royalty-vault). By default, they are in the IP Account, which is just the `ipId` of the parent asset
3. `childIpId` is obviously the `ipId` of the child asset
4. `royaltyPolicy` is the address of the royalty policy. As explained in [💸 Royalty Module](doc:royalty-module), this is either `RoyaltyPolicyLAP` or `RoyaltyPolicyLRP`, depending on the license terms. In this case, let's assume the license terms specify a `RoyaltyPolicyLAP`. Simply go to [Deployed Smart Contracts](doc:deployed-smart-contracts) and find the correct address.
5. `currencyToken` is the address of SUSD, which is currently the only [whitelisted revenue token](https://docs.story.foundation/docs/ip-royalty-vault#whitelisted-revenue-tokens)
6. `amount` is 1, since the parent should have 1 SUSD available to claim

```typescript main.ts
import { client } from './utils'
import { zeroAaddress } from 'viem'

async function main() {
  // previous code here ...

  const response = await client.royalty.transferToVaultAndSnapshotAndClaimByTokenBatch({
    ancestorIpId: '0x42595dA29B541770D9F9f298a014bF912655E183',
    claimer: '0x42595dA29B541770D9F9f298a014bF912655E183',
    royaltyClaimDetails: [{ 
      childIpId: '0xeaa4Eed346373805B377F5a4fe1daeFeFB3D182a', 
      royaltyPolicy: '0x793Df8d32c12B0bE9985FFF6afB8893d347B6686', 
      currencyToken: '0x91f6F05B08c16769d3c85867548615d270C42fC7', 
      amount: 1 
    }],
    txOptions: { waitForTransaction: true },
  })
  console.log(`Claimed revenue: ${response.amountsClaimed} at snapshotId ${response.snapshotId}`)
}

main();
```

## 4. Done!

<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/registerDerivativeCommercial.ts" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    See a completed, working example of setting up a simple derivative chain and then tipping the child IP Asset.
  </Card>
</Cards>

# Using a Smart Contract

> 🚧 Not Completed
>
> A full written tutorial is coming soon. For now, you can see a code example at the bottom of [this file in our boilerplate](https://github.com/storyprotocol/story-protocol-boilerplate/blob/main/test/5_Royalty.t.sol).