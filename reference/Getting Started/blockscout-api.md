---
title: Blockscout API
excerpt: Get gas price, average block time, market cap, token price, and more.
deprecated: false
hidden: false
metadata:
  robots: index
---
[Storyscan](https://storyscan.xyz) has a public API endpoint that returns gas price, average block time, market cap, token price (coin gecko), and several other stats: `https://www.storyscan.xyz/api/v2/stats`

Here is an example response :arrow_heading_down:

```json
{
  "average_block_time": 2364,
  "coin_image": "https://coin-images.coingecko.com/coins/images/54035/small/Transparent_bg.png?1738075331",
  "coin_price": "4.83",
  "coin_price_change_percentage": null,
  "gas_price_updated_at": "2025-03-10T14:47:27.175157Z",
  "gas_prices": {
    "slow": 0.1,
    "average": 0.57,
    "fast": 1.05
  },
  "gas_prices_update_in": 11735,
  "gas_used_today": "147032238744",
  "market_cap": "1209228486.984",
  "network_utilization_percentage": 10.8968948333333,
  "secondary_coin_image": null,
  "secondary_coin_price": null,
  "static_gas_price": null,
  "total_addresses": "686024",
  "total_blocks": "1765700",
  "total_gas_used": "0",
  "total_transactions": "5606580",
  "transactions_today": "221320",
  "tvl": null
}
```