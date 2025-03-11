---
title: Email Login & Sponsored Transactions with Pivy
excerpt: >-
  Learn how to implement email logins and sponsored transactions with Privy &
  Pimlico.
deprecated: false
hidden: true
metadata:
  robots: index
---
[https://docs.dynamic.xyz/smart-wallets/add-smart-wallets](https://docs.dynamic.xyz/smart-wallets/add-smart-wallets)

### :warning: Prerequisites

There are a few steps you have to complete before you can start the tutorial.

1. Create a new project on <a href="https://dashboard.privy.io" target="_blank">Privy's Dashboard ↗️</a>
2. Copy your **"App ID"** under **"App settings > API keys"**. In your local project, make a `.env` file and add your App ID:

```Text .env
NEXT_PUBLIC_PRIVY_APP_ID=
```

3. On your project dashboard, enable Smart Wallets under "**Wallet Configuration > Smart wallets**" and select "**Kernel (ZeroDev)**" as shown below:

![](https://files.readme.io/4d62b6c1080f012ddb0899498bb6af24b834928ef4c5e97359ffb56223675658-image.png)

4. Once you enable Smart wallets, right underneath make sure to put a "Custom chain" with the following values:
   1. Name: `Story Aeneid Testnet`
   2. ID number: `1315`
   3. RPC URL: `https://aeneid.storyrpc.io`
   4. For the Bundler URL and Paymaster URL, go to <a href="https://dashboard.pimlico.io" target="_blank">Pimlico's Dashboard ↗️</a> and create a new app. Then click on "API Keys", create a new API Key, and click "RPC URLs" as shown below:

![](https://files.readme.io/eb5092fec55f86d23003b4cf44d1f07a028952c04196b47a9460ca30c4667567-image.png)

5. Install the dependencies:

```Text Terminal
npm install @story-protocol/core-sdk permissionless viem @privy-io/react-auth
```

## 1. Set up Privy

<Cards columns={1}>
  <Card title="Official Privy Tutoral" href="https://docs.privy.io/guide/react/wallets/smart-wallets/usage#setup" icon="fa-home" target="_blank">
    Follow Privy's official tutorial for setup instead of reading this step.
  </Card>
</Cards>

You must wrap any component that will be using embedded/smart wallets with the `PrivyProvider` and `SmartWalletsProvider`. In a `providers.tsx` (or whatever you want to call it) file, add the following code:

```jsx providers.tsx
"use client";

import { PrivyProvider } from "@privy-io/react-auth";
import { SmartWalletsProvider } from "@privy-io/react-auth/smart-wallets";
import { aeneid } from "@story-protocol/core-sdk";

export default function Providers({ children }: { children: React.ReactNode }) {
  return (
    <PrivyProvider
      appId={process.env.NEXT_PUBLIC_PRIVY_APP_ID as string}
      config={{
        // Customize Privy's appearance in your app
        appearance: {
          theme: "light",
          accentColor: "#676FFF",
          logo: "/story-logo.jpg",
        },
        // Create embedded wallets for users who don't have a wallet
        // when they sign in with email
        embeddedWallets: {
          createOnLogin: "all-users",
        },
        defaultChain: aeneid,
        supportedChains: [aeneid],
      }}
    >
      <SmartWalletsProvider>{children}</SmartWalletsProvider>
    </PrivyProvider>
  );
}
```

Then you can simply add it to your`layout.tsx` like so:

```jsx layout.tsx
import Providers from "@/providers/providers";

/* other code here... */

export default function RootLayout({
  children,
}: Readonly<{
  children: React.ReactNode;
}>) {
  return (
    <html lang="en">
      <body
        className={`${geistSans.variable} ${geistMono.variable} antialiased`}
      >
        <Providers>{children}</Providers>
      </body>
    </html>
  );
}

```

## 2. Login & Logout

You can add email login to your app like so:

```jsx page.tsx
import { usePrivy } from "@privy-io/react-auth";

export default function Home() {
  const { login, logout, user } = usePrivy();

  useEffect(() => {
    if (user) {
      const smartWallet = user.linkedAccounts.find((account) => account.type === 'smart_wallet');
      // Logs the smart wallet's address
      console.log(smartWallet.address);
      // Logs the smart wallet type (e.g. 'safe', 'kernel', 'light_account', 'biconomy', 'thirdweb', 'coinbase_smart_wallet')
      console.log(smartWallet.type);
    }
  }, [user])

  return (
    <div>
      <button onClick={user ? logout : login}>
        {user ? "Logout" : "Login with Privy"}
      </button>
    </div>
  )
}
```

## 3. Sign a Message with Privy

<Cards columns={1}>
  <Card title="Official Privy Tutoral" href="https://docs.privy.io/guide/react/wallets/smart-wallets/usage#signing-messages" icon="fa-home" target="_blank">
    Follow Privy's official tutorial for signing messages instead of reading this step.
  </Card>
</Cards>

We can use the generated smart wallet to sign messages:

```jsx page.tsx
import { useSmartWallets } from "@privy-io/react-auth/smart-wallets";

export default function Home() {
  const { client: smartWalletClient } = useSmartWallets();

  /* previous code here */

  async function sign() {
    const uiOptions = {
      title: "Example Sign",
      description: "This is an example for a user to sign.",
      buttonText: "Sign",
    };
    const request = {
      message: "IP is cool",
    };
    const signature = await smartWalletClient?.signMessage(request, {
      uiOptions
    });
  }

  return (
    <div>
      {/* previous code here */}
      <button onClick={sign}>Sign</button>
    </div>
  )
}
```

## 4. Send an Arbitrary Transaction

<Cards columns={1}>
  <Card title="Official Privy Tutoral" href="https://docs.privy.io/guide/react/wallets/smart-wallets/usage#sending-transactions" icon="fa-home" target="_blank">
    Follow Privy's official tutorial for sending transactions instead of reading this step.
  </Card>
</Cards>

We can also use the generated smart wallet to sponsor transactions for our users:

```jsx page.tsx
import { useSmartWallets } from "@privy-io/react-auth/smart-wallets";
import { encodeFunctionData } from "viem";
import { defaultNftContractAbi } from "./defaultNftContractAbi";

export default function Home() {
  const { client: smartWalletClient } = useSmartWallets();

  /* previous code here */

  async function mintNFT() {
    const uiOptions = {
      title: "Mint NFT",
      description: "This is an example transaction that mints an NFT.",
      buttonText: "Mint",
    };

    const transactionRequest = {
      to: "0x937bef10ba6fb941ed84b8d249abc76031429a9a", // example nft contract
      data: encodeFunctionData({
        abi: defaultNftContractAbi, // abi from another file
        functionName: "mintNFT",
        args: ["0x6B86B39F03558A8a4E9252d73F2bDeBfBedf5b68", "test-uri"],
      }),
    } as const;

    const txHash = await smartWalletClient?.sendTransaction(
      transactionRequest,
      { uiOptions }
    );
    console.log(`View Tx: https://aeneid.storyscan.xyz/tx/${txHash}`);
  }

  return (
    <div>
      {/* previous code here */}
      <button onClick={mintNFT}>Mint NFT</button>
    </div>
  )
}
```
```Text defaultNftContractAbi.ts
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

## 5. Send a Transaction from Story SDK

We can also use the generated smart wallet to send transactions from the [🛠️ TypeScript SDK](doc:typescript-sdk). Some of the functions have an option to return the `encodedTxData`, which we can use to pass into Privy's smart wallet. You can see which functions support this in the [SDK Reference](doc:sdk-overview).

```jsx page.tsx
import { useSmartWallets } from "@privy-io/react-auth/smart-wallets";
import {
  EncodedTxData,
  StoryClient,
  StoryConfig,
} from "@story-protocol/core-sdk";
import { http } from 'viem'

export default function Home() {
  const { client: smartWalletClient } = useSmartWallets();

  /* previous code here */

  async function setupStoryClient() {
    const config: StoryConfig = {
      account: smartWalletClient!.account,
      transport: http("https://aeneid.storyrpc.io"),
      chainId: "aeneid",
  	};
  	const client = StoryClient.newClient(config);
  	return client;
	}

  async function registerIp() {
    const storyClient = await setupStoryClient();

    const response = await storyClient.ipAsset.mintAndRegisterIp({
      spgNftContract: "0xc32A8a0FF3beDDDa58393d022aF433e78739FAbc", // public spg contract for testing
      txOptions: { encodedTxDataOnly: true },
    });

    const uiOptions = {
      title: "Register IP",
      description: "This is an example transaction that registers an IP.",
      buttonText: "Register",
    };

    const txHash = await smartWalletClient?.sendTransaction(
      response.encodedTxData as EncodedTxData,
      { uiOptions }
  	);
  	console.log(`View Tx: https://aeneid.storyscan.xyz/tx/${txHash}`);
  }

  return (
    <div>
      {/* previous code here */}
      <button onClick={registerIp}>Register IP</button>
    </div>
  )
}
```

## 5. Upload your IP and NFT Metadata to IPFS

In a separate file, create a function to upload your IP & NFT Metadata objects to IPFS:

```javascript utils/uploadToIpfs.ts
const pinataSDK = require("@pinata/sdk");

export async function uploadJSONToIPFS(jsonMetadata): Promise<string> {
  const pinata = new pinataSDK({ pinataJWTKey: process.env.PINATA_JWT });
  const { IpfsHash } = await pinata.pinJSONToIPFS(jsonMetadata);
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

## 6. Register the NFT as an IP Asset

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
  chainId: 'aeneid',
}
const client = StoryClient.newClient(config)

const newCollection = await client.nftClient.createNFTCollection({
  name: 'Dall-E NFTs',
  symbol: 'DALLE',
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
import {
  PIL_TYPE,
  CreateIpAssetWithPilTermsResponse,
} from "@story-protocol/core-sdk";
import { Address } from "viem";

// previous code here ...

const response: CreateIpAssetWithPilTermsResponse =
  await client.ipAsset.mintAndRegisterIpAssetWithPilTerms({
    spgNftContract: process.env.SPG_NFT_CONTRACT_ADDRESS as Address,
    pilType: PIL_TYPE.NON_COMMERCIAL_REMIX,
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
  `View on the explorer: https://aeneid.explorer.story.foundation/ipa/${response.ipId}`
);
```

## 7. Done!