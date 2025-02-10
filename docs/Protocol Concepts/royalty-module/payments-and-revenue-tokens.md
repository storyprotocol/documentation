---
title: Payments and Revenue Tokens
excerpt: The requirements and details on making payments with Revenue Tokens on Story.
deprecated: false
hidden: true
metadata:
  robots: index
---
All payments on Story are made with **Revenue Tokens**. These are ERC-20 tokens that have been whitelisted by our protocol.

## When do payments happen?

There are three common scenarios when you'd make a payment on Story:

1. Mint License Tokens - minting a license token from an IP Asset that has a minting fee. You do this either to hold the license token or use it to register your IP Asset as a derivative
2. Paying Commercial Revenue Share - your license terms with an ancestor IP Asset require you to forward a certain % of your revenue. For ex. share 50% of your revenue share with the parent IP Asset
3. Tipping - you just want to send some money to an IP Asset

## Whitelisted Revenue Tokens

Revenue Tokens are ERC-20 tokens that have been whitelisted by our protocol. Currently, the whitelisted revenue tokens are:

<Tabs>
  <Tab title="Aeneid Testnet">
    | Token  | Contract Address                             | Explorer                                                                                                                   | Mint                                                                                                                                                                                                                            |
    | :----- | :------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
    | WIP    | `0x1514000000000000000000000000000000000000` | <a href="https://aeneid.storyscan.xyz/address/0x1514000000000000000000000000000000000000" target="_blank">View here ↗️</a> | N/A                                                                                                                                                                                                                             |
    | MERC20 | `0xF2104833d386a2734a4eB3B8ad6FC6812F29E38E` | <a href="https://aeneid.storyscan.xyz/address/0xF2104833d386a2734a4eB3B8ad6FC6812F29E38E" target="_blank">View here ↗️</a> | [https://aeneid.storyscan.xyz/address/0xF2104833d386a2734a4eB3B8ad6FC6812F29E38E?tab=write\_contract#0x40c10f19](https://aeneid.storyscan.xyz/address/0xF2104833d386a2734a4eB3B8ad6FC6812F29E38E?tab=write_contract#0x40c10f19) |
  </Tab>

  <Tab title="Mainnet">
    | Token | Contract Address                             | Explorer                                                                                                                   | Mint |
    | :---- | :------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------- | :--- |
    | WIP   | `0x1514000000000000000000000000000000000000` | <a href="https://aeneid.storyscan.xyz/address/0x1514000000000000000000000000000000000000" target="_blank">View here ↗️</a> | N/A  |
  </Tab>
</Tabs>

## Testing

On Aeneid testnet, we have provided a `MockERC20` token for you to be able to mint and use