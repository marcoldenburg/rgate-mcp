# Example Agent Workflow

Use RGate after an agent has changed a public website, landing page, documentation site, or web app and before recommending release.

## Flow

1. Read the RGate agent guide at `https://www.rgate.io/agents.md`.
2. Connect to the MCP server at `https://www.rgate.io/api/mcp`.
3. Call `start_scan` with the public URL.
4. Poll `get_scan` with the returned `scanId`.
5. Review the `access` object in the scan response:
   - `full`: use findings and fix plans directly.
   - `teaser`: send the human to the report URL for unlock.
   - `none`: wait, retry, or explain that the scan failed.
6. After checkout, poll the same `scanId` again. No one-time token exchange is required.
7. Use `generate_fix_plan` or `export_scan_markdown` to hand implementation work to the coding agent.

## Example Prompt

```text
Run RGate against the public preview URL. If the scan passes green, tell me the site passed the release gate. If RGate returns locked access, send me the report URL and wait until I unlock it before creating implementation tasks.
```
