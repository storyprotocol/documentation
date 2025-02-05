---
title: IP Derivation (Remixing)
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
An IP Asset registered with no parent IP Assets signals an *original work* in Story Protocol. Meanwhile, a *derivative work* must be registered as a *derivative IP Asset*, which has one or more parent IP Assets from which it derived the work.

To register a derivative work from a parent IP Asset, one must hold a License Token (an NFT) from that parent (licensor) and pass all licensing conditions expressed in the license's terms.

Below is the flow diagram of checks executed when registering a derivative IP.

<Image alt="Story Protocol IP Derivation Loop" align="center" src="https://files.readme.io/16db4c5-image.png">
  Story Protocol IP Derivation Loop
</Image>

# IP Asset allowing derivative IPs

When an IP Asset is registered, it does not by default allow others to create derivative works from it. In order to enable derivation, an IP Asset has two ways to enable the derivation of children.

1. Permissioned Licensing
2. Permissionless Licensing

In either case, the result is the issuance of License NFTs from the (to-be parent) IP Asset. The IP Asset issuing License NFTs is referred to as the *licensor*. Any IP Assets can acquire a License NFT and try to "link" to the licensor.

To link an IPA as a descendant (derivative) from another IPA, the descendant needs to specify a `Policy` that states the conditions under which 

## Permissioned Licensing

An IPAccount owner can mint License NFTs from a policy that is *not* attached to the IPAccount. These License NFTs are considered "permissioned" because only the IPAccount owner can generate these licenses from the policy. Thus, the IPAccount owner controls the minting and distribution of License NFTs for a policy.

Once users acquire those License NFTs (minted permissioned), they can call `registerDerivativeIp` or `linkIpToParents` to mark their derivative IP work as a derivative of the licensor IP Asset. Conversely, the licensor IP Asset becomes a parent of these derivative IP Assets.

## Permissionless Licensing

An IPAccount owner can attach a policy to the IPAccount via `addPolicyToIp`, which is an irreversible action, to signal permissionless licensing of that policy on the IPAccount. In other words, anyone can mint License NFTs of the attached policy on the IPAccount and use them to create and register derivative work.

For example, Alice can add a policy to her book IPAccount to allow others to license the book and create derivatives of the book. The derivative registration steps follow the same process as mentioned in Permissioned Licensing.

## Combining Both

IPA owners can set a policy to enable permissionless remixing under certain terms, then be more exclusive about other derivation options by keeping them permissioned. 

For example, the owner sets a policy that enables permissionless non-commercial remixing while minting a limited set of License NFTs with commercial rights to list on marketplaces.

# Registering Derivative IPs

Derivative IPs are IPAccounts with [policies inherited](https://docs.storyprotocol.xyz/docs/licensing-1#policy-attached-to-ipasset) from one or more parent IP Assets via `linkIpToParents`. Being a derivative indicates it has used the work of the parent IPs. Each policy inherited by a derivative is directly generated from a License NFT.

> ⚠️ Linking to a parent will BURN the License NFT
>
> Consider License NFTs like tickets to create a derivative. Linking to a parent, which signals that it's a derivative work, spends the ticket. 
>
> Note that the policy that is referenced in the burned License NFT (from the parent) is set in the derivative IP Asset. Since a License NFT is an ERC-1155, the actual data of license is maintained in the protocol and only that particular license used is burned.

# IP Derivations & IP Graph

An IP Asset may have multiple derivative trees operating with different rules. Each derivative tree of an IP Asset is a line of descendant IP Assets (child IPs and their children) that is based on a particular policy of the IP Asset.

Creating derivatives of derivatives gets a bit trickier — the rights of each derivative are limited by the policy that it inherited from its parents. Also, a derivative can have multiple parents, but only when the rules and limitations of policies from the parents are compatible.

For example, if a derivative is linked to three parents via one License NFT from each, all policies (represented by License NFTs) must have the SAME royalty policy.

> 🚧 IP trees with commercial IP Assets have some limitations on tree size.
>
> [See this page for more information](https://docs.storyprotocol.xyz/docs/licensing-and-royalties)

## Single-Parent Derivation

<Image align="center" width="200px" src="https://files.readme.io/ce400c0-image.png" />

Above is a simple example of single-parent derivation, ie. IPAsset is a child of one parent.

## Multi-Parent Derivation

In many scenarios, an IPAsset might want to derive from multiple IPAssets, such as a music IP and a cartoon IP. Story Protocol enables multi-parent derivation under the conditions that each License NFT's policy

* Allows for derivatives

* Either has the same licensing terms, or it is compatible enough to set terms from both licenses.

The terms that are not checked for compatibility have to be verified individually for each license. For instance, royalties must be set and IPA#2 requires the LNFT token holder to also hold a particular NFT

### License NFTs from Same Policy

Below is an example where an IPAsset (IPA#3) derives from two parents using License NFTs of the *same* policy (attached to the parent IPAssets).

<Image align="center" width="2px" src="https://files.readme.io/d3bcd36-image.png" />

<Image align="center" width="400px" src="https://files.readme.io/0c724f5-image.png" />

### License NFTs from Different Policy

In this case, it's easier for IPA#3 to be remixed in turn, if Policy 1 allows for it.

<Image align="center" width="400px" src="https://files.readme.io/323d6f6-image.png" />

## Incompatible Licenses

When deriving from multiple parents through License NFTs of different policies, the licensing terms of License NFTs can be *incompatible*, such as commercial vs. non-commercial licenses.

Below, compatibility fails when linking parents at the same time.

<Image align="center" width="400px" src="https://files.readme.io/871078d-image.png" />

Below, compatibility fails when linking one parent after the other

<Image align="center" width="400px" src="https://files.readme.io/936ff77-image.png" />

This highlights the importance of social coordination around common Policy Flavors for maximal composability.

> 📘 [See the available PIL Flavors](doc:licensing-presets-flavors-old)
