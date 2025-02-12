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
<Cards columns={1}>
  <Card title="IPAssetRegistry.sol" href="https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/registries/IPAssetRegistry.sol" icon="fa-scroll" iconColor="#ccb092" target="_blank">
    View the smart contract for the IP Asset Registry.
  </Card>
</Cards>

The IP Asset Registry is responsible for registering IPs into the protocol. It deploys a dedicated [IP Account](doc:ip-account) contract for each new IP Asset registered on the protocol (*NOTE: This registry inherits from the* [IP Account Registry](https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/registries/IPAccountRegistry.sol))

### Notable Functions

```sol IPAssetRegistry.sol
function register(uint256 chainid, address tokenContract, uint256 tokenId) external whenNotPaused returns (address id)
```

This function registers an ERC-721 NFT as a new IP Asset on Story.