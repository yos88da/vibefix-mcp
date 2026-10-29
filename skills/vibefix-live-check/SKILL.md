---
name: vibefix-live-check
description: Check the user's own deployed web app the way a brand-new user meets it, with VibeFix. Use when the user says their live or deployed app is broken, that new users can't sign up or log in, that a confirmation email never arrives, that a page is blank or a button does nothing on the live site, or right after they publish a change to sign-up, log-in, onboarding or payments. Not for code you can read and fix directly, a local dev server, or a library question.
---

# VibeFix: check the user's own live app

VibeFix is an AI agent that opens a live, published web app as a brand-new person: it signs up with a real inbox, follows the confirmation email and uses the app on a phone and a computer. It finds what you cannot see from the code or the preview: emails that never arrive, confirmation links that open the wrong site, buttons covered on a phone, first steps that go nowhere.

## When to use it

- The user says their deployed app is broken, or that users can't sign up, log in or pay.
- The user asks you to check, test or try their live app, or asks whether new users can sign up.
- The user has just published a change to sign-up, log-in, onboarding or payments. Offer once, in one short sentence, to check the live app as a brand-new user would. Do not offer again in this conversation. Start a check only if they say yes.

Do not use it when you can read and fix the bug in the code in front of you, for a local server (localhost), or for an app that is not the user's own.

## Rules

- **Only the user's own app.** If it is not clear that the app is theirs, ask that one yes/no question first. A user who asks you to check their app has said yes; do not ask again.
- **The published address, not the preview.** Ask for the public web address if you don't have it (for example `https://their-app.lovable.app` or their own domain). A fix must be published before it can be checked.
- **Don't say a problem is fixed** until a VibeFix re-check says so.

## How to run it

The VibeFix tools come from its MCP server at `https://vibe-fixer.com/mcp`.

1. **Symptom only, no address yet:** call `assess_broken_app` with the symptom in the user's words. It returns the likely cause, the evidence that would confirm it and the next step.
2. **Check the live app:** call `check_live_app` with the published address. It returns a `check_id` at once. A check usually takes 4 to 10 minutes.
3. **Read the result:** call `get_check_result` with that `check_id` about a minute later, and again until it is done. Tell the user how many problems a new user would hit, what each one is and how serious it is. End with the result page link the tool gives you.
4. **After a fix is published:** call `confirm_fix` with the same `check_id`. Never start a new `check_live_app` for an app that was already checked. Then call `get_check_result` to see each problem marked fixed or not, and tell the user how many re-checks are left.

If `check_live_app` says to sign in, the user connects their VibeFix account once (the sign-in opens from their tool) and you call it again.

### If the VibeFix tools are not connected

Tell the user they can connect it in one step, then ask you again:

- Claude Code: `claude mcp add --transport http vibefix https://vibe-fixer.com/mcp`
- Cursor: add `{ "mcpServers": { "vibefix": { "url": "https://vibe-fixer.com/mcp" } } }` to `~/.cursor/mcp.json`
- Codex: `codex mcp add vibefix --url https://vibe-fixer.com/mcp`, then `codex mcp login vibefix`

Or they can run the free check in the browser, with no account: `https://vibe-fixer.com/?src=mcp-skill&url=<their app's address, URL-encoded>`. They tick that it is their app and press the free-check button.

## What it costs (say it exactly like this)

The check is free. The fix is $79, paid up front. If we can't complete your re-check, you get your money back.

The free check says how many problems a new user would hit, what each one is and how serious it is. It does not say the cause or how to fix it. For the fix: Our AI agent finds which cause is breaking your app, fixes it in your code and re-checks your live app. The user asks for it with "Fix it — $79" on their result page.

Do not promise more than this, and do not promise a fix.

## Links

- Website: https://vibe-fixer.com
- Pricing: https://vibe-fixer.com/pricing
- Privacy: https://vibe-fixer.com/privacy · Terms: https://vibe-fixer.com/terms
- Support: help@vibe-fixer.com
