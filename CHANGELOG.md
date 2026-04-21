# Changelog

## [1.2.3](https://github.com/Elnora-AI/elnora-plugins/compare/elnora-plugins-v1.2.2...elnora-plugins-v1.2.3) (2026-04-21)


### Bug Fixes

* **commands:** refresh `/elnora:protocol` slash command to use the renamed MCP tool names ([#19](https://github.com/Elnora-AI/elnora-plugins/issues/19))
* **skills:** use underscore tool names in agent skill warning ([46a0558](https://github.com/Elnora-AI/elnora-plugins/commit/46a0558573ae028ad9a6a7bfbdc688ff13a41f67))

## [1.2.2](https://github.com/Elnora-AI/elnora-plugins/compare/elnora-plugins-v1.2.1...elnora-plugins-v1.2.2) (2026-04-09)


### Bug Fixes

* sync skills with TypeScript CLI rewrite ([7147093](https://github.com/Elnora-AI/elnora-plugins/commit/71470934659cbb447108ef3248965a96d253e2a2))
* sync skills with TypeScript CLI rewrite and update tool counts ([ab12dfe](https://github.com/Elnora-AI/elnora-plugins/commit/ab12dfecca834fbf98d8a2f805d2cfc858f8de2e))

## [1.2.1](https://github.com/Elnora-AI/elnora-plugins/compare/elnora-plugins-v1.2.0...elnora-plugins-v1.2.1) (2026-03-23)


### Bug Fixes

* pin release-please-action to commit SHA ([0ccfcac](https://github.com/Elnora-AI/elnora-plugins/commit/0ccfcacc9e761ee727a0a83fda39eb511a23ff5d))
* pin release-please-action to commit SHA for supply chain security ([783bb24](https://github.com/Elnora-AI/elnora-plugins/commit/783bb2490b3e405c5a0c2e50757786557a8fac3e))

## [1.2.0](https://github.com/Elnora-AI/elnora-plugins/compare/elnora-plugins-v1.1.0...elnora-plugins-v1.2.0) (2026-03-14)


### Features

* **skills:** add agent capabilities skill, file content search, and presigned uploads ([4433ee8](https://github.com/Elnora-AI/elnora-plugins/commit/4433ee88e4d0e257d1fd2e08d52daaf0d5c3dcc0))
* **skills:** add agent capabilities, file content search, and presigned uploads ([3157a0d](https://github.com/Elnora-AI/elnora-plugins/commit/3157a0d3d61aab27a8d595e14707b1040e846fa7))

## [1.1.0](https://github.com/Elnora-AI/elnora-plugins/compare/elnora-plugins-v1.0.0...elnora-plugins-v1.1.0) (2026-03-06)


### Features

* initial Elnora plugins marketplace ([0a2be9a](https://github.com/Elnora-AI/elnora-plugins/commit/0a2be9a16b864ee171f4bcd99f963728710bb6ab))

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
