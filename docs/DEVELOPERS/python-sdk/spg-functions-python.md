---
title: SPG Functions
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
A group of functions provided by the [Story Protocol Gateway (SPG) contract](https://github.com/storyprotocol/protocol-periphery-v1/blob/main/contracts/StoryProtocolGateway.sol), which is essentially a way to combine independent operations like [Register an NFT as an IP Asset](doc:register-an-nft-as-an-ip-asset-python) and [Attach License Terms to an IP Asset](doc:attach-license-terms-to-an-ip-asset-python) into one transaction to make your life easier.

> 🚧 Warning!
>
> While these functions are publicly supported in SDK, **it is recommended to NOT use them** in your applications yet. They are still being tested.
>
> Things like the IP-related metadata, represented in the `metadata` and `metadataURI` fields you'll see below, do not have official standards yet although we are working on them.

## Prerequisites

* [Setup](doc:python-sdk-setup) the client object.

# Mint + Register + Metadata + Attach Terms

This function allows you to do all of the following: 

Mint an NFT :arrow_forward: [Register an NFT as an IP Asset](doc:register-an-nft-as-an-ip-asset-python) :arrow_forward: Add Metadata to an IP Asset :arrow_forward: [Attach License Terms to an IP Asset](doc:attach-license-terms-to-an-ip-asset-python)

```python Python
response = story_client.IPAsset.mintAndRegisterIpAssetWithPilTerms(
    nft_contract="0xd516482bef63Ff19Ed40E4C6C2e626ccE04e19ED",  # your NFT contract address
    pil_type="non_commercial_remix",  # the type of the PIL
    metadata={
        'metadataURI': "test-uri",  # uri of IP metadata
        'metadataHash': web3.to_hex(web3.keccak(text="test-metadata-hash")),  # hash of IP metadata
        'nftMetadataHash': web3.to_hex(web3.keccak(text="test-nft-metadata-hash"))  # hash of NFT metadata
    }
)

print(f"Completed at transaction hash {response['txHash']}, NFT Token ID: {response['tokenId']}, IPA ID: {response['ipId']}, License Terms ID: {response['licenseTermsId']}")
```
```python Request Type
mint_and_register_request = {
    'nft_contract': str,
    'pil_type': str,
    'metadata': dict,
    'recipient': str,
    'minting_fee': int,
    'commercial_rev_share': int,
    'currency': str,
    'tx_options': dict
}
```
```python Response Type
mint_and_register_response = {
    'txHash': str,
    'ipId': str,
    'licenseTermsId': int,
    'tokenId': int
}
```

# Register + Metadata + Attach Terms

This function allows you to do all of the following: 

[Register an NFT as an IP Asset](doc:register-an-nft-as-an-ip-asset-python) :arrow_forward: Add Metadata to an IP Asset :arrow_forward: [Attach License Terms to an IP Asset](doc:attach-license-terms-to-an-ip-asset-python)

```python Python
response = story_client.IPAsset.registerIpAndAttachPilTerms(
    nft_contract="0xd516482bef63Ff19Ed40E4C6C2e626ccE04e19ED",  # your NFT contract address
    token_id=127,  # your NFT token ID
    pil_type="non_commercial_remix",  # the type of the PIL
    metadata={
        'metadataURI': "test-uri",  # URI of IP metadata
        'metadataHash': web3.to_hex(web3.keccak(text="test-metadata-hash")),  # hash of IP metadata
        'nftMetadataHash': web3.to_hex(web3.keccak(text="test-nft-metadata-hash"))  # hash of NFT metadata
    }
)

print(f"Completed at transaction hash {response['txHash']}, IP ID: {response['ipId']}, License Terms ID: {response['licenseTermsId']}")
```
```python Request Type
register_ip_and_attach_pil_terms_request = {
    'nft_contract': str,
    'token_id': int,
    'pil_type': str,
    'metadata': dict,
    'deadline': int,
    'minting_fee': int,
    'commercial_rev_share': int,
    'currency': str,
    'tx_options': dict
}
```
```python Response Type
register_ip_and_attach_pil_terms_response = {
    'txHash': str,
    'licenseTermsId': int,
    'ipId': str
}
```

# Register + Derivative

This function allows you to do all of the following: 

[Register an NFT as an IP Asset](doc:register-an-nft-as-an-ip-asset-python) :arrow_forward: [Register an IPA as a Derivative](doc:register-an-ipa-as-a-derivative-python)

```python Python
response = story_client.IPAsset.registerDerivativeIp(
    nft_contract="0xd516482bef63Ff19Ed40E4C6C2e626ccE04e19ED",  # your NFT contract address
    token_id=127,  # your NFT token ID
    deriv_data={
        'parentIpIds': ['0x4c1f8c1035a8cE379dd4ed666758Fb29696CF721'],  # parent IP IDs
        'licenseTermsIds': ['13']  # license terms IDs
    },
    metadata={
        'metadataURI': "test-uri",  # uri of IP metadata
        'metadataHash': web3.to_hex(web3.keccak(text="test-metadata-hash")),  # hash of IP metadata
        'nftMetadataHash': web3.to_hex(web3.keccak(text="test-nft-metadata-hash"))  # hash of NFT metadata
    }
)

print(f"Completed at transaction hash {response['txHash']}, IP ID: {response['ipId']}")
```
```python Request Type
register_derivative_ip_request = {
    'nft_contract': str,
    'token_id': int,
    'deriv_data': dict,
    'metadata': dict,
    'deadline': int,
    'tx_options': dict
}
```
```python Response Type
register_derivative_ip_response = {
    'txHash': str,
    'ipId': str
}
```
