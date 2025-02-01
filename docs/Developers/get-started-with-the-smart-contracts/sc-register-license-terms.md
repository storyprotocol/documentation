---
title: Register License Terms
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
Now we will learn how to register new [License Terms](doc:license-terms).

## Prerequisites

* Understand what [License Terms](doc:license-terms) are.

## Register License Terms

Create a new file under `./test/1_LicenseTerms.t.sol` and paste the following:

> 📘 Contract Addresses
>
> We have filled in the addresses from the Story contracts for you. However you can also find the addresses for them here: [Deployed Smart Contracts](doc:deployed-smart-contracts)

```sol 1_LicenseTerms.t.sol
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.26;

import { Test } from "forge-std/Test.sol";
import { PILicenseTemplate } from "@storyprotocol/core/modules/licensing/PILicenseTemplate.sol";
import { PILTerms } from "@storyprotocol/core/interfaces/modules/licensing/IPILicenseTemplate.sol";
import { RoyaltyPolicyLAP } from "@storyprotocol/core/modules/royalty/policies/LAP/RoyaltyPolicyLAP.sol";
import { RoyaltyPolicyLAP } from "@storyprotocol/core/modules/royalty/policies/LAP/RoyaltyPolicyLAP.sol";
import { MockERC20 } from "@storyprotocol/test/mocks/token/MockERC20.sol";

// Run this test:
// forge test --fork-url https://aeneid.storyrpc.io/ --match-path test/1_LicenseTerms.t.sol
contract LicenseTermsTest is Test {
    address internal alice = address(0xa11ce);

    // For addresses, see https://docs.story.foundation/docs/deployed-smart-contracts
    // Protocol Core - PILicenseTemplate
    PILicenseTemplate internal PIL_TEMPLATE = PILicenseTemplate(0x2E896b0b2Fdb7457499B56AAaA4AE55BCB4Cd316);
    // Protocol Core - RoyaltyPolicyLAP
    RoyaltyPolicyLAP internal ROYALTY_POLICY_LAP = RoyaltyPolicyLAP(0xBe54FB168b3c982b7AaE60dB6CF75Bd8447b390E);
    // Mock - SUSD
    MockERC20 internal MERC20 = MockERC20(0xF2104833d386a2734a4eB3B8ad6FC6812F29E38E);

    function setUp() public {}

    /// @notice Registers new PIL Terms. Anyone can register PIL Terms.
    function test_registerPILTerms() public {
        PILTerms memory pilTerms = PILTerms({
            transferable: true,
            royaltyPolicy: address(ROYALTY_POLICY_LAP),
            defaultMintingFee: 0,
            expiration: 0,
            commercialUse: true,
            commercialAttribution: true,
            commercializerChecker: address(0),
            commercializerCheckerData: "",
            commercialRevShare: 0,
            commercialRevCeiling: 0,
            derivativesAllowed: true,
            derivativesAttribution: true,
            derivativesApproval: true,
            derivativesReciprocal: true,
            derivativeRevCeiling: 0,
            currency: address(MERC20),
            uri: ""
        });
        uint256 licenseTermsId = PIL_TEMPLATE.registerLicenseTerms(pilTerms);

        uint256 selectedLicenseTermsId = PIL_TEMPLATE.getLicenseTermsId(pilTerms);
        assertEq(licenseTermsId, selectedLicenseTermsId);
    }
}
```

Run `forge build`. If everything is successful, the command should successfully compile.

To test this out, simply run the following command:

```shell
forge test --fork-url https://aeneid.storyrpc.io/ --match-path test/1_LicenseTerms.t.sol
```