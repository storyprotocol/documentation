---
title: Mint a License Token from an IP Asset
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
Once [License Terms](doc:license-terms) have been attached to an IP Asset, you can then mint a [License Token](doc:license-token). This token is an ERC-721 NFT that gives you the right to use the associated IP given the terms that define it. A License Token can then be burned to register your IPA as a derivative of another, which we will cover in the next section. For now, lets learn how to mint a License Token.

## Prerequisites

- An IP Asset has License Terms attached to it. You can learn how to do that here: [Attach License Terms to an IP Asset](doc:sc-attach-license-terms)

## Mint a License Token

Create a new file under `./test/3_LicenseToken.t.sol` and paste the following:

> 📘 Contract Addresses
> 
> We have filled in the addresses from the Story contracts for you. However you can also find the addresses for them here: [Deployed Smart Contracts](doc:deployed-smart-contracts)

```sol 3_LicenseToken.t.sol
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.26;

import { Test } from "forge-std/Test.sol";
// for testing purposes only
import { MockIPGraph } from "@storyprotocol/test/mocks/MockIPGraph.sol";
import { IPAssetRegistry } from "@storyprotocol/core/registries/IPAssetRegistry.sol";
import { LicenseRegistry } from "@storyprotocol/core/registries/LicenseRegistry.sol";
import { PILicenseTemplate } from "@storyprotocol/core/modules/licensing/PILicenseTemplate.sol";
import { RoyaltyPolicyLAP } from "@storyprotocol/core/modules/royalty/policies/LAP/RoyaltyPolicyLAP.sol";
import { PILFlavors } from "@storyprotocol/core/lib/PILFlavors.sol";
import { PILTerms } from "@storyprotocol/core/interfaces/modules/licensing/IPILicenseTemplate.sol";
import { ILicensingModule } from "@storyprotocol/core/interfaces/modules/licensing/ILicensingModule.sol";
import { LicenseToken } from "@storyprotocol/core/LicenseToken.sol";

import { SimpleNFT } from "../src/mocks/SimpleNFT.sol";
import { SUSD } from "../src/mocks/SUSD.sol";

// Run this test:
// forge test --fork-url https://odyssey.storyrpc.io/ --match-path test/3_LicenseToken.t.sol
contract LicenseTokenTest is Test {
    address internal alice = address(0xa11ce);
    address internal bob = address(0xb0b);

    // For addresses, see https://docs.story.foundation/docs/deployed-smart-contracts
    // Protocol Core - IPAssetRegistry
    IPAssetRegistry internal IP_ASSET_REGISTRY = IPAssetRegistry(0x28E59E91C0467e89fd0f0438D47Ca839cDfEc095);
    // Protocol Core - LicenseRegistry
    LicenseRegistry internal LICENSE_REGISTRY = LicenseRegistry(0xBda3992c49E98392e75E78d82B934F3598bA495f);
    // Protocol Core - LicensingModule
    ILicensingModule internal LICENSING_MODULE = ILicensingModule(0x5a7D9Fa17DE09350F481A53B470D798c1c1aabae);
    // Protocol Core - PILicenseTemplate
    PILicenseTemplate internal PIL_TEMPLATE = PILicenseTemplate(0x58E2c909D557Cd23EF90D14f8fd21667A5Ae7a93);
    // Protocol Core - RoyaltyPolicyLAP
    RoyaltyPolicyLAP internal ROYALTY_POLICY_LAP = RoyaltyPolicyLAP(0x28b4F70ffE5ba7A26aEF979226f77Eb57fb9Fdb6);
    // Protocol Core - LicenseToken
    LicenseToken internal LICENSE_TOKEN = LicenseToken(0xB138aEd64814F2845554f9DBB116491a077eEB2D);
    // Mock - SUSD
    SUSD internal SUSD_TOKEN = SUSD(0xC0F6E387aC0B324Ec18EAcf22EE7271207dCE3d5);

    SimpleNFT public SIMPLE_NFT;
    uint256 public tokenId;
    address public ipId;
    uint256 public licenseTermsId;

    function setUp() public {
        // this is only for testing purposes
        // due to our IPGraph precompile not being
        // deployed on the fork
        vm.etch(address(0x0101), address(new MockIPGraph()).code);

        SIMPLE_NFT = new SimpleNFT("Simple IP NFT", "SIM");
        tokenId = SIMPLE_NFT.mint(alice);
        ipId = IP_ASSET_REGISTRY.register(block.chainid, address(SIMPLE_NFT), tokenId);

        licenseTermsId = PIL_TEMPLATE.registerLicenseTerms(
            PILFlavors.commercialRemix({
                mintingFee: 0,
                commercialRevShare: 10 * 10 ** 6, // 10%
                royaltyPolicy: address(ROYALTY_POLICY_LAP),
                currencyToken: address(SUSD_TOKEN)
            })
        );

        vm.prank(alice);
        LICENSING_MODULE.attachLicenseTerms(ipId, address(PIL_TEMPLATE), licenseTermsId);
    }

    /// @notice Mints license tokens for an IP Asset.
    /// Anyone can mint a license token.
    function test_mintLicenseToken() public {
        uint256 startLicenseTokenId = LICENSING_MODULE.mintLicenseTokens({
            licensorIpId: ipId,
            licenseTemplate: address(PIL_TEMPLATE),
            licenseTermsId: licenseTermsId,
            amount: 2,
            receiver: bob,
            royaltyContext: "" // for PIL, royaltyContext is empty string
        });

        assertEq(LICENSE_TOKEN.ownerOf(startLicenseTokenId), bob);
        assertEq(LICENSE_TOKEN.ownerOf(startLicenseTokenId + 1), bob);
    }
}
```

Run `forge build`. If everything is successful, the command should successfully compile.

To test this out, simply run the following command:

```shell
forge test --fork-url https://rpc.odyssey.storyrpc.io/ --match-path test/3_LicenseToken.t.sol
```