---
title: 💸 Royalty Module
excerpt: Learn how revenue flows between parent and child IP on Story.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
The Royalty Module defines how revenue flows between parent and child IP Assets. There are two common scenarios when revenue flow would happen:

1. Minting a License - sometimes there is a minting fee to mint a [License Token](doc:license-token) from an IP Asset. When this is paid by someone (who wants to register a derivative or simply hold a license), the revenue should flow up the chain.
2. Tipping Directly - if someone sends revenue to an IP Asset directly, it should flow up the chain.

The below example (using [Liquid Absolute Percentage](doc:policy-liquid-absolute-percentage)) shows what happens when an IP Asset 4 (IPA4) tips IPA3 1M USDC.

1. Revenue first flows to the Royalty Module contract
2. Royalty Module sends USDC to both IPA3 and the LAP contract based on the **royalty stack** (15%)
3. LAP will distribute funds to further ancestors since they have negotiated some license agreement where they are due revenue from IPA3's earnings.

**Don't worry if you don't understand everything in the picture, this is just to show you an overview of what the Royalty Module is all about.**

![](https://files.readme.io/25e44cabafe06886fef078422c3d48c472f25a12b6ea60207ffa0b63ef2cd65b-image.png)

## Royalty Policies

Royalty policies are a component of the license agreement between two IP Assets. It defines how revenue flow actually happens.

The Royalty Module supports both whitelisted/native policies created by our team directly, and external ones created by you.

> 📘 Note on Royalty Policies
>
> An IP Asset without any parents can mint licenses with different royalty policies while a derivative IP Asset inherits the royalty policy of its parents.
>
> Additionally, there will always be one royalty policy governing every link an IP Asset has with each of its derivatives.

### Whitelisted/Native Royalty Policies

These policies require governance whitelisting and guarantee royalty token distribution to ancestors.

1. [Liquid Absolute Percentage (LAP)](doc:liquid-absolute-percentage)
2. [Liquid Relative Percentage (LRP)](doc:liquid-relative-percentage)

### External Royalty Policies

These policies can be registered in a permission-less way and stipulate their own royalty and revenue distribution rules.

* [External Royalty Policies](doc:external-royalty-policies)

<br />

#### Royalty Tokens vs Royalty Stack

When creating a derivative, the creator will want to answer the question: "How much percentage of my IP earnings will I keep and how much will go to ancestor IPs?

To answer this question two concepts are important:

1. Royalty Token - Each IP Asset has 100,000,000 Royalty Tokens associated, where each token represents 0.000001% of the capital that enters each IP Royalty Vault. The holders of these Royalty Tokens can claim the Revenue Tokens that are in the associated IP Royalty Vault.
2. Royalty Stack - is the percentage of IP revenue that has to be paid to ancestors via Whitelisted/Native royalty policies. External royalty policies do not use the royalty stack percentgae - only Whitelisted/Native royaltys policies do.

Let's imagine the scenario below:

* IP1 is a root IP Asset.
* IP2 is a derivative of IP1.
* User A has 100% of Royalty Tokens of IP1
* User B has 20% of Royalty Tokens of IP2
* User C has 80% of Royalty Tokens of IP3
* IP2 Royalty Stack is 10% - meaning that all its ancestor IPs via Native/Whitelisted policies require IP2 to pay 10% of its revenue in order to create the derivative. In this case, there is only 1 ancestor which is IP1. IP1 demands 10% of IP2's revenue in order to create a derivative.

In the image below there is an example of a one million USDC payment made to IP2. In the image we can see how much each Royalty Token holder of the entire derivative chain receives when the payment is made.

![](https://files.readme.io/a96e7d196a85f69dceb2b125ce70008115e15d0aa76b4e14b0dff2007525051b-image.png)

* RT Holder A - From the one million USDC payment gets 100k USDC. Royalty Stack percentage is paid first and RT Holder A has 100% of Royalty Tokens of IP1 so gets to keep the whole 100k USDC.
* RT Holder B - From the one million USDC payment gets 180k USDC. IP2 holders as a whole receive 900k USDC from the original one million USDC payment. Those 900k USDC are then split among the different Royalty Token holders of IP2 which are B and C. B has 20% of Royalty Tokens of IP2 so it receives 900k USDC \* 20% = 180k.
* RT Holder C - From the one million USDC payment gets 720k USDC. IP2 holders as a whole receive 900k USDC from the original one million USDC payment. Those 900k USDC are then split among the different Royalty Token holders of IP2 which are B and C. C has 80% of Royalty Tokens of IP2 so it receives 900k USDC \* 80% = 720k.

<br />

<br />

## Derivative Chain Configurations

![](https://files.readme.io/79bd27f-image.png)

The derivative chain can assume multiple configurations.

Each IP Asset is restricted to a total royalty % of 100%. It will revert when minting a license that would make the IPA reserve more than 100% of its royalty tokens for ancestors, since this would make no sense.