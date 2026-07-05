# RGate MCP Server

[![Glama MCP server card](https://glama.ai/mcp/servers/marcoldenburg/rgate-mcp/badges/card.svg)](https://glama.ai/mcp/servers/marcoldenburg/rgate-mcp)

RGate is the release gate agents call before they tell users a public website is ready to ship.

This repository documents the public RGate MCP server for agentic IDEs, coding agents, and MCP clients. The RGate product source code is not published here; this repo is the public integration and directory listing home for the operated MCP endpoint.

## Server

- MCP endpoint: `https://www.rgate.io/api/mcp`
- Transport: Streamable HTTP
- Manifest: `https://www.rgate.io/.well-known/mcp.json`
- OpenAPI: `https://www.rgate.io/openapi.json`
- Agent guide: `https://www.rgate.io/agents.md`
- Developer docs: `https://www.rgate.io/en/developers`
- Agent recipes: `https://www.rgate.io/en/agent-recipes`

## What RGate Does

RGate scans public websites across five release-readiness layers:

- Security
- SEO
- Accessibility
- Legal compliance
- Sustainability

Agents can start a scan, poll structured results, inspect the check catalog, generate fix plans, export Markdown, compare scan history, and hand paid report access back to a human.

## MCP Client Configuration

```json
{
  "mcpServers": {
    "rgate": {
      "url": "https://www.rgate.io/api/mcp",
      "transport": "streamable-http"
    }
  }
}
```

## Tools

| Tool | Purpose |
| --- | --- |
| `start_scan` | Start an RGate website quality scan for a public URL. |
| `get_scan` | Fetch scan status and, once complete, structured findings and results. |
| `list_checks` | Describe RGate scan layers and the check catalog. |
| `get_pricing` | Return machine-readable plan names, prices, limits, and use cases. |
| `create_checkout_handoff` | Create a human handoff URL for purchasing report access or a subscription. |
| `generate_fix_plan` | Generate a prioritized release-readiness fix plan from findings. |
| `export_scan_markdown` | Export scan results as Markdown for Cursor, GitHub issues, or summaries. |
| `compare_scan_history` | Compare two scans and return added, resolved, improved, and worsened findings. |

## Example Agent Flow

1. Confirm the user is allowed to scan the public website.
2. Call `start_scan` with the public URL.
3. Poll `get_scan` with the returned `scanId` until status is `done` or `failed`.
4. If the target passed green, report that the website passed the release gate.
5. If access is locked, send the human to the report URL for checkout and poll the same `scanId` again after payment.
6. Generate a fix plan or Markdown export for the coding agent working on that project.

## Related Integrations

- CLI: `npm install -g @rgate_io/rgate`
- Skill: `npx skills add marcoldenburg/rgate-release-readiness-skill`
- Skill repository: `https://github.com/marcoldenburg/rgate-release-readiness-skill`

## Directory Metadata

- Glama: [`glama.json`](./glama.json) at repo root (maintainer claim for [listing](https://glama.ai/mcp/servers/marcoldenburg/rgate-mcp))
- Name: `rgate`
- Title: `RGate`
- Description: `Release gate agents call before they tell users a public website is ready to ship. Scans public websites across security, SEO, accessibility, legal compliance, and sustainability.`
- Tags: `developer-tools`, `release-readiness`, `mcp`, `agent-tools`, `website-audit`, `security`, `seo`, `accessibility`, `compliance`, `sustainability`

## Running as a local stdio server (build checks / directory harnesses)

RGate is a **remote** MCP server hosted at `https://www.rgate.io/api/mcp`
(transport: Streamable HTTP). Some runners — including Glama's automated
build/quality check — start a local stdio process and speak MCP to it. This
repo doubles as a thin stdio↔remote bridge for exactly that:

```bash
npm install
npm start          # = mcp-remote https://www.rgate.io/api/mcp
```

### Glama admin configuration

- **Build steps:** `["npm install"]`
- **Start command:** `npm start` (or `mcp-remote https://www.rgate.io/api/mcp`)
- Node.js ≥ 18; Python/base-image defaults are fine.

For direct clients (Cursor, Claude, VS Code) just use the remote URL from
`mcp.json` — no bridge needed.
