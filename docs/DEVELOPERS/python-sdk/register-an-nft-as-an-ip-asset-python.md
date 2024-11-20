---
title: Register an IP Asset in Python
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
This section demonstrates how to register an IP Asset using the Python SDK. There are generally two methods of IP registration:

1. Register an existing NFT as an IP Asset
2. Create an NFT and register it as an IP Asset in one transaction (**NOT IMPLEMENTED YET**)

## Prerequisites

- [Setup](doc:python-sdk-setup) the client object.

# Register an Existing NFT as an IP Asset

You can register your NFT as an IP Asset by simply calling `story_client.IPAsset.register()` and passing in the token's contract address and token ID, like so:

> :information_source: If the provided `token_id` from the `token_Contract` has already been registered, the `response` object will contain the `ipId` of the existing IP Asset and a `txHash` as `None`

```python Python
response = story_client.IPAsset.register(
    token_contract="0xd516482bef63Ff19Ed40E4C6C2e626ccE04e19ED", # your NFT contract address
    token_id=12, # your NFT token ID
    metadata={
        'metadataURI': "test-uri", #uri of IP metadata
        'metadataHash': web3.to_hex(web3.keccak(text="test-metadata-hash")), #hash of IP metadata
        'nftMetadataHash': web3.to_hex(web3.keccak(text="test-nft-metadata-hash")) #hash of NFT metadata
    }
)

print(f"Root IPA created at transaction hash {response['txHash']}")
print(f"IPA ID: {response['ipId']}")
```
```python Request Type
register_request = {
    'token_contract': str,
    'token_id': int,
    'metadata': dict,
    'deadline': int,
    'tx_options': dict
}
```
```python Response Type
register_response = {
    'txHash': str,
    'ipId': str
}
```

After we run the above code, the console output will look like:

```text Console Output
Root IPA created at transaction hash 0xa3e1caa7c2124b1550d459abc739291cb1be77ac73b99c097707878ac4ef57ae,
IPA ID: 0x4c1f8c1035a8cE379dd4ed666758Fb29696CF721
```