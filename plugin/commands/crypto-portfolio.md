---
description: Fetch wallet portfolio balances across 60+ chains via Zapper ($0.005/request)
---

# Crypto Portfolio

Fetch a wallet's complete portfolio — token balances, DeFi positions, and NFTs — across all supported chains using Zapper through the Obul proxy.

**Cost:** $0.005 per request

The user provides `$ARGUMENTS` which should be a wallet address (e.g., `0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045`). Run:

```sh
curl -s -X POST "https://proxy.obul.ai/proxy/https/public.zapper.xyz/x402/portfolio" \
  -H "Content-Type: application/json" \
  -H "x-obul-api-key: $OBUL_API_KEY" \
  -d '{"variables": {"addresses": ["'"$ARGUMENTS"'"], "chainIds": [1, 8453, 137, 42161, 10]}}'
```

Parse the JSON response and present a clean portfolio summary: total USD value, top token holdings with amounts and values, and any DeFi positions.
