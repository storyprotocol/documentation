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
      <th style={{ textAlign: "left" }}>
        Others can
      </th>

      <th style={{ textAlign: "left" }}>
        Others cannot
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        ✅ Remix this work
        (`derivativesAllowed == true`)
      </td>

      <td style={{ textAlign: "left" }}>
        ❌ Commercialize the original and derivative works
        (`commercialUse == false`)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        ✅ Distribute their remix anywhere
      </td>

      <td style={{ textAlign: "left" }}>
        :x: Claim credit for any derivative works
        (`derivativesAttribution == true`)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        :white_check_mark: Get the license for free\
        (`defaultMintingFee == 0`)
      </td>

      <td style={{ textAlign: "left" }}>
        :x: Claim credit for the original work\
        ("Attribution" is true in the off-chain terms)
      </td>
    </tr>
  </tbody>
</Table>

###  PIL Term Values

* **On-chain**:

```sol Solidity
PILTerms({
  transferable: true,
  royaltyPolicy: zeroAddress,
  defaultMintingFee: BigInt(0),
  expiration: BigInt(0),
  commercialUse: false,
  commercialAttribution: false,
  commercializerChecker: zeroAddress,
  commercializerCheckerData: "0x",
  commercialRevShare: 0,
  commercialRevCeiling: BigInt(0),
  derivativesAllowed: true,
  derivativesAttribution: true,
  derivativesApproval: false,
  derivativesReciprocal: true,
  derivativeRevCeiling: BigInt(0),
  currency: zeroAddress,
  uri: "",
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
      <th style={{ textAlign: "left" }}>
        Others can
      </th>

      <th style={{ textAlign: "left" }}>
        Others cannot
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        ✅ Remix this work
        (`derivativesAllowed == true`)
      </td>

      <td style={{ textAlign: "left" }}>
        ❌ Claim credit for the original work
        (`commercialAttribution == true`)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        ✅ Commercialize the original and derivative works
        (`commercialUse == true`)
      </td>

      <td style={{ textAlign: "left" }}>
        :x: Claim credit for any derivative works
        (`derivativesAttribution == true`)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        ✅ Distribute their remix anywhere
      </td>

      <td style={{ textAlign: "left" }}>
        :x: Get the license for free\
        (`defaultMintingFee` is set)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        :white_check_mark: Keep all revenue\
        (`commercialRevShare == 0`)
      </td>

      <td style={{ textAlign: "left" }}>
        :x: Claim credit for the original work even non-commercially\
        ("Attribution is true in the off-chain terms)
      </td>
    </tr>
  </tbody>
</Table>

###  PIL Term Values

* **On-chain**:

```sol Solidity
PILTerms({
  transferable: true,
  royaltyPolicy: ROYALTY_POLICY, // ex. RoyaltyPolicyLAP address
  defaultMintingFee: BigInt(100), // ex. costs 100 $WIP to mint
  expiration: BigInt(0),
  commercialUse: true,
  commercialAttribution: true,
  commercializerChecker: zeroAddress,
  commercializerCheckerData: zeroAddress,
  commercialRevShare: 0,
  commercialRevCeiling: BigInt(0),
  derivativesAllowed: true,
  derivativesAttribution: true,
  derivativesApproval: false,
  derivativesReciprocal: false,
  derivativeRevCeiling: BigInt(0),
  currency: CURRENCY, // ex. $WIP address
  uri: "",
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

### What others can do?

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        Others can
      </th>

      <th style={{ textAlign: "left" }}>
        Others cannot
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        ✅ Remix this work
        (`derivativesAllowed == true`)
      </td>

      <td style={{ textAlign: "left" }}>
        ❌ Claim credit for the original work
        (`commercialAttribution == true`)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        ✅ Commercialize the original and derivative works
        (`commercialUse == true`)
      </td>

      <td style={{ textAlign: "left" }}>
        :x: Claim credit for any derivative works
        (`derivativesAttribution == true`)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        ✅ Distribute their remix anywhere
      </td>

      <td style={{ textAlign: "left" }}>
        ❌ Keep all revenue\
        (`commercialRevShare` is set)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>

      </td>

      <td style={{ textAlign: "left" }}>
        :x: Get the license for free\
        (`defaultMintingFee` is set)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>

      </td>

      <td style={{ textAlign: "left" }}>
        :x: Claim credit for the original work even non-commercially\
        ("Attribution is true in the off-chain terms)
      </td>
    </tr>
  </tbody>
</Table>

###  PIL Term Values

* **On-chain**:

```sol Solidity
PILTerms({
  transferable: true,
  royaltyPolicy: ROYALTY_POLICY, // ex. RoyaltyPolicyLAP address
  defaultMintingFee: BigInt(100), // ex. costs 100 $WIP to mint
  expiration: BigInt(0),
  commercialUse: true,
  commercialAttribution: true,
  commercializerChecker: zeroAddress,
  commercializerCheckerData: zeroAddress,
  commercialRevShare: 50, // ex. can claim 50% of derivative revenue
  commercialRevCeiling: BigInt(0),
  derivativesAllowed: true,
  derivativesAttribution: true,
  derivativesApproval: false,
  derivativesReciprocal: true,
  derivativeRevCeiling: BigInt(0),
  currency: CURRENCY, // ex. $WIP address
  uri: "",
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
      <th style={{ textAlign: "left" }}>
        Others can
      </th>

      <th style={{ textAlign: "left" }}>
        Others cannot
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        ✅ Remix this work
        (`derivativesAllowed == true`)
      </td>

      <td style={{ textAlign: "left" }}>
        ❌ Claim credit for the original work
        (`commercialAttribution == true`)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        ✅ Commercialize the original and derivative works
        (`commercialUse == true`)
      </td>

      <td style={{ textAlign: "left" }}>
        :x: Claim credit for any derivative works
        (`derivativesAttribution == true`)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        ✅ Distribute their remix anywhere
      </td>

      <td style={{ textAlign: "left" }}>
        :x: Claim credit for the original work even non-commercially\
        ("Attribution is true in the off-chain terms)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        :white_check_mark: Get the license for free\
        (`defaultMintingFee == 0`)
      </td>

      <td style={{ textAlign: "left" }}>

      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        :white_check_mark: Keep all revenue\
        (`commercialRevShare == 0`)
      </td>

      <td style={{ textAlign: "left" }}>

      </td>
    </tr>
  </tbody>
</Table>

###  PIL Term Values

* **On-chain**:

```sol Solidity
PILTerms({
  transferable: true,
  royaltyPolicy: zeroAddress,
  defaultMintingFee: 0,
  expiration: 0,
  commercialUse: true,
  commercialAttribution: true,
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

<Image align="center" src="https://files.readme.io/574c9f3-Screenshot_2024-08-16_at_9.54.00_PM.png" />

### Explanation

Someone registers their Azuki on Story. By default, that IP Asset has Non-Commercial Social Remixing Terms, which specify that anyone can create derivatives of that work but cannot commercialize them. So, someone else creates & registers a remix of that work (IPA2) which inherits those same terms. Someone else then does the same to IPA2, creating & registering IPA3.

The owner of IPA1 then decides that others can commercialize the work, but they cannot create derivatives to do so, they must pay a 10 USDC minting fee, and they must share 10% of all revenue earned. So, someone wants to commercialize IPA1 by putting it on a t-shirt. They pay the 10 USDC minting fee to get a License Token, which represents the license to commercialize IPA1. They then put the image on a t-shirt and sell it. 10% of revenue earned by that t-shirt must be sent on-chain to IPA1.

## Example 2

<Image align="center" src="https://files.readme.io/e3c7fbf-Screenshot_2024-08-16_at_9.54.16_PM.png" />

### Explanation

Someone registers their Azuki on Story. By default, that IP Asset has Non-Commercial Social Remixing Terms, which specify that anyone can create derivatives of that work but cannot commercialize them. So, someone else creates & registers a remix of that work (IPA2) which inherits those same terms. Someone else then does the same to IPA2, creating & registering IPA3.

The owner of IPA1 then decides that others can create derivatives of their work and commercialize them, but they must pay a 10 USDC minting fee and share 10% of all revenue earned. So, someone wants to commercialize IPA1 by putting it on a t-shirt. They pay the 10 USDC minting fee to get a License Token and burn it to create their own derivative, which changes the background color to red. They then put the remixed image on a t-shirt and sell it. 10% of revenue earned by that t-shirt must be sent on-chain to IPA1.

A third person wants to commercialize the remix by putting it in a TV advertisement, but they want to change the hair color to white. So, they pay a 10 USDC minting fee (of which, 1 USDC gets sent back to IPA1) to create their own derivative. They then put the remixed image in a TV ad. 10% of revenue earned by that t-shirt must be sent on-chain to IPA4, of which 10% will be distributed back to IPA1.