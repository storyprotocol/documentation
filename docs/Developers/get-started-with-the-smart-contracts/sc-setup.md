---
title: Setup Your Own Project
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
In this guide, we will show you how to setup the Story smart contract development environment in just a few minutes

### :warning: Prerequisites

* [Install Foundry](https://book.getfoundry.sh/getting-started/installation)
* [Install yarn](https://classic.yarnpkg.com/lang/en/docs/install/)

## Creating a Project

1. Run `foundryup` to automatically install the latest stable version of the precompiled binaries: forge, cast, anvil, and chisel
2. Run the following command in a new directory: `forge init`. This will create a `foundry.toml` and example project files in the project root. By default, forge init will also initialize a new git repository.
3. Initialize a new yarn project: `yarn init`. Alternatively, you can use `npm init` or `pnpm init`.
4. Open up your `foundry.toml` file and replace it with this

```toml foundry.toml
[profile.default]
out = 'out'
libs = ['node_modules', 'lib']
cache_path  = 'forge-cache'
gas_reports = ["*"]
optimizer = true
optimizer_runs = 20000
test = 'test'
solc = '0.8.26'
fs_permissions = [{ access = 'read', path = './out' }, { access = 'read-write', path = './deploy-out' }]
evm_version = 'cancun'
remappings = [
    '@openzeppelin/=node_modules/@openzeppelin/', 
    '@storyprotocol/core/=node_modules/@story-protocol/protocol-core/contracts/', 
    '@storyprotocol/periphery/=node_modules/@story-protocol/protocol-periphery/contracts/', 
    'erc6551/=node_modules/erc6551/', 
    'forge-std/=node_modules/forge-std/src/', 
    'ds-test/=node_modules/ds-test/src/', 
    '@storyprotocol/test/=node_modules/@story-protocol/protocol-core/test/foundry/', 
    '@solady/=node_modules/solady/'
]
```

5. Remove the example contract files: `rm src/Counter.sol script/Counter.s.sol test/Counter.t.sol`

### :package: Installing Dependencies

Now, we are ready to start installing our dependencies. To incorporate the Story Protocol core and periphery modules, run the following to have them added to your `package.json`. We will also install `openzeppelin` and `erc6551` as a dependency for the contract and test.

```powershell Terminal
# note: you can run them one-by-one, or all at once
yarn add @story-protocol/protocol-core@https://github.com/storyprotocol/protocol-core-v1
yarn add @story-protocol/protocol-periphery@https://github.com/storyprotocol/protocol-periphery-v1
yarn add @openzeppelin/contracts
yarn add @openzeppelin/contracts-upgradeable
yarn add erc6551
yarn add solady
```

Additionally, for working with Foundry's test kit, we also recommend adding the following `devDependencies`:

```Text Terminal
yarn add -D https://github.com/dapphub/ds-test
yarn add -D github:foundry-rs/forge-std#v1.7.6
```

Now we are ready to build a simple test registration contract!
