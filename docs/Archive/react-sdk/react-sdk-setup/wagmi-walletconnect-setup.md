---
title: Wagmi + WalletConnect Setup
excerpt: ""
deprecated: false
hidden: true
metadata:
  title: ""
  description: ""
  robots: index
next:
  description: ""
---

_Optional: Check out the official Wagmi + WalletConnect installation docs[here](https://docs.walletconnect.com/appkit/next/core/installation)._

### Install the Dependencies

```shell npm
npm install --save @story-protocol/react-sdk @web3modal/wagmi wagmi viem @tanstack/react-query
```

```shell pnpm
pnpm install @story-protocol/core-sdk viem
```

```shell yarn
yarn add @story-protocol/core-sdk viem
```

### Setup

Before diving into the example, make sure you have two things setup:

1. Make sure to have `NEXT_PUBLIC_RPC_PROVIDER_URL` set up in your `.env` file. You can use the public default one (`NEXT_PUBLIC_RPC_PROVIDER_URL=https://testnet.storyrpc.io`) or any other RPC [here](https://docs.story.foundation/docs/aeneid#-rpcs).
2. Make sure to have `NEXT_PUBLIC_WALLET_CONNECT_PROJECT_ID` set up in your .env file. Do this by logging into [WalletConnect](https://cloud.walletconnect.com/) and creating a project.

You can then configure your DApp with help from the following example:

```jsx Web3Providers.tsx
"use client";
import { defaultWagmiConfig } from "@web3modal/wagmi/react/config";
import { cookieStorage, createStorage, http, useWalletClient } from "wagmi";
import React, { PropsWithChildren, ReactNode } from "react";
import { createWeb3Modal } from "@web3modal/wagmi/react";
import { QueryClient, QueryClientProvider } from "@tanstack/react-query";
import { State, WagmiProvider } from "wagmi";
import { StoryProvider } from "@story-protocol/react-sdk";

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

// Get projectId from https://cloud.walletconnect.com
const projectId = process.env.NEXT_PUBLIC_WALLET_CONNECT_PROJECT_ID;

if (!projectId) throw new Error("Project ID is not defined");

const metadata = {
  name: "Artcast",
  description: "Create and remix each others AI-generated images.",
  url: "https://artcast.ai", // origin must match your domain & subdomain
  icons: ["https://artcast.ai/logo.png"],
};

// Create wagmiConfig
const chains = [iliad] as const;
const config = defaultWagmiConfig({
  chains,
  projectId,
  metadata,
  ssr: true,
  storage: createStorage({
    storage: cookieStorage,
  }),
});

// Setup queryClient
const queryClient = new QueryClient();

// Create modal
createWeb3Modal({
  metadata,
  //@ts-ignore
  wagmiConfig: config,
  projectId,
  enableAnalytics: true, // Optional - defaults to your Cloud configuration
});

export default function Web3Providers({
  children,
  initialState,
}: {
  children: ReactNode;
  initialState?: State;
}) {
  return (
    <WagmiProvider config={config} initialState={initialState}>
      <QueryClientProvider client={queryClient}>
        <StoryProviderWrapper>{children}</StoryProviderWrapper>
      </QueryClientProvider>
    </WagmiProvider>
  );
}

// we use this component to pass in our
// wallet from wagmi
function StoryProviderWrapper({ children }: PropsWithChildren) {
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
  );
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

export default function RootLayout({ children }: PropsWithChildren) {
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
import { custom, toHex } from "viem";
import { useIpAsset } from "@story-protocol/react-sdk";

// example of how you would now use the fully setup react sdk

export default async function TestComponent() {
  const { register } = useIpAsset();

  const response = await register({
    nftContract: "0x01...",
    tokenId: "1",
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

  return {
    /* */
  };
}
```
