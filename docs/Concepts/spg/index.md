---
title: 📦 SPG
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
The Story Protocol Gateway (SPG) is a group of periphery/utility smart contracts, deployed on our protocol that **allows you to combine independent operations** - like registering an [🧩 IP Asset](doc:ip-asset) and attaching License Terms to that IP Asset - **into one transaction to make your life easier**.

> 🗒️ Contracts
>
> View the smart contracts [here](https://github.com/storyprotocol/protocol-periphery-v1/tree/main/contracts).

For example, this `mintAndRegisterIpAndAttachPILTerms` is one of the functions in the SPG (more specifically in the "License Attachment Workflows") that allows you to mint an NFT, register it as an IP Asset, and attach License Terms to it all in one call:

```sol LicenseAttachmentWorkflows.sol
function mintAndRegisterIpAndAttachPILTerms(
  address nftContract,
  address recipient,
  IPMetadata calldata ipMetadata,
  PILTerms calldata terms
) external onlyCallerWithMinterRole(nftContract) returns (address ipId, uint256 tokenId, uint256 licenseTermsId)
```

## Supported Workflows

As mentioned above, there are many different functions we have created for you that combine multiple functions into one. Because we have created many of them, we have categorized them into different groups. These groups are called "workflows". 

> 📘 Below Workflow Docs
>
> The below docs on the supported workflows is a copy + paste of [this document](https://github.com/storyprotocol/protocol-periphery-v1/blob/main/docs/WORKFLOWS.md). While we will try to keep the below list up to date, you can always go there for the latest version.

### [Registration Workflows](https://github.com/storyprotocol/protocol-periphery-v1/blob/main/contracts/interfaces/workflows/IRegistrationWorkflows.sol)

* `createCollection`: Creates a SPGNFT Collection
* `registerIp`: Registers an IP
* `mintAndRegisterIp`: Mints a NFT → Registers it as an IP

### [License Attachment Workflows](https://github.com/storyprotocol/protocol-periphery-v1/blob/main/contracts/interfaces/workflows/ILicenseAttachmentWorkflows.sol)

* `registerPILTermsAndAttach`: Registers PIL terms → Attaches them to an IP
* `registerIpAndAttachPILTerms`: Registers an IP → Registers PIL terms → Attaches them to the IP
* `mintAndRegisterIpAndAttachPILTerms`: Mints a NFT → Registers it as an IP → Registers PIL terms → Attaches them to the IP

### [Derivative Workflows](https://github.com/storyprotocol/protocol-periphery-v1/blob/main/contracts/interfaces/workflows/IDerivativeWorkflows.sol)

* `registerIpAndMakeDerivative`: Registers an IP → Registers it as a derivative of another IP
* `mintAndRegisterIpAndMakeDerivative`: Mints a NFT → Registers it as an IP → Registers the IP as a derivative of another IP
* `registerIpAndMakeDerivativeWithLicenseTokens`: Registers an IP → Registers the IP as a derivative of another IP using the license tokens
* `mintAndRegisterIpAndMakeDerivativeWithLicenseTokens`: Mints a NFT → Registers it as an IP → Registers the IP as a derivative of another IP using the license tokens

### [Grouping Workflows](https://github.com/storyprotocol/protocol-periphery-v1/blob/main/contracts/interfaces/workflows/IGroupingWorkflows.sol)

* `mintAndRegisterIpAndAttachLicenseAndAddToGroup`: Mints a NFT → Registers it as an IP → Attaches the given license terms to the IP → Adds the IP to a group IP
* `registerIpAndAttachLicenseAndAddToGroup`: Registers an IP → Attaches the given license terms to the IP → Adds the IP to a group IP
* `registerGroupAndAttachLicense`: Registers a group IP → Attaches the given license terms to the group IP
* `registerGroupAndAttachLicenseAndAddIps`: Registers a group IP → Attaches the given license terms to the group IP → Adds existing IPs to the group IP

### [Royalty Workflows](https://github.com/storyprotocol/protocol-periphery-v1/blob/main/contracts/interfaces/workflows/IRoyaltyWorkflows.sol)

* `transferToVaultAndSnapshotAndClaimByTokenBatch`: Transfers revenue tokens to ancestor IP’s royalty vault → Takes a snapshot of the royalty vault → Claims all available revenue tokens from the snapshot to the claimer’s wallet
  * *Use Case*: For IP royalty token holders who want to claim both their direct revenue and royalties from descendant IPs.
* `transferToVaultAndSnapshotAndClaimBySnapshotBatch`: Transfers revenue tokens to ancestor IP’s royalty vault → Takes a snapshot of the royalty vault → Claims all available revenue tokens from the new snapshot to the claimer’s wallet → Claims all available revenue tokens from each provided unclaimed snapshot to the claimer’s wallet
  * *Use Case*: For IP royalty token holders who want to claim both direct revenue and descendant royalties from the latest snapshot and previously taken snapshots.
* `snapshotAndClaimByTokenBatch`: Takes a snapshot of the royalty vault → Claims all available revenue tokens from the new snapshot to the claimer’s wallet
  * *Use Case*: For IP royalty token holders who want to claim the current revenue in their IP’s royalty vault (which may or may not include descendant royalties).
* `snapshotAndClaimBySnapshotBatch`: Takes a snapshot of the royalty vault → Claims all available revenue tokens from the new snapshot to the claimer’s wallet → Claims all available revenue tokens from each provided unclaimed snapshot to the claimer’s wallet
  * *Use Case*: For IP royalty token holders who want to claim the current revenue in their IP’s royalty vault from the latest snapshot and previously taken snapshots.

## Batching Calls

Although the SPG contains certain functions like `mintAndRegisterIpAndAttachPILTerms`, `registerIpAndAttachPILTerms`, and a bunch more, it would be tedious for us to continually update the contract to account for every single combination of possible interactions with an IP Asset.

Instead, we have allowed for a "Multicall" mechanism where you can batch transactions how you like. For more info, see [Batch Function Calls](doc:batch-spg-function-calls).
