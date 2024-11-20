---
title: What is Story
excerpt: Introduction to Story
deprecated: false
hidden: false
metadata:
  title: Story Documentation
  description: >-
    The official Story documentation. Covering Story Network ("the World's IP
    Blockchain"), our "Proof-of-Creativity" protocol, the Programmable IP
    License, and more.
  image: https://files.readme.io/2d35525-header_story.png
  robots: index
next:
  description: ''
---
<Image align="center" src="https://files.readme.io/3e11869-header_story.png" />

# Introducing the World's IP Blockchain

Story is making the legal system for creative Intellectual Property (IP) more efficient by turning IP "programmable" on the blockchain. To do this, we have created Story Network: a purpose-built layer 1 blockchain where people or programs alike can license, remix, and monetize IP according to transparent terms set by creators themselves.

> ⏩ Skip the Read - 1 Minute Summary
>
> Want to skip to a summary? Check out [Explain Like I'm Five](doc:explain-like-im-five) for a super fast & easy to understand explanation of everything Story.

## The "Why"

First, the problem: Creators want to make sure their IP (a legal concept) is protected, easy to license, and monetize. Right now this is done via the traditional legal system, where handling infringement on IP requires lawyers, is extremely expensive, and is time consuming. 

Additionally, allowing others to use your IP and grow it requires cutting one-to-one licensing deals which requires more lawyers, more money, and is unscalable. As a result, most licensing simply doesn't happen.

And there's another major issue: the sheer speed and superabundance of AI-generated media is outpacing the current IP system that was designed for physical replication. In many cases, [LLMs are trained on and are producing copyrighted data](https://twitter.com/BriannaWu/status/1823833723764084846). We need to automate and make more efficient the licensing of IP.

## The "How"

There are several elements that make up Story as a whole. Below we will cover the most important components: Story Network (the L1), the Proof-of-Creativity Protocol (the smart contracts), and the Programmable IP License.

![](https://files.readme.io/56f3c96-image.png)

### Story Network: "The World's IP Blockchain"

Story Network is a purpose-built layer 1 blockchain achieving the best of EVM and Cosmos SDK. It is 100% EVM-compatible alongside deep execution layer optimizations to support graph data structures, purpose-built for handling complex data structures like IP quickly and cost-efficiently. It does this by:

* using precompiled primitives to traverse complex data structures like IP graphs within seconds at marginal costs
* a consensus layer based on the mature CometBFT stack to ensure fast finality and cheap transactions

### Proof-of-Creativity Protocol

Our "Proof-of-Creativity" Protocol, made up of smart contracts, is natively deployed on Story Network and allows anyone to onramp IP to Story. Most of our documentation focuses on the protocol.

Creators can register their IP as “**IP Assets**” on our protocol. IP Assets (IPA) are the foundational programmable IP metadata on the protocol. Each IPA is composed of:

1. an on-chain NFT. This could be an existing NFT like [Azuki](https://www.azuki.com/en) that itself is the IP, or a new NFT specifically minted to represent some off-chain IP like a real-world asset.
2. its associated IP Account, which is a modified ERC-6551 (Token Bound Account) implementation.

To interact with IP Assets within the protocol, we use “**modules**” such as the licensing, royalty, and dispute modules. For example, the owner of an IP Asset can set **terms** on their IP such as whether or not derivative works can use the IP commercially (and at what cost). Creators of derivative works can then seamlessly extend their work by minting “**License Tokens**” - outlined by the terms and also represented as NFTs themselves - using the [Licensing Module](doc:licensing-module), create revenue streams from derivative works using the [Royalty Module](doc:royalty-module), or raise a dispute using the [Dispute Module](doc:dispute-module).

### Programmable IP License

Although on-chain, an IPA’s terms and minted License Tokens are enforced by an off-chain legal contract called the [Programmable IP License (PIL💊)](doc:programmable-ip-license-pil), which allows anyone to offramp tokenized IP into the off-chain legal system and outlines real legal terms for how creators can remix, monetize, and create derivatives of their IP. *The protocol, or more specifically the IP Assets and modules described above, are what automate and enforce those terms on-chain*, creating a mapping between the legal world (PIL) and the blockchain.

Like USDC enables redemption for fiat, the PIL enables redemption for IP.

## An Example

Before Story, if you wanted to create a comic with someone else’s Azuki NFT and your Pudgy NFT, you would first need to hope the Azuki has a license and then find & contact the owner. You would also need to research your license terms for your Pudgy. Since you’re not a legal expert, you would probably need a lawyer to create a new contract between the two IPs. This creates a burden on anything ever getting created legally, because few people have the time, expertise, or money for that except large studios. Thus IP is not easily composable.

With Story, Azuki & Pudgy holders can register their IP as **IP Assets** and then set terms - using the PIL - outlining how people can license and remix their IP. If your Pudgy is registered on Story, anyone can see those terms on-chain and license your Pudgy automatically using the licensing module, which generates a real license agreement. You can then register the resulting comic as a derivative, and any revenue that flows in can be distributed between you and the IP holders automatically.
