---
title: Dynamic Setup
excerpt: Learn how to setup Dynamic Wallet in your Story DApp.
deprecated: false
hidden: false
metadata:
  robots: index
---
> 📘 Optional: Official Dynamic Docs
>
> Check out the official Wagmi + Dynamic installation docs [here](https://docs.dynamic.xyz/react-sdk/using-wagmi).

## Install the Dependencies

```shell npm
npm install --save @story-protocol/core-sdk viem wagmi @dynamic-labs/sdk-react-core @dynamic-labs/wagmi-connector @dynamic-labs/ethereum @tanstack/react-query
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
   1. You can use the public default one (`https://aeneid.storyrpc.io`) or any other RPC [here](https://docs.story.foundation/docs/aeneid#-rpcs).
2. Make sure to have `NEXT_PUBLIC_DYNAMIC_ENV_ID` set up in your `.env` file. Do this by logging into [Dynamic](https://app.dynamic.xyz/) and creating a project.

```jsx Web3Providers.tsx
"use client";
import { createConfig, WagmiProvider } from "wagmi";
import { http } from 'viem';
import { QueryClient, QueryClientProvider } from "@tanstack/react-query";
import { DynamicContextProvider } from "@dynamic-labs/sdk-react-core";
import { DynamicWagmiConnector } from "@dynamic-labs/wagmi-connector";
import { EthereumWalletConnectors } from "@dynamic-labs/ethereum";
import { PropsWithChildren } from "react";
import { aeneid } from "@story-protocol/core-sdk";

// setup wagmi
const config = createConfig({
  chains: [aeneid],
  multiInjectedProviderDiscovery: false,
  transports: {
    [aeneid.id]: http(),
  },
});
const queryClient = new QueryClient();

export default function Web3Providers({ children }: PropsWithChildren) {
  return (
    // setup dynamic
    <DynamicContextProvider
      settings={{
        // Find your environment id at https://app.dynamic.xyz/dashboard/developer
        environmentId: process.env.NEXT_PUBLIC_DYNAMIC_ENV_ID as string,
        walletConnectors: [EthereumWalletConnectors],
      }}
    >
      <WagmiProvider config={config}>
        <QueryClientProvider client={queryClient}>
          <DynamicWagmiConnector>
            {children}
          </DynamicWagmiConnector>
        </QueryClientProvider>
      </WagmiProvider>
    </DynamicContextProvider>
  );
}
```
```jsx layout.tsx
import type { Metadata } from "next";
import { Inter } from "next/font/google";
import "./globals.css";
import { PropsWithChildren } from "react";
import Web3Providers from "./Web3Providers";
import { DynamicWidget } from "@dynamic-labs/sdk-react-core";

const inter = Inter({ subsets: ["latin"] });

export const metadata: Metadata = {
  title: "Example",
  description: "This is an Example DApp",
};

export default function RootLayout({ children }: PropsWithChildren) {
  return (
    <html lang="en">
      <body>
        <Web3Providers>
          <DynamicWidget />
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
      wallet: wallet,
      transport: custom(wallet!.transport),
      chainId: "aeneid",
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