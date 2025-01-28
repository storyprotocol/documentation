---
title: 🔍 Overview
excerpt: A broad overview of Story's "Proof-of-Creativity" protocol.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
A piece of Intellectual Property is represented as an [🧩 IP Asset](doc:ip-asset) and its associated [⚙️ IP Account](doc:ip-account), a smart contract designed to serve as the core identity for each IP. We also have various [🧱 Modules](doc:story-modules)to add functionality to IP Assets, like creating derivatives of them, disputing IP, and automating revenue flow between them.

# Architecture

<Image align="center" src="https://files.readme.io/251dabc-story-v1-architecture.png" />

Let's briefly introduce the layers mentioned in the above diagram:

## [🧩 IP Asset](doc:ip-asset)

When you want to bring an IP on-chain, you mint an ERC-721 NFT. This NFT represents **ownership** over your IP.

Then, you **register** the NFT in our protocol through the [IP Asset Registry](doc:ip-asset-registry). This deploys an [⚙️ IP Account](doc:ip-account), effectively creating an "IP Asset". The address of that contract is the identifier for the IP Asset (the `ipId`).

The underlying NFT can be traded/sold like any other NFT, and the new owner will own the IP Asset and all revenue associated with it.

## [⚙️ IP Account](doc:ip-account)

IP Accounts are smart contracts that are tied to an IP Asset, and do two main things:

1. Store the associated IP Asset's data, such as the associated licenses and royalties created from the IP
2. Facilitates the utilization of this data by various modules. For example, licensing, revenue/royalty sharing, remixing, and other critical features are made possible due to the IP Account's programmability.

The address of the IP Account is the IP Asset's identifier (the `ipId`).

## [🧱 Modules](doc:modules-1)

Modules are customizable smart contracts that define and extend the functionality of IP Accounts. Modules empower developers to create functions and interactions for each IP to make IPs truly programmable.

We already have a few core modules:

1. \[📜 Licensing Module]\(doc:licensing-module): create parent\<->child relationships between IPs, enabling derivatives of IPs that are restricted by the agreements in the license terms (must give attribution, share 10% revenue, etc)
2. [💸 Royalty Module](doc:royalty-module): automate revenue flow between IPs, abiding by the negotiated revenue sharing in license terms
3. [❌ Dispute Module](doc:dispute-module): facilitates the disputing and flagging of IP
4. [👥 Grouping Module](doc:grouping-module): allows for IPs to be grouped together

## [🗂️ Registry](doc:registry)

The various registries on our protocol function as a primary directory/storage for the global states of the protocol. Unlike IP Accounts, which manage the state of specific IPs, a registry oversees the broader states of the protocol.

## [💊 Programmable IP License (PIL)](doc:programmable-ip-license)

The PIL is a real, off-chain legal contract that defines certain **License Terms** for how an IP Asset can be legally licensed. For example, how an IP Asset is commercialized, remixed, or attributed, and who is allowed to do that and under what conditions.

We have mapped these same terms on-chain so you can easily attach terms to your IP Asset for others to seamlessly and transparently license your IP.