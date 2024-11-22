---
title: UMA Arbitration Policy
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
> 🚧 Warning: Only in v1.3
>
> The UMA Arbitration Policy is only available in v1.3 of our protocol, which is not yet documented.

> 📘 UMA
>
> For detailed information on how UMA's dispute resolution works, [visit their website](https://uma.xyz/).

This arbitration policy is a dispute resolution mechanism that follows [UMA's](https://uma.xyz/) rules. Below we share a high-level overview of how the UMA dispute process works.

## Smart Contract Flow Diagram

![](https://files.readme.io/e0dfb0a226bdd29ab3adede7d1df7d6662497331e1b92319ee1ad8344dc5dfa3-image.png)

1. Raise Dispute - The first step to initiate a dispute against an IP Asset is to call the `raiseDispute` function on [DisputeModule.sol](https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/modules/dispute/DisputeModule.sol). This function will in turn call `assertTruth` on UMA's `OptimisticOracleV3.sol`. To initiate a dispute the dispute initiator will need to post a bond of at least the minimum bond defined by UMA for the selected currency.
2. (Optional) Dispute Assertion / Counter Dispute - After the `raiseDispute` call there is a period of time called liveness in which a counter dispute can be submitted. The liveness period is split in two parts: (i) the first part of the liveness period in which only the IP owner can counter dispute and (ii) a second part in which any address can counter dispute - which can be done by calling `disputeAssertion` on `ArbitrationPolicyUMA.sol`. To counter a dispute the caller will need to post a bond of the same amount and currency that was used by the dispute initiator when raising a dispute.
3. Settle Assertion
   1. If nobody submitted a counter dispute then when the liveness period is over, any address can call `settleAssertion` on UMA's `OptimisticOracleV3.sol`.
   2. If somebody has submitted a counter dispute before the liveness period is over, then the dispute is escalated to UMA decision makers who will judge and make a decision on whether the IP is infringing or not. After the decision has been made, then any address can call `settleAssertion` on UMA's `OptimisticOracleV3.sol`.

<br />

## Dispute Evidence Submission Guidelines

When raising a dispute or making a counter dispute both parties can submit dispute evidence. Overall, the dispute evidence refers to a text document that UMA will use/read to make a judgement on the dispute.

Dispute evidence document characteristics

* General

  * It should be a text document. Can have images or video if necessary.
  * It should be uploaded on IPFS.
  * It should not take the reviewer more than 2 hours to review the dispute evidence document - the reviewer's time is limited and the evidence could be deemed invalid if it would take too much time to review. Best efforts will be applied to solve a dispute but please keep it concise to have your dispute evidence be valid.

  * Specific by Dispute Tag

    ![](https://files.readme.io/78d5ab85e87ac2178ae566cbbd1fe59430d64965b9a126c38dd70febcf7d01f0-image.png)

    <br />

Note: As the process is still experimental, we can expect iteration and fine tuning on the contents/formats of how the evidence should be submitted.