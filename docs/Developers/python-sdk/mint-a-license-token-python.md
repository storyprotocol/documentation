---
title: Mint a License Token in Python
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
This section demonstrates how to mint a License Token for an IPA. You can only mint a License Token for an IPA if it has License Terms attached to it. A License Token is minted as an ERC721 token and contains the necessary licensing details.

> 💰 Paid Licenses
>
> Note that some IP Assets may have license terms attached that require the user minting the license to pay a `mintingFee`. You can see an example of that soon.

> 📘 Max Number of Licenses
>
> If you're curious about setting the maximum number of licesnes that can be created from your IP, check out the [License Config / Hook](doc:license-config-hook) section of our documentation.

## Prerequisites

* [Setup](doc:python-sdk-setup) the client object.
* An IPA that has License Terms added. Learn how to add License Terms to an IPA [here](doc:attach-license-terms-to-an-ip-asset-python).

# Mint License

To mint a License Token, we will need the:

* `licensor_ip_id` - the ipId of the IPA we are minting a License Token from
* `license_template` - the address of the license template
* `license_terms_id` - the id of the License Terms (ex. "1" for Non-Commercial Social Remixing)
* `amount` - # of License Tokens to be minted (usually 1, unless the minter decides to mint a batch and distribute it in another way later on)
* `receiver` - wallet receiving the License Token, usually the wallet address that is executing the transaction

```python Python
response = story_client.License.mintLicenseTokens(
    licensor_ip_id="0xC92EC2f4c86458AFee7DD9EB5d8c57920BfCD0Ba",
    license_template="0x58E2c909D557Cd23EF90D14f8fd21667A5Ae7a93",
    license_terms_id=1,
    amount=1,
    receiver="0x14dC79964da2C08b23698B3D3cc7Ca32193d9955"
)

print(f"License Token minted at transaction hash {response['txHash']} License ID: {response['licenseTokenIds']}")
```
```python Request Type
MintLicenseTokensRequest = {
    'licensor_ip_id': str,
    'license_template': str,
    'license_terms_id': int,
    'amount': int,
    'receiver': str,
    'tx_options': dict
}
```
```python Response Type
MintLicenseTokensResponse = {
    'txHash': str,
    'licenseTokenIds': list
}
```