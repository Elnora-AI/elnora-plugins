# Changelog

## [1.0.0](https://github.com/Elnora-AI/elnora-plugins/releases/tag/v1.0.0) (2026-03-06)

### Features

* Initial release of the Elnora plugins marketplace
* **elnora plugin**: 8 Agent Skills for bioprotocol generation and lab workflow management
  * `elnora-platform` — router skill with progressive disclosure
  * `elnora-tasks` — task management and protocol generation
  * `elnora-files` — file browsing, versioning, upload/download
  * `elnora-projects` — project CRUD and member management
  * `elnora-search` — cross-project full-text search
  * `elnora-orgs` — organization management, billing, shared library
  * `elnora-folders` — project folder hierarchy
  * `elnora-admin` — health checks, API keys, audit logs
* **`/elnora:protocol` command** — one-shot bioprotocol generation
* MCP server integration at `mcp.elnora.ai/mcp` (OAuth 2.1 + API key auth)
* Universal compatibility: Claude Code, Cursor, Codex, VS Code Copilot, Gemini CLI, and more
