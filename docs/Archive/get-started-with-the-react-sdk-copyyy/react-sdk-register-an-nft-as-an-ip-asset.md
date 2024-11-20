---
title: Register an NFT as an IP Asset
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
      slug: react-sdk-create-a-pil-policy
      title: Create a PIL Policy
---
This section demonstrates how to register an IP asset using the React SDK. There are generally two methods of IP registration:

1. Register an existing NFT as an IP asset
2. Create an NFT and register it as an IP asset in one transaction

For this section, we will register an existing NFT as a IP asset (#1 above).

## Prerequisites

- [Setup](doc:react-sdk-setup) the Web3Provider.
- Own an ERC721 NFT in your wallet. Have it's contract address and token ID ready. 

# Register an Existing NFT as an IP Asset

You can register your NFT as an IP Asset by utilizing the `useRegisterRootIp` hook and passing in the following options:

```jsx RegisterIPAsset.tsx
'use client';
import { useRegisterRootIp } from '@story-protocol/react';
import { stringToHex } from 'viem';

export default function RegisterIpAsset() {
  const {
    writeContractAsync,
    isPending: isPendingInWallet,
    data: txHash,
  } = useRegisterRootIp();

  const tokenId = BigInt(12); // Update if using your own NFT
  const nftContract = '0xd516482bef63Ff19Ed40E4C6C2e626ccE04e19ED'; // Update if using your own NFT

  // You can use an existing `policyId` or register your own
  // in `RegisterPILPolicy.tsx`
  const policyId = BigInt(0);
  const ipName = 'IP Man'; // Name of your IP, if applicable
  const contentHash = stringToHex('0x', { size: 32 }); // Content hash of your NFT, if applicable
  const externalURL = 'https://example.com'; // External URL for your IP, if applicable

  async function handleClick() {
    await writeContractAsync({
      functionName: 'registerRootIp',
      args: [policyId, nftContract, tokenId, ipName, contentHash, externalURL],
    });
  }

  return (
    <button onClick={() => handleClick()}>
    	Register IP Asset
    </button>
  );
}
```