---
title: Base Module
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
The Base Module provides a standard set of must-have functionalities for all modules registered on Story Protocol. Anyone wishing to create and register a module on Story Protocol must inherit and override the Base Module.

> 🗒️ Contract
>
> View the smart contract [here](https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/modules/BaseModule.sol).

# Key Features

## Simplicity and Flexibility

The BaseModule is intentionally kept simple and generalized. It only implements the ERC165 interface, which is crucial for interface detection. This design choice allows for maximum flexibility when developing more specific modules within the Story Protocol.

## ERC165 Interface Implementation

By implementing the ERC165 interface, BaseModule allows other contracts to query whether it supports a specific interface. This feature is essential for ensuring compatibility and interoperability within the Story Protocol ecosystem and beyond.

```sol BaseModule.sol
abstract contract BaseModule is ERC165, IModule {
    ...
}
```

## `supportsInterface` Function

A key function in the BaseModule is `supportsInterface`, which overrides the ERC165's `supportsInterface` method. This function is crucial for interface detection, allowing the contract to declare support for both its own `IModule` interface and any other interfaces it might inherit.

```sol BaseModule.sol
function supportsInterface(bytes4 interfaceId) public view virtual override(ERC165, IERC165) returns (bool) {
    return interfaceId == type(IModule).interfaceId || super.supportsInterface(interfaceId);
}
```
