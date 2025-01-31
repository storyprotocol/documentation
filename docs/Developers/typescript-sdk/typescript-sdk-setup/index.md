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

In the current folder `story-ts-example`, install the Story Protocol SDK node package, as well as `viem` ([https://www.npmjs.com/package/viem](https://www.npmjs.com/package/viem)) to access the DeFi wallet accounts.

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

<Cards columns={1}>
  <Card title="Working Example" href="https://github.com/storyprotocol/typescript-tutorial/blob/v1.3/scripts/utils/utils.ts" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    Check out the TypeScript Tutorial for a working example of how to set up the Story SDK Client.
  </Card>
</Cards>

Before continuing with the code below:

1. Make sure to have `WALLET_PRIVATE_KEY` set up in your `.env` file.
2. Make sure to have `RPC_PROVIDER_URL` set up in your `.env` file.
   1. You can use the public default one (`https://aeneid.storyrpc.io`) or check out the other RPCs [here](https://docs.story.foundation/docs/story-network#-rpcs).

```typescript index.ts
import { http } from 'viem';
import { Account, privateKeyToAccount, Address } from 'viem/accounts';
import { StoryClient, StoryConfig } from "@story-protocol/core-sdk";

const privateKey: Address = `0x${process.env.WALLET_PRIVATE_KEY}`;
const account: Account = privateKeyToAccount(privateKey);

const config: StoryConfig = {
  account: account, // the account object from above
  transport: http(process.env.RPC_PROVIDER_URL),
  chainId: 'aeneid'
};
export const client = StoryClient.newClient(config);
```

## Set Up JSON-RPC Account (ex. Metamask)

We can also use the TypeScript SDK to delay signing & sending transactions to a JSON-RPC account like Metamask.

We recommend using wagmi as a Web3 provider and then installing a wallet service like Dynamic, RainbowKit, or WalletConnect. We provide an example for all 3:

* [Dynamic Setup Tutorial](doc:dynamic-setup)
* [RainbowKit Setup Tutorial](doc:rainbowkit-setup)
* [WalletConnect Setup Tutorial](doc:walletconnect-setup)
* [Tomo Setup Tutorial](doc:tomo-setup)