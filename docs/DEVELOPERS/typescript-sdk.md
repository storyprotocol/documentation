---
title: 🛠️ TypeScript SDK
excerpt: For TypeScript developers who want to build with Story.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
  pages:
    - type: basic
      slug: typescript-sdk-setup
      title: TypeScript SDK Setup
---
The best way to get started is to get your hands dirty and start building. 

> 🏁 TypeScript Tutorial & Video
>
> Extremely easy & straightforward examples for all of the following tutorials, combined into 1 script.
>
> * Try the script yourself and read the code <a href="https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts" target="_blank">here</a>.
> * Watch a YouTube video overviewing the script <a href="https://www.youtube.com/watch?v=ty5GiNekVh0" target="_blank">here</a>.

In the following series of tutorials, you will learn how to build IP applications with Story SDK and the concepts we mentioned in the [Architecture Overview](doc:overview) of Story Protocol.

## Navigating Around the SDK Docs

Before you begin, make sure to read the [TypeScript SDK Setup](doc:typescript-sdk-setup).

Because there are a lot of functions to interact with the [📜 Licensing Module](doc:licensing-module), we have broken them down into a helpful chart so you can identify what you're looking for, and then find the associated docs.

<Table align={[null,"center","center","center","center","center","center"]}>
  <thead>
    <tr>
      <th>
        **Function**
      </th>

      <th style={{ textAlign: "center" }}>
        **Mint an NFT**
      </th>

      <th style={{ textAlign: "center" }}>
        **Register IPA**
      </th>

      <th style={{ textAlign: "center" }}>
        **Create License Terms**
      </th>

      <th style={{ textAlign: "center" }}>
        **Attach License Terms**
      </th>

      <th style={{ textAlign: "center" }}>
        **Mint License Token**
      </th>

      <th style={{ textAlign: "center" }}>
        **Register as Derivative**
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        <span style="color: #6741d9;">register</span>
      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>
        ✓
      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>

      </td>
    </tr>

    <tr>
      <td>
        <span style="color: #6741d9;">mintAndRegisterIp</span>
      </td>

      <td style={{ textAlign: "center" }}>
        ✓
      </td>

      <td style={{ textAlign: "center" }}>
        ✓
      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>

      </td>
    </tr>

    <tr>
      <td>
        <span style="color: #2f9e43;">registerPILTerms</span>
      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>
        ✓
      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>

      </td>
    </tr>

    <tr>
      <td>
        <span style="color: #e03130;">attachLicenseTerms</span>
      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>
        ✓
      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>

      </td>
    </tr>

    <tr>
      <td>
        <span style="color: #e03130;">registerIpAndAttachPilTerms</span>
      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>
        ✓
      </td>

      <td style={{ textAlign: "center" }}>
        ✓
      </td>

      <td style={{ textAlign: "center" }}>
        ✓
      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>

      </td>
    </tr>

    <tr>
      <td>
        <span style="color: #e03130;">registerPilTermsAndAttach</span>
      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>
        ✓
      </td>

      <td style={{ textAlign: "center" }}>
        ✓
      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>

      </td>
    </tr>

    <tr>
      <td>
        <span style="color: #e03130;">mintAndRegisterIpAssetWithPilTerms</span>
      </td>

      <td style={{ textAlign: "center" }}>
        ✓
      </td>

      <td style={{ textAlign: "center" }}>
        ✓
      </td>

      <td style={{ textAlign: "center" }}>
        ✓
      </td>

      <td style={{ textAlign: "center" }}>
        ✓
      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>

      </td>
    </tr>

    <tr>
      <td>
        <span style="color: #1971c2;">mintLicenseTokens</span>
      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>
        ✓
      </td>

      <td style={{ textAlign: "center" }}>

      </td>
    </tr>

    <tr>
      <td>
        <span style="color: #f08c00;">registerDerivative</span>
      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>
        ✓
      </td>
    </tr>

    <tr>
      <td>
        <span style="color: #f08c00;">registerDerivativeWithLicenseTokens</span>
      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>
        ✓
      </td>
    </tr>

    <tr>
      <td>
        <span style="color: #f08c00;">registerDerivativeIp</span>
      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>
        ✓
      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>
        ✓
      </td>
    </tr>

    <tr>
      <td>
        <span style="color: #f08c00;">registerIpAndMakeDerivativeWithLicenseTokens</span>
      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>
        ✓
      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>
        ✓
      </td>
    </tr>

    <tr>
      <td>
        <span style="color: #f08c00;">mintAndRegisterIpAndMakeDerivative</span>
      </td>

      <td style={{ textAlign: "center" }}>
        ✓
      </td>

      <td style={{ textAlign: "center" }}>
        ✓
      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>
        ✓
      </td>
    </tr>

    <tr>
      <td>
        <span style="color: #f08c00;">mintAndRegisterIpAndMakeDerivativeWithLicenseTokens</span>
      </td>

      <td style={{ textAlign: "center" }}>
        ✓
      </td>

      <td style={{ textAlign: "center" }}>
        ✓
      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>

      </td>

      <td style={{ textAlign: "center" }}>
        ✓
      </td>
    </tr>
  </tbody>
</Table>

* <span style="color: #6741d9">Purple</span>: [Register an IP Asset](doc:register-an-ip-asset)
* <span style="color: #2f9e43">Green</span>: [Register License Terms](doc:register-pil-terms)
* <span style="color: #e03130">Red</span>: [Attach Terms to an IPA](doc:attach-terms-to-an-ip-asset)
* <span style="color: #1971c2">Blue</span>: [Mint a License Token](doc:mint-a-license)
* <span style="color: #f08c00">Orange</span>: [Register a Derivative](doc:register-a-derivative)

You can also check out [Pay an IPA](doc:pay-ipa) and [Claim Revenue](doc:claim-revenue) to interact with the [💸 Royalty Module](doc:royalty-module) with the SDK.
