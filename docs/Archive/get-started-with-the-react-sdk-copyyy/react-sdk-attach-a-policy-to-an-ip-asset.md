---
title: Attach a Policy to an IP Asset
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
      slug: react-sdk-mint-a-license
      title: Mint a License
---
This section demonstrates how to add an existing policy to an IPA. By creating such a policy, an IPA becomes eligible for licensing creation. Users who then wish to creative derivatives of the IP may then mint licenses, which can be burned to enroll their IPs as derivative IPAs of the original work.

## Prerequisites

* [Setup](doc:react-sdk-setup) the Web3Provider.
* An existing IPA (`ipId`). Learn how to register an IP Asset [here](doc:react-sdk-register-an-nft-as-an-ip-asset).
* An existing Policy (`policyId`). Learn how to create a PIL Policy [here](doc:react-sdk-create-a-pil-policy).

# Attach a Policy to an IP asset

Once the policy is created, you can add the policy to your IP asset so you will be able to mint licenses based on the policy later on. One IP asset can have multiple policies.

Below is the code example to add the policy we just created to our IP Asset using the `useMintLicense` hook:

```jsx AddPolicyToIPAsset.tsx
'use client';
import { useAddPolicyToIp, useMintLicense } from '@story-protocol/react';

export default function AddPolicyToIp() {
  const { writeContractAsync, isPending, data: txHash } = useAddPolicyToIp();

  const ipId = '0x12591729eDd365807C48AC90dc857f6f28b5e448'; // Update this with the IP ID address you registered from RegisterRootIp.tsx
  const policyId = BigInt(4); // Update this with the policy registered in RegisterPILPolicy.tsx, or use a pre-existing policy that allows for derivatives

  function handleClick() {
    writeContractAsync({
      functionName: 'addPolicyToIp',
      args: [ipId, policyId],
    });
  }

  return (
    <button onClick={() => handleClick()}>
      Add Policy to IP Asset
    </button>
  );
}
```
