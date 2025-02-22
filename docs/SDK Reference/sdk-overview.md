---
title: Overview
excerpt: View every single function in our SDK with examples and types.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
This section provides a detailed description of every function in our SDK.

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        Package
      </th>

      <th style={{ textAlign: "left" }}>
        Status
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        🔷 TypeScript
      </td>

      <td style={{ textAlign: "left" }}>
        :white_check_mark: Full Compatibility
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        :snake: Python
      </td>

      <td style={{ textAlign: "left" }}>
        :warning: Partial Compatibility (some functions are not yet released)
      </td>
    </tr>
  </tbody>
</Table>

***

<Cards columns={3}>
  <Card title="TypeScript NPM Package" href="https://www.npmjs.com/package/@story-protocol/core-sdk" icon="fa-home" iconColor="red" target="_blank">
    View our `@story-protocol/core-sdk` package on npm.
  </Card>

  <Card title="Python Package" href="https://pypi.org/project/story-protocol-python-sdk" icon="fa-home" iconColor="#0273b7" target="_blank">
    View our `story_protocol_python_sdk` package on pypi.
  </Card>

  <Card title="Step-by-Step Guide" href="https://docs.story.foundation/docs/typescript-sdk" icon="fa-home" target="_blank">
    Learn our SDK through a series of tutorials with the SDK Guide.
  </Card>
</Cards>

Interact with the [📜 Licensing Module](doc:licensing-module):

* [Register an IP Asset](doc:sdk-ipasset)
* [Mint & Attach License Terms](doc:sdk-license)

Interact with the [💸 Royalty Module](doc:royalty-module):

* [Pay & Claim Royalty](doc:sdk-royalty)

Interact with the [❌ Dispute Module](doc:dispute-module):

* [Raise a Dispute](doc:sdk-dispute)

Interact with the [👥 Grouping Module](doc:grouping-module):

* [Manage Groups](doc:sdk-group)

And lastly, some utility/extra clients:

* [Set Permissions](doc:sdk-permissions)
* [NFT Client](doc:sdk-nftclient)
* [WIP Client](doc:wip-client)