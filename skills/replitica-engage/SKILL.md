---
name: replitica-engage
description: Find X/Twitter engagement opportunities in Replitica and draft on-brand replies for them. Use when the user wants to find people to reply to, draft replies, or queue replies for review.
---

# Replitica — Engage

Find opportunities and turn them into reviewed, queued replies via the **replitica**
MCP server. This skill focuses on the engagement loop only.

If the `replitica` tools aren't available, tell the user to connect their account in
**Replitica → Skills** first.

## Flow

1. `get_project_info` once, to learn the brand voice and what it does.
2. `list_opportunities { min_score?, platform?, status?, limit? }` to find posts
   worth replying to. They're ranked by relevance. If the user wants fresh ones (or
   there are none), `scan_opportunities` first — confirm it with them, it uses scan
   credits — then wait ~30-60s and `list_opportunities` again.
3. `draft_reply { opportunity_id, instructions? }` for each chosen opportunity.
   Show the user every draft.
4. `schedule_reply { opportunity_id, scheduled_for? }` ONLY for drafts the user
   approves. This queues them in Replitica — it does not post them.

Use `update_opportunity { opportunity_id, status: "skipped" }` to dismiss
irrelevant ones, or `feedback: "up" | "down"` to improve future targeting.

## Rules

- Reply tools draft and queue only. Never say a reply was posted/published.
- React to what the post actually says; don't paraphrase it. Human tone, no
  emojis/hashtags/exclamation marks/em dashes, under ~220 chars.
- Confirm before queueing in bulk. Default to drafts the user can review.
- Surface `list_scheduled_replies` when the user asks what's queued.
