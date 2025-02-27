---
title: 🪝 Hooks
excerpt: Learn about and view the available hooks on Story.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
Hooks are defined as a specialized interface that inherits from the Module framework. They are designed for developers to create custom implementations that integrate seamlessly with existing Modules.

<Image align="center" src="https://files.readme.io/2ea34f7-Screenshot_2024-02-05_at_16.09.49.png" />

## Concept and Functionality

While Modules are the backbone of the Story Protocol, executing actions and managing interactions, Hooks are distinct in their specific focus. They are tailored to verify conditions, an essential feature embodied in their `verify()` functionality. This design choice positions Hooks as a unique subset of Modules, specialized for particular tasks within the ecosystem.

## Design Philosophy

From a structural standpoint, Hooks are not treated as separate entities from Modules. This decision avoids unnecessary complexity in the architecture. Viewing Hooks as specialized Modules allows for a simplified, efficient design that emphasizes clarity in roles and interactions.

## Available Hooks

| Hook                       | Description                                                                            | Contract Code                                                                                                                                              |
| :------------------------- | :------------------------------------------------------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------- |
| LockLicenseHook            | Stop the minting of license tokens or registering new derivatives.                     | <a href="https://github.com/storyprotocol/protocol-periphery-v1/blob/main/contracts/hooks/LockLicenseHook.sol" target="_blank">View here ↗️</a>            |
| TotalLicenseTokenLimitHook | Set a limit on the amount of license tokens that can be minted, updatable at any time. | <a href="https://github.com/storyprotocol/protocol-periphery-v1/blob/main/contracts/hooks/TotalLicenseTokenLimitHook.sol" target="_blank">View here ↗️</a> |