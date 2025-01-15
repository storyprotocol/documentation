---
title: AI Agents on Story
excerpt: Learn how to register and add terms to your AI Agent on Story.
deprecated: false
hidden: false
metadata:
  robots: index
---
Below are details on how to:

* Register an AI Agent as IP
* Add License Terms to your AI Agent
* Register Your Agent's Outputs as IP

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

Upon registering your AI Agent, you can add license terms to it. However you can add more license terms to your AI Agent afterwards as well. Here is an example of how you attach commercial license terms to your agnet using the SDK:

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
  ipId: '0x4c1f8c1035a8cE379dd4ed666758Fb29696CF721', // the ipId of your AI Agent
  terms: [commercialRemixTerms],
  txOptions: { waitForTransaction: true },
})
console.log(`License Terms ${response.licenseTermsId} attached to IP Asset.`)
```

## Registering your Agent's Outputs

In the same way you registered your AI Agent on Story, you can register its outputs as well. For example, if you agent produces images and you want to register your image, follow the [How to Register IP on Story](https://docs.story.foundation/docs/how-to-register-ip-on-story#/) tutorial.