---
title: PIL Flavors (examples)
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
PIL is very configurable, but we support pre-configured License Terms for ease of use. We expect these to be the most popular options:

## Flavor #1: Non-Commercial Social Remixing

_This flavor is already registered as `licenseTermsId = 1` on Story. **In addition, every single IP Asset has these terms attached by default.**_

Let the world build on and play with your creation. This license allows for endless free remixing while tracking all uses of your work while giving you full credit. Similar to: TikTok plus attribution.

### What others can do?

[block:parameters]
{
  "data": {
    "h-0": "Others can",
    "h-1": "Others cannot",
    "0-0": "✅ Remix this work  \n(`derivativesAllowed == true`)",
    "0-1": "❌ Commercialize the original and derivative works  \n(`commercialUse == false`)",
    "1-0": "✅ Distribute their remix anywhere",
    "1-1": "❌ Claim credit for the remix as original work  \n(`derivativesAttribution == true`)",
    "2-0": "✅ Credit you appropriately  \n(`derivativesAttribution == true`)",
    "2-1": ""
  },
  "cols": 2,
  "rows": 3,
  "align": [
    "left",
    "left"
  ]
}
[/block]


###  PIL Term Values

- **On-chain**:

```sol Solidity
PILTerms({
  transferable: true,
  royaltyPolicy: zeroAddress,
  defaultMintingFee: 0,
  expiration: 0,
  commercialUse: false,
  commercialAttribution: false,
  commercializerChecker: zeroAddress,
  commercializerCheckerData: EMPTY_BYTES,
  commercialRevShare: 0,
  commercialRevCelling: 0,
  derivativesAllowed: true,
  derivativesAttribution: true,
  derivativesApproval: false,
  derivativesReciprocal: true,
  derivativeRevCelling: 0,
  currency: zeroAddress,
  uri: ''
});
```

- **Off-chain:**

| Parameter                         | Options / Tags                                                              |
| --------------------------------- | --------------------------------------------------------------------------- |
| Territory                         | No restrictions                                                             |
| Channels of Distribution          | No Restriction                                                              |
| Attribution                       | True                                                                        |
| Content Standards                 | No Restriction                                                              |
| Sublicensable                     | False                                                                       |
| AI Learning Models                | True                                                                        |
| Restriction on Cross-Platform Use | False                                                                       |
| Governing Law                     | California                                                                  |
| Alternative Dispute Resolution    | Tag: Alternative-Dispute-Resolution Ledger-Authoritative-Dispute-Resolution |
| Additional License Parameters     | None                                                                        |

## Flavor #2: Commercial Use

Retain control over reuse of your work, while allowing anyone to appropriately use the work in exchange for the economic terms you set. This is similar to Shutterstock with creator-set rules.

### What others can do?

[block:parameters]
{
  "data": {
    "h-0": "Others can",
    "h-1": "Others cannot",
    "0-0": "✅ Purchase the right to use your creation  \n(`defaultMintingFee` is set)",
    "0-1": "❌ Claim credit for the original work  \n(`commercialAttribution == true`)",
    "1-0": "✅ Commercialize the original and derivative works  \n(`commercialUse == true`)",
    "1-1": "",
    "2-0": "✅ Distribute their remix anywhere",
    "2-1": ""
  },
  "cols": 2,
  "rows": 3,
  "align": [
    "left",
    "left"
  ]
}
[/block]


###  PIL Term Values

- **On-chain**:

```sol Solidity
PILTerms({
  transferable: true,
  royaltyPolicy: royaltyPolicy,
  defaultMintingFee: mintingFee,
  expiration: 0,
  commercialUse: true,
  commercialAttribution: true,
  commercializerChecker: zeroAddress,
  commercializerCheckerData: EMPTY_BYTES,
  commercialRevShare: 0,
  commercialRevCelling: 0,
  derivativesAllowed: false,
  derivativesAttribution: false,
  derivativesApproval: false,
  derivativesReciprocal: false,
  derivativeRevCelling: 0,
  currency: currencyToken,
  uri: ''
})
```

- **Off-chain**

| Parameter                         | Options / Tags                                                              |
| --------------------------------- | --------------------------------------------------------------------------- |
| Territory                         | No restrictions                                                             |
| Channels of Distribution          | No Restriction                                                              |
| Attribution                       | True                                                                        |
| Content Standards                 | No Restriction                                                              |
| Sublicensable                     | False                                                                       |
| AI Learning Models                | True                                                                        |
| Restriction on Cross-Platform Use | False                                                                       |
| Governing Law                     | California                                                                  |
| Alternative Dispute Resolution    | Tag: Alternative-Dispute-Resolution Ledger-Authoritative-Dispute-Resolution |
| Additional License Parameters     | None                                                                        |

## Flavor #3: Commercial Remix

Let the world build on and play with your creation... and earn money together from it! This license allows for endless free remixing while tracking all uses of your work while giving you full credit, with each derivative paying a percentage of revenue to its "parent" IP.

### What others can do?

[block:parameters]
{
  "data": {
    "h-0": "Others can",
    "h-1": "Others cannot",
    "0-0": "✅ Remix this work  \n(`derivativesAllowed == true`)",
    "0-1": "❌ Claim credit for the remix as original work  \n(`derivativesAttribution == true`)",
    "1-0": "✅ Distribute their remix anywhere",
    "1-1": "❌ Claim credit for the original work  \n(`commercialAttribution == true`)",
    "2-0": "✅ Credit you appropriately  \n(`derivativesAttribution == true`)",
    "2-1": "❌ Claim all the revenue from commercial use of the original work or derivative works  \n(`commercialRevShare` is set)",
    "3-0": "✅ Commercialize the original and derivative works  \n(`commercialUse == true`)",
    "3-1": ""
  },
  "cols": 2,
  "rows": 4,
  "align": [
    "left",
    "left"
  ]
}
[/block]


###  PIL Term Values

- **On-chain**:

```sol Solidity
PILTerms({
  transferable: true,
  royaltyPolicy: royaltyPolicy,
  defaultMintingFee: 0,
  expiration: 0,
  commercialUse: true,
  commercialAttribution: true,
  commercializerChecker: zeroAddress,
  commercializerCheckerData: EMPTY_BYTES,
  commercialRevShare: commercialRevShare,
  commercialRevCelling: 0,
  derivativesAllowed: true,
  derivativesAttribution: true,
  derivativesApproval: false,
  derivativesReciprocal: true,
  derivativeRevCelling: 0,
  currency: zeroAddress,
  uri: ''
});
```

- **Off-chain**

| Parameter                         | Options / Tags                                                              |
| --------------------------------- | --------------------------------------------------------------------------- |
| Territory                         | No restrictions                                                             |
| Channels of Distribution          | No Restriction                                                              |
| Attribution                       | True                                                                        |
| Content Standards                 | No Restriction                                                              |
| Sublicensable                     | False                                                                       |
| AI Learning Models                | True                                                                        |
| Restriction on Cross-Platform Use | False                                                                       |
| Governing Law                     | California                                                                  |
| Alternative Dispute Resolution    | Tag: Alternative-Dispute-Resolution Ledger-Authoritative-Dispute-Resolution |
| Additional License Parameters     | None                                                                        |

# Examples

Here are some common examples of royalty flow. _More coming soon!_

## Example 1

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/574c9f3-Screenshot_2024-08-16_at_9.54.00_PM.png",
        "",
        ""
      ],
      "align": "center"
    }
  ]
}
[/block]


### Explanation

Someone registers their Azuki on Story. By default, that IP Asset has Non-Commercial Social Remixing Terms, which specify that anyone can create derivatives of that work but cannot commercialize them. So, someone else creates & registers a remix of that work (IPA2) which inherits those same terms. Someone else then does the same to IPA2, creating & registering IPA3.

The owner of IPA1 then decides that others can commercialize the work, but they cannot create derivatives to do so, they must pay a 10 USDC minting fee, and they must share 10% of all revenue earned. So, someone wants to commercialize IPA1 by putting it on a t-shirt. They pay the 10 USDC minting fee to get a License Token, which represents the license to commercialize IPA1. They then put the image on a t-shirt and sell it. 10% of revenue earned by that t-shirt must be sent on-chain to IPA1.

## Example 2

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/e3c7fbf-Screenshot_2024-08-16_at_9.54.16_PM.png",
        "",
        ""
      ],
      "align": "center"
    }
  ]
}
[/block]


### Explanation

Someone registers their Azuki on Story. By default, that IP Asset has Non-Commercial Social Remixing Terms, which specify that anyone can create derivatives of that work but cannot commercialize them. So, someone else creates & registers a remix of that work (IPA2) which inherits those same terms. Someone else then does the same to IPA2, creating & registering IPA3.

The owner of IPA1 then decides that others can create derivatives of their work and commercialize them, but they must pay a 10 USDC minting fee and share 10% of all revenue earned. So, someone wants to commercialize IPA1 by putting it on a t-shirt. They pay the 10 USDC minting fee to get a License Token and burn it to create their own derivative, which changes the background color to red. They then put the remixed image on a t-shirt and sell it. 10% of revenue earned by that t-shirt must be sent on-chain to IPA1.

A third person wants to commercialize the remix by putting it in a TV advertisement, but they want to change the hair color to white. So, they pay a 10 USDC minting fee (of which, 1 USDC gets sent back to IPA1) to create their own derivative. They then put the remixed image in a TV ad. 10% of revenue earned by that t-shirt must be sent on-chain to IPA4, of which 10% will be distributed back to IPA1.