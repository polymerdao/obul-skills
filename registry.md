# Skill Registries

Where to publish Obul skills so agents and developers can discover them. Each registry below is a marketplace, directory, or listing where skills can be submitted.

## Quick Reference

| Registry              | Tier | Type              | Submission              | URL                                           |
| --------------------- | ---- | ----------------- | ----------------------- | --------------------------------------------- |
| x402-bazaar           | 1    | x402 discovery    | Config flag             | https://x402.org                              |
| mcp-registry          | 1    | MCP registry      | CLI                     | https://registry.mcphub.io                    |
| smithery              | 1    | MCP marketplace   | CLI + YAML              | https://smithery.ai                           |
| x402-org              | 1    | x402 ecosystem    | Submission form         | https://x402.org/ecosystem                    |
| nullpath              | 1    | x402 marketplace  | API                     | https://nullpath.io                           |
| clawhub               | 1    | Skill store       | Dashboard upload        | https://clawhub.com                           |
| glama                 | 2    | MCP directory     | API / auto-indexed      | https://glama.ai/mcp/servers                  |
| pulsemcp              | 2    | MCP directory     | REST API                | https://pulsemcp.com                          |
| mcp-so                | 2    | MCP directory     | Community               | https://mcp.so                                |
| awesome-mcp-servers   | 2    | GitHub list       | Pull request            | https://github.com/punkpeye/awesome-mcp-servers |
| skillregistry-io      | 2    | SKILLS.md registry| Registration            | https://skillregistry.io                      |
| tessl                 | 2    | Skill marketplace | CLI                     | https://tessl.io                              |
| skills-sh             | 2    | Skill registry    | CLI                     | https://skills.sh                             |
| opentools             | 2    | MCP directory     | Submission              | https://opentools.com                         |
| awesome-x402          | 2    | GitHub list       | Pull request            | https://github.com/anthropics/awesome-x402    |
| anthropic-skills      | 2    | Reference skills  | Pull request            | https://github.com/anthropics/skills          |

---

## Tier 1 — Must Publish

High-impact registries. Publish every skill here first.

### x402-bazaar

Native x402 discovery layer operated by Coinbase. Agents querying the x402 network find services registered here automatically.

- **URL:** https://x402.org
- **How to publish:**
  1. Set `discoverable: true` in your CDP facilitator configuration
  2. Ensure your x402 endpoint is live and responding to payment headers
  3. The bazaar indexes discoverable endpoints automatically

### mcp-registry

The canonical MCP server registry at mcphub.io. The primary place developers search for MCP servers.

- **URL:** https://registry.mcphub.io
- **How to publish:**
  1. Install the publisher CLI: `npm install -g mcp-publisher`
  2. Run `mcp-publisher init` to generate a registry manifest
  3. Run `mcp-publisher login` and authenticate
  4. Run `mcp-publisher publish` to submit

### smithery

The largest hosted MCP marketplace with install-and-run capability. Agents can discover and connect to servers directly.

- **URL:** https://smithery.ai
- **How to publish:**
  1. Create a `smithery.yaml` config in the skill repo
  2. Install the CLI: `npm install -g @smithery/cli`
  3. Run `smithery mcp publish` to submit
  4. The server appears on smithery.ai after review

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

### clawhub

OpenClaw skill store with 5,700+ skills. The largest general-purpose skill marketplace.

- **URL:** https://clawhub.com
- **How to publish:**
  1. Package the skill as a `.tar.gz` archive
  2. Log in to the ClawHub dashboard
  3. Upload via the "Publish Skill" form
  4. Fill in metadata (name, description, pricing, tags)
  5. Skill is listed after automated validation

---

## Tier 2 — Should Publish

Secondary registries that increase discoverability. Publish here after Tier 1 is covered.

### glama

MCP server directory with ~10K servers. Can auto-index from public GitHub repos.

- **URL:** https://glama.ai/mcp/servers
- **How to publish:**
  1. Submit via the Glama API, or
  2. Ensure your GitHub repo is public — Glama auto-indexes MCP servers with standard manifests

### pulsemcp

MCP server directory with 8,600+ servers. REST API for submission.

- **URL:** https://pulsemcp.com
- **How to publish:**
  1. Submit your server details via the PulseMCP REST API
  2. Include name, description, repository URL, and endpoint

### mcp-so

Community-driven MCP server directory with 17,800+ servers.

- **URL:** https://mcp.so
- **How to publish:**
  1. Submit your server through the community submission flow on mcp.so
  2. Include repository URL and description

### awesome-mcp-servers

Curated GitHub list of MCP servers. High visibility in the MCP community.

- **URL:** https://github.com/punkpeye/awesome-mcp-servers
- **How to publish:**
  1. Fork the repository
  2. Add your server to the appropriate category in `README.md`
  3. Submit a pull request with a one-line description

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

### opentools

Task-oriented MCP server directory organized by what agents can do with each server.

- **URL:** https://opentools.com
- **How to publish:**
  1. Submit your server via the OpenTools submission form
  2. Include task descriptions, endpoint details, and pricing

### awesome-x402

Curated GitHub lists of x402-compatible services and tools.

- **URL:** https://github.com/anthropics/awesome-x402
- **How to publish:**
  1. Fork the repository
  2. Add your service to the appropriate section
  3. Submit a pull request

### anthropic-skills

Official Anthropic reference skills repository.

- **URL:** https://github.com/anthropics/skills
- **How to publish:**
  1. Fork the repository
  2. Add your skill following the contribution guidelines
  3. Submit a pull request for review

---

## Publishing Checklist

Step-by-step playbook for publishing a new skill to all registries.

1. **Verify the skill** — Ensure the skill's `SKILL.md` is complete, endpoints are live, and pricing is accurate
2. **Tier 1 first** — Publish to all 6 Tier 1 registries:
   - [ ] `x402-bazaar` — Set `discoverable: true` in CDP config
   - [ ] `mcp-registry` — Run `mcp-publisher publish`
   - [ ] `smithery` — Create `smithery.yaml`, run `smithery mcp publish`
   - [ ] `x402-org` — Submit to x402 Foundation
   - [ ] `nullpath` — Register via API
   - [ ] `clawhub` — Upload `.tar.gz` via dashboard
3. **Update frontmatter** — Add each `registry-name: published-slug` to the skill's `registries` dict in `SKILL.md`
4. **Tier 2 next** — Publish to Tier 2 registries as time allows:
   - [ ] `glama` — Submit via API or ensure auto-indexing
   - [ ] `pulsemcp` — Submit via REST API
   - [ ] `mcp-so` — Community submission
   - [ ] `awesome-mcp-servers` — Submit PR
   - [ ] `skillregistry-io` — Register repo
   - [ ] `tessl` — Run `tessl skill publish`
   - [ ] `skills-sh` — Run `npx skills add`
   - [ ] `opentools` — Submit via form
   - [ ] `awesome-x402` — Submit PR
   - [ ] `anthropic-skills` — Submit PR
5. **Update frontmatter again** — Add Tier 2 slugs to `registries` dict
6. **Verify** — Confirm each listing is live and the slug in frontmatter resolves correctly
