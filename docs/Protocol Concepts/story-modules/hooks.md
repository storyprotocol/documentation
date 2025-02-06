---
title: Hooks
excerpt: Introduction of hooks in Story Protocol
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# Overview

Hooks in Story Protocol are defined as a specialized interface that inherits from the Module framework. They are designed for developers to create custom implementations that integrate seamlessly with existing Modules.

<Image align="center" src="https://files.readme.io/2ea34f7-Screenshot_2024-02-05_at_16.09.49.png" />

# Concept and Functionality

While Modules are the backbone of the Story Protocol, executing actions and managing interactions, Hooks are distinct in their specific focus. They are tailored to verify conditions, an essential feature embodied in their `verify()` functionality. This design choice positions Hooks as a unique subset of Modules, specialized for particular tasks within the ecosystem.

# Design Philosophy

From a structural standpoint, Hooks are not treated as separate entities from Modules. This decision avoids unnecessary complexity in the architecture. Viewing Hooks as specialized Modules allows for a simplified, efficient design that emphasizes clarity in roles and interactions.

# Sample Token Gated Hook Module

```coffeescript
pragma solidity 0.8.23;

import { IERC165 } from "@openzeppelin/contracts/utils/introspection/ERC165.sol";
import { ERC165Checker } from "@openzeppelin/contracts/utils/introspection/ERC165Checker.sol";
import { IERC721 } from "@openzeppelin/contracts/token/ERC721/IERC721.sol";
import { IHookModule } from "../../../contracts/interfaces/modules/base/IHookModule.sol";
import { BaseModule } from "../../../contracts/modules/BaseModule.sol";

/// @title Token Gated Hook.
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
        require(tokenAddress.supportsInterface(type(IERC721).interfaceId), "TokenGatedHook: Invalid token address");
    }

    function supportsInterface(bytes4 interfaceId) public view virtual override(BaseModule, IERC165) returns (bool) {
        return interfaceId == type(IHookModule).interfaceId || super.supportsInterface(interfaceId);
    }
}

```

Using The `TokenGatedHook` as `commercializerChecker` with `LicensingModule` for use case that only allow mint license to licensee who own a specific NFT token. 

```
        licensingModule.registerPolicyFrameworkManager(address(pilManager));

        PILPolicy memory policyData = PILPolicy({
            attribution: true,
            commercialUse: true,
            commercialAttribution: false,
            commercializerChecker: address(0),
            commercializerCheckerData: "",
            commercialRevShare: 100,
            derivativesAllowed: false,
            derivativesAttribution: false,
            derivativesApproval: false,
            derivativesReciprocal: false,
            territories: new string[](1),
            distributionChannels: new string[](1),
            contentRestrictions: emptyStringArray
        });

        gatedNftFoo.mintId(address(this), 1);

        MockTokenGatedHook tokenGatedHook = new MockTokenGatedHook();
        policyData.commercializerChecker = address(tokenGatedHook);
        // address(this) doesn't hold token of NFT collection gatedNftBar, so the verification will fail
        policyData.commercializerCheckerData = abi.encode(address(gatedNftBar));
        policyData.territories[0] = "territory1";
        policyData.distributionChannels[0] = "distributionChannel1";

        uint256 policyId = pilManager.registerPolicy(
            RegisterPILPolicyParams({
                transferable: true,
                royaltyPolicy: address(mockRoyaltyPolicyLAP),
                mintingFee: 0,
                mintingFeeToken: address(0),
                policy: policyData
            })
        );

        vm.prank(ipOwner);
        licensingModule.addPolicyToIp(ipId1, policyId);
        licensingModule.mintLicense(policyId, ipId1, 1, licenseHolder, "");
```
