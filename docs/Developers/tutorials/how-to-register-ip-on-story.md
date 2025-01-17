---
title: How to Register IP on Story
excerpt: Learn how to add IP & NFT metadata to an IP Asset using the Typescript SDK.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
* [Use the SDK](https://docs.story.foundation/docs/how-to-register-ip-on-story#using-the-sdk)
* [Use a Smart Contract](https://docs.story.foundation/docs/how-to-register-ip-on-story#using-a-smart-contract)

# Using the SDK

<Cards columns={3}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/simpleMintAndRegister.ts" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    Follow the completed code all the way through step 5a.
  </Card>

  <Card title="Completed Code" href="https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/simpleMintAndRegisterSpg.ts" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    Follow the completed code all the way through step 5b.
  </Card>

  <Card title="Video Walkthrough" href="https://www.youtube.com/watch?v=r3iIChDhp1g" icon="fa-video" iconColor="#FF0000" target="_blank">
    Check out a video walkthrough of this tutorial!
  </Card>
</Cards>

In this tutorial, you will learn how to register your IP on Story using the TypeScript SDK.

## The Explanation

Let's say you have some off-chain IP (ex. a book, a character, a drawing, etc). In order to register that IP on Story, you first need to mint an NFT to represent that IP, and then register that NFT on Story, turning it into an [🧩 IP Asset](doc:ip-asset). The below tutorial will walk you through 2 different ways to do that (step 5a and step 5b).

## 0. Before you Start

There are a few steps you have to complete before you can start the tutorial.

1. Add your Story Network Testnet wallet's private key to `.env` file:

```yaml .env
WALLET_PRIVATE_KEY=<YOUR_WALLET_PRIVATE_KEY>
```

2. Go to [Pinata](https://pinata.cloud/) and create a new API key. Add the JWT to your `.env` file:

```yaml .env
PINATA_JWT=<YOUR_PINATA_JWT>
```

3. Add your preferred RPC URL to your `.env` file. You can just use the public default one we provide:

```yaml .env
RPC_PROVIDER_URL=https://rpc.odyssey.storyrpc.io
```

4. Install the dependencies:

```Text Terminal
npm install @story-protocol/core-sdk pinata-web3 viem
```

## 1. Set up your Story Config

In a `utils.ts` file, add the following code to set up your Story Config:

* Associated docs: [TypeScript SDK Setup](doc:typescript-sdk-setup)

```typescript utils.ts
import { StoryClient, StoryConfig } from '@story-protocol/core-sdk'
import { http } from 'viem'
import { privateKeyToAccount, Address, Account } from 'viem/accounts'

const privateKey: Address = `0x${process.env.WALLET_PRIVATE_KEY}`
export const account: Account = privateKeyToAccount(privateKey)

const config: StoryConfig = {  
  account: account,  
  transport: http(process.env.RPC_PROVIDER_URL),  
  chainId: 'odyssey',  
}  
export const client = StoryClient.newClient(config)
```

## 2. Set up your IP Metadata

View the [IPA Metadata Standard](doc:ipa-metadata-standard) and construct your metadata for your IP. Y

In a `main.ts` file, use the `generateIpMetadata` function to properly format your metadata and ensure it is of the correct type, as shown below:

```typescript main.ts
import { IpMetadata } from '@story-protocol/core-sdk'
import { client } from './utils'

async function main() {
  const ipMetadata: IpMetadata = client.ipAsset.generateIpMetadata({
    title: 'My IP Asset',
    description: 'This is a test IP asset',
    watermarkImg: 'https://picsum.photos/200',
    attributes: [
      {
        key: 'Rarity',
        value: 'Legendary',
      },
    ],
  })
}

main();
```

## 3. Set up your NFT Metadata

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

## 4. Upload your IP and NFT Metadata to IPFS

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

## 5. Register the NFT as an IP Asset

There are two ways to do this step. Either:

1. [5a. Mint an NFT and register it separately](https://docs.story.foundation/docs/how-to-register-ip-on-story#5a-mint-an-nft-and-register-it-separately)
2. [5b. Mint an NFT + register in the same transaction](https://docs.story.foundation/docs/how-to-register-ip-on-story#5b-mint-an-nft--register-in-the-same-transaction)

### 5a. Mint an NFT and register it separately

In this step, we will first mint an NFT and then register it as a IP Asset.

**You can mint an NFT on Story however you want, as long as it is an ERC-721 and you have the NFT contract address and token ID**. Below, you will use an example NFT contract address we've created to do the minting.

In your `.env` file, add the following NFT contract address:

```yaml .env
NFT_CONTRACT_ADDRESS=0x041B4F29183317Fd352AE57e331154b73F8a1D73
```

Then, in two separate files, add the following `mintNFT` function and the `defaultNftContractAbi`:

```typescript mintNFT.ts
import { http, createWalletClient, createPublicClient, Address } from 'viem'
import { privateKeyToAccount } from 'viem/accounts'
import { odyssey } from '@story-protocol/core-sdk'
import { defaultNftContractAbi } from './defaultNftContractAbi'
import { account } from './utils'

const baseConfig = {
  chain: odyssey,
  transport: http(process.env.RPCProviderUrl),
} as const
export const publicClient = createPublicClient(baseConfig)
export const walletClient = createWalletClient({
  ...baseConfig,
  account,
})

export async function mintNFT(to: Address, uri: string): Promise<number | undefined> {
  const { request } = await publicClient.simulateContract({
    address: process.env.NFT_CONTRACT_ADDRESS as Address,
    functionName: 'mintNFT',
    args: [to, uri],
    abi: defaultNftContractAbi,
  })
  const hash = await walletClient.writeContract(request)
  const { logs } = await publicClient.waitForTransactionReceipt({
    hash,
  })
  if (logs[0].topics[3]) {
    return parseInt(logs[0].topics[3], 16)
  }
}
```
```typescript defaultNftContractAbi.ts
export const defaultNftContractAbi = [
  {
    inputs: [],
    stateMutability: "nonpayable",
    type: "constructor",
  },
  {
    inputs: [
      {
        internalType: "address",
        name: "recipient",
        type: "address",
      },
      {
        internalType: "string",
        name: "tokenURI",
        type: "string",
      },
    ],
    name: "mintNFT",
    outputs: [
      {
        internalType: "uint256",
        name: "",
        type: "uint256",
      },
    ],
    stateMutability: "nonpayable",
    type: "function",
  },
  {
    inputs: [
      {
        internalType: "uint256",
        name: "tokenId",
        type: "uint256",
      },
    ],
    name: "tokenURI",
    outputs: [
      {
        internalType: "string",
        name: "",
        type: "string",
      },
    ],
    stateMutability: "view",
    type: "function",
  },
  {
    inputs: [
      {
        internalType: "uint256",
        name: "tokenId",
        type: "uint256",
      },
    ],
    name: "ownerOf",
    outputs: [
      {
        internalType: "address",
        name: "",
        type: "address",
      },
    ],
    stateMutability: "view",
    type: "function",
  },
  {
    inputs: [],
    name: "symbol",
    outputs: [
      {
        internalType: "string",
        name: "",
        type: "string",
      },
    ],
    stateMutability: "view",
    type: "function",
  },
  {
    inputs: [],
    name: "name",
    outputs: [
      {
        internalType: "string",
        name: "",
        type: "string",
      },
    ],
    stateMutability: "view",
    type: "function",
  },
  {
    inputs: [],
    name: "totalSupply",
    outputs: [
      {
        internalType: "uint256",
        name: "",
        type: "uint256",
      },
    ],
    stateMutability: "view",
    type: "function",
  },
];
```

Now we can call that `mintNFT` function to get a `tokenId`, and then register the NFT as an [🧩 IP Asset](doc:ip-asset), set [License Terms](doc:license-terms) on the IP, and then set both NFT & IP metadata.

* Associated Docs: [ Register New IP Asset and Attach License Terms](https://docs.story.foundation/docs/attach-terms-to-an-ip-asset#register-new-ip-asset-and-attach-license-terms)

```typescript main.ts
import { IpMetadata } from '@story-protocol/core-sdk'
import { client, account } from './utils'
import { uploadJSONToIPFS } from './uploadToIpfs'
import { createHash } from 'crypto'
import { Address } from 'viem'
import { mintNFT } from './mintNFT'

async function main() {
  // previous code here ...

  const tokenId = await mintNFT(account.address, `https://ipfs.io/ipfs/${nftIpfsHash}`)
  const response = await client.ipAsset.registerIpAndAttachPilTerms({
    nftContract: process.env.NFT_CONTRACT_ADDRESS as Address,
    tokenId: tokenId!,
    terms: [], // IP already has non-commercial social remixing terms. You can add more here.
    ipMetadata: {
      ipMetadataURI: `https://ipfs.io/ipfs/${ipIpfsHash}`,
      ipMetadataHash: `0x${ipHash}`,
      nftMetadataURI: `https://ipfs.io/ipfs/${nftIpfsHash}`,
      nftMetadataHash: `0x${nftHash}`,
    },
    txOptions: { waitForTransaction: true },
  })

  console.log(`Root IPA created at transaction hash ${response.txHash}, IPA ID: ${response.ipId}`)
  console.log(`View on the explorer: https://explorer.story.foundation/ipa/${response.ipId}`) 
}
```

### 5b. Mint an NFT + register in the same transaction

In this step, instead of minting an NFT and then registering it separately (2 different transactions), we will use the [📦 SPG](doc:spg) to combine them into one transaction call for convenience.

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

Look at the console output, and copy the SPG NFT contract address. Add that value as `SPG_NFT_CONTRACT_ADDRESS` to your `.env` file:

```Text env
SPG_NFT_CONTRACT_ADDRESS=<SPG_NFT_CONTRACT_ADDRESS>
```

> 📘 Note
>
> You only have to do the above step **once**. Once you have your SPG NFT contract address, you can register any amount of IPs and will **not** have to do this again.

The code below will mint an NFT, register it as an [🧩 IP Asset](doc:ip-asset), set [License Terms](doc:license-terms) on the IP, and then set both NFT & IP metadata.

* Associated Docs: [Mint, Register, and Attach Terms](https://docs.story.foundation/docs/attach-terms-to-an-ip-asset#mint-nft-register-as-ip-asset-and-attach-terms)

```typescript main.ts
import { IpMetadata } from '@story-protocol/core-sdk'
import { client } from './utils'
import { uploadJSONToIPFS } from './uploadToIpfs'
import { createHash } from 'crypto'
import { Address } from 'viem'

async function main() {
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
  })
  
  console.log(`Root IPA created at transaction hash ${response.txHash}, IPA ID: ${response.ipId}`)
  console.log(`View on the explorer: https://explorer.story.foundation/ipa/${response.ipId}`)
}

main();
```

## 6. Done!

<Cards columns={3}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/simpleMintAndRegister.ts" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    Follow the completed code all the way through step 5a.
  </Card>

  <Card title="Completed Code" href="https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/simpleMintAndRegisterSpg.ts" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    Follow the completed code all the way through step 5b.
  </Card>

  <Card title="Video Walkthrough" href="https://www.youtube.com/watch?v=zGQPiszTs40" icon="fa-video" iconColor="#FF0000" target="_blank">
    Check out a video walkthrough of this tutorial!
  </Card>
</Cards>

# Using a Smart Contract

<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/story-protocol-boilerplate/blob/main/test/0_IPARegistrar.t.sol" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    See the completed, working code of how to register your IP Asset.
  </Card>
</Cards>

Let's say you have some off-chain IP (ex. a book, a character, a drawing, etc). In order to register that IP on Story, you first need to mint an NFT to represent that IP, and then register that NFT on Story, turning it into an [🧩 IP Asset](doc:ip-asset). As you can probably tell, this is two transactions: Mint NFT ▶️ Register NFT

So, we will separate this tutorial into two sections:

1. [You already have an NFT and want to register it as IP](https://docs.story.foundation/docs/how-to-register-ip-on-story#you-already-have-an-nft-and-want-to-register-it-as-ip)
2. [You want to mint + register an NFT as IP in the same transaction](https://docs.story.foundation/docs/how-to-register-ip-on-story#you-want-to-mint--register-an-nft-as-ip-in-the-same-transaction)

## You already have an NFT and want to register it as IP

If you already have an NFT minted, or you want to register IP using a custom-built ERC-721 contract, this is the section for you.

As you can see below, the registration process is relatively straightforward. We use `SimpleNFT` as an example, but you can replace it with your own ERC-721 contract.

All you have to do is call `register` on the [IP Asset Registry](doc:ip-asset-registry) with:

* `chainid` - you can simply use `block.chainid`
* `tokenContract` - the address of your NFT collection
* `tokenId` - your NFT's ID

```sol IPARegistrar.sol
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.23;

import { IPAssetRegistry } from "@storyprotocol/core/registries/IPAssetRegistry.sol";
// your own ERC-721 NFT contract
import { SimpleNFT } from "./SimpleNFT.sol";

/// @notice Register an NFT as an IP Account.
contract IPARegistrar {
    IPAssetRegistry public immutable IP_ASSET_REGISTRY;
    SimpleNFT public immutable SIMPLE_NFT;

  	// you can get the addresses for these 
  	// here: https://docs.story.foundation/docs/deployed-smart-contracts
    constructor(address ipAssetRegistry) {
        IP_ASSET_REGISTRY = IPAssetRegistry(ipAssetRegistry);
        // Create a new Simple NFT collection
        SIMPLE_NFT = new SimpleNFT("Simple IP NFT", "SIM");
    }

    /// @notice Mint an IP NFT and register it as an IP Account via Story Protocol core.
    /// @return ipId The address of the IP Account.
    /// @return tokenId The token ID of the IP NFT.
    function mintIp() external returns (address ipId, uint256 tokenId) {
        tokenId = SIMPLE_NFT.mint(msg.sender);
        ipId = IP_ASSET_REGISTRY.register(block.chainid, address(SIMPLE_NFT), tokenId);
    }
}
```

## You want to mint + register an NFT as IP in the same transaction

If you don't have your own NFT contract and want to mint + register in the same step, this is the section for you.

To achieve this, we will be using the [📦 SPG](doc:spg), which is a utility contract that allows us to combine multiple transactions into one. In this case, we'll be using the SPG's `mintAndRegisterIp` function which combines both minting an NFT and registering it as an IP Asset.

In order to use `mintAndRegisterIp`, we first have to create a new `SPGNFT` collection. We can do this simply by calling `createCollection` on the `StoryProtocolGateway` contract. Or, if you want to create your own `SPGNFT` for some reason, you can implement the [ISPGNFT](https://github.com/storyprotocol/protocol-periphery-v1/blob/main/contracts/interfaces/ISPGNFT.sol) contract interface. Follow the example below to see example parameters you can use to initialize a new SPGNFT.

Once you have your own SPGNFT, all you have to do is call `mintAndRegisterIp` with:

* `spgNftContract` - the address of your SPGNFT contract
* `recipient` - the address of who will receive the NFT and thus be the owner of the newly registered IP. *Note: remember that registering IP on Story is permissionless, so you can register an IP for someone else (by paying for the transaction) yet they can still be the owner of that IP Asset.*
* `ipMetadata` - the metadata associated with your NFT & IP. See [this](https://docs.story.foundation/docs/ip-asset#nft-vs-ip-metadata) section to better understand setting NFT & IP metadata.

```sol IPARegistrar.sol
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.23;

import { StoryProtocolGateway } from "@storyprotocol/periphery/StoryProtocolGateway.sol";
import { IStoryProtocolGateway as ISPG } from "@storyprotocol/periphery/interfaces/IStoryProtocolGateway.sol";
import { ISPGNFT } from "@storyprotocol/periphery/interfaces/ISPGNFT.sol";

/// @notice Register an NFT as an IP Account.
contract IPARegistrar {
    StoryProtocolGateway public immutable SPG;
    ISPGNFT public immutable SPG_NFT;

    // you can get the addresses for these 
    // here: https://docs.story.foundation/docs/deployed-smart-contracts
    constructor(address storyProtocolGateway) {
        SPG = StoryProtocolGateway(storyProtocolGateway);
        // Create a new NFT collection via SPG
        SPG_NFT = ISPGNFT(
            SPG.createCollection(
                ISPGNFT.InitParams({
                  name: "Test Collection",
                  symbol: "TEST",
                  baseURI: "https://test-base-uri.com/",
                  maxSupply: 1000,
                  mintFee: 0,
                  mintFeeToken: address(0),
                  mintFeeRecipient: address(this),
                  owner: address(this),
                  mintOpen: true,
                  isPublicMinting: false
                })
            )
        );
    }

  /// @notice Mint an IP NFT and register it as an IP Account via Story Protocol Gateway (periphery).
  /// @dev Requires the collection to be created via SPG (createCollection).
  function spgMintIp() external returns (address ipId, uint256 tokenId) {
        (ipId, tokenId) = SPG.mintAndRegisterIp(
            address(SPG_NFT),
            msg.sender,
            ISPG.IPMetadata({
                ipMetadataURI: "https://ipfs.io/ipfs/QmZHfQdFA2cb3ASdmeGS5K6rZjz65osUddYMURDx21bT73",
                ipMetadataHash: keccak256(
                    abi.encodePacked(
                        "{'title':'My IP Asset','description':'This is a test IP asset','ipType':'','relationships':[],'createdAt':'','watermarkImg':'https://picsum.photos/200','creators':[],'media':[],'attributes':[{'key':'Rarity','value':'Legendary'}],'tags':[]}"
                    )
                ),
                nftMetadataURI: "https://ipfs.io/ipfs/QmRL5PcK66J1mbtTZSw1nwVqrGxt98onStx6LgeHTDbEey",
                nftMetadataHash: keccak256(
                    abi.encodePacked(
                        "{'name':'Test NFT','description':'This is a test NFT','image':'https://picsum.photos/200'}"
                    )
                )
            })
        );
    }
}
```