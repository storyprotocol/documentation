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

<Cards columns={1}>
  <Card title="GroupingModule.sol" href="https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/modules/grouping/GroupingModule.sol" icon="fa-scroll" iconColor="#ccb092" target="_blank">
    View the smart contract for the Grouping Module.
  </Card>
</Cards>

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

### The Group's IP Account

The Group IP Account should function equivalently to a normal IP Account, allowing attachment of license terms, creation of derivatives, execution with modules, and other interactions. It also has the same common interface of IP Account. Hence, the Group IP Account can be applied to anywhere where IP Account can be applied.

Besides the common interfaces of IP Account, the Group IP Account has functions to manage the adding/removing of individual IPAs in the group.

## :lock: Group Restrictions

Here are the restrictions associated with a Group IPA:

* A derivative IP of a group IP can only have the group IP as its sole parent
* A group IP cannot attach License Terms that use the [Liquid Absolute Percentage (LAP)](doc:liquid-absolute-percentage) royalty policy
* An empty group cannot have derivative IPs or mint License Tokens
* A group IP cannot be registered as a derivative
* A group IP can only attach one license term common to all members
* Once a Group gains its first member, the `mintingFee`, `licensingHook`, and `licensingHookData` are frozen. The Group’s `commercialRevShare` can only increase
* A group has a maximum size of 1000 members

### Adding & Removing from a Group

* Only the owner of a group can add/remove IP Assets. You **do not** have to own an IP Asset to add it to your group.
* An IPA must include one license terms that matches the license terms of the group. An IPA may include other license terms in addition to the one that matches the group.
* When adding an IP to a group, the Group and IP must have the same `mintingFee` and `licenseHook` in the LicenseConfig, and the Group’s `commercialRevShare` must be greater than or equal to the IP’s share

### Groups Becoming Locked

When a group is locked, IPAs cannot be removed from it, but new IPAs can still be added.

A group IPA is locked when:

1. it has a derivative IP registered OR
2. when someone mints a license token from the group

## :blue_book: Example

Let's say you have an AI bot that uses training data to continuously learn and produce better content. The training data is a Group IPA that is the root, and the AI bot is a derivative IPA of the training data. And any time the AI bot gets paid, the revenue flows back to the training data as revenue.

Now you want to add more training data to the group. Since the group is now locked (you linked a derivative to it), you should register a new Group IPA as a root, and then a new AI bot as a derivative.