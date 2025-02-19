---
title: Register an IP Asset
excerpt: Learn how to Register an NFT as an IP Asset in TypeScript.
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
  <Card title="Completed Code" href="https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/simpleMintAndRegisterSpg.ts" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    Follow the completed code all the way through.
  </Card>
</Cards>

Let's say you have some off-chain IP (ex. a book, a character, a drawing, etc). In order to register that IP on Story, you first need to mint an NFT. This NFT is the **ownership** over the IP. Then you **register** that NFT on Story, turning it into an [🧩 IP Asset](doc:ip-asset). The below tutorial will walk you through how to do this.

### :warning: Prerequisites

There are a few steps you have to complete before you can start the tutorial.

1. Complete the [TypeScript SDK Setup](doc:typescript-sdk-setup)
2. \[OPTIONAL] Go to [Pinata](https://pinata.cloud/) and create a new API key. Add the JWT to your `.env` file:

```yaml .env
PINATA_JWT=<YOUR_PINATA_JWT>
```

3. \[OPTIONAL] Install the `pinata-web3` dependency:

```Text Terminal
npm install pinata-web3
```

## 1. \[OPTIONAL] Set up your IP Metadata

We can set metadata on our NFT & IP, *but you don't have to*. To do this, view the [IPA Metadata Standard](doc:ipa-metadata-standard) and construct your metadata for both your NFT & IP.

Use the `generateIpMetadata` function to properly format your metadata and ensure it is of the correct type, as shown below:

```typescript main.ts
import { IpMetadata } from '@story-protocol/core-sdk'
// you should already have a client set up (prerequisite)
import { client } from './utils'

async function main() {
  const ipMetadata: IpMetadata = client.ipAsset.generateIpMetadata({
    title: 'My IP Asset',
    description: 'This is a test IP asset',
    creators: [{
      "name": "Story Foundation",
      "address": "0x67ee74EE04A0E6d14Ca6C27428B27F3EFd5CD084",
      "description": "The World's IP Blockchain",
      "contributionPercent": 100,
      "socialMedia": [
        {
          "platform": "Twitter",
          "url": "https://twitter.com/storyprotocol"
        },
        {
          "platform": "Website",
          "url": "https://story.foundation"
        },
      ]
    }]
  })
}

main();
```

## 2. \[OPTIONAL] Set up your NFT Metadata

The NFT Metadata follows the [ERC-721 Metadata Standard](https://eips.ethereum.org/EIPS/eip-721).

```typescript main.ts
import { IpMetadata } from '@story-protocol/core-sdk'
import { client } from './utils'

async function main() {
  // previous code here ...

  const nftMetadata = {
    name: 'Ownership NFT',
    description: 'This is an NFT representing owernship of our IP Asset.',
    image: 'https://picsum.photos/200',
  }
}

main();
```

## 3. \[OPTIONAL] Upload your IP and NFT Metadata to IPFS

In a separate `uploadToIpfs` file, create a function to upload your IP & NFT Metadata objects to IPFS:

```typescript uploadToIpfs.ts
import { PinataSDK } from "pinata-web3";

const pinata = new PinataSDK({
  pinataJwt: process.env.PINATA_JWT
});

export async function uploadJSONToIPFS(jsonMetadata: any): Promise<string> {
  const { IpfsHash } = await pinata.upload.json(jsonMetadata);
  return IpfsHash;
}
```

You can then use that function to upload your metadata, as shown below:

```typescript main.ts
import { IpMetadata } from '@story-protocol/core-sdk'
import { client } from './utils'
import { uploadJSONToIPFS } from './uploadToIpfs'
import { createHash } from 'crypto'

async function main() {
  // previous code here ...

  const ipIpfsHash = await uploadJSONToIPFS(ipMetadata)
  const ipHash = createHash('sha256').update(JSON.stringify(ipMetadata)).digest('hex')
  const nftIpfsHash = await uploadJSONToIPFS(nftMetadata)
  const nftHash = createHash('sha256').update(JSON.stringify(nftMetadata)).digest('hex')
}

main();
```

## 4. Register an NFT as an IP Asset

Remember that in order to register a new IP, we first have to mint an NFT, which will represent the underlying ownership of the IP. This NFT then gets "registered" and becomes an [🧩 IP Asset](doc:ip-asset).

For simplicity, we can use the SDK's `createNFTCollection` to create a new NFT collection for us. In a separate file `createSpgNftCollection.ts`, paste the following code (view a working example [here](https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/utils/createSpgNftCollection.ts)):

> ❓ Why do we have to do this?
>
> In order to use the `mintAndRegisterIp` function in the next step, we'll have to deploy an **SPG NFT** collection. **This is any contract that implements** [ISPGNFT](https://github.com/storyprotocol/protocol-periphery-v1/blob/main/contracts/interfaces/ISPGNFT.sol). An easy way to deploy a contract like this is to call the `createNFTCollection` as you'll see below.
>
> :warning: You only have to create a new SPG Collection **once**. Once you have your SPG NFT contract address, you can register any amount of IPs and will **not** have to do this again.
>
> Instead of doing this, you can mint an NFT from your own contract and use the [register](https://docs.story.foundation/docs/sdk-ipasset#register) function (providing an `nftContract` and `tokenId`) *instead of* using the `mintAndRegisterIp` function. See a working code example [here](https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/simpleMintAndRegister.ts).

```typescript createSpgNftCollection.ts
import { zeroAddress } from 'viem'
import { client } from './utils'

async function createSpgNftCollection() {
  const newCollection = await client.nftClient.createNFTCollection({
    name: 'Test NFTs',
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

If you run this code and look at the console output, you will find the SPG NFT contract address. Save this for later.

Now we can actually register our IP by minting an NFT, registering it as an [🧩 IP Asset](doc:ip-asset), and setting both NFT & IP metadata.

> Associated Docs: [ipAsset.mintAndRegisterIp](https://docs.story.foundation/docs/sdk-ipasset#mintandregisterip)

```typescript main.ts
import { IpMetadata } from '@story-protocol/core-sdk'
import { client } from './utils'
import { uploadJSONToIPFS } from './uploadToIpfs'
import { createHash } from 'crypto'
import { Address } from 'viem'

async function main() {
  // previous code here ...

  const response = await client.ipAsset.mintAndRegisterIp({
    // TODO: insert your SPG_NFT_CONTRACT_ADDRESS here
    spgNftContract: SPG_NFT_CONTRACT_ADDRESS as Address,
    allowDuplicates: true,
    ipMetadata: {
      ipMetadataURI: `https://ipfs.io/ipfs/${ipIpfsHash}`,
      ipMetadataHash: `0x${ipHash}`,
      nftMetadataURI: `https://ipfs.io/ipfs/${nftIpfsHash}`,
      nftMetadataHash: `0x${nftHash}`,
    },
    txOptions: { waitForTransaction: true },
  })
  
  console.log(`Root IPA created at transaction hash ${response.txHash}, IPA ID: ${response.ipId}`)
  console.log(`View on the explorer: https://aeneid.explorer.story.foundation/ipa/${response.ipId}`)
}

main();
```

## 5. :checkered_flag: View Completed Code

Congratulations, you registered an IP!

<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/simpleMintAndRegisterSpg.ts" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    Follow the completed code all the way through.
  </Card>
</Cards>

## 6. Add License Terms to IP

Now that your IP is registered, you can create and attach [License Terms](doc:license-terms) to it. This will allow others to mint a license and use your IP, restricted by the terms.

:point_right: We will go over this in the next section, but it's worth mentioning that instead of using the `mintAndRegisterIp` function, you can **register IP + create terms + attach terms** all in the same step with the following functions:

* [mintAndRegisterIpAssetWithPilTerms](https://docs.story.foundation/docs/sdk-ipasset#mintandregisteripassetwithpilterms)
* [registerIpAndAttachPilTerms](https://docs.story.foundation/docs/sdk-ipasset#registeripandattachpilterms)