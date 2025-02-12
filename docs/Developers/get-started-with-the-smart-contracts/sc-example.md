---
title: Using an Example
excerpt: Combine all of our tutorials together in a practical example.
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

### ⚠️ Prerequisites

* Complete [Register an IP Asset](doc:sc-register-an-ip-asset)
* Complete [Register License Terms](doc:sc-register-license-terms)
* Complete [Attach Terms to an IPA](doc:sc-attach-license-terms)

### Writing our Contract

Create a new file under `./src/Example.sol` and paste the following:

> 📘 Contract Addresses
>
> In order to get the contract addresses to pass in the constructor, go to [Deployed Smart Contracts](doc:deployed-smart-contracts).

```sol src/Example.sol
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.26;

import { IIPAssetRegistry } from "@storyprotocol/core/interfaces/registries/IIPAssetRegistry.sol";
import { ILicensingModule } from "@storyprotocol/core/interfaces/modules/licensing/ILicensingModule.sol";
import { IPILicenseTemplate } from "@storyprotocol/core/interfaces/modules/licensing/IPILicenseTemplate.sol";
import { PILFlavors } from "@storyprotocol/core/lib/PILFlavors.sol";

import { SimpleNFT } from "./mocks/SimpleNFT.sol";

import { ERC721Holder } from "@openzeppelin/contracts/token/ERC721/utils/ERC721Holder.sol";

/// @notice An example contract that demonstrates how to mint an NFT, register it as an IP Asset,
/// attach license terms to it, mint a license token from it, and register it as a derivative of the parent.
contract Example is ERC721Holder {
  IIPAssetRegistry public immutable IP_ASSET_REGISTRY;
  ILicensingModule public immutable LICENSING_MODULE;
  IPILicenseTemplate public immutable PIL_TEMPLATE;
  address public immutable ROYALTY_POLICY_LAP;
  address public immutable WIP;
  SimpleNFT public immutable SIMPLE_NFT;

  constructor(
    address ipAssetRegistry,
    address licensingModule,
    address pilTemplate,
    address royaltyPolicyLAP,
    address wip
  ) {
    IP_ASSET_REGISTRY = IIPAssetRegistry(ipAssetRegistry);
    LICENSING_MODULE = ILicensingModule(licensingModule);
    PIL_TEMPLATE = IPILicenseTemplate(pilTemplate);
    ROYALTY_POLICY_LAP = royaltyPolicyLAP;
    WIP = wip;
    // Create a new Simple NFT collection
    SIMPLE_NFT = new SimpleNFT("Simple IP NFT", "SIM");
  }

  /// @notice Mint an NFT, register it as an IP Asset, and attach License Terms to it.
  /// @param receiver The address that will receive the NFT/IPA.
  /// @return tokenId The token ID of the NFT representing ownership of the IPA.
  /// @return ipId The address of the IP Account.
  /// @return licenseTermsId The ID of the license terms.
  function mintAndRegisterAndCreateTermsAndAttach(
    address receiver
  ) external returns (uint256 tokenId, address ipId, uint256 licenseTermsId) {
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
        royaltyPolicy: ROYALTY_POLICY_LAP,
        currencyToken: WIP
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

### :warning: Prerequisites

* Complete [Mint a License Token](doc:sc-mint-license-token)
* Complete [Register a Derivative](doc:sc-remix-ipa)

### Writing our Contract

In your `Example.sol` contract, add the following function at the bottom:

```sol src/Example.sol
/// @notice Mint and register a new child IPA, mint a License Token
/// from the parent, and register it as a derivative of the parent.
/// @param parentIpId The ipId of the parent IPA.
/// @param licenseTermsId The ID of the license terms you will
/// mint a license token from.
/// @param receiver The address that will receive the NFT/IPA.
/// @return childTokenId The token ID of the NFT representing ownership of the child IPA.
/// @return childIpId The address of the child IPA.
function mintLicenseTokenAndRegisterDerivative(
  address parentIpId,
  uint256 licenseTermsId,
  address receiver
) external returns (uint256 childTokenId, address childIpId) {
  // We mint to this contract so that it has permissions
  // to register itself as a derivative of another
  // IP Asset.
  // We will later transfer it to the intended `receiver`
  childTokenId = SIMPLE_NFT.mint(address(this));
  childIpId = IP_ASSET_REGISTRY.register(block.chainid, address(SIMPLE_NFT), childTokenId);

  // mint a license token from the parent
  uint256 licenseTokenId = LICENSING_MODULE.mintLicenseTokens({
    licensorIpId: parentIpId,
    licenseTemplate: address(PIL_TEMPLATE),
    licenseTermsId: licenseTermsId,
    amount: 1,
    // mint the license token to this contract so it can
    // use it to register as a derivative of the parent
    receiver: address(this),
    royaltyContext: "", // for PIL, royaltyContext is empty string
    maxMintingFee: 0,
    maxRevenueShare: 0
  });

  uint256[] memory licenseTokenIds = new uint256[](1);
  licenseTokenIds[0] = licenseTokenId;

  // register the new child IPA as a derivative
  // of the parent
  LICENSING_MODULE.registerDerivativeWithLicenseTokens({
    childIpId: childIpId,
    licenseTokenIds: licenseTokenIds,
    royaltyContext: "", // empty for PIL
    maxRts: 0
  });

  // transfer the NFT to the receiver so it owns the child IPA
  SIMPLE_NFT.transferFrom(address(this), receiver, childTokenId);
}
```

# Testing our Contract

Create another new file under `test/Example.t.sol` and paste the following:

```sol Example.t.sol
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.26;

import { Test } from "forge-std/Test.sol";
// for testing purposes only
import { MockIPGraph } from "@storyprotocol/test/mocks/MockIPGraph.sol";
import { IIPAssetRegistry } from "@storyprotocol/core/interfaces/registries/IIPAssetRegistry.sol";
import { ILicenseRegistry } from "@storyprotocol/core/interfaces/registries/ILicenseRegistry.sol";

import { Example } from "../src/Example.sol";
import { SimpleNFT } from "../src/mocks/SimpleNFT.sol";

// Run this test:
// forge test --fork-url https://aeneid.storyrpc.io/ --match-path test/Example.t.sol
contract ExampleTest is Test {
  address internal alice = address(0xa11ce);
  address internal bob = address(0xb0b);

  // For addresses, see https://docs.story.foundation/docs/deployed-smart-contracts
  // Protocol Core - IPAssetRegistry
  address internal ipAssetRegistry = 0x77319B4031e6eF1250907aa00018B8B1c67a244b;
  // Protocol Core - LicenseRegistry
  address internal licenseRegistry = 0x529a750E02d8E2f15649c13D69a465286a780e24;
  // Protocol Core - LicensingModule
  address internal licensingModule = 0x04fbd8a2e56dd85CFD5500A4A4DfA955B9f1dE6f;
  // Protocol Core - PILicenseTemplate
  address internal pilTemplate = 0x2E896b0b2Fdb7457499B56AAaA4AE55BCB4Cd316;
  // Protocol Core - RoyaltyPolicyLAP
  address internal royaltyPolicyLAP = 0xBe54FB168b3c982b7AaE60dB6CF75Bd8447b390E;
  // Revenue Token - WIP
  address internal wip = 0x1514000000000000000000000000000000000000;

  SimpleNFT public SIMPLE_NFT;
  Example public EXAMPLE;

  function setUp() public {
    // this is only for testing purposes
    // due to our IPGraph precompile not being
    // deployed on the fork
    vm.etch(address(0x0101), address(new MockIPGraph()).code);

    EXAMPLE = new Example(ipAssetRegistry, licensingModule, pilTemplate, royaltyPolicyLAP, wip);
    SIMPLE_NFT = SimpleNFT(EXAMPLE.SIMPLE_NFT());
  }

  function test_mintAndRegisterAndCreateTermsAndAttach() public {
    ILicenseRegistry LICENSE_REGISTRY = ILicenseRegistry(licenseRegistry);
    IIPAssetRegistry IP_ASSET_REGISTRY = IIPAssetRegistry(ipAssetRegistry);

    uint256 expectedTokenId = SIMPLE_NFT.nextTokenId();
    address expectedIpId = IP_ASSET_REGISTRY.ipId(block.chainid, address(SIMPLE_NFT), expectedTokenId);

    (uint256 tokenId, address ipId, uint256 licenseTermsId) = EXAMPLE.mintAndRegisterAndCreateTermsAndAttach(alice);

    assertEq(tokenId, expectedTokenId);
    assertEq(ipId, expectedIpId);
    assertEq(SIMPLE_NFT.ownerOf(tokenId), alice);

    assertTrue(LICENSE_REGISTRY.hasIpAttachedLicenseTerms(ipId, pilTemplate, licenseTermsId));
    assertEq(LICENSE_REGISTRY.getAttachedLicenseTermsCount(ipId), 1);
    (address licenseTemplate, uint256 attachedLicenseTermsId) = LICENSE_REGISTRY.getAttachedLicenseTerms({
      ipId: ipId,
      index: 0
    });
    assertEq(licenseTemplate, pilTemplate);
    assertEq(attachedLicenseTermsId, licenseTermsId);
  }

  function test_mintLicenseTokenAndRegisterDerivative() public {
    ILicenseRegistry LICENSE_REGISTRY = ILicenseRegistry(licenseRegistry);
    IIPAssetRegistry IP_ASSET_REGISTRY = IIPAssetRegistry(ipAssetRegistry);

    (uint256 parentTokenId, address parentIpId, uint256 licenseTermsId) = EXAMPLE
    .mintAndRegisterAndCreateTermsAndAttach(alice);

    (uint256 childTokenId, address childIpId) = EXAMPLE.mintLicenseTokenAndRegisterDerivative(
      parentIpId,
      licenseTermsId,
      bob
    );

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
forge test --fork-url https://aeneid.storyrpc.io/ --match-path test/Example.t.sol
```

# Deploy & Verify the Example Contract

The `--constructor-args` come from [Deployed Smart Contracts](doc:deployed-smart-contracts).

```shell
forge create \
  --rpc-url https://aeneid.storyrpc.io/ \
  --private-key $PRIVATE_KEY \
  ./src/Example.sol:Example \
  --verify \
  --verifier blockscout \
  --verifier-url https://aeneid.storyscan.xyz/api/ \
  --constructor-args 0x77319B4031e6eF1250907aa00018B8B1c67a244b 0x04fbd8a2e56dd85CFD5500A4A4DfA955B9f1dE6f 0x2E896b0b2Fdb7457499B56AAaA4AE55BCB4Cd316 0xBe54FB168b3c982b7AaE60dB6CF75Bd8447b390E 0xF2104833d386a2734a4eB3B8ad6FC6812F29E38E
```

If everything worked correctly, you should see something like `Deployed to: 0xfb0923D531C1ca54AB9ee10CB8364b23d0C7F47d` in the console. Paste that address into [the explorer](https://aeneid.storyscan.xyz/) and see your verified contract!

# Great job! :)

<Cards columns={2}>
  <Card title="Completed Code" href="https://github.com/storyprotocol/story-protocol-boilerplate/blob/main/src/Example.sol" icon="fa-thumbs-up" iconColor="#51af51" target="_blank">
    See the completed code.
  </Card>

  <Card title="Video Walkthrough" href="https://www.youtube.com/watch?v=X421IuZENqM" icon="fa-video" iconColor="#FF0000" target="_blank">
    Check out a video walkthrough of this tutorial!
  </Card>
</Cards>