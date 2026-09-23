# VibeFix — MCP server for broken AI-built apps

**Triage for apps built with Lovable, Base44, v0, Bolt, Replit and other AI app builders.**

Someone built an app with an AI builder, and now something is broken that
they can't debug themselves: a button that does nothing, a blank page after
login, data that won't save, or a deploy that broke a working app. Connect
this server, and your agent can turn that description into a likely cause,
how to confirm it, and what a fix involves.

- **Endpoint:** `https://vibe-fixer.com/mcp`
- **Transport:** Streamable HTTP
- **Authentication:** none
- **Cost:** free to connect and call
- **Website:** [vibe-fixer.com](https://vibe-fixer.com)

---

## Connect

### Claude Code

```bash
claude mcp add --transport http vibefix https://vibe-fixer.com/mcp
```

### Cursor

Add to `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "vibefix": { "url": "https://vibe-fixer.com/mcp" }
  }
}
```

### VS Code

Add to `.vscode/mcp.json`:

```json
{
  "servers": {
    "vibefix": { "type": "http", "url": "https://vibe-fixer.com/mcp" }
  }
}
```

### Claude (desktop and web)

**Settings → Connectors → Add custom connector**, then paste
`https://vibe-fixer.com/mcp`.

### As a plugin

This repository is an [Agent Plugins](https://agent-plugins.org) package: `plugin.json`
and `mcp.json` at the root declare the server, so plugin-aware clients and directories
can install it straight from `https://github.com/yos88da/vibefix-mcp`.

### Any other MCP client

Point it at `https://vibe-fixer.com/mcp` over Streamable HTTP. The server
supports MCP protocol versions `2026-07-28`, `2025-11-25`, `2025-06-18` and
`2025-03-26`. A machine-readable server card is published at
[`/.well-known/mcp.json`](https://vibe-fixer.com/.well-known/mcp.json).

---

## Tool

### `assess_broken_app` — Assess a broken AI-built app

Read-only (`readOnlyHint: true`). It works from the description alone: it
does not read code, open the app, or change anything.

**Input**

| Field | Required | Description |
|---|---|---|
| `symptom` | yes | What goes wrong, in the person's own words, e.g. "The save button does nothing". |
| `platform` | no | The builder it was made with: `lovable`, `base44`, `v0`, `bolt`, `replit`, `other`. |
| `url` | no | The public URL of the deployed app, if there is one. |

**Output** (structured)

| Field | Description |
|---|---|
| `likely_cause` | The most common cause behind that symptom in AI-built apps. |
| `confidence` | `low` or `medium`. It is never higher, because this is inferred from a sentence, not from the app. |
| `evidence_to_confirm` | What to look at to confirm or rule out the cause. |
| `what_a_fix_involves` | The shape of the fix. |
| `what_this_assessment_cannot_know` | The limits of a description-only triage, stated plainly. |
| `next_step` | How to get the app actually repaired. |
| `platform` | Echoed back when it was provided. |

**Example**

Prompt: *"My Lovable app's save button does nothing and my changes are gone after refresh."*

```json
{
  "likely_cause": "A request the page makes when you press the control is failing — most often an authentication or permissions error on the backend, or a call to an endpoint that no longer exists.",
  "confidence": "medium",
  "evidence_to_confirm": [
    "The network log at the moment of the click: a request with a 4xx or 5xx status is the answer.",
    "The browser console usually reports only that \"a resource failed to load\" and not which one, so the console alone is rarely enough."
  ],
  "what_a_fix_involves": "Reproducing the click against the live app, reading the failing request, and correcting the call or the permission rule behind it. Usually a small change in one file.",
  "what_this_assessment_cannot_know": "This is inferred from the description alone — no code, no logs and no access to the app. The cause named above is the most common one behind that symptom in apps from AI builders, not a diagnosis of this one.",
  "platform": "lovable"
}
```

*(`next_step` is left out of this example for length.)*

---

## Example prompts

- "My Lovable app shows a blank white page right after I log in. What's wrong?"
- "The Save button in my Bolt app does nothing — no error, nothing saves."
- "I redeployed my v0 app and now the signup form fails with 'permission denied'. It worked yesterday."
- "My Base44 app's dashboard loads forever after I added a new page."

---

## When the app needs an actual fix

`assess_broken_app` is triage: an informed first read of a symptom. To get
the app repaired, go to [vibe-fixer.com](https://vibe-fixer.com). There, an
AI agent opens the real app, reproduces the failure live, finds the cause and
fixes it while you watch. When the code lives on GitHub, the fix arrives as a
pull request for you to approve. The first diagnosis is free.

Plans and pricing: [vibe-fixer.com/pricing](https://vibe-fixer.com/pricing)

---

## Privacy

The tool receives only what the calling agent sends: the symptom, and
optionally the builder name and a public URL. It needs no account and no
access to your code or your app.
Full policy: [vibe-fixer.com/privacy](https://vibe-fixer.com/privacy) ·
Terms: [vibe-fixer.com/terms](https://vibe-fixer.com/terms)

## Support

[help@vibe-fixer.com](mailto:help@vibe-fixer.com)

---

This repository contains documentation only. The VibeFix service itself is
hosted at vibe-fixer.com.
