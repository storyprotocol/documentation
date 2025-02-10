---
title: mint
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
## Contents

1. [Contents](#contents)
2. [State](#state)
3. [Begin Block](#begin-block)
4. [Parameters](#parameters)
5. [Events](#events)

## State

### Params

* Params: `mint/params -> legacy_amino(params)`

```protobuf
message Params {
  option (amino.name) = "client/x/mint/Params";

  // type of coin to mint
  string mint_denom = 1;
  // inflation amount per year
  string inflations_per_year = 2 [
    (cosmos_proto.scalar)  = "cosmos.Dec",
    (gogoproto.customtype) = "cosmossdk.io/math.LegacyDec",
    (gogoproto.nullable)   = false
  ];
  // expected blocks per year
  uint64 blocks_per_year = 3;
}
```