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

- Understand what [License Terms](doc:license-terms) are.

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

import { SUSD } from "../src/mocks/SUSD.sol";

// Run this test:
// forge test --fork-url https://odyssey.storyrpc.io/ --match-path test/1_LicenseTerms.t.sol
contract LicenseTermsTest is Test {
    address internal alice = address(0xa11ce);

    // For addresses, see https://docs.story.foundation/docs/deployed-smart-contracts
    // Protocol Core - PILicenseTemplate
    PILicenseTemplate public immutable PIL_TEMPLATE = PILicenseTemplate(0x58E2c909D557Cd23EF90D14f8fd21667A5Ae7a93);
    // Protocol Core - RoyaltyPolicyLAP
    RoyaltyPolicyLAP public immutable ROYALTY_POLICY_LAP = RoyaltyPolicyLAP(0x28b4F70ffE5ba7A26aEF979226f77Eb57fb9Fdb6);
    // Mock - SUSD
    SUSD public immutable SUSD_TOKEN = SUSD(0xC0F6E387aC0B324Ec18EAcf22EE7271207dCE3d5);

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
            currency: address(SUSD_TOKEN),
            uri: ""
        });
        uint256 licenseTermsId = PIL_TEMPLATE.registerLicenseTerms(pilTerms);

        uint256 selectedLicenseTermsId = PIL_TEMPLATE.getLicenseTermsId(pilTerms);
        assertEq(licenseTermsId, selectedLicenseTermsId);
    }
}
```

Create another new file under `./src/mocks/SUSD.sol` and paste the following:

```sol SUSD.sol
// SPDX-License-Identifier: BUSL-1.1
pragma solidity ^0.8.23;

import { ERC20 } from "@openzeppelin/contracts/token/ERC20/ERC20.sol";

contract SUSD is ERC20 {
    constructor() ERC20("Story USD", "SUSD") {}

    function mint(address to, uint256 amount) external {
        _mint(to, amount);
    }

    function burn(address from, uint256 amount) external {
        _burn(from, amount);
    }
}
```

Run `forge build`. If everything is successful, the command should successfully compile.

To test this out, simply run the following command:

```shell
forge test --fork-url https://rpc.odyssey.storyrpc.io/ --match-path test/1_LicenseTerms.t.sol
```