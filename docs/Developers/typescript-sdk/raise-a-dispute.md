---
title: Raise a Dispute
excerpt: Learn how to create an on-chain dispute in TypeScript.
deprecated: false
hidden: false
metadata:
  robots: index
---
<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/disputeIp.ts" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    All of this page is covered in this working code example.
  </Card>
</Cards>

This section demonstrates how to dispute an IP on Story. There are many instances where you may want to dispute an IP - whether that IP is or is not owned by you. Disputing IP on Story is easy thanks to our [❌ Dispute Module](doc:dispute-module) and the [UMA Arbitration Policy](doc:uma-arbitration-policy).

Let's say you register a drawing, and then someone else registers that drawing with 1 pixel off. You can dispute it along a `IMPROPER_REGISTRATION` tag, which communicates potential plagiarism.

In this tutorial, you will simply learn how to flag an IP as being disputed.

### :warning: Prerequisites

There are a few steps you have to complete before you can start the tutorial.

1. Complete the [TypeScript SDK Setup](doc:typescript-sdk-setup)
2. Have a basic understanding of the [❌ Dispute Module](doc:dispute-module)

## 1. Dispute an IP

To dispute an IP Asset, you will need:

* the `targetIpId` of the IP Asset you are disputing (we use a test one below)
* the `targetTag` that you are applying to the dispute. Only [whitelisted tags](https://docs.story.foundation/docs/dispute-module#dispute-tags) can be applied.
* a `cid` (Content Identifier) is a unique identifier in IPFS that represents the dispute evidence you must provide, as described [here](https://docs.story.foundation/docs/uma-arbitration-policy#dispute-evidence-submission-guidelines) (we use a test one below).
  * :warning: **Note you can only provide a CID one time.** After it is used, it can't be used as evidence again.

Create a `main.ts` file and add the code below:

```typescript main.ts
import { client } from './utils'

async function main() {
  const disputeResponse = await client.dispute.raiseDispute({
    targetIpId: '0x6b42d065aDCDA6fA83B59ad731841360dC5321fB',
    // NOTE: you must use your own CID here, because every time it is used,
    // the protocol does not allow you to use it again
    cid: 'QmbWqxBEKC3P8tqsKc98xmWNzrzDtRLMiMPL8wBuTGsMnR',
    // you must pick from one of the whitelisted tags here: https://docs.story.foundation/docs/dispute-module#dispute-tags
    targetTag: 'IMPROPER_REGISTRATION',
    bond: 0,
    liveness: 2592000,
    txOptions: { waitForTransaction: true },
  })
  console.log(`Dispute raised at transaction hash ${disputeResponse.txHash}, Dispute ID: ${disputeResponse.disputeId}`) 
}

main();
```

## 2. :checkered_flag: View Completed Code

<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/disputeIp.ts" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    See a completed, working example disputing an IP.
  </Card>
</Cards>