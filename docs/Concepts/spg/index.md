---
title: 📦 SPG (Periphery)
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

For example, this `mintAndRegisterIpAndAttachPILTerms` is one of the functions in the SPG (more specifically in the `LicenseAttachmentWorkflows.sol`) that allows you to mint an NFT, register it as an IP Asset, and attach License Terms to it all in one call:

```sol LicenseAttachmentWorkflows.sol
function mintAndRegisterIpAndAttachPILTerms(
  address spgNftContract,
  address recipient,
  WorkflowStructs.IPMetadata calldata ipMetadata,
  WorkflowStructs.LicenseTermsData[] calldata licenseTermsData,
  bool allowDuplicates
) external onlyMintAuthorized(spgNftContract) returns (address ipId, uint256 tokenId, uint256[] memory licenseTermsIds)
```

## All Supported Workflows

As mentioned above, there are many different functions we have created for you that combine multiple functions into one. We have categorized them into different groups. These groups are called "workflows".

<Cards columns={2}>
  <Card title="View all Workflows" href="https://github.com/storyprotocol/protocol-periphery-v1/blob/main/docs/WORKFLOWS.md" icon="fa-eyes" iconColor="white" target="_blank">
    Click here to view all of the supported workflows.
  </Card>

  <Card title="Smart Contracts" href="https://github.com/storyprotocol/protocol-periphery-v1/tree/main/contracts/workflows" icon="fa-scroll" iconColor="#ccb092" target="_blank">
    Click here to view the workflow smart contracts.
  </Card>
</Cards>

## Batching Calls

Although the SPG contains certain functions like `mintAndRegisterIpAndAttachPILTerms`, `registerIpAndAttachPILTerms`, and a bunch more, it would be tedious for us to continually update the contract to account for every single combination of possible interactions with an IP Asset.

Instead, we have allowed for a "Multicall" mechanism where you can batch transactions how you like. For more info, see [Batch Function Calls](doc:batch-spg-function-calls).