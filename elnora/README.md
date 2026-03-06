# Elnora Plugin

Connect AI agents to the [Elnora AI Platform](https://elnora.ai) for bioprotocol generation, task management, and file workflows.

## What is Elnora?

Elnora is an AI-powered platform that helps researchers generate, optimize, and manage bioprotocols for wet-lab experiments. This plugin bundles an MCP server connection with Agent Skills that teach your AI agent how to use Elnora effectively.

## MCP Server

**Endpoint**: `https://mcp.elnora.ai/mcp`

### Authentication

- **OAuth 2.1** (recommended): Browser popup on first connection, automatic token refresh
- **API Key**: Create at [platform.elnora.ai](https://platform.elnora.ai) > Settings > API Keys, pass as Bearer token

## Available MCP Tools

### Tasks & Messages
| Tool | Description |
|------|-------------|
| `elnora_create_task` | Create a new conversation thread with Elnora AI |
| `elnora_list_tasks` | List tasks with optional project/status filter |
| `elnora_get_task` | Get full task detail including messages |
| `elnora_get_task_messages` | Get message history for a task |
| `elnora_send_message` | Send a message and receive AI response (30-120s for complex requests) |
| `elnora_update_task` | Update task title or status |
| `elnora_archive_task` | Archive a task |
| `elnora_generate_protocol` | One-shot protocol generation |

### Files
| Tool | Description |
|------|-------------|
| `elnora_list_files` | List files in a project |
| `elnora_get_file` | Get file metadata |
| `elnora_get_file_content` | Get file content |
| `elnora_upload_file` | Upload a file to a project |
| `elnora_download_file` | Download file content |
| `elnora_create_file` | Create a new file |
| `elnora_update_file` | Update file name or folder |
| `elnora_archive_file` | Archive a file |
| `elnora_get_file_versions` | Get version history |
| `elnora_create_version` | Create a new version |
| `elnora_restore_version` | Restore a previous version |
| `elnora_fork_file` | Copy a file to another project |
| `elnora_promote_file` | Change file visibility |
| `elnora_create_working_copy` | Create an editable copy |
| `elnora_commit_working_copy` | Commit changes back |

### Projects
| Tool | Description |
|------|-------------|
| `elnora_list_projects` | List all projects |
| `elnora_get_project` | Get project details with members |
| `elnora_create_project` | Create a new project |
| `elnora_update_project` | Update project name/description |
| `elnora_archive_project` | Archive a project |
| `elnora_add_project_member` | Add a member to a project |
| `elnora_remove_project_member` | Remove a member |
| `elnora_update_project_member_role` | Change a member's role |

### Search
| Tool | Description |
|------|-------------|
| `elnora_search_tasks` | Search tasks by keyword |
| `elnora_search_files` | Search files by keyword |
| `elnora_search_all` | Search both tasks and files |

### Organizations & Library
| Tool | Description |
|------|-------------|
| `elnora_list_orgs` | List organizations |
| `elnora_get_org` | Get organization details |
| `elnora_create_org` | Create an organization |
| `elnora_invite_org_member` | Invite a member by email |
| `elnora_get_org_billing` | Get billing information |
| `elnora_list_library_files` | Browse shared library |

### Admin
| Tool | Description |
|------|-------------|
| `elnora_health_check` | Check platform availability |
| `elnora_create_api_key` | Create a new API key |
| `elnora_list_audit_log` | View audit trail |
| `elnora_submit_feedback` | Submit feedback to Elnora team |

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
