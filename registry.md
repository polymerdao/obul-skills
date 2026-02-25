# Skill Registries

Where to publish Obul skills so agents and developers can discover them.

Skills fall into two categories for registry purposes:

- **Own x402 endpoints** (hosted at `*.x402endpoints.com`) — publish to both x402 endpoint registries and agent skill registries
- **Third-party x402 endpoints** (e.g., CoinGecko, Browserbase) — publish to agent skill registries only

## Quick Reference

| Registry              | Category        | Submission         | URL                                           |
| --------------------- | --------------- | ------------------ | --------------------------------------------- |
| x402-bazaar           | x402 endpoint   | Config flag        | https://x402.org                              |
| x402-org              | x402 endpoint   | Submission form    | https://x402.org/ecosystem                    |
| nullpath              | x402 endpoint   | API                | https://nullpath.io                           |
| awesome-x402          | x402 endpoint   | Pull request       | https://github.com/anthropics/awesome-x402    |
| clawhub               | Agent skill     | Dashboard upload   | https://clawhub.com                           |
| skillregistry-io      | Agent skill     | Registration       | https://skillregistry.io                      |
| tessl                 | Agent skill     | CLI                | https://tessl.io                              |
| skills-sh             | Agent skill     | CLI                | https://skills.sh                             |
| anthropic-skills      | Agent skill     | Pull request       | https://github.com/anthropics/skills          |

---

## x402 Endpoint Registries

For skills hosted at `*.x402endpoints.com` only. These registries discover and index x402-compatible endpoints — you must own the endpoint to register it.

### x402-bazaar

Native x402 discovery layer operated by Coinbase. Agents querying the x402 network find services registered here automatically.

- **URL:** https://x402.org
- **How to publish:**
  1. Set `discoverable: true` in your CDP facilitator configuration
  2. Ensure your x402 endpoint is live and responding to payment headers
  3. The bazaar indexes discoverable endpoints automatically

### x402-org

Official x402.org ecosystem page listing all x402-compatible services and integrations.

- **URL:** https://x402.org/ecosystem
- **How to publish:**
  1. Submit via the Coinbase/x402 Foundation submission form
  2. Include service name, description, x402 endpoint URL, and pricing
  3. Listing appears after foundation review

### nullpath

x402-native agent marketplace. Agents browse and pay for services directly. Charges $0.001/request + 15% platform fee.

- **URL:** https://nullpath.io
- **How to publish:**
  1. Register a developer account at nullpath.io
  2. Submit your x402 endpoint via the API
  3. Set pricing and description
  4. Skill goes live after approval

### awesome-x402

Curated GitHub lists of x402-compatible services and tools.

- **URL:** https://github.com/anthropics/awesome-x402
- **How to publish:**
  1. Fork the repository
  2. Add your service to the appropriate section
  3. Submit a pull request

---

## Agent Skill Registries

For all skills regardless of endpoint ownership. These registries index agent skill definitions and documentation.

### clawhub

OpenClaw skill store with 5,700+ skills. The largest general-purpose skill marketplace.

- **URL:** https://clawhub.com
- **How to publish:**
  1. Package the skill as a `.tar.gz` archive
  2. Log in to the ClawHub dashboard
  3. Upload via the "Publish Skill" form
  4. Fill in metadata (name, description, pricing, tags)
  5. Skill is listed after automated validation

### skillregistry-io

Registry that indexes skills defined in `SKILLS.md` files.

- **URL:** https://skillregistry.io
- **How to publish:**
  1. Ensure your repo has a valid `SKILLS.md` file
  2. Register your repository URL on skillregistry.io

### tessl

Quality-scored skill marketplace. Skills are scored on documentation, reliability, and usage.

- **URL:** https://tessl.io
- **How to publish:**
  1. Install the CLI: `npm install -g @tessl/cli`
  2. Run `tessl skill publish` from the skill directory
  3. The skill is scored and listed after review

### skills-sh

Cross-platform skill registry. Skills are installable via `npx skills add`.

- **URL:** https://skills.sh
- **How to publish:**
  1. Format your skill according to skills.sh conventions
  2. Run `npx skills add` to register
  3. Users can then install with `npx skills add <your-skill>`

### anthropic-skills

Official Anthropic reference skills repository.

- **URL:** https://github.com/anthropics/skills
- **How to publish:**
  1. Fork the repository
  2. Add your skill following the contribution guidelines
  3. Submit a pull request for review

---

## Publishing Checklist

Step-by-step playbook for publishing a skill.

1. **Verify the skill** — Ensure `SKILL.md` is complete, endpoints are live, and pricing is accurate
2. **Determine endpoint ownership** — Check if the skill uses `*.x402endpoints.com` (own) or a third-party domain
3. **Agent skill registries** (all skills):
   - [ ] `clawhub` — Upload `.tar.gz` via dashboard
   - [ ] `skillregistry-io` — Register repo
   - [ ] `tessl` — Run `tessl skill publish`
   - [ ] `skills-sh` — Run `npx skills add`
   - [ ] `anthropic-skills` — Submit PR
4. **x402 endpoint registries** (own endpoints at `*.x402endpoints.com` only):
   - [ ] `x402-bazaar` — Set `discoverable: true` in CDP config
   - [ ] `x402-org` — Submit to x402 Foundation
   - [ ] `nullpath` — Register via API
   - [ ] `awesome-x402` — Submit PR
5. **Update frontmatter** — Add each `registry-name: published-slug` to the skill's `registries` dict in `SKILL.md`
6. **Verify** — Confirm each listing is live and the slug in frontmatter resolves correctly
