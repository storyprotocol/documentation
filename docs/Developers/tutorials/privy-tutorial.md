---
title: Email Login & Sponsored Transactions with Privy
excerpt: >-
  Learn how to implement email logins and sponsored transactions with Privy &
  Pimlico.
deprecated: false
hidden: false
metadata:
  robots: index
---
<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/jacob-tucker/story-privy-tutorial" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    View the completed code for this tutorial.
  </Card>
</Cards>

You are reading this tutorial because you probably want to do one or both of these things:

1. Enable users who don't have a wallet to login with email to your app ("Embedded Wallets")
2. Sponsor transactions for your users so they don't have to pay gas ("Smart Wallets")

Here is how Privy describes both of these things:

> Embedded wallets are self-custodial wallets provisioned by Privy itself for a wallet experience that is directly embedded in your application. Embedded wallets do not require a separate wallet client, like a browser extension or a mobile app, and can be accessed directly from your product. These are primarily designed for users of your app who may not already have an external wallet, or don't want to connect their external wallet.
>
> Smart wallets are programmable, onchain accounts that incorporate the features of account abstraction. With just a few lines of code, you can create smart wallets for your users to sponsor gas payments, send batched transactions, and more.

We will be implementing both using [Privy](https://www.privy.io/) + [Pimlico](https://www.pimlico.io/).

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

## 1. Set up Embedded Wallets

<Cards columns={1}>
  <Card title="Official Privy Tutoral" href="https://docs.privy.io/guide/react/wallets/smart-wallets/usage#setup" icon="fa-home" target="_blank">
    Follow Privy's official tutorial for setup instead of reading this step.
  </Card>
</Cards>

> 📘 Learn more on Embedded Wallets
>
> You can read Privy's tutorial [here](https://docs.privy.io/guide/react/wallets/embedded/creation) that describes setting up Embedded Wallets, which is a fancy way of saying email login for your users. In the below example, we simply create an embedded wallet for every user, but you may want more customization by reading their tutorial.

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
      <body className={`${geistSans.variable} ${geistMono.variable} antialiased`}>
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
import { http } from "viem";

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
  );
}
```

## 6. Done!

<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/jacob-tucker/story-privy-tutorial" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    View the completed code for this tutorial.
  </Card>
</Cards>