---
title: 📜 Licensing Module
excerpt: Learn about creating & attaching real legal license to your IP on Story.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
> ⏩ Skip the Read - 1 Minute Summary
>
> The Licensing Module allows you to create a real legal license from a **License Template** (which is the [Programmable IP License (PIL💊)](doc:programmable-ip-license)) and attach it to your IP Asset. This license, and the **License Terms** that define it, restrict how others can use your IP, commercialize it, and remix it.
>
> If License Terms are attached to an IP Asset, anyone can mint a **License Token** (an ERC-721 NFT) from it which acts as the license to use that work based on the terms that define it. This token can then be burned to register a derivative work. This then establishes a parent-child relationship between assets, unlocking things like automatic royalty flow from the [💸 Royalty Module](doc:royalty-module).

The owner address of an IP Asset owns intellectual property rights such as creating derivatives, being commercially exploited, and being reproduced in different platforms. IP Assets can programmatically grant permissions for any users to exercise those rights with some autonomy via **License Tokens** (an ERC-721 NFT), which point to a particular set of conditions, known as **License Terms**.

<Image alt="The contracts in blue are built into the protocol. The contracts in white can be developed by the community or 3rd party vendor. " align="center" src="https://files.readme.io/3be1037-Screenshot_2024-05-07_at_17.52.53.png">
  Blue: contracts built into the protocol. White: contracts developed by the community or 3rd party vendor.
</Image>

## `LicensingModule.sol`

> 🗒️ Contract
>
> View the smart contract [here](https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/modules/licensing/LicensingModule.sol).

The `LicensingModule.sol` contract is the main entry point for the licensing system. It is responsible for:

* Attaching License Terms to IP Assets
* Minting License Tokens
* Registering derivatives
* Setting License Configs

## Further Readings

The following document will walk through all of the major components of the Licensing Module as shown above:

* [License Template](doc:license-template)
* [License Terms](doc:license-terms)
* [License Token](doc:license-token)
* [License Registry](doc:license-registry)
* [License Config / Hook](doc:license-config-hook)
