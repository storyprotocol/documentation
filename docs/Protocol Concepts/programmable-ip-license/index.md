---
title: 💊 Programmable IP License (PIL)
excerpt: Story Programmable IP License
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
The PIL is a legal off-chain document based on US copyright law created by the Story team.

The parameters outlined in the PIL (ex. "Commercial Use", "Derivatives Allowed", etc) have been mapped on-chain, which means they can be enforced on-chain via our protocol, bridging code and law and unlocking the benefit of transparent, autonomous, and permission-less smart contracts for the world of intellectual property.

<Cards columns={1}>
  <Card title="PIL Legal Text" href="https://github.com/storyprotocol/protocol-core-v1/blob/main/PIL_Testnet.pdf" icon="fa-scroll" iconColor="#ccb092" target="_blank">
    Check out the actual PIL legal text. It is very human-readable for a legal text!
  </Card>
</Cards>

The PIL is the first and currently only example of a [License Template](doc:license-template). A License Template is simply a traditional legal document that has been brought on-chain and contains a set of pre-defined terms that people must set, like:

* `commercialUse` - can someone use my work commercially?
* `mintingFee`  - the cost of minting a license to use my work in your own works.
* `derivativesAttribution` - does someone have to credit me in their derivative works?

In code, these terms form a struct that represent their legal off-chain counterparts. To see all of the terms defined by the PIL and their associated explanations in code, see [PIL Terms](doc:pil-terms).

To see example configurations ("flavors") of the PIL, see [PIL Flavors (examples)](doc:pil-flavors).

## The Background Story

> 📘 Skip This Section
>
> If you just want to get started developing with the PIL, you can skip this section.

We designed Story's [📜 Licensing Module](doc:licensing-module) to power the expansion of emerging forms of creativity, such as authorized remixes and co-creation. Our protocol can support any media format or project, ranging from user-generated social videos & images to Hollywood-grade collaborative storytelling.

Intellectual property owners can permit other parties to use, or build on, their work by granting rights in a license, which can be for profit or for the common good. In the media world, these licenses are generally highly tailored contracts, which vary by media formats and the unique needs of licensors - often requiring unique expertise (via lawyers) and significant resources to create.

We searched for a form of a "universal license" that could support these emerging activities at scale. Hat tip to [Creative Commons](https://creativecommons.org/mission/), [Arweave](https://mirror.xyz/0x64eA438bd2784F2C52a9095Ec0F6158f847182d9/AjNBmiD4A4Sw-ouV9YtCO6RCq0uXXcGwVJMB5cdfbhE), A16Z / [Can’t Be Evil,](https://a16zcrypto.com/posts/article/introducing-nft-licenses/) The [Token-Bound NFT License](https://james.grimmelmann.net/files/articles/token-bound-nft-license.pdf) and music rights organizations, among others. But we simply couldn't find one framework or agreement robust enough - so with our expert legal counsel (with special thanks to Ghaith Mahmood and Heather Liu) we created one ourselves! **Introducing the Programmable IP License (PIL:pill:)**, the first example of a [License Template](doc:license-template) on the protocol.

## Feedback

We are excited to collect feedback and collaborate with IP owners to unlock the potential of their works - please let us know what you think! We can be reached at `legal@storyprotocol.xyz`.

<Cards columns={1}>
  <Card title="PIL Legal Text" href="https://github.com/storyprotocol/protocol-core-v1/blob/main/PIL_Testnet.pdf" icon="fa-scroll" iconColor="#ccb092" target="_blank">
    Check out the actual PIL legal text. It is very human-readable for a legal text!
  </Card>
</Cards>