# Security Policy

## Supported Versions

Security updates are provided for the most recent release of `elnora-plugins`. The marketplace and its plugins are versioned together; we recommend always running the latest tag.

| Version | Supported |
| ------- | --------- |
| Latest release (`1.x`) | Yes |
| Older releases | No — please upgrade |

## Reporting a Vulnerability

Please report suspected vulnerabilities privately. Do not open a public issue for security reports.

- **Email:** [security@elnora.ai](mailto:security@elnora.ai)
- **GitHub:** use [private vulnerability reporting](https://github.com/Elnora-AI/elnora-plugins/security/advisories/new) on this repository

Include the affected version or commit, reproduction steps, and impact. We aim to acknowledge within 3 business days and to provide a remediation timeline after triage. Please give us a reasonable window to release a fix before any public disclosure. We are happy to credit reporters who wish to be named.

## Data Flow, Credentials & Secrets

This repository ships **client-side configuration and Agent Skills only** — it contains no server code and stores no credentials. Understanding what leaves your machine:

- **What it does:** installing a plugin from this marketplace registers the Elnora MCP server (`https://mcp.elnora.ai/mcp`) with your AI tool and copies skill files into that tool's skills directory. When you invoke a skill, your AI client sends requests to the Elnora Platform over HTTPS.
- **Credentials:** the MCP server authenticates by one of two methods, both handled by your MCP client — never by files in this repo:
  - **OAuth 2.1** (recommended) — a browser popup completes authorization on first connection; tokens are stored and refreshed by your AI client, not this repository.
  - **API key** — a Bearer token passed in the `Authorization` header, generated from your Elnora account settings. Keep it in an environment variable or your client's secret store; never commit it or paste it into skill files, configs, or issues.
- **What leaves your machine:** the prompts, task content, and files you choose to send to the Elnora Platform, plus your auth token, all over TLS to `mcp.elnora.ai`. The skills in this repo do not exfiltrate data to any other host.
- **No secrets in the repo:** no API keys, tokens, OAuth client secrets, or personal data are committed to this repository. `.env` and `*.local` files are gitignored. If you find a secret committed to git history, treat it as a vulnerability and report it via the channels above.

Because Agent Skills execute in the context of your AI tool, only install marketplaces and skills you trust, and review skill files before running them in a privileged environment.
