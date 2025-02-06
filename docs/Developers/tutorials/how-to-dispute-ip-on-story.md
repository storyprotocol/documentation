---
title: How to Dispute an IP on Story
excerpt: Learn how to dispute an IP on Story.
deprecated: false
hidden: false
metadata:
  robots: index
---
* [Use the SDK](https://docs.story.foundation/docs/how-to-dispute-ip-on-story#using-the-sdk)
* [Use a Smart Contract](https://docs.story.foundation/docs/how-to-dispute-ip-on-story#using-a-smart-contract)

# Using the SDK

<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/disputeIp.ts" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    View the completed code for this tutorial.
  </Card>
</Cards>

In this tutorial, you will learn how to dispute an IP on Story using the TypeScript SDK.

## The Explanation

There are many instances where you may want to dispute an IP - whether that IP is or is not owned by you. Disputing IP on Story is easy thanks to our [❌ Dispute Module](doc:dispute-module) and the [UMA Arbitration Policy](doc:uba-arbitration-policy).

Let's say you register a drawing, and then someone else registers that drawing with 1 pixel off. You can dispute it along a `IMPROPER_REGISTRATION` tag, which communicates potential plagiarism.

In this tutorial, you will simply learn how to flag an IP as being disputed.

## 0. Before you Start

There are a few steps you have to complete before you can start the tutorial.

1. You will need to install [Node.js](https://nodejs.org/en/download) and [npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm). If you've coded before, you likely have these.
2. Add your Story Network Testnet wallet's private key to `.env` file:

```yaml .env
WALLET_PRIVATE_KEY=<YOUR_WALLET_PRIVATE_KEY>
```

3. Add your preferred RPC URL to your `.env` file. You can just use the public default one we provide:

```yaml .env
RPC_PROVIDER_URL=https://aeneid.storyrpc.io
```

4. Install the dependencies:

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
  chainId: 'aeneid',  
}
export const client = StoryClient.newClient(config)
```

## 2. Dispute an IP

To dispute an IP Asset, you will need:

* the `ipId` of the IP Asset you are disputing (we use a test one below)
* the `targetTag` that you are applying to the dispute. Check out the [list of tags](https://docs.story.foundation/docs/dispute-module#/dispute-tags) that can be applied.
* a `cid` (Content Identifier) is a unique identifier in IPFS, including CID v0 (base58) and CID v1 (base32). It represents the dispute evidence you must provide, as described [here](https://docs.story.foundation/docs/uma-arbitration-policy#/dispute-evidence-submission-guidelines) (we use a test one below).

Create a `main.ts` file and add the code below:

```typescript main.ts
import { client } from './utils'

async function main() {
  const disputeResponse = await client.dispute.raiseDispute({
    targetIpId: '0x6b42d065aDCDA6fA83B59ad731841360dC5321fB',
    // this is "PLAGIARISM" in base32, and is currently the only whitelisted
    // tag for protocol v1.2
    targetTag: '0x504c414749415249534d00000000000000000000000000000000000000000000',
    cid: 'QmbWqxBEKC3P8tqsKc98xmWNzrzDtRLMiMPL8wBuTGsMnR',
  })
  console.log(`Dispute raised at transaction hash ${disputeResponse.txHash}, Dispute ID: ${disputeResponse.disputeId}`) 
}

main();
```

## 3. Done!

<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/disputeIp.ts" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    Follow the completed code all the way through step 5a.
  </Card>
</Cards>

# Using a Smart Contract

> 🚧 Coming soon...