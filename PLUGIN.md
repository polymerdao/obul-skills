# Obul Plugin for Claude Code

Pay-per-use API access for Claude Code agents — web scraping, crypto data, X/Twitter search, headless browsers, and AI models. No external accounts needed, just an `OBUL_API_KEY`.

## Prerequisites

1. Sign up at [my.obul.ai](https://my.obul.ai) (free, ~30 seconds)
2. Set your API key:
   ```sh
   export OBUL_API_KEY="your-key-here"
   ```

## Installation

Add this repo as a marketplace, then install:

```sh
claude plugin marketplace add /path/to/obul-skills
claude plugin install obul
```

Or from a git remote:

```sh
claude plugin marketplace add https://github.com/anthropics/obul-skills
claude plugin install obul
```

## Available Skills

| Skill | Description | Pricing |
|---|---|---|
| obul-firecrawl | Web scraping, crawling, site mapping, search, structured extraction | $0.001–$0.01 |
| obul-browserbase | Headless browser sessions with Chrome DevTools Protocol | $0.01–$0.05 |
| obul-coingecko | Crypto prices, on-chain token data, DEX pool analytics | $0.01 |
| obul-minimax | MiniMax M2.5 chat completions (200K context, reasoning) | $0.001 |
| obul-proxy | Generic x402 proxy for any supported upstream service | Varies |
| obul-x-search | X/Twitter search, user profiles, tweets, timelines, trends | $0.001 |
| obul-zapper | Multi-chain portfolio balances, token prices, onchain search | $0.005–$0.01 |

## Commands

| Command | Description | Cost |
|---|---|---|
| `/obul:scrape <url>` | Scrape a URL to clean markdown | $0.001 |
| `/obul:search <query>` | Web search with scraped results | $0.002 |
| `/obul:x-search <query>` | Search X/Twitter for tweets | $0.001 |
| `/obul:crypto-price <token>` | Crypto price, market cap, 24h change | $0.01 |
| `/obul:crypto-portfolio <address>` | Wallet portfolio across 60+ chains | $0.005 |
| `/obul:browse <url>` | Open URL in cloud browser, screenshot + content | $0.051 |
| `/obul:browser-session` | Create cloud browser session, return CDP URL | $0.05 |

## How It Works

All requests route through the [Obul proxy](https://obul.ai), which handles [x402](https://www.x402.org/) payment negotiation automatically. You make normal HTTP requests with your `OBUL_API_KEY` — no crypto wallets, no USDC, no gas fees.

```
https://proxy.obul.ai/proxy/{scheme}/{host}{path}
```

## Resources

- [Obul Dashboard](https://my.obul.ai) — manage keys, view usage, set spending caps
- [x402 Protocol](https://www.x402.org/) — the payment protocol powering Obul
- [Skill Definitions](skills/) — full API reference for each service
