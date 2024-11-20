---
title: 🔍 Overview
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
Story's "Proof-of-Creativity" protocol introduces a revolutionary open** Programmable IP layer**, elevating IP to a first-class entity in the blockchain ecosystem. At the heart of this system is the [🧩 IP Asset](doc:ip-asset) and its associated [⚙️ IP Account](doc:ip-account), a smart contract designed to serve as the core identity for each IP. This account-centric approach enables the storage and management of IP-related data, as well as the execution of diverse functions to manipulate that data via [🧱 Modules](doc:story-modules).

# Architecture

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/251dabc-story-v1-architecture.png",
        "",
        ""
      ],
      "align": "center"
    }
  ]
}
[/block]


Let's briefly introduce the layers mentioned in the above diagram:

## [🧩 IP Asset](doc:ip-asset)

An IP Asset is an on-chain NFT, which represents an IP. If your IP is already on-chain (like an Azuki or Pudgy Penguin), it is already ready to be registered on Story. If your IP is off-chain, you would simply mint an NFT to represent it and then register it as an IP Asset on Story.

Upon registering an NFT as an IP Asset, an associated IP Account is created.

## [⚙️ IP Account](doc:ip-account)

IP Accounts are smart contracts that are tied to an IP Asset, and do two main things:

1. Store its associated IP Asset's data (such as the associated License Tokens and Royalty Tokens) created from an IP
2. Facilitates the utilization of this data by various modules. For example, licensing, revenue/royalty sharing, remixing, and other critical features are made possible due to the IP Account's programmability.

## [🧱 Modules](doc:modules-1)

Modules are customizable smart contracts that define and extend the functionality of IP Accounts. Modules empower developers to create functions and interactions for each IP to make IPs truly programmable.

We already have a few core modules:

1. [📜 Licensing Module](doc:licensing-module)
2. [💸 Royalty Module](doc:royalty-module)
3. [❌ Dispute Module](doc:dispute-module)
4. [👥 Grouping Module](doc:grouping-module)

## [🗂️ Registry](doc:registry)

The various registries on Story function as a primary directory/storage for the global states of the protocol. Unlike IP Accounts, which manage the state of specific IPs, a registry oversees the broader states of the protocol.

## [💊 Programmable IP License (PIL)](doc:programmable-ip-license)

The PIL is a real, off-chain legal contract that defines certain **License Terms** for how an IP Asset can be legally licensed. For example, how an IP Asset is commercialized or remixed, and who is allowed to do that and under what conditions.

We have mapped these same terms on-chain so you can easily attach terms to your IP Asset for others to seamlessly and transparently license your IP.