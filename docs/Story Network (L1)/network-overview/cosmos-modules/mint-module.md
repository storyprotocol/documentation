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

<br />

## Begin Block

Minting parameters are calculated and inflation paid at the beginning of each block.

### Inflation amount calculation

Inflation amount is calculated using an "inflation calculation function" that's\
passed to the `NewAppModule` function. If no function is passed, then the SDK's
default inflation function will be used (`DefaultInflationCalculationFn`). In case a custom
inflation calculation logic is needed, this can be achieved by defining and
passing a function that matches `InflationCalculationFn`'s signature.

```go
type InflationCalculationFn func(ctx sdk.Context, minter Minter, params Params, bondedRatio math.LegacyDec) math.LegacyDec
```

<br />

## Parameters

The minting module contains the following parameters:

| Key               | Type            | Example             |
| ----------------- | --------------- | ------------------- |
| MintDenom         | string          | "stake"             |
| InflationsPerYear | string (dec)    | "20000000000000000" |
| BlocksPerYear     | string (uint64) | "10368000"          |

* `MintDenom` is the coin denominator used.
* `InflationsPerYear` is the target inflation per year, in 1e18 decimals.
* `BlocksPerYear` is the target number of blocks per year.

<br />

## Events

The minting module emits the following events:

### BeginBlocker

| Type | Attribute Key | Attribute Value |
| :--- | :------------ | :-------------- |
| mint | amount        | "1000"          |