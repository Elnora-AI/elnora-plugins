# Elnora Plugins — AI-Powered Bioprotocol Generation

The [Elnora AI Platform](https://elnora.ai) for generating, optimizing, and managing bioprotocols for wet-lab experiments. This marketplace provides a Claude Code plugin with 9 skills + an MCP server declaration.

## Installation (Claude Code)

There are three paths, depending on your setup. Path 1 is recommended.

### Path 1: CLI + Plugin (recommended)

One command installs everything — CLI, plugin (skills + MCP), authentication:

```bash
curl -fsSL https://cli.elnora.ai/install.sh | bash    # macOS/Linux
irm https://cli.elnora.ai/install.ps1 | iex           # Windows
npm install -g @elnora-ai/cli                          # npm
brew install elnora-ai/cli/elnora                      # Homebrew
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

Plugin provides: 9 skills + MCP declaration. Say "Use Elnora to list projects" — Claude invokes MCP, triggers OAuth on first use, then cached. No CLI needed.

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

- **OAuth 2.1** (recommended) — A browser popup opens automatically on first connection. No manual configuration required.
- **API Key** — Pass a Bearer token in the Authorization header. Generate keys from your Elnora account settings.

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
| `elnora-folders` | Create, rename, move, and delete project folders |
| `elnora-search` | Search across files, tasks, and all resources in the platform |
| `elnora-admin` | Manage API keys, audit logs, and administrative operations |

## Links

- **Website**: [elnora.ai](https://elnora.ai)
- **Agent Skills Standard**: [agentskills.io](https://agentskills.io)
- **MCP Endpoint**: [mcp.elnora.ai/mcp](https://mcp.elnora.ai/mcp)
- **Issues**: [GitHub Issues](https://github.com/Elnora-AI/elnora-plugins/issues)

## License

[Apache-2.0](LICENSE)
