---
title: Register Stability Images
excerpt: Learn how to license Stability AI-Generated images on Story.
deprecated: false
hidden: true
metadata:
  robots: index
---
In this tutorial, you will learn how to license and protect Stability AI-Generated images by registering it on Story.

## The Explanation

Let's say you generate an image using Stability AI. Without adding a proper license to your image, others could use it freely. In this tutorial, you will learn how to add a license to your Stability AI-Generated image so that if others want to use it, they must properly license it from you.

In order to register that IP on Story, you first need to mint an NFT to represent that IP, and then register that NFT on Story, turning it into an [🧩 IP Asset](doc:ip-asset).

## 0. Before you Start

There are a few steps you have to complete before you can start the tutorial.

1. Add your Story Network Testnet wallet's private key to `.env` file:

```yaml .env
WALLET_PRIVATE_KEY=
```

2. Go to [the Pinata dashboard](https://app.pinata.cloud/developers/api-keys) and create a new API key and a gateway. Add the JWT along with the gateway to your `.env` file:

```yaml .env
PINATA_JWT=
PINATA_GATEWAY=
```

3. Go to [Stability](https://platform.stability.ai/account/keys) and create a new API key. Add the new key to your `.env` file:

> 🚧 Stability Credits
>
> In order to generate an image, you'll need Stability credits. If you just created an account, you will probably have a free trial that will give you a few credits to start with.

```yaml .env
STABILITY_API_KEY=
```

4. Add your preferred RPC URL to your `.env` file. You can just use the public default one we provide:

```yaml .env
RPC_PROVIDER_URL=https://rpc.odyssey.storyrpc.io
```

5. Install the dependencies:

```Text Terminal
npm install @story-protocol/core-sdk pinata viem axios sharp form-data
```

## 1. Generate an Image

You can follow the [Stability API Reference](https://platform.stability.ai/docs/api-reference) to use the model of your choice. For this tutorial, we'll be using Stability's **Stable Image Core** generate endpoint to generate an image. The below is taken directly from their documentation.

```typescript main.ts
import fs from "node:fs";
import axios from "axios";
import FormData from "form-data";

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
```

## 1.5. (Optional) Condense the Image

Stability generates images that are heavy in size, and therefore expensive to store. Optionally, we can condense the produced image for faster loading speeds and less expensive storage costs.

```typescript main.ts
// previous code here ...

const condensedImgBuffer = await sharp(response.data)
  .png({ quality: 10 }) // Adjust the quality value as needed (between 0 and 100)
  .toBuffer()
```

## 2. Store Image in IPFS

Now that we have our image, we need to store it on IPFS so we can get a URL back to access it. In this tutorial we'll be using [Pinata](https://pinata.cloud/), a decentralized storage solution that makes storing images easy.

In a separate file, create two functions:

1. `createPublicGroup`: so we can make our stored images public
2. `uploadBlobToIPFS`: uploads our buffer to IPFS in the public group

```javascript utils/uploadToIpfs.ts
import { PinataSDK } from 'pinata'

const pinata = new PinataSDK({
  pinataJwt: process.env.PINATA_JWT,
	// you can put your pinata gateway here, or leave it empty
  pinataGateway: process.env.PINATA_GATEWAY
})

// create a group so we can store public images
export async function createPublicGroup() {
  const group = await pinata.groups.create({
    name: 'Public Stability Images',
    isPublic: true,
  })
  return group.id
}

// upload our image to ipfs by putting it in a public group
export async function uploadBlobToIPFS(blob: Blob, fileName: string, groupId: string): Promise<string> {
  const file = new File([blob], fileName, { type: 'image/png' })
  const { cid } = await pinata.upload.file(file).group(groupId)
  return cid
}
```

Back in the main file, use the `createPublicGroup` function we implemented to create a public space where we can store our images, and then call the `uploadBlobToIPFS` function to store our image in that public group:

```typescript main.ts
import { createPublicGroup, uploadBlobToIPFS } from './utils/uploadToIpfs.ts'

// previous code here ...

// only have to call this function once
// (if you want to create more images in the future, you don't need to call this again)
const groupId = await createPublicGroup()
// convert the buffer to a blob
const blob = new Blob([consenscedImageBuffer], { type: 'image/png' });
// store the blob on ipfs
const imageCid = await uploadBlobToIPFS(blob, 'lighthouse.png', groupId);
```

## 3. Set up your Story Config

Now that we have generated and stored our image, we can register the image as IP on Story. First, let's set up our config.

* Associated docs: [TypeScript SDK Setup](doc:typescript-sdk-setup)

```javascript main.ts
import { StoryClient, StoryConfig } from "@story-protocol/core-sdk";
import { http } from "viem";
import { privateKeyToAccount, Address, Account } from "viem/accounts";

// previous code here ...

const privateKey: Address = `0x${process.env.WALLET_PRIVATE_KEY}`;
const account: Account = privateKeyToAccount(privateKey);

const config: StoryConfig = {
  account: account,
  transport: http(process.env.RPC_PROVIDER_URL),
  chainId: "odyssey",
};
const client = StoryClient.newClient(config);
```

## 4. Set up your IP Metadata

View the [IPA Metadata Standard](doc:ipa-metadata-standard) and construct the metadata for your IP. You can use the `generateIpMetadata` function to properly format your metadata and ensure it is of the correct type, as shown below:

```javascript main.ts
import { IpMetadata } from "@story-protocol/core-sdk";

// previous code here...

const ipMetadata: IpMetadata = client.ipAsset.generateIpMetadata({
  title: 'Lighthousd',
  description: 'A lighthouse image generated by Stability Stable Image Core',
  ipType: 'image',
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
  ],
  creators: [
    {
      name: 'Jacob Tucker',
      contributionPercent: 100,
      address: account.address,
    },
  ],
})
```

## 5. Set up your NFT Metadata

The NFT Metadata follows the [ERC-721 Metadata Standard](https://eips.ethereum.org/EIPS/eip-721).

```javascript main.ts
// previous code here...

const nftMetadata = {
  name: 'NFT representing ownership of our image',
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
```

## 6. Upload your IP and NFT Metadata to IPFS

In the `uploadToIpfs.ts` file, create a function to upload your IP & NFT Metadata objects to IPFS:

```javascript utils/uploadToIpfs.ts
// previous code here ...

export async function uploadJSONToIPFS(jsonMetadata: any): Promise<string> {
  const { cid } = await pinata.upload.json(jsonMetadata)
	return cid
}
```

You can then use that function to upload your metadata, as shown below:

```javascript main.ts
import { uploadJSONToIPFS } from "./utils/uploadToIpfs";
import { createHash } from "crypto";

// previous code here...

const ipIpfsHash = await uploadJSONToIPFS(ipMetadata);
const ipHash = createHash("sha256")
  .update(JSON.stringify(ipMetadata))
  .digest("hex");
const nftIpfsHash = await uploadJSONToIPFS(nftMetadata);
const nftHash = createHash("sha256")
  .update(JSON.stringify(nftMetadata))
  .digest("hex");
```

## 7. Register the NFT as an IP Asset

In this step, we will use the [📦 SPG](doc:spg) to combine minting and registering our NFT into one transaction call.

First, in a separate script, you must create a new SPG NFT collection. You can do this with the SDK (view a working example [here](https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/utils/createSpgNftCollection.ts)):

> ❓ Why do we have to do this?
>
> In order to use the `mintAndRegisterIpAssetWithPilTerms` function below, we'll have to deploy an SPG NFT collection so that the SPG can do the minting for us.
>
> Instead of doing this, you could technically write your own contract that implements [ISPGNFT](https://github.com/storyprotocol/protocol-periphery-v1/blob/main/contracts/interfaces/ISPGNFT.sol). But an easy way to create a collection that implements `ISPGNFT` is just to call the `createCollection` function in the SPG contract using the SDK, as shown below.

```typescript utils/createSpgNftCollection.ts
import { StoryClient, StoryConfig } from '@story-protocol/core-sdk'
import { http } from 'viem

const privateKey: Address = `0x${process.env.WALLET_PRIVATE_KEY}`
const account: Account = privateKeyToAccount(privateKey)

const config: StoryConfig = {
  account: account,
  transport: http(process.env.RPC_PROVIDER_URL),
  chainId: 'odyssey',
}
const client = StoryClient.newClient(config)

const newCollection = await client.nftClient.createNFTCollection({
  name: 'Stability NFTs',
  symbol: 'SNFT',
  isPublicMinting: true,
  mintOpen: true,
  mintFeeRecipient: zeroAddress,
  contractURI: '',
  txOptions: { waitForTransaction: true },
})

console.log(
  `New SPG NFT collection created at transaction hash ${newCollection.txHash}`,
  `SPG NFT contract address: ${newCollection.spgNftContract}`
)
```

Run this file and look at the console output. Copy the SPG NFT contract address and add that value as `SPG_NFT_CONTRACT_ADDRESS` to your `.env` file:

```Text env
SPG_NFT_CONTRACT_ADDRESS=
```

> 📘 Note
>
> You only have to do the above step **once**. Once you have your SPG NFT contract address, you can register any amount of IPs and will **not** have to do this again.

The code below will mint an NFT, register it as an [🧩 IP Asset](doc:ip-asset), set [License Terms](doc:license-terms) on the IP, and then set both NFT & IP metadata.

* Associated Docs: [Mint, Register, and Attach Terms](https://docs.story.foundation/docs/attach-terms-to-an-ip-asset#mint-nft-register-as-ip-asset-and-attach-terms)

```typescript main.ts
import { CreateIpAssetWithPilTermsResponse } from "@story-protocol/core-sdk";
import { Address } from "viem";

// previous code here ...

const response: CreateIpAssetWithPilTermsResponse =
  await client.ipAsset.mintAndRegisterIpAssetWithPilTerms({
    spgNftContract: process.env.SPG_NFT_CONTRACT_ADDRESS as Address,
    terms: [], // IP already has non-commercial social remixing terms. You can add more here.
    ipMetadata: {
      ipMetadataURI: process.env.PINATA_GATEWAY + '/files/' + ipIpfsHash,
      ipMetadataHash: `0x${ipHash}`,
      nftMetadataURI: process.env.PINATA_GATEWAY + '/files/' + nftIpfsHash,
      nftMetadataHash: `0x${nftHash}`,
    },
    txOptions: { waitForTransaction: true },
  });

console.log(
  `Root IPA created at transaction hash ${response.txHash}, IPA ID: ${response.ipId}`
);
console.log(
  `View on the explorer: https://explorer.story.foundation/ipa/${response.ipId}`
);
```

## 8. Done!