---
title: Licensing and Royalties
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
## Licensing and Royalties Call Flow

![](https://files.readme.io/20dce78-image.png)

Whenever a user wants to mint a commercial license or links to parents using a commercial license, then the licensing module will call the royalty module, which will perform checks and redirect the call to the appropriate royalty policy contract - which for the beta version is always the `RoyaltyPolicyLAP.sol`. As more royalty policies are added in the future new options will be available.

In the call flows above there is a need to pass in the variable `bytes[] royaltyContext`. Each royalty policy will decode this variable in its custom way - in the case of the royalty policy LAP - the decoding happens in the form of the struct below:

```sol
struct InitParams {
    address[] targetAncestors; // the expected ancestors addresses of an ipId
    uint32[] targetRoyaltyAmount; // the expected royalties of each of the ancestors for a given ipId
    address[] parentAncestors1; // addresses of the ancestors of the first parent
    address[] parentAncestors2; // addresses of the ancestors of the second parent
    uint32[] parentAncestorsRoyalties1; // the royalties of each of the first parent ancestors
    uint32[] parentAncestorsRoyalties2; // the royalties of each of the second parent ancestors
}
```

Let's give an example - in the IP graph below let's imagine IP Asset 0 is linking to parents IPAs 1 and 2:

![](https://files.readme.io/3e07223-image.png)

In the example mentioned the struct `InitParams` then would look like this:

* `targetAncestors = [0x1,0x3,0x7,0x8,0x4,0x9,0x10,0x2,0x5,0x11,0x12,0x6,0x13,0x14] ` | When linking IPA 0 to its new parents IPAs 1 and 2

if we assume (for example) that the royalties demanded by each IP are equal to 5% if the IP Asset number has 2 digits and 1% if lower we get:

* `targetRoyaltyAmount= [10,10,10,10,10,10,50,10,10,50,50,10,50,50]` \| `targetRoyaltyAmount` is a mirror array of `targetAncestors` and shows how many RNFTs each ancestor address demands from IP Asset 0
* `parentAncestors1 = [0x3,0x7,0x8,0x4,0x9,0x10]`
* `parentAncestorsRoyalties1 = [10,10,10,10,10,50]`
* `parentAncestors2=[0x5,0x11,0x12,0x6,0x13,0x14]`
* `parentAncestorsRoyalties2 =[10,50,50,10,50,50]`

The struct `InitParams` is prepared under the hood by the SDK. More information in [SDK documentation](doc:sdk-overview)
