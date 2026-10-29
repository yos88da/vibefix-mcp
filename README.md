# VibeFix: an AI agent that finds what is breaking your live app and fixes it in your code

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

VibeFix is an AI agent. A free check signs up to your live, published app like a brand-new person, with a real inbox, on a phone and a computer, and confirms what is broken. A paid fix is available on vibe-fixer.com.

Every app gets one free check. This repository is the MCP connection for it: add the server to your AI tool, then ask it to check your live app.

- **Endpoint:** `https://vibe-fixer.com/mcp`
- **Transport:** Streamable HTTP
- **Authentication:** two tools need no sign-in; five use OAuth sign-in to your VibeFix account (the client is prompted when you call one)
- **Website:** [vibe-fixer.com](https://vibe-fixer.com)
- **Server card:** [`/.well-known/mcp.json`](https://vibe-fixer.com/.well-known/mcp.json)
- **Registry name:** `com.vibe-fixer/vibefix`

## What it does

1. **Check.** `check_live_app` starts a check of your published app. It signs up as a brand-new person with a real inbox, follows the confirmation email, and uses the app on a phone and a computer. Every app gets one free check (one new app a day per account).
2. **Read.** `get_check_result` returns what it found, with evidence.
3. **Re-check.** `confirm_fix` re-checks those problems on your live app and marks each one fixed or not.

The free check never changes your app. A full check runs only when you confirm the app is yours; otherwise the check is look-only and does not sign up. Today the fix is for apps built with Lovable, or any app whose code is on GitHub.

## Free check, paid fix

The check is free: every app gets one free check. A paid fix is available on vibe-fixer.com.

Details: [vibe-fixer.com](https://vibe-fixer.com)

## Tools

| Tool | Sign-in | What it does |
|---|---|---|
| `assess_broken_app` | none | Describe a symptom in plain words; get the likely cause, the evidence that would confirm it, and what a fix involves. Reads the description only: no code, no access to your app. |
| `get_fix_options` | none | Returns the link to a sign-up check of your live app and how it works. Read-only: starts nothing, charges nothing. |
| `list_projects` | OAuth | Lists the checks on your VibeFix account. Read-only. |
| `check_repair_status` | OAuth | Says whether one check is still running. Read-only. |
| `check_live_app` | OAuth | Starts a check of your live app. Every app gets one free check. |
| `get_check_result` | OAuth | Reads what a check (or a re-check) found. Read-only. |
| `confirm_fix` | OAuth | Re-checks the reported problems on your live app after a fix. |

## Connect

### Claude Code

    claude mcp add --transport http vibefix https://vibe-fixer.com/mcp

### Cursor

Add to `~/.cursor/mcp.json`:

    { "mcpServers": { "vibefix": { "url": "https://vibe-fixer.com/mcp" } } }

Or use the one-click link on [vibe-fixer.com/tools](https://vibe-fixer.com/tools).

### VS Code

Add to `.vscode/mcp.json`:

    { "servers": { "vibefix": { "type": "http", "url": "https://vibe-fixer.com/mcp" } } }

### Claude (desktop and web)

Go to Customize, Connectors, press +, then Add custom connector. Paste `https://vibe-fixer.com/mcp` and press Add.

### Replit, Lovable and other tools

See [vibe-fixer.com/tools](https://vibe-fixer.com/tools). Any tool that takes a remote MCP server works with the address above. VibeFix is independent. Product names belong to their owners and are used only to say VibeFix works with them; VibeFix is not affiliated with or endorsed by them.

### As a plugin

    claude plugin marketplace add yos88da/vibefix-mcp
    claude plugin install vibefix@vibefix

This repository is also an [Agent Plugins](https://agent-plugins.org) package (`plugin.json`, `mcp.json`).

## Agent skill: tell your coding agent when to check

The skill tells a coding agent (Claude Code, Cursor, Codex and others) when to call VibeFix: when you say your deployed app is broken, when new users can't sign up or log in, or right after you publish a sign-up, log-in or payment change. It only checks your own app, and only when you say yes.

| Agent | One-line install |
|---|---|
| Claude Code (skill + server) | `claude plugin marketplace add yos88da/vibefix-mcp && claude plugin install vibefix@vibefix` |
| Any agent with skills | `npx skills add yos88da/vibefix-mcp` |
| Cursor (project rule) | `curl -fsSL https://raw.githubusercontent.com/yos88da/vibefix-mcp/main/agent-rules/cursor/vibefix.mdc --create-dirs -o .cursor/rules/vibefix.mdc` |
| Codex (AGENTS.md) | `curl -fsSL https://raw.githubusercontent.com/yos88da/vibefix-mcp/main/agent-rules/AGENTS.md >> AGENTS.md` |

The skill is [`skills/vibefix-live-check/SKILL.md`](skills/vibefix-live-check/SKILL.md). The Cursor rule and the AGENTS.md section say the same thing in short. Cursor and Codex also need the server connected (see Connect above).

### Codex

    codex mcp add vibefix --url https://vibe-fixer.com/mcp
    codex mcp login vibefix

## Example prompts

- "Check my live app at https://example.com like a brand-new user."
- "Re-check the problems from my last check."
- "New people say the confirmation email never arrives. What is the likely cause?"

## Limits

- Only apps with a public web address can be checked.
- The AI agent is often right and sometimes wrong. Look at the evidence each finding shows.
- `assess_broken_app` works from a description alone and says so in its answer.

## Privacy and support

Privacy: [vibe-fixer.com/privacy](https://vibe-fixer.com/privacy). Terms: [vibe-fixer.com/terms](https://vibe-fixer.com/terms). Support: [help@vibe-fixer.com](mailto:help@vibe-fixer.com).

This repository contains documentation, plugin metadata and the agent skill only. The service itself is at vibe-fixer.com.
