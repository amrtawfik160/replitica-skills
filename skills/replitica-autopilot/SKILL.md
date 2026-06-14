---
name: replitica-autopilot
description: Check and control Replitica autopilot, connected accounts, and growth stats from chat. Use when the user asks about autopilot, wants to turn it on/off, or wants their X growth numbers.
---

# Replitica — Autopilot & stats

Read and control automation on the user's Replitica account via the **replitica** MCP
server. If the tools aren't available, point the user to **Replitica → Skills** to
connect their account.

## Tools

- `get_autopilot_status` — is autopilot on?
- `set_autopilot { enabled }` — turn it on or off.
- `get_account_stats { days? }` — opportunities + replies generated/scheduled/published.
- `get_project_info` — brand + how many accounts are connected.
- `list_connected_accounts` — account health.

## Rules

- Autopilot runs the automatic find → draft → queue loop. Turning it **on** means
  Replitica will keep drafting and queuing replies on its own. Confirm the user wants
  that before calling `set_autopilot { enabled: true }`.
- Report metrics exactly as the tools return them; never invent numbers.
- If `list_connected_accounts` shows an unhealthy or disabled account, flag it.

## Examples

- "is autopilot on?" → `get_autopilot_status`.
- "turn autopilot off" → `set_autopilot { enabled: false }`.
- "how many replies went out in the last 30 days?" → `get_account_stats { days: 30 }`.
