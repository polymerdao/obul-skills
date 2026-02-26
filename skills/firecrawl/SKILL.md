---
name: obul-firecrawl
description: "USE THIS SKILL WHEN: the user wants to scrape a URL, crawl a website, search the web, map site URLs, or extract structured data from web pages. Provides pay-per-use web scraping via Firecrawl through the Obul proxy."
homepage: https://www.firecrawl.dev
metadata:
  obul-skill:
    emoji: "🔥"
    requires:
      env: [ "OBUL_API_KEY" ]
      primaryEnv: "OBUL_API_KEY"
registries: {}
---

# Firecrawl

Firecrawl provides pay-per-use web scraping, crawling, site mapping, web search, and structured data extraction with
built-in JavaScript rendering, anti-bot handling, and proxy rotation. Through the Obul proxy, each request is paid
individually — no Firecrawl account or API key required.

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

Base URL: `https://proxy.obul.ai/proxy/https/firecrawl.x402endpoints.com`

To get an Obul API key, sign up at **https://my.obul.ai**.

## Common Operations

### Scrape a URL

Scrape a single URL with JavaScript rendering and anti-bot handling. Returns clean markdown by default, with optional
HTML, raw HTML, links, and screenshot formats.

**Pricing:** $0.001

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/firecrawl.x402endpoints.com/v1/scrape",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "url": "https://example.com",
    "formats": ["markdown"]
  }
}
```

**Response:** JSON object with scraped content in the requested formats. The `markdown` format returns clean,
well-structured markdown extracted from the page. Additional formats include `html`, `rawHtml`, `links` (list of all
URLs on the page), and `screenshot` (base64-encoded image).

### Search the Web

Search the web and get clean, scraped content from the results. Useful for finding and reading pages on a topic without
knowing specific URLs.

**Pricing:** $0.002

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/firecrawl.x402endpoints.com/v1/search",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "query": "firecrawl web scraping API"
  }
}
```

**Response:** Array of search results, each containing the page URL, title, and scraped content in clean markdown
format.

### Map a Website

Discover all URLs on a website. Returns a complete sitemap-style list of pages without scraping their content.

**Pricing:** $0.001

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/firecrawl.x402endpoints.com/v1/map",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "url": "https://example.com"
  }
}
```

**Response:** JSON object with an array of all discovered URLs on the site. Use this to plan targeted scraping or
crawling of specific pages.

### Crawl a Website

Start an asynchronous crawl job that scrapes multiple pages on a website. The `limit` parameter is required and
determines both the maximum number of pages crawled and the total cost.

**Pricing:** $0.001 per page (total = `limit` × $0.001, max 1000 pages)

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/firecrawl.x402endpoints.com/v1/crawl",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "url": "https://example.com",
    "limit": 10
  }
}
```

**Response:** JSON object with a crawl job `id` and `status`. The crawl runs asynchronously — use the returned job ID
to poll for results. Each crawled page is returned as clean markdown content.

### Extract Structured Data

Extract structured data from one or more URLs. Firecrawl scrapes the pages and returns data in a structured format,
ideal for pulling specific fields from product pages, articles, or directories.

**Pricing:** $0.001

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/firecrawl.x402endpoints.com/v1/extract",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "urls": ["https://example.com/product/123"]
  }
}
```

**Response:** JSON object with structured data extracted from the provided URLs. The extraction identifies and returns
key fields like titles, prices, descriptions, and other structured content from the pages.

## Endpoint Pricing Reference

| Endpoint       | Price              | Purpose                                  |
|----------------|--------------------|------------------------------------------|
| `POST /v1/scrape`  | $0.001             | Scrape a single URL to clean markdown    |
| `POST /v1/search`  | $0.002             | Web search with scraped content          |
| `POST /v1/map`     | $0.001             | Discover all URLs on a website           |
| `POST /v1/crawl`   | $0.001 per page    | Async crawl a website (limit required)   |
| `POST /v1/extract` | $0.001             | Structured data extraction from URLs     |

## When to Use

- **Web scraping** — Scrape any URL to clean markdown with JavaScript rendering and anti-bot bypass, no browser setup
  needed.
- **Web search** — Search the web and get scraped content from results in a single call, ideal for research tasks.
- **Site discovery** — Map all URLs on a website before deciding which pages to scrape or crawl.
- **Bulk content ingestion** — Crawl entire websites or sections to build knowledge bases, datasets, or RAG indexes.
- **Structured extraction** — Pull specific data fields from product pages, directories, or listings without writing
  custom parsers.
- **AI agent browsing** — Give autonomous agents the ability to read and understand web pages at a predictable
  per-call cost.


## Best Practices

- **Use map before crawl** — Call `/v1/map` first to discover available URLs, then crawl only the pages you need
  instead of blindly crawling an entire site.
- **Set conservative crawl limits** — Start with a small `limit` (e.g., 10) and increase as needed. The cost scales
  linearly with the limit, and the maximum is 1000 pages.
- **Choose the right format** — Default to `markdown` for most use cases. Use `html` when you need structure, `links`
  when you only need URLs, and `screenshot` for visual verification.
- **Prefer scrape over crawl for single pages** — Use `/v1/scrape` for individual URLs. Only use `/v1/crawl` when you
  need multiple pages from the same site.
- **Use search for discovery** — When you don't have a specific URL, use `/v1/search` to find relevant pages and get
  their content in one step.

## Error Handling

| Error                       | Cause                                    | Solution                                                                                   |
|-----------------------------|------------------------------------------|--------------------------------------------------------------------------------------------|
| `402 Payment Required`      | Payment not processed or insufficient    | Verify your OBUL_API_KEY is valid and your account has sufficient balance at my.obul.ai.   |
| `400 Bad Request`           | Missing or invalid request body          | Ensure required fields (`url`, `query`, `urls`, `limit`) are present and correctly typed.  |
| `422 Unprocessable Entity`  | Valid request but URL cannot be scraped   | Verify the URL is accessible and not blocked. Try a different URL or format.               |
| `429 Too Many Requests`     | Rate limit exceeded                      | Add a short delay between requests and avoid rapid-fire calls.                             |
| `500 Internal Server Error` | Upstream Firecrawl service issue         | Wait a few seconds and retry. If persistent, the service may be experiencing downtime.     |
| `503 Service Unavailable`   | Firecrawl service temporarily down       | Retry after a brief wait.                                                                  |
