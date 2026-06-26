---
name: check-credits
description: Check the authenticated user's Sync.me Caller ID lookup credit balance. Use when the user asks how many credits they have, asks for lookup balance, or says things like "check credits", "credits balance", or "how many lookups do I have left?".
---

# Caller ID Credits Skill

## Purpose

Check the user's available Caller ID lookup credits. Treat MCP tool results as the source of truth. Never invent balances.

## Response Language

Respond in the same language the user used in the request.

## Required MCP Tools

Requires the SyncMe MCP server.

- `check_credits`: returns the authenticated user's credit balance.

## Workflow

`check_credits` results are never cached — always call the tool to get a fresh balance.

1. Attempt the requested Sync.me MCP tool call only if the tool is already available in the current environment. If it is unavailable or unauthorized, say the balance could not be retrieved and ask the user to connect Sync.me mcp first.
2. Call `check_credits` with no input.
3. Render the result:

```markdown
💳 **Credits balance**

💚 Available: <balance>
📊 Used: <used_credits> of <total_credits>

🛒 [Get more credits](https://mcp.sync.me/pricing)
```
