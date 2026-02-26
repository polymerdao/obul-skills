---
name: minimax
description: Call MiniMax M2.5 (200k context, reasoning) via the minimaxxing x402 endpoint through the Obul proxy. Includes OpenClaw model provider setup.
homepage: https://x402endpoints.com
metadata:
  obul-skill:
    emoji: "🧠"
    requires:
      env: [ "OBUL_API_KEY" ]
      primaryEnv: "OBUL_API_KEY"
registries: {}
---

# MiniMax

MiniMax M2.5 is a reasoning-capable frontier model with a 200k token context window and 128k max output tokens,
available via `minimaxxing.x402endpoints.com`. The endpoint uses the x402 USDC micropayment protocol — $0.001
per request on Base mainnet. Route through the Obul proxy to handle x402 payments automatically with just an
API key.

## Authentication

All requests route through the Obul proxy, which handles x402 payment negotiation automatically. Include your
Obul API key in every request:

```json
{
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}",
    "anthropic-version": "2023-06-01"
  }
}
```

Base URL: `https://proxy.obul.ai/proxy/https/minimaxxing.x402endpoints.com`

The Anthropic Messages API is at `/v1/messages`. There is no `/anthropic` prefix.

## Common Operations

### Health Check

Verify the endpoint is operational.

**Pricing:** $0.00

```json
{
  "method": "GET",
  "url": "https://proxy.obul.ai/proxy/https/minimaxxing.x402endpoints.com/health",
  "headers": {
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  }
}
```

**Response:** `{"status":"ok","model":"MiniMax-M2.5"}`

### Chat Completion (Anthropic Messages API)

Send a message to MiniMax M2.5 using the Anthropic-compatible Messages API.

**Pricing:** $0.001 per request

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/minimaxxing.x402endpoints.com/v1/messages",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}",
    "anthropic-version": "2023-06-01"
  },
  "body": {
    "model": "MiniMax-M2.5",
    "max_tokens": 1024,
    "messages": [
      { "role": "user", "content": "Hello, what can you do?" }
    ]
  }
}
```

**Response:** Anthropic-format `message` object with a `content` array. Note: MiniMax may include a `thinking`
block alongside the `text` block when reasoning is active. The response also includes `usage` with
`input_tokens` and `output_tokens`.

### Chat Completion with System Prompt

Include a system prompt to set the model's behavior.

**Pricing:** $0.001 per request

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/minimaxxing.x402endpoints.com/v1/messages",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}",
    "anthropic-version": "2023-06-01"
  },
  "body": {
    "model": "MiniMax-M2.5",
    "max_tokens": 2048,
    "system": "You are a concise analyst. Respond in bullet points.",
    "messages": [
      { "role": "user", "content": "Summarize the key risks of DeFi protocols." }
    ]
  }
}
```

### Check Pricing

Get the current price per request.

**Pricing:** $0.00

```json
{
  "method": "GET",
  "url": "https://proxy.obul.ai/proxy/https/minimaxxing.x402endpoints.com/pricing",
  "headers": {
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  }
}
```

**Response:** `{"price":"$0.001","priceUsd":0.001,"model":"MiniMax-M2.5","network":"eip155:8453"}`

## Endpoint Pricing Reference

| Endpoint              | Price   | Purpose                              |
|-----------------------|---------|--------------------------------------|
| `GET /health`         | $0.00   | Health check                         |
| `GET /pricing`        | $0.00   | Current price per request            |
| `POST /v1/messages`   | $0.001  | Anthropic Messages API (MiniMax M2.5)|
| `POST /v1/chat/completions` | $0.001 | OpenAI-compatible chat completions |

## OpenClaw Model Provider Configuration

### Option A: CLI (recommended)

```bash
# Register the MiniMax provider
openclaw config set models.mode merge
openclaw config set models.providers.minimax.baseUrl "https://proxy.obul.ai/proxy/https/minimaxxing.x402endpoints.com"
openclaw config set models.providers.minimax.api anthropic-messages
openclaw config set models.providers.minimax.headers '{"x-obul-api-key":"${OBUL_API_KEY}"}'
openclaw config set models.providers.minimax.models '[{"id":"MiniMax-M2.5","name":"MiniMax M2.5","reasoning":true,"contextWindow":200000,"maxTokens":128000}]'

# Set as the default model
openclaw config set agents.defaults.model.primary minimax/MiniMax-M2.5
```

### Option B: Manual JSON

Add the following to `~/.openclaw/openclaw.json`:

```json
{
  "models": {
    "mode": "merge",
    "providers": {
      "minimax": {
        "baseUrl": "https://proxy.obul.ai/proxy/https/minimaxxing.x402endpoints.com",
        "api": "anthropic-messages",
        "headers": { "x-obul-api-key": "${OBUL_API_KEY}" },
        "models": [
          {
            "id": "MiniMax-M2.5",
            "name": "MiniMax M2.5",
            "reasoning": true,
            "contextWindow": 200000,
            "maxTokens": 128000
          }
        ]
      }
    }
  },
  "agents": {
    "defaults": {
      "model": {
        "primary": "minimax/MiniMax-M2.5"
      }
    }
  }
}
```

**Key points:**
- `baseUrl` does NOT include `/v1` — OpenClaw strips any trailing `/v1` and constructs the full path as `baseUrl + /v1/messages`
- Use `headers` (not `apiKey`) to pass `x-obul-api-key` — this is the correct Obul auth header
- `"api": "anthropic-messages"` tells OpenClaw to use Anthropic Messages format
- No `/anthropic` prefix — the endpoint exposes Anthropic API directly at `/v1/messages`

Set `OBUL_API_KEY` in the systemd service drop-in:

```ini
# /home/ubuntu/.config/systemd/user/openclaw-gateway.service.d/obul.conf
[Service]
Environment=OBUL_API_KEY=obul_live_xxxx
```

Reload after changes:

```bash
systemctl --user daemon-reload && systemctl --user restart openclaw-gateway
```

## When to Use

- **Long-context tasks** — 200k token context handles large documents, codebases, or lengthy conversations.
- **Reasoning tasks** — Active reasoning/thinking mode for complex multi-step analysis.
- **Pay-per-request** — $0.001 per request via Obul, no subscription required.
- **Primary OpenClaw model** — Drop-in Anthropic-compatible model via Obul proxy.

## Best Practices

- **Do not include `/v1` in `baseUrl`** — OpenClaw strips trailing `/v1` and constructs the endpoint as `baseUrl + /v1/messages` automatically.
- **Do not add `/anthropic`** — Unlike `api.minimax.io`, this endpoint has no `/anthropic` prefix.
- **Reasoning blocks** — Responses may include `thinking` content blocks. Parse the `text` block for the
  final answer.
- **Never expose `OBUL_API_KEY`** — Store as an environment variable; never hardcode in requests or logs.

## Error Handling

| Error                       | Cause                                        | Solution                                                                              |
|-----------------------------|----------------------------------------------|---------------------------------------------------------------------------------------|
| `402 Payment Required`      | x402 payment required (direct call, no Obul) | Route through Obul proxy with `x-obul-api-key`.                                      |
| `403 Forbidden`             | Invalid or missing Obul API key              | Verify `OBUL_API_KEY` is set correctly in the `headers` field.                        |
| `404 Not Found`             | Wrong path                                   | Use `/v1/messages` — no `/anthropic` prefix.                                          |
| `429 Too Many Requests`     | Rate limit exceeded                          | Add a short delay between requests.                                                   |
| `502 Bad Gateway`           | Upstream MiniMax API error                   | Retry. If persistent, check `/health`.                                                |
