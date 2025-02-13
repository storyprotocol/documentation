---
title: ❓ FAQ
excerpt: Get answers to the most common questions about Story as a whole.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
## *"Is on-chain IP real?"*

Story isn't replacing the legal system, it's providing on-chain rails to make the legal system more efficient for creative IP.

We have worked with world class legal teams to craft a real, off-chain legal contract called the [Programmable IP License (PIL💊)](doc:programmable-ip-license) that has simple terms allowing creators to state who can remix, monetize, and create derivatives of their IP and at what cost.

We've then built business logic on-chain (in the form of smart contracts) to automate & enforce those terms. This creates a tight mapping between the legal world and our on-chain terms.

## *"My asset is off-chain. How do I register it as IP on Story Protocol?"*

You would mint an NFT that represents your off-chain asset, and register that NFT on Story.

## *"I am selling an off-chain asset (eg. merch). How do I make sure that automatic royalty flow is enforced?"*

Automatic royalty flow can be enforced off-chain. Although the royalty infrastructure is on-chain, the associated license is valid beyond the chain. To legally abide by the license, any derivative work will need to pay royalties on-chain to the IP Account that is attached to your IP Asset, or face consequences like in the real world (e.g. being taken to court for abusing a license) according to the terms set out by the license.

Furthermore, one of the terms of the [Programmable IP License (PIL💊)](doc:programmable-ip-license-pil) is that licensees are obligated to provide revenue data for off-chain transactions (e.g. merch) to licensors, if there's a revenue share involved.

## *"What is a simple example?"*

Before Story, if you wanted to create a comic with someone else’s Azuki NFT and your Pudgy NFT, you would first need to hope the Azuki has a license and then find & contact the owner. You would also need to research your license terms for your Pudgy. Since you’re not a legal expert, you would probably need a lawyer to create a new contract between the two IPs. This creates a burden on anything ever getting created legally, because few people have the time, expertise, or money for that except large studios. Thus IP is not easily composable.

With Story, Azuki & Pudgy holders can register their IP as **IP Assets** and then set terms - using the PIL - outlining how people can license and remix their IP. If your Pudgy is registered on Story, anyone can see those terms on-chain and license your Pudgy automatically using the licensing module, which generates a real license agreement. You can then register the resulting comic as a derivative, and any revenue that flows in can be distributed between you and the IP holders automatically.

## *"What are the challenges?"*

The natural question arises: "*What if I'm a bad actor and I ignore all of this and just Right Click Save As?*"

First, its underestimated the extent to which people want to follow the law. This is why PIL is so important - all of the\
IP is not just on-chain, it is tied to a real legal contract! If people rip off your IP, they can be sued in court. However this "happy path" doesn't always happen. When things go wrong, we want to provide as many layers of escalation before resorting to off-chain arbitration.

Thus, we've created the [Dispute Module](doc:dispute-module) that allows anyone to flag violating content on-chain. If the dispute is\
successful, that IP will be flagged and no longer able to monetize or generate licenses.

The worst case scenario on Story is the best case scenario anywhere else: legal arbitration. Creators using us can always use the traditional legal system as a backstop. This is a situation we want to avoid, but one that's necessary for creators to have trust in the system.

## *“Why not be a protocol on every existing chain?”*

We started as a protocol, supporting projects on Polygon, Ethereum, and more. But this siloed IP to the chain that it lived on, and did not connect IP between chains. This does not realize the vision of Story as a global IP repo.

We want to be the IP settlement layer, or in other words, bring composability across chains. We need a single hub blockchain for all registered IP. This IP can be on Story, on other chains, or off-chain, but we need a hub such that all the licenses, royalties, and IP metadata live in a single unified execution environment.

## *"Okay, so you need a singular blockchain. Why not use an existing blockchain?"*

First and most obvious, building our own Layer 1 allows us to innovate across the entire technical stack. This enables us to implement essential features like on-chain dispute resolution or on-chain IP graphs without waiting on existing L1 roadmaps.

The need to innovate the entire stack ourselves was proven when we tried to build on a few existing chains and quickly realized it would not work due to technical limitations of those chains. Here are a few reasons we decided - or rather were forced - to build an L1:

1. Existing L1s are simply not able to handle the registration of IP upon 1000s of parents - each with their own complicated licensing details - in one transaction efficiently. As you can see below, gas usage on Geth EVM rapidly increases with ancestor IPs, making it impractical beyond 670 ancestors due to block gas limits. So we improved the efficiency of graph traversing and storing for our IP graph by leveraging a new stateful precompile approach, written directly in the execution client. This creates a "shortcut" around EVM to write and read certain values in the blockchain we use for our graphs, under the restrictions of PoC protocol.
2. Similarly, existing L1s are not able to handle the royalty token flow between 1000s of IP Assets. Again, this was solved by making improvements directly in the execution client.
3. Max composability by having license data on-chain, allows for 100s of IPs to remix at once.
4. Potential rollup support with bespoke X-CHAIN data (e.g. [Initia's](https://initia.xyz/) model) for Web2 apps with millions of transactions requiring rollups. Also support Web2 apps running their own validators for max incentive alignment.
5. Future roadmap for tokenization of IPs on Story Network (more info to come).
6. Future validator-enshrined L1 tech like native oracles for NFTs and off-chain RWA IPs (Cosmos SDK vote extensions).

Lastly, by building an L1, we provide a flexible foundation that supports diverse IP-specific L2 solutions. Such that, for example, each IP can potentially launch its own L2, driving our grand vision for the future of global IP.

## *"Why not just settle for an L2?"*

We actually tried to build an L2 before deciding to build an L1. However, we not only wanted to innovate the execution layer (also possible on L2), but add in a consensus mechanism, make improvements to validator node storage, and novel validator features.

For example, we eventually want to allow Netflix, TikTok, etc to run validators to stream IP data directly on-chain, and also allow those nodes to have graph DBs optimized for IP graphs. Imagine any transaction involving a disputed IP Asset being immediately rejected.

## *"Why is Story making improvements at the validator level?"*

The alternative is running an adjacent system outside the L1 to provide these features, which will strictly be less decentralized than the L1 itself. By making these features native to L1 (validators), we can have sufficient decentralization, guaranteed execution, and less system to maintain.

Namely, if the adjacent system goes down (so disputes and oracles stop) but the L1 works fine, it'd be a huge problem. This can be remedied with re-staking like AVS but the tech is not battle-tested and there's no precedence of success using re-staking (EIGEN token is still in work).

One big incentive alignment Story can have: IP companies running validators & providing custom off-chain IP data to the network natively via validator-enshrined features.

Or a law firm automating disputes and broadcasting them to other validators. After an agreement of disputes, validators can immediately block transactions that include disputed IPs, which is not possible with an adjacent system providing (unless we have preconf, which is a debated topic in the Ethereum land).

## *"You mention needing an L1 to improve upon the efficiency of an IP graph. Why not build an L2 with off-chain graph indexing?"*

The IP graph must be on-chain because certain on-chain features require the ability to traverse and aggregate the IP graph. For example, royalty and revenue distribution need to occur on-chain through the IP graph. Using off-chain graph indexing would make these on-chain features either unfeasible or overly complex, as it would necessitate involving an oracle.

Similarly, the dispute module can check if an IP has been flagged due to its ancestor being flagged by traversing the IP graph directly on-chain, and then take immediate action such as aborting a transaction that would license a disputed IP without the need of an oracle.

## *"How will Story prevent users from registering off-chain IP that isn't theirs?"*

We will support a few ways, including the [Dispute Module](doc:dispute-module), to deter IP infringement. For example if someone were to register someone else's IP, it could be disputed on-chain. And in the worst case, it would be brought to court just as it works in the traditional legal system today.

A more nuanced answer to this (one that we're constantly exploring/improving upon) is there may be additional ways to deter IP infringement. For example, a staking validation mechanism where users could stake tokens on a piece of IP being valid, and if it were to be disputed and marked as copyright, the tokens get slashed and distributed to the creator who was harmed. Additionally we've thought of introducing external IP infringement detection services directly into our L1 at the lowest level that could flag or automatically mark IP as potential infringement the moment its registered.

Ultimately Story is not a system built to prevent bad actors, rather it is meant to help facilitate honest actors to more easily register their IP, remix from others, and set proper terms for their work. The protocol is permissionless and stopping bad actors entirely would be near impossible, but we can try to disincentivize them as best we can. Much like how the pirating of media plummeted when Apple Music, Spotify, and Netflix made such media more accessible by creating a "path of least resistance", we see a similar future with Story & IP.