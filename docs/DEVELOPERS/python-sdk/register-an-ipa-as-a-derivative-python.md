---
title: Register an IPA as a Derivative in Python
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
This section demonstrates how to register an IP Asset as a derivative of another. There are generally three methods of registering an IPA remix:

1. Link an existing IPA as a derivative of a "parent" IPA using a License Token.
2. Link an existing IPA as a derivative of a "parent" IPA directly.

## Prerequisites

- [Setup](doc:python-sdk-setup) the client object.
- The "parent" IPA must be registered.
- The "child" IPA must be registered.

# Register Derivative using License Token

## Additional Prerequisites

- An already minted License Token from the "parent" IPA. Learn how to mint a License Token [here](doc:mint-a-license-token-python).

If you already have a License Token, it is easier to register a derivative this way. We can register a child IPA as a derivative of a parent IPA by calling `client.ipAsset.registerDerivativeWithLicenseTokens()` like so:

```python Python
response = story_client.IPAsset.registerDerivativeWithLicenseTokens(
    child_ip_id="0xDd4330c5aeA6ab3a3E9620D2c74799bAb3BD117D",
    license_token_ids=[1199]
)

print(f"Derivative IPA linked to parent at transaction hash {response['txHash']}")
```
```python Request Type
RegisterDerivativeWithLicenseTokensRequest = {
    'child_ip_id': str,
    'license_token_ids': list,
    'tx_options': dict
}
```
```python Response Type
RegisterDerivativeWithLicenseTokensResponse = {
    'txHash': str
}
```

# Register Derivative Directly

You can also register a derivative directly, without needing a License Token. There is no real difference between `registerDerivativeWithLicenseTokens` (above) and `registerDerivative` (below) except that the former requires an already minted License Token and the latter is a convenient function that handles it for you.

> ❓ "Why would I ever use a License Token then?"
> 
> There are a few times when you would need a License Token to register a derivative:
> 
> - The License Token contains private license terms, so you would only be able to register if you had the License Token that was manually minted by the owner. More on that [here](https://docs.story.foundation/docs/licensing-module#private-licenses).
> - The License Token (which is an NFT) costs a `mintingFee` to mint, and you were able to buy it on a marketplace for a cheaper price. Then it makes more sense to simply register with the License Token then have to pay the more expensive `mintingFee`.

> 📘 Quick Note
> 
> Remember that once License Terms are attached to an IP Asset, it becomes public to register a derivative with those terms, which is why you don't necessarily need a License Token first. Read more on that [here](https://docs.story.foundation/v1/docs/licensing-module#license-terms-attached-to-ip-asset) .

```python Python
response = story_client.IPAsset.registerDerivative(
    child_ip_id="0xc478561B1A18331d92b2b6f6a863dbAA874729c3",
    parent_ip_ids=["0x567603411Fb957759Ac2090659B73cC5f099456D"],
    license_terms_ids=[2],
    license_template="0x260B6CB6284c89dbE660c0004233f7bB99B5edE7"
)

print(f"Derivative IPA linked to parent at transaction hash {response['txHash']}")
```
```python Request Type
RegisterDerivativeRequest = {
    'child_ip_id': str,
    'parent_ip_ids': list,
    'license_terms_ids': list,
    'license_template': str,
    'tx_options': dict
}
```
```python Response Type
RegisterDerivativeResponse = {
    'txHash': str
}
```