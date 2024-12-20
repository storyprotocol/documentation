---
title: Register PIL Terms in Python
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
This section demonstrates how to register a selection of License Terms using the PIL.

> 🪙 Whitelisted Revenue Tokens
>
> At the moment, a token must be whitelisted by Story Protocol to be used in the royalty module as the `currency` field.
>
> <a href="https://sepolia.etherscan.io/address/0xB132A6B7AE652c974EE1557A3521D53d18F6739f#writeContract" target="_blank">This</a> is the only whitelisted token right now on Sepolia testnet. You can mint tokens directly from that link.

## Prerequisites

* [Setup](doc:python-sdk-setup) the client object.
* If License Terms already exist for the identical set of parameters you intend to create, it is unnecessary to create it again and the function will simply return the existing `licenseTermsId` and an undefined `txHash`. You can use existing License Terms by its `licenseTermsId`.

# Create New License Terms

Create your own PIL Terms that you can later attach to an IP Asset.

```python Python
# Define license terms parameters
license_terms = {
    "transferable": False,
    "royalty_policy": "0x28b4F70ffE5ba7A26aEF979226f77Eb57fb9Fdb6", #insert RoyaltyPolicyLAP address from https://docs.story.foundation/docs/deployed-smart-contracts
    "default_minting_fee": 0,
    "expiration": 0,
    "commercial_use": False,
    "commercial_attribution": False,
    "commercializer_checker": ZERO_ADDRESS,
    "commercializer_checker_data": "0x",
    "commercial_rev_share": 0,
    "commercial_rev_ceiling": 0,
    "derivatives_allowed": False,
    "derivatives_attribution": False,
    "derivatives_approval": False,
    "derivatives_reciprocal": False,
    "derivative_rev_ceiling": 0,
    "currency": "0x12A8b0DcC6e3bB0915638361D9D49942Da07F455", #insert MockERC20 address from https://docs.story.foundation/docs/deployed-smart-contracts 
    "uri": "",
}

response = story_client.License.registerPILTerms(**license_terms)
print(f"PIL Terms registered at transaction hash {response['txHash']} License Terms ID: {response['licenseTermsId']}")
```
```python Request Type
RegisterCommercialUsePILRequest = {
    'default_minting_fee': int,
    'currency': str,
    'royalty_policy': str,
    'tx_options': dict
}
```
```python Response Type
RegisterPILResponse = {
    'txHash': str,
    'licenseTermsId': int
}
```

# Create a Commercial Use License

```python Python
response = story_client.License.registerCommercialUsePIL(
    default_minting_fee=10, # 10 of the currency (using the above currency, 10 MERC20)
    currency="0x12A8b0DcC6e3bB0915638361D9D49942Da07F455", # see the above note on whitelisted revenue tokens
    royalty_policy="0x28b4F70ffE5ba7A26aEF979226f77Eb57fb9Fdb6"
)

print(f"PIL Terms registered at transaction hash {response['txHash']} License Terms ID: {response['licenseTermsId']}")
```
```python Request Type
RegisterCommercialUsePILRequest = {
    'default_minting_fee': int,
    'currency': str,
    'royalty_policy': str,
    'tx_options': dict
}
```
```python Response Type
RegisterPILResponse = {
    'txHash': str,
    'licenseTermsId': int
}
```

# Create a Commercial Remix License

```python Python
response = story_client.License.registerCommercialRemixPIL(
    default_minting_fee=10, # 10 of the currency (using the above currency, 10 MERC20)
    currency="0x12A8b0DcC6e3bB0915638361D9D49942Da07F455", # see the above note on whitelisted revenue tokens
    commercial_rev_share=10,
    royalty_policy="0x28b4F70ffE5ba7A26aEF979226f77Eb57fb9Fdb6"
)

print(f"PIL Terms registered at transaction hash {response['txHash']} License Terms ID: {response['licenseTermsId']}")
```
```python Request Type
RegisterCommercialRemixPILRequest = {
    'default_minting_fee': int,
    'currency': str,
    'commercial_rev_share': int,
    'royalty_policy': str,
    'tx_options': dict
}
```
```python Response Type
RegisterPILResponse = {
    'txHash': str,
    'licenseTermsId': int
}
```

# Create a Non-Commercial Social Remixing License

There are currently no parameters to be passed in here, so this function should not really be used other than to get back the `licenseTermsId` for this license.

```python Python
response = story_client.License.registerNonComSocialRemixingPIL()

print(f"PIL Terms registered at transaction hash {response['txHash']} License Terms ID: {response['licenseTermsId']}")
```
```python Request Type
RegisterNonComSocialRemixingPILRequest = {
    'tx_options': dict
}
```
```python Response Type
RegisterPILResponse = {
    'txHash': str,
    'licenseTermsId': int
}
```