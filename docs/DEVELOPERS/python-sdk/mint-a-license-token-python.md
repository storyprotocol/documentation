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

## Prerequisites

- [Setup](doc:python-sdk-setup) the client object.
- An IPA that has License Terms added. Learn how to add License Terms to an IPA [here](doc:attach-license-terms-to-an-ip-asset-python).

# Mint License

To mint a License Token, we will need the:

- `licensor_ip_id` - the ipId of the IPA we are minting a License Token from
- `license_template` - the address of the license template
- `license_terms_id` - the id of the License Terms (ex. "1" for Non-Commercial Social Remixing)
- `amount` - # of License Tokens to be minted (usually 1, unless the minter decides to mint a batch and distribute it in another way later on)
- `receiver` - wallet receiving the License Token, usually the wallet address that is executing the transaction

```python Python
response = story_client.License.mintLicenseTokens(
    licensor_ip_id="0x431A7Cc86381F9bA437b575D3F9E931652fFbbdd",  # Licensor IP ID
    license_template="0x260B6CB6284c89dbE660c0004233f7bB99B5edE7",  # License Template address
    license_terms_id=2,  # License Terms ID
    amount=1,  # Amount of license tokens to mint
    receiver="0x14dC79964da2C08b23698B3D3cc7Ca32193d9955"  # Address of the receiver
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