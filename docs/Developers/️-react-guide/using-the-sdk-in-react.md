---
title: Using the SDK in React
excerpt: Once you have the TypeScript SDK setup in React, learn how to use it.
deprecated: false
hidden: false
metadata:
  robots: index
---
Once you have the SDK setup in React, you can use it just as we describe in the [🛠️ TypeScript SDK Guide](doc:typescript-sdk).

<Cards columns={2}>
  <Card title="Working Code Examples" href="https://github.com/storyprotocol/typescript-tutorial" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    Extremely easy & straightforward working code examples for all of the following tutorials.
  </Card>

  <Card title="SDK Reference" href="https://docs.story.foundation/docs/sdk-overview#/" icon="fa-books" iconColor="#51af51" target="_blank">
    View the whole SDK reference, which shows examples and types for every function in our SDK.
  </Card>
</Cards>

### :warning: Prerequisites

1. Complete the [SDK set up in React](doc:react-setup)

## Example

Here is an example of calling an SDK function in React, which will look the same for any function you use:

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
    const response = await client.ipAsset.mintAndRegisterIp({
      spgNftContract: '0xc32A8a0FF3beDDDa58393d022aF433e78739FAbc',
      ipMetadata: {
        ipMetadataURI: "test-metadata-uri",
        ipMetadataHash: toHex("test-metadata-hash", { size: 32 }),
        nftMetadataURI: "test-nft-metadata-uri",
        nftMetadataHash: toHex("test-nft-metadata-hash", { size: 32 }),
      },
      txOptions: { waitForTransaction: true }
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