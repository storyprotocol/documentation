---
title: Reown (WalletConnect) Setup
excerpt: Learn how to setup Reown (WalletConnect) in your Story DApp.
deprecated: false
hidden: false
metadata:
  robots: index
---
> 📘 Optional: Official WalletConnect Docs
>
> Check out the official Wagmi + Reown installation docs [here](https://docs.walletconnect.com/appkit/next/core/installation).

## Install the Dependencies

```shell npm
npm install --save @story-protocol/core-sdk @reown/appkit @reown/appkit-adapter-wagmi wagmi viem @tanstack/react-query
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
2. Make sure to have `NEXT_PUBLIC_WALLET_CONNECT_PROJECT_ID` set up in your `.env` file. Do this by logging into [Reown (prev. WalletConnect)](https://reown.com/) and creating a project.

```jsx config/index.tsx
import { cookieStorage, createStorage, http } from '@wagmi/core'
import { WagmiAdapter } from '@reown/appkit-adapter-wagmi'
import { mainnet, arbitrum } from '@reown/appkit/networks'
import { aeneid } from '@story-protocol/core-sdk';

// Get projectId from https://cloud.reown.com
export const projectId = process.env.NEXT_PUBLIC_PROJECT_ID

if (!projectId) {
  throw new Error('Project ID is not defined')
}

export const networks = [aeneid]

//Set up the Wagmi Adapter (Config)
export const wagmiAdapter = new WagmiAdapter({
  storage: createStorage({
    storage: cookieStorage
  }),
  ssr: true,
  projectId,
  networks
})

export const config = wagmiAdapter.wagmiConfig
```
```jsx context/index.tsx
'use client'

import { wagmiAdapter, projectId } from '@/config'
import { QueryClient, QueryClientProvider } from '@tanstack/react-query'
import { createAppKit } from '@reown/appkit/react'
import { mainnet, arbitrum } from '@reown/appkit/networks'
import React, { type ReactNode } from 'react'
import { cookieToInitialState, WagmiProvider, type Config } from 'wagmi'

// Set up queryClient
const queryClient = new QueryClient()

if (!projectId) {
  throw new Error('Project ID is not defined')
}

// Set up metadata
const metadata = {
  name: 'appkit-example',
  description: 'AppKit Example',
  url: 'https://appkitexampleapp.com', // origin must match your domain & subdomain
  icons: ['https://avatars.githubusercontent.com/u/179229932']
}

// Create the modal
const modal = createAppKit({
  adapters: [wagmiAdapter],
  projectId,
  networks: [mainnet, arbitrum],
  defaultNetwork: mainnet,
  metadata: metadata,
  features: {
    analytics: true // Optional - defaults to your Cloud configuration
  }
})

function ContextProvider({ children, cookies }: { children: ReactNode; cookies: string | null }) {
  const initialState = cookieToInitialState(wagmiAdapter.wagmiConfig as Config, cookies)

  return (
    <WagmiProvider config={wagmiAdapter.wagmiConfig as Config} initialState={initialState}>
      <QueryClientProvider client={queryClient}>{children}</QueryClientProvider>
    </WagmiProvider>
  )
}

export default ContextProvider
```
```jsx app/layout.tsx
import type { Metadata } from 'next'
import { Inter } from 'next/font/google'
import './globals.css'

const inter = Inter({ subsets: ['latin'] })

import { headers } from 'next/headers' // added
import ContextProvider from '@/context'

export const metadata: Metadata = {
  title: 'AppKit Example App',
  description: 'Powered by Reown'
}

export default function RootLayout({
  children
}: Readonly<{
  children: React.ReactNode
}>) {

  const headersObj = await headers();
  const cookies = headersObj.get('cookie')

  return (
    <html lang="en">
      <body className={inter.className}>
        <ContextProvider cookies={cookies}>
          <appkit-button />
          {children}
        </ContextProvider>
      </body>
    </html>
  )
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