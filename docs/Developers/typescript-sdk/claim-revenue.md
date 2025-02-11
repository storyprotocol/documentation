---
title: Claim Revenue
excerpt: Learn how to claim due revenue from a child IP Asset in TypeScript.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/registerDerivativeCommercialSpg.ts" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    All of this page is covered in this working code example.
  </Card>
</Cards>

This section demonstrates how to claim due revenue from an IP Asset.

There are two main ways revenue can be claimed:

1. **Scenario #1**: Someone pays my IP Asset directly, and I claim that revenue.
2. **Scenario #2**: Someone pays a derivative IP Asset of my IP, and I have the right to a % of their revenue based on the `commercialRevShare` in the license terms.

> 🚧 Quick Note
>
> The below examples and diagrams show USDC. In reality, you'd be using one of the whitelisted revenue tokens [listed here](https://docs.story.foundation/docs/deployed-smart-contracts#/), which is more often than not $WIP.

### :warning: Prerequisites

There are a few steps you have to complete before you can start the tutorial.

1. Complete the [TypeScript SDK Setup](doc:typescript-sdk-setup)
2. Have a basic understanding of the [💸 Royalty Module](doc:royalty-module)
3. Obviously, there must be a payment to be claimed. Read [Pay an IPA](doc:pay-ipa)

## Before We Start

When payments are made, they eventually end up in an IP Asset's [IP Royalty Vault](doc:ip-royalty-vault). From here, they are claimed/transferred to whoever owns the Royalty Tokens associated with it, which represent a % of revenue share for a given IP Asset's IP Royalty Vault.

The IP Account (the smart contract that represents the [🧩 IP Asset](doc:ip-asset)) is what holds 100% of the Royalty Tokens when it's first registered. So usually, it indeed holds most of the Royalty Tokens.

## Scenario #1

In this scenario, I own IP Asset 3. Someone pays my IP Asset 3 directly, and I claim that revenue.

![](https://files.readme.io/a39e345a512747ff414668309a80ec1b01048c238bf1adef4d0760d830006cbd-image.png)

As we can see in the above diagram, when IP Asset 4 (it doesn't have to be an IP Asset, it can be any address) pays IP Asset 3 1M USDC, 850k USDC automatically gets deposited into IP Royalty Vault 3.

Below is how IP Asset 3 would claim their revenue:

> :eyes: Note the comments under the `claimOptions` object.

```typescript main.ts
import { WIP_TOKEN_ADDRESS } from '@story-protocol/core-sdk'
// you should already have a client set up (prerequisite)
import { client } from './client'

async function main() {
  const claimRevenue = await client.royalty.claimAllRevenue({
    // IP Asset 3's ipId
    ancestorIpId: '0xDa03c4B278AD44f5a669e9b73580F91AeDE0E3B2',
    // whoever owns the royalty tokens associated with IP Royalty Vault 3
    // (most likely the associated ipId, which is IP Asset 3's ipId)
    claimer: '0xDa03c4B278AD44f5a669e9b73580F91AeDE0E3B2',
    currencyTokens: [WIP_TOKEN_ADDRESS],
    childIpIds: [],
    royaltyPolicies: [],
    claimOptions: {
      // If the wallet claiming the revenue is the owner of the 
      // IP Account/IP Asset (in other words, the owner of the 
      // IP's underlying NFT), `claimAllRevenue` will transfer all 
      // earnings to the user's external wallet holding the NFT 
      // instead of the IP Account, for convenience. You can disable it here.
      autoTransferAllClaimedTokensFromIp: true,
      // Unwraps the claimed $WIP to $IP for you
      autoUnwrapIpTokens: true
    }
  })

  console.log(`Claimed revenue: ${claimRevenue.claimedTokens}`);
}

main();
```

## Scenario #2

In this scenario, I own IP Asset 1. Someone pays a derivative IP Asset 3, and I have the right to a % of their revenue based on the `commercialRevShare` in the license terms. This is exactly the same as Scenario #1, except one extra step is added.

![](https://files.readme.io/04a92b164d8f25119881efe784aed4bb539cdd955f553d74191b513ac0623b68-image.png)

Similar to Scenario #1, as we can see in the above diagram, when IP Asset 4 (it doesn't have to be an IP Asset, it can be any address) pays IP Asset 3 1M USDC, 150k USDC automatically gets deposited to the LAP royalty policy contract to be distributed to ancestors.

![](https://files.readme.io/8f11792c3e3d2e25f74a469996b9c000c61eac167d9512f9a0f353bf62b77abf-image.png)

Then, in a second step, the tokens are transferred to the ancestors' [IP Royalty Vault](doc:ip-royalty-vault) based on the negotiated `commercialRevShare` in the license terms.

Below is how IP Asset 1 (or 2) would claim their revenue:

> :eyes: Note the comments under the `claimOptions` object.

```typescript main.ts
import { WIP_TOKEN_ADDRESS } from '@story-protocol/core-sdk'
// you should already have a client set up (prerequisite)
import { client } from './client'

async function main() {
  const claimRevenue = await client.royalty.claimAllRevenue({
    // IP Asset 1's ipId
    ancestorIpId: '0x089d75C9b7E441dA3115AF93FF9A855BDdbfe384',
    // whoever owns the royalty tokens associated with IP Royalty Vault 1
    // (most likely the associated ipId, which is IP Asset 1's ipId)
    claimer: '0x089d75C9b7E441dA3115AF93FF9A855BDdbfe384',
    currencyTokens: [WIP_TOKEN_ADDRESS],
    // IP Asset 3's ipId
    childIpIds: ['0xDa03c4B278AD44f5a669e9b73580F91AeDE0E3B2'],
    // Aeneid testnet address of RoyaltyPolicyLAP
    royaltyPolicies: ['0xBe54FB168b3c982b7AaE60dB6CF75Bd8447b390E'],
    claimOptions: {
      // If the wallet claiming the revenue is the owner of the 
      // IP Account/IP Asset (in other words, the owner of the 
      // IP's underlying NFT), `claimAllRevenue` will transfer all 
      // earnings to the user's external wallet holding the NFT 
      // instead of the IP Account, for convenience. You can disable it here.
      autoTransferAllClaimedTokensFromIp: true,
      // Unwraps the claimed $WIP to $IP for you
      autoUnwrapIpTokens: true
    }
  })

  console.log(`Claimed revenue: ${claimRevenue.claimedTokens}`);
}

main();
```

## Dispute an IP

Congratulations, you claimed revenue using the [💸 Royalty Module](doc:royalty-module)!

<Cards columns={1}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts/registerDerivativeCommercialSpg.ts" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    All of this page is covered in this working code example.
  </Card>
</Cards>

Now what happens if an IP Asset doesn't pay their due share? We can dispute the IP on-chain, which we will cover on the next page.

> 🚧 Coming soon!