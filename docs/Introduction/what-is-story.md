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

Story is a purpose-built layer 1 blockchain designed specifically for intellectual property.

Story tokenizes any type of IP; whether that’s an idea, an image, a real world asset, a song, an AI model, an NFT or anything in-between. By embedding usage terms, attribution, and royalty agreements directly into the blockchain, Story provides a transparent, decentralized solution for IP management. This enables IP holders to protect their work, collaborate on IP seamlessly, and unlock revenue opportunities in an AI-powered economy.

> ⏩ Skip the Read - 1 Minute Summary
>
> Want to skip to a summary? Check out [Explain Like I'm Five](doc:explain-like-im-five) for a super fast & easy to understand explanation of everything Story.

## The "Why"

When IP owners share their work online, it’s easy for others to use or change it without crediting them, and they often don't get paid fairly if their work becomes popular or valuable. This can be discouraging for people who want to share their ideas and creations but don’t want to lose control over them.

Additionally, the sheer speed and superabundance of AI-generated media is outpacing the current IP system that was designed for physical replication. In many cases, [AI is trained on and is producing copyrighted data](https://twitter.com/BriannaWu/status/1823833723764084846).

Story fixes this by providing a way for creators to share their work with built-in protection. When someone (including an AI model) uses a song, image, or any creative work that’s registered on Story, the system automatically tracks who the original owner is and makes sure they get credited. Plus, if that work generates revenue—say someone remixes a song and it earns money—the original owner automatically receives their fair share based on license terms that were set on-chain.

## The "How"

There are several elements that make up Story as a whole. Below we will cover the most important components: Story Network (the L1), the Proof-of-Creativity Protocol (the smart contracts), and the Programmable IP License.

![](https://files.readme.io/56f3c96-image.png)

### Story Network: "The World's IP Blockchain"

Story Network is a purpose-built layer 1 blockchain achieving the best of EVM and Cosmos SDK. It is 100% EVM-compatible alongside deep execution layer optimizations to support graph data structures, purpose-built for handling complex data structures like IP quickly and cost-efficiently. It does this by:

* using precompiled primitives to traverse complex data structures like IP graphs within seconds at marginal costs
* a consensus layer based on the mature CometBFT stack to ensure fast finality and cheap transactions

### Proof-of-Creativity Protocol

Our "Proof-of-Creativity" Protocol, made up of smart contracts, is natively deployed on Story Network and allows anyone to onramp IP to Story. Most of our documentation focuses on the protocol.

Creators can register their IP as [🧩 IP Assets](doc:ip-asset) on our protocol. IP Assets (IPA) are the foundational programmable IP metadata on the protocol. Each IPA is composed of:

1. an on-chain NFT. This could be an existing NFT like [Azuki](https://www.azuki.com/en) that itself is the IP, or a new NFT specifically minted to represent some off-chain IP like a real-world asset.
2. its associated IP Account, which is a modified ERC-6551 (Token Bound Account) implementation.

To interact with IP Assets within the protocol, we use “**modules**” such as the licensing, royalty, and dispute modules. For example, the owner of an IP Asset can set **terms** on their IP such as whether or not derivative works can use the IP commercially (and at what cost). Creators of derivative works can then seamlessly extend their work by minting “**License Tokens**” - outlined by the terms and also represented as NFTs themselves - using the [📜 Licensing Module](doc:licensing-module), create revenue streams from derivative works using the [💸 Royalty Module](doc:royalty-module), or raise a dispute using the [❌ Dispute Module](doc:dispute-module).

### Programmable IP License

Although on-chain, an IP's usage terms and minted licenses are enforced by an off-chain legal contract called the [Programmable IP License (PIL💊)](doc:programmable-ip-license-pil), which allows anyone to offramp tokenized IP into the off-chain legal system and outlines real legal terms for how creators can remix, monetize, and create derivatives of their IP. *The protocol, or more specifically the IP Assets and modules described above, are what automate and enforce those terms on-chain*, creating a mapping between the legal world (PIL) and the blockchain.

Like USDC enables redemption for fiat, the PIL enables redemption for IP.

## An Example

**Example #1**: Imagine you're an artist who creates digital paintings, or a musician who makes original songs. You want to share your work online, but you want to ensure that if others use or change your work, they give you credit and—if they make money from it—you get a share. That’s where Story comes in. It's a platform that uses technology to give IP owners like you control over how your work is used, tracked, and shared, so it’s both protected and fairly rewarded.

Think of it like this: Suppose you upload a song to Story. Now, anyone can see that you’re the original creator, and if someone wants to remix it, they can do so through Story. The system then automatically tracks the remix as a "derivative" of your song and notes you as the original artist. This way, if the remix becomes popular and earns money, Story   can help you earn a portion of those earnings, just like the remixer.

**Example #2**: Let’s say a scientist uploads an image dataset to be used by artificial intelligence (AI) models for research. Through Story, that dataset is registered, so if any company uses it to train their AI, the original scientist is credited. If that dataset then contributes to a profitable AI application, Story ensures a fair share goes to the original contributor.

With Story, you can share your work freely, knowing that wherever it goes, it’s tracked and fairly credited back to you. The idea is to create a fair environment for sharing, building upon, and growing creative work.