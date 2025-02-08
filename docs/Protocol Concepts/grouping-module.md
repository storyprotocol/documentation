---
title: 👥 Grouping Module
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
The Grouping Module enables the creation and management of group IP Assets, supporting a royalty pool for the group.

## `GroupingModule.sol`

> 🗒️ Contract
>
> View the smart contract [here](https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/modules/grouping/GroupingModule.sol).

`GroupingModule.sol` is the main entry point for the grouping workflows. It is **stateless** and responsible for:

* Registering a new group
* Adding an IPA to a group
* Removing an IPA from a group
* Checking whether a group contains a specific IPA
* Get the total number of IPAs of a group

## Creating a Group IPA

Similar to the IP Asset registration process, in which you must have a minted NFT to register and then an IP Account is created, the same applies to Group IP Assets. You must have a minted ERC-721 NFT (that represents the ownership of the group) to register as a group, and then when you register, an IP Account for the group is deployed.

Anyone can create a new group.

### Group IP Asset Registry

Similar to how when an IP Asset is created an IP Account is deployed & registered through the [IP Asset Registry](doc:ip-asset-registry), the Group's IP Account is deployed and registered through the [Group IP Asset Registry](doc:group-ip-asset-registry). This is responsible for managing the registration and tracking of Group IP Assets, including the group members and reward pools.

## The Group's IP Account

The Group IP Account should function equivalently to a normal IP Account, allowing attachment of license terms, creation of derivatives, execution with modules, and other interactions. It also has the same common interface of IP Account. Hence, the Group IP Account can be applied to anywhere where IP Account can be applied.

Besides the common interfaces of IP Account, the Group IP Account has functions to manage the adding/removing of individual IPAs in the group.

## Adding & Removing from a Group

Only the owner of a group can add/remove IP Assets. You **do not** have to own an IP Asset to add it to your group.

### Conditions to Add to a Group

An IPA must have the same license terms of the group it is trying to join **or** no license terms at all. It can also have other license terms attached, as long as one of them is the same.

### Groups Becoming Locked

Once the group receives any type of payment, it becomes "locked", meaning you can't add/remove any more members and you must create a new group if you wish to add/remove members.

## Group Restrictions

* A derivative IP of a group IP can only have the group IP as its sole parent
* A Group IP cannot attach License Terms that use the LAP Royalty policy
* An empty group cannot have derivative IPs or mint License Tokens
* A Group IP cannot be registered as a derivative as another parent IP
* **Single License Term Validation:** Ensure that a Group IPA can only attach one license term common to all members
* **Consistent License Config Validation:** When adding an IP to a group, the Group and IP must have the same mintingFee and licenseHook in the LicenseConfig, and the Group’s commercial revenue share must be greater than or equal to the IP’s share
* **Freezing License Config Items:** Once a Group gains its first member, the mintingFee, licensingHook, and licensingHookData are frozen. The Group’s commercial revenue share can only increase
* **Group Max Size Limit:** Enforce a maximum group size of 1000

> 📘 Example
>
> Let's say you have an AI bot that uses training data to continuously learn and produce better content. The training data is a Group IPA that is the root, and the AI bot is a derivative IPA of the training data. And any time the AI bot gets paid, the revenue flows back to the training data as revenue.
>
> Now you want to add more training data to the group. Since the group is now locked (you linked a derivative to it), you should register a new Group IPA as a root, and then a new AI bot as a derivative.