---
title: 🛠️ TypeScript SDK Guide
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
> ❗️ v1.3 of SDK is not fully functional yet
>
> v1.3 of SDK is expected to be released on February 7th, 2025. In the meantime, we will be updating these docs beforehand to prepare for the changes.

The best way to get started is to get your hands dirty and start building.

<Cards columns={2}>
  <Card title="Working Code Examples" href="https://github.com/storyprotocol/typescript-tutorial/tree/v1.3" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    Extremely easy & straightforward working code examples for all of the following tutorials.
  </Card>

  <Card title="Video Tutorial" href="https://www.youtube.com/watch?v=r3iIChDhp1g" icon="fa-video" iconColor="#FF0000" target="_blank">
    Watch a video tutorial overviewing an example script.
  </Card>
</Cards>

In the following series of tutorials, you will learn how to build IP applications with Story SDK and the concepts we mentioned in the [Architecture Overview](doc:overview) of Story Protocol.

## Navigating Around the SDK Docs

Before you begin, make sure to read the [TypeScript SDK Setup](doc:typescript-sdk-setup).

Because there are a lot of functions to interact with the [📜 Licensing Module](doc:licensing-module), we have broken them down into a helpful chart so you can identify what you're looking for, and then find the associated docs.

| **Function**                                                                                | **Mint an NFT** | **Register IPA** | **Create License Terms** | **Attach License Terms** | **Mint License Token** | **Register as Derivative** |
| ------------------------------------------------------------------------------------------- | :-------------: | :--------------: | :----------------------: | :----------------------: | :--------------------: | :------------------------: |
| <span style={{color: "#6741d9"}}>register</span>                                            |                 |         ✓        |                          |                          |                        |                            |
| <span style={{color: "#6741d9"}}>mintAndRegisterIp</span>                                   |        ✓        |         ✓        |                          |                          |                        |                            |
| <span style={{color: "#2f9e43"}}>registerPILTerms</span>                                    |                 |                  |             ✓            |                          |                        |                            |
| <span style={{color: "#e03130"}}>attachLicenseTerms</span>                                  |                 |                  |                          |             ✓            |                        |                            |
| <span style={{color: "#e03130"}}>registerIpAndAttachPilTerms</span>                         |                 |         ✓        |             ✓            |             ✓            |                        |                            |
| <span style={{color: "#e03130"}}>registerPilTermsAndAttach</span>                           |                 |                  |             ✓            |             ✓            |                        |                            |
| <span style={{color: "#e03130"}}>mintAndRegisterIpAssetWithPilTerms</span>                  |        ✓        |         ✓        |             ✓            |             ✓            |                        |                            |
| <span style={{color: "#1971c2"}}>mintLicenseTokens</span>                                   |                 |                  |                          |                          |            ✓           |                            |
| <span style={{color: "#f08c00"}}>registerDerivative</span>                                  |                 |                  |                          |                          |                        |              ✓             |
| <span style={{color: "#f08c00"}}>registerDerivativeWithLicenseTokens</span>                 |                 |                  |                          |                          |                        |              ✓             |
| <span style={{color: "#f08c00"}}>registerDerivativeIp</span>                                |                 |         ✓        |                          |                          |                        |              ✓             |
| <span style={{color: "#f08c00"}}>registerIpAndMakeDerivativeWithLicenseTokens</span>        |                 |         ✓        |                          |                          |                        |              ✓             |
| <span style={{color: "#f08c00"}}>mintAndRegisterIpAndMakeDerivative</span>                  |        ✓        |         ✓        |                          |                          |                        |              ✓             |
| <span style={{color: "#f08c00"}}>mintAndRegisterIpAndMakeDerivativeWithLicenseTokens</span> |        ✓        |         ✓        |                          |                          |                        |              ✓             |

* <span style={{color: "#6741d9"}}>Purple</span>: [Register an IP Asset](doc:register-an-ip-asset)
* <span style={{color: "#2f9e43"}}>Green</span>: [Register License Terms](doc:register-pil-terms)
* <span style={{color: "#e03130"}}>Red</span>: [Attach Terms to an IPA](doc:attach-terms-to-an-ip-asset)
* <span style={{color: "#1971c2"}}>Blue</span>: [Mint a License Token](doc:mint-a-license)
* <span style={{color: "#f08c00"}}>Orange</span>: [Register a Derivative](doc:register-a-derivative)

You can also check out [Pay an IPA](doc:pay-ipa) and [Claim Revenue](doc:claim-revenue) to interact with the [💸 Royalty Module](doc:royalty-module) with the SDK.