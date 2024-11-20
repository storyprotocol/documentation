---
title: React SDK Setup
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
  pages:
    - type: basic
      slug: react-sdk-register-an-nft-as-an-ip-asset
      title: Register an NFT as an IP Asset
---
# Prerequisite

For running the tutorial for developers, we require node version 16 or later version and npm version 8 to be installed in your environment. To install node and npm, we recommend you go to the [Node.js official website](https://nodejs.org) and download the latest LTS (Long Term Support) version.

If you want to upgrade node to 16 or higher version, we recommend you to use [nvm](https://github.com/nvm-sh/nvm) (Node Version Manager). If you want to upgrade npm to the latest version, you can run the command `npm install -g nvm`.

# Create a React Project

Create a new React project using `create-react-app`:

```Text Shell
npx create-react-app my-story-protocol-example
```

Go into the folder and run `npm init` to initialize the project:

```Text Shell
cd my-story-protocol-example/
npm init
```

You can keep all values as default by hitting enter key when `npm init` command prompting you to input `package name`, `version`,`description` etc. After the command is executed, you will see a file named `package.json` is created.

# Install the Dependencies

In the current folder `my-story-protocol-example`, install the Story Protocol React SDK package, as well as `viem` ([https://www.npmjs.com/package/viem](https://www.npmjs.com/package/viem)) to access the DeFi wallet accounts, wagmi & connectkit:

```shell npm
npm install --save @story-protocol/react viem@1.21.4 wagmi connectkit
```
```shell pnpm
pnpm install @story-protocol/core-sdk@0.1.0-alpha viem@1.21.4
```
```shell yarn
yarn add @story-protocol/core-sdk@0.1.0-alpha viem@1.21.4
```

# Set up a Web3Provider

First, make sure to add the following to your .env file:

* `NEXT_PUBLIC_WC_PROJECT_ID`: A [WalletConnect](https://walletconnect.com/) ID
* `NEXT_PUBLIC_RPC_PROVIDER_URL`: the RPC endpoint for your desired chain. We recommend using the Sepolia network ([https://rpc.ankr.com/eth\_sepolia](https://rpc.ankr.com/eth_sepolia))

Next, create a file called `Web3Provider.tsx` that looks like this:

```jsx Web3Provider.tsx
'use client';

import { WagmiProvider, createConfig, http } from 'wagmi';
import { sepolia } from 'wagmi/chains';
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import { ConnectKitProvider, getDefaultConfig } from 'connectkit';
import { ReactNode } from 'react';

const config = createConfig(
  getDefaultConfig({
    chains: [sepolia],
    transports: {
      [sepolia.id]: http(
        process.env.NEXT_PUBLIC_RPC_PROVIDER_URL ?? 'https://rpc.ankr.com/eth_sepolia'
      ),
    },
    ssr: true,
    walletConnectProjectId: process.env.NEXT_PUBLIC_WC_PROJECT_ID!, // Required API Keys
    appName: 'Your App Name',
  })
);

const queryClient = new QueryClient();

export const Web3Provider = ({ children }: { children: ReactNode }) => {
  return (
    <WagmiProvider config={config}>
      <QueryClientProvider client={queryClient}>
        <ConnectKitProvider>{children}</ConnectKitProvider>
      </QueryClientProvider>
    </WagmiProvider>
  );
};
```

We can use this Web3Provider to wrap our app in the `layout.tsx` like so:

```jsx layout.tsx
import type { Metadata } from 'next';
import { Inter } from 'next/font/google';
import './globals.css';
import { Web3Provider } from '../utils/Web3Provider';

const inter = Inter({ subsets: ['latin'] });

export const metadata: Metadata = {
  title: 'Story Protocol React SDK Test',
  description: 'Story Protocol React SDK Test',
};

export default function RootLayout({
  children,
}: Readonly<{
  children: React.ReactNode;
}>) {
  return (
    <html lang="en">
      <Web3Provider>
        <body className={inter.className}>{children}</body>
      </Web3Provider>
    </html>
  );
}
```
