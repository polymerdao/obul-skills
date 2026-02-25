---
name: skill-creator
description: >
  Create a new Obul skill definition (SKILL.md) for any x402 pay-per-use API
  accessible through the Obul proxy. Invoke this whenever adding a new service
  to the obul-skills repo, wrapping a new x402 endpoint, or asked to write/create/add
  an Obul skill for any service — even if the user just says "add [service]" or
  "write a skill for [service]".
homepage: https://proxy.obul.ai
metadata:
  obul-skill:
    emoji: "🛠️"
    requires:
      env: [ ]
      primaryEnv: ""
registries: { }
---

# Obul Skill Creator

An Obul skill is a Markdown file that wraps an x402 pay-per-use API so agents can call it through
`proxy.obul.ai` using only an `OBUL_API_KEY`. This meta-skill guides you through researching a
service and producing a correctly-formatted `SKILL.md`.

> **Note:** This skill is an intentional exception in the obul-skills repo. It has no Authentication,
> Pricing, or Error Handling sections because it is a workflow guide, not an API wrapper.

## Output Contract

Produce a single file at: `skills/<service-name>/SKILL.md`

### Required frontmatter fields

| Field                                     | Value                                                          |
|-------------------------------------------|----------------------------------------------------------------|
| `name`                                    | kebab-case, no "x402" (e.g., `browserbase`)                    |
| `description`                             | One sentence: what the service does + "through the Obul proxy" |
| `homepage`                                | Upstream service URL                                           |
| `metadata.obul-skill.emoji`               | Relevant emoji in quotes                                       |
| `metadata.obul-skill.requires.env`        | `[ "OBUL_API_KEY" ]`                                           |
| `metadata.obul-skill.requires.primaryEnv` | `"OBUL_API_KEY"`                                               |
| `registries`                              | `{}` (empty until published)                                   |

### Required body sections

1. **H1 title** — Service name
2. **Overview paragraph** — What the service does, pay-per-use through Obul, no account needed
3. **Authentication** — Obul proxy pattern (see template below)
4. **Common Operations** — 3–5 operations, each with H3 header, description, pricing, JSON example, response description
5. **Endpoint Pricing Reference** — Table of all endpoints with price and purpose
6. **When to Use** — Bullet list of use cases
7. **Best Practices** — Service-specific tips
8. **Error Handling** — Table of error codes, causes, and solutions

## Workflow

1. **Research** — Web-search the service's x402 API: find the upstream host, all available endpoints,
   HTTP methods, request/response shapes, and real published pricing. Do not invent endpoints or prices.

2. **Name** — Choose a lowercase kebab-case folder/skill name matching the service (e.g., `firecrawl`,
   `browserbase`). No "x402" in the name.

3. **Create** — Make `skills/<name>/SKILL.md`. Use `skills/firecrawl/SKILL.md` as the primary
   structural reference (multiple REST ops, async pattern) or `skills/browserbase/SKILL.md` for
   session-lifecycle patterns.

4. **Fill in** — Complete all 8 required body sections with real data from step 1.

5. **Validate** — Run through the Review Checklist before committing.

## SKILL.md Template

```markdown
---
name: <service-name>
description: <One-sentence description of what the service does through the Obul proxy.>
homepage: https://<upstream-service-domain>
metadata:
  obul-skill:
    emoji: "<emoji>"
    requires:
      env: [ "OBUL_API_KEY" ]
      primaryEnv: "OBUL_API_KEY"
registries: {}
---

# <Service Name>

<Service> provides <what it does>. Through the Obul proxy, each request is paid individually —
no <Service> account or API key required.

## Authentication

All requests route through the Obul proxy. Include your Obul API key in every request:

\`\`\`json
{
"headers": {
"Content-Type": "application/json",
"x-obul-api-key": "{{OBUL_API_KEY}}"
}
}
\`\`\`

Base URL: `https://proxy.obul.ai/proxy/https/<upstream-host>`

To get an Obul API key, sign up at **https://my.obul.ai**.

## Common Operations

### <Operation 1>

<Description of what this operation does.>

**Pricing:** $X.XX

\`\`\`json
{
"method": "POST",
"url": "https://proxy.obul.ai/proxy/https/<upstream-host>/<endpoint>",
"headers": {
"Content-Type": "application/json",
"x-obul-api-key": "{{OBUL_API_KEY}}"
},
"body": {
"<param>": "<value>"
}
}
\`\`\`

**Response:** <Description of what the response contains.>

### <Operation 2>

<Description.>

**Pricing:** $X.XX

\`\`\`json
{
"method": "GET",
"url": "https://proxy.obul.ai/proxy/https/<upstream-host>/<endpoint>",
"headers": {
"Content-Type": "application/json",
"x-obul-api-key": "{{OBUL_API_KEY}}"
}
}
\`\`\`

**Response:** <Description.>

### <Operation 3>

<Description.>

**Pricing:** $X.XX

\`\`\`json
{
"method": "POST",
"url": "https://proxy.obul.ai/proxy/https/<upstream-host>/<endpoint>",
"headers": {
"Content-Type": "application/json",
"x-obul-api-key": "{{OBUL_API_KEY}}"
},
"body": {
"<param>": "<value>"
}
}
\`\`\`

**Response:** <Description.>

## Endpoint Pricing Reference

| Endpoint              | Price  | Purpose              |
|-----------------------|--------|----------------------|
| `POST /<endpoint-1>`  | $X.XX  | <Purpose>            |
| `GET /<endpoint-2>`   | $X.XX  | <Purpose>            |
| `POST /<endpoint-3>`  | $X.XX  | <Purpose>            |

## When to Use

- **<Use case 1>** — <When and why to use this service for this purpose.>
- **<Use case 2>** — <When and why.>
- **<Use case 3>** — <When and why.>

## Best Practices

- **<Tip 1>** — <Explanation.>
- **<Tip 2>** — <Explanation.>
- **<Tip 3>** — <Explanation.>

## Error Handling

| Error                       | Cause                                    | Solution                                                                                 |
|-----------------------------|------------------------------------------|------------------------------------------------------------------------------------------|
| `402 Payment Required`      | Payment not processed or insufficient    | Verify your OBUL_API_KEY is valid and your account has sufficient balance at my.obul.ai. |
| `400 Bad Request`           | Missing or invalid request body          | Check that all required fields are present and correctly typed.                          |
| `429 Too Many Requests`     | Rate limit exceeded                      | Add a short delay between requests and avoid rapid-fire calls.                           |
| `500 Internal Server Error` | Upstream service issue                   | Wait a few seconds and retry. If persistent, the service may be experiencing downtime.   |
| `503 Service Unavailable`   | Service temporarily down                 | Retry after a brief wait.                                                                |
```

## Conventions

| Rule                 | Detail                                                                     |
|----------------------|----------------------------------------------------------------------------|
| **Pricing format**   | Always dollar amounts (e.g., `$0.0001`, `$0.001`, `$0.01`). Never credits. |
| **Request URLs**     | `https://proxy.obul.ai/proxy/https/<upstream-host>/path`                   |
| **Auth headers**     | Always include both `Content-Type: application/json` and `x-obul-api-key`  |
| **Published slug**   | `obul-` + skill name (e.g., name `firecrawl` → slug `obul-firecrawl`)      |
| **registries field** | Leave `{}` until published; add `registry-name: published-slug` after      |

### Pricing tiers

| Price          | Tier             |
|----------------|------------------|
| `$0.00`        | Health checks    |
| `$0.0001`      | Status polling   |
| `$0.001–$0.01` | Simple queries   |
| `$0.01–$0.05`  | Complex queries  |
| `$0.05+`       | Heavy operations |

See `CLAUDE.md` for the full conventions reference and `registry.md` for publishing steps.

## Review Checklist

Before committing, verify:

- [ ] YAML frontmatter parses (no tabs, valid YAML arrays)
- [ ] `name` is kebab-case, no "x402"
- [ ] All pricing in dollar format (e.g., `$0.0001`, `$0.001`, `$0.01`)
- [ ] 3–5 Common Operations with real pricing and realistic JSON examples
- [ ] Endpoint Pricing Reference table is complete
- [ ] All 8 body sections (H1, overview, authentication, common ops, pricing table, when to use, best practices, error
  handling) are present
- [ ] `registries: {}` (empty until published)
- [ ] All request examples use full Obul proxy URLs (`https://proxy.obul.ai/proxy/https/...`)

## Registry Publishing

After the skill is complete and merged, see `registry.md` for publishing steps. Once live, add each
`registry-name: published-slug` entry to the frontmatter `registries` field.
