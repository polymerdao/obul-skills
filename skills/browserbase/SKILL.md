---
name: browserbase
description: Run pay-per-use headless browser sessions for web scraping, automation, and AI agent workflows through the Obul proxy.
homepage: https://www.browserbase.com
metadata:
  obul-skill:
    emoji: "🌐"
    requires:
      env: [ "OBUL_API_KEY" ]
      primaryEnv: "OBUL_API_KEY"
registries: {}
---

# Browserbase

Browserbase provides serverless, programmable Chromium browsers in the cloud. Through the Obul proxy, you can create and
manage browser sessions with pay-per-use pricing — no Browserbase account or API key required. Sessions return a
WebSocket URL for full browser control via Chrome DevTools Protocol (CDP), compatible with Playwright, Puppeteer, and
Selenium.

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

Base URL: `https://proxy.obul.ai/proxy/https/x402.browserbase.com`

## Common Operations

### Create a Browser Session

Spin up a new headless Chromium browser session. Returns a WebSocket `connectUrl` for CDP-based automation.

**Pricing:** $0.05

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/x402.browserbase.com/browser/session/create",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  }
}
```

**Response:** JSON object with session `id`, `status` ("RUNNING"), `connectUrl` (WebSocket URL for CDP connection),
`region`, `expiresAt`, and other session metadata. Use the `connectUrl` with Playwright's
`chromium.connectOverCDP(connectUrl)` or Puppeteer's `puppeteer.connect({ browserWSEndpoint: connectUrl })`.

### Get Session Status

Check whether a browser session is still running, completed, or errored.

**Pricing:** $0.0001

```json
{
  "method": "GET",
  "url": "https://proxy.obul.ai/proxy/https/x402.browserbase.com/browser/session/abc123-session-id/status",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  }
}
```

**Response:** JSON object with session `id` and `status` field — one of `RUNNING`, `COMPLETED`, `ERROR`, or `TIMED_OUT`.

### Extend a Session

Add more time to an active browser session before it expires.

**Pricing:** $0.01

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/x402.browserbase.com/browser/session/abc123-session-id/extend",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  }
}
```

**Response:** JSON object confirming the session extension with updated `expiresAt` timestamp.

### Terminate a Session

End a browser session immediately and release resources.

**Pricing:** $0.001

```json
{
  "method": "DELETE",
  "url": "https://proxy.obul.ai/proxy/https/x402.browserbase.com/browser/session/abc123-session-id",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  }
}
```

**Response:** Confirmation that the session has been terminated.

## Endpoint Pricing Reference

| Endpoint                           | Price   | Purpose                                   |
|------------------------------------|---------|-------------------------------------------|
| `POST /browser/session/create`     | $0.05   | Create a new headless browser session     |
| `GET /browser/session/:id/status`  | $0.0001 | Poll session status                       |
| `POST /browser/session/:id/extend` | $0.01   | Extend an active session's duration       |
| `DELETE /browser/session/:id`      | $0.001  | Terminate a session and release resources |

## When to Use

- **Web scraping** — Extract data from JavaScript-rendered pages that require a real browser to load content.
- **Form automation** — Fill and submit web forms, handle multi-step workflows, and interact with dynamic UI elements.
- **Screenshot and PDF capture** — Generate screenshots or PDFs of live web pages for reports or monitoring.
- **AI agent browsing** — Give autonomous agents the ability to navigate websites, click links, and gather information.
- **Testing and verification** — Validate that web pages render correctly or that flows work end-to-end.
- **Low-volume or exploratory use** — When you need occasional browser automation without committing to a Browserbase
  subscription.

## Best Practices

- **Terminate sessions when done** — Always send a DELETE request when you're finished to avoid paying for idle time.
- **Use status polling sparingly** — Check session status only when needed. Avoid tight polling loops; wait a few
  seconds between checks.
- **Extend before expiry** — If your task runs long, extend the session before it times out rather than creating a new
  one and losing state.
- **Choose the right region** — Sessions run in `us-west-2` by default. If you're scraping region-locked content,
  consider this when planning requests.
- **Connect via CDP** — Use the `connectUrl` with Playwright (`chromium.connectOverCDP`) or Puppeteer
  (`puppeteer.connect`) for full browser control.

## Error Handling

| Error                       | Cause                                 | Solution                                                                                      |
|-----------------------------|---------------------------------------|-----------------------------------------------------------------------------------------------|
| `402 Payment Required`      | Payment not processed or insufficient | Verify your OBUL_API_KEY is valid and your account has sufficient balance.                    |
| `404 Not Found`             | Invalid session ID                    | Confirm the session ID is correct and the session has not already been terminated or expired. |
| `408 Request Timeout`       | Session creation timed out            | Retry the request. If persistent, try again after a brief wait.                               |
| `409 Conflict`              | Session already terminated or expired | Create a new session instead of extending or querying a finished one.                         |
| `429 Too Many Requests`     | Rate limit exceeded                   | Add a short delay between requests and avoid rapid-fire session creation.                     |
| `500 Internal Server Error` | Upstream Browserbase service issue    | Wait a few seconds and retry. If persistent, the service may be experiencing downtime.        |
| `503 Service Unavailable`   | Browserbase service temporarily down  | Retry after a brief wait.                                                                     |
