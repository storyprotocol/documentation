---
title: Wagmi + Dynamic Setup
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
_Optional: Check out the official Wagmi + Dynamic installation docs [here](https://docs.dynamic.xyz/adding-dynamic/using-wagmi)._

### Install the Dependencies

```shell npm
npm install --save @story-protocol/react-sdk viem wagmi @dynamic-labs/sdk-react-core @dynamic-labs/wagmi-connector @dynamic-labs/ethereum @tanstack/react-query
```
```shell pnpm
pnpm install @story-protocol/core-sdk viem
```
```shell yarn
yarn add @story-protocol/core-sdk viem
```

### Setup

Before diving into the example, make sure you have two things setup:

1. Make sure to have `NEXT_PUBLIC_RPC_PROVIDER_URL` set up in your `.env` file. You can use the public default one (`NEXT_PUBLIC_RPC_PROVIDER_URL=https://testnet.storyrpc.io`) or any other RPC [here](https://docs.story.foundation/docs/story-network#-rpcs).
2. Make sure to have `NEXT_PUBLIC_DYNAMIC_ENV_ID` set up in your .env file. Do this by logging into [Dynamic](https://app.dynamic.xyz/) and creating a project.

You can then configure your DApp with help from the following example:

```jsx Web3Providers.tsx
"use client";
import { http, createConfig, WagmiProvider } from "wagmi";
import { QueryClient, QueryClientProvider } from "@tanstack/react-query";
import { DynamicContextProvider } from "@dynamic-labs/sdk-react-core";
import { DynamicWagmiConnector } from "@dynamic-labs/wagmi-connector";
import { EthereumWalletConnectors } from "@dynamic-labs/ethereum";
import { PropsWithChildren } from "react";
import { StoryProvider } from "@story-protocol/react-sdk";
import { createWalletClient, type Chain } from "viem";

export const iliad = {
  id: 1513, // Your custom chain ID
  name: "Story Network Testnet",
  nativeCurrency: {
    name: "Testnet IP",
    symbol: "IP",
    decimals: 18,
  },
  rpcUrls: {
    default: { http: ["https://testnet.storyrpc.io"] },
  },
  blockExplorers: {
    default: { name: "Blockscout", url: "https://testnet.storyscan.xyz" },
  },
  testnet: true,
} as const satisfies Chain;

// setup wagmi
const config = createConfig({
  chains: [iliad],
  multiInjectedProviderDiscovery: false,
  transports: {
    [iliad.id]: http(),
  },
});
const queryClient = new QueryClient();

// add any extra networks here
const evmNetworks = [
  {
    blockExplorerUrls: ["https://testnet.storyscan.xyz"],
    chainId: 1513,
    iconUrls: ["https://app.dynamic.xyz/assets/networks/sepolia.svg"],
    name: "Story Network Testnet",
    nativeCurrency: {
      decimals: 18,
      name: "Testnet IP",
      symbol: "IP",
    },
    networkId: 1513,
    rpcUrls: ["https://testnet.storyrpc.io"],
    vanityName: "Iliad",
  },
];

export default function Web3Providers({ children }: PropsWithChildren) {
  return (
    // setup dynamic
    <DynamicContextProvider
      settings={{
        appName: "Story Documentation",
        // Find your environment id at https://app.dynamic.xyz/dashboard/developer
        environmentId: process.env.NEXT_PUBLIC_DYNAMIC_ENV_ID as string,
        walletConnectors: [EthereumWalletConnectors],
        overrides: { evmNetworks },
        networkValidationMode: "always",
      }}
    >
      <WagmiProvider config={config}>
        <QueryClientProvider client={queryClient}>
          <DynamicWagmiConnector>
            <StoryProviderWrapper>
              {children}
            </StoryProviderWrapper>
          </DynamicWagmiConnector>
        </QueryClientProvider>
      </WagmiProvider>
    </DynamicContextProvider>
  );
}

// we use this component to pass in our 
// wallet from wagmi
function StoryProviderWrapper({ children}: PropsWithChildren) {
  const { data: wallet } = useWalletClient();
  
	const dummyWallet = createWalletClient({
    chain: iliad,
    transport: http("https://testnet.storyrpc.io"),
  });
  
  return (
    <StoryProvider
      config={{
        chainId: "iliad",
        transport: http(process.env.NEXT_PUBLIC_RPC_PROVIDER_URL),
        wallet: wallet || dummyWallet,
      }}
    >
      {children}
    </StoryProvider>
  )
}
```
```jsx layout.tsx
import type { Metadata } from "next";
import { Inter } from "next/font/google";
import "./globals.css";
import { PropsWithChildren } from "react";
import Web3Providers from "./Web3Providers";

const inter = Inter({ subsets: ["latin"] });

export const metadata: Metadata = {
  title: "Example",
  description: "This is an Example DApp",
};

export default function RootLayout({
  children
}: PropsWithChildren) {
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
import { custom, toHex } from 'viem';
import { useIpAsset } from "@story-protocol/react-sdk";

// example of how you would now use the fully setup react sdk

export default async function TestComponent() {
  const { register } = useIpAsset();
  
  const response = await register({
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
  
  return (
    {/* */} 
  )
}
```