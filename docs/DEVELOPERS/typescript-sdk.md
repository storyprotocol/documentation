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
> - Try the script yourself and read the code <a href="https://github.com/storyprotocol/typescript-tutorial/blob/main/scripts" target="_blank">here</a>.
> - Watch a YouTube video overviewing the script <a href="https://www.youtube.com/watch?v=ty5GiNekVh0" target="_blank">here</a>.

In the following series of tutorials, you will learn how to build IP applications with Story SDK and the concepts we mentioned in the [Architecture Overview](doc:overview) of Story Protocol.

## Navigating Around the SDK Docs

Before you begin, make sure to read the [TypeScript SDK Setup](doc:typescript-sdk-setup).

Because there are a lot of functions to interact with the [📜 Licensing Module](doc:licensing-module), we have broken them down into a helpful chart so you can identify what you're looking for, and then find the associated docs.

[block:parameters]
{
  "data": {
    "h-0": "**Function**",
    "h-1": "**Mint an NFT**",
    "h-2": "**Register IPA**",
    "h-3": "**Create License Terms**",
    "h-4": "**Attach License Terms**",
    "h-5": "**Mint License Token**",
    "h-6": "**Register as Derivative**",
    "0-0": "<span style=\"color: #6741d9;\">register</span>",
    "0-1": "",
    "0-2": "✓",
    "0-3": "",
    "0-4": "",
    "0-5": "",
    "0-6": "",
    "1-0": "<span style=\"color: #6741d9;\">mintAndRegisterIp</span>",
    "1-1": "✓",
    "1-2": "✓",
    "1-3": "",
    "1-4": "",
    "1-5": "",
    "1-6": "",
    "2-0": "<span style=\"color: #2f9e43;\">registerPILTerms</span>",
    "2-1": "",
    "2-2": "",
    "2-3": "✓",
    "2-4": "",
    "2-5": "",
    "2-6": "",
    "3-0": "<span style=\"color: #e03130;\">attachLicenseTerms</span>",
    "3-1": "",
    "3-2": "",
    "3-3": "",
    "3-4": "✓",
    "3-5": "",
    "3-6": "",
    "4-0": "<span style=\"color: #e03130;\">registerIpAndAttachPilTerms</span>",
    "4-1": "",
    "4-2": "✓",
    "4-3": "✓",
    "4-4": "✓",
    "4-5": "",
    "4-6": "",
    "5-0": "<span style=\"color: #e03130;\">registerPilTermsAndAttach</span>",
    "5-1": "",
    "5-2": "",
    "5-3": "✓",
    "5-4": "✓",
    "5-5": "",
    "5-6": "",
    "6-0": "<span style=\"color: #e03130;\">mintAndRegisterIpAssetWithPilTerms</span>",
    "6-1": "✓",
    "6-2": "✓",
    "6-3": "✓",
    "6-4": "✓",
    "6-5": "",
    "6-6": "",
    "7-0": "<span style=\"color: #1971c2;\">mintLicenseTokens</span>",
    "7-1": "",
    "7-2": "",
    "7-3": "",
    "7-4": "",
    "7-5": "✓",
    "7-6": "",
    "8-0": "<span style=\"color: #f08c00;\">registerDerivative</span>",
    "8-1": "",
    "8-2": "",
    "8-3": "",
    "8-4": "",
    "8-5": "",
    "8-6": "✓",
    "9-0": "<span style=\"color: #f08c00;\">registerDerivativeWithLicenseTokens</span>",
    "9-1": "",
    "9-2": "",
    "9-3": "",
    "9-4": "",
    "9-5": "",
    "9-6": "✓",
    "10-0": "<span style=\"color: #f08c00;\">registerDerivativeIp</span>",
    "10-1": "",
    "10-2": "✓",
    "10-3": "",
    "10-4": "",
    "10-5": "",
    "10-6": "✓",
    "11-0": "<span style=\"color: #f08c00;\">registerIpAndMakeDerivativeWithLicenseTokens</span>",
    "11-1": "",
    "11-2": "✓",
    "11-3": "",
    "11-4": "",
    "11-5": "",
    "11-6": "✓",
    "12-0": "<span style=\"color: #f08c00;\">mintAndRegisterIpAndMakeDerivative</span>",
    "12-1": "✓",
    "12-2": "✓",
    "12-3": "",
    "12-4": "",
    "12-5": "",
    "12-6": "✓",
    "13-0": "<span style=\"color: #f08c00;\">mintAndRegisterIpAndMakeDerivativeWithLicenseTokens</span>",
    "13-1": "✓",
    "13-2": "✓",
    "13-3": "",
    "13-4": "",
    "13-5": "",
    "13-6": "✓"
  },
  "cols": 7,
  "rows": 14,
  "align": [
    null,
    "center",
    "center",
    "center",
    "center",
    "center",
    "center"
  ]
}
[/block]


- <span style="color: #6741d9">Purple</span>: [Register an IP Asset](doc:register-an-ip-asset)
- <span style="color: #2f9e43">Green</span>: [Register License Terms](doc:register-pil-terms)
- <span style="color: #e03130">Red</span>: [Attach Terms to an IPA](doc:attach-terms-to-an-ip-asset)
- <span style="color: #1971c2">Blue</span>: [Mint a License Token](doc:mint-a-license)
- <span style="color: #f08c00">Orange</span>: [Register a Derivative](doc:register-a-derivative)

You can also check out [Pay an IPA](doc:pay-ipa) and [Claim Revenue](doc:claim-revenue) to interact with the [💸 Royalty Module](doc:royalty-module) with the SDK.