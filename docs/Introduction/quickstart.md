---
title: 🏁 Quickstart
excerpt: Start building on Story quickly.
deprecated: false
hidden: false
metadata:
  robots: index
---
You want to start building on Story quickly... so let's get started!

> 📘 Looking to read up on Story first?
>
> If you'd like to read up on Story before diving into the technical details, check out our awesome [Learn Hub](https://learn.story.foundation/) which will explain the who, what, and why of Story.

## :globe_with_meridians: Add Network

<Cards columns={2}>
  <Card title="Add Story Mainnet" href="https://chainid.network/chain/1514/" icon="fa-home" target="_blank">
    Connect your wallet to Story's mainnet.
  </Card>

  <Card title="Add Story 'Aeneid' Testnet" href="https://chainid.network/chain/1315/" icon="fa-home" target="_blank">
    Connect your wallet to Story's 'Aeneid' testnet.
  </Card>
</Cards>

## :fast_forward: Skip everything. Go to the code.

<Cards columns={2}>
  <Card title="TypeScript Code Example" href="https://github.com/storyprotocol/typescript-tutorial/tree/main" icon="fa-user" target="_blank">
    This is a clone-able quickstart for you to check out. You can clone it directly and follow the associated README.
  </Card>

  <Card title="Smart Contract Code Example" href="https://github.com/storyprotocol/story-protocol-boilerplate" icon="fa-user" target="_blank">
    This is a boilerplate for you to check out. You can clone it directly, study the example smart contracts, and follow the associated README for running the tests.
  </Card>
</Cards>

## :building_construction: Story Network Infra

See [Network Info](doc:network-info) for all RPC, explorer, and faucet info.

## :computer: Use our SDKs

We have built a [🛠️ TypeScript SDK](doc:typescript-sdk) with its own in-depth tutorials for popular functions and use cases.

## :gear: Deployed Smart Contracts

Check out the addresses for the deployed smart contracts [here](doc:deployed-smart-contracts). Note that there are two different kinds of contracts:

* [Story Protocol Core](https://github.com/storyprotocol/protocol-core-v1) - This repository contains the core protocol logic, consisting of a thin IP registry (the [IP Asset Registry](doc:ip-asset-registry)), a set of [🧱 Modules](doc:story-modules) defining logic around [📜 Licensing](doc:licensing-module), [💸 Royalty](doc:royalty-module), [❌ Dispute](doc:dispute-module), metadata, and a module manager for administering module and user access control.
* [Story Protocol Periphery](https://github.com/storyprotocol/protocol-periphery-v1)- Whereas the core contracts deal with the underlying protocol logic, the periphery contracts deal with protocol extensions that greatly increase UX and simplify IPA management. This is mostly handled through the [📦 SPG](doc:spg).

## 🔌 Use our API

Check out the entire [API Reference](https://docs.story.foundation/reference/api-introduction) for learning how to use our API.

## :memo: Register IP on Story

Let's start with the most basic question: *"What does it take to register IP on Story in my app? How do I do this?"*

To register IP on Story, you'll first need an NFT. If your IP is an ERC-721 NFT (ex. an Azuki or Pudgy Penguin on Story), you're already set. If not, you must mint an NFT to represent your off-chain IP. And don't worry, we'll help you do this in the following tutorials.

Next you'd register that NFT on Story, ultimately creating an [🧩 IP Asset](doc:ip-asset). An "IP Asset" is your IP registered on Story, empowered by:

* all of Story's [🧱 Modules](doc:story-modules) like transparent licensing, automatic royalty payments, and disputing of wrongfully registered IP
* IP protection through the [💊 Programmable IP License (PIL)](doc:programmable-ip-license)

Follow the below tutorials to register IP on Story:

<Cards columns={2}>
  <Card title="Using the SDK" href="https://docs.story.foundation/docs/register-an-ip-asset" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    Learn how to register IP on Story using the TypeScript SDK.
  </Card>

  <Card title="Using the Smart Contracts" href="https://docs.story.foundation/docs/sc-register-an-ip-asset" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    Learn how to register IP on Story using the Smart Contracts.
  </Card>
</Cards>

### Difference Between IP Metadata vs. NFT Metadata

A common question we get from developers while registering their IP on Story is: *"What metadata should be/is expected to be attached to the NFT, and then separately, the IP Asset?"*

To answer that question, please see [NFT vs. IP Metadata](https://docs.story.foundation/docs/ip-asset#nft-vs-ip-metadata).

## :scroll: Licensing Your IP

You may be wondering, *"How do I take advantage of Story's on-chain licensing? How do I make sure my registered IP has a license ready to go?"*

Before you attach any sort of licenses or license terms to your [🧩 IP Asset](doc:ip-asset), it would be best to first understand what the [💊 Programmable IP License (PIL)](doc:programmable-ip-license) actually is. This "PIL" is what defines the available [License Terms](doc:license-terms) on Story, which in turn - when attached to an IP Asset - is what defines how others can use (commercially, create derivatives, etc) that IP Asset.

Our tutorials will show you exactly how to attach license terms to your IP Asset:

<Cards columns={2}>
  <Card title="Using the SDK" href="https://docs.story.foundation/docs/attach-terms-to-an-ip-asset" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    Learn how to attach license terms to your IP on Story using the TypeScript SDK.
  </Card>

  <Card title="Using the Smart Contracts" href="https://docs.story.foundation/docs/sc-attach-license-terms" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    Learn how to attach license terms to your IP on Story using the Smart Contracts.
  </Card>
</Cards>

> 📘 Learn More
>
> For more information on licensing and the terminology behind it, check out the [📜 Licensing Module](doc:licensing-module).

## :money_with_wings: Royalties / Revenue Sharing

Now you may be wondering, *"How do I set up automatic royalty sharing between my IP Asset and someone else's? How do I then collect that payment?"*

When you attach [License Terms](doc:license-terms) to your [🧩 IP Asset](doc:ip-asset), you can specify certain commercial terms such as `commercialRevShare`, which is the amount of revenue (from any source, original & derivative) that must be shared by derivative works with the original IP. See the above section for licensing questions.

If someone then creates a derivative of my IP Asset - which has a `commercialRevShare` of let's say 10% in its license terms - and earns revenue on it, Story enforces the share of this revenue through the [💊 Programmable IP License (PIL)](doc:programmable-ip-license) (otherwise resulting in an on-chain dispute using the [❌ Dispute Module](doc:dispute-module) or traditional legal arbitration) and then handles the upstream revenue share at the protocol level. If the derivative work earns 100 $WIP, my original IP Asset could claim 10 $WIP.

Our tutorials will show you exactly how to claim revenue:

<Cards columns={2}>
  <Card title="Using the SDK" href="https://docs.story.foundation/docs/claim-revenue" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    Learn how to claim revenue on Story using the TypeScript SDK.
  </Card>

  <Card title="Using the Smart Contracts" href="https://docs.story.foundation/docs/sc-claiming-royalty" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    Learn how to claim revenue on Story using the Smart Contracts.
  </Card>
</Cards>

> 📘 Learn More
>
> For more information on royalty and how it functions, check out the [💸 Royalty Module](doc:royalty-module).