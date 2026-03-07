# Elnora Plugin

Connect AI agents to the [Elnora AI Platform](https://elnora.ai) for bioprotocol generation, task management, and file workflows.

## What is Elnora?

Elnora is an AI-powered platform that helps researchers generate, optimize, and manage bioprotocols for wet-lab experiments. This plugin bundles an MCP server connection with Agent Skills that teach your AI agent how to use Elnora effectively.

## MCP Server

**Endpoint**: `https://mcp.elnora.ai/mcp`

### Authentication

- **OAuth 2.1** (recommended): Browser popup on first connection, automatic token refresh
- **API Key**: Create at [platform.elnora.ai](https://platform.elnora.ai) > Settings > API Keys, pass as Bearer token

## Capabilities

71 tools across 7 categories. All tools are discoverable through your MCP client's tool listing.

| Category | Tools | What you can do |
|----------|------:|-----------------|
| Tasks & Protocols | 8 | Create conversation threads, send messages, generate bioprotocols |
| Files | 15 | Upload, download, version, fork, and manage protocol files |
| Projects | 10 | Create projects and manage team membership |
| Organizations | 16 | Org settings, billing, member invitations, shared library |
| Folders | 5 | Organize files into nested folder structures |
| Search | 3 | Full-text search across tasks, files, and all resources |
| Admin | 14 | API keys, audit logs, account, health checks, feature flags |

See the individual [skill files](skills/) for full tool reference with parameters and workflow recipes.

## Skills

| Skill | Description |
|-------|-------------|
| `elnora-platform` | Router — directs queries to the right sub-skill |
| `elnora-tasks` | Task management and protocol generation workflows |
| `elnora-files` | File browsing, versioning, upload, and download |
| `elnora-projects` | Project CRUD and member management |
| `elnora-search` | Cross-project search for tasks and files |
| `elnora-orgs` | Organization management, billing, shared library |
| `elnora-folders` | Project folder hierarchy management |
| `elnora-admin` | Health checks, API keys, audit logs, account settings |

## Commands

| Command | Description |
|---------|-------------|
| `/elnora:protocol` | Quick protocol generation — describe what you need |

## Example Workflows

### Generate a protocol
> "Use Elnora to generate a HEK 293 cell maintenance protocol"

### Search and read files
> "Search my Elnora files for PCR protocols, then show me the latest one"

### Manage tasks
> "Show my active Elnora tasks and get the messages from the latest one"

## Support

- [GitHub Issues](https://github.com/Elnora-AI/elnora-plugins/issues)
- [support@elnora.ai](mailto:support@elnora.ai)
- [security@elnora.ai](mailto:security@elnora.ai)
