---
name: elnora-platform
description: >
  Use when the user asks about Elnora platform, Elnora AI, bioprotocol generation,
  platform API, elnora projects, elnora tasks, elnora files, protocol generation,
  platform search, elnora orgs, elnora folders, elnora admin, API keys,
  elnora health, elnora auth, or any task involving the Elnora AI Platform.
  Routes to domain-specific sub-skills for token-efficient guidance.
---

# Elnora Platform

Route Elnora Platform queries to the correct sub-skill. Load only what is needed.

## What is Elnora?

Elnora is an AI-powered platform that helps researchers generate, optimize, and manage bioprotocols for wet-lab experiments. This plugin connects your agent to the Elnora Platform via MCP.

## Authentication

Auth is handled automatically by the MCP connection:

- **OAuth 2.1** (recommended): Browser popup on first use, tokens refresh automatically
- **API Key**: Set as Bearer token in MCP headers (create at platform.elnora.ai > Settings > API Keys)

No manual login required.

## Routing Table

| Need | Sub-skill | Trigger keywords |
|------|-----------|------------------|
| List/get/create/update/archive projects, manage members | `elnora-projects` | project, workspace, create project, members |
| Create/manage/message tasks, protocol generation | `elnora-tasks` | task, protocol, send message, generate |
| Browse/read/upload/create/version/fork files | `elnora-files` | file, content, version history, upload, download, fork |
| Find tasks or files by keyword | `elnora-search` | search, find, query |
| Manage project folder trees | `elnora-folders` | folder, create folder, move folder |
| Org management, members, billing, invitations, shared library | `elnora-orgs` | organization, org, billing, invite, library |
| Auth, API keys, account, health, diagnostics | `elnora-admin` | api key, health, account, feedback, audit, flags |

## ID Format

All IDs are UUIDs (e.g., `bfdc6fbd-40ed-4042-9ea7-c79a5ec90085`).

## Pagination

List endpoints return paginated results:

```json
{"items": [...], "page": 1, "pageSize": 25, "totalCount": N, "totalPages": N, "hasNextPage": true}
```

Use `page` and `pageSize` parameters (max 100). Check `hasNextPage` to paginate.

Message endpoints use cursor-based pagination: check `hasMore` and pass `cursor` from `nextCursor`.

## Common Workflow

Projects contain tasks and files. Typical flow:

1. `elnora_list_projects` — get project ID
2. `elnora_create_task` — start a conversation with Elnora AI
3. `elnora_send_message` — describe the protocol you need
4. `elnora_get_task_messages` — read the AI response
5. `elnora_list_files` — browse generated outputs
6. `elnora_get_file_content` — read a protocol file

## Quick Protocol Generation

For one-shot protocol generation, use `elnora_generate_protocol`:

- `description`: What protocol you need (e.g., "HEK 293 cell maintenance protocol")
- Returns the complete generated protocol

## All Available MCP Tools

### Tasks & Messages
`elnora_create_task`, `elnora_list_tasks`, `elnora_get_task`, `elnora_get_task_messages`, `elnora_send_message`, `elnora_update_task`, `elnora_archive_task`

### Files
`elnora_list_files`, `elnora_get_file`, `elnora_get_file_content`, `elnora_upload_file`, `elnora_get_file_versions`, `elnora_restore_version`, `elnora_create_version`, `elnora_create_file`, `elnora_update_file`, `elnora_archive_file`, `elnora_fork_file`, `elnora_promote_file`, `elnora_create_working_copy`, `elnora_commit_working_copy`, `elnora_download_file`

### Projects
`elnora_list_projects`, `elnora_get_project`, `elnora_create_project`, `elnora_update_project`, `elnora_archive_project`, `elnora_list_project_members`, `elnora_add_project_member`, `elnora_remove_project_member`, `elnora_update_project_member_role`, `elnora_leave_project`

### Search
`elnora_search_tasks`, `elnora_search_files`, `elnora_search_all`

### Organizations & Library
`elnora_list_orgs`, `elnora_get_org`, `elnora_create_org`, `elnora_update_org`, `elnora_list_org_members`, `elnora_update_org_member_role`, `elnora_remove_org_member`, `elnora_invite_org_member`, `elnora_list_org_invitations`, `elnora_cancel_org_invitation`, `elnora_get_org_billing`, `elnora_list_library_files`, `elnora_list_library_folders`, `elnora_create_library_folder`, `elnora_rename_library_folder`, `elnora_delete_library_folder`

### Folders
`elnora_list_folders`, `elnora_create_folder`, `elnora_rename_folder`, `elnora_move_folder`, `elnora_delete_folder`

### Admin & Diagnostics
`elnora_health_check`, `elnora_get_account`, `elnora_update_account`, `elnora_list_api_keys`, `elnora_create_api_key`, `elnora_revoke_api_key`, `elnora_list_agreements`, `elnora_accept_terms`, `elnora_submit_feedback`, `elnora_list_audit_log`, `elnora_list_flags`, `elnora_get_flag`, `elnora_accept_invitation`, `elnora_get_invitation_info`
