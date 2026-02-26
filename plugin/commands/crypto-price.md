---
description: Look up cryptocurrency price, market cap, and 24h change via CoinGecko ($0.01/request)
---

# Crypto Price Lookup

Fetch current price, market cap, 24h volume, and 24h change for one or more cryptocurrencies using CoinGecko through the Obul proxy.

**Cost:** $0.01 per request

The user provides `$ARGUMENTS` which should be a coin name or comma-separated list (e.g., "bitcoin", "ethereum,solana"). Convert common names to CoinGecko IDs (bitcoin, ethereum, solana, cardano, dogecoin, etc.) and run:

```sh
curl -s "https://proxy.obul.ai/proxy/https/pro-api.coingecko.com/api/v3/x402/simple/price?ids=$ARGUMENTS&vs_currencies=usd&include_market_cap=true&include_24hr_vol=true&include_24hr_change=true&include_last_updated_at=true" \
  -H "Content-Type: application/json" \
  -H "x-obul-api-key: $OBUL_API_KEY"
```

Parse the JSON response and present a clean summary: price, market cap, 24h volume, and 24h change percentage for each token.
