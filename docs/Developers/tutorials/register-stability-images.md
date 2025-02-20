---
title: Register & Monetize Stability Images
excerpt: >-
  Learn how to register, protect, and monetize Stability AI-Generated images on
  Story.
deprecated: false
hidden: false
metadata:
  robots: index
---
In this tutorial, you will learn how to:

1. Generate an image with Stability AI
2. Upload your image to Pinata IPFS
3. Register your image as IP on Story
4. Attach License Terms to your IP

## The Explanation

Let's say you generate an image using Stability AI. Without adding a proper license to your image, others could use it freely. In this tutorial, you will learn how to add a license to your Stability AI-Generated image so that if others want to use it, they must properly license it from you.

## 0. Before you Start

There are a few steps you have to complete before you can start the tutorial.

1. You will need to install [Node.js](https://nodejs.org/en/download) and [npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm). If you've coded before, you likely have these.
2. Add your Story Network Testnet wallet's private key to `.env` file:

```yaml .env
WALLET_PRIVATE_KEY=
```

3. Go to [the Pinata dashboard](https://app.pinata.cloud/developers/api-keys) and create a new API key and a gateway. Add the JWT along with the gateway to your `.env` file:

```yaml .env
PINATA_JWT=
PINATA_GATEWAY=
```

4. Go to [Stability](https://platform.stability.ai/account/keys) and create a new API key. Add the new key to your `.env` file:

> 🚧 Stability Credits
>
> In order to generate an image, you'll need Stability credits. If you just created an account, you will probably have a free trial that will give you a few credits to start with.

```yaml .env
STABILITY_API_KEY=
```

5. Add your preferred RPC URL to your `.env` file. You can just use the public default one we provide:

```yaml .env
RPC_PROVIDER_URL=https://aeneid.storyrpc.io
```

6. Install the dependencies:

```Text Terminal
npm install @story-protocol/core-sdk pinata-web3 viem axios sharp form-data
```

## 1. Generate an Image

You can follow the [Stability API Reference](https://platform.stability.ai/docs/api-reference) to use the model of your choice. For this tutorial, we'll be using Stability's **Stable Image Core** generate endpoint to generate an image. The below is taken directly from their documentation.

Create a `main.ts` file and add the following code:

```typescript main.ts
import fs from "fs";
import axios from "axios";
import FormData from "form-data";

async function main() {
  const payload = {
    prompt: 'Lighthouse on a cliff overlooking the ocean',
    output_format: 'png',
  }

  const response = await axios.postForm(
    `https://api.stability.ai/v2beta/stable-image/generate/core`,
    axios.toFormData(payload, new FormData()),
    {
      validateStatus: undefined,
      responseType: 'arraybuffer',
      headers: {
        Authorization: `Bearer ${process.env.STABILITY_API_KEY}`,
        Accept: 'image/*',
      },
    }
  ) 
}

main();
```

## 1.5. (Optional) Condense the Image

Stability generates images that are heavy in size, and therefore expensive to store. Optionally, we can condense the produced image for faster loading speeds and less expensive storage costs.

```typescript main.ts
import fs from "fs";
import axios from "axios";
import FormData from "form-data";

async function main() {
  // previous code here ...

  const condensedImgBuffer = await sharp(response.data)
    .png({ quality: 10 }) // Adjust the quality value as needed (between 0 and 100)
    .toBuffer() 
}

main();
```

## 2. Store Image in IPFS

Now that we have our image, we need to store it on IPFS so we can get a URL back to access it. In this tutorial we'll be using [Pinata](https://pinata.cloud/), a decentralized storage solution that makes storing images easy.

In a separate file `uploadToIpfs.ts`, create a function `uploadBlobToIPFS` that uploads our buffer to IPFS:

```typescript uploadToIpfs.ts
import { PinataSDK } from 'pinata-web3'

const pinata = new PinataSDK({
  pinataJwt: process.env.PINATA_JWT,
  // you can put your pinata gateway here, or leave it empty
  pinataGateway: process.env.PINATA_GATEWAY
})

// upload our image to ipfs by putting it in a public group
export async function uploadBlobToIPFS(blob: Blob, fileName: string): Promise<string> {
  const file = new File([blob], fileName, { type: 'image/png' })
  const { IpfsHash } = await pinata.upload.file(file)
  return IpfsHash
}
```

Back in the main file, call the `uploadBlobToIPFS` function to store our image:

```typescript main.ts
import fs from "fs";
import axios from "axios";
import FormData from "form-data";
import { uploadBlobToIPFS } from './uploadToIpfs.ts'

async function main() {
  // previous code here ...

  // convert the buffer to a blob
  const blob = new Blob([condensedImgBuffer], { type: 'image/png' });
  // store the blob on ipfs
  const imageCid = await uploadBlobToIPFS(blob, 'lighthouse.png'); 
}

main();
```

## 3. Set up your Story Config

Now that we have generated and stored our image, we can register the image as IP on Story. First, let's set up our config. In a `utils.ts` file, add the following code:

* Associated docs: [TypeScript SDK Setup](doc:typescript-sdk-setup)

```typescript utils.ts
import { StoryClient, StoryConfig } from "@story-protocol/core-sdk";
import { http } from "viem";
import { privateKeyToAccount, Address, Account } from "viem/accounts";

const privateKey: Address = `0x${process.env.WALLET_PRIVATE_KEY}`;
export const account: Account = privateKeyToAccount(privateKey);

const config: StoryConfig = {
  account: account,
  transport: http(process.env.RPC_PROVIDER_URL),
  chainId: "aeneid",
};
export const client = StoryClient.newClient(config);
```

## 4. Set up your IP Metadata

View the [IPA Metadata Standard](doc:ipa-metadata-standard) and construct the metadata for your IP. You can use the `generateIpMetadata` function to properly format your metadata and ensure it is of the correct type, as shown below:

```typescript main.ts
import fs from "fs";
import axios from "axios";
import FormData from "form-data";
import { uploadBlobToIPFS } from './uploadToIpfs.ts'
import { client, account } from './utils'

async function main() {
  // previous code here ...

  const ipMetadata = {
    "title": "Lighthouse",
    "description": "A generated picture of a lighthouse.",
    "createdAt": "1728401700",
    "image": process.env.PINATA_GATEWAY + '/files/' + imageCid,
    "imageHash": "0x...", // a hash of the image
    "mediaUrl": process.env.PINATA_GATEWAY + '/files/' + imageCid,
    "mediaHash": "0x...", // a hash of the image
    "mediaType": "image/png",
    "creators": [
      {
        "name": "Jacob Tucker",
        "address": "0x67ee74EE04A0E6d14Ca6C27428B27F3EFd5CD084",
        "description": "A cool dev rel person.",
        "contributionPercent": 100,
        "socialMedia": [
          {
            "platform": "Twitter",
            "url": "https://x.com/jacobmtucker"
          }
        ]
      }
    ]
  }
}

main();
```

## 5. Set up your NFT Metadata

The NFT Metadata follows the [ERC-721 Metadata Standard](https://eips.ethereum.org/EIPS/eip-721).

```javascript main.ts
import fs from "fs";
import axios from "axios";
import FormData from "form-data";
import { uploadBlobToIPFS } from './uploadToIpfs.ts'
import { client, account } from './utils'

async function main() {
  // previous code here ...

  const nftMetadata = {
    name: 'Ownership NFT',
    description: 'This NFT represents ownership of the image generated by Stability',
    image: process.env.PINATA_GATEWAY + '/files/' + imageCid,
    attributes: [
      {
        key: 'Model',
        value: 'Stability',
      },
      {
        key: 'Service',
        value: 'Stable Image Core'
      },
      {
        key: 'Prompt',
        value: 'Lighthouse on a cliff overlooking the ocean',
      },
    ]
  } 
}

main();
```

## 6. Upload your IP and NFT Metadata to IPFS

In the `uploadToIpfs.ts` file, create a function to upload your IP & NFT Metadata objects to IPFS:

```typescript uploadToIpfs.ts
// previous code here ...

export async function uploadJSONToIPFS(jsonMetadata: any): Promise<string> {
  const { IpfsHash } = await pinata.upload.json(jsonMetadata)
  return IpfsHash
}
```

You can then use that function to upload your metadata, as shown below:

```typescript main.ts
import fs from "fs";
import axios from "axios";
import FormData from "form-data";
import { uploadBlobToIPFS, uploadJSONToIPFS } from './uploadToIpfs.ts'
import { client, account } from './utils'
import { createHash } from "crypto";

async function main() {
  // previous code here ...

  const ipIpfsHash = await uploadJSONToIPFS(ipMetadata);
  const ipHash = createHash("sha256")
    .update(JSON.stringify(ipMetadata))
    .digest("hex");
  const nftIpfsHash = await uploadJSONToIPFS(nftMetadata);
  const nftHash = createHash("sha256")
    .update(JSON.stringify(nftMetadata))
    .digest("hex");
}

main();
```

## 7. Create License Terms

When registering your image on Story, you can attach [License Terms](doc:license-terms) to the IP. These are real, legally binding terms enforced on-chain by the [📜 Licensing Module](doc:licensing-module), disputable by the [❌ Dispute Module](doc:dispute-module), and in the worst case, able to be enforced off-chain in court through traditional means.

Let's say we want to monetize our image such that every time someone wants to use it (on merch, advertisement, or whatever) they have to pay an initial minting fee of 10 $WIP. Additionally, every time they earn revenue on derivative work, they owe 5% revenue back as royalty.

```typescript main.ts
import fs from "fs";
import axios from "axios";
import FormData from "form-data";
import { uploadBlobToIPFS, uploadJSONToIPFS } from './uploadToIpfs.ts'
import { client, account } from './utils'
import { createHash } from "crypto";
import { LicenseTerms, WIP_TOKEN_ADDRESS } from '@story-protocol/core-sdk';
import { zeroAddress } from 'viem';

async function main() {
  // previous code here ...

  const commercialRemixTerms: LicenseTerms = {
    transferable: true,
    royaltyPolicy: RoyaltyPolicyLAP, // insert RoyaltyPolicyLAP address from https://docs.story.foundation/docs/deployed-smart-contracts
    defaultMintingFee: BigInt(10),
    expiration: BigInt(0),
    commercialUse: true,
    commercialAttribution: true, // must give us attribution
    commercializerChecker: zeroAddress,
    commercializerCheckerData: zeroAddress,
    commercialRevShare: 5, // can claim 50% of derivative revenue
    commercialRevCeiling: BigInt(0),
    derivativesAllowed: true,
    derivativesAttribution: true,
    derivativesApproval: false,
    derivativesReciprocal: true,
    derivativeRevCeiling: BigInt(0),
    currency: WIP_TOKEN_ADDRESS,
    uri: '',
  } 
}

main();
```

## 8. Register an NFT as an IP Asset

In this step, we will use the [📦 SPG](doc:spg) to combine minting and registering our NFT into one transaction call.

First, in a separate file `createSpgNftCollection.ts`, you must create a new SPG NFT collection. You can do this with the SDK (view a working example [here](https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/utils/createSpgNftCollection.ts)):

> ❓ Why do we have to do this?
>
> In order to use the `mintAndRegisterIpAssetWithPilTerms` function below, we'll have to deploy an SPG NFT collection so that the SPG can do the minting for us.
>
> Instead of doing this, you could technically write your own contract that implements [ISPGNFT](https://github.com/storyprotocol/protocol-periphery-v1/blob/main/contracts/interfaces/ISPGNFT.sol). But an easy way to create a collection that implements `ISPGNFT` is just to call the `createCollection` function in the SPG contract using the SDK, as shown below.

```typescript createSpgNftCollection.ts
import { zeroAddress } from 'viem'
import { client } from './utils'

async function createSpgNftCollection() {
  // Create a new SPG NFT collection
  //
  // NOTE: Use this code to create a new SPG NFT collection. You can then use the
  // `newCollection.spgNftContract` address as the `spgNftContract` argument in
  // functions like `mintAndRegisterIpAssetWithPilTerms`, which you'll see later.
  //
  // You will mostly only have to do this once. Once you get your nft contract address,
  // you can use it in SPG functions.
  //
  const newCollection = await client.nftClient.createNFTCollection({
    name: 'Test NFT',
    symbol: 'TEST',
    isPublicMinting: true,
    mintOpen: true,
    mintFeeRecipient: zeroAddress,
    contractURI: '',
    txOptions: { waitForTransaction: true },
  })

  console.log(`New SPG NFT collection created at transaction hash ${newCollection.txHash}`)
  console.log(`NFT contract address: ${newCollection.spgNftContract}`)
}

createSpgNftCollection();
```

Run this file and look at the console output. Copy the SPG NFT contract address and add that value as `SPG_NFT_CONTRACT_ADDRESS` to your `.env` file:

```Text env
SPG_NFT_CONTRACT_ADDRESS=
```

> 📘 Note
>
> You only have to do the above step **once**. Once you have your SPG NFT contract address, you can register any amount of IPs and will **not** have to do this again.

The code below will mint an NFT, register it as an [🧩 IP Asset](doc:ip-asset), set [License Terms](doc:license-terms) on the IP, and then set both NFT & IP metadata.

* Associated Docs: [ipAsset.mintAndRegisterIp](https://docs.story.foundation/docs/sdk-ipasset#mintandregisterip)

```typescript main.ts
import fs from "fs";
import axios from "axios";
import FormData from "form-data";
import { uploadBlobToIPFS, uploadJSONToIPFS } from './uploadToIpfs.ts'
import { WIP_TOKEN_ADDRESS } from "@story-protocol/core-sdk";
import { client, account } from './utils'
import { createHash } from "crypto";
import { LicenseTerms, LicensingConfig } from '@story-protocol/core-sdk';
import { zeroAddress, Address } from 'viem';

async function main() {
  // previous code here ...
  
  // default license config
  const licensingConfig: LicensingConfig = {
    isSet: false,
    mintingFee: BigInt(0),
    licensingHook: zeroAddress,
    hookData: zeroHash,
    commercialRevShare: 0,
    disabled: false,
    expectMinimumGroupRewardShare: 0,
    expectGroupRewardPool: zeroAddress,
  };

  const response = await client.ipAsset.mintAndRegisterIpAssetWithPilTerms({
    spgNftContract: process.env.SPG_NFT_CONTRACT_ADDRESS as Address,
    allowDuplicates: true,
    // the terms we created in the previous step
    licenseTermsData: [{ terms: commercialRemixTerms, licensingConfig }],
    ipMetadata: {
      ipMetadataURI: process.env.PINATA_GATEWAY + '/files/' + ipIpfsHash,
      ipMetadataHash: `0x${ipHash}`,
      nftMetadataURI: process.env.PINATA_GATEWAY + '/files/' + nftIpfsHash,
      nftMetadataHash: `0x${nftHash}`,
    },
    txOptions: { waitForTransaction: true },
  });

  console.log(`Root IPA created at transaction hash ${response.txHash}, IPA ID: ${response.ipId}`);
  console.log(`View on the explorer: https://aeneid.explorer.story.foundation/ipa/${response.ipId}`); 
}

main();
```

## 9. Done!

Congratulations! Now your image is registered on Story with commercial license terms.