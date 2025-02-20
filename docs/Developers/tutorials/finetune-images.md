---
title: Finetune Images on Story
excerpt: >-
  Learn how to use the FLUX Finetuning API to finetune images and then register
  the output on Story in TypeScript.
deprecated: false
hidden: false
metadata:
  robots: index
---
<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/jacob-tucker/finetune-story-flux" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    View the completed code for this tutorial.
  </Card>
</Cards>

In this tutorial, you will use the FLUX Finetuning API to take a bunch of images of Story's mascot "Ippy" and finetune an AI model to create similar images along with a prompt. Then you will monetize and protect the output IP on Story.

> 📘 This Tutorial is in TypeScript
>
> Steps 1-3 of this tutorial are based on the [FLUX Finetuning Beta Guide](https://docs.bfl.ml/finetuning/), which contains examples for calling their API in Python, however I have rewritten them in TypeScript.

## The Explanation

Generative text-to-image models often do not fully capture a creator’s unique vision, and have insufficient knowledge about specific objects, brands or visual styles. With the FLUX Pro Finetuning API, creators can use existing images to finetune an AI to create similar images, along with a prompt.

When an image is created, we will register it as IP on Story in order to grow, monetize, and protect the IP.

## 0. Before you Start

There are a few steps you have to complete before you can start the tutorial.

1. You will need to install [Node.js](https://nodejs.org/en/download) and [npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm). If you've coded before, you likely have these.
2. Add your Story Network Testnet wallet's private key to `.env` file:

```yaml .env
WALLET_PRIVATE_KEY=
```

3. Go to [Pinata](https://pinata.cloud/) and create a new API key. Add the JWT to your `.env` file:

```yaml .env
PINATA_JWT=
```

4. Go to [BFL](https://api.us1.bfl.ai/auth/profile) and create a new API key. Add the new key to your `.env` file:

> 🚧 BFL Credits
>
> In order to generate an image, you'll need BFL credits. If you just created an account, you will need to purchase credits [here](https://api.us1.bfl.ai/auth/profile).
>
> You can also see the pricing for each of the API endpoints [here](https://docs.bfl.ml/pricing/).

```yaml .env
BFL_API_KEY=
```

5. Add your preferred Story RPC URL to your `.env` file. You can just use the public default one we provide:

```yaml .env
RPC_PROVIDER_URL=https://aeneid.storyrpc.io
```

6. Install the dependencies:

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

Let's make a function that calls FLUX's `/v1/finetune` API route. Create a `flux` folder, and inside that folder add a file named `requestFinetuning.ts` and add the following code:

> 📘 Official Docs
>
> In order to learn what each of the parameters in the payload are, see the official `/v1/finetune` API docs [here](https://api.us1.bfl.ai/scalar#tag/tasks/POST/v1/finetune).

```typescript flux/requestFinetuning.ts
import axios from "axios";
import fs from "fs";

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

export async function requestFinetuning(
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

Next, create a file named `train.ts` and call the `requestFinetuning` function we just made:

> 🚧 Warning: This is expensive!
>
> Creating a new finetune is expensive, ranging from $2-$6 at the time of me writing this tutorial. Please review the "FLUX PRO FINETUNE: TRAINING" section on the [pricing page](https://docs.bfl.ml/pricing/).

```typescript train.ts
import { requestFinetuning } from "./flux/requestFinetuning";

async function main() {
  const response = await requestFinetuning("./images.zip", "ippy-finetune");
  console.log(response);
}

main();
```

This will log something that looks like:

```json
{ 
  finetune_id: '6fc5e628-6f56-48ec-93cb-c6a6b22bf5a' 
}
```

This is your `finetune_id`, and will be used to create images in the following steps.

## 3. Wait for Finetune

Before we can generate images with our finetuned model, we have to wait for FLUX to finish training!

In our `flux` folder, create a file named `finetune-progress.ts` and add the following code:

> 📘 Official Docs
>
> In order to learn what each of the parameters in the payload are, see the official `/v1/get_result` API docs [here](https://api.us1.bfl.ai/scalar#tag/utility/GET/v1/get_result).

```typescript flux/finetuneProgress.ts
import axios from "axios";

export async function finetuneProgress(finetuneId: string) {
  const url = "https://api.us1.bfl.ai/v1/get_result";
  const headers = {
    "Content-Type": "application/json",
    "X-Key": process.env.BFL_API_KEY,
  };
  try {
    const response = await axios.get(url, {
      headers,
      params: { id: finetuneId },
    });
    return response.data;
  } catch (error) {
    throw new Error(`Finetune progress failed: ${error}`);
  }
}
```

Next, create a file named `finetune-progress.ts` and call the `finetuneProgress` function we just made:

```typescript finetune-progress.ts
import { finetuneProgress } from "./flux/finetuneProgress";

// input your finetune_id here
const FINETUNE_ID = '';

async function main() {
  const progress = await finetuneProgress(FINETUNE_ID);
  console.log(progress);
}

main();
```

This will log something that looks like:

```json
{
  id: '023a1507-369e-46e0-bd6d-1f3446d7d5f2',
  status: 'Pending',
  result: null,
  progress: null
}
```

As you can see, the status is still pending. We must wait until the training is 'Ready' before we can move on to the next step.

## 4. Run Inference

> 🚧 Warning: This costs money.
>
> Although very cheap, running an inference does cost money, ranging from $0.06-0.07 at the time of me writing this tutorial. Please review the "FLUX PRO FINETUNE: INFERENCE" section on the [pricing page](https://docs.bfl.ml/pricing/).

Now that we have trained a finetune, we will use the model to create images. "Running an inference" simply means using our new model (identified by its `finetune_id`), which is trained on our images, to create new images.

There are several different inference endpoints we can use, each with [their own pricing](https://docs.bfl.ml/pricing/) (found at the bottom of the page). For this tutorial, I'll be using the `/v1/flux-pro-1.1-ultra-finetuned` endpoint, which is documented [here](https://api.us1.bfl.ai/scalar#tag/tasks/POST/v1/flux-pro-1.1-ultra-finetuned).

In our `flux` folder, create a `finetuneInference.ts` file and add the following code:

> 📘 Official Docs
>
> In order to learn what each of the parameters in the payload are, see the official `/v1/flux-pro-1.1-ultra-finetuned` API docs [here](https://api.us1.bfl.ai/scalar#tag/tasks/POST/v1/flux-pro-1.1-ultra-finetuned).

```typescript flux/finetineInference.ts
import axios from "axios";

export async function finetuneInference(
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

Next, create a file named `inference.ts` and call the `finetuneInference` function we just made. The first parameter should be the `finetune_id` we got from running the script above, and the second parameter is a prompt to generate a new image.

```typescript inference.ts
import { finetuneInference } from './flux/finetuneInference'

// input your finetune_id here
const FINETUNE_ID = '';
// add your prompt here
const PROMPT = "A picture of Ippy being really happy."

async function main() {
  const inference = await finetuneInference(FINETUNE_ID, PROMPT);
  console.log(inference)
}

main();
```

This will log something that looks like:

```json
{
  id: '023a1507-369e-46e0-bd6d-1f3446d7d5f2',
  status: 'Pending',
  result: null,
  progress: null
}
```

As you can see, the status is still pending. We must wait until the generation is ready to view our image. To do this, we will need a function to fetch our new inference to see if its ready and view the details about it.

In our `flux` folder, create a file named `getInference.ts` and add the following code:

> 📘 Official Docs
>
> In order to learn what each of the parameters in the payload are, see the official `/v1/get_result` API docs [here](https://api.us1.bfl.ai/scalar#tag/utility/GET/v1/get_result).

```typescript flux/getInference.ts
import axios from "axios";

export async function getInference(id: string) {
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

Back in our `inference.ts` file, lets add a loop that continuously fetches the inference until it's ready. When it's ready, we will view the new image.

```typescript inference.ts
import { finetuneInference } from './flux/finetuneInference'
import { getInference } from './flux/getInference'

// input your finetune_id here
const FINETUNE_ID = '';
// add your prompt here
const PROMPT = "A picture of Ippy being really happy."

async function main() {
  const inference = await finetuneInference(FINETUNE_ID, PROMPT);
  
  let inferenceData = await getInference(inference.id);
  while (inferenceData.status != "Ready") {
    console.log("Waiting for inference to complete...");
    // wait 5 seconds
    await new Promise((resolve) => setTimeout(resolve, 5000));
    // fetch the inference again
    inferenceData = await getInference(inference.id);
  }
  // now the inference data is ready
  console.log(inferenceData);
}

main();
```

Once the loop completed, the final log will look like:

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

## 5. Set up your Story Config

Next we will register this image on Story as an [🧩 IP Asset](doc:ip-asset) in order to monetize and license the IP. Create a `story` folder and add a `utils.ts` file. In there, add the following code to set up your Story Config:

* Associated docs: [TypeScript SDK Setup](doc:typescript-sdk-setup)

```typescript story/utils.ts
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

## 6. Set up your IP Metadata

In your `story` folder, create a `registerIp.ts` file.

View the [IPA Metadata Standard](doc:ipa-metadata-standard) and construct the metadata for your IP as shown below:

```typescript story/registerIp.ts
import { client, account } from './utils'

export async function registerIp(inference) {
  const ipMetadata = {
    "title": "Happy Ippy",
    "description": "A finetuned image of Ippy being happy.",
    "createdAt": "1728401700",
    "image": inferenceData.result.sample,
    "imageHash": "0x...", // a hash of the image
    "mediaUrl": inferenceData.result.sample,
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
```

## 7. Set up your NFT Metadata

In the `registerIp.ts` file, configure your NFT Metadata, which follows the [ERC-721 Metadata Standard](https://eips.ethereum.org/EIPS/eip-721).

```typescript story/registerIp.ts
import { IpMetadata } from "@story-protocol/core-sdk";
import { client, account } from './utils'

export async function registerIp(inference) {
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
}
```

## 8. Upload your IP and NFT Metadata to IPFS

In a new `pinata` folder, create a `uploadToIpfs` file and create a function to upload your IP & NFT Metadata objects to IPFS:

```typescript pinata/uploadToIpfs.ts
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

```typescript story/registerIp.ts
import { IpMetadata } from "@story-protocol/core-sdk";
import { client, account } from './utils'
import { uploadJSONToIPFS } from "../pinata/uploadToIpfs";
import { createHash } from "crypto";

export async function registerIp(inference) {
  // previous code here...

  const ipIpfsHash = await uploadJSONToIPFS(ipMetadata);
  const ipHash = createHash("sha256")
    .update(JSON.stringify(ipMetadata))
    .digest("hex");
  const nftIpfsHash = await uploadJSONToIPFS(nftMetadata);
  const nftHash = createHash("sha256")
    .update(JSON.stringify(nftMetadata))
    .digest("hex");
}
```

## 9. Register the NFT as an IP Asset

In this step, we will use the [📦 SPG](doc:spg) to combine minting and registering our NFT into one transaction call.

First, in your `story` folder, create a separate `createSpgNftCollection.ts` file and add the following code:

> ❓ Why do we have to do this?
>
> In order to use the `mintAndRegisterIpAssetWithPilTerms` function below, we'll have to deploy an SPG NFT collection so that the SPG can do the minting for us.
>
> Instead of doing this, you could technically write your own contract that implements [ISPGNFT](https://github.com/storyprotocol/protocol-periphery-v1/blob/main/contracts/interfaces/ISPGNFT.sol). But an easy way to create a collection that implements `ISPGNFT` is just to call the `createCollection` function in the SPG contract using the SDK, as shown below.

```typescript story/createSpgNftCollection.ts
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

## 10. Finish our Register Function

Back in our `registerIp.ts` file, add the following code. It will mint an NFT, register it as an [🧩 IP Asset](doc:ip-asset), set [License Terms](doc:license-terms) on the IP, and then set both NFT & IP metadata.

* Associated Docs: [ipAsset.mintAndRegisterIp](https://docs.story.foundation/docs/sdk-ipasset#mintandregisterip)

```typescript story/registerIp.ts
import { IpMetadata } from "@story-protocol/core-sdk";
import { client, account } from './utils'
import { uploadJSONToIPFS } from "../pinata/uploadToIpfs";
import { createHash } from "crypto";
import { Address } from "viem";

export async function registerIp(inference) {
  // previous code here ...

  const response = await client.ipAsset.mintAndRegisterIp({
    spgNftContract: process.env.SPG_NFT_CONTRACT_ADDRESS as Address,
    allowDuplicates: true,
    ipMetadata: {
      ipMetadataURI: `https://ipfs.io/ipfs/${ipIpfsHash}`,
      ipMetadataHash: `0x${ipHash}`,
      nftMetadataURI: `https://ipfs.io/ipfs/${nftIpfsHash}`,
      nftMetadataHash: `0x${nftHash}`,
    },
    txOptions: { waitForTransaction: true },
  });

  console.log(`Root IPA created at transaction hash ${response.txHash}, IPA ID: ${response.ipId}`);
  console.log(`View on the explorer: https://aeneid.explorer.story.foundation/ipa/${response.ipId}`); 
}
```

## 11. Register our Inference

Now that we have completed our `registerIp` function, let's add it to our `inference.ts` file:

```typescript inference.ts
import { finetuneInference } from './flux/finetuneInference'
import { getInference } from './flux/getInference'
import { registerIp } from './story/registerIp'

const FINETUNE_ID = '';
const PROMPT = "A picture of Ippy being really happy."

async function main() {
  const inference = await finetuneInference(FINETUNE_ID, PROMPT);
  
  let inferenceData = await getInference(inference.id);
  while (inferenceData.status != "Ready") {
    console.log("Waiting for inference to complete...");
    await new Promise((resolve) => setTimeout(resolve, 5000));
    inferenceData = await getInference(inference.id);
  }
  // now the inference data is ready
  console.log(inferenceData);
  
  // add the function here
  await registerIp(inferenceData);
}

main();
```

## 12. Done!

<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/jacob-tucker/finetune-story-flux" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    View the completed code for this tutorial.
  </Card>
</Cards>