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
<Cards columns={3}>
  <Card title="Read the Overview" href="https://docs.story.foundation/docs/programmable-ip-license" icon="fa-pills" iconColor="yellow">
    If you haven't already, read the Programmable IP License (PIL💊) overview.
  </Card>

  <Card title="Preset PIL Terms" href="https://docs.story.foundation/docs/pil-flavors" icon="fa-thumbs-up" iconColor="#51af51">
    Since there are so many possible combinations of the PIL, we have created preset "flavors" for you to use while developing.
  </Card>

  <Card title="PIL Legal Text" href="https://github.com/storyprotocol/protocol-core-v1/blob/main/PIL_Testnet.pdf" icon="fa-scroll" iconColor="#ccb092" target="_blank">
    Check out the actual PIL legal text. It is very human-readable for a legal text!
  </Card>
</Cards>

# On-chain terms

Most PIL terms are on-chain. They are implemented in the `IPILicenseTemplate.sol` contract as a `PILTerms` struct [here](https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/interfaces/modules/licensing/IPILicenseTemplate.sol).

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

<Table align={[null,"left",null]}>
  <thead>
    <tr>
      <th>
        Parameter
      </th>

      <th style={{ textAlign: "left" }}>
        Values
      </th>

      <th>
        Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        `transferable`
      </td>

      <td style={{ textAlign: "left" }}>
        True/False
      </td>

      <td>
        If false, the License Token cannot be transferred once it is minted to a recipient address.
      </td>
    </tr>

    <tr>
      <td>
        `royaltyPolicy`
      </td>

      <td style={{ textAlign: "left" }}>
        Address
      </td>

      <td>
        The address of the royalty policy contract. The royalty policy must have been approved by Story Protocol in advance.
      </td>
    </tr>

    <tr>
      <td>
        `defaultMintingFee`
      </td>

      <td style={{ textAlign: "left" }}>
        \#
      </td>

      <td>
        The fee to be paid when minting a license.
      </td>
    </tr>

    <tr>
      <td>
        `expiration`
      </td>

      <td style={{ textAlign: "left" }}>
        \#
      </td>

      <td>
        The expiration period of the license.
      </td>
    </tr>

    <tr>
      <td>
        `commercialUse`
      </td>

      <td style={{ textAlign: "left" }}>
        True/False
      </td>

      <td>
        You can make money from using the original IP Asset, subject to limitations below.
      </td>
    </tr>

    <tr>
      <td>
        `commercialAttribution`
      </td>

      <td style={{ textAlign: "left" }}>
        True/False
      </td>

      <td>
        If true, people must give credit to the original work in their commercial application (eg. merch)
      </td>
    </tr>

    <tr>
      <td>
        `commercializerChecker`
      </td>

      <td style={{ textAlign: "left" }}>
        Address
      </td>

      <td>
        Commercializers that are allowed to commercially exploit the original work. If zero address, then no restrictions are enforced.
      </td>
    </tr>

    <tr>
      <td>
        `commercializerCheckerData`
      </td>

      <td style={{ textAlign: "left" }}>
        Bytes
      </td>

      <td>
        The data to be passed to the commercializer checker contract.
      </td>
    </tr>

    <tr>
      <td>
        `commercialRevShare`
      </td>

      <td style={{ textAlign: "left" }}>
        # \[0-100,000,000]
      </td>

      <td>
        Amount of revenue (from any source, original & derivative) that must be shared with the licensor (a value of 10,000,000 == 10% of revenue share).

        This will collect all revenue from tokens that are whitelisted in the [RoyaltyModule.sol contract](https://github.com/storyprotocol/protocol-core-v1/blob/e339f0671c9172a6699537285e32aa45d4c1b57b/contracts/modules/royalty/RoyaltyModule.sol#L50).
      </td>
    </tr>

    <tr>
      <td>
        `commercialRevCelling`
      </td>

      <td style={{ textAlign: "left" }}>
        \#
      </td>

      <td>
        If `commercialUse` is set to true, this value determines the maximum revenue you can earn from the original work.
      </td>
    </tr>

    <tr>
      <td>
        `derivativesAllowed`
      </td>

      <td style={{ textAlign: "left" }}>
        True/False
      </td>

      <td>
        Indicates whether the licensee can create derivatives of his work or not.
      </td>
    </tr>

    <tr>
      <td>
        `derivativesAttribution`
      </td>

      <td style={{ textAlign: "left" }}>
        True/False
      </td>

      <td>
        If true, derivatives that are made must give credit to the original work.
      </td>
    </tr>

    <tr>
      <td>
        `derivativesApproval`
      </td>

      <td style={{ textAlign: "left" }}>
        True/False
      </td>

      <td>
        If true, the licensor must approve derivatives of the work.
      </td>
    </tr>

    <tr>
      <td>
        `derivativesReciprocal`
      </td>

      <td style={{ textAlign: "left" }}>
        True/False
      </td>

      <td>
        If true, derivatives of this derivative can be created indefinitely as long as they have the exact same terms.
      </td>
    </tr>

    <tr>
      <td>
        `derivativeRevCelling`
      </td>

      <td style={{ textAlign: "left" }}>
        \#
      </td>

      <td>
        If `commercialUse` is set to true, this value determines the maximum revenue you can earn from derivative works.
      </td>
    </tr>

    <tr>
      <td>
        `currency`
      </td>

      <td style={{ textAlign: "left" }}>
        Address
      </td>

      <td>
        The ERC20 token to be used to pay the minting fee. The token must be registered in Story Protocol.
      </td>
    </tr>

    <tr>
      <td>
        `uri`
      </td>

      <td style={{ textAlign: "left" }}>
        String
      </td>

      <td>
        The URI of the license terms, which can be used to fetch [off-chain license terms](https://docs.story.foundation/v1/docs/pil-for-devs-and-creators#off-chain-parameters-to-be-included-in-uri-field).
      </td>
    </tr>
  </tbody>
</Table>

# Off-chain terms to be included in `uri` field

Some PIL terms must be stored off-chain and passed in the `uri` field above. This is because these terms are often more lengthy and/or descriptive, so it would not make sense to store them on-chain.

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        Parameter
      </th>

      <th style={{ textAlign: "left" }}>
        Description
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        Territory
      </td>

      <td style={{ textAlign: "left" }}>
        Limit usage of the IP to certain regions and/or countries.

        By default, the IP can be used globally.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Channels of Distribution
      </td>

      <td style={{ textAlign: "left" }}>
        Restrict usage of the IP to certain media formats and use in certain channels of distribution.

        By default, the IP can be used across all possible channels of distribution.

        Examples: "television", "physical consumer products", "video games", etc.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Attribution
      </td>

      <td style={{ textAlign: "left" }}>
        If the original author should be credited for usage of the IP.

        By default, you do not need to provide credit to the original author.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Content Standards
      </td>

      <td style={{ textAlign: "left" }}>
        Set content standards around use of the IP.

        By default, no standards apply.

        Examples: "No-Hate", "Suitable-for-All-Ages", "No-Drugs-or-Weapons", "No-Pornography".
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Sublicensable
      </td>

      <td style={{ textAlign: "left" }}>
        Derivative works can grant the same rights they received under this license to a 3rd party, without approval from the original licensor.

        By default, derivatives may not do so.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        AI Learning Models
      </td>

      <td style={{ textAlign: "left" }}>
        Whether or not the IP can be used to develop AI learning models.

        By default, the IP can be used for such development.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Restriction On Cross-Platform Use
      </td>

      <td style={{ textAlign: "left" }}>
        Limit licensing and creation of derivative works solely on the app on which the IP is made available.

        By default, the IP can be used anywhere.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Governing Law
      </td>

      <td style={{ textAlign: "left" }}>
        The laws of a certain jurisdiction by which this license abides.

        By default, this is California, USA.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Alternative Dispute Resolution
      </td>

      <td style={{ textAlign: "left" }}>
        Please see section 3.1 (s) [here](https://github.com/storyprotocol/protocol-core-v1/blob/main/PIL_Testnet.pdf).
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Additional License Parameters
      </td>

      <td style={{ textAlign: "left" }}>
        There may be other terms the licensor would like to add and they can do so in this tag.
      </td>
    </tr>
  </tbody>
</Table>
