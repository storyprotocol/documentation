---
title: Finetune Images on Story
excerpt: >-
  Learn how to use the FLUX Finetuning API to finetune images and then register
  the output on Story in TypeScript.
deprecated: false
hidden: true
metadata:
  robots: index
---
In this tutorial, you will use the FLUX Finetuning API to take a bunch of images of Story's mascot "Ippy" and finetune an AI model to create similar images along with a prompt. Then you will monetize and protect the output IP on Story.

> 📘 This Tutorial is in TypeScript
>
> Steps 1-3 of this tutorial are based on the [FLUX Finetuning Beta Guide](https://docs.bfl.ml/finetuning/), which contains examples for calling their API in Python, however I have rewritten them in TypeScript.

## The Explanation

Generative text-to-image models often do not fully capture a creator’s unique vision, and have insufficient knowledge about specific objects, brands or visual styles. With the FLUX Pro Finetuning API, creators can use existing images to finetune an AI to create similar images, along with a prompt.

When an image is created, we will register it as IP on Story in order to grow, monetize, and protect the IP.

## 0. Before you Start

There are a few steps you have to complete before you can start the tutorial.

1. Add your Story Network Testnet wallet's private key to `.env` file:

```yaml .env
WALLET_PRIVATE_KEY=
```

2. Go to [Pinata](https://pinata.cloud/) and create a new API key. Add the JWT to your `.env` file:

```yaml .env
PINATA_JWT=
```

3. Go to [BFL](https://api.us1.bfl.ai/auth/profile) and create a new API key. Add the new key to your `.env` file:

> 🚧 BFL Credits
>
> In order to generate an image, you'll need BFL credits. If you just created an account, you will need to purchase credits [here](https://api.us1.bfl.ai/auth/profile).
>
> You can also see the pricing for each of the API endpoints [here](https://docs.bfl.ml/pricing/).

```yaml .env
BFL_API_KEY=
```

4. Add your preferred Story RPC URL to your `.env` file. You can just use the public default one we provide:

```yaml .env
RPC_PROVIDER_URL=https://rpc.odyssey.storyrpc.io
```

5. Install the dependencies:

```Text Terminal
npm install @story-protocol/core-sdk axios pinata-web3 viem
```

## 1. Compile the Training Data

In order to create a finetune, we'll need the input training data!

1. Create a folder in your project called `images`. In that folder, add a bunch of images that you want your finetune to train on. *Supported formats: JPG, JPEG, PNG, and WebP. Also recommended to use more than 5 images.*
2. Add Text Descriptions (Optional): In the same folder, create text files with descriptions for your images. Text files should share the same name as their corresponding images. *Example: if your image is "sample.jpg", create "sample.txt"*
3. Compress your folder into a ZIP file. It should be named `images.zip`

## 2. Create a Finetune

In order to generate an image using a similar style as input images, we need to create a **finetune**. Think of a finetune as an AI that knows all of your input images and can then start producing new ones.

Let's make a function that calls FLUX's `/v1/finetune` API route.

> 📘 Official Docs
>
> In order to learn what each of the parameters in the payload are, see the official `/v1/finetune` API docs [here](https://api.us1.bfl.ai/scalar#tag/tasks/POST/v1/finetune).

```typescript main.ts
interface FinetunePayload {
  finetune_comment: string;
  trigger_word: string;
  file_data: string;
  iterations: number;
  mode: string;
  learning_rate: number;
  captioning: boolean;
  priority: string;
  lora_rank: number;
  finetune_type: string;
}

async function requestFinetuning(
  zipPath: string,
  finetuneComment: string,
  triggerWord = "TOK",
  mode = "general",
  iterations = 300,
  learningRate = 0.00001,
  captioning = true,
  priority = "quality",
  finetuneType = "full",
  loraRank = 32
) {
  if (!fs.existsSync(zipPath)) {
    throw new Error(`ZIP file not found at ${zipPath}`);
  }

  const modes = ["character", "product", "style", "general"];
  if (!modes.includes(mode)) {
    throw new Error(`Invalid mode. Must be one of: ${modes.join(", ")}`);
  }

  const fileData = fs.readFileSync(zipPath);
  const encodedZip = Buffer.from(fileData).toString("base64");

  const url = "https://api.us1.bfl.ai/v1/finetune";
  const headers = {
    "Content-Type": "application/json",
    "X-Key": process.env.BFL_API_KEY,
  };

  const payload: FinetunePayload = {
    finetune_comment: finetuneComment,
    trigger_word: triggerWord,
    file_data: encodedZip,
    iterations,
    mode,
    learning_rate: learningRate,
    captioning,
    priority,
    lora_rank: loraRank,
    finetune_type: finetuneType,
  };

  try {
    const response = await axios.post(url, payload, { headers });
    return response.data;
  } catch (error) {
    throw new Error(`Finetune request failed: ${error}`);
  }
}
```

Next, call the `requestFinetuning` function we just made:

```typescript main.ts
const finetune = await requestFinetuning(
  "./images.zip",
  "ippy-finetune",
  "TOK",
  "general",
  300,
  0.00001,
  true,
  "quality",
  "full",
  32
);
```

This will return something that looks like:

```json
{ 
  finetune_id: '6fc5e628-6f56-48ec-93cb-c6a6b22bf5a' 
}
```

This is your `finetune_id`, and will be used to create images in the following steps.

## 3. Run Inference

Now that we have trained a finetune, we will use the model to create images. "Running an inference" simply means using our new model (identified by its `finetune_id`), which is trained on our images, to create new images.

There are several different inference endpoints we can use, each with [their own pricing](https://docs.bfl.ml/pricing/) (found at the bottom of the page). For this tutorial, I'll be using the `/v1/flux-pro-1.1-ultra-finetuned` endpoint, which is documented [here](https://api.us1.bfl.ai/scalar#tag/tasks/POST/v1/flux-pro-1.1-ultra-finetuned).

Here is how we can call this endpoint:

```typescript main.ts
async function finetuneInference(
  finetuneId: string,
  prompt: string,
  finetuneStrength = 1.2,
  endpoint = "flux-pro-1.1-ultra-finetuned",
  additionalParams: Record<string, any> = {}
) {
  const url = `https://api.us1.bfl.ai/v1/${endpoint}`;
  const headers = {
    "Content-Type": "application/json",
    "X-Key": process.env.BFL_API_KEY,
  };

  const payload = {
    finetune_id: finetuneId,
    finetune_strength: finetuneStrength,
    prompt,
    ...additionalParams,
  };

  try {
    const response = await axios.post(url, payload, { headers });
    return response.data;
  } catch (error) {
    throw new Error(`Finetune inference failed: ${error}`);
  }
}
```

Next, call the `finetuneInference` function we just made:

```typescript main.ts
const inference = await finetuneInference(
  "6fc5e628-6f56-48ec-93cb-c6a6b22bf5a", // the finetune_id we got above
  "A picture of Ippy being really happy."
);
```

This will return something that looks like:

```json
{
  id: '023a1507-369e-46e0-bd6d-1f3446d7d5f2',
  status: 'Pending',
  result: null,
  progress: null
}
```

As you can see, the status is still pending. We must wait until the generation is ready to view our image. To do this, we will need a function to fetch our new inference to see if its ready and view the details about it:

```typescript main.ts
async function getInference(id: string) {
  const url = "https://api.us1.bfl.ai/v1/get_result";
  const headers = {
    "Content-Type": "application/json",
    "X-Key": process.env.BFL_API_KEY,
  };

  try {
    const response = await axios.get(url, { headers, params: { id } });
    return response.data;
  } catch (error) {
    throw new Error(`Inference retrieval failed: ${error}`);
  }
}
```

In our code, lets add a loop that continuously fetches the inference until it's ready. When it's ready, we will view the new image.

```typescript main.ts
// already called this
const inference = await finetuneInference(
  "6fc5e628-6f56-48ec-93cb-c6a6b22bf5a",
  "A picture of Ippy being really happy."
);

let inferenceData = await getInference(inference.id);
while (inferenceData.status != "Ready") {
  console.log("Waiting for inference to complete...");
  // wait 5 seconds
  await new Promise((resolve) => setTimeout(resolve, 5000));
  // fetch the inference again
  inferenceData = await getInference(inference.id);
}
// now the inference is ready
console.log(inferenceData);
```

Once the loop completed, the final `console.log` will look like:

```json
{
  id: '023a1507-369e-46e0-bd6d-1f3446d7d5f2',
  status: 'Ready',
  result: {
    sample: 'https://delivery-us1.bfl.ai/results/746585f8d1b341f3a8735ababa563ac1/sample.jpeg?se=2025-01-16T19%3A50%3A11Z&sp=r&sv=2024-11-04&sr=b&rsct=image/jpeg&sig=pPtWnntLqc49hfNnGPgTf4BzS6MZcBgHayrYkKe%2BZIc%3D',
    prompt: 'A picture of Ippy being really happy.'
  },
  progress: null
}
```

You can paste the `sample` into your browser and see the final result! Make sure to save this image as it will disappear eventually.

## 4. Set up your Story Config

Next we will register this image on Story as an [🧩 IP Asset](doc:ip-asset) in order to monetize and license the IP.

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

## 5. Set up your IP Metadata

View the [IPA Metadata Standard](doc:ipa-metadata-standard) and construct your metadata for your IP. You can use the `generateIpMetadata` function to properly format your metadata and ensure it is of the correct type, as shown below:

```javascript main.ts
import { IpMetadata } from "@story-protocol/core-sdk";

// previous code here...

const ipMetadata: IpMetadata = client.ipAsset.generateIpMetadata({
  title: 'Happy Ippy',
  description: "An image of Ippy being really happy, generated by FLUX's 1.1 [pro] ultra Finetune",
  ipType: 'image',
  attributes: [
    {
      key: 'Model',
      value: 'FLUX 1.1 [pro] ultra Finetune',
    },
    {
      key: 'Prompt',
      value: 'A picture of Ippy being really happy.',
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

## 6. Set up your NFT Metadata

The NFT Metadata follows the [ERC-721 Metadata Standard](https://eips.ethereum.org/EIPS/eip-721).

```javascript main.ts
// previous code here...

const nftMetadata = {
  name: 'Ippy Ownership NFT',
  description: "This NFT represents ownership of the Happy Ippy image generated by FLUX's 1.1 [pro] ultra Finetune",
  image: inferenceData.result.sample,
  attributes: [
    {
      key: 'Model',
      value: 'FLUX 1.1 [pro] ultra Finetune',
    },
    {
      key: 'Prompt',
      value: 'A picture of Ippy being really happy.',
    },
  ],
}
```

## 7. Upload your IP and NFT Metadata to IPFS

In a separate file, create a function to upload your IP & NFT Metadata objects to IPFS:

```javascript utils/uploadToIpfs.ts
import { PinataSDK } from "pinata-web3";

const pinata = new PinataSDK({
  pinataJwt: process.env.PINATA_JWT,
});

export async function uploadJSONToIPFS(jsonMetadata: any): Promise<string> {
  const { IpfsHash } = await pinata.upload.json(jsonMetadata);
  return IpfsHash;
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

## 8. Register the NFT as an IP Asset

In this step, we will use the [📦 SPG](doc:spg) to combine minting and registering our NFT into one transaction call.

First, in a separate script, you must create a new SPG NFT collection. You can do this with the SDK (view a working example [here](https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/utils/createSpgNftCollection.ts)):

> ❓ Why do we have to do this?
>
> In order to use the `mintAndRegisterIpAssetWithPilTerms` function below, we'll have to deploy an SPG NFT collection so that the SPG can do the minting for us.
>
> Instead of doing this, you could technically write your own contract that implements [ISPGNFT](https://github.com/storyprotocol/protocol-periphery-v1/blob/main/contracts/interfaces/ISPGNFT.sol). But an easy way to create a collection that implements `ISPGNFT` is just to call the `createCollection` function in the SPG contract using the SDK, as shown below.

```typescript utils/createSpgNftCollection.ts
import { StoryClient, StoryConfig } from '@story-protocol/core-sdk'
import { http } from 'viem'

const privateKey: Address = `0x${process.env.WALLET_PRIVATE_KEY}`
const account: Account = privateKeyToAccount(privateKey)

const config: StoryConfig = {
  account: account,
  transport: http(process.env.RPC_PROVIDER_URL),
  chainId: 'odyssey',
}
const client = StoryClient.newClient(config)

const newCollection = await client.nftClient.createNFTCollection({
  name: 'Ippy NFTs',
  symbol: 'IPNFT',
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
import { Address } from "viem";

// previous code here ...

const response = await client.ipAsset.mintAndRegisterIpAssetWithPilTerms({
  spgNftContract: process.env.SPG_NFT_CONTRACT_ADDRESS as Address,
  terms: [], // IP already has non-commercial social remixing terms. You can add more here.
  ipMetadata: {
    ipMetadataURI: `https://ipfs.io/ipfs/${ipIpfsHash}`,
    ipMetadataHash: `0x${ipHash}`,
    nftMetadataURI: `https://ipfs.io/ipfs/${nftIpfsHash}`,
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

## 9. Done!