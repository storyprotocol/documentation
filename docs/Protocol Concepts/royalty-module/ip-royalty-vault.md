---
title: IP Royalty Vault
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
<Accordion title="Skip the Read - 1 Minute Summary" icon="fa-info-circle">
  An IP Royalty Vault is a pool for all monetary inflows related to an IP Asset.

  Every IP Asset has 100,000,000 Royalty Tokens associated with it, where each token represents the right to 0.000001% of that IPA's revenue (*"Revenue Tokens"*) stored in the pool.

  Revenue Tokens are ERC-20 tokens used for payment (ex. USDC). These tokens must be whitelisted by the protocol to be used.
</Accordion>

Each IP Asset has an IP Royalty Vault, which acts as a pool for all monetary inflows related to an IP Asset's commercial exploration or from minting licenses. Anyone who holds Royalty Tokens (defined below) has the right to claim their share of this pool.

<Cards columns={1}>
  <Card title="IPRoyaltyVault.sol" href="https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/modules/royalty/policies/IpRoyaltyVault.sol" icon="fa-scroll" iconColor="#ccb092" target="_blank">
    View the smart contract for the IP Royalty Vault.
  </Card>
</Cards>

## Token Terminology

1. **Royalty Tokens**: the IP Royalty Vault contract is also the ERC-20 contract for the Royalty Tokens of each IP Asset. This means the address of the IP Royalty Vault for an IP Asset is also the ERC-20 token address of the Royalty Tokens. Each IP Asset has 100,000,000 Royalty Tokens associated, where each token represents 0.000001% of those gains. The holders of these Royalty Tokens can claim the Revenue Tokens (defined below) that are in the associated IP Royalty Vault.
2. **Revenue Tokens**: these are the tokens used for payment (ie. ETH, USDC, etc). Royalty Tokens can be used to claim Revenue Tokens. Read below on whitelisting :arrow_heading_down:

### Whitelisted Revenue Tokens

An ERC-20 token must be whitelisted by our protocol in the [RoyaltyModule.sol contract](https://github.com/storyprotocol/protocol-core-v1/blob/e339f0671c9172a6699537285e32aa45d4c1b57b/contracts/modules/royalty/RoyaltyModule.sol#L50) to be used as a Revenue Token. Here are the whitelisted tokens:

<Tabs>
  <Tab title="Aeneid Testnet">
    | Token  | Contract Address                             | Explorer                                                                                                                   | Mint                                                                                                                                                |
    | :----- | :------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------- |
    | WIP    | `0x1514000000000000000000000000000000000000` | <a href="https://aeneid.storyscan.xyz/address/0x1514000000000000000000000000000000000000" target="_blank">View here ↗️</a> | N/A                                                                                                                                                 |
    | MERC20 | `0xF2104833d386a2734a4eB3B8ad6FC6812F29E38E` | <a href="https://aeneid.storyscan.xyz/address/0xF2104833d386a2734a4eB3B8ad6FC6812F29E38E" target="_blank">View here ↗️</a> | <a href="https://aeneid.storyscan.xyz/address/0xF2104833d386a2734a4eB3B8ad6FC6812F29E38E?tab=write_contract#0x40c10f19" target="_blank">Mint ↗️</a> |
  </Tab>

  <Tab title="Mainnet">
    | Token | Contract Address                             | Explorer                                                                                                                   | Mint |
    | :---- | :------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------- | :--- |
    | WIP   | `0x1514000000000000000000000000000000000000` | <a href="https://aeneid.storyscan.xyz/address/0x1514000000000000000000000000000000000000" target="_blank">View here ↗️</a> | N/A  |
  </Tab>
</Tabs>

## How to obtain Royalty Tokens?

When an IP Asset receives revenue, it is deposited into its IP Royalty Vault. In order to claim revenue from this vault, you must have the associated Royalty Tokens. Once any address owns Royalty Tokens of a given IP Asset, it is entitled to that % (% of the total supply of Royalty Tokens owned) of any future Revenue Token (that is whitelisted) received & in the IP Royalty Vault.

There are two ways that trigger the IP Royalty Vault deployment and make the initial Royalty Token distribution - whichever comes first:

1. A License Token is minted from an IP for the first time: the associated IP Account (the parent's IP Account) receives 100% of the Royalty Tokens
2. An IP registers as a derivative: the associated IP Account (the child's IP Account) receives 100% Royalty Tokens, and then distributes x% of it to ancestors based on the License Terms

Because Royalty Tokens are ERC-20, they can be transferred like any other token. Thus, the IP Account could send them to someone else, or even put them up for sale on the secondary market.

## How Revenue Flows

> 🚧 Quick Note
>
> The below example uses $USDC, when in reality the token would be one of our [whitelisted revenue tokens](https://docs.story.foundation/docs/deployed-smart-contracts#whitelisted-revenue-tokens) like $WIP.

This section will help explain how revenue flows from the time of payment to being claimed by the royalty token holder. For the purposes of explanation, we will use an example from the [Liquid Absolute Percentage (LAP)](doc:liquid-absolute-percentage), but it is the same for any royalty policy.

Imagine we have a scenario where IPA4 tips IPA3 1M USDC by calling `payRoyaltyOnBehalf`.

1. Revenue Tokens flow to the Royalty Module contract. This contract then splits up the tokens based on the **royalty stack** on the receiving IPA. In this case, IPA3 has a royalty stack of 15%, so 850k tokens flow to IP Royalty Vault 3, and 150k tokens flow to the LAP contract.

![](https://files.readme.io/9cc0e9761fddd67bfeae74b14e2ec2bf51f686f72a145df096a96b54a17c5cd1-image.png)

<br />

2. The LAP contract separates the payment to the ancestors by calling `transferToVault`. In this case, IPA2 deserves 100k (10% of IPA3's earnings) and IPA1 deserves 50k (5% of IPA3's earnings).

![](https://files.readme.io/d91316a30d117ea76bd42eecf3030aa1c6bf251217193d4e6e504d0f7b7367b8-image.png)

<br />

3. Now that the Revenue Tokens are in the IP Royalty Vaults, the associated Royalty Token holders can claim from the vaults. Remember, the Revenue Tokens get claimed to whoever holds the Royalty Tokens. In the most common case, they are in the IP Account since that's where they originate. To claim, you would call either `claimRevenueOnBehalfByTokenBatch` or `claimRevenueOnBehalf`.

![](https://files.readme.io/ef288631b074cd82d2edb95b6b27844db6a36cd8976a29f20a0cd9b393752218-image.png)

<br />

### External Royalty Policies

Revenue Tokens can also move from a vault to another vault via the functions `claimByTokenBatchAsSelf` located in the `IpRoyaltyVault.sol` contract. For this to be possible the vault that is claiming revenue tokens needs to own Royalty Tokens of the vault being claimed from. This can be particularly useful when used together with external royalty policies.

Vaults can only claim from other vaults if those other vaults belong to IPs in the same derivative chain. If a vault owns royalty tokens from an IP but it is not an ancestor of that IP, it is not possible to claim rewards with those royalty tokens.