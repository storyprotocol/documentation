---
title: ModuleRegistry.sol
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
This contract is used to register and track modules in the protocol.

### modules

```solidity
mapping(string => address) modules
```

*Returns the address of a registered module by its name.*

### moduleTypes

```solidity
mapping(address => string) moduleTypes
```

*Returns the module type of a registered module by its address.*

### allModuleTypes

```solidity
mapping(string => bytes4) allModuleTypes
```

*Returns the interface ID of a registered module type.*

### constructor

```solidity
constructor(address governance) public
```

### registerModuleType

```solidity
function registerModuleType(string name, bytes4 interfaceId) external
```

Registers a new module type in the registry associated with an interface.

*Enforced to be only callable by the protocol admin in governance.*

#### Parameters

| Name        | Type   | Description                                       |
| ----------- | ------ | ------------------------------------------------- |
| name        | string | The name of the module type to be registered.     |
| interfaceId | bytes4 | The interface ID associated with the module type. |

### removeModuleType

```solidity
function removeModuleType(string name) external
```

Removes a module type from the registry.

*Enforced to be only callable by the protocol admin in governance.*

#### Parameters

| Name | Type   | Description                                |
| ---- | ------ | ------------------------------------------ |
| name | string | The name of the module type to be removed. |

### registerModule

```solidity
function registerModule(string name, address moduleAddress) external
```

Registers a new module in the registry.

*Enforced to be only callable by the protocol admin in governance.*

#### Parameters

| Name          | Type    | Description                |
| ------------- | ------- | -------------------------- |
| name          | string  | The name of the module.    |
| moduleAddress | address | The address of the module. |

### registerModule

```solidity
function registerModule(string name, address moduleAddress, string moduleType) external
```

Registers a new module in the registry with an associated module type.

#### Parameters

| Name          | Type    | Description                              |
| ------------- | ------- | ---------------------------------------- |
| name          | string  | The name of the module to be registered. |
| moduleAddress | address | The address of the module.               |
| moduleType    | string  | The type of the module being registered. |

### removeModule

```solidity
function removeModule(string name) external
```

Removes a module from the registry.

*Enforced to be only callable by the protocol admin in governance.*

#### Parameters

| Name | Type   | Description             |
| ---- | ------ | ----------------------- |
| name | string | The name of the module. |

### isRegistered

```solidity
function isRegistered(address moduleAddress) external view returns (bool)
```

Checks if a module is registered in the protocol.

#### Parameters

| Name          | Type    | Description                |
| ------------- | ------- | -------------------------- |
| moduleAddress | address | The address of the module. |

#### Return Values

| Name | Type | Description                                                     |
| ---- | ---- | --------------------------------------------------------------- |
| \[0] | bool | isRegistered True if the module is registered, false otherwise. |

### getModule

```solidity
function getModule(string name) external view returns (address)
```

Returns the address of a module.

#### Parameters

| Name | Type   | Description             |
| ---- | ------ | ----------------------- |
| name | string | The name of the module. |

#### Return Values

| Name | Type    | Description                |
| ---- | ------- | -------------------------- |
| \[0] | address | The address of the module. |

### getModuleType

```solidity
function getModuleType(address moduleAddress) external view returns (string)
```

Returns the module type of a given module address.

#### Parameters

| Name          | Type    | Description                |
| ------------- | ------- | -------------------------- |
| moduleAddress | address | The address of the module. |

#### Return Values

| Name | Type   | Description                         |
| ---- | ------ | ----------------------------------- |
| \[0] | string | The type of the module as a string. |

### getModuleTypeInterfaceId

```solidity
function getModuleTypeInterfaceId(string moduleType) external view returns (bytes4)
```

Returns the interface ID associated with a given module type.

#### Parameters

| Name       | Type   | Description                         |
| ---------- | ------ | ----------------------------------- |
| moduleType | string | The type of the module as a string. |

#### Return Values

| Name | Type   | Description                                    |
| ---- | ------ | ---------------------------------------------- |
| \[0] | bytes4 | The interface ID of the module type as bytes4. |

### \_registerModule

```solidity
function _registerModule(string name, address moduleAddress, string moduleType) internal
```

*Registers a new module in the registry.*
