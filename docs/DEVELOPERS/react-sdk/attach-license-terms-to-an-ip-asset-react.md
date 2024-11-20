---
title: Attach Terms to an IP Asset in React
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

- [React SDK Setup](doc:react-sdk-setup)
- An existing IPA (`ipId`). Learn how to register an IP Asset [here](doc:register-an-nft-as-an-ip-asset-react).
- An existing License Terms (`termsId`). Learn how to create PIL Terms [here](doc:register-pil-terms-react).

# Attach License Terms

Below is a code example to add License Terms to our IP Asset:

```jsx AttachTerms.tsx
import { useLicense } from "@story-protocol/react-sdk";

export default async function AttachTerms() {
  const { attachLicenseTerms } = useLicense();
  
  const response = await attachLicenseTerms({
     licenseTermsId: "1", 
     ipId: "0x4c1f8c1035a8cE379dd4ed666758Fb29696CF721",
     txOptions: { waitForTransaction: true }
  });
  
  if (response.success) {
    console.log(`Attached License Terms to IPA at transaction hash ${response.txHash}.`)
  } else {
    console.log(`License Terms already attached to this IPA.`)
  }
  
  return (
    {/* */} 
  )
}
```
```typescript Request Type
export type AttachLicenseTermsRequest = {
  ipId: Address;
  licenseTermsId: string | number | bigint;
  licenseTemplate?: Address;
  txOptions?: TxOptions;
};
```
```typescript Response Type
export type AttachLicenseTermsResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  success?: boolean;
};
```

_Note: [Non-Commercial Social Remixing](https://docs.story.foundation/docs/pil-flavors#flavor-1-non-commercial-social-remixing) License Terms are attached **by default** to every IP Asset._