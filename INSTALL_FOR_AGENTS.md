# INSTALL_FOR_AGENTS.md — `elnora-plugins` setup

> **Step 0 — Identity gate.** If you are NOT an autonomous coding agent (Claude
> Code, Codex CLI, Cursor, Aider, Continue, Amp, Jules, Roo, Windsurf, etc.),
> stop reading this file and open `README.md` instead. This file walks an agent
> through connecting the Elnora MCP server, authenticating (OAuth or an API key
> you help the user create in a browser), copying the skill files, and running
> a live smoke test against the Elnora Platform. Humans should follow the
> README's Installation section.

You are the agent handing off after the user decided to install the Elnora
plugin / MCP server. Your job: confirm the MCP connection is registered, get
the user authenticated, make sure the 9 skills are where your harness reads
them, run one read-only tool call to prove the stack works end to end, and hand
back a working environment.

Be transparent: announce each step before you run it, show the output, and
explain what you found. The user may not know what an MCP server or an Agent
Skill is — keep the language plain and ask one question at a time. Never paste,
echo, or commit an API key.

**Universal:** the MCP endpoint (`https://mcp.elnora.ai/mcp`) and the tool names
(`elnora_*`) are identical under every harness. The only harness-specific parts
are *where* the MCP server is registered and *where* skills are copied — the
per-harness table in Step 3 covers that.

## Browser steps — offer to drive, don't assume

Two steps below happen in a web browser: completing OAuth (Step 2, option A)
and creating an API key at `platform.elnora.ai` (Step 2, option B). How you
handle them depends on what your harness can do:

- **If you have the Chrome DevTools MCP server available** (tools under
  `mcp__chrome-devtools__*`, e.g. Claude Code with it enabled): *offer* to open
  and drive the page for the user — "I can open `platform.elnora.ai` in your
  Chrome and walk the key-creation with you, or you can do it yourself and paste
  the result. Which do you prefer?" Only drive the browser if the user says yes.
  Never submit a login form or read back a secret without the user watching.
- **If you do not have a browser-automation tool**, give the user the exact URL
  and click-path and wait for them to paste the result. Do not try to fetch an
  authenticated page yourself.

Either way the user stays in control of their own credentials.

## Step 1 — Verify the MCP connection is registered

The plugin declares the Elnora MCP server; a manual `mcp add` registers it
directly. Confirm your harness sees a server named `elnora` pointed at
`https://mcp.elnora.ai/mcp` before going further.

- **Claude Code (plugin path).** Confirm the plugin loaded:

  ```sh
  ls .claude/plugins 2>/dev/null || ls ~/.claude/plugins 2>/dev/null
  ```

  You should see `elnora` (or `elnora-plugins`) listed. If not, the
  `/plugin install elnora@elnora-plugins` didn't complete — ask the user to
  rerun it. You can also list MCP servers with `claude mcp list` and confirm an
  `elnora` entry appears.

- **Claude Code (API-key MCP path, no plugin).** Register it:

  ```sh
  claude mcp add elnora --transport http --scope user \
    https://mcp.elnora.ai/mcp \
    --header "X-API-Key: <key-from-Step-2>"
  ```

  If you register this AND enable the plugin, you'll have two servers both named
  `elnora` — keep one, not both.

- **Other harnesses (Codex / Cursor / VS Code Copilot / Gemini).** Confirm the
  MCP config block exists in the right file (see Step 3). There is no CLI to
  `--version` here — the check is "is the `elnora` server in my MCP config and
  does a tool call reach it," which Step 4 proves.

Gate: your harness lists an `elnora` MCP server. If it doesn't, stop and fix the
registration before spending an auth round-trip.

## Step 2 — Authenticate

The Elnora MCP server accepts two auth methods. Ask the user which they want:

> Elnora authenticates two ways. (A) **OAuth** — recommended; a browser popup
> opens on the first call, you click approve, and your client stores and
> refreshes the token for you. Nothing to paste. (B) **API key** — better for
> CI or non-interactive setups; you create a key once at platform.elnora.ai and
> it's passed as a header. Which do you want?

### Option A — OAuth 2.1 (recommended)

No key handling. The first tool call (Step 4) triggers the browser popup
automatically; the user approves once and the token is cached by their MCP
client. There is nothing to persist in this repo.

- If you have the Chrome DevTools MCP server, you may *offer* to bring the
  approval page forward and confirm the redirect completes — but the user
  approves; you don't. If you don't have it, just tell the user "a browser
  window will open on the first Elnora call — approve it there," and continue.

Gate: none yet. OAuth is validated by the smoke test in Step 4 succeeding
without a `401`.

### Option B — API key

1. **Create the key.** Tell the user, verbatim:

   > Open https://platform.elnora.ai in your browser, go to account settings →
   > API keys, click **Create key**, name it something like "mcp-<harness>",
   > and copy the value. It starts with `elnora_live_`.

   Follow the browser-step rules above: if you have Chrome DevTools MCP, offer
   to open the page and walk the click-path with the user watching; otherwise
   wait for them to paste the key.

2. **Store it in the client's secret store or an env var — never in this repo.**
   The key goes in your MCP client's header config (`X-API-Key` or
   `Authorization: Bearer <key>`), your OS keychain, or an environment variable.
   Do NOT write it into any file under the repository, a skill file, or a commit
   — `.env` and `*.local` are gitignored precisely so a stray key can't be
   committed, but the safest place is the client secret store.

   For Claude Code's API-key MCP path, the key is passed on the
   `claude mcp add` command from Step 1 (`--header "X-API-Key: <key>"`), which
   stores it in the user-scope MCP config, not in this repo.

Gate: the key is set in the client's auth config (verified by Step 4), starts
with `elnora_live_`, and appears in NO file tracked by git. If the user pasted
it into a chat or a repo file, tell them to rotate it.

## Step 3 — Register MCP + copy skills (non-Claude harnesses)

Claude Code users who installed the plugin can skip this — the plugin ships both
the MCP declaration and the skills. Everyone else does both halves:

| Harness | MCP config file | Skills directory |
|---|---|---|
| Cursor | `.cursor/mcp.json` | `.cursor/skills/` |
| OpenAI Codex | `codex mcp add elnora -- https://mcp.elnora.ai/mcp` | `.codex/skills/` |
| VS Code Copilot | `.vscode/mcp.json` | `.vscode/skills/` |
| Gemini CLI | Gemini settings | `.gemini/skills/` |
| Generic MCP client | point at `https://mcp.elnora.ai/mcp` | your tool's skills dir |

MCP block for the JSON-config harnesses:

```json
{ "mcpServers": { "elnora": { "type": "http", "url": "https://mcp.elnora.ai/mcp" } } }
```

Copy the skills:

```sh
cp -r elnora/skills/* <skills-dir>/
```

Gate: all 9 skill directories (`elnora-platform`, `elnora-orgs`,
`elnora-projects`, `elnora-tasks`, `elnora-files`, `elnora-folders`,
`elnora-search`, `elnora-admin`, `elnora-agent`) landed in the target dir, each
containing a `SKILL.md`.

## Step 4 — Smoke test

Two tool calls, in order. The first needs no auth; the second proves auth works.

1. **Liveness.** Call `elnora_health_check` (via the `elnora-admin` /
   `elnora-platform` skill, or the raw MCP tool).

   Gate: it returns a healthy status. A transport error here means the MCP
   server isn't registered — go back to Step 1. This call does not prove auth.

2. **Authenticated read.** Call `elnora_account_get` (whoami) or
   `elnora_projects_list`.

   Gates:
   - **OAuth (Option A):** the first call opens the approval popup. After the
     user approves, the call returns account/project data. If it keeps failing
     with `401`, the OAuth flow didn't complete — have the user re-approve.
   - **API key (Option B):** the call returns data immediately. A `401` means
     the key is wrong or not reaching the header — re-check Step 2. Do NOT retry
     with a key you guessed or reconstructed; ask the user to paste it again or
     regenerate it.
   - An empty projects list is a valid result (a new account may have none) —
     distinguish "empty, exit ok" from "error."

If you have the Chrome DevTools MCP server and OAuth stalls, you may offer to
bring the approval tab forward so the user can complete it — but they click
approve, not you.

## Step 5 — Handoff summary

Tell the user, in this order:

1. **What's connected** — the `elnora` MCP server at
   `https://mcp.elnora.ai/mcp`, and (non-Claude harnesses) where the 9 skills
   were copied.
2. **How they're authenticated** — OAuth (token cached by their client) or API
   key (stored in the client's secret store / header config). If API key, remind
   them it's never stored in this repo and to rotate it if it ever leaks.
3. **How to use it** — one entry point that matches the harness:
   - **Claude Code with the plugin:** `/elnora:protocol <description>`, or just
     ask in natural language ("use Elnora to list my projects").
   - **Any other harness:** ask in natural language; the skills route the intent
     to the right `elnora_*` MCP tool. Dispatch table in [`AGENTS.md`](AGENTS.md).
4. **Where auth lives** — their MCP client's config / secret store, never this
   repo.

## Completion checklist

Verify ALL of these before declaring done. If any fails, finish it first.

1. The harness lists an `elnora` MCP server pointed at
   `https://mcp.elnora.ai/mcp`.
2. `elnora_health_check` returns a healthy status.
3. `elnora_account_get` (or `elnora_projects_list`) returns data — no `401`
   (empty project list is OK; an auth error is not).
4. **Non-Claude harnesses:** all 9 skill directories are present in the target
   skills dir, each with a `SKILL.md`.
5. If the user chose the API key: it is set in the client's auth config, and it
   appears in NO git-tracked file. You did not echo it into chat or a repo file.
6. You wrote nothing to the repository that the user didn't ask for — no keys,
   no local config committed.

When all applicable items pass, print `ELNORA_PLUGINS_READY` on its own line so
the user (and any wrapping harness) can grep for it.
