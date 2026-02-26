---
name: obul-x-search
description: "USE THIS SKILL WHEN: the user wants to search X/Twitter, look up a Twitter user profile, read a specific tweet, browse a user's timeline, or check trending topics. Provides pay-per-use X/Twitter data via TweetX402 through the Obul proxy."
homepage: https://tweetx402.com
metadata:
  obul-skill:
    emoji: "🔍"
    requires:
      env: [ "OBUL_API_KEY" ]
      primaryEnv: "OBUL_API_KEY"
registries: {}
---

# TweetX402

TweetX402's x402 API provides pay-per-use access to X/Twitter data — search tweets, look up user profiles, read
individual tweets, browse user timelines, and check trending topics. No X/Twitter API key or developer account required —
each request is paid individually through the Obul proxy.

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

Base URL: `https://proxy.obul.ai/proxy/https/x402.tweetx402.com`

## Common Operations

### Search Tweets

Search X/Twitter for tweets matching a query. Supports search operators like `from:user`, `#hashtag`, and `@mention`.
Use the `mode` parameter to switch between Top, Latest, Photos, or Videos results.

**Pricing:** $0.001

```json
{
  "method": "GET",
  "url": "https://proxy.obul.ai/proxy/https/x402.tweetx402.com/api/search?q=bitcoin&mode=Latest",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  }
}
```

**Response:** Array of tweet objects matching the query, each with tweet text, author info, engagement metrics (likes,
retweets, replies), and timestamps. May include a `next_cursor` field for paginating through additional results.

### Get User Profile

Fetch a public X/Twitter profile by username, including bio, follower and following counts, and avatar.

**Pricing:** $0.001

```json
{
  "method": "GET",
  "url": "https://proxy.obul.ai/proxy/https/x402.tweetx402.com/api/profile/elonmusk",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  }
}
```

**Response:** JSON object with the user's display name, username, bio, follower count, following count, tweet count, and
profile image URL.

### Read Tweet

Retrieve a single tweet by its ID, including full text, author info, and engagement metrics.

**Pricing:** $0.001

```json
{
  "method": "GET",
  "url": "https://proxy.obul.ai/proxy/https/x402.tweetx402.com/api/tweet/1234567890",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  }
}
```

**Response:** JSON object with the tweet's full text, author details, engagement metrics (likes, retweets, replies,
views), timestamp, and any media attachments.

### Get User Timeline

Fetch recent tweets from a specific user's timeline by username.

**Pricing:** $0.001

```json
{
  "method": "GET",
  "url": "https://proxy.obul.ai/proxy/https/x402.tweetx402.com/api/user/vaborisov/tweets?limit=20",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  }
}
```

**Response:** Array of the user's recent tweets, each with tweet text, engagement metrics, and timestamps. Default limit
is 20 tweets.

### Get Trending Topics

Retrieve currently trending topics on X/Twitter.

**Pricing:** $0.001

```json
{
  "method": "GET",
  "url": "https://proxy.obul.ai/proxy/https/x402.tweetx402.com/api/trends",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  }
}
```

**Response:** Array of trending topics with topic name, tweet volume, and category information.

## Endpoint Pricing Reference

| Endpoint                       | Price  | Purpose                                  |
|--------------------------------|--------|------------------------------------------|
| `GET /api/search`              | $0.001 | Search tweets by keyword or operator     |
| `GET /api/profile/:username`   | $0.001 | Public user profile lookup               |
| `GET /api/tweet/:id`           | $0.001 | Read a single tweet by ID                |
| `GET /api/user/:username/tweets` | $0.001 | Browse a user's recent tweets          |
| `GET /api/trends`              | $0.001 | Current trending topics                  |

## When to Use

- **Real-time tweet search** — Find tweets about a topic, token, event, or person without an X/Twitter API key.
- **User research** — Look up public profiles to check follower counts, bios, and recent activity.
- **Individual tweet lookup** — Read a specific tweet's full text and engagement metrics by ID.
- **Timeline monitoring** — Track what specific accounts are posting by browsing their recent tweets.
- **Trending analysis** — See what's trending on X/Twitter for market sentiment or news monitoring.
- **AI agent social monitoring** — Give autonomous agents on-demand access to X/Twitter data at a predictable per-call
  cost.
- **Low-volume or exploratory use** — When you need occasional X/Twitter data without committing to an API subscription.

## Best Practices

- **Use search operators** — Combine operators like `from:username`, `#hashtag`, `@mention`, `min_retweets:100`, and
  quoted phrases for precise results.
- **Set the right mode** — Use `mode=Latest` for real-time results, `mode=Top` for popular tweets, or `mode=Photos` /
  `mode=Videos` to filter by media type.
- **Paginate with cursor** — When search results include a `next_cursor` field, pass it as a query parameter to fetch
  the next page of results.
- **Use limit on timelines** — The `/api/user/:username/tweets` endpoint accepts a `limit` parameter (default 20) to
  control how many tweets are returned.
- **Cache profile data** — User profiles don't change frequently. Cache profile lookups to reduce costs when freshness
  isn't critical.

## Error Handling

| Error                       | Cause                                       | Solution                                                                                  |
|-----------------------------|---------------------------------------------|-------------------------------------------------------------------------------------------|
| `402 Payment Required`      | Payment not processed or insufficient       | Verify your OBUL_API_KEY is valid and your account has sufficient balance.                |
| `400 Bad Request`           | Missing or invalid query parameters         | Ensure required parameters like `q` for search are provided and properly formatted.       |
| `404 Not Found`             | Invalid username or tweet ID                | Double-check the username exists or the tweet ID is correct and the tweet is still public. |
| `429 Too Many Requests`     | Rate limit exceeded                         | Add a short delay between requests and avoid unnecessary rapid-fire calls.                |
| `500 Internal Server Error` | Upstream TweetX402 service issue            | Wait a few seconds and retry. If persistent, check TweetX402's status.                    |
