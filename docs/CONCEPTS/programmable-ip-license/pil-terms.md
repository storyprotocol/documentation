---
title: PIL Terms
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
> 👍 Easy Mode: We Have Preset PIL Terms
> 
> [Check the PIL Flavors here](https://docs.story.foundation/docs/pil-flavors-preset-policy).

PIL is the first License Agreement for medial license developed by Story Protocol and inspired by [Token Bound License](https://james.grimmelmann.net/files/articles/token-bound-nft-license.pdf). If you haven't already, read the overview: [Programmable IP License (PIL💊)](doc:programmable-ip-license)

> 📘 PIL Legal Text
> 
> Check out the actual PIL legal text [here](https://github.com/storyprotocol/protocol-core-v1/blob/main/PIL_Testnet.pdf). It is very human readable for a legal text!

# On-chain terms

Most PIL terms are on-chain. They are implemented in the `IPILicenseTemplate` contract as a `PILTerms` struct [here](https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/interfaces/modules/licensing/IPILicenseTemplate.sol).

```sol IPILicenseTemplate.sol
/// @notice This struct defines the terms for a Programmable IP License (PIL).
/// These terms can be attached to IP Assets.
struct PILTerms {
  bool transferable;
  address royaltyPolicy;
  uint256 defaultMintingFee;
  uint256 expiration;
  bool commercialUse;
  bool commercialAttribution;
  address commercializerChecker;
  bytes commercializerCheckerData;
  uint32 commercialRevShare;
  uint256 commercialRevCeiling;
  bool derivativesAllowed;
  bool derivativesAttribution;
  bool derivativesApproval;
  bool derivativesReciprocal;
  uint256 derivativeRevCeiling;
  address currency;
  string uri;
}
```

## Descriptions

[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Values",
    "h-2": "Description",
    "0-0": "`transferable`",
    "0-1": "True/False",
    "0-2": "If false, the License Token cannot be transferred once it is minted to a recipient address.",
    "1-0": "`royaltyPolicy`",
    "1-1": "Address",
    "1-2": "The address of the royalty policy contract. The royalty policy must have been approved by Story Protocol in advance.",
    "2-0": "`defaultMintingFee`",
    "2-1": "\\#",
    "2-2": "The fee to be paid when minting a license.",
    "3-0": "`expiration`",
    "3-1": "\\#",
    "3-2": "The expiration period of the license.",
    "4-0": "`commercialUse`",
    "4-1": "True/False",
    "4-2": "You can make money from using the original IP Asset, subject to limitations below.",
    "5-0": "`commercialAttribution`",
    "5-1": "True/False",
    "5-2": "If true, people must give credit to the original work in their commercial application (eg. merch)",
    "6-0": "`commercializerChecker`",
    "6-1": "Address",
    "6-2": "Commercializers that are allowed to commercially exploit the original work. If zero address, then no restrictions are enforced.",
    "7-0": "`commercializerCheckerData`",
    "7-1": "Bytes",
    "7-2": "The data to be passed to the commercializer checker contract.",
    "8-0": "`commercialRevShare`",
    "8-1": "# [0-100,000,000]",
    "8-2": "Amount of revenue (from any source, original & derivative) that must be shared with the licensor (a value of 10,000,000 == 10% of revenue share).  \n  \nThis will collect all revenue from tokens that are whitelisted in the [RoyaltyModule.sol contract](https://github.com/storyprotocol/protocol-core-v1/blob/e339f0671c9172a6699537285e32aa45d4c1b57b/contracts/modules/royalty/RoyaltyModule.sol#L50).",
    "9-0": "`commercialRevCelling`",
    "9-1": "\\#",
    "9-2": "If `commercialUse` is set to true, this value determines the maximum revenue you can earn from the original work.",
    "10-0": "`derivativesAllowed`",
    "10-1": "True/False",
    "10-2": "Indicates whether the licensee can create derivatives of his work or not.",
    "11-0": "`derivativesAttribution`",
    "11-1": "True/False",
    "11-2": "If true, derivatives that are made must give credit to the original work.",
    "12-0": "`derivativesApproval`",
    "12-1": "True/False",
    "12-2": "If true, the licensor must approve derivatives of the work.",
    "13-0": "`derivativesReciprocal`",
    "13-1": "True/False",
    "13-2": "If true, derivatives of this derivative can be created indefinitely as long as they have the exact same terms.",
    "14-0": "`derivativeRevCelling`",
    "14-1": "\\#",
    "14-2": "If `commercialUse` is set to true, this value determines the maximum revenue you can earn from derivative works.",
    "15-0": "`currency`",
    "15-1": "Address",
    "15-2": "The ERC20 token to be used to pay the minting fee. The token must be registered in story protocol.",
    "16-0": "`uri`",
    "16-1": "String",
    "16-2": "The URI of the license terms, which can be used to fetch [off-chain license terms](https://docs.story.foundation/v1/docs/pil-for-devs-and-creators#off-chain-parameters-to-be-included-in-uri-field)."
  },
  "cols": 3,
  "rows": 17,
  "align": [
    null,
    "left",
    null
  ]
}
[/block]


# Off-chain terms to be included in `uri` field

Some PIL terms must be stored off-chain and passed in the `uri` field above. This is because these terms are often more lengthy and/or descriptive, so it would not make sense to store them on-chain.

[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Description",
    "0-0": "Territory",
    "0-1": "Limit usage of the IP to certain regions and/or countries.  \n  \nBy default, the IP can be used globally.",
    "1-0": "Channels of Distribution",
    "1-1": "Restrict usage of the IP to certain media formats and use in certain channels of distribution.  \n  \nBy default, the IP can be used across all possible channels of distribution.  \n  \nExamples: \"television\", \"physical consumer products\", \"video games\", etc.",
    "2-0": "Attribution",
    "2-1": "If the original author should be credited for usage of the IP.  \n  \nBy default, you do not need to provide credit to the original author.",
    "3-0": "Content Standards",
    "3-1": "Set content standards around use of the IP.  \n  \nBy default, no standards apply.  \n  \nExamples: \"No-Hate\", \"Suitable-for-All-Ages\", \"No-Drugs-or-Weapons\", \"No-Pornography\".",
    "4-0": "Sublicensable",
    "4-1": "Derivative works can grant the same rights they received under this license to a 3rd party, without approval from the original licensor.  \n  \nBy default, derivatives may not do so.",
    "5-0": "AI Learning Models",
    "5-1": "Whether or not the IP can be used to develop AI learning models.  \n  \nBy default, the IP can be used for such development.",
    "6-0": "Restriction On Cross-Platform Use",
    "6-1": "Limit licensing and creation of derivative works solely on the app on which the IP is made available.  \n  \nBy default, the IP can be used anywhere.",
    "7-0": "Governing Law",
    "7-1": "The laws of a certain jurisdiction by which this license abides.  \n  \nBy default, this is California, USA.",
    "8-0": "Alternative Dispute Resolution",
    "8-1": "Please see section 3.1 (s) [here](https://github.com/storyprotocol/protocol-core-v1/blob/main/PIL_Beta_Final_2024_02_Plain_English.pdf). ",
    "9-0": "Additional License Parameters",
    "9-1": "There may be other terms the licensor would like to add and they can do so in this tag."
  },
  "cols": 2,
  "rows": 10,
  "align": [
    "left",
    "left"
  ]
}
[/block]