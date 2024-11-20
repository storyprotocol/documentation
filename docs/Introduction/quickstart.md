---
title: Quickstart
excerpt: Start building on Story quickly.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
You want to start building on Story quickly... so let's get started!

- [I'm building an app](https://docs.story.foundation/docs/quickstart#app-developers)
- [I'm a smart contract developer](https://docs.story.foundation/docs/quickstart#smart-contract-developers) 

> 📘 Looking to read up on Story first?
> 
> If you'd like to read up on Story before diving into the technical details, check out our awesome [Learn Hub](https://learn.story.foundation/) which will explain the who, what, and why of Story.

# App Developers

If you want to deploy an app using Story, this section is for you.

> ⏩ Skip everything. Go to the code.
> 
> If you want to skip everything and go right to coding, we have a clone-able quickstart for you to check out. You can clone it directly and follow the associated README.
> 
> :hammer_and_wrench: **TypeScript SDK** Node.js Example :arrow_forward: [Here](https://github.com/storyprotocol/typescript-tutorial/tree/main)

## :building_construction: Story Network Infra

The [Story Network Guide](doc:story-network) provides all RPC, explorer, and faucet info. 

## :computer: Use our SDKs

We have built a [🛠️ TypeScript SDK](doc:typescript-sdk) with its own in-depth tutorials for popular functions and use cases.

## :globe_with_meridians: Use our API

Check out the entire [API Reference](https://docs.story.foundation/reference/api-introduction) for learning how to use our API.

## :memo: Register IP on Story

Let's start with the most basic question: _"What does it take to register IP on Story in my app? How do I do this?"_

To register IP on Story, you'll first need an NFT. If your IP is an ERC-721 NFT (ex. an Azuki or Pudgy Penguin on Story), you're already set. If not, you must mint an NFT to represent your off-chain IP. And don't worry, we'll help you do this in the following tutorials.

Next you'd register that NFT on Story, ultimately creating an [🧩 IP Asset](doc:ip-asset). An "IP Asset" is your IP registered on Story, empowered by:

- all of Story's [🧱 Modules](doc:story-modules) like transparent licensing, automatic royalty payments, and disputing of wrongfully registered IP
- IP protection through the [💊 Programmable IP License (PIL)](doc:programmable-ip-license)

To actually do this in code, our SDK tutorial will show you exactly how to do these things:

- [Register an IP Asset](doc:register-an-ip-asset)

### Difference Between IP Metadata vs. NFT Metadata

A common question we get from developers while registering their IP on Story is: _"What metadata should be/is expected to be attached to the NFT, and then separately, the IP Asset?"_

To answer that question, please see [NFT vs. IP Metadata](https://docs.story.foundation/docs/ip-asset#nft-vs-ip-metadata).

## :scroll: Licensing Your IP

You may be wondering, _"How do I take advantage of Story's on-chain licensing? How do I make sure my registered IP has a license ready to go?"_

Before you attach any sort of licenses or license terms to your [🧩 IP Asset](doc:ip-asset), it would be best to first understand what the [💊 Programmable IP License (PIL)](doc:programmable-ip-license) actually is. This "PIL" is what defines the available [License Terms](doc:license-terms) on Story, which in turn - when attached to an IP Asset - is what defines how others can use (commercially, create derivatives, etc) that IP Asset.

Our SDK tutorial will show you exactly how to attach license terms to your IP Asset:

- [Attach Terms to an IPA](doc:attach-terms-to-an-ip-asset)

> 📘 Learn More
> 
> For more information on licensing and the terminology behind it, check out the [📜 Licensing Module](doc:licensing-module).

## :money_with_wings: Royalties / Revenue Sharing

Now you may be wondering, _"How do I set up automatic royalty sharing between my IP Asset and someone else's? How do I then collect that payment?"_

When you attach [License Terms](doc:license-terms) to your [🧩 IP Asset](doc:ip-asset), you can specify certain commercial terms such as `commercialRevShare`, which is the amount of revenue (from any source, original & derivative) that must be shared by derivative works with the original IP. See the above section for licensing questions.

If someone then creates a derivative of my IP Asset - which has a `commercialRevShare` of let's say 10% in its license terms - and earns revenue on it, Story enforces the share of this revenue through the [💊 Programmable IP License (PIL)](doc:programmable-ip-license) (otherwise resulting in an on-chain dispute using the [❌ Dispute Module](doc:dispute-module) or traditional legal arbitration) and then handles the upstream revenue share at the protocol level. If the derivative work earns 100 $USDC, my original IP Asset could claim 10 $USDC.

Our SDK tutorial will show you exactly how to claim revenue:

- [Claim Revenue](doc:claim-revenue)

> 📘 Learn More
> 
> For more information on royalty and how it functions, check out the [💸 Royalty Module](doc:royalty-module).

# Smart Contract Developers

If you want to deploy/build smart contracts on Story, this section is for you.

> ⏩ Skip everything. Go to the code.
> 
> If you want to skip everything and go right to coding, we have a [boilerplate](https://github.com/storyprotocol/story-protocol-boilerplate) for you to check out. You can clone it directly, study the example smart contracts, and follow the associated README for running the tests.

## :building_construction: Story Network Infra

The [Story Network Guide](doc:story-network) provides all RPC, explorer, and faucet info.

## :gear: Deployed Smart Contracts

Check out the addresses for the deployed smart contracts [here](doc:deployed-smart-contracts). Note that there are two different kinds of contracts:

- [Story Protocol Core](https://github.com/storyprotocol/protocol-core-v1) - This repository contains the core protocol logic, consisting of a thin IP registry (the [IP Asset Registry](doc:ip-asset-registry)), a set of [🧱 Modules](doc:story-modules) defining logic around [📜 Licensing](doc:licensing-module), [💸 Royalty](doc:royalty-module), [❌ Dispute](doc:dispute-module), metadata, and a module manager for administering module and user access control.
- [Story Protocol Periphery](https://github.com/storyprotocol/protocol-periphery-v1)- Whereas the core contracts deal with the underlying protocol logic, the periphery contracts deal with protocol extensions that greatly increase UX and simplify IPA management. This is mostly handled through the [📦 SPG](doc:spg).

## :memo: Register IP on Story

Let's start with the most basic question: _"What does it take to register IP on Story in my smart contract? How do I do this?"_

To register IP on Story, you'll first need an NFT. If your IP is an ERC-721 NFT (ex. an Azuki or Pudgy Penguin on Story), you're already set. If not, you must mint an NFT to represent your off-chain IP. And don't worry, we'll help you do this in the following tutorial.

Next you'd register that NFT on Story, ultimately creating an [🧩 IP Asset](doc:ip-asset). An "IP Asset" is your IP registered on Story, empowered by:

- all of Story's [🧱 Modules](doc:story-modules) like transparent licensing, automatic royalty payments, and disputing of wrongfully registered IP
- IP protection through the [💊 Programmable IP License (PIL)](doc:programmable-ip-license)

To actually do this in your smart contract, follow our smart contract tutorial:

- [Register an NFT as an IP Asset](doc:sc-tutorial-registering-an-ip-asset)

### Difference Between IP Metadata vs. NFT Metadata

A common question we get from developers while registering their IP on Story is: _"What metadata should be/is expected to be attached to the NFT, and then separately, the IP Asset?"_

To answer that question, please see [NFT vs. IP Metadata](https://docs.story.foundation/docs/ip-asset#nft-vs-ip-metadata).

## :scroll: Licensing Your IP

You may be wondering, _"How do I take advantage of Story's on-chain licensing? How do I make sure my registered IP has a license ready to go?"_

Before you attach any sort of licenses or license terms to your [🧩 IP Asset](doc:ip-asset), it would be best to first understand what the [💊 Programmable IP License (PIL)](doc:programmable-ip-license) actually is. This "PIL" is what defines the available [License Terms](doc:license-terms) on Story, which in turn - when attached to an IP Asset - is what defines how others can use (commercially, create derivatives, etc) that IP Asset.

Our smart contract tutorial will show you exactly how to attach license terms to your IP Asset:

- [Adding License Terms to an IP Asset](doc:sc-tutorial-add-license-terms)

> 📘 Learn More
> 
> For more information on licensing and the terminology behind it, check out the [📜 Licensing Module](doc:licensing-module).

## :money_with_wings: Royalties / Revenue Sharing

Now you may be wondering, _"How do I set up automatic royalty sharing between my IP Asset and someone else's? How do I then collect that payment?"_

When you attach [License Terms](doc:license-terms) to your [🧩 IP Asset](doc:ip-asset), you can specify certain commercial terms such as `commercialRevShare`, which is the amount of revenue (from any source, original & derivative) that must be shared by derivative works with the original IP. See the above section for licensing questions.

If someone then creates a derivative of my IP Asset - which has a `commercialRevShare` of let's say 10% in its license terms - and earns revenue on it, Story enforces the share of this revenue through the [💊 Programmable IP License (PIL)](doc:programmable-ip-license) (otherwise resulting in an on-chain dispute using the [❌ Dispute Module](doc:dispute-module) or traditional legal arbitration) and then handles the upstream revenue share at the protocol level. If the derivative work earns 100 $USDC, my original IP Asset could claim 10 $USDC.

Our smart contract tutorial will show you exactly how to claim royalty from a child IP Asset:

- [Claiming Royalty](doc:sc-claiming-royalty)

> 📘 Learn More
> 
> For more information on royalty and how it functions, check out the [💸 Royalty Module](doc:royalty-module).