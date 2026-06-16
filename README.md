# Replitica — Agent Skills

Markdown skills that let any AI agent operate your [Replitica](https://replitica.com)
X/Twitter-growth account — Claude, Cursor, ChatGPT, Hermes, OpenClaw, and anything
else that loads agent skills or speaks MCP.

## Install

```bash
npx skills add amrtawfik160/replitica-skills
```

Then connect your account once:

1. Open **Replitica → Skills** and generate a personal access token.
2. Add the Replitica MCP server to your agent with that token. For Claude Code:
   ```bash
   claude mcp add --transport http --scope user replitica \
     https://ncvhxplskpmjintktwqj.supabase.co/functions/v1/mcp \
     --header "Authorization: Bearer replitica_pat_…"
   ```
   For other agents (Cursor, Hermes, OpenClaw, …), paste the JSON snippet shown on
   the Skills page into their MCP config.
3. Ask in chat: *"find my 10 best opportunities and draft replies."*

## Skills included

| Skill | Triggers on… |
|---|---|
| `replitica-grow` | "grow my account", the full scan → draft → queue loop, "check my numbers" |
| `replitica-engage` | "find opportunities", "draft replies", "queue these" |
| `replitica-autopilot` | "turn autopilot on/off", "is autopilot running", growth stats |

## What an agent can do

Scan for new opportunities · list & curate them · draft on-brand replies · queue
replies for review · manage autopilot · read keyword and growth stats.

## Backend

The skills call the **Replitica MCP server** (a Supabase Edge Function at
`/functions/v1/mcp`), authenticated with a personal access token you generate in
**Replitica → Skills**. Tokens are scoped to a single project and can be revoked
anytime.

## Safety

These skills never post publicly on their own. Reply tools only **draft** and
**queue** work into Replitica for you to review and publish.
