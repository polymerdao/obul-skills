---
description: Create a cloud browser session and return the CDP WebSocket URL for interactive use ($0.05)
---

# Create Browser Session

Create a Browserbase cloud browser session via the Obul proxy and return the CDP (Chrome DevTools Protocol) WebSocket URL. Use this URL to connect with Playwright for interactive browsing.

**Cost:** $0.05 to create, $0.001 to terminate when done

## Create the session

```sh
curl -s -X POST "https://proxy.obul.ai/proxy/https/x402.browserbase.com/browser/session/create" \
  -H "Content-Type: application/json" \
  -H "x-obul-api-key: $OBUL_API_KEY"
```

Extract `connectUrl` and `id` from the JSON response. Present them to the user.

## How to use the CDP URL

Once you have the `connectUrl`, connect to it with Playwright to control the browser. All standard Playwright actions work — navigation, clicking, typing, screenshots, etc.

**Connect to the session:**

```javascript
const { chromium } = require('playwright');
const browser = await chromium.connectOverCDP('wss://...');  // the connectUrl
const context = browser.contexts()[0];
const page = context.pages()[0] || await context.newPage();
```

**Navigate and interact:**

```javascript
await page.goto('https://example.com');
await page.click('button#submit');
await page.fill('input[name="search"]', 'query');
await page.screenshot({ path: 'screenshot.png' });
const content = await page.content();  // full HTML
const text = await page.evaluate(() => document.body.innerText);  // text only
```

**Close when done:**

```javascript
await browser.close();
```

Then terminate the session to stop billing:

```sh
curl -s -X DELETE "https://proxy.obul.ai/proxy/https/x402.browserbase.com/browser/session/SESSION_ID" \
  -H "Content-Type: application/json" \
  -H "x-obul-api-key: $OBUL_API_KEY"
```

Tell the user the session ID so they can terminate it later if needed. Remind them that sessions expire automatically but terminating early saves cost.
