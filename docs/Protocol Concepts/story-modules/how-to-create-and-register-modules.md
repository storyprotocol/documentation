---
title: How to Create and Register Modules
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
This guide will walk you through the process of creating a Module and registering it with the Story Protocol, enabling you to contribute to its ecosystem.

# How to Create a Module

Creating a module is straightforward. You need to develop a contract and implement the `IModule` interface. While inheriting from `BaseModule` is recommended for convenience, it's not mandatory.

Below is an example of how to create a HookModule:

```sol TokenGatedHook.sol
// SPDX-License-Identifier: MIT
pragma solidity 0.8.23;

import { IERC165 } from "@openzeppelin/contracts/utils/introspection/ERC165.sol";
import { ERC165Checker } from "@openzeppelin/contracts/utils/introspection/ERC165Checker.sol";
import { IERC721 } from "@openzeppelin/contracts/token/ERC721/IERC721.sol";
import { IHookModule } from "contracts/interfaces/modules/base/IHookModule.sol";
import { BaseModule } from "contracts/modules/BaseModule.sol";

/// @title Mock Token Gated Hook.
/// @notice Hook for ensursing caller is the owner of an NFT token.
contract TokenGatedHook is BaseModule, IHookModule {
    using ERC165Checker for address;

    string public constant override name = "TokenGatedHook";

    function verify(address caller, bytes calldata data) external view returns (bool) {
        address tokenAddress = abi.decode(data, (address));
        if (caller == address(0)) {
            return false;
        }
        if (!tokenAddress.supportsInterface(type(IERC721).interfaceId)) {
            return false;
        }
        return IERC721(tokenAddress).balanceOf(caller) > 0;
    }

    function validateConfig(bytes calldata configData) external view override {
        address tokenAddress = abi.decode(configData, (address));
        require(tokenAddress.supportsInterface(type(IERC721).interfaceId), "MockTokenGatedHook: Invalid token address");
    }

    function supportsInterface(bytes4 interfaceId) public view virtual override(BaseModule, IERC165) returns (bool) {
        return interfaceId == type(IHookModule).interfaceId || super.supportsInterface(interfaceId);
    }
}

```

# How to Register Your Module

After creating your module, the next step is to register it with the Story Protocol to make it available within its ecosystem. We have established a public GitHub repository at [here](https://github.com/storyprotocol/registered-modules) to facilitate the submission process. You are invited to register your module here. The Story Protocol team will conduct a thorough review of your submission. This process ensures that only safe and compliant modules are integrated into the ecosystem.

## Steps to Register Your Module

To get your module verified and listed in this repository, please follow these steps:

1. **Fork this[Repository](https://github.com/storyprotocol/registered-modules):** Begin by forking this repository to your own GitHub account.
2. **Update the Module List:** Add your module's details to the appropriate JSON file according to module types in the repository. Ensure that you adhere to the following JSON structure:

```json

{
  "name": "YourModuleName",
  "address": "YourModuleAddress",
  "blockExplorerLink": "YourModuleBlockExplorerLink"
}
```

* Replace `YourModuleName`, `YourModuleAddress`, and `YourModuleBlockExplorerLink` with your module's name, its address, and the link to its block explorer page, respectively.
* Example:

```json json
{  
  "name": "LicensingModule",  
  "address": "0xd81fd78f557b457b4350cB95D20b547bFEb4D857",  
  "blockExplorerLink": "https://testnet.storyscan.xyz/address/0xd81fd78f557b457b4350cB95D20b547bFEb4D857?tab=contract">  
}
```

3. **Create a Pull Request (PR)**: Once you have added your module, create a pull request against this repository. Utilize the provided PR template to ensure all necessary information is included.
4. **Await Verification:** After your PR is submitted, it will be reviewed. Upon approval and merging of your PR, your module will be officially registered and recognized as safe for use within the StoryProtocol community.

We look forward to seeing your contributions and expanding the StoryProtocol module ecosystem!