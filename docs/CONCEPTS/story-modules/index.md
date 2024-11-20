---
title: 🧱 Modules
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
Modules are standalone contracts that adhere to the [`IModule` interface](https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/interfaces/modules/base/IModule.sol). These modules play a crucial role in taking action on IP to change the data/state around or of IP.

# Existing Modules

There are a few important modules, created by the Story team, to be aware of:

## [📜 Licensing Module](doc:licensing-module)

Responsible for defining and attaching licenses to IP Assets.

## [💸 Royalty Module](doc:royalty-module)

Responsible for handling royalty flow between parent & child IP Assets.

## [❌ Dispute Module](doc:dispute-module)

Responsible for handling the dispute of wrongfully registered or misbehaved IP Assets.

## [👥 Grouping Module](doc:grouping-module)

Responsible for handling groups of IPAs.
