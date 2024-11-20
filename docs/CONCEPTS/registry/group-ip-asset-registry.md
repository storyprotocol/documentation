---
title: Group IP Asset Registry
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
> View the smart contract [here](https://github.com/storyprotocol/protocol-core-v1/blob/main/contracts/registries/GroupIPAssetRegistry.sol).

The Group IP Asset Registry is responsible for managing the registration and tracking of Group IP Assets, including the group members and reward pools.

The Group IP Asset Registry will maintain grouping relationship on-chain between the Group's IP Account and individual IP Accounts through a mapping:

```sol GroupIPAssetRegistry.sol
mapping(address groupIpId => EnumerableSet.AddressSet memberIpIds) groups;
```

### Notable Functions

```sol GroupIPAssetRegistry.sol
function registerGroup(address groupNft, uint256 groupNftId, address rewardPool) external onlyGroupingModule whenNotPaused returns (address groupId)
```

This function registers a new Group IPA on Story.

```sol GroupIPAssetRegistry.sol
function addGroupMember(address groupId, address[] calldata ipIds) external onlyGroupingModule whenNotPaused
```

Adds already registered IPAs to an existing Group IPA.

```sol GroupIPAssetRegistry.sol
function removeGroupMember(address groupId, address[] calldata ipIds) external onlyGroupingModule whenNotPaused
```

Removes registered IPAs from a Group IPA.