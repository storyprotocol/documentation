---
title: License Config / Hook
excerpt: >-
  An optional hook that can be attached to an entire IP Asset or specific
  license for dynamic minting fees.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
## License Config

> 🗒️ Contract
>
> View the smart contract [here](https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/lib/Licensing.sol).

Optionally, you can attach a `LicensingConfig` to an IP Asset (for a specific `licenseTermsId`) which contains fields like a `mintingFee` and a `licensingHook`, as shown below. The `licensingHook` is an address to a smart contract that implements the `ILicensingHook.sol` interface, which contains a `beforeMintLicenseTokens` function which will be run before a user mints a License Token. This means you can insert logic to be run upon minting a license.

```sol Licensing.sol
/// @notice This struct is used by IP owners to define the configuration
/// when others are minting license tokens of their IP through the LicensingModule.
/// When the `mintLicenseTokens` function of LicensingModule is called, the LicensingModule will read
/// this configuration to determine the minting fee and execute the licensing hook if set.
/// IP owners can set these configurations for each License or set the configuration for the IP
/// so that the configuration applies to all licenses of the IP.
/// If both the license and IP have the configuration, then the license configuration takes precedence.
/// @param isSet Whether the configuration is set or not.
/// @param mintingFee The minting fee to be paid when minting license tokens.
/// @param licensingHook  The hook contract address for the licensing module, or address(0) if none
/// @param hookData The data to be used by the licensing hook.
/// @param commercialRevShare The commercial revenue share percentage.
/// @param disabled Whether the license is disabled or not.
/// @param expectMinimumGroupRewardShare The minimum percentage of the group’s reward share
/// (from 0 to 100%, represented as 100 * 10 ** 6) that can be allocated to the IP when it is added to the group.
/// If the remaining reward share in the group is less than the minimumGroupRewardShare,
/// the IP cannot be added to the group.
/// @param expectGroupRewardPool The address of the expected group reward pool.
/// The IP can only be added to a group with this specified reward pool address,
/// or address(0) if the IP does not want to be added to any group.
struct LicensingConfig {
  bool isSet;
  uint256 mintingFee;
  address licensingHook;
  bytes hookData;
  uint32 commercialRevShare;
  bool disabled;
  uint32 expectMinimumGroupRewardShare;
  address expectGroupRewardPool;
}
```

You can also attach the `LicensingConfig` to an IP Asset as a whole, so it will execute on every license terms that belongs to the IP. Note that if both an IP-wide config and license-specific config are set, the license-specific config will take priority.

The hook itself is defined below in a different section. You can see it contains information about the license, who is minting the License Token, and who is receiving it.

## Setting the License Config

You can set the License Config by calling the `setLicenseConfig` function in the [LicensingModule.sol contract](https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/modules/licensing/LicensingModule.sol).

If `licenseTemplate == address(0) && licenseTermsId == 0`, it will set the License Config on the IP Asset as a whole and work for any license attached to the IP Asset. However if specific terms have a License Config set, it will overwrite the IP-wide config.

## Logic that is Possible with License Config

1. **Max Number of Licenses**: The `licensingHook` (described in the next section) is where you can define logic for the max number of licenses that can be minted. For example, reverting the transaction if the max number of licenses has already been minted.
2. **Disallowing Derivatives**: If you register a derivative of an IP Asset, that derivative cannot change its License Terms as described [here](https://docs.story.foundation/docs/license-terms#inherited-license-terms). You can be wondering: "What if I, as a derivative, want to disallow derivatives of myself, but my License Terms allow derivatives and I cannot change this?" To solve this, you can simply set `disabled` to true.
3. **Minting Fee**: Similar to #2 above... what about the minting fee? Although you cannot change License Terms on a derivative IP Asset (and thus the minting fee inside of it), you can change the minting fee for that derivative by modifying the `mintingFee` in the License Config, or returning a `totalMintingFee` from the `licensingHook` (described in the next section).
4. **Dynamic Pricing for Minting a License Token**: Set dynamic pricing for minting a License Token from an IP Asset based on how many total have been minted, how many licenses the user is minting, or even who the user is. All of this data is available in the `licensingHook` (described in the next section).

... and more.

## Licensing Hook

> 🗒️ Contract
>
> View the smart contract [here](https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/interfaces/modules/licensing/ILicensingHook.sol#L26).

The `beforeMintLicenseTokens` function, which acts as a hook, is a function that can be called before a License Token is minted to implement custom logic and determine the final `totalMintingFee` of that License Token. The owner of an IP Asset must set the License Config (of which the hook is contained in), with their own implementation of the `beforeMintLicenseTokens` function, for this to be called.

It can also be used to implement various checks and logic, as [outlined above](https://docs.story.foundation/docs/license-config-hook#logic-that-is-possible-with-license-config).

> 🚧 Warning!
>
> Beware of potentially malicious implementations of external license hooks. Please first verify the code of the hook you choose because it may be not reviewed or audited by the Story team.

```sol ILicensingHook.sol
/// @notice This function is called when the LicensingModule mints license tokens.
/// @dev The hook can be used to implement various checks and determine the minting price.
/// The hook should revert if the minting is not allowed.
/// @param caller The address of the caller who calling the mintLicenseTokens() function.
/// @param licensorIpId The ID of licensor IP from which issue the license tokens.
/// @param licenseTemplate The address of the license template.
/// @param licenseTermsId The ID of the license terms within the license template,
/// which is used to mint license tokens.
/// @param amount The amount of license tokens to mint.
/// @param receiver The address of the receiver who receive the license tokens.
/// @param hookData The data to be used by the licensing hook.
/// @return totalMintingFee The total minting fee to be paid when minting amount of license tokens.
function beforeMintLicenseTokens(
  address caller,
  address licensorIpId,
  address licenseTemplate,
  uint256 licenseTermsId,
  uint256 amount,
  address receiver,
  bytes calldata hookData
) external returns (uint256 totalMintingFee);
```

Note that it returns the `totalMintingFee`. You may be wondering, "I can set the minting fee in the License Terms, in the `LicenseConfig`, and return a dynamic price from `beforeMintLicenseTokens`. What will the final minting fee actually be?" Here is the priority:

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        Minting Fee
      </th>

      <th style={{ textAlign: "left" }}>
        Importance
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        The `totalMintingFee` returned from `beforeMintLicenseTokens`
      </td>

      <td style={{ textAlign: "left" }}>
        Highest Priority
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        The `mintingFee` set in the `LicenseConfig`
      </td>

      <td style={{ textAlign: "left" }}>
        :arrow_down:
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        The `mintingFee` set in the License Terms
      </td>

      <td style={{ textAlign: "left" }}>
        Lowest Priority
      </td>
    </tr>
  </tbody>
</Table>