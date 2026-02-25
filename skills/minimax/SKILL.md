---
name: minimax
description: Generate text with MiniMax M2.5 — a 200K-context reasoning model with tool calling — via OpenAI-compatible pay-per-use API through the Obul proxy.
homepage: https://www.minimax.io
metadata:
  obul-skill:
    emoji: "🧠"
    requires:
      env: [ "OBUL_API_KEY" ]
      primaryEnv: "OBUL_API_KEY"
registries: {}
---

# MiniMax M2.5

MiniMax M2.5 is a 229B-parameter mixture-of-experts reasoning model with a ~200K token context window. Through the Obul
proxy, you get pay-per-use access to OpenAI-compatible chat completions at a flat $0.001 per request — no MiniMax account
or API key required. The model includes built-in chain-of-thought reasoning, streaming, and function/tool calling.

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

Base URL: `https://proxy.obul.ai/proxy/https/minimaxxing.x402endpoints.com`

To get an Obul API key, sign up at **https://my.obul.ai**.

## Common Operations

### Chat Completion

Send a message and get a response. The model always reasons internally before answering — expect `<think>...</think>`
tags in the output containing the model's chain-of-thought.

**Pricing:** $0.001

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/minimaxxing.x402endpoints.com/v1/chat/completions",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "model": "MiniMax-M2.5",
    "messages": [
      { "role": "system", "content": "You are a helpful assistant." },
      { "role": "user", "content": "Explain how x402 payment protocol works in two sentences." }
    ],
    "temperature": 1.0,
    "max_tokens": 1024
  }
}
```

**Response:** JSON object with `choices[0].message.content` containing the model's answer. The content may include
`<think>...</think>` tags with reasoning before the final answer.

### Streaming Chat

Stream the response token-by-token via Server-Sent Events. Useful for long responses where you want incremental output.

**Pricing:** $0.001

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/minimaxxing.x402endpoints.com/v1/chat/completions",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "model": "MiniMax-M2.5",
    "messages": [
      { "role": "user", "content": "Write a short poem about decentralized payments." }
    ],
    "stream": true,
    "stream_options": { "include_usage": true }
  }
}
```

**Response:** Server-Sent Events stream of `chat.completion.chunk` objects. Each chunk contains a `delta` with partial
content. The final chunk includes `usage` statistics when `include_usage` is set.

### Function / Tool Calling

Define tools the model can call. The model reasons about which tool to use, returns a `tool_calls` array, and you send
the result back for a final answer. Supports parallel tool calls.

**Pricing:** $0.001

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/minimaxxing.x402endpoints.com/v1/chat/completions",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "model": "MiniMax-M2.5",
    "messages": [
      { "role": "user", "content": "What's the weather in San Francisco?" }
    ],
    "tools": [
      {
        "type": "function",
        "function": {
          "name": "get_weather",
          "description": "Get current weather for a location",
          "parameters": {
            "type": "object",
            "properties": {
              "location": { "type": "string", "description": "City name" }
            },
            "required": ["location"]
          }
        }
      }
    ],
    "tool_choice": "auto"
  }
}
```

**Response:** JSON object with `choices[0].message.tool_calls` containing an array of tool call requests. Each has an
`id`, `function.name`, and `function.arguments` (JSON string). Send the tool result back as a `tool` role message with
the matching `tool_call_id` to get the final answer.

### Reasoning with Split Output

For complex problems, use `reasoning_split` to separate the model's chain-of-thought from its final answer into distinct
fields. Without this, `<think>` tags appear inline in the content.

**Pricing:** $0.001

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/minimaxxing.x402endpoints.com/v1/chat/completions",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "model": "MiniMax-M2.5",
    "messages": [
      { "role": "user", "content": "Prove that the square root of 2 is irrational." }
    ],
    "reasoning_split": true,
    "max_tokens": 4096
  }
}
```

**Response:** JSON object with `choices[0].message.content` containing only the final answer, and
`choices[0].message.reasoning_details` containing an array of thinking steps. This makes it easy to display or discard
the reasoning separately.

### Long Context Analysis

Analyze large documents by passing them as context. The ~200K token input window handles entire codebases, legal
documents, or research papers. Pricing is flat per request regardless of input size.

**Pricing:** $0.001

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/minimaxxing.x402endpoints.com/v1/chat/completions",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "model": "MiniMax-M2.5",
    "messages": [
      { "role": "system", "content": "You are a document analyst. Answer questions about the provided text." },
      { "role": "user", "content": "<paste large document here>\n\nSummarize the key findings and list any action items." }
    ],
    "max_tokens": 4096
  }
}
```

**Response:** JSON object with the model's analysis in `choices[0].message.content`. The flat $0.001 pricing makes
large-context requests extremely cost-effective compared to per-token pricing.

## Endpoint Pricing Reference

| Endpoint                     | Price  | Purpose                                         |
|------------------------------|--------|--------------------------------------------------|
| `POST /v1/chat/completions`  | $0.001 | Chat completion with reasoning, streaming, tools |

## When to Use

- **Text generation** — Generate text, answer questions, write code, or summarize content with a state-of-the-art
  reasoning model.
- **Complex reasoning** — Math proofs, code debugging, multi-step analysis — the model reasons through problems with
  built-in chain-of-thought.
- **Tool-augmented agents** — Give agents function calling to interact with external APIs, databases, or services.
- **Long document analysis** — Analyze entire codebases, contracts, or research papers within the ~200K context window
  at flat per-request cost.
- **Cost-effective AI backbone** — At $0.001/request flat rate, use as the reasoning engine for autonomous agents
  without worrying about token-based cost spikes.

## Best Practices

- **Use streaming for long responses** — Set `stream: true` to get incremental output instead of waiting for the full
  response, especially for reasoning-heavy queries.
- **Leverage flat pricing** — The $0.001/request rate is independent of token count. Long-context requests that would
  cost $0.03–$0.05 at per-token rates cost the same $0.001 here.
- **Handle reasoning tags** — The model always produces `<think>...</think>` reasoning. Strip these tags for user-facing
  output, or use `reasoning_split: true` to get them in a separate field.
- **Set max_tokens** — Control output length explicitly. The model can generate up to 128K output tokens, so set a
  reasonable limit for your use case.
- **Use system messages** — Set persona, instructions, and constraints via the `system` role message at the start of the
  conversation.

## OpenClaw Integration

OpenClaw can use MiniMax M2.5 as its LLM provider via the Obul proxy. This gives OpenClaw agents access to a 200K-context
reasoning model at $0.001/request.

### Quick Setup

```bash
# 1. Set your Obul API key
export OBUL_API_KEY="your_obul_api_key_here"

# 2. Add the provider
openclaw config set models.mode merge
openclaw config set models.providers.minimax-x402.baseUrl "https://proxy.obul.ai/proxy/https/minimaxxing.x402endpoints.com/v1"
openclaw config set models.providers.minimax-x402.apiKey '${OBUL_API_KEY}'
openclaw config set models.providers.minimax-x402.api openai-completions
openclaw config set models.providers.minimax-x402.authHeader false
openclaw config set models.providers.minimax-x402.headers.X-Obul-Api-Key '${OBUL_API_KEY}'

# 3. Register the model
openclaw config set models.providers.minimax-x402.models '[{"id":"MiniMax-M2.5","name":"MiniMax M2.5 (x402 via Obul)","contextWindow":200000,"maxTokens":128000}]' --strict-json

# 4. Set as default (optional)
openclaw config set agents.defaults.model.primary "minimax-x402/MiniMax-M2.5"

# 5. Restart the gateway
openclaw gateway restart
```

### Manual Configuration

Alternatively, add this block to `~/.openclaw/openclaw.json` under `models.providers`:

```json
"minimax-x402": {
  "baseUrl": "https://proxy.obul.ai/proxy/https/minimaxxing.x402endpoints.com/v1",
  "apiKey": "${OBUL_API_KEY}",
  "api": "openai-completions",
  "authHeader": false,
  "headers": {
    "X-Obul-Api-Key": "${OBUL_API_KEY}"
  },
  "models": [
    {
      "id": "MiniMax-M2.5",
      "name": "MiniMax M2.5 (x402 via Obul)",
      "contextWindow": 200000,
      "maxTokens": 128000
    }
  ]
}
```

Set `authHeader: false` because the Obul proxy authenticates via the `X-Obul-Api-Key` header, not a standard
`Authorization: Bearer` header. Set `mode: "merge"` to keep any existing providers alongside MiniMax.

### Verify

```bash
openclaw config get models.providers.minimax-x402
```

## Error Handling

| Error                       | Cause                                    | Solution                                                                                   |
|-----------------------------|------------------------------------------|--------------------------------------------------------------------------------------------|
| `402 Payment Required`      | Payment not processed or insufficient    | Verify your OBUL_API_KEY is valid and your account has sufficient balance at my.obul.ai.   |
| `400 Bad Request`           | Missing or invalid request body          | Ensure `model` and `messages` fields are present and correctly formatted.                  |
| `429 Too Many Requests`     | Rate limit exceeded                      | Add a short delay between requests and avoid rapid-fire calls.                             |
| `500 Internal Server Error` | Upstream MiniMax service issue           | Wait a few seconds and retry. If persistent, the service may be experiencing downtime.     |
| `503 Service Unavailable`   | MiniMax service temporarily down         | Retry after a brief wait.                                                                  |
