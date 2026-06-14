---
name: replitica-grow
description: Operate a Replitica X/Twitter growth account — understand the brand, find engagement opportunities, draft on-brand replies, queue them for review, and manage autopilot. Use whenever the user wants to grow their X account, engage with prospects, or check their Replitica numbers.
---

# Replitica — Grow

You can operate the user's Replitica account through the **replitica** MCP server. Replitica is
an X/Twitter growth tool: it scans for posts matching the brand's keywords
("opportunities"), drafts on-brand replies, queues them, and can run that loop on
autopilot.

## Before you act

1. If the `replitica` MCP tools are not available, the user hasn't connected their
   account. Tell them to open **Replitica → Skills**, generate a token, and add the
   `replitica` MCP server to this agent. Do not guess endpoints.
2. Always call **`get_project_info`** first in a session to learn the brand name,
   what it does, how many accounts are connected, and whether autopilot is on.
   Ground every reply draft in that context.

## Tools

- `get_project_info` — brand name, description, website, autopilot state, accounts.
- `get_account_stats { days? }` — opportunities found + replies generated/scheduled/published over N days.
- `scan_opportunities { count? }` — start a fresh scan for NEW opportunities. Background, ~30-60s, uses scan credits.
- `list_opportunities { status?, platform?, min_score?, limit? }` — posts worth replying to, ranked by relevance.
- `get_opportunity { opportunity_id }` — full detail incl. any saved draft.
- `update_opportunity { opportunity_id, status?, feedback? }` — skip/dismiss, reopen, or thumbs up/down. Does not publish.
- `list_keywords { limit? }` — tracked keywords and how many opportunities each drove.
- `draft_reply { opportunity_id, instructions? }` — generate & save a reply DRAFT. Does not publish.
- `schedule_reply { opportunity_id, reply_text?, scheduled_for? }` — queue a reply for review. Does not publish.
- `list_scheduled_replies { status?, limit? }` — what's queued.
- `list_connected_accounts` — connected X accounts and their health.
- `get_autopilot_status` / `set_autopilot { enabled }` — read/toggle autopilot.

## The core loop

When the user says something like "find opportunities and draft replies":
1. `list_opportunities` first — there may already be fresh ones to work with.
2. If they want NEW ones (or the list is stale/empty), `scan_opportunities`, wait
   ~30-60s, then `list_opportunities` again. Confirm first — scans cost credits.
3. For each chosen one, `draft_reply`. Show the user the drafts.
4. Only `schedule_reply` for the ones the user approves.

## Rules

- **Never imply something was posted.** Reply tools only draft & queue. Always say
  "drafted" / "queued for review", and point to Replitica → Replies / Schedule.
- **Scans cost credits.** Always confirm before `scan_opportunities`, and prefer
  existing opportunities (`list_opportunities`) before starting a new scan.
- Before queueing more than a few replies, show the drafts and get a clear yes.
- Keep replies on-brand and human: react to the post, don't paraphrase it; no
  emojis, hashtags, exclamation marks, or em dashes; under ~220 characters.
- If a tool returns an error about a missing scope, tell the user to recreate the
  token with the needed permission in Replitica → Skills.
- Report numbers plainly from the tools; don't invent metrics.

## Examples

- "how's my account doing this week?" → `get_account_stats { days: 7 }` + summarize.
- "find founders talking about churn and draft replies" → `list_opportunities`,
  then `draft_reply` for the top matches, show drafts.
- "queue the first three for tomorrow morning" → `schedule_reply` for those three
  with a morning `scheduled_for`.
- "turn on autopilot" → confirm intent, then `set_autopilot { enabled: true }`.
