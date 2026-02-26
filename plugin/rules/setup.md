---
description: OBUL_API_KEY setup and verification
---

# Obul API Key Setup

## Required Environment Variable

All Obul skills require `OBUL_API_KEY` to be set. This key authenticates requests to the Obul proxy, which handles x402 payments automatically.

## Getting Your Key

1. Sign up at [my.obul.ai](https://my.obul.ai) (free, ~30 seconds)
2. Copy your API key from the dashboard

## Setting the Key

Add to your shell profile (`~/.zshrc`, `~/.bashrc`, or `~/.profile`):

```sh
export OBUL_API_KEY="your-key-here"
```

Or set it in `.claude/.env` for Claude Code:

```
OBUL_API_KEY=your-key-here
```

## Verification

Test your key with a simple request:

```sh
curl -s "https://proxy.obul.ai/proxy/https/firecrawl.x402endpoints.com/v1/scrape" \
  -H "Content-Type: application/json" \
  -H "x-obul-api-key: $OBUL_API_KEY" \
  -d '{"url": "https://example.com", "formats": ["markdown"]}' | head -c 200
```

A successful response returns JSON with scraped content. If you see an authentication error, verify your key is correct.

## Troubleshooting

| Problem | Solution |
|---|---|
| `401 Unauthorized` | Check that `OBUL_API_KEY` is set and valid |
| `402 Payment Required` | Your Obul balance may be depleted — top up at [my.obul.ai](https://my.obul.ai) |
| `403 Forbidden` | Your scoped key may not have access to this service |
| `OBUL_API_KEY` not found | Restart your shell or Claude Code after setting the variable |

## Spending Controls

Obul supports scoped API keys with spending caps. Create restricted keys in the [dashboard](https://my.obul.ai) to limit per-key or per-day spending.
