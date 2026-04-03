---
name: elnora-admin
description: >
  Manages Elnora platform administration: creates and revokes API keys,
  queries audit logs, checks feature flags, accepts terms-of-service
  agreements, inspects and accepts org invitations, submits feedback, and
  monitors platform health. Handles account retrieval and updates. Applies
  when the user mentions "Elnora account", "API keys", "audit log",
  "feature flags", "health check", "terms of service", "platform status",
  "feedback", or "invitation".
---

# Elnora Admin

Platform administration tools for account management, API keys, audit logs,
feature flags, agreements, invitations, and system health.

## Auth

The Elnora MCP server handles authentication automatically via OAuth 2.1
(browser popup) or API key bearer tokens in the MCP connection headers.
There is no manual login/logout step — auth "just works" when the MCP
server is configured.

## Core Concepts

- **Health check** requires no authentication — use it to verify the platform is up.
- **API keys** are shown in full **only once** at creation time. Store them securely immediately.
- **Audit logs** track actions across the org. Filterable by org, action type, and user.
- **Feature flags** are read-only. Use them to check what capabilities are enabled.
- **Agreements** (terms of service) must be accepted before using certain features.
- **Invitations** can be inspected and accepted to join an org.

## Tools Reference

### System Health

| Tool | Purpose | Key Parameters |
|------|---------|----------------|
| `elnora_health_check` | Check platform status | — (no auth required) |

### Account

| Tool | Purpose | Key Parameters |
|------|---------|----------------|
| `elnora_get_account` | Get current user's account info | — |
| `elnora_update_account` | Update account details | fields to update |

### API Keys

| Tool | Purpose | Key Parameters |
|------|---------|----------------|
| `elnora_list_api_keys` | List existing API keys | — |
| `elnora_create_api_key` | Create a new API key | `name` |
| `elnora_revoke_api_key` | Revoke an API key | `key_id` |

**Critical**: `elnora_create_api_key` returns the full key **only once**. The key is never
shown again. Always present it to the user immediately and remind them to store it securely.

### Agreements

| Tool | Purpose | Key Parameters |
|------|---------|----------------|
| `elnora_list_agreements` | List terms/agreements | — |
| `elnora_accept_terms` | Accept terms of service | `agreement_id` |

### Feedback

| Tool | Purpose | Key Parameters |
|------|---------|----------------|
| `elnora_submit_feedback` | Submit platform feedback | `message`, optional `type` |

### Audit Log

| Tool | Purpose | Key Parameters |
|------|---------|----------------|
| `elnora_list_audit_log` | Query audit events | `org_id`, optional filters |

Filters may include action type, user, and date range. Always scope to a specific
`org_id` to get relevant results.

### Feature Flags

| Tool | Purpose | Key Parameters |
|------|---------|----------------|
| `elnora_list_flags` | List all feature flags | — |
| `elnora_get_flag` | Get a specific flag's value | `flag_name` |

Feature flags are **read-only**. Use them to check whether a feature is enabled
before attempting to use it.

### Invitations

| Tool | Purpose | Key Parameters |
|------|---------|----------------|
| `elnora_get_invitation_info` | Inspect an invitation | `invitation_id` |
| `elnora_accept_invitation` | Accept an org invitation | `invitation_id` |

## Common Workflows

### Create an API Key

1. Run `elnora_health_check` to confirm the platform is reachable.
2. Call `elnora_create_api_key` with a descriptive `name`.
3. If creation fails, verify the user has org-level key creation permission and retry once.
4. On success, **immediately** present the full key to the user — it is never shown again.
5. Remind the user to store the key in a secrets manager.

### Accept an Org Invitation

1. Call `elnora_get_invitation_info` with the `invitation_id` to retrieve details.
2. Present the org name, role, and expiry to the user for confirmation.
3. Only after the user confirms, call `elnora_accept_invitation`.
4. If the invitation is expired (404), ask the user to request a new one from the org admin.

### Query Audit Logs

1. Determine the target `org_id` (use `elnora_get_account` if the user does not know it).
2. Call `elnora_list_audit_log` with `org_id` and any filters (action type, user, date range).
3. If the result set is large, apply narrower filters or paginate by date range.

## Error Handling

- **Auth failure (401/403)**: The MCP server's OAuth token or API key may have expired. Ask the user to restart the MCP server or check their API key. Verify the key has not been revoked with `elnora_list_api_keys`.
- **API key creation fails**: Confirm org-level permission, retry once, then run `elnora_health_check` to rule out a platform outage.
- **Resource not found (404)**: Verify the ID is a valid UUID and belongs to the correct org. For invitations, the link may have expired.
- **Duplicate API key name**: The platform may reject duplicate names within an org. Append a timestamp or suffix and retry.
- **Agreement required (403 on non-auth endpoints)**: Some operations require accepted terms. Call `elnora_list_agreements`, accept any pending agreements with `elnora_accept_terms`, then retry the original operation.

## ID Format

All IDs are UUIDs (e.g., `bfdc6fbd-40ed-4042-9ea7-c79a5ec90085`).
