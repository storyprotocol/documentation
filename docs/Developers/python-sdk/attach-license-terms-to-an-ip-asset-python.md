---
title: Attach Terms to an IP Asset in Python
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
This section demonstrates how to add License Terms to an IPA. By attaching terms, an IPA becomes eligible for licensing creation. Users who then wish to create derivatives of the IP may then mint licenses, which can be burned to enroll their IPs as derivative IPAs of the original work.

> 📘 Note
>
> [Non-Commercial Social Remixing](https://docs.story.foundation/docs/pil-flavors#flavor-1-non-commercial-social-remixing) License Terms are attached **by default** to every IP Asset.

There are a few different ways you can do this, depending on what works best for you:

1. Attach Existing License Terms to Existing IP Asset
2. Register New IP Asset and Attach License Terms (COMING SOON)
3. Register New Terms and Attach to an Existing IP Asset (COMING SOON)
4. Mint NFT, Register as IP Asset, and Attach Terms

## Prerequisites

* [Setup](doc:python-sdk-setup) the client object.

# Attach License Terms

Below is a code example to add License Terms to our IP Asset:

```python Python
try:
  response = story_client.License.attachLicenseTerms(
    ip_id="0x4c1f8c1035a8cE379dd4ed666758Fb29696CF721", 
    license_template="0x58E2c909D557Cd23EF90D14f8fd21667A5Ae7a93", 
    license_terms_id=1
    print(f"Attached License Terms to IPA at transaction hash {response['txHash']}.")
  )
except Exception as e:
  print("License Terms already attached to this IPA")
```
```python Request Type
AttachLicenseTermsRequest = {
    'ip_id': str,
    'license_template': str,
    'license_terms_id': int,
    'tx_options': dict
}
```
```python Response Type
AttachLicenseTermsResponse = {
    'txHash': str
}
```

# Mint NFT, Register as IP Asset, and Attach Terms

Below is a code example that mints a new NFT, registers it as an IP Asset, and attaches terms to it all in one transaction

```python Python
commercial_remix_terms = {
    "transferable": True,
    "royalty_policy": "0x28b4F70ffE5ba7A26aEF979226f77Eb57fb9Fdb6", # insert RoyaltyPolicyLAP address from https://docs.story.foundation/docs/deployed-smart-contracts
    "default_minting_fee": 0,
    "expiration": 0,
    "commercial_use": True,
    "commercial_attribution": True,
    "commercializer_checker": ZERO_ADDRESS,
    "commercializer_checker_data": ZERO_ADDRESS,
    "commercial_rev_share": 50,  # can claim 50% of derivative revenue
    "commercial_rev_ceiling": 0,
    "derivatives_allowed": True,
    "derivatives_attribution": True,
    "derivatives_approval": False,
    "derivatives_reciprocal": True,
    "derivative_rev_ceiling": 0,
    "currency": "0x12A8b0DcC6e3bB0915638361D9D49942Da07F455",  # insert MockERC20 address from https://docs.story.foundation/docs/deployed-smart-contracts
    "uri": ""
}

response = story_client.IPAsset.mintAndRegisterIpAssetWithPilTerms(
    spg_nft_contract="0xfE265a91dBe911db06999019228a678b86C04959",
    terms=[commercial_remix_terms], # IP already has non-commercial social remixing terms. You can add more here.
    ip_metadata={ # https://docs.story.foundation/docs/ip-asset#adding-nft--ip-metadata-to-ip-asset
        "ipMetadataURI": "test-uri",
        "ipMetadataHash": web3.keccak(text="test-metadata-hash").hex(),
        "nftMetadataHash": web3.keccak(text="test-nft-metadata-hash").hex(),
        "nftMetadataURI": "test-nft-uri",
    }
)

print(f"""
Transaction Hash: {response['txHash']}
IPA ID: {response['ipId']}
Token ID: {response['tokenId']}
License Terms IDs: {response['licenseTermsIds']}
""")
```
```python Request Type
MintAndRegisterIpAssetWithPilTermsRequest = {
    'spg_nft_contract': str,
    'terms': list,
    'ip_metadata': dict,
    'recipient': str,
    'tx_options': dict
}
```
```python Response Type
MintAndRegisterIpAssetWithPilTermsResponse = {
    'txHash': str,
    'ipId': str,
    'tokenId': int,
    'licenseTermsIds': list
}
```
