---
title: Create a PIL Policy
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
      slug: react-sdk-attach-a-policy-to-an-ip-asset
      title: Attach a Policy to an IP Asset
---
This section demonstrates how to create a policy with PIL licensing framework parameters that follows the pre-set [Non-Commercial Social Remixing license](doc:pil-flavors-preset-policy)

## Prerequisites

* [Setup](doc:react-sdk-setup) the Web3Provider.
* Understand the concept of policies and licenses.
* If a PIL policy already exists for the identical set of parameters you intend to create, it is unnecessary to create it again and the function will error. You can simply use an existing policy by its `policyId`.

# Create a new PIL Policy

You can register your NFT as an IP Asset by utilizing the `useRegisterPILPolicy` hook and passing in the following options. We first construct an object that consists of the relevant policy parameters we intend to set explicitly.

```jsx RegisterPILPolicy.tsx
'use client';

import { useRegisterPILPolicy } from '@story-protocol/react';
import { zeroAddress } from 'viem';

export default function RegisterPILPolicy() {
  const {
    writeContractAsync,
    isPending,
    data: txHash,
  } = useRegisterPILPolicy();

  const policyParameters = {
    attribution: true, // Whether or not attribution is required when reproducing the work
    commercialUse: false, // Whether or not the work can be used commercially
    commercialAttribution: false, // Whether or not attribution is required when reproducing the work commercially
    commercializerChecker: zeroAddress, // commercializers that are allowed to commercially exploit the work. If zero address, then no restrictions is enforced
    commercializerCheckerData: '0x', // Additional calldata for the commercializer checker
    commercialRevShare: 0, // Percentage of revenue that must be shared with the licensor
    derivativesAllowed: true, // Whether or not the licensee can create derivatives of his work
    derivativesAttribution: true, // Whether or not attribution is required for derivatives of the work
    derivativesApproval: false, // Whether or not the licensor must approve derivatives of the work before they can be linked to the licensor IP ID
    derivativesReciprocal: false, // Whether or not the licensee must license derivatives of the work under the same terms
    territories: ['USA', 'CANADA'], // List of territories where the license is valid. If empty, global
    distributionChannels: [], // List of distribution channels where the license is valid. Empty if no restrictions.
    contentRestrictions: []
  };

  const registrationParams = {
    transferable: true, // Whether or not attribution is required when reproducing the work
    royaltyPolicy: zeroAddress, // Address of a royalty policy contract that will handle royalty payments
    mintingFee: BigInt(0),
    mintingFeeToken: zeroAddress,
    policy: policyParameters,
  };

  async function handleClick() {
    await writeContractAsync({
      functionName: 'registerPolicy',
      args: [registrationParams],
    });
  }

  return (
    <button onClick={() => handleClick()}>
			Register PIL Policy
		</button>
  );
}
```
