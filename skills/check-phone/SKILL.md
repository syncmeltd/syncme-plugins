---
name: check-phone
description: Check phone numbers for spam, fraud, or safety concerns. Use when the user shares a phone number, including casual prompts like "is this legit?", "missed call from unknown number", or "should I call back?", even without exact keywords like "verify" or "spam".
---

# Check Phone Skill

## Purpose

Verify whether a phone number should be trusted. Treat Sync.me MCP tool results as the source of truth. Avoid heuristic phone, spam, or fraud judgments when the MCP tools are available.

Phone number lookups use account credits. Each successful phone lookup may consume credits from the authenticated user's balance.

## Response Language

Respond in the same language the user used in the request.

## Required MCP Tools

Requires the Sync.me MCP server `https://mcp.sync.me/mcp`.

- `check_phone_number`: check whether a phone number should be treated as spammer.

## Phone Verification Workflow

1. Attempt the requested Sync.me MCP tool call only if the tool is already available in the current environment. If it is unavailable or unauthorized, say the number could not be verified and ask the user to connect Sync.me mcp connector first.
2. Extract the phone number from the user message.
3. Call `check_phone_number` with the extracted value as `phone_number`.
4. Decide the verdict using this priority order — first match wins:
   - `🚨 Do not trust this number` when `status` is `ok` and `is_fraud` is `true` OR `is_big_spammer` is `true` (heavy spam volume — treat as equally serious as fraud).
   - `⚠️ Be careful with this number` when `status` is `ok` and `is_spam` is `true` OR `spam_reports` >= 5.
   - `✅ Known business number` when `status` is `ok`, `is_business` is `true`, and all negative flags are `false` and `spam_reports` is absent or < 5.
   - `✅ No risk signals found` when `status` is `ok` and all negative flags are `false`.
   - `❌ Invalid phone number` when `status` is `invalid_phone_number`.
   - `💳 Credits required` when `status` is `insufficient_credits`.
   - `🔎 Could not verify` when `status` is `too_many_requests`, `forbidden`, `not_found`, `error`, the MCP tool/server is unavailable, the number is missing, or the response is invalid.

   When multiple negative flags are true, use the highest-priority verdict and list all signals. Do not call something fraud only because it is spam. Use `phone_number` from the response in the answer when not `null`; it is returned in international display format.

### Phone Output Format

Use emojis as visual markers, not as a replacement for evidence. Omit lines with missing or `null` values.

```markdown
## <verdict>

<one short sentence explaining the result>

**📞 Number:** <phone_number>

**👤 Name:** <name>

**📌 Signals:**

- <present signal>
- <present signal>

<one short sentence directly answering what the user asked>
```

Signals — list in this order:

- `🚨 Fraud signal detected` when `is_fraud` is `true`.
- `🔥 Heavy spam complaints` when `is_big_spammer` is `true`.
- `⚠️ Spam complaints detected` when `is_spam` is `true`.
- `📣 Spam reports: <spam_reports>` when `spam_reports` is present.
- `🏢 Business number` when `is_business` is `true`. If `company_domain` is present, use it directly as the industry label (e.g. `company_domain: "Delivery"` → `🏢 Business number · Delivery`). Otherwise fall back to `name` if it contains a recognizable company name. Prioritize local brands from the phone number's country. Omit if neither field gives a clear signal.

For `insufficient_credits`, use:

```markdown
## 💳 Credits required

I could not verify this number because your account does not have enough lookup credits.

**🛒 Purchase credits:** `https://mcp.sync.me/pricing`

Buy credits, then retry the phone check.
```

Results with `status` of `insufficient_credits` or `too_many_requests` are transient errors — never treat them as a cached answer. Always call `check_phone_number` again when the user asks to verify a number, even if the same number was checked earlier in the conversation and returned one of these statuses.
