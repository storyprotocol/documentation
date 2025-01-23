---
title: Mint a License
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
This section demonstrates how to mint a license for an IP asset. You can only mint a license for an IP asset if a policy has been added to its configuration. A license is minted as an ERC1155 token and contains the necessary licensing details.

## Prerequisites

* [Setup](doc:react-sdk-setup) the Web3Provider.
* An IP asset that has a policy added. Learn how to add a policy to an IP asset [here](doc:react-sdk-attach-a-policy-to-an-ip-asset).

# Mint License

To mint a license, we will need the `ipId` (address of the IP asset), the `policyId` string of the policy for the intended license, the `mintAmount` of licenses to be minted (usually 1, unless the minter decides to mint a batch and distribute it in another way later on), and the `receiverAddress` (usually the wallet address that is executing the transaction).

We can use the `useMintLicense` hook to mint a license:

```jsx MintLicense.tsx
'use client';
import { useMintLicense } from '@story-protocol/react';
import { Address, zeroAddress } from 'viem';
import { useAccount } from 'wagmi';

export default function MintLicense() {
  const { writeContractAsync, isPending, data: txHash } = useMintLicense();
  const { address } = useAccount();

  //* NOTE: if the `policyId` has commercialUse = true, then this transaction below will fail because it requires payments.
  //* For the sake of this example, we will keep it simpler (non commercial)

  // Update these
  const ipId = '0x12591729eDd365807C48AC90dc857f6f28b5e448'; // Update this with the IP ID address from RegisterRootIp.tsx
  const policyId = BigInt(4); // Update this with a policy that's attached to the IP Asset

  const amount = BigInt(1); // The amount of licenses to mint
  const minter = address as Address; // The recipient of the license
  const royaltyContext = '0x'; // Additional calldata for the royalty policy

  function handleClick() {
    writeContractAsync({
      functionName: 'mintLicense',
      args: [policyId, ipId, amount, minter, royaltyContext],
    });
  }

  return (
    <button onClick={() => handleClick()}>
      Mint License
    </button>
  );
}
```

Setting`waitForTransaction: true` in the transaction options will return the `licenseId` of the newly minted license(s).
