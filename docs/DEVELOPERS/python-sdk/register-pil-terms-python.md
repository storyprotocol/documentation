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

- [Setup](doc:python-sdk-setup) the client object.
- If License Terms already exist for the identical set of parameters you intend to create, it is unnecessary to create it again and the function will simply return the existing `licenseTermsId` and an undefined `txHash`. You can use existing License Terms by its `licenseTermsId`. 

# Create a Commercial Use License

```python Python
response = story_client.License.registerCommercialUsePIL(
    minting_fee=10, # 10 of the currency (using the above currency, 10 MERC20)
    currency="0xB132A6B7AE652c974EE1557A3521D53d18F6739f", # see the above note on whitelisted revenue tokens
    royalty_policy="0xAAbaf349C7a2A84564F9CC4Ac130B3f19A718E86"
)

print(f"PIL Terms registered at transaction hash {response['txHash']} License Terms ID: {response['licenseTermsId']}")
```
```python Request Type
RegisterCommercialUsePILRequest = {
    'minting_fee': int,
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
    minting_fee=10, # 10 of the currency (using the above currency, 10 MERC20)
    currency="0xB132A6B7AE652c974EE1557A3521D53d18F6739f", # see the above note on whitelisted revenue tokens
    commercial_rev_share=10,
    royalty_policy="0xAAbaf349C7a2A84564F9CC4Ac130B3f19A718E86"
)

print(f"PIL Terms registered at transaction hash {response['txHash']} License Terms ID: {response['licenseTermsId']}")
```
```python Request Type
RegisterCommercialRemixPILRequest = {
    'minting_fee': int,
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