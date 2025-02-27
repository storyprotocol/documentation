---
title: PIL Flavors (examples)
excerpt: Pre-configured License Terms for ease of use.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
The [💊 Programmable IP License (PIL)](doc:programmable-ip-license) is very configurable, but we support popular pre-configured License Terms (also known as "flavors") for ease of use. We expect these to be the most popular options:

## Flavor #1: Non-Commercial Social Remixing

> 📘 Already Registered
>
> This flavor is already registered as `licenseTermsId = 1` on our protocol. This is because it doesn't take any inputs, so we registered it ahead of time.

Let the world build on and play with your creation. This license allows for endless free remixing while tracking all uses of your work while giving you full credit. Similar to: TikTok plus attribution.

### What others can do?

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th>
        Others can
      </th>

      <th>
        Others cannot
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        ✅ Remix this work
        (`derivativesAllowed == true`)
      </td>

      <td>
        ❌ Commercialize the original and derivative works
        (`commercialUse == false`)
      </td>
    </tr>

    <tr>
      <td>
        ✅ Distribute their remix anywhere
      </td>

      <td>
        :x: Claim credit for any derivative works
        (`derivativesAttribution == true`)
      </td>
    </tr>

    <tr>
      <td>
        :white_check_mark: Get the license for free\
        (`defaultMintingFee == 0`)
      </td>

      <td>
        :x: Claim credit for the original work\
        ("Attribution" is true in the off-chain terms)
      </td>
    </tr>
  </tbody>
</Table>

### PIL Term Values

* **On-chain**:

```sol Solidity
PILTerms({
  transferable: true,
  royaltyPolicy: address(0),
  defaultMintingFee: 0,
  expiration: 0,
  commercialUse: false,
  commercialAttribution: false,
  commercializerChecker: address(0),
  commercializerCheckerData: EMPTY_BYTES,
  commercialRevShare: 0,
  commercialRevCeiling: 0,
  derivativesAllowed: true,
  derivativesAttribution: true,
  derivativesApproval: false,
  derivativesReciprocal: true,
  derivativeRevCeiling: 0,
  currency: address(0),
  uri: "https://github.com/piplabs/pil-document/blob/998c13e6ee1d04eb817aefd1fe16dfe8be3cd7a2/off-chain-terms/NCSR.json",
});
```

* **Off-chain:**

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

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th>
        Others can
      </th>

      <th>
        Others cannot
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        ✅ Commercialize the original and derivative works\\
        (`commercialUse == true`)
      </td>

      <td>
        :x: Remix this work\\
        (`derivativesAllowed == false`)
      </td>
    </tr>

    <tr>
      <td>
        ✅ Distribute their remix anywhere
      </td>

      <td>
        ❌ Claim credit for the original work\
        (`commercialAttribution == true`)
      </td>
    </tr>

    <tr>
      <td>
        :white_check_mark: Keep all revenue\\\
        (`commercialRevShare == 0`)
      </td>

      <td>
        :x: Claim credit for any derivative works\
        (`derivativesAttribution == true`)
      </td>
    </tr>

    <tr>
      <td>

      </td>

      <td>
        :x: Get the license for free\
        (`defaultMintingFee` is set)
      </td>
    </tr>

    <tr>
      <td>

      </td>

      <td>
        :x: Claim credit for the original work even non-commercially\
        ("Attribution is true in the off-chain terms)
      </td>
    </tr>
  </tbody>
</Table>

### PIL Term Values

* **On-chain**:

```sol Solidity
PILTerms({
  transferable: true,
  royaltyPolicy: ROYALTY_POLICY, // ex. RoyaltyPolicyLAP address
  defaultMintingFee: MINTING_FEE, // ex. costs 100 $WIP to mint
  expiration: 0,
  commercialUse: true,
  commercialAttribution: true,
  commercializerChecker: address(0),
  commercializerCheckerData: EMPTY_BYTES,
  commercialRevShare: 0,
  commercialRevCeiling: 0,
  derivativesAllowed: false,
  derivativesAttribution: false,
  derivativesApproval: false,
  derivativesReciprocal: false,
  derivativeRevCeiling: 0,
  currency: CURRENCY, // ex. $WIP address
  uri: "https://github.com/piplabs/pil-document/blob/9a1f803fcf8101a8a78f1dcc929e6014e144ab56/off-chain-terms/CommercialUse.json",
})
```

* **Off-chain**

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

### Example

Check out Story's official mascot **Ippy**, which we have registered with commercial remix terms on both [Mainnet](https://explorer.story.foundation/ipa/0xB1D831271A68Db5c18c8F0B69327446f7C8D0A42) and [Aeneid Testnet](https://aeneid.explorer.story.foundation/ipa/0x641E638e8FCA4d4844F509630B34c9D524d40BE5).

### What others can do?

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th>
        Others can
      </th>

      <th>
        Others cannot
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        ✅ Remix this work
        (`derivativesAllowed == true`)
      </td>

      <td>
        ❌ Claim credit for the original work
        (`commercialAttribution == true`)
      </td>
    </tr>

    <tr>
      <td>
        ✅ Commercialize the original and derivative works
        (`commercialUse == true`)
      </td>

      <td>
        :x: Claim credit for any derivative works
        (`derivativesAttribution == true`)
      </td>
    </tr>

    <tr>
      <td>
        ✅ Distribute their remix anywhere
      </td>

      <td>
        ❌ Keep all revenue\
        (`commercialRevShare` is set)
      </td>
    </tr>

    <tr>
      <td>

      </td>

      <td>
        :x: Get the license for free\
        (`defaultMintingFee` is set)
      </td>
    </tr>

    <tr>
      <td>

      </td>

      <td>
        :x: Claim credit for the original work even non-commercially\
        ("Attribution is true in the off-chain terms)
      </td>
    </tr>
  </tbody>
</Table>

### PIL Term Values

* **On-chain**:

```sol Solidity
PILTerms({
  transferable: true,
  royaltyPolicy: ROYALTY_POLICY, // ex. RoyaltyPolicyLAP address
  defaultMintingFee: MINTING_FEE, // ex. costs 100 $WIP to mint
  expiration: 0,
  commercialUse: true,
  commercialAttribution: true,
  commercializerChecker: address(0),
  commercializerCheckerData: EMPTY_BYTES,
  commercialRevShare: COMMERCIAL_REV_SHARE, // ex. 50 * 10 ** 6 (which means 50% of derivative revenue)
  commercialRevCeiling: 0,
  derivativesAllowed: true,
  derivativesAttribution: true,
  derivativesApproval: false,
  derivativesReciprocal: true,
  derivativeRevCeiling: 0,
  currency: CURRENCY, // ex. $WIP address
  uri: "https://github.com/piplabs/pil-document/blob/ad67bb632a310d2557f8abcccd428e4c9c798db1/off-chain-terms/CommercialRemix.json",
});
```

* **Off-chain**

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

## Flavor #4: Creative Commons Attribution

Let the world build on and play with your creation - including making money.

### What others can do?

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th>
        Others can
      </th>

      <th>
        Others cannot
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        ✅ Remix this work
        (`derivativesAllowed == true`)
      </td>

      <td>
        ❌ Claim credit for the original work
        (`commercialAttribution == true`)
      </td>
    </tr>

    <tr>
      <td>
        ✅ Commercialize the original and derivative works
        (`commercialUse == true`)
      </td>

      <td>
        :x: Claim credit for any derivative works
        (`derivativesAttribution == true`)
      </td>
    </tr>

    <tr>
      <td>
        ✅ Distribute their remix anywhere
      </td>

      <td>
        :x: Claim credit for the original work even non-commercially\
        ("Attribution is true in the off-chain terms)
      </td>
    </tr>

    <tr>
      <td>
        :white_check_mark: Get the license for free\
        (`defaultMintingFee == 0`)
      </td>

      <td>

      </td>
    </tr>

    <tr>
      <td>
        :white_check_mark: Keep all revenue\
        (`commercialRevShare == 0`)
      </td>

      <td>

      </td>
    </tr>
  </tbody>
</Table>

### PIL Term Values

* **On-chain**:

```sol Solidity
PILTerms({
  transferable: true,
  royaltyPolicy: ROYALTY_POLICY, // ex. RoyaltyPolicyLAP address
  defaultMintingFee: 0,
  expiration: 0,
  commercialUse: true,
  commercialAttribution: true,
  commercializerChecker: address(0),
  commercializerCheckerData: EMPTY_BYTES,
  commercialRevShare: 0,
  commercialRevCelling: 0,
  derivativesAllowed: true,
  derivativesAttribution: true,
  derivativesApproval: false,
  derivativesReciprocal: true,
  derivativeRevCelling: 0,
  currency: CURRENCY, // ex. $WIP address
  uri: 'https://github.com/piplabs/pil-document/blob/998c13e6ee1d04eb817aefd1fe16dfe8be3cd7a2/off-chain-terms/CC-BY.json'
});
```

* **Off-chain**

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

Here are some common examples of royalty flow. *More coming soon!*

## Example 1

<Image align="center" src="https://files.readme.io/01710d13ebea0b9946a65dca0d5948b4a70d6ee6e5a3fbdbb722f74b588a6e42-Screenshot_2025-02-12_at_9.24.01_PM.png" />

### Explanation

Someone registers their Azuki on Story. By default, that IP Asset has Non-Commercial Social Remixing Terms, which specify that anyone can create derivatives of that work but cannot commercialize them. So, someone else creates & registers a remix of that work (IPA2) which inherits those same terms. Someone else then does the same to IPA2, creating & registering IPA3.

The owner of IPA1 then decides that others can commercialize the work, but they cannot create derivatives to do so, they must pay a 10 $WIP minting fee, and they must share 10% of all revenue earned. So, someone wants to commercialize IPA1 by putting it on a t-shirt. They pay the 10 $WIP minting fee to get a License Token, which represents the license to commercialize IPA1. They then put the image on a t-shirt and sell it. 10% of revenue earned by that t-shirt must be sent on-chain to IPA1.

## Example 2

<Image align="center" src="https://files.readme.io/9763a4efe56b88e8a29531e11341538ccd040a8c7d0a786fee8d1d237a2e7a85-Screenshot_2025-02-12_at_9.24.31_PM.png" />

### Explanation

Someone registers their Azuki on Story. By default, that IP Asset has Non-Commercial Social Remixing Terms, which specify that anyone can create derivatives of that work but cannot commercialize them. So, someone else creates & registers a remix of that work (IPA2) which inherits those same terms. Someone else then does the same to IPA2, creating & registering IPA3.

The owner of IPA1 then decides that others can create derivatives of their work and commercialize them, but they must pay a 10 $WIP minting fee and share 10% of all revenue earned. So, someone wants to commercialize IPA1 by putting it on a t-shirt. They pay the 10 $WIP minting fee to get a License Token and burn it to create their own derivative, which changes the background color to red. They then put the remixed image on a t-shirt and sell it. 10% of revenue earned by that t-shirt must be sent on-chain to IPA1.

A third person wants to commercialize the remix by putting it in a TV advertisement, but they want to change the hair color to white. So, they pay a 10 $WIP minting fee (of which, 1 $WIP gets sent back to IPA1) to create their own derivative. They then put the remixed image in a TV ad. 10% of revenue earned by that t-shirt must be sent on-chain to IPA4, of which 10% will be distributed back to IPA1.