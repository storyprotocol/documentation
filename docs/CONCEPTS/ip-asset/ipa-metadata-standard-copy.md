---
title: IP Modification & Condition
deprecated: false
hidden: true
metadata:
  robots: index
---
All IPs can be modified/customized with [License Config](doc:license-config-hook):

| **Action**                  | **Modifiable Until**                       | **Conditions**                                    |
| --------------------------- | ------------------------------------------ | ------------------------------------------------- |
| Modify license minting fee  | Always modifiable                          | N/A                                               |
| Modify licensing hook       | Always modifiable                          | N/A                                               |
| Modify `commercialRevShare` | Always modifiable (only increases allowed) | Cannot decrease `commercialRevShare` percentage   |
| Disable/Enable the license  | Always modifiable                          | License can be disabled or re-enabled at any time |

The IP can be further customized or modified using the [Licensing Hook](https://docs.story.foundation/docs/license-config-hook#/licensing-hook):

| **Action**                                               | **Modifiable Until** | **Conditions**                                                    |
| -------------------------------------------------------- | -------------------- | ----------------------------------------------------------------- |
| Dynamic bonding curve license minting fee of the license | Always modifiable    | Can be updated dynamically via bonding curve logic                |
| Total license tokens limit of the license                | Always modifiable    | Defines the maximum number of license tokens that can be minted   |
| Licensing Agent                                          | Always modifiable    | Restricts minting of license tokens to a specific receiver        |
| More...                                                  | Always modifiable    | Additional licensing hook features can be implemented as required |

[Group IPAs](doc:grouping-module) are subject to the following additional restrictions:

| **Action**               | **Modifiable Until**                                               | **Conditions**                                                    |
| ------------------------ | ------------------------------------------------------------------ | ----------------------------------------------------------------- |
| Add IP to the group      | Before the first license token is minted or linked by a derivative | No additions allowed once a derivative or license token is minted |
| Remove IP from the group | Before the first license token is minted or linked by a derivative | No removals allowed once a derivative or license token is minted  |