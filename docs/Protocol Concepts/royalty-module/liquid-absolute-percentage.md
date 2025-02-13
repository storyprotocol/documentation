---
title: Liquid Absolute Percentage (LAP)
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
<Accordion title="Skip the Read - 1 Minute Summary" icon="fa-info-circle">
  Let's come up with an example: An IP Asset ('C') is a child of 'B', and 'B' is a child of 'A', such that it goes A▶️B▶️C. 'A' specifies that any descendant must share 5% of their revenue with it. On the other hand, 'B' specifies that any descendants must share 10% of their revenue with it.

  Okay, great. Let's see what happens in two (independent) common scenarios:

  1. **Minting a License** - 'C' mints a license from 'B' that costs 100 WIP. When 'C' pays 'B' 100 WIP to mint a license, 'A' claims 5 WIP from B. In the end, 'B' only gets 95 WIP.
  2. **Tipping Directly** - 'C' is a comic book that is super well written. Someone tips 100 WIP to 'C' because they love it. 'A' claims 5 WIP from 'C'. 'B' claims 10 WIP from 'C'. In the end, 'C' only gets 85 WIP.
</Accordion>

The Liquid Absolute Percentage (LAP) defines that each parent IP Asset can choose a minimum royalty percentage that all of its downstream IP Assets in a derivative chain will share from their monetary gains as defined in the license agreement.

<Cards columns={1}>
  <Card title="RoyaltyPolicyLAP.sol" href="https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/modules/royalty/policies/LAP/RoyaltyPolicyLAP.sol" icon="fa-scroll" iconColor="#ccb092" target="_blank">
    View the smart contract for the LAP Royalty Policy.
  </Card>
</Cards>

## Prerequisites

Before continuing, make sure you have read the [IP Royalty Vault](doc:ip-royalty-vault) terminology.

The License Royalty % in this page correspond to the same value as the `commercialRevShare` on the PIL terms.

## Royalty Payment and Claiming Flow

In the image below, IPA 1 and IPA 2 - due to being ancestors of IPA 3 - have a % economic right over revenue made by IPA 3. Key notes to understand the derivative chain below:

* License Royalty Percentage - this percentage is selected by the user and it means the percentage that the user wants - according to LAP rules - in return for allowing other users to remix its IPA.
* Royalty Stack - is the revenue an IPA has to pay all its ancestors. For LAP royalty stack = sum of parents royalty stack + sum of licenses percentages used to connect to each parent
  * Royalty Stack IPA 2 = Royalty Stack IPA 1 + License Royalty % between IPAs 1 and 2 = 0% + 5% = 5%
  * Royalty Stack IPA 3 = Royalty Stack IPA 2 + License Royalty % between IPAs 2 and 3 = 5% + 10% = 15%
* Royalty Tokens flow to the IPA initially when a vault is deployed. The Royalty Tokens can be transferred to any other address and after that transfer any future royalty inflow will be claimable by that new address which now holds the RTs.

![](https://files.readme.io/906b26bc193243391a86a33823d56568afd60eb0cc57b3ab42b77a97f1975142-image.png)

Now, let's imagine a scenario where a new IP Asset 4 intends to join the derivative chain as a derivative of IP Asset 3. An example flow sequence below:

1. IP Asset 4 pays 1M WIP in royalties to its parent IPA 3 by calling `payRoyaltyOnBehalf`. Note that the royalty process is the same whether the payment is the license minting fee or any other royalty payment - with the difference being that the license minting fee is made via `payLicenseMintingFee` and is mandatory upon derivative creation. Once a payment is made, a share equivalent to the IPA 3 royalty stack % is sent to the royalty policy contract and the remaining amount is sent to the IPA 3 vault.

![](https://files.readme.io/3cb2287fee2bcbdfaed6a7068b7fcd1769d32f73f46d5f29e2c4b404b9d79f64-image.png)

2. Each ancestor can call `transferToVault` on the royalty policy contract to receive the amount each ancestor has the right to claim from a given descendant. Funds are moved to the ancestor's IP Royalty Vault.
   1. 100k WIP are transferred to the IP Royalty Vault 2 since it the right to 10% of all IPA 2 descendants revenue
   2. 50k WIP are transferred to the IP Royalty Vault 1 since it the right to 5% of all IPA 2 descendants revenue

![](https://files.readme.io/2c12c635e25d9a815cd7d47ece1b75b72974208540b136e4d2405c994b791bd4-image.png)

3. In the final step of the claiming flow, any Royalty Token holder address can call `claimRevenueOnBehalfByTokenBatch`/`claimRevenueOnBehalf` (for non-vault claimers) or `claimRevenueByTokenBatchAsSelf` (when the claimer is an IP Royalty Vault) to claim revenue tokens. In the current example:
   1. 50k WIP are claimed to the IPA 1 which holds 100% RT1
   2. 100k WIP are claimed to the IPA 2 which holds 100% RT2
   3. 850k WIP are claimed by IPA 3 which holds 100% RT3\
      Note: Any royalty token holder address can claim - whether it is a smart contract, IPA, or EOA.

![](https://files.readme.io/dffc067a7dc834888bebb3c84a293ce120a30a8160f81dd744356c6428244df6-image.png)