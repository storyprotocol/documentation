---
title: License Token
excerpt: >-
  An ERC-721 NFT that allows you to register your IP as a derivative of another,
  based on the License Terms defined in the token.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
> 🗒️ Contract
> 
> View the smart contract [here](https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/LicenseToken.sol).

A **License Token** is represented as an **ERC-721 NFT** and contains the specific [License Terms](doc:license-terms) it represents. Its associated `licenseTokenId` is global, as there is one License Token contract.

Once License Terms are attached to an IP Asset, it becomes public so that anyone can mint a License Token for those terms. A License Token is burned when it is used to register another IP as a derivative of the original IP Asset.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/2c2938f-Screenshot_2024-05-07_at_18.42.00.png",
        "",
        "A diagram showing what happens when a License Token is minted."
      ],
      "align": "center",
      "caption": "A diagram showing what happens when a License Token is minted."
    }
  ]
}
[/block]


## Private Licenses

In order to mint a private License Token, the owner of a root IP Asset can issue License Tokens that have terms **not yet attached to the IP Asset itself**. It is important to also note that derivative IP Assets cannot issue private licenses because it is restricted to only issue licenses of its inherited terms.

## Transferability of the License Token

License Tokens might be transferrable or not, depending on the values of the License Terms terms they point to.

Once a non-transferrable License Token is minted to a recipient, it is locked there forever.

## Registering a Derivative

There are two ways to register a derivative IP Asset.

> 📘 Small Note
> 
> An IP Asset can only register as a derivative one time. If an IP Asset has multiple parents, it must register both at the same time. Once an IP Asset is a derivative, it cannot link any more parents.

### 1. Using an Existing License Token

A License Token is burned when it is used to register another IP as a derivative of the original IP Asset.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/9bc3615-image.png",
        null,
        ""
      ],
      "align": "center"
    }
  ]
}
[/block]


### 2. Registering a Derivative Directly

You can also register a derivative directly, without the need for a License Token. Remember that if License Terms are attached to an IP Asset it is public to mint the License Token anyway, so this is simply a convenient way to go about it, thus skipping the middle step of minting a License Token.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/02181c4-Screenshot_2024-05-07_at_18.51.15.png",
        "",
        ""
      ],
      "align": "center"
    }
  ]
}
[/block]