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
> 📘 UMA
>
> For detailed information on how UMA's dispute resolution works, [visit their website](https://uma.xyz/).

This arbitration policy is a dispute resolution mechanism that uses UMA’s optimistic oracle to verify disputes. Below we share a high-level overview of how the UMA dispute process works.

## Smart Contract Flow Diagram

![](https://files.readme.io/e0dfb0a226bdd29ab3adede7d1df7d6662497331e1b92319ee1ad8344dc5dfa3-image.png)

1. Raise Dispute - The first step to initiate a dispute against an IP Asset is to call the `raiseDispute` function on [DisputeModule.sol](https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/modules/dispute/DisputeModule.sol). This function will in turn call `assertTruth` on UMA's `OptimisticOracleV3.sol`. To initiate a dispute the dispute initiator will need to post a bond of at least the minimum bond defined by UMA for the selected currency. Note that this bond will be lost if the dispute is deemed not verifiably correct by the oracle.
2. (Optional) Dispute Assertion / Counter Dispute - After the `raiseDispute` call there is a period of time called liveness in which a counter dispute can be submitted. The liveness period is split in two parts: (i) the first part of the liveness period in which only the IP owner can counter dispute and (ii) a second part in which any address can counter dispute - which can be done by calling `disputeAssertion` on `ArbitrationPolicyUMA.sol`. To counter a dispute the caller will need to post a bond of the same amount and currency that was used by the dispute initiator when raising a dispute. Note that this bond will be lost if the original dispute is deemed to be verifiably correct by the oracle.
3. Settle Assertion
   1. If nobody submitted a counter dispute then when the liveness period is over, any address can call `settleAssertion` on UMA's `OptimisticOracleV3.sol`.
   2. If somebody has submitted a counter dispute before the liveness period is over, then the dispute is escalated to UMA decision makers who will judge and make a decision on whether the IP is infringing or not. After the decision has been made, then any address can call `settleAssertion` on UMA's `OptimisticOracleV3.sol`.

## Dispute Evidence Submission Guidelines

When raising a dispute or making a counter dispute, both parties can submit dispute evidence. Dispute evidence refers to a text document that oracle participants will use & read from to make a judgement on the dispute.

### Burden of Proof

In all disputes with UMA arbitration policy, the burden of proof lies with the party creating the dispute. This means that the disputer must provide clear, compelling, and verifiable evidence to prove the dispute beyond reasonable doubt. Disputes that do not meet this high bar can be counter-disputed with the initial disputing party losing their bond.

### Document Characteristics

Every document should have the following characteristics:

* It should be a text document. Can have images or video if necessary.

* It should be uploaded on IPFS.

* It should not take the reviewer more than 2 hours to review the dispute evidence document - the reviewer's time is limited and the evidence could be deemed invalid if it would take too much time to review. Best efforts will be applied to solve a dispute but please keep it concise to have your dispute evidence be valid.

Depending on what the type of the Dispute Tag is, you also need to include extra evidence:

<Table align={["left","left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        Dispute Tag
      </th>

      <th style={{ textAlign: "left" }}>
        Dispute Evidence Contents
      </th>

      <th style={{ textAlign: "left" }}>
        Dispute review process (Human reviewer instructions)
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        `IMPROPER_REGISTRATION`
      </td>

      <td style={{ textAlign: "left" }}>
        1. Proof of pre-existing IP with earlier registration date (onchain or offchain) and/or instructions on where/how to check it.
      </td>

      <td style={{ textAlign: "left" }}>
        1. Check if the pre-existing  is the same or very similar to the disputed IP using input A
           * Mickey Mouse with 1 pixel difference is an infringement
           * Mickey Mouse with a new hat is an infringement unless it’s a derivative of Mickey Mouse
        2. Check the registration date of the pre-existing IP using input B
        3. Confirm that the disputed IP has a later registration date by checking on the Hub
        4. Confirm that the disputed IP is not a derivative of the pre-existing IP by checking on the Hub

        <br />
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `IMPROPER_USAGE`

        Examples (non-exhaustive):\
        Territory
        Channels of Distribution
        Expiration
        Irrevocable
        Attribution
        Derivatives
        Limitations on Creation of Derivatives
        Commercial Use
        Sublicensable
        Non-Transferable
        Restriction on Cross-Platform Use
      </td>

      <td style={{ textAlign: "left" }}>
        1. PIL terms that have been violated
        2. Description of the violations
        3. Proof of the violations
      </td>

      <td style={{ textAlign: "left" }}>
        1. Read the associated PIL term description on the PIL license official document using input A
        2. Read the violation description using input B
        3. Decide on the veracity of the proof presented by checking on associated platforms when possible using input C
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `IMPROPER_PAYMENT`
      </td>

      <td style={{ textAlign: "left" }}>
        1. Description of each payment the disputed IP received that should have been shared with its royalty vault
        2. Proof of payments
      </td>

      <td style={{ textAlign: "left" }}>
        1. Check veracity of the proof of payments by checking on the associated platforms when possible using input A and B
        2. If proof of payments are deemed to be real, confirm that the payment has indeed not been made onchain by checking on blockchain explorer

        <br />
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `CONTENT_STANDARDS_VIOLATION`

        No-Hate\
        Suitable-for-All-Ages
        No-Drugs-or-Weapons
        No-Pornography
      </td>

      <td style={{ textAlign: "left" }}>
        1. The content standard point that has been violated
        2. Description of the violation
        3. Proof of violation
      </td>

      <td style={{ textAlign: "left" }}>
        1. Read the associated content standards description on the official content standards section in the PIL using input A
        2. Read the violation description using input B
        3. Decide on the veracity of the proof presented by checking on associated platforms when possible using input C

        <br />
      </td>
    </tr>
  </tbody>
</Table>

> 📘 Note
>
> As the process is still experimental, we can expect iteration and fine-tuning on the contents/formats of how the evidence should be submitted.