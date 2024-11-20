---
title: ❓ Concepts FAQ
excerpt: Common technical questions related to our protocol documentation.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
## _"What is the difference between License Tokens, Royalty Tokens, and Revenue Tokens?"_

[block:parameters]
{
  "data": {
    "h-0": "",
    "h-1": "License Tokens",
    "h-2": "Royalty Tokens",
    "h-3": "Revenue Tokens",
    "0-0": "**Module**",
    "0-1": "[📜 Licensing Module](doc:licensing-module)",
    "0-2": "[💸 Royalty Module](doc:royalty-module)",
    "0-3": "[💸 Royalty Module](doc:royalty-module)",
    "1-0": "**Explanation**",
    "1-1": "An ERC-721 NFT that gets minted from an IP Asset with specific license terms. It is essentially the license you hold that gives you access to use the associated IP Asset based on the terms in the License Token.  \n  \nA License Token is burned when it is used to register an IP Asset as a derivative of another.",
    "1-2": "Each IP Asset has 100,000,000 Royalty Tokens associated, where each token represents the right of whoever owns them to claim 0.000001% of the gains (\"_Revenue Tokens_\") deposited into the IPA's Royalty Vault.",
    "1-3": "These are the tokens that are actually used for payment (ex. ETH, USDC, etc).  \n  \n\"_Royalty Tokens_\" are used to claim these Revenue Tokens when an IP Asset earns them.",
    "2-0": "**Associated Docs**",
    "2-1": "[License Token](doc:license-token)",
    "2-2": "[IP Royalty Vault](doc:ip-royalty-vault)",
    "2-3": "[IP Royalty Vault](doc:ip-royalty-vault)"
  },
  "cols": 4,
  "rows": 3,
  "align": [
    "left",
    "left",
    "left",
    "left"
  ]
}
[/block]