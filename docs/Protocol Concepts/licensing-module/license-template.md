---
title: License Template
excerpt: >-
  A legal framework, written in code ("programmable"), that defines various
  licensing terms for an IP.
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
> View the smart contract for the [💊 Programmable IP License (PIL)](doc:programmable-ip-license), the first and currently only example of a License Template, [here](https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/modules/licensing/PILicenseTemplate.sol).

A License Template is a legal framework, written in code ("programmable"), that defines various licensing terms for an IP. Such as:

* "Is commercial use allowed?" - true/false (bool)
* "Is the license transferrable?" - true/false (bool)
* "If commercial, what % of royalty do I receive?" - number

These terms and values differ per License Template.

The first (and currently only) example of a License Template was developed by the Story team directly, and is called the Programmable IP License (PIL :pill:).

> 💊 Programmable IP License Framework
>
> To learn about the first implementation of a License Template, [read this page](doc:programmable-ip-license-pil).

## License Template Requirements

License Templates are responsible for:

* Providing a link to the actual, off-chain, legal contract template, with all the parameters, their possible values, and the correspondent legalese, in `licenseTextUrl`.
  * For a licensing framework to be compatible with Story Protocol, the legal text **must** be clear and parametrized, with each licensing parameter establishing the possible outcomes of each value.
  * The parameter values in each License Template (called "License Template terms") drive the legal text for each license agreement.
* Defining a `struct` with the particular definitions of the parameters in accordance, which must be encoded into the License Terms struct (described below).
* Providing registration methods for the License Terms, and getters.
* **Verifying** that both the **minter** and the address **linking a derivative are allowed by the License Template terms to perform those actions**. 
  * These conditions could be enforced by the License Template itself or through hooks. They can range from limitations on the derivative creations, token-gating LNFT holders, creative control from licensors, KYC, etc. It's up to the implementation of each License Template.
* **Verifying that the License Terms are compatible if a derivative has or will have multiple parents**

## Create Your Own Template

You can create your own License Template (like the PIL), but it must be approved by the Story team to be fully embedded into the protocol.
