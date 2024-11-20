---
title: ❌ Dispute Module
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
The Dispute Module creates a way for users to raise and resolve disputes through arbitration.

## Dispute Terminology

The main components of the arbitration system are:

* **Arbitration Policies:** the arbitration policy refers to the set rules/process/entities that combined will decide on a dispute. Currently the only supported arbitration policy is the [UMA Arbitration Policy](doc:uma-arbitration-policy).
* **Arbitration Penalty:** what happens to an IP Asset after it has been "tagged". An IPA is not deemed "tagged" unless the dispute is decided to be correct. Once tagged, an IPA will not be able to:
  * mint licenses
  * link to any parents
  * claim royalties
  * and all of its existing licenses become unusable
* **Tags:** refers to which "labels" can be applied to IP Assets in the protocol. The allowed tags are whitelisted by protocol governance. The initial set of tags is planned to be.

<Table align={["left","left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        Dispute Tag
      </th>

      <th style={{ textAlign: "left" }}>
        Explanation
      </th>

      <th style={{ textAlign: "left" }}>
        Dispute Raise Process
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        `IMPROPER_REGISTRATION`
      </td>

      <td style={{ textAlign: "left" }}>
        Refers to registration of IP that already exists.
      </td>

      <td style={{ textAlign: "left" }}>
        Inputs:\
        A. text: URL to pre-existing IP (offchain or onchain)  

        B. text: proof of pre-existing IP registration date or instructions on where to check it with appropriate links
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `IMPROPER_USAGE`  

        Examples (non-exhaustive):\
        Territory\
        Channels of Distribution\
        Expiration\
        Irrevocable\
        Attribution\
        Derivatives\
        Limitations on Creation of Derivatives\
        Commercial Use\
        Sublicensable\
        Non-Transferable\
        Restriction on Cross-Platform Use
      </td>

      <td style={{ textAlign: "left" }}>
        Refers to improper use of an IP Asset across multiple items (examples on the left). These items can be found in more detail in the [💊 Programmable IP License (PIL)](doc:programmable-ip-license)   legal document.
      </td>

      <td style={{ textAlign: "left" }}>
        Inputs:\
        A. text: PIL term that has been violated  

        B. text: description of the violation  

        C. text: proof of violation and appropriate links
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `IMPROPER_PAYMENT`
      </td>

      <td style={{ textAlign: "left" }}>
        Refers to missing payments associated with an IP.
      </td>

      <td style={{ textAlign: "left" }}>
        Inputs:\
        A. text: description of each of each payment the disputed IP received that should have been shared with its royalty vault  

        B. text: proof of payments with appropriate links
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `CONTENT_STANDARDS_VIOLATION`  

        No-Hate\
        Suitable-for-All-Ages\
        No-Drugs-or-Weapons\
        No-Pornography
      </td>

      <td style={{ textAlign: "left" }}>
        Refers to "No-Hate", "Suitable-for-All-Ages", "No-Drugs-or-Weapons" and "No-Pornography". These items can be found in more detail in the [💊 Programmable IP License (PIL)](doc:programmable-ip-license) legal document.
      </td>

      <td style={{ textAlign: "left" }}>
        Inputs:\
        A. text: the content standard point that has been violated  

        B. text: description of the violation  

        C. text: proof of violation and appropriate links
      </td>
    </tr>
  </tbody>
</Table>

## Dispute Process Flow

![](https://files.readme.io/a1dc371-image.png)

### Raise Dispute

The `raiseDispute` function is permissionless and allows any address to raise a dispute against any IP Asset registered on the protocol. The dispute initiator has to:

1. Select which "tag" it is raising a dispute on which will be applied to the IP Asset if the arbitration decision is positive. This means an IP Asset is officially "tagged" only when the proposed tag is confirmed as correct ("positive decision" in the diagram above).
2. Submit the dispute evidence for evaluation
3. Other conditions custom to each arbitration policy - such as payment rules, etc.

### Set Dispute Judgement

The `setDisputeJudgement` can only be called by whitelisted addresses and allows the caller to set the dispute judgment. Can only be called once as dispute decisions are immutable. If 3rd parties want to offer the possibility for recourse they can do so on their end and relay the final judgment.

### Tag Derivative If Parent Infringed

If the `setDisputeJudgement` has tagged an IP as infringing then any address can call `tagDerivativeIfParentInfringed` to apply the same tag as the parent to the derivatives all the way down the derivative chain.

> 📘 Looking Ahead
>
> In the future, the idea is that any derivative IP Asset of an infringed IP Asset would automatically be tagged without needed someone to call `tagDerivativeIfParentInfringed`. This is currently a limitation that we are aware of.

The derivatives are then tagged directly without any need for judgment given that it is considered that if a parent IP license has been infringed then all derivatives that come from that license are also implicitly in an infringement situation.

**Example**: IPA 7 is first tagged ("PLAGIARISM") as infringing via `setDisputeJudgement` after having gone through a dispute process. Only after that can IPAs 3, 1, and 0 can be tagged via `tagDerivativeIfParentInfringed` by any address without needing to go through a new dispute process.

![](https://files.readme.io/ee69754-image.png)

### Resolve Dispute

Resolving a dispute removes the tag from the IP Asset. Since there are two ways in which a tag can be applied, there are two ways for it to be resolved:

1. Tag was applied via the`setDisputeJudgement` function

In a case where a dispute judgment was positive, then a tag was applied. After the tag has been applied to an IP Asset, the **dispute initiator** can, if he/she believes the matter to be resolved and the tag to no longer apply, choose to remove it by calling `resolveDispute`. For example, if one party owed money to the dispute initiator and paid the full amount after the dispute judgment then the tag could be cleared and the IP Asset would have a clean slate again.

If the dispute initiator chooses to not resolve, then the tag that was defined in `setDisputeJudgement` remains in force.

2. Tag was applied via the`tagDerivativeIfParentInfringed` function

If an IP has been previously tagged as infringing via `tagDerivativeIfParentInfringed`, such tag can be removed via `resolveDispute` in a permissionless way as long as the parent is no longer considered an infringing IP Asset.

This mechanism of permissionless resolving disputes exists to make it easier to propagate down the derivative chain and remove infringement tags from derivative IPs when the parent has resolved its original dispute and is no longer considered as being in an infringing situation, and therefore neither are its derivatives.

If no address chooses to resolve, then the tag that was applied from the parent to the derivative remains in force.

### Cancel Dispute

In a case where a dispute was raised but the matter has been resolved before the dispute judgment, the dispute initiator can cancel the dispute. However, depending on the conditions of each arbitration policy, there may be non-refundable fees that are not recouped on cancellation.
