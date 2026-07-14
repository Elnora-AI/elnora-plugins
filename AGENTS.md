# AGENTS.md

Universal guide for any coding agent working with `elnora-plugins`. Read natively by Codex, Cursor, Aider, Continue, Amp, Jules, and Roo. Claude Code reads `CLAUDE.md` instead — but note this repo's `CLAUDE.md` is the marketplace-maintainer guide, not a copy of this file (see [Claude Code](#claude-code) below).

## What this is

`elnora-plugins` is the Elnora AI plugins marketplace: one Claude Code plugin (`elnora`) bundling **9 Agent Skills** plus an **MCP server declaration** for the Elnora AI Platform at `https://mcp.elnora.ai/mcp`. It ships client-side configuration only — no CLI in this repo, no server code, no credentials. Agents drive the platform by invoking the Elnora MCP tools; the skills tell you which tool serves which intent.

Every skill routes to MCP tools named `elnora_*` (e.g. `elnora_health_check`, `elnora_projects_list`, `elnora_tasks_create`). Under a namespaced MCP client the same tools surface as `elnora__elnora_*` (or `mcp__elnora__elnora_*` in Claude Code).

## Setup

Every platform needs two things: (1) the MCP server connection and (2) the skill files copied into your tool's skills directory.

MCP config — use wherever your platform expects MCP server definitions:

```json
{ "mcpServers": { "elnora": { "type": "http", "url": "https://mcp.elnora.ai/mcp" } } }
```

Skills:

```sh
cp -r elnora/skills/* <your-tool-skills-dir>/   # e.g. .cursor/skills, .codex/skills, .vscode/skills, .gemini/skills
```

Authentication happens on the first MCP call — OAuth 2.1 (browser popup, recommended) or an API key generated at `platform.elnora.ai`. For a guided, gated walkthrough (verify → auth → smoke test), point your agent at [`INSTALL_FOR_AGENTS.md`](INSTALL_FOR_AGENTS.md).

## Dispatch — which skill for what

| User intent | Skill | Representative MCP tools |
|---|---|---|
| Generate / iterate a protocol; manage tasks + messages | `elnora-tasks` | `elnora_tasks_create`, `elnora_tasks_send`, `elnora_protocols_generate` |
| Projects — list, create, members | `elnora-projects` | `elnora_projects_list` |
| Files — list, read, upload, version, fork/promote, working copies | `elnora-files` | `elnora_files_list`, `elnora_files_content`, `elnora_files_upload`, `elnora_files_version` |
| Folders / knowledge-base tree | `elnora-folders` | `elnora_folders_list`, `elnora_folders_create`, `elnora_folders_move` |
| Search tasks / files / everything by keyword | `elnora-search` | `elnora_files_search` |
| Organizations, members, invitations, billing, shared library | `elnora-orgs` | `elnora_orgs_get`, `elnora_orgs_invite`, `elnora_library_files` |
| Auth, API keys, health, account, audit log, feature flags, agreements | `elnora-admin` | `elnora_health_check`, `elnora_account_get`, `elnora_audit_list`, `elnora_flags_list` |
| Scientific tools + literature (PubMed / ArXiv / web) via the cloud agent | `elnora-agent` | `elnora_ask_agent` |
| General "how do I use Elnora" / routing | `elnora-platform` | routes to the domain skill above |

A read-only `elnora_health_check` proves the MCP connection; `elnora_account_get` or `elnora_projects_list` proves auth works.

## Pitfalls

- **MCP-only.** This repo references no CLI commands inside skills — every skill maps to an `elnora_*` MCP tool. Keep it that way when editing skills.
- **Skill dir must match `name`.** A skill's directory name must equal the `name:` in its `SKILL.md` frontmatter — CI (`Validate Skills`) fails otherwise.
- **Two "elnora" servers.** If you register the API-key MCP manually (`claude mcp add elnora …`) AND enable the plugin, you get two servers both named `elnora`. Use one, not both.
- **OAuth is per-client.** Tokens are stored and refreshed by your MCP client, never by files in this repo. There is nothing to persist here.

## Safety

Client-side config + skills only; no secrets in-repo. `.env` and `*.local` files are gitignored, and secret scanning (`gitleaks`) plus CodeQL run on every PR. Agent Skills execute in your tool's context — only install marketplaces you trust and review skill files before running them in a privileged environment. Report vulnerabilities privately per [`.github/SECURITY.md`](.github/SECURITY.md) (security@elnora.ai) — do not open a public issue.

## Claude Code

The plugin provides a native slash command, the 9 skills, and the MCP declaration. Install as **two separate slash commands** (paste the first, wait for it to finish, then paste the second):

```
/plugin marketplace add https://github.com/Elnora-AI/elnora-plugins.git
```

```
/plugin install elnora@elnora-plugins
```

Surface: `/elnora:protocol <description>` generates a bioprotocol; the 9 skills auto-load and route by intent. Definitions live in [`elnora/commands/`](elnora/commands/) and [`elnora/skills/`](elnora/skills/).

This repo already ships a `CLAUDE.md`, but it is the **marketplace-maintainer** guide (structure, skill spec, how to add a plugin) — a different audience from this file. Do **not** `ln -s AGENTS.md CLAUDE.md` here; the two are intentionally distinct.

## Per-harness install

- **Codex CLI** — `AGENTS.md` is auto-loaded at repo root. `codex mcp add elnora -- https://mcp.elnora.ai/mcp`, then `cp -r elnora/skills/* .codex/skills/`.
- **Cursor** — reads `AGENTS.md` natively. MCP in `.cursor/mcp.json`; `cp -r elnora/skills/* .cursor/skills/`.
- **VS Code Copilot** — MCP in `.vscode/mcp.json`; `cp -r elnora/skills/* .vscode/skills/`.
- **Gemini CLI** — MCP in your Gemini settings; `cp -r elnora/skills/* .gemini/skills/`.
- **Continue / Amp / Jules / Roo** — read `AGENTS.md` at repo root automatically; add the MCP config per that tool's convention.

## Contributing to this repo

Skill spec (agentskills.io): `name` is lowercase letters/digits/hyphens, ≤64 chars; `description` ≤1024 chars; body under 500 lines; the directory name matches the frontmatter `name`. Register new plugins in `.claude-plugin/marketplace.json`.

CI on every PR: `Validate Skills` (manifest + frontmatter checks), CodeQL (`actions`), and `gitleaks` secret scan. PR titles must be Conventional Commits — `feat`, `fix`, `chore`, `docs`, `ci`, `refactor`, `perf`, `test`, `revert`, `build`.
