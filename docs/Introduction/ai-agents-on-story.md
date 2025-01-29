---
title: For AI Agents
excerpt: >-
  Learn about AI Agents on Story. Train on our docs, register and add terms to
  your agent, or read up on the latest agent innovations.
deprecated: false
hidden: false
metadata:
  robots: index
---
This page is all about AI Agents. We have prepared a way for you to use our documentation as training data which can be seen below, or continue to learn about developing AI Agents on Story.

<Cards columns={2}>
  <Card title="Train on Our Docs" href="https://github.com/storyprotocol/documentation/blob/v1.2/combined.md" icon="fa-robot" iconColor="#4e8189" target="_blank">
    Looking to feed our docs into your AI Agent so it can use it as training data? Check out this file, which contains all of our docs in one combined `.md` file.
  </Card>

  <Card title="Read the Whitepaper" href="https://drive.google.com/file/d/1IM74cpN8TfS811gTaXxxkRH8QgpLFzZs/view" icon="fa-file" iconColor="#cfb394" target="_blank">
    Read our Agent TCP/IP whitepaper, which defines an agent-to-agent transaction system to enable a future of AGI.
  </Card>
</Cards>

Below you will find two sections:

1. **Developing an AI Agent** - this section is for registering an agent itself
2. **Implementing ATCP/IP** - this section is for implementing the ***2. An ATCP/IP Transaction*** section of the <a href="https://drive.google.com/file/d/1IM74cpN8TfS811gTaXxxkRH8QgpLFzZs/view" target="_blank">:page_with_curl: Whitepaper</a>.

# Developing an Agent

Below are details on how to:

* Register an AI Agent as IP
* Add License Terms to your AI Agent

## Registering an Agent

<Cards columns={1}>
  <Card title="Example Agent" href="https://explorer.story.foundation/ipa/0x123C52537C98d6064bF7BbdC7D4A93A9D9c8E43A" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    View an example Agent named Benjamin, registered on our explorer, here.
  </Card>
</Cards>

In order to register an AI Agent (or any IP) on Story, follow the [How to Register IP on Story](https://docs.story.foundation/docs/how-to-register-ip-on-story#/) tutorial. The only difference for AI Agents is how you structure your IP Metadata, which should follow the [IPA Metadata Standard](https://docs.story.foundation/docs/ipa-metadata-standard#/).

Here is an example of what the IP Metadata should look like for your AI Agent (using Benjamin as an example):

```json
{
  title: "Benjamin",
  description: "Benjamin is the official mascot of Unleash Protocol and the First IPFi AI Agent.",
  ipType: "AI Agent",
  creators: [
    {
      name: "Unleash Protocol",
      address: "0xBBE1725E1Fef9C2C20Ac6CB984bB0E970Ab18aa9",
      contributionPercent: 100,
      description: "Benjamin is the official mascot of Unleash Protocol and the First IPFi AI Agent.",
      socialMedia: [{ platform: "twitter", url: "https://x.com/BenjaminOnIP" }]
    }
  ],
  tags: ["Benjamin", "Unleash Protocol", "Story", "AI Agent", "IPFI"],
}
```

## Adding Terms to your AI Agent

Upon registering your AI Agent, you can add license terms to it. However you can add more license terms to your AI Agent afterwards as well. Here is an example of how you attach commercial license terms to your agent using the SDK:

```typescript TypeScript
import { LicenseTerms } from '@story-protocol/core-sdk';

const commercialRemixTerms: LicenseTerms = {
  transferable: true,
  royaltyPolicy: RoyaltyPolicyLAP, // insert RoyaltyPolicyLAP address from https://docs.story.foundation/docs/deployed-smart-contracts
  defaultMintingFee: BigInt(0),
  expiration: BigInt(0),
  commercialUse: true,
  commercialAttribution: true,
  commercializerChecker: zeroAddress,
  commercializerCheckerData: zeroAddress,
  commercialRevShare: 50, // can claim 50% of derivative revenue
  commercialRevCeiling: BigInt(0),
  derivativesAllowed: true,
  derivativesAttribution: true,
  derivativesApproval: false,
  derivativesReciprocal: true,
  derivativeRevCeiling: BigInt(0),
  currency: SUSD, // insert SUSD address from https://docs.story.foundation/docs/deployed-smart-contracts
  uri: '',
}

const response = await client.ipAsset.registerPilTermsAndAttach({
  ipId: '0x4c1f8c1035a8cE379dd4ed666758Fb29696CF721',
  terms: [commercialRemixTerms],
  txOptions: { waitForTransaction: true },
})
console.log(`License Terms ${response.licenseTermsId} attached to IP Asset.`)
```
```typescript Request Type
export type RegisterPilTermsAndAttachRequest = {
  ipId: Address;
  terms: RegisterPILTermsRequest[];
  deadline?: string | number | bigint;
  txOptions?: TxOptions;
};
```
```typescript Response Type
export type RegisterPilTermsAndAttachResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  licenseTermsIds?: bigint[];
};
```

# Implementing ATCP/IP

Below are details on how to actually implement the ***2. An ATCP/IP Transaction*** section of the <a href="https://drive.google.com/file/d/1IM74cpN8TfS811gTaXxxkRH8QgpLFzZs/view" target="_blank">:page_with_curl: Whitepaper</a>.

## Registering your Agent's Outputs

In the same way you registered your AI Agent on Story, you can register its outputs as well. For example, if your agent produces images and you want to register your image, follow the [How to Register IP on Story](https://docs.story.foundation/docs/how-to-register-ip-on-story#/) tutorial.

## Creating Agreement Terms

As described in the <a href="https://drive.google.com/file/d/1IM74cpN8TfS811gTaXxxkRH8QgpLFzZs/view" target="_blank">:page_with_curl: Whitepaper</a>, agents will negotiate on what agreement terms are appropriate for the requested task:

> 2 **Terms formulation**: The provider agent will consider the request and choose an appropriate set of\
> license terms for the information being requested. The terms system used should be programmable in
> nature to facilitate the parsing and formulation of the terms, such as Story’s Programmable IP License
> (PIL)\[6].
>
> 3 **Negotiation** (optional): The agents may have an optional negotiation phase where terms may be\
> altered until they are deemed appropriate for both parties.
>
> * **Counter terms** (optional): During this step, the requester agent who is unsatisfied with the\
>   initial proposed terms can issue a counterproposal set of terms. Both agents have access to a
>   standardized terms system, enabling them to reference, add, or remove specific clauses without
>   ambiguity. These counter terms may include modifications to pricing, usage rights, durations,
>   licensing restrictions, or any other negotiated variables. By using a consistent, machine-readable
>   format for their counter terms, agents can seamlessly iterate and respond to each other’s proposals,
>   ensuring that the negotiation process remains logically coherent and easy to follow.
> * **Revised terms** (optional): After receiving counter terms, the provider agent can present revised\
>   terms, taking into account the requested modifications while retaining non-negotiable core principles. The agents effectively refine the licensing conditions through successive rounds of structured
>   interaction, where each iteration refines points of contention into more acceptable middle grounds.
>   Because both parties rely on the same underlying terms specification, these revisions maintain
>   internal consistency and simplify the comparison of multiple drafts over time. This mechanism
>   ensures that both agents can converge toward an agreement that accurately reflects their mutual
>   understanding and commercial intentions.
> * *This process could have multiple iterations until an agreement is reached*

Once agents agree on the terms, they can be created and attached to the registered asset with the SDK:

```typescript TypeScript
import { LicenseTerms } from '@story-protocol/core-sdk';

const commercialRemixTerms: LicenseTerms = {
  transferable: true,
  royaltyPolicy: RoyaltyPolicyLAP, // insert RoyaltyPolicyLAP address from https://docs.story.foundation/docs/deployed-smart-contracts
  defaultMintingFee: BigInt(1), // costs 1 SUSD to mint a license
  expiration: BigInt(0),
  commercialUse: true,
  commercialAttribution: true,
  commercializerChecker: zeroAddress,
  commercializerCheckerData: zeroAddress,
  commercialRevShare: 50, // can claim 50% of derivative revenue
  commercialRevCeiling: BigInt(0),
  derivativesAllowed: true,
  derivativesAttribution: true,
  derivativesApproval: false,
  derivativesReciprocal: true,
  derivativeRevCeiling: BigInt(0),
  currency: SUSD, // insert SUSD address from https://docs.story.foundation/docs/deployed-smart-contracts
  uri: '',
}

const response = await client.ipAsset.registerPilTermsAndAttach({
  ipId: '0x4c1f8c1035a8cE379dd4ed666758Fb29696CF721', // the ipId of the asset we're attaching to
  terms: [commercialRemixTerms],
  txOptions: { waitForTransaction: true },
})
console.log(`License Terms ${response.licenseTermsId} attached to IP Asset.`)
```
```typescript Request Type
export type RegisterPilTermsAndAttachRequest = {
  ipId: Address;
  terms: RegisterPILTermsRequest[];
  deadline?: string | number | bigint;
  txOptions?: TxOptions;
};
```
```typescript Response Type
export type RegisterPilTermsAndAttachResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  licenseTermsIds?: bigint[];
};
```

## Mint a License

As stated in the <a href="https://drive.google.com/file/d/1IM74cpN8TfS811gTaXxxkRH8QgpLFzZs/view" target="_blank">:page_with_curl: Whitepaper</a>, after agents have negotiated on a set of terms, the requester agent can mint a license from the provider agent with specific agreement terms attached:

> 4 **Acceptance**: The requester agent will formally accept the terms by minting an immutable token (the\
> agreement token) that encapsulates the terms and rules by which the information being provided is
> to be used. Once minted the agreement is binding and the agent should commit to memory all of the
> terms associated with the information.
>
> * **Payment** (optional): depending on the license agreement terms chosen, some agents will require\
>   an upfront payment in order to mint a license. Further, terms may stipulate a recurring fee or a
>   revenue share, which can be automated via Story’s royalty system for example.

Here is how that can be done in the SDK:

> ❗️ Paid Licenses
>
> Note that sometimes minting a license might cost a `mintingFee` (based on what the value is in the terms).
>
> The `mintingFee` is paid in a ERC20 `currency` (also in the terms) that must be whitelisted by the protocol. For example, one of the only whitelisted revenue tokens is SUSD, which can be minted for test purposes [here](https://odyssey.storyscan.xyz/address/0xC0F6E387aC0B324Ec18EAcf22EE7271207dCE3d5?tab=write_contract#0x40c10f19).
>
> Assuming the `mintingFee` is in SUSD and you have some, you have to approve the `RoyaltyModule.sol` contract to spend them on your behalf. Run the [approve transaction](https://odyssey.storyscan.xyz/address/0xC0F6E387aC0B324Ec18EAcf22EE7271207dCE3d5?tab=write_contract#0x095ea7b3) where the `spender` is the v1.2 (current deployment supported by the SDK) address of `RoyaltyModule.sol` [here](https://docs.story.foundation/docs/deployed-smart-contracts). And the value is >= the amount you're paying with the SDK.

```typescript TypeScript
const response = await client.license.mintLicenseTokens({
   licenseTermsId: "1", // the license terms id that are attached to the `licensorIpId`
   licensorIpId: "0xC92EC2f4c86458AFee7DD9EB5d8c57920BfCD0Ba", // the ipId of the asset
   receiver: "0x14dC79964da2C08b23698B3D3cc7Ca32193d9955", // who receives the minted license
   amount: 1, // the amount of licenses to mint
   txOptions: { waitForTransaction: true }
});

console.log(`License Token minted at transaction hash ${response.txHash}, License IDs: ${response.licenseTokenIds}`)
```
```typescript Request Type
export type MintLicenseTokensRequest = {
  licensorIpId: Address;
  licenseTermsId: string | number | bigint;
  licenseTemplate?: Address;
  amount?: number | string | bigint;
  receiver?: Address;
  txOptions?: TxOptions;
};
```
```typescript Response Type
export type MintLicenseTokensResponse = {
  licenseTokenIds?: bigint[];
  txHash?: string;
  encodedTxData?: EncodedTxData;
};
```

Now, the requesting agent has a license token that can be held, giving it the rights to use the provided asset based on the attached terms.

## Claim Revenue

Once the providing agent has been paid for their work (when the requesting agent minted a license that costed $), they can claim their due revenue with the SDK like so:

```typescript TypeScript
const response = await client.royalty.snapshotAndClaimByTokenBatch({
  royaltyVaultIpId: "0xC92EC2f4c86458AFee7DD9EB5d8c57920BfCD0Ba",
  currencyTokens: [SUSDAddress], // insert SUSD address from https://docs.story.foundation/docs/deployed-smart-contracts
  claimer: "0xC92EC2f4c86458AFee7DD9EB5d8c57920BfCD0Ba", // same as `royaltyVaultIpId` because the IP Account holds the royalty tokens
  txOptions: { waitForTransaction: true },
})

console.log(`Claimed revenue: ${response.amountsClaimed}`);
```
```typescript Request Type
export type SnapshotAndClaimByTokenBatchRequest = {
  royaltyVaultIpId: Address;
  currencyTokens: Address[];
  claimer?: Address;
  txOptions?: TxOptions;
};
```
```typescript Response Type
export type SnapshotAndClaimByTokenBatchResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  snapshotId?: bigint;
  amountsClaimed?: bigint;
};
```