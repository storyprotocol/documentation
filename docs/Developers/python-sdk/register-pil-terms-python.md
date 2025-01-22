---
title: Register License Terms
excerpt: Learn how to register new License Terms in Python.
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

## Prerequisites

* [Setup](doc:python-sdk-setup) the client object.
* If License Terms already exist for the identical set of parameters you intend to create, it is unnecessary to create it again and the function will simply return the existing `licenseTermsId` and an undefined `txHash`. You can use existing License Terms by its `licenseTermsId`.

> 🪙 Whitelisted Revenue Tokens
>
> At the moment, a token must be whitelisted in our protocol to be used in the royalty module as the `currency` field.
>
> Please see the [currently whitelisted revenue tokens](https://docs.story.foundation/docs/royalty-module#whitelisted-revenue-tokens), where you can also mint tokens directly to be used in the below commercial sections.

# Create New License Terms

Create your own [PIL Terms](doc:pil-terms) that you can later attach to an IP Asset.

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
register_pil_terms_request = {
  'transferable': bool,
  'royalty_policy': str,
  'default_minting_fee': int,
  'expiration': int,
  'commercial_use': bool,
  'commercial_attribution': bool,
  'commercializer_checker': str,
  'commercializer_checker_data': str,
  'commercial_rev_share': int,
  'commercial_rev_ceiling': int,
  'derivatives_allowed': bool,
  'derivatives_attribution': bool,
  'derivatives_approval': bool,
  'derivatives_reciprocal': bool,
  'derivative_rev_ceiling': int,
  'currency': str,
  'uri': str,
  'tx_options': dict # optional
}
```
```python Response Type
register_pil_terms_response = {
  'txHash': str,
  'licenseTermsId': int
}
```

# Create a Commercial Use License

* Click [here](https://docs.story.foundation/docs/pil-flavors#flavor-1-non-commercial-social-remixing) to see info on Commercial Use licenses.

```python Python
response = story_client.License.registerCommercialUsePIL(
  default_minting_fee=10, # 10 of the currency (using the above currency, 10 MERC20)
  currency="0x12A8b0DcC6e3bB0915638361D9D49942Da07F455", # see the above note on whitelisted revenue tokens
  royalty_policy="0x28b4F70ffE5ba7A26aEF979226f77Eb57fb9Fdb6"
)

print(f"PIL Terms registered at transaction hash {response['txHash']} License Terms ID: {response['licenseTermsId']}")
```
```python Request Type
register_commercial_use_pil_request = {
  'default_minting_fee': int,
  'currency': str,
  'royalty_policy': str, # optional
  'tx_options': dict # optional
}
```
```python Response Type
register_commercial_use_pil_response = {
  'txHash': str,
  'licenseTermsId': int
}
```

# Create a Commercial Remix License

* Click [here](https://docs.story.foundation/docs/pil-flavors#flavor-1-non-commercial-social-remixing) to see info on Commercial Remix licenses.

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
register_commercial_remix_pil_request = {
  'default_minting_fee': int,
  'currency': str,
  'commercial_rev_share': int,
  'royalty_policy': str,
  'tx_options': dict # optional
}
```
```python Response Type
register_commercial_remix_pil_response = {
  'txHash': str,
  'licenseTermsId': int
}
```

# Create a Non-Commercial Social Remixing License

* Click [here](https://docs.story.foundation/docs/pil-flavors#flavor-1-non-commercial-social-remixing) to see info on Non-Commercial Social Remixing licenses.

There are currently no parameters to be passed in here, so this function should not really be used other than to get back the `licenseTermsId` for this license.

In addition, NCSR terms are already attached to every single IP Asset **by default**, with `licenseTermsId == 1`.

```python Python
response = story_client.License.registerNonComSocialRemixingPIL()

print(f"PIL Terms registered at transaction hash {response['txHash']} License Terms ID: {response['licenseTermsId']}")
```
```python Request Type
register_non_com_social_remixing_pil_request = {
  'tx_options': dict # optional
}
```
```python Response Type
register_non_com_social_remixing_pil_response = {
  'txHash': str,
  'licenseTermsId': int
}
```