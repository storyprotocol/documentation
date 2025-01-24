---
title: Liquid Relative Percentage (LRP)
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
> ⏩ Skip the Read - 1 Minute Summary
>
> Let's come up with an example: An IP Asset ('C') is a child of 'B', and 'B' is a child of 'A', such that it goes A▶️B▶️C. 'A' specifies that any **direct** descendant must share 5% of their revenue with it. On the other hand, 'B' specifies that any **direct** descendants must share 10% of their revenue with it.
>
> Okay, great. Let's see what happens in two (independent) common scenarios:
>
> 1. **Minting a License** - 'C' mints a license from 'B' that costs 100 USDC. When 'C' pays 'B' 100 USDC to mint a license, 'A' claims 5 USDC from B. In the end, 'B' only gets 95 USDC.
> 2. **Tipping Directly** - 'C' is a comic book that is super well written. Someone tips 100 USDC to 'C' because they love it. 'B' claims 10 USDC from 'C'. 'A' claims 0.5 USDC from 'B' (5% of 10). In the end, 'C' only gets 90 USDC.

The Liquid Relative Percentage (LRP) royalty policy defines that each parent IP Asset can choose a minimum royalty percentage that only the direct derivative IP Assets in a derivative chain will share from their monetary gains as defined in the license agreement.

## Prerequisites

Before continuing, make sure you have read the [IP Royalty Vault](doc:ip-royalty-vault) terminology.ads

## Royalty Payment and Claiming Flow

In the image below, IPA 1 and IPA 2 - due to being ancestors of IPA 3 - have a % economic right over revenue made by IPA 3. Key notes to understand the derivative chain below:

* License Royalty Percentage - this percentage is selected by the user and it means the percentage that the user wants - according to LRP rules - in return for allowing other users to remix its IPA.
* Royalty Stack LRP - is the revenue an IPA has to pay all its parent. For LRP royalty stack = sum of licenses percentages used to connect to each parent
  * Royalty Stack IPA 2 = License Royalty % between IPAs 1 and 2 = 5%
  * Royalty Stack IPA 3 = License Royalty % between IPAs 2 and 3 = 10%
* Royalty tokens flow to the IPA initially when a vault is deployed. The Royalty Tokens can be transferred to any other address and after that transfer any future royalty inflow will be claimable by that new address which now holds the RTs.

![](https://files.readme.io/de296f0efbb58233b5f340b127dc66a48c80eff88a75b809107ea8b95beca5a6-image.png)

Now, let's imagine a scenario where a new IP Asset 4 intends to join the derivative chain as a derivative of IP Asset 3. An example flow sequence below:

1. IP Asset 4 pays 1M USDC in royalties to its parent IPA 3 by calling `payRoyaltyOnBehalf`. Note that the royalty process is the same whether the payment is the license minting fee or any other royalty payment - with the difference being that the license minting fee is made via `payLicenseMintingFee` and is mandatory upon derivative creation. Once a payment is made, a share equivalent to the IPA 3 royalty stack % is sent to the royalty policy contract and the remaining amount is sent to the IPA 3 vault.

![](https://files.readme.io/29ac6f6c55c2bb507ef8344fbc4351aa5574154e4d8577ec949d96d44cfffb68-image.png)

<br />

2. Each ancestor can call `transferToVault` on the royalty policy contract to receive the amount each ancestor has the right to claim from a given descendant. Funds are moved to the ancestor's IP Royalty Vault.

   1. 95k USDC are transferred to the IP Royalty Vault 2 since it has the right to 10% of all IPA 2 descendants revenue and has to pay 5% of its revenue to its direct parent IPA 1. So 100k is received from IPA 3 and 5k is paid to IPA 1, resulting in IPA 2 keeping 100k - 5k = 95k.
   2. 5k USDC are transferred to the IP Royalty Vault 1 since it has the right to 0.5% of all IPA 2 descendants revenue. IPA 1 has the right to 5% of revenue earned by IPA 2, which in turn has 10% of revenue earned by IPA 3. Given LRP royalty policy considers relative percentages, then IPA 1 has the right to 10%\*5% = 0.5% of revenue earned by IPA 3.

   ![](https://files.readme.io/4956d4151c271dd42773b83ca75e23794c6318b8850cf046156f86b04c783f71-image.png)

   <br />
3. In the final step of the claiming flow, any Royalty Token holder address can call `claimRevenueOnBehalfByTokenBatch`/`claimRevenueOnBehalf` (for non-vault claimers) or `claimRevenueByTokenBatchAsSelf` (when the claimer is an IP Royalty Vault) to claim revenue tokens. In the current example:
   1. 5k USDC are claimed to the IPA 1 which holds 100% RT1
   2. 95k USDC are claimed to the IPA 2 which holds 100% RT2
   3. 900k USDC are claimed by IPA 3 which holds 100% RT3\
      Note: Any royalty token holder address can claim - whether it is a smart contract, IPA, or EOA.

![](https://files.readme.io/b39ed27190ace760f4b8cb788fdf5ad28e93e3d3f3a5c5b23b122c9a812564bd-image.png)