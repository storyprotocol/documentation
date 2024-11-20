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
> ⏩ Skip the Read - 1 Minute Summary
> 
> The Programmable IP License, also called the PIL, is a legal off-chain document based on US copyright law. It is the first and currently only example of a [License Template](doc:license-template), created by the Story team. A License Template is simply a legal document containing a set of pre-defined terms that people must set, like:
> 
> - `Commercial Use` - can someone use my work commercially?
> - `Minting Fee`  - the cost of minting a license to use my work in your own works.
> - `Derivatives Attribution` - does someone have to credit me in their derivative works?
> 
> In code, these terms form a struct that represent their real, legal off-chain counterparts. 
> 
> To see all of the terms defined by the PIL and their associated explanations in code, go [here](doc:pil-terms). To see example configurations ("flavors") of the PIL, go [here](doc:pil-flavors).

> 📘 PIL Legal Text
> 
> Check out the actual PIL legal text [here](https://github.com/storyprotocol/protocol-core-v1/blob/main/PIL_Testnet.pdf). It is very human readable for a legal text!

## The Background Story

We designed Story Protocol's [Licensing Module](doc:licensing-module) to power the expansion of emerging forms of creativity, such as authorized remixes and co-creation. Our protocol can support any media format or project, ranging from user-generated social videos & images to Hollywood-grade collaborative storytelling.

Intellectual property owners can permit other parties to use, or build on, their work by granting rights in a license, which can be for profit or for the common good. In the media world, these licenses are generally highly tailored contracts, which vary by media formats and the unique needs of licensors - often requiring unique expertise (via lawyers) and significant resources to create.

We searched for a form of a "universal license" that could support these emerging activities at scale. Hat tip to [Creative Commons](https://creativecommons.org/mission/), [Arweave](https://mirror.xyz/0x64eA438bd2784F2C52a9095Ec0F6158f847182d9/AjNBmiD4A4Sw-ouV9YtCO6RCq0uXXcGwVJMB5cdfbhE), A16Z / [Can’t Be Evil,](https://a16zcrypto.com/posts/article/introducing-nft-licenses/) The [Token-Bound NFT License](https://james.grimmelmann.net/files/articles/token-bound-nft-license.pdf) and music rights organizations, among others. But we simply couldn't find one framework or agreement robust enough - so with our expert legal counsel (with special thanks to Ghaith Mahmood and Heather Liu) we created one ourselves! **Introducing the Story Protocol Programmable IP License (PIL :pill:), the first example of a [License Template](doc:license-template) on the protocol.**

## PIL Properties

The protocol and the license work closely together. The parameters outlined in our license are enforced on-chain via Story Protocol, bridging code and law. By doing so, we unlock the benefit of transparent, autonomous, and permission-less smart contracts for the world of intellectual property. 

Some of our unique facets include: 

- **Universal**: To use one of the existing open source licenses, the parties must agree on using one particular version. For Creative Commons, the agreement for commercial use is distinct from the non-commercial contract. Separate versions permitting derivatives, or relicensing, are needed to address different requirements. This results in many different licenses to represent each selection of multiple options. Why can’t there be a single flexible agreement to rule them all?
- **Permission-less**: A goal of Web3 is permission-less licensing. Creating derivatives without requesting permission (or forgiveness!) is the default in our agreement. For certain IPAs, particularly those from established brands or media properties, approvals might be required before derivatives are created. That’s OK - there’s a tag for that.
- **Commercial Use**: To effectively support commercial use, we need more than a simple binary selection between commercial and non-commercial, A media company or creator might want to limit the parties that can commercialize a work (perhaps certain NFT holders), have a clear and transparent definition of commercial use, or may have constraints on activity in certain media formats (perhaps from pre-existing license arrangements).
- **Progressive Complexity**: Under each parameter, the IPA can select tags appropriate to its project. Absent a selection, we have provided defaults. For example, if you agree that your license is limited to a particular country you might select "United States." To keep it simple, our default is global. This allows an experienced media company to apply the same business logic in current usage, while the hobbyist could apply a reasonable and industry-standard default.
- **Adaptable**: Not everyone may want to use our form. We can separately publish our parameters as a license library, which would allow others to easily map their existing agreements to our smart contracts.
- **Content Standards**: A healthy ratings system is crucial for mass market media. The original licensor can select from certain standards that content creators must stick to. 

In future versions, we might consider adding:

- Rules for when a remix becomes a new work
- Co-creation / co-ownership terms
- Features for music licenses
- Sub-licensing capabilities
- Fractionalization permissions and rights
- New royalty pool structures

## Feedback

We are excited to collect feedback and collaborate with IP owners to unlock the potential of their works - please let us know what you think! We can be reached at `legal@storyprotocol.xyz`.

> 📘 PIL Legal Text
> 
> Check out the actual PIL legal text [here](https://github.com/storyprotocol/protocol-core-v1/blob/main/PIL_Testnet.pdf). It is very human readable for a legal text!