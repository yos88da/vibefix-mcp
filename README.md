# VibeFix: fixed in your code, then tested on your live app.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

App broke after you published it? Our AI agent fixes it in your code, then tests your live app again so you see the result yourself. The first check is free.

You built the app with an AI tool, published it, and then something stopped working. Maybe new users can't finish signing up. Maybe the confirmation email never arrives, or checkout throws an error. Maybe you asked your builder's AI to fix it, it said it had, and the problem is still there for your users.

VibeFix starts where your users start. Paste the link to your live app and our AI agent signs up as a brand-new user. It walks through your pages the way a stranger would and sends you a screenshot of the problem it hits. The free check never changes your app.

If you want the problem fixed, the agent goes into your code. It looks for the cause, makes the change in your code, and then opens your live app again to test that the problem is gone. You see the re-check, so you're not taking anyone's word that it's fixed.

What it can work on: sign-up and login, emails that don't arrive or link to the wrong place, payments, API, integration and Supabase errors, and front-end bugs, as long as the cause is in the code.

Today the fix is for apps built with Lovable, or any app whose code is on GitHub.

A paid fix is available on vibe-fixer.com.

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

The skill tells a coding agent (Claude Code, Cursor, Codex and others) when to call VibeFix: when you say your deployed app is broken (log-in, emails, payments, an API or Supabase error, a blank page), or right after you publish a change. It only checks your own app, and only when you say yes.

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

## GitHub Action: check after every deploy

[VibeFix sign-up check](https://github.com/marketplace/actions/vibefix-sign-up-check) on the GitHub Marketplace ([source](https://github.com/yos88da/vibefix-signup-check)) runs the free check after each deploy and reports on the pull request whether a new user can get in. No API key: it proves which repo it runs in with GitHub's own token.

    - uses: yos88da/vibefix-signup-check@v1

See its README for the workflow, the permissions and which addresses it checks.

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
