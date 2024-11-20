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
This section demonstrates how to add License Terms to an IPA. By attaching terms, an IPA becomes eligible for licensing creation. Users who then wish to creative derivatives of the IP may then mint licenses, which can be burned to enroll their IPs as derivative IPAs of the original work.

## Prerequisites

* [Setup](doc:python-sdk-setup) the client object.
* An existing IPA (`ipId`). Learn how to register an IP Asset [here](doc:register-an-nft-as-an-ip-asset-python).
* An existing License Terms (`termsId`). Learn how to create a PIL Policy [here](doc:register-pil-terms-python).

# Attach License Terms

Below is a code example to add License Terms to our IP Asset:

```python Python
try:
  response = story_client.License.attachLicenseTerms(
    ip_id="0x431A7Cc86381F9bA437b575D3F9E931652fFbbdd", 
    license_template="0x260B6CB6284c89dbE660c0004233f7bB99B5edE7", 
    license_terms_id=3
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
