# Elnora Plugins — AI-Powered Bioprotocol Generation

The [Elnora AI Platform](https://elnora.ai) for generating, optimizing, and managing bioprotocols for wet-lab experiments. This marketplace provides a Claude Code plugin with 10 skills + an MCP server declaration.

## Installation (Claude Code)

There are three paths, depending on your setup. Path 1 is recommended.

### Path 1: CLI + Plugin (recommended)

One command installs everything — CLI, plugin (skills + MCP), authentication. Pick the line for your OS or package manager:

**macOS / Linux**

```bash
curl -fsSL https://cli.elnora.ai/install.sh | bash
```

**Windows**

```powershell
irm https://cli.elnora.ai/install.ps1 | iex
```

**npm (any OS with Node.js 20+)**

```bash
npm install -g @elnora-ai/cli
```

**Homebrew (macOS / Linux)**

```bash
brew install elnora-ai/cli/elnora
```

The installer prompts for your API key, then `elnora setup claude` registers the plugin. First MCP use triggers OAuth (one browser click), then cached.

### Path 2: Plugin only — OAuth in browser

Prefer browser-based OAuth, or running without a CLI? Install the plugin alone. In Claude Code:

```
/plugin
# Choose: Add marketplace → Elnora-AI/elnora-plugins
/plugin
# Choose: Enable → elnora
```

Plugin provides: 10 skills + MCP declaration. Say "Use Elnora to list projects" — Claude invokes MCP, triggers OAuth on first use, then cached. No CLI needed.

### Path 3: Advanced — API key MCP (CI / skip OAuth)

If you prefer API key auth over OAuth (useful for CI or non-interactive environments):

```bash
claude mcp add elnora --transport http --scope user \
  https://mcp.elnora.ai/mcp \
  --header "X-API-Key: <your-key-from-platform.elnora.ai>"
```

Use this instead of the plugin's built-in MCP declaration. If the plugin is also enabled, you'll have two entries named "elnora" — pick one or the other, don't enable both.

## Installation (other platforms)

For Cursor, Codex, VS Code Copilot, Gemini CLI, and any other MCP-compatible client, configure the Elnora MCP server as follows.

### MCP Server Config

Use this config snippet wherever your platform expects MCP server definitions:

```json
{
  "mcpServers": {
    "elnora": {
      "type": "http",
      "url": "https://mcp.elnora.ai/mcp"
    }
  }
}
```

### Cursor

Add the MCP config above to `.cursor/mcp.json`, then copy skills:

```bash
cp -r elnora/skills/* .cursor/skills/
```

### OpenAI Codex

```bash
codex mcp add elnora -- https://mcp.elnora.ai/mcp
cp -r elnora/skills/* .codex/skills/
```

### VS Code Copilot

Add the MCP config above to `.vscode/mcp.json`. Skills are loaded from `.vscode/skills/`:

```bash
cp -r elnora/skills/* .vscode/skills/
```

### Gemini CLI

Add the MCP config to your Gemini settings, then copy skills:

```bash
cp -r elnora/skills/* .gemini/skills/
```

### Generic MCP Client

Point any MCP-compatible client at:

```
https://mcp.elnora.ai/mcp
```

## Authentication

The Elnora MCP server supports two authentication methods:

- **OAuth 2.1** — A browser popup opens automatically on first connection; no manual configuration. Best for interactive clients (Claude Code, Cursor, VS Code).
- **API Key** — Generate a key at [platform.elnora.ai](https://platform.elnora.ai) and send it in a request header. Best for CI, scripts, and non-interactive clients. Use **either**:
  - `X-API-Key: <your-key>` — the standard header, or
  - `Authorization: Bearer <your-key>` — if your client only exposes a single bearer/token field.

## For coding agents

Running Codex, Cursor, Aider, Continue, Amp, Jules, or Roo? Two files are written for agents rather than humans:

- **[`AGENTS.md`](AGENTS.md)** — usage conventions: the MCP endpoint, the `elnora_*` tools, and a dispatch table mapping user intent to the right skill. Auto-loaded at repo root by most agent harnesses.
- **[`INSTALL_FOR_AGENTS.md`](INSTALL_FOR_AGENTS.md)** — a gated, step-by-step setup runbook (verify → authenticate → copy skills → smoke test), with browser-assist offers for the OAuth and API-key steps.

## Available Plugins

| Plugin | Description |
|--------|-------------|
| `elnora` | Connect to the Elnora AI Platform for bioprotocol generation, task management, and file workflows |

## Available Skills

| Skill | Description |
|-------|-------------|
| `elnora-platform` | Route requests across the Elnora platform — account, health checks, feature flags, and agreements |
| `elnora-orgs` | Manage organizations, members, invitations, billing, and the shared org library |
| `elnora-projects` | Create and manage projects, project members, and roles |
| `elnora-tasks` | Create, update, search, and manage tasks and task messages |
| `elnora-files` | Create, upload, download, version, and manage files and working copies |
| `elnora-folders` | Browse, create, rename, move, delete, and share Knowledge Base folders |
| `elnora-review` | Approve or reject Knowledge Base auto-tidy proposals |
| `elnora-search` | Search across files, tasks, and all resources in the platform |
| `elnora-admin` | Manage API keys, audit logs, and administrative operations |
| `elnora-agent` | Run scientific tools and literature lookups (PubMed, ArXiv, web search) via the cloud agent |

## Links

- **Website**: [elnora.ai](https://elnora.ai)
- **Agent Skills Standard**: [agentskills.io](https://agentskills.io)
- **MCP Endpoint**: [mcp.elnora.ai/mcp](https://mcp.elnora.ai/mcp)
- **For agents**: [AGENTS.md](AGENTS.md) · [INSTALL_FOR_AGENTS.md](INSTALL_FOR_AGENTS.md)
- **Issues**: [GitHub Issues](https://github.com/Elnora-AI/elnora-plugins/issues)

## License

[Apache-2.0](LICENSE)
