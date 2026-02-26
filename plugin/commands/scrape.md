---
description: Scrape a URL and return clean markdown content via Firecrawl ($0.001/request)
---

# Scrape URL

Scrape the given URL using Firecrawl through the Obul proxy and return the page content as clean markdown.

**Cost:** $0.001 per request

Run this curl command to scrape `$ARGUMENTS`:

```sh
curl -s -X POST "https://proxy.obul.ai/proxy/https/firecrawl.x402endpoints.com/v1/scrape" \
  -H "Content-Type: application/json" \
  -H "x-obul-api-key: $OBUL_API_KEY" \
  -d '{"url": "'"$ARGUMENTS"'", "formats": ["markdown"]}'
```

Parse the JSON response and present the scraped markdown content to the user. If the response contains an error, explain what went wrong (invalid URL, site blocked, etc.).
