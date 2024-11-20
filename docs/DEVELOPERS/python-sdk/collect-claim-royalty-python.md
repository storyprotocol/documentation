---
title: Collect & Claim Royalty in Python
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
This section demonstrates how to collect royalty tokens and then claim actual revenue.

## Prerequisites

- [Setup](doc:python-sdk-setup) the client object.
- Understand the [Royalty Module](doc:royalty-module)

# Collect Royalty Tokens

You can collect royalty tokens from children IP Assets, which represent your share of the royalty %. This is only done one time, and anyone can call it (since it doesn't matter if someone else helps me collect my royalty tokens). The royalty tokens will be deposited to the IP Account.

This function will also claim any outstanding revenue tokens that would normally be claimed with `claimRoyalty`, but since royalty tokens were not yet collected, were not deposited to ancestors.

```python Python
response = story_client.Royalty.collectRoyaltyTokens(
    parent_ip_id="0xA34611b0E11Bba2b11c69864f7D36aC83D862A9c",
    child_ip_id="0x9C098DF37b2324aaC8792dDc7BcEF7Bb0057A9C7"
)

print(f"Collected royalty token {response['royaltyTokensCollected']} at transaction hash {response['txHash']}")
```
```python Request Type
CollectRoyaltyTokensRequest = {
    'parent_ip_id': str,
    'child_ip_id': str,
    'tx_options': dict
}
```
```python Response Type
CollectRoyaltyTokensResponse = {
    'txHash': str,
    'royaltyTokensCollected': int
}
```

# Claim Revenue

To claim your % of actual money paid to child IP Assets, otherwise known as revenue tokens, you can call `claimRoyalty` whenever you want.

However before you do this, you must have `snapshotIds` to provide to the transaction. Anyone can call this function, since it doesn't matter who snapshots. You can get that like so:

```python Python
response = story_client.Royalty.snapshot(
    child_ip_id="0x9C098DF37b2324aaC8792dDc7BcEF7Bb0057A9C7"
)

print(f"Took a snapshot with id {response['snapshotId']} at transaction hash {response['txHash']}")
```
```python Request Type
SnapshotRequest = {
    'child_ip_id': str,
    'tx_options': dict
}
```
```python Response Type
SnapshotResponse = {
    'txHash': str,
    'snapshotId': int
}
```

Now you can `claimRevenue` as shown below. **Only the owner of the royalty tokens at the time of the provided snapshot can call this function.**

```python Python
response = story_client.Royalty.claimRevenue(
    snapshotIds=[1, 2],
    child_ip_id="0x9C098DF37b2324aaC8792dDc7BcEF7Bb0057A9C7",
    token="0xB132A6B7AE652c974EE1557A3521D53d18F6739f"
)

print(f"Claimed revenue token {response['claimableToken']} at transaction hash {response['txHash']}")
```
```python Request Type
ClaimRevenueRequest = {
    'snapshot_ids': list,
    'child_ip_id': str,
    'token': str,
    'tx_options': dict
}
```
```python Response Type
ClaimRevenueResponse = {
    'txHash': str,
    'claimableToken': int
}
```