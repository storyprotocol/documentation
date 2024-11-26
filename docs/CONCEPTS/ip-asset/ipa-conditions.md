---
title: IP Modifications & Restrictions
deprecated: false
hidden: true
metadata:
  robots: index
---
# IP Asset Modifications

All IPs can be modified/customized with the [License Config](doc:license-config-hook), and can **always be changed unless there is a certain condition**:

| **Action**                  | **Conditions**                                     |
| --------------------------- | -------------------------------------------------- |
| Modify License Minting Fee  | N/A                                                |
| Modify Licensing Hook       | N/A                                                |
| Modify `commercialRevShare` | Cannot decrease `commercialRevShare` percentage.   |
| Disable/Enable the license  | License can be disabled or re-enabled at any time. |

## License Hook Modifications

The IP can be further customized or modified using the [License Hook](https://docs.story.foundation/docs/license-config-hook#/licensing-hook). This is a function that gets set within the License Config that gets called before a [License Token](doc:license-token) (or more simply, a "license") is minted. There are various features you can implement with the License Hook, and are **always modifiable**:

| **Feature**         | **Description**                                                                                                     |
| ------------------- | ------------------------------------------------------------------------------------------------------------------- |
| Dynamic License Fee | You can dynamically set the price of a license. For example, it can be updated dynamically via bonding curve logic. |
| Total # of Licenses | You can abort the function based on a maximum number of license tokens that can be minted.                          |
| Specific Receivers  | You can restrict minting of license to a specific receiver.                                                         |
| More...             | Additional licensing hook features can be implemented as required.                                                  |

# Group IPA Restrictions

In addition, [Group IPAs](doc:grouping-module) are subject to the following additional restrictions:

| **Action**                   | **Restriction**                                                                                                                                                                    |
| ---------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Add IP Asset to a Group      | You can only add an IP Asset to a group if that group is not "locked". A group becomes locked once the first license token is minted from it or a derivative is linked to it.      |
| Remove IP Asset from a Group | You can only remove an IP Asset from a group if that group is not "locked". A group becomes locked once the first license token is minted from it or a derivative is linked to it. |