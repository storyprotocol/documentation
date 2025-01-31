---
title: IP Modifications & Restrictions
deprecated: false
exerpt: Learn about the modifications and restrictions for IP Assets.
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# IP Asset Modifications

IP Assets can be modified/customized a few ways. For example, by [setting the License Config](doc:license-config-hook) which allows you to change a few things as you'll see below, changing its metadata, and more. These things can **always be changed unless there is a certain condition**.

<Table align={[null,null,"left"]}>
  <thead>
    <tr>
      <th>
        Action
      </th>

      <th>
        Conditions
      </th>

      <th style={{ textAlign: "left" }}>
        Via The...
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        Modify License Minting Fee
      </td>

      <td>
        N/A
      </td>

      <td style={{ textAlign: "left" }}>
        [License Config](doc:license-config-hook)
      </td>
    </tr>

    <tr>
      <td>
        Modify Licensing Hook
      </td>

      <td>
        N/A
      </td>

      <td style={{ textAlign: "left" }}>
        [License Config](doc:license-config-hook)
      </td>
    </tr>

    <tr>
      <td>
        Modify `commercialRevShare`
      </td>

      <td>
        Cannot **decrease** `commercialRevShare` percentage. You can only increase it.

        However, you **can** set it to 0 to disable the overwrite.
      </td>

      <td style={{ textAlign: "left" }}>
        [License Config](doc:license-config-hook)
      </td>
    </tr>

    <tr>
      <td>
        Disable/Enable the License
      </td>

      <td>
        License can be disabled or re-enabled at any time.

        *Note that disabling a license disallows future licenses from being minted, but does not affect existing ones.*
      </td>

      <td style={{ textAlign: "left" }}>
        [License Config](doc:license-config-hook)
      </td>
    </tr>

    <tr>
      <td>
        Modify Metadata
      </td>

      <td>
        Cannot modify if the metadata is **frozen**. This is done by calling `freezeMetadata` in the [CoreMetadataModule.sol](https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/modules/metadata/CoreMetadataModule.sol).
      </td>

      <td style={{ textAlign: "left" }}>
        [CoreMetadataModule.sol](https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/modules/metadata/CoreMetadataModule.sol)
      </td>
    </tr>
  </tbody>
</Table>

## License Hook Modifications

The IP can be further customized or modified using the [License Hook](https://docs.story.foundation/docs/license-config-hook#/licensing-hook). This is a function that gets set within the License Config that gets called before a [License Token](doc:license-token) (or more simply, a "license") is minted. There are various features you can implement with the License Hook, and are **always modifiable**:

| Feature             | Description                                                                                                         |
| ------------------- | ------------------------------------------------------------------------------------------------------------------- |
| Dynamic License Fee | You can dynamically set the price of a license. For example, it can be updated dynamically via bonding curve logic. |
| Total # of Licenses | You can abort the function based on a maximum number of license tokens that can be minted.                          |
| Specific Receivers  | You can restrict minting of license to a specific receiver.                                                         |
| More...             | Additional licensing hook features can be implemented as required.                                                  |

# Group IPA Restrictions

In addition, [Group IPAs](doc:grouping-module) are subject to the following additional restrictions:

| Action                       | Restriction                                                                                                                                                                        |
| ---------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Add IP Asset to a Group      | You can only add an IP Asset to a group if that group is not "locked". A group becomes locked once the first license token is minted from it or a derivative is linked to it.      |
| Remove IP Asset from a Group | You can only remove an IP Asset from a group if that group is not "locked". A group becomes locked once the first license token is minted from it or a derivative is linked to it. |