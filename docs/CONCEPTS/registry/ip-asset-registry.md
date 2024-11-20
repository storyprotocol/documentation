---
title: IP Asset Registry
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
> 🗒️ Contract
> 
> View the smart contract [here](https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/registries/IPAssetRegistry.sol).

The IP Asset Registry is responsible for registering IPs into the protocol. It deploys a dedicated [IP Account](doc:ip-account) contract for each new IP Asset registered on the protocol (_NOTE: This registry inherits from IP Account Registry_)

### Notable Functions

```sol IPAssetRegistry.sol
function register(uint256 chainid, address tokenContract, uint256 tokenId) external whenNotPaused returns (address id)
```

This function registers an ERC-721 NFT as a new IP Asset on Story.