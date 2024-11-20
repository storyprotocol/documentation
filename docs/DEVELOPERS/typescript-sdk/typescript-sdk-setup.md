---
title: TypeScript SDK Setup
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Prerequisite

For running the tutorial for developers, we require node version 18 or later version and npm version 8 to be installed in your environment. To install node and npm, we recommend you go to the [Node.js official website](https://nodejs.org) and download the latest LTS (Long Term Support) version.

# Install the Dependencies

In the current folder `story-ts-example`, install the Story Protocol SDK node package, as well as `viem` (<https://www.npmjs.com/package/viem>) to access the DeFi wallet accounts.

```shell npm
npm install --save @story-protocol/core-sdk viem
```
```shell pnpm
pnpm install @story-protocol/core-sdk viem
```
```shell yarn
yarn add @story-protocol/core-sdk viem
```

# Initiate SDK Client

Next we can initiate the SDK Client. There are two ways to do this:

1. Using a private key (preferable for some backend admin)
2. JSON-RPC account like Metamask where users sign their own transactions

## Set Up Private Key Account

> ✅ See a Working Example
> 
> Check out the [TypeScript Tutorial](https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/simpleMintAndRegister.ts) for a working example of how to set up the Story SDK Client.

> :information_source: Make sure to have WALLET_PRIVATE_KEY set up in your .env file.
>
> :information_source: Make sure to have RPC_PROVIDER_URL for your desired chain set up in your .env file. You can use the public default one (`RPC_PROVIDER_URL=https://rpc.odyssey.storyrpc.io`) or check out the other RPCs [here](https://docs.story.foundation/docs/story-network#explorers).

```typescript index.ts
import { http } from 'viem';
import { Account, privateKeyToAccount, Address } from 'viem/accounts';
import { StoryClient, StoryConfig } from "@story-protocol/core-sdk";

const privateKey: Address = `0x${process.env.WALLET_PRIVATE_KEY}`;
const account: Account = privateKeyToAccount(privateKey);

const config: StoryConfig = {
  account: account, // the account object from above
  transport: http(process.env.RPC_PROVIDER_URL),
  chainId: 'odyssey'
};
export const client = StoryClient.newClient(config);
```

## Set Up JSON-RPC Account (ex. Metamask)

> ✅ See a Working Example
> 
> Check out the [Developer Sandbox](https://github.com/jacob-tucker/story-developer-sandbox/blob/main/lib/context/AppContext.tsx) for a working example of how to set up the Story SDK Client in a frontend setting.

We can also use the TypeScript SDK to delay signing & sending transactions to a JSON-RPC account like Metamask.

We recommend first [setting up wagmi](https://wagmi.sh/react/getting-started) in your application as a Web3 provider. Next, you must install a wallet service like [RainbowKit](https://www.rainbowkit.com/) (you can also use Dynamic, WalletConnect, or any other) so that users can log in to their preferred wallet. Ultimately you will end up with something that looks like this:

> :information_source: Make sure to have `RPC_PROVIDER_URL` for your desired chain set up in your .env file. You can use the public default one (`RPC_PROVIDER_URL=https://rpc.odyssey.storyrpc.io`) or check out the other RPCs [here](https://docs.story.foundation/docs/story-network#explorers).
>
> :information_source: Make sure to have `NEXT_PUBLIC_WALLET_CONNECT_PROJECT_ID` set up in your .env file. Do this by logging into [Reown (prev. WalletConnect)](https://reown.com/) and creating a project.

```jsx Web3Providers.tsx
"use client";
import "@rainbow-me/rainbowkit/styles.css";
import { getDefaultConfig, RainbowKitProvider } from "@rainbow-me/rainbowkit";
import { WagmiProvider } from "wagmi";
import { QueryClientProvider, QueryClient } from "@tanstack/react-query";
import { PropsWithChildren } from "react";
import { odyssey } from "@story-protocol/core-sdk";

const config = getDefaultConfig({
  appName: "Test Story App",
  projectId: process.env.NEXT_PUBLIC_WALLET_CONNECT_PROJECT_ID as string,
  chains: [odyssey],
  ssr: true, // If your dApp uses server side rendering (SSR)
});

const queryClient = new QueryClient();

export default function Web3Providers({ children }: PropsWithChildren) {
  return (
    <WagmiProvider config={config}>
      <QueryClientProvider client={queryClient}>
        <RainbowKitProvider>{children}</RainbowKitProvider>
      </QueryClientProvider>
    </WagmiProvider>
  );
}
```
```jsx layout.tsx
import Web3Providers from "./Web3Providers";

export default function RootLayout({
  children
}) {
  return (
    <html lang="en">
      <body>
        <Web3Providers>{children}</Web3Providers>
      </body>
    </html>
  );
}
```
```jsx TestComponent.tsx
import { custom } from 'viem';
import { StoryClient, StoryConfig } from "@story-protocol/core-sdk";
import { useWalletClient } from "wagmi";

export default function TestComponent() {
  const { data: wallet } = useWalletClient();
  
  function setupStoryClient(): StoryClient | null {
    if (!wallet) return null;
    
    const config: StoryConfig = {
      wallet: wallet,
      transport: custom(wallet.transport),
      chainId: "odyssey"
    };
    const client = StoryClient.newClient(config); 
    return client;
  }
  
  return (
    {/* */} 
  )
}
```