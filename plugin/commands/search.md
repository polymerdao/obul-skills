---
description: Search the web and return scraped results via Firecrawl ($0.002/request)
---

# Web Search

Search the web for the given query using Firecrawl through the Obul proxy. Returns search results with scraped page content.

**Cost:** $0.002 per request

Run this curl command to search for `$ARGUMENTS`:

```sh
curl -s -X POST "https://proxy.obul.ai/proxy/https/firecrawl.x402endpoints.com/v1/search" \
  -H "Content-Type: application/json" \
  -H "x-obul-api-key: $OBUL_API_KEY" \
  -d '{"query": "'"$ARGUMENTS"'"}'
```

Parse the JSON response and present the search results to the user. Each result includes a URL, title, and scraped markdown content. Summarize the key findings.
