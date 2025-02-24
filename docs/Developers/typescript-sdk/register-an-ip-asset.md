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

We can set metadata on our NFT & IP, *but you don't have to*. To do this, view the [📝 IPA Metadata Standard](doc:ipa-metadata-standard) and construct your metadata for both your NFT & IP.

```typescript main.ts
// you should already have a client set up (prerequisite)
import { client } from './utils'

async function main() {
  const ipMetadata = {
    "title": "Ippy",
    "description": "Official mascot of Story.",
    "image": "https://ipfs.io/ipfs/QmSamy4zqP91X42k6wS7kLJQVzuYJuW2EN94couPaq82A8",
    "imageHash": "0x21937ba9d821cb0306c7f1a1a2cc5a257509f228ea6abccc9af1a67dd754af6e",
    "mediaUrl": "https://ipfs.io/ipfs/QmSamy4zqP91X42k6wS7kLJQVzuYJuW2EN94couPaq82A8",
    "mediaHash": "0x21937ba9d821cb0306c7f1a1a2cc5a257509f228ea6abccc9af1a67dd754af6e",
    "mediaType": "image/png",
    "creators": [
      {
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
          }
        ]
      }
    ]
  }
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

We can use the `mintAndRegisterIp` function to mint an NFT and register it as an IP Asset in the same transaction.

This function needs an SPG NFT Contract to mint from. For simplicity, you can use a public collection we have created for you on Aeneid testnet: `0xc32A8a0FF3beDDDa58393d022aF433e78739FAbc`.

<Accordion title="Creating your own custom ERC-721 collection" icon="fa-info-circle">
  Using the public collection we provide for you is fine, but when you do this for real, you should make your own NFT Collection for your IPs. You can do this in 2 ways:

  1. Deploy a contract that implements the [ISPGNFT](https://github.com/storyprotocol/protocol-periphery-v1/blob/main/contracts/interfaces/ISPGNFT.sol) interface, or use the SDK's `createNFTCollection` function (shown below) to do it for you. This will give you your own SPG NFT Collection that only you can mint from.

  ```typescript createSpgNftCollection.ts
  import { zeroAddress } from 'viem'
  import { client } from './utils'

  async function createSpgNftCollection() {
  const newCollection = await client.nftClient.createNFTCollection({
    name: 'Test NFTs',
    symbol: 'TEST',
    isPublicMinting: false,
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

  2. Create a custom ERC-721 NFT collection on your own and use the [register](https://docs.story.foundation/docs/sdk-ipasset#register) function - and providing an `nftContract` and `tokenId` - *instead of* using the `mintAndRegisterIp` function. See a working code example [here](https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/simpleMintAndRegister.ts). This is helpful if you already have a custom NFT contract that has your own custom logic, or if your IPs themselves are NFTs.
</Accordion>

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
    spgNftContract: '0xc32A8a0FF3beDDDa58393d022aF433e78739FAbc',
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