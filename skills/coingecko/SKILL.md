---
name: obul-coingecko
description: "USE THIS SKILL WHEN: the user asks for cryptocurrency prices, token data, DEX pool analytics, or trending pools. Provides pay-per-use crypto market data via CoinGecko through the Obul proxy."
homepage: https://www.coingecko.com/en/api/x402
metadata:
  obul-skill:
    emoji: "🦎"
    requires:
      env: [ "OBUL_API_KEY" ]
      primaryEnv: "OBUL_API_KEY"
registries: {}
---

# CoinGecko x402

CoinGecko's x402 API provides pay-per-use access to real-time cryptocurrency prices, on-chain token data, DEX pool
search, and trending pool analytics across 250+ blockchain networks. No CoinGecko API key or subscription required —
each request is paid individually through the Obul proxy.

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

Base URL: `https://proxy.obul.ai/proxy/https/pro-api.coingecko.com/api/v3/x402`

## Common Operations

### Get Simple Price

Fetch current prices for one or more coins by CoinGecko ID. Supports multiple vs currencies and optional market cap,
volume, and 24h change data.

**Pricing:** $0.01

```json
{
  "method": "GET",
  "url": "https://proxy.obul.ai/proxy/https/pro-api.coingecko.com/api/v3/x402/simple/price?ids=bitcoin,ethereum,solana&vs_currencies=usd&include_market_cap=true&include_24hr_vol=true&include_24hr_change=true&include_last_updated_at=true",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  }
}
```

**Response:** JSON object keyed by coin ID, each containing price, market cap, 24h volume, 24h change percentage, and
last updated timestamp.

### Get On-Chain Token Price

Fetch the current price for a specific token by its contract address on a given network.

**Pricing:** $0.01

```json
{
  "method": "GET",
  "url": "https://proxy.obul.ai/proxy/https/pro-api.coingecko.com/api/v3/x402/onchain/simple/networks/base/token_price/0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913?include_market_cap=true&include_24hr_vol=true&include_24hr_change=true",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  }
}
```

**Response:** JSON object with token price in USD, optional market cap, 24h volume, and 24h change for the specified
token on the given network.

### Search DEX Pools

Search for decentralized exchange liquidity pools by token name, symbol, or contract address. Optionally filter by
network.

**Pricing:** $0.01

```json
{
  "method": "GET",
  "url": "https://proxy.obul.ai/proxy/https/pro-api.coingecko.com/api/v3/x402/onchain/search/pools?query=WETH&network=eth",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  }
}
```

**Response:** Array of matching pools with pool address, DEX name, token pair details, liquidity, volume, and price
data.

### Get Trending Pools by Network

Retrieve the currently trending DEX pools on a specific blockchain network.

**Pricing:** $0.01

```json
{
  "method": "GET",
  "url": "https://proxy.obul.ai/proxy/https/pro-api.coingecko.com/api/v3/x402/onchain/networks/base/trending_pools?include_composition=true",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  }
}
```

**Response:** Array of trending pools on the specified network, each with pool address, DEX name, token pair, liquidity,
volume, price change, and optional composition breakdown.

### Get Token Data by Address

Fetch detailed on-chain token information by contract address on a specific network, including metadata, price, and
liquidity composition.

**Pricing:** $0.01

```json
{
  "method": "GET",
  "url": "https://proxy.obul.ai/proxy/https/pro-api.coingecko.com/api/v3/x402/onchain/networks/eth/tokens/0xdac17f958d2ee523a2206206994597c13d831ec7?include_composition=true",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  }
}
```

**Response:** JSON object with token name, symbol, decimals, price, total supply, market cap, liquidity data, and
composition details for the specified token.

## Endpoint Pricing Reference

| Endpoint                                                       | Price | Purpose                                     |
|----------------------------------------------------------------|-------|---------------------------------------------|
| `GET /x402/simple/price`                                       | $0.01 | Aggregated prices for coins by CoinGecko ID |
| `GET /x402/onchain/simple/networks/{id}/token_price/{address}` | $0.01 | On-chain token price by contract address    |
| `GET /x402/onchain/search/pools`                               | $0.01 | Search DEX liquidity pools by query         |
| `GET /x402/onchain/networks/{id}/trending_pools`               | $0.01 | Trending DEX pools on a network             |
| `GET /x402/onchain/networks/{id}/tokens/{address}`             | $0.01 | Detailed on-chain token data by address     |

## When to Use

- **Real-time price checks** — Get current prices for Bitcoin, Ethereum, Solana, or any supported coin without a
  subscription.
- **On-chain token lookups** — Fetch price and metadata for any token by its contract address across 250+ networks.
- **DEX pool discovery** — Search for liquidity pools by token name or address to find trading venues.
- **Trending analysis** — Identify hot pools on specific networks (Base, Ethereum, Solana, etc.) for market monitoring.
- **AI agent data feeds** — Provide autonomous agents with on-demand crypto market data at a predictable per-call cost.
- **Low-volume or exploratory use** — When you need occasional crypto data without committing to a monthly API plan.

## Best Practices

- **Batch coin IDs in simple/price** — Pass multiple comma-separated IDs (`bitcoin,ethereum,solana`) in a single request
  instead of making separate calls for each coin.
- **Use the right endpoint** — Use `/simple/price` for CoinGecko-aggregated prices by coin ID; use
  `/onchain/simple/networks/...` for contract-address-based lookups on specific chains.
- **Specify only needed fields** — Only set `include_market_cap`, `include_24hr_vol`, or `include_24hr_change` to `true`
  when you actually need that data.
- **Use network IDs consistently** — Common network IDs: `eth` (Ethereum), `base` (Base), `solana` (Solana),
  `polygon_pos` (Polygon), `arbitrum` (Arbitrum), `bsc` (BNB Chain).
- **Cache responses when possible** — Token metadata and pool composition don't change frequently. Cache these to reduce
  costs when freshness isn't critical.

## Error Handling

| Error                       | Cause                                         | Solution                                                                                                                  |
|-----------------------------|-----------------------------------------------|---------------------------------------------------------------------------------------------------------------------------|
| `402 Payment Required`      | Payment was not processed or insufficient     | Verify your OBUL_API_KEY is valid and your account has sufficient balance.                                                |
| `404 Not Found`             | Invalid coin ID, network ID, or token address | Double-check the coin ID exists on CoinGecko, the network ID is valid, and the token address is correct for that network. |
| `400 Bad Request`           | Missing or invalid query parameters           | Ensure required parameters like `ids`, `vs_currencies`, or `query` are provided and properly formatted.                   |
| `429 Too Many Requests`     | Rate limit exceeded                           | Add a short delay between requests and avoid unnecessary rapid-fire calls.                                                |
| `500 Internal Server Error` | Upstream CoinGecko service issue              | Wait a few seconds and retry. If persistent, check CoinGecko's status page.                                               |
| `503 Service Unavailable`   | CoinGecko x402 service temporarily down       | This is an experimental service. Retry after a brief wait; fall back to standard CoinGecko API if available.              |
