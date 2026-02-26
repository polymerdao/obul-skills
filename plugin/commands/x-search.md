---
description: Search X/Twitter for tweets matching a query via TweetX402 ($0.001/request)
---

# X/Twitter Search

Search X/Twitter for tweets matching the given query using TweetX402 through the Obul proxy. Supports search operators like `from:user`, `#hashtag`, and `@mention`.

**Cost:** $0.001 per request

URL-encode the query from `$ARGUMENTS`, then run this curl command:

```sh
curl -s -G "https://proxy.obul.ai/proxy/https/x402.tweetx402.com/api/search" \
  --data-urlencode "q=$ARGUMENTS" \
  -d "mode=Latest" \
  -H "Content-Type: application/json" \
  -H "x-obul-api-key: $OBUL_API_KEY"
```

Parse the JSON response and present the tweets to the user. Each tweet includes text, author info, engagement metrics (likes, retweets, replies), and timestamps.
