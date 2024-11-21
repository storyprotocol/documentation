---
title: ❓ Concepts FAQ (COPY)
excerpt: Common technical questions related to our protocol documentation.
deprecated: false
hidden: false
metadata:
  robots: index
---
## *"What is the difference between License Tokens, Royalty Tokens, and Revenue Tokens?"*

<Table align={["left","left","left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>

      </th>

      <th style={{ textAlign: "left" }}>
        License Tokens
      </th>

      <th style={{ textAlign: "left" }}>
        Royalty Tokens
      </th>

      <th style={{ textAlign: "left" }}>
        Revenue Tokens
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        **Module**
      </td>

      <td style={{ textAlign: "left" }}>
        [📜 Licensing Module](doc:licensing-module)
      </td>

      <td style={{ textAlign: "left" }}>
        [💸 Royalty Module](doc:royalty-module)
      </td>

      <td style={{ textAlign: "left" }}>
        [💸 Royalty Module](doc:royalty-module)
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        **Explanation**
      </td>

      <td style={{ textAlign: "left" }}>
        An ERC-721 NFT that gets minted from an IP Asset with specific license terms. It is essentially the license you hold that gives you access to use the associated IP Asset based on the terms in the License Token.  

        A License Token is burned when it is used to register an IP Asset as a derivative of another.
      </td>

      <td style={{ textAlign: "left" }}>
        Each IP Asset has 100,000,000 Royalty Tokens associated, where each token represents the right of whoever owns them to claim 0.000001% of the gains ("*Revenue Tokens*") deposited into the IPA's Royalty Vault.
      </td>

      <td style={{ textAlign: "left" }}>
        These are the tokens that are actually used for payment (ex. ETH, USDC, etc).  

        "*Royalty Tokens*" are used to claim these Revenue Tokens when an IP Asset earns them.
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        **Associated Docs**
      </td>

      <td style={{ textAlign: "left" }}>
        [License Token](doc:license-token)
      </td>

      <td style={{ textAlign: "left" }}>
        [IP Royalty Vault](doc:ip-royalty-vault)
      </td>

      <td style={{ textAlign: "left" }}>
        [IP Royalty Vault](doc:ip-royalty-vault)
      </td>
    </tr>
  </tbody>
</Table>