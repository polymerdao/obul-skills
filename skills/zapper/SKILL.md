---
name: obul-zapper
description: "USE THIS SKILL WHEN: the user asks for wallet portfolio balances, onchain token prices, transaction history, or needs to search addresses/tokens across multiple blockchains. Provides pay-per-use multi-chain data via Zapper through the Obul proxy."
homepage: https://build.zapper.xyz
metadata:
  obul-skill:
    emoji: "⚡"
    requires:
      env: [ "OBUL_API_KEY" ]
      primaryEnv: "OBUL_API_KEY"
registries: {}
---

# Zapper

Zapper aggregates portfolio balances, token prices, NFT metadata, transaction histories, and DeFi positions into a
single GraphQL API across 60+ blockchain networks. Through the Obul proxy, you get pay-per-use access — no Zapper
account or API key required.

The API exposes REST-style endpoints under `/x402/` that accept GraphQL variables in the request body, making each query
a simple POST request.

## Authentication

All requests route through the Obul proxy. Include your Obul API key in every request:

```json
{
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  }
}
```

Base URL: `https://proxy.obul.ai/proxy/https/public.zapper.xyz`

## Common Operations

### Get Token Price

Fetch real-time price, market cap, 24h change, and historical data for any token by contract address and chain ID.

**Pricing:** $0.005

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/public.zapper.xyz/x402/token-price",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "variables": {
      "address": "0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48",
      "chainId": 1
    }
  }
}
```

**Response:** JSON object with token `symbol`, `name`, `decimals`, `imageUrlV2`, and `priceData` containing `price`,
`priceChange5m`, `priceChange1h`, `priceChange24h`, `marketCap`, `totalLiquidity`, and `priceTicks` for charting.

### Get Portfolio Balances

Fetch a wallet's complete portfolio — token balances, DeFi positions, and NFTs — across all supported chains in a single
call. Supports querying multiple addresses at once.

**Pricing:** $0.005

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/public.zapper.xyz/x402/portfolio",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "variables": {
      "addresses": [
        "0xd8dA6BF26964aF9D7eEd9e03E53415D37aA96045"
      ],
      "chainIds": [
        1,
        8453,
        137
      ]
    }
  }
}
```

**Response:** JSON object with `tokenBalances` (total USD value and per-token breakdown), `appBalances` (DeFi positions
like Aave lending, Uniswap LPs with underlying token details), and `nftBalances` (estimated values and collection
metadata). Each section supports grouping by token, account, or network.

### Get Transaction Details

Retrieve human-readable details for a specific on-chain transaction by its hash, including asset transfers, app context,
and formatted descriptions.

**Pricing:** $0.005

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/public.zapper.xyz/x402/transaction-details",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "variables": {
      "hash": "0x1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef",
      "chainId": 1
    }
  }
}
```

**Response:** JSON object with `processedDescription` (human-readable summary), associated `app` info, `timestamp`,
signing and recipient account identities, `inboundAttachments` and `outboundAttachments` (asset movements with token
details and USD values).

### Onchain Search

Search across tokens, NFT collections, accounts, apps, and Farcaster profiles in a single query. Results are ranked by
relevance.

**Pricing:** $0.005

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/public.zapper.xyz/x402/search",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "variables": {
      "search": "USDC",
      "chainIds": [
        1,
        8453
      ],
      "maxResultsPerCategory": 3
    }
  }
}
```

**Response:** Array of results, each with `category` (ERC20_TOKEN, NFT_COLLECTION, USER, APP, FARCASTER_PROFILE, etc.),
`title`, `imageUrl`, `score`, and category-specific fields like `address`, `symbol`, `decimals` for tokens or
`floorPrice` for NFT collections.

## Endpoint Pricing Reference

| Endpoint                         | Price  | Purpose                                             |
|----------------------------------|--------|-----------------------------------------------------|
| `POST /x402/token-price`         | $0.005 | Token price, market data, and historical charts     |
| `POST /x402/portfolio`           | $0.005 | Multi-chain portfolio balances (tokens, DeFi, NFTs) |
| `POST /x402/transaction-details` | $0.005 | Human-readable transaction details by hash          |
| `POST /x402/search`              | $0.005 | Cross-chain search for tokens, NFTs, accounts, apps |

## When to Use

- **Portfolio tracking** — Get a wallet's complete token, DeFi, and NFT holdings across 60+ chains in one call.
- **Token price lookups** — Fetch real-time and historical prices for any on-chain token with market cap and liquidity
  data.
- **Transaction analysis** — Get human-readable summaries of on-chain transactions with structured asset deltas.
- **Onchain discovery** — Search for tokens, NFT collections, accounts, or apps across all supported chains.
- **AI agent data feeds** — Give autonomous agents structured blockchain data at a predictable per-call cost.
- **Low-volume or exploratory use** — When you need occasional blockchain data without committing to a Zapper
  subscription.

## Best Practices

- **Filter by chain IDs** — Always pass `chainIds` when you only need data from specific networks to reduce response
  size and improve speed.
- **Use batch token queries** — When fetching prices for multiple tokens, check if a batch variant is available to
  reduce the number of requests.
- **Pass multiple addresses at once** — The portfolio endpoint accepts an array of addresses. Query them together
  instead
  of making separate calls.
- **Use chain IDs, not network names** — Common chain IDs: `1` (Ethereum), `137` (Polygon), `42161` (Arbitrum), `8453`
  (Base), `10` (Optimism), `43114` (Avalanche), `56` (BNB Chain).
- **Cache static data** — Token metadata (name, symbol, decimals) and NFT collection details rarely change. Cache these
  to reduce costs.

## Error Handling

| Error                       | Cause                                        | Solution                                                                                          |
|-----------------------------|----------------------------------------------|---------------------------------------------------------------------------------------------------|
| `402 Payment Required`      | Payment not processed or insufficient        | Verify your OBUL_API_KEY is valid and your account has sufficient balance.                        |
| `400 Bad Request`           | Missing or invalid variables in request body | Ensure required fields like `address`, `chainId`, or `addresses` are present and correctly typed. |
| `404 Not Found`             | Invalid endpoint path                        | Double-check the endpoint path matches one of the documented routes.                              |
| `422 Unprocessable Entity`  | Valid request but invalid data               | Verify the address is a valid contract/wallet and the chain ID is supported.                      |
| `429 Too Many Requests`     | Rate limit exceeded                          | Add a short delay between requests and avoid rapid-fire calls.                                    |
| `500 Internal Server Error` | Upstream Zapper service issue                | Wait a few seconds and retry. If persistent, the service may be experiencing downtime.            |
