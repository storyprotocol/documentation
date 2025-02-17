---
title: Attach Terms to an IPA
excerpt: Learn how to Attach License Terms to an IP Asset in TypeScript.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
This section demonstrates how to attach [License Terms](doc:license-terms) to an [🧩 IP Asset](doc:ip-asset). By attaching terms, users can publicly mint [License Tokens](doc:license-token) (the on-chain "license") with those terms from the IP.

### :warning: Prerequisites

There are a few steps you have to complete before you can start the tutorial.

1. Complete the [TypeScript SDK Setup](doc:typescript-sdk-setup)

## 1. Before We Start

We should mention that you do not need an existing IP Asset to attach terms to it. There are two functions you can use that allow you to **register IP + create terms + attach terms** in the same function:

* [mintAndRegisterIpAssetWithPilTerms](https://docs.story.foundation/docs/sdk-ipasset#mintandregisteripassetwithpilterms)
* [registerIpAndAttachPilTerms](https://docs.story.foundation/docs/sdk-ipasset#registeripandattachpilterms)

## 2. Register License Terms

In order to attach terms to an IP Asset, let's first create them!

[License Terms](doc:license-terms) are a configurable set of values that define restrictions on licenses minted from your IP that have those terms. For example, "If you mint this license, you must share 50% of your revenue with me." You can view the full set of terms in [PIL Terms](doc:pil-terms).

> 📘 Duplicate Terms
>
> If License Terms already exist on our protocol for the identical set of parameters you intend to create, it is unnecessary to create it again and the function will simply return the existing `licenseTermsId` and an undefined `txHash`. License Terms are protocol-wide, so you can use existing License Terms by its `licenseTermsId`.

Below is a code example showing how to create new terms:

> Associated Docs: [license.registerPILTerms](https://docs.story.foundation/docs/sdk-license#registerpilterms)

```typescript main.ts
import { LicenseTerms } from '@story-protocol/core-sdk';
import { zeroAddress } from 'viem';
// you should already have a client set up (prerequisite)
import { client } from './utils';

async function main() {
  const licenseTerms: LicenseTerms = {
    defaultMintingFee: 0n,
    // must be a whitelisted revenue token from https://docs.story.foundation/docs/deployed-smart-contracts
    currency: '0x1514000000000000000000000000000000000000',
    // insert RoyaltyPolicyLAP address from https://docs.story.foundation/docs/deployed-smart-contracts
    royaltyPolicy: '0xBe54FB168b3c982b7AaE60dB6CF75Bd8447b390E',
    transferable: false,
    expiration: 0n,
    commercialUse: false,
    commercialAttribution: false,
    commercializerChecker: zeroAddress,
    commercializerCheckerData: '0x',
    commercialRevShare: 0,
    commercialRevCeiling: 0n,
    derivativesAllowed: false,
    derivativesAttribution: false,
    derivativesApproval: false,
    derivativesReciprocal: false,
    derivativeRevCeiling: 0n,
    uri: '',
  }

  const response = await client.license.registerPILTerms({ 
    ...licenseTerms, 
    txOptions: { waitForTransaction: true } 
  })

  console.log(`PIL Terms registered at transaction hash ${response.txHash}, License Terms ID: ${response.licenseTermsId}`)  
}

main();
```

### 2a. :icecream: PIL Flavors

As you see above, you have to choose between a lot of terms.

We have convenience functions to help you register new terms. We have created [PIL Flavors](doc:pil-flavors), which are pre-configured popular combinations of License Terms to help you decide what terms to use. You can view those PIL Flavors and then register terms using the following convenience functions:

<Cards columns={4}>
  <Card title="Non-Commercial Social Remixing" href="https://docs.story.foundation/docs/pil-flavors#flavor-1-non-commercial-social-remixing" icon="fa-file" target="_blank">
    Free remixing with attribution. No commercialization.
  </Card>

  <Card title="Commercial Use" href="https://docs.story.foundation/docs/pil-flavors#flavor-2-commercial-use" icon="fa-file" target="_blank">
    Pay to use the license with attribution, but don't have to share revenue.
  </Card>

  <Card title="Commercial Remix" href="https://docs.story.foundation/docs/pil-flavors#flavor-3-commercial-remix" icon="fa-file" target="_blank">
    Pay to use the license with attribution and pay % of revenue earned.
  </Card>

  <Card title="Creative Commons Attribution" href="https://docs.story.foundation/docs/pil-flavors#flavor-4-creative-commons-attribution" icon="fa-file" target="_blank">
    Free remixing and commercial use with attribution.
  </Card>
</Cards>

## 3. Attach License Terms

Now that we have created terms and have the associated `licenseTermsId`, we can attach them to an existing IP Asset like so:

> Associated Docs: [license.attachLicenseTerms](https://docs.story.foundation/docs/sdk-license#attachlicenseterms)

```typescript TypeScript
import { LicenseTerms } from '@story-protocol/core-sdk';
import { zeroAddress } from 'viem';
// you should already have a client set up (prerequisite)
import { client } from './utils';

async function main() {
  // previous code here ...
  
  const response = await client.license.attachLicenseTerms({
    // insert your newly created license terms id here
    licenseTermsId: LICENSE_TERMS_ID, 
    // insert the ipId you want to attach terms to here
    ipId: "0x4c1f8c1035a8cE379dd4ed666758Fb29696CF721",
    txOptions: { waitForTransaction: true }
  });

  if (response.success) {
    console.log(`Attached License Terms to IPA at transaction hash ${response.txHash}.`)
  } else {
    console.log(`License Terms already attached to this IPA.`)
  }
}

main();
```

### 3a. Create Terms + Attach

:point_right: It's worth mentioning that you can **create terms + attach terms** all in the same step with the the [registerPilTermsAndAttach](https://docs.story.foundation/docs/sdk-ipasset#registerpiltermsandattach) function. Whatever is easiest for you!

:point_right: And, like we mentioned at the beginning, there are two functions you can use that allow you to **register IP + create terms + attach terms** in the same function:

* [mintAndRegisterIpAssetWithPilTerms](https://docs.story.foundation/docs/sdk-ipasset#mintandregisteripassetwithpilterms)
* [registerIpAndAttachPilTerms](https://docs.story.foundation/docs/sdk-ipasset#registeripandattachpilterms)

## 4. Mint a License

Now that we have attached License Terms to our IP, the next step is minting a License Token, which we'll go over on the next page.