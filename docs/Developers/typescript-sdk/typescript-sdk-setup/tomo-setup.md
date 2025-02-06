---
title: Tomo Setup
excerpt: Learn how to setup TomoEVMKit in your Story DApp.
deprecated: false
hidden: false
metadata:
  robots: index
---
> 📘 Optional: Official TomoEVMKit Docs
>
> Check out the official Wagmi + TomoEVMKit installation docs [here](https://docs.tomo.inc/tomo-sdk/tomoevmkit/quick-start).

## Install the Dependencies

```shell npm
npm install --save @story-protocol/core-sdk @tomo-inc/tomo-evm-kit wagmi viem @tanstack/react-query
```
```shell pnpm
pnpm install @story-protocol/core-sdk viem
```
```shell yarn
yarn add @story-protocol/core-sdk viem
```

## Setup

Before diving into the example, make sure you have two things setup:

1. Make sure to have `NEXT_PUBLIC_RPC_PROVIDER_URL` set up in your `.env` file.
   1. You can use the public default one (`https://aeneid.storyrpc.io`) or any other RPC [here](https://docs.story.foundation/docs/story-network#-rpcs).
2. Make sure to have `NEXT_PUBLIC_TOMO_CLIENT_ID` set up in your `.env` file. Do this by logging into the [Tomo Dashboard](https://dashboard.tomo.inc/) and creating a project.
3. Make sure to have `NEXT_PUBLIC_WALLET_CONNECT_PROJECT_ID` set up in your `.env` file. Do this by logging into [Reown (prev. WalletConnect)](https://reown.com/) and creating a project.

```jsx Web3Providers.tsx
"use client";
import '@tomo-inc/tomo-evm-kit/styles.css';
import { getDefaultConfig, TomoEVMKitProvider } from "@tomo-inc/tomo-evm-kit";
import { WagmiProvider } from "wagmi";
import { QueryClientProvider, QueryClient } from "@tanstack/react-query";
import { PropsWithChildren } from "react";
import { aeneid } from "@story-protocol/core-sdk";

const config = getDefaultConfig({
  appName: "Test Story App",
  clientId: process.env.NEXT_PUBLIC_TOMO_CLIENT_ID as string,
  projectId: process.env.NEXT_PUBLIC_WALLET_CONNECT_PROJECT_ID as string,
  chains: [aeneid],
  ssr: true, // If your dApp uses server side rendering (SSR)
});

const queryClient = new QueryClient();

export default function Web3Providers({ children }: PropsWithChildren) {
  return (
    <WagmiProvider config={config}>
      <QueryClientProvider client={queryClient}>
        <TomoEVMKitProvider>
          {children}
        </TomoEVMKitProvider>
      </QueryClientProvider>
    </WagmiProvider>
  );
}
```
```jsx layout.tsx
import type { Metadata } from "next";
import { Inter } from "next/font/google";
import "./globals.css";
import { PropsWithChildren } from "react";
import Web3Providers from "./Web3Providers";
import { useConnectModal } from '@tomo-inc/tomo-evm-kit';

const inter = Inter({ subsets: ["latin"] });

export const metadata: Metadata = {
  title: "Example",
  description: "This is an Example DApp",
};

export default function RootLayout({
  children
}: PropsWithChildren) {
  const { openConnectModal } = useConnectModal();
  
  return (
    <html lang="en">
      <body>
        <Web3Providers>
          <button onClick={openConnectModal}>
            Connect Wallet
          </button>
          {children}
        </Web3Providers>
      </body>
    </html>
  );
}
```
```jsx TestComponent.tsx
import { custom, toHex } from 'viem';
import { useWalletClient } from "wagmi";
import { StoryClient, StoryConfig } from "@story-protocol/core-sdk";

// example of how you would now use the fully setup sdk

export default function TestComponent() {
  const { data: wallet } = useWalletClient();
  
  async function setupStoryClient(): Promise<StoryClient> {
    const config: StoryConfig = {
      account: wallet!.account,
      transport: custom(wallet!.transport),
      chainId: "odyssey",
    };
    const client = StoryClient.newClient(config);
    return client;
  }
  
  async function registerIp() {
    const client = await setupStoryClient();
    const response = await client.ipAsset.register({
      nftContract: '0x01...',
      tokenId: '1',
      ipMetadata: {
        ipMetadataURI: "test-metadata-uri",
        ipMetadataHash: toHex("test-metadata-hash", { size: 32 }),
        nftMetadataURI: "test-nft-metadata-uri",
        nftMetadataHash: toHex("test-nft-metadata-hash", { size: 32 }),
      },
      txOptions: { waitForTransaction: true },
    });
    console.log(
      `Root IPA created at tx hash ${response.txHash}, IPA ID: ${response.ipId}`
    );
  }
  
  return (
    {/* */} 
  )
}
```