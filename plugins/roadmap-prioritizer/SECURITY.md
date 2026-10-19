# What this plugin runs and connects to

**`roadmap-prioritizer` is instructions only.** It contains Markdown files (`SKILL.md` and reference docs) that tell the AI agent how to run a product-management conversation. Nothing else.

| Question | Answer |
|----------|--------|
| Executable code or scripts? | None. No `.sh`, `.js`, `.py`, or binaries. |
| Hooks? | None. No `hooks/` directory, no lifecycle hooks. |
| MCP servers? | None. No `.mcp.json`. |
| Network access? | None. No URLs, API calls, telemetry, or downloads. |
| Credentials or secrets? | None requested, read, or stored. |
| Tool restrictions? | Skills do not request extra permissions; the agent uses only the tools your session already allows. |
| Files written? | Only Markdown docs in the user's workspace, shown to the user as they are produced. |

You can verify this yourself: `find plugins/roadmap-prioritizer -type f` lists only `.json` manifests and `.md` files.

Source: https://github.com/oferregev81pt/ai-pm-skills
