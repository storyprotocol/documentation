---
title: Using an Example
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

<Cards columns={2}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/story-protocol-boilerplate/blob/main/src/Example.sol" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    See the completed code.
  </Card>

  <Card title="Video Walkthrough" href="https://www.youtube.com/watch?v=X421IuZENqM" icon="fa-video" iconColor="#FF0000" target="_blank">
    Check out a video walkthrough of this tutorial!
  </Card>
</Cards>

# Writing the Smart Contract

Now that we have walked through each of the individual steps, let's try to write, deploy, and verify our own smart contract.

## Register IPA, Register License Terms, and Attach to IPA

In this first section, we will combine a few of the tutorials into one. We will create a function named `mintAndRegisterAndCreateTermsAndAttach` that allows you to mint & register a new IP Asset, register new License Terms, and attach those terms to an IP Asset. It will also accept a `receiver` field to be the owner of the new IP Asset.

### Prerequisites

* Complete [Register an NFT as an IP Asset](doc:sc-tutorial-registering-an-ip-asset__old)
* Complete [Register License Terms](doc:sc-register-license-terms)
* Complete [Attach License Terms to an IP Asset](doc:sc-attach-license-terms)

### Writing our Contract

Create a new file under `./src/Example.sol` and paste the following:

> 📘 Contract Addresses
>
> In order to get the contract addresses to pass in the constructor, go to [Deployed Smart Contracts](doc:deployed-smart-contracts).

```sol Example.sol
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.26;

import { IPAssetRegistry } from "@storyprotocol/core/registries/IPAssetRegistry.sol";
import { RegistrationWorkflows } from "@storyprotocol/periphery/workflows/RegistrationWorkflows.sol";
import { WorkflowStructs } from "@storyprotocol/periphery/lib/WorkflowStructs.sol";
import { LicenseRegistry } from "@storyprotocol/core/registries/LicenseRegistry.sol";
import { LicensingModule } from "@storyprotocol/core/modules/licensing/LicensingModule.sol";
import { PILicenseTemplate } from "@storyprotocol/core/modules/licensing/PILicenseTemplate.sol";
import { RoyaltyPolicyLAP } from "@storyprotocol/core/modules/royalty/policies/LAP/RoyaltyPolicyLAP.sol";
import { PILFlavors } from "@storyprotocol/core/lib/PILFlavors.sol";

import { SUSD } from "./mocks/SUSD.sol";
import { SimpleNFT } from "./mocks/SimpleNFT.sol";

/// @notice Register an NFT as an IP Account.
contract Example {
    IPAssetRegistry public immutable IP_ASSET_REGISTRY;
    LicenseRegistry public immutable LICENSE_REGISTRY;
    LicensingModule public immutable LICENSING_MODULE;
    PILicenseTemplate public immutable PIL_TEMPLATE;
    RoyaltyPolicyLAP public immutable ROYALTY_POLICY_LAP;
    SUSD public immutable SUSD_TOKEN;
    SimpleNFT public immutable SIMPLE_NFT;

    constructor(
        address ipAssetRegistry,
        address licensingModule,
        address pilTemplate,
        address royaltyPolicyLAP,
        address susdToken
    ) {
        IP_ASSET_REGISTRY = IPAssetRegistry(ipAssetRegistry);
        LICENSING_MODULE = LicensingModule(licensingModule);
        PIL_TEMPLATE = PILicenseTemplate(pilTemplate);
        ROYALTY_POLICY_LAP = RoyaltyPolicyLAP(royaltyPolicyLAP);
        SUSD_TOKEN = SUSD(susdToken);
        // Create a new Simple NFT collection
        SIMPLE_NFT = new SimpleNFT("Simple IP NFT", "SIM");
    }

    /// @notice Mint an NFT, register it as an IP Asset, and attach License Terms to it.
    /// @param receiver The address that will receive the NFT/IPA.
    /// @return ipId The address of the IP Account.
    /// @return tokenId The token ID of the NFT representing ownership of the IPA.
    /// @return licenseTermsId The ID of the license terms.
    function mintAndRegisterAndCreateTermsAndAttach(
        address receiver
    ) external returns (address ipId, uint256 tokenId, uint256 licenseTermsId) {
        // We mint to this contract so that it has permissions
        // to attach license terms to the IP Asset.
        // We will later transfer it to the intended `receiver`
        tokenId = SIMPLE_NFT.mint(address(this));
        ipId = IP_ASSET_REGISTRY.register(block.chainid, address(SIMPLE_NFT), tokenId);

        // register license terms so we can attach them later
        licenseTermsId = PIL_TEMPLATE.registerLicenseTerms(
            PILFlavors.commercialRemix({
                mintingFee: 0,
                commercialRevShare: 10 * 10 ** 6, // 10%
                royaltyPolicy: address(ROYALTY_POLICY_LAP),
                currencyToken: address(SUSD_TOKEN)
            })
        );

        // attach the license terms to the IP Asset
        LICENSING_MODULE.attachLicenseTerms(ipId, address(PIL_TEMPLATE), licenseTermsId);

        // transfer the NFT to the receiver so it owns the IPA
        SIMPLE_NFT.transferFrom(address(this), receiver, tokenId);
    }
}
```

## Mint a License Token and Register as Derivative

In this next section, we will combine a few of the later tutorials into one. We will create a function named `mintLicenseTokenAndRegisterDerivative` that allows a potentially different user to register their own "child" (derivative) IP Asset, mint a License Token from the "parent" (root) IP Asset, and register their child IPA as a derivative of the parent IPA. It will accept a few parameters:

1. `parentIpId`: the `ipId` of the parent IPA
2. `licenseTermsId`: the id of the License Terms you want to mint a License Token for
3. `receiver`: the owner of the child IPA

### Prerequisites

* Complete [Mint a License Token from an IP Asset](doc:sc-mint-license-token)
* Complete [Remix an IP Asset](doc:sc-remix-ipa)

### Writing our Contract

In your `Example.sol` contract, add the following function:

```sol Example.sol
/// @notice Mint and register a new child IPA, mint a License Token
/// from the parent, and register it as a derivative of the parent.
/// @param parentIpId The ipId of the parent IPA.
/// @param licenseTermsId The ID of the license terms you will
/// mint a license token from.
/// @param receiver The address that will receive the NFT/IPA.
/// @return childIpId The address of the child IPA.
/// @return childTokenId The token ID of the NFT representing ownership of the child IPA.
/// @return licenseTokenId The ID of the license token.
function mintLicenseTokenAndRegisterDerivative(
  address parentIpId,
  uint256 licenseTermsId,
  address receiver
) external returns (address childIpId, uint256 childTokenId, uint256 licenseTokenId) {
  // We mint to this contract so that it has permissions
  // to register itself as a derivative of another
  // IP Asset.
  // We will later transfer it to the intended `receiver`
  childTokenId = SIMPLE_NFT.mint(address(this));
  childIpId = IP_ASSET_REGISTRY.register(block.chainid, address(SIMPLE_NFT), childTokenId);

  // mint a license token from the parent
  licenseTokenId = LICENSING_MODULE.mintLicenseTokens({
    licensorIpId: parentIpId,
    licenseTemplate: address(PIL_TEMPLATE),
    licenseTermsId: licenseTermsId,
    amount: 1,
    // mint the license token to this contract so it can
    // use it to register as a derivative of the parent
    receiver: address(this),
    royaltyContext: "" // for PIL, royaltyContext is empty string
  });

  uint256[] memory licenseTokenIds = new uint256[](1);
  licenseTokenIds[0] = licenseTokenId;

  // register the new child IPA as a derivative
  // of the parent
  LICENSING_MODULE.registerDerivativeWithLicenseTokens({
    childIpId: childIpId,
    licenseTokenIds: licenseTokenIds,
    royaltyContext: "" // empty for PIL
  });

  // transfer the NFT to the receiver so it owns the child IPA
  SIMPLE_NFT.transferFrom(address(this), receiver, childTokenId);
}
```

# Testing our Contract

Create another new file under `./test/Example.t.sol` and paste the following:

```sol Example.t.sol
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.26;

import { Test } from "forge-std/Test.sol";
// for testing purposes only
import { MockIPGraph } from "@storyprotocol/test/mocks/MockIPGraph.sol";
import { IPAssetRegistry } from "@storyprotocol/core/registries/IPAssetRegistry.sol";
import { LicenseRegistry } from "@storyprotocol/core/registries/LicenseRegistry.sol";

import { Example } from "../src/Example.sol";
import { SimpleNFT } from "../src/mocks/SimpleNFT.sol";

// Run this test:
// forge test --fork-url https://odyssey.storyrpc.io/ --match-path test/Example.t.sol
contract ExampleTest is Test {
    address internal alice = address(0xa11ce);
    address internal bob = address(0xb0b);
    // For addresses, see https://docs.story.foundation/docs/deployed-smart-contracts
    // Protocol Core - IPAssetRegistry
    address internal ipAssetRegistry = 0x28E59E91C0467e89fd0f0438D47Ca839cDfEc095;
    // Protocol Core - LicenseRegistry
    address internal licenseRegistry = 0xBda3992c49E98392e75E78d82B934F3598bA495f;
    // Protocol Core - LicensingModule
    address internal licensingModule = 0x5a7D9Fa17DE09350F481A53B470D798c1c1aabae;
    // Protocol Core - PILicenseTemplate
    address internal pilTemplate = 0x58E2c909D557Cd23EF90D14f8fd21667A5Ae7a93;
    // Protocol Core - RoyaltyPolicyLAP
    address internal royaltyPolicyLAP = 0x28b4F70ffE5ba7A26aEF979226f77Eb57fb9Fdb6;
    // Mock - SUSD
    address internal susd = 0xC0F6E387aC0B324Ec18EAcf22EE7271207dCE3d5;

    SimpleNFT public SIMPLE_NFT;
    Example public EXAMPLE;

    function setUp() public {
        // this is only for testing purposes
        // due to our IPGraph precompile not being
        // deployed on the fork
        vm.etch(address(0x0101), address(new MockIPGraph()).code);

        EXAMPLE = new Example(ipAssetRegistry, licensingModule, pilTemplate, royaltyPolicyLAP, susd);
        SIMPLE_NFT = SimpleNFT(EXAMPLE.SIMPLE_NFT());
    }

    function test_mintAndRegisterAndCreateTermsAndAttach() public {
        LicenseRegistry LICENSE_REGISTRY = LicenseRegistry(licenseRegistry);
        IPAssetRegistry IP_ASSET_REGISTRY = IPAssetRegistry(ipAssetRegistry);

        uint256 expectedTokenId = SIMPLE_NFT.nextTokenId();
        address expectedIpId = IP_ASSET_REGISTRY.ipId(block.chainid, address(SIMPLE_NFT), expectedTokenId);

        (address ipId, uint256 tokenId, uint256 licenseTermsId) = EXAMPLE.mintAndRegisterAndCreateTermsAndAttach(alice);

        assertEq(tokenId, expectedTokenId);
        assertEq(ipId, expectedIpId);
        assertEq(SIMPLE_NFT.ownerOf(tokenId), alice);

        assertTrue(LICENSE_REGISTRY.hasIpAttachedLicenseTerms(ipId, pilTemplate, licenseTermsId));
        // We expect 2 because the IPA has the default license terms (licenseTermsId = 1)
        // and the one we attached.
        assertEq(LICENSE_REGISTRY.getAttachedLicenseTermsCount(ipId), 2);
        // Although an IP Asset has default license terms, index 0 is
        // still the one we attached.
        (address licenseTemplate, uint256 attachedLicenseTermsId) = LICENSE_REGISTRY.getAttachedLicenseTerms({
            ipId: ipId,
            index: 0
        });
        assertEq(licenseTemplate, pilTemplate);
        assertEq(attachedLicenseTermsId, licenseTermsId);
    }

    function test_mintLicenseTokenAndRegisterDerivative() public {
        LicenseRegistry LICENSE_REGISTRY = LicenseRegistry(licenseRegistry);
        IPAssetRegistry IP_ASSET_REGISTRY = IPAssetRegistry(ipAssetRegistry);

        (address parentIpId, uint256 parentTokenId, uint256 licenseTermsId) = EXAMPLE
            .mintAndRegisterAndCreateTermsAndAttach(alice);

        (address childIpId, uint256 childTokenId, uint256 licenseTokenId) = EXAMPLE
            .mintLicenseTokenAndRegisterDerivative(parentIpId, licenseTermsId, bob);

        assertTrue(LICENSE_REGISTRY.hasDerivativeIps(parentIpId));
        assertTrue(LICENSE_REGISTRY.isParentIp(parentIpId, childIpId));
        assertTrue(LICENSE_REGISTRY.isDerivativeIp(childIpId));
        assertEq(LICENSE_REGISTRY.getDerivativeIpCount(parentIpId), 1);
        assertEq(LICENSE_REGISTRY.getParentIpCount(childIpId), 1);
        assertEq(LICENSE_REGISTRY.getParentIp({ childIpId: childIpId, index: 0 }), parentIpId);
        assertEq(LICENSE_REGISTRY.getDerivativeIp({ parentIpId: parentIpId, index: 0 }), childIpId);
    }
}
```

Run `forge build`. If everything is successful, the command should successfully compile.

To test this out, simply run the following command:

```shell
forge test --fork-url https://rpc.odyssey.storyrpc.io/ --match-path test/Example.t.sol
```

# Deploy & Verify the Example Contract

The `--constructor-args` come from [Deployed Smart Contracts](doc:deployed-smart-contracts).

```shell
forge create \
  --rpc-url https://odyssey.storyrpc.io/ \
  --private-key $PRIVATE_KEY \
  ./src/Example.sol:Example \
  --verify \
  --verifier blockscout \
  --verifier-url https://odyssey.storyscan.xyz/api/ \
  --constructor-args 0x28E59E91C0467e89fd0f0438D47Ca839cDfEc095 0x5a7D9Fa17DE09350F481A53B470D798c1c1aabae 0x58E2c909D557Cd23EF90D14f8fd21667A5Ae7a93 0x28b4F70ffE5ba7A26aEF979226f77Eb57fb9Fdb6 0xC0F6E387aC0B324Ec18EAcf22EE7271207dCE3d5
```

If everything worked correctly, you should see something like `Deployed to: 0xfb0923D531C1ca54AB9ee10CB8364b23d0C7F47d` in the console. Paste that address into [the explorer](https://odyssey.storyscan.xyz/) and see your verified contract!

# Great job! :)
