# CLAUDE.md — Elnora Plugins Marketplace

This is the Elnora AI plugins marketplace for the [Agent Skills](https://agentskills.io) standard.

## Structure

```
.claude-plugin/marketplace.json   # Marketplace registry — lists all plugins
elnora/                           # The "elnora" plugin
  .claude-plugin/plugin.json      # Plugin metadata + MCP server config
  skills/                         # Agent Skills (one directory per skill)
    <skill-name>/SKILL.md
  commands/                       # Slash commands (if any)
  hooks/                          # Lifecycle hooks (if any)
```

## MCP Server

All skills reference MCP tools from `https://mcp.elnora.ai/mcp`. Never reference CLI commands in skills — tools must be platform-agnostic MCP calls.

## Skill Spec (agentskills.io)

- Name: lowercase letters, digits, and hyphens only. Max 64 characters.
- Description (SKILL.md frontmatter `description`): max 1024 characters.
- Body: under 500 lines.
- Directory name must match the `name` field in SKILL.md frontmatter.

## Testing Locally

```bash
/plugin marketplace add ./
/plugin install elnora@elnora-plugins
```

## Adding a New Plugin

1. Create a directory with `.claude-plugin/plugin.json` and a `skills/` folder.
2. Register the plugin in `.claude-plugin/marketplace.json`.
3. Validate all skills against the spec constraints above.
