---
title: ⚙️ Smart Contracts
excerpt: For smart contract developers who wish to build on top of Story directly.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
In this section, we will briefly go over the protocol contracts and then guide you through how to start building on top of the protocol. If you haven't yet familiarized yourself with the overall architecture, we recommend first going over the [Overview](doc:overview) section.

> 🚧 Our contracts are undergoing audits.
>
> The v1 release of our "Proof-of-Creativity Protocol" is currently undergoing audits, which means that we could change the smart contracts at any time without warning to address the audit findings. When the contracts are fully audited, we will remove this warning and ensure regular updates to this document with formal notice.

# Smart Contract Tutorial

> ✅ Completed Code
>
> Skip the tutorial and view the completed code [here](https://github.com/storyprotocol/story-protocol-boilerplate). Follow the README instructions to run the tests, or go to the `/test` folder to view all of the example contracts.

**If you want to set things up from scratch**, then continue with the following tutorials, starting with the [Setup Your Own Project](doc:sc-tutorial-quick-setup) step.

# Our Smart Contracts

As of the current version, our Proof-of-Creativity Protocol is compatible with all EVM chains and is written as a set of Smart Contracts in Solidity. There are two repositories that you may interact with as a developer:

* [Story Protocol Core](https://github.com/storyprotocol/protocol-core-v1) - This repository contains the core protocol logic, consisting of a thin IP registry (the [IP Asset Registry](doc:ip-asset-registry)), a set of [🧱 Modules](doc:story-modules) defining logic around [📜 Licensing](doc:licensing-module), [💸 Royalty](doc:royalty-module), [❌ Dispute](doc:dispute-module), metadata, and a module manager for administering module and user access control.
* [Story Protocol Periphery](https://github.com/storyprotocol/protocol-periphery-v1)- Whereas the core contracts deal with the underlying protocol logic, the periphery contracts deal with protocol extensions that greatly increase UX and simplify IPA management. This is mostly handled through the [📦 SPG](doc:spg).

## Deploy & Verify Contracts on Story

*The approach to deploy & verify contracts comes from the[Blockscout official documentation](https://docs.blockscout.com/developer-support/verifying-a-smart-contract/foundry-verification).*

Verify a contract with Blockscout right after deployment (make sure you add "/api/" to the end of the Blockscout homepage explorer URL):

```shell Script
forge create \
  --rpc-url <rpc_https_endpoint> \
  --private-key $PRIVATE_KEY \
  <contract_file>:<contract_name> \
  --verify \
  --verifier blockscout \
  --verifier-url <blockscout_homepage_explorer_url>/api/
```

Or if using foundry scripts:

```shell Script
forge script <script_file> \
  --rpc-url <rpc_https_endpoint> \
  --private-key $PRIVATE_KEY \
  --broadcast \
  --verify \
  --verifier blockscout \
  --verifier-url <blockscout_homepage_explorer_url>/api/
```
