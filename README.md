# Obul Skills

Obul is the **universal API gateway for the agent economy**. It proxies requests to x402-enabled APIs and handles payment automatically — no crypto wallet, no USDC, no gas fees. One `OBUL_API_KEY`, consolidated billing, scoped keys with spending caps. Pay-per-use access to any supported service.

## What is x402?

[x402](https://www.x402.org/) is an open payment protocol (by Coinbase) that uses HTTP 402 for instant, per-request stablecoin payments. Obul abstracts x402 away entirely — you just make normal HTTP requests.

## Getting Started

1. **Sign up** at [my.obul.ai](https://my.obul.ai) (free, ~30 seconds)
2. **Set your API key** as an environment variable:
   ```sh
   export OBUL_API_KEY="your-key-here"
   ```
3. **Make requests** through the proxy. URL pattern:
   ```
   https://proxy.obul.ai/proxy/{scheme}/{host}{path}
   ```
   All requests must include the `x-obul-api-key` header.

## Available Skills

| | Skill | Description |
|---|---|---|
| 🔗 | [obul-proxy](skills/obul-proxy/SKILL.md) | Proxy x402 requests through Obul with automatic payment handling |
| 🌐 | [browserbase](skills/browserbase/SKILL.md) | Headless browser sessions for web scraping and automation |
| 🦎 | [coingecko](skills/coingecko/SKILL.md) | Real-time crypto market data, token prices, and DEX analytics |
| 🔥 | [firecrawl](skills/firecrawl/SKILL.md) | Web scraping, crawling, site mapping, search, and structured extraction |
| 🧠 | [minimax](skills/minimax/SKILL.md) | MiniMax M2.5 chat completions with 200K context and reasoning |
| 🔍 | [x-search](skills/x-search/SKILL.md) | X/Twitter search, user profiles, and trending topics |
| ⚡ | [zapper](skills/zapper/SKILL.md) | Multi-chain portfolio balances, token prices, and onchain data |

## Usage Example

```sh
curl -X POST "https://proxy.obul.ai/proxy/https/www.browserbase.com/v1/sessions" \
  -H "Content-Type: application/json" \
  -H "x-obul-api-key: $OBUL_API_KEY" \
  -d '{"projectId": "default"}'
```

## Resources

- [Obul Dashboard](https://my.obul.ai) — manage keys, view usage, set spending caps
- [x402 Protocol](https://www.x402.org/) — the payment protocol powering Obul
- [Writing a New Skill](https://github.com/dpbmaverick98/Agent_Army_Skills/blob/main/Agent_Army_Skills_Obul/how-to-write-obul-skills.md) — guide for contributors
