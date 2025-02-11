---
title: Register a Derivative
excerpt: >-
  Learn how to register a derivative/remix IP Asset as a child of another in
  TypeScript.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/registerDerivativeCommercialSpg.ts" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    All of this page is covered in this working code example.
  </Card>
</Cards>

This section demonstrates how to register an IP Asset as a derivative of another.

### :warning: Prerequisites

There are a few steps you have to complete before you can start the tutorial.

1. Complete the [TypeScript SDK Setup](doc:typescript-sdk-setup)

## 0. Before We Start

There are a lot of ways to register an IP Asset as a derivative of another. Below, we will help you figure out what function you should use.

> 📘 Have no idea?
>
> It is best to figure out which SDK function to use based on what is easiest for you. But if you have no idea, simply continue to the next section.

Do you already have a [License Token](doc:license-token) you can use?

* :white_check_mark: Yes: Is the derivative IP Asset already registered?
  * :white_check_mark: Yes: Use [registerDerivativeWithLicenseTokens](https://docs.story.foundation/docs/sdk-ipasset#/registerderivativewithlicensetokens)
  * :x: No: Do you have your own NFT contract, or an already minted NFT?
    * :white_check_mark: Yes: Use [registerIpAndMakeDerivativeWithLicenseTokens](https://docs.story.foundation/docs/sdk-ipasset#/registeripandmakederivativewithlicensetokens)
    * :x: No: Use [mintAndRegisterIpAndMakeDerivativeWithLicenseTokens](https://docs.story.foundation/docs/sdk-ipasset#/mintandregisteripandmakederivativewithlicensetokens)
* :x: No: Is the derivative IP Asset already registered?
  * :white_check_mark: Yes: Use [registerDerivative](https://docs.story.foundation/docs/sdk-ipasset#/registerderivative)
  * :x: No: Do you have your own NFT contract, or an already minted NFT?
    * :white_check_mark: Yes: Use [registerDerivativeIp](https://docs.story.foundation/docs/sdk-ipasset#/registerderivativeip)
    * :x: No: Use [mintAndRegisterIpAndMakeDerivative](https://docs.story.foundation/docs/sdk-ipasset#/mintandregisteripandmakederivative)

### 0a. :question: Why would I ever use a License Token if it's not needed?

There are a few times when **you would need** a License Token to register a derivative:

* The License Token contains private license terms, so you would only be able to register as a derivative if you had the License Token that was manually minted by the owner. More on that [here](https://docs.story.foundation/docs/license-token#private-licenses).
* The License Token (which is an NFT) costs a `mintingFee` to mint, and you were able to buy it on a marketplace for a cheaper price. Then it makes more sense to simply register with the License Token then have to pay the more expensive `defaultMintingFee`.

## 1. Register Derivative

> 📘 This is just an example
>
> You are encouraged to figure out the best derivative function to use based on the survey above. However, if you don't know and want to be walked through one solution, this next part is for you.

We're going to assume you have :x: no license tokens, :x: the derivative IP is not yet registered, and :x: you don't have your own NFT contract or an already minted NFT.

**Follow steps 1-4 of** [Register an IP Asset](doc:register-an-ip-asset). Note you can skip step 4 if you already have an SPG NFT Collection. Then, come back here.

Modify your code such that...

1. Instead of using `mintAndRegisterIp`, use `mintAndRegisterIpAndMakeDerivative`
2. Add a `derivData` field, where:
   1. `parentIpIds` is the `ipIds` of the parents you want to become a derivative of. **NOTE: Once you become a derivative, you cannot add more parents**
   2. `licenseTermIds` is an array of license terms you want to register under. These are the terms your derivative must abide by
   3. Set `maxMintingFee`, `maxRts`, and `maxRevenueShare` to be left as disabled/default as shown below

Now we can call the function like so:

* Associated Docs: [ipAsset.mintAndRegisterIpAndMakeDerivative](https://docs.story.foundation/docs/sdk-ipasset#/mintandregisteripandmakederivative)

```typescript main.ts
import { IpMetadata, DerivativeData } from '@story-protocol/core-sdk'
import { client } from './utils'
import { uploadJSONToIPFS } from './uploadToIpfs'
import { createHash } from 'crypto'
import { Address } from 'viem'

async function main() {
  // previous code here ...
  
  const derivData: DerivativeData = {
    // TODO: insert the parent's ipId
    parentIpIds: [PARENT_IP_ID],
    // TODO: insert the licenseTermsId attached to parent IpId
    licenseTermsIds: [LICENSE_TERMS_ID],
    maxMintingFee: BitInt(0), // disabled
    maxRts: 100_000_000, // default
    maxRevenueShare: 100, // default
  };

  const response = await client.ipAsset.mintAndRegisterIpAndMakeDerivative({
    // TODO: insert your NFT contract address created by the SPG
    spgNftContract: SPG_NFT_CONTRACT_ADDRESS as Address,
    derivData,
    allowDuplicates: true,
    ipMetadata: {
      ipMetadataURI: `https://ipfs.io/ipfs/${ipIpfsHash}`,
      ipMetadataHash: `0x${ipHash}`,
      nftMetadataURI: `https://ipfs.io/ipfs/${nftIpfsHash}`,
      nftMetadataHash: `0x${nftHash}`,
    },
    txOptions: { waitForTransaction: true }
  });

  console.log(`Completed at transaction hash ${response.txHash}, IPA ID: ${response.ipId}, Token ID: ${response.tokenId}`);
}
```

## 2. Paying and Claiming Revenue

Congratulations, you registered a derivative IP Asset!

<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/registerDerivativeCommercialSpg.ts" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    All of this page is covered in this working code example.
  </Card>
</Cards>

Now that we have established parent-child IP relationships, we can begin to explore payments and automated revenue share based on the license terms. We'll cover that in the upcoming pages.