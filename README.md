# Elnora Plugins — AI-Powered Bioprotocol Generation

The [Elnora AI Platform](https://elnora.ai) for generating, optimizing, and managing bioprotocols for wet-lab experiments. This marketplace provides plugins with MCP server + [Agent Skills](https://agentskills.io) for use across 30+ AI coding tools.

## Quick Start (Claude Code)

```
/plugin marketplace add Elnora-AI/elnora-plugins
/plugin install elnora@elnora-plugins
```

This installs the Elnora MCP server and all skills automatically.

## Setup for Other Platforms

Every platform needs two things: (1) the MCP server connection and (2) the skill files copied into the platform's skills directory.

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
