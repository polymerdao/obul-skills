---
description: Open a URL in a cloud browser, take a screenshot, and return the page content ($0.051)
---

# Browse URL

Create a Browserbase cloud browser session via the Obul proxy, navigate to the given URL, capture a screenshot, extract the page content, and terminate the session.

**Cost:** ~$0.051 ($0.05 create + $0.001 terminate)

## Steps

### 1. Create a browser session

```sh
curl -s -X POST "https://proxy.obul.ai/proxy/https/x402.browserbase.com/browser/session/create" \
  -H "Content-Type: application/json" \
  -H "x-obul-api-key: $OBUL_API_KEY"
```

Extract the `connectUrl` from the JSON response. This is a WebSocket URL for Chrome DevTools Protocol.

### 2. Connect with Playwright, navigate, screenshot, and get content

Use Playwright to connect to the cloud browser via CDP. Run this code:

```javascript
const { chromium } = require('playwright');

const browser = await chromium.connectOverCDP('CONNECT_URL_FROM_STEP_1');
const context = browser.contexts()[0];
const page = context.pages()[0] || await context.newPage();

await page.goto('$ARGUMENTS', { waitUntil: 'networkidle', timeout: 30000 });
await page.screenshot({ path: '/tmp/obul-browse-screenshot.png', fullPage: true });

const title = await page.title();
const text = await page.evaluate(() => document.body.innerText);

console.log(JSON.stringify({ title, text: text.substring(0, 5000) }));
await browser.close();
```

Show the screenshot to the user using the Read tool on `/tmp/obul-browse-screenshot.png`, and summarize the page content.

### 3. Terminate the session

```sh
curl -s -X DELETE "https://proxy.obul.ai/proxy/https/x402.browserbase.com/browser/session/SESSION_ID" \
  -H "Content-Type: application/json" \
  -H "x-obul-api-key: $OBUL_API_KEY"
```

Always terminate the session when done to avoid paying for idle time.
