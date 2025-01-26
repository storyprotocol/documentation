---
title: ⚙️ IP Account
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
> 🐦 Skip the Read
>
> Get a quick 2-minute overview of IP Accounts [here](https://x.com/jacobmtucker/status/1787603252198134234).

When an [🧩 IP Asset](doc:ip-asset) is registered, it is given an associated **IP Account**. An IP Account is a modified ERC-6551 (Token Bound Account) implementation. It is a separate contract bound to the IP Asset for controlling permissions around interactions with Story's modules or storing the IP's associated data. Upon registration, an IP Asset is assigned a unique ID. This ID is the address of the IP Account that is bound to the IP Asset.

![](https://files.readme.io/aab60607fd795080b061d93bfdfaf9a800930db861be332d205a48d637e234f1-image.png)

An IP Account mainly does two things:

1. Stores comprehensive IP-related data, including metadata and ownership details of associated assets such as the License Tokens or Royalty Tokens that are created from the IP.
2. Facilitates the utilization of this data by various modules. These modules interact with and contribute to the IP Account, creating and storing data. For example, licensing, revenue/royalty sharing, remixing, disputing an IP, and other modules are made possible due to the IP Account's programmability.

> 📘 Transferring the Underlying NFT
>
> If the underlying NFT is transferred, the new owner is also automatically the owner of the associated IP Asset & IP Account.

## `execute` and `executeWithSig`

A key feature of IP Account is the generic `execute()` function, which allows calling arbitrary modules within Story via encoded bytes data (thus extensible for future modules). Additionally, there is a `executeWithSig()` function that enables users to sign transactions and have others execute on their behalf for seamless UX.
