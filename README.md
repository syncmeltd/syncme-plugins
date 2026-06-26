# syncme-plugins

Official [Sync.me](https://sync.me/) plugin marketplace for Claude Code and Cowork.

## Plugins

| Plugin | Description |
|---|---|
| [Sync.me Caller ID](#syncme-caller-id) | Identify spam callers, detect fraud numbers, and look up businesses |

---

# Sync.me Caller ID

Identify spam callers, detect fraud numbers, and look up businesses with the [Sync.me](https://sync.me/) MCP Server.
- Fast to install.
- Real-world phone data from the Sync.me database.
- Designed for everyday safety workflows.

---

## 🔌 One-Click MCP Server Integration

This plugin automatically configures the Sync.me MCP Server when installed.
No manual server setup or config files.
Install the plugin, authenticate with Sync.me, and run `/syncme-caller-id:*` commands.

---

## ✅ Skills

| Skill | What it does |
|---|---|
| `/syncme-caller-id:check-phone` | Share a number and instantly find out if it's a known business, spammer, or fraud source |
| `/syncme-caller-id:check-credits` | Check your remaining lookup credit balance |

### `/syncme-caller-id:check-phone`

Best for: verifying unknown calls before calling back, flagging suspicious numbers.<br>
Input: any phone number — or just paste it in a message ("is +1 (415) 555-XXXX legit?")<br>
Output: verdict with fraud/spam/business signals and a plain-English summary

Triggers automatically — no slash command needed. Just mention a phone number or ask "should I call back?".

### `/syncme-caller-id:check-credits`

Best for: checking how many lookups you have left.

---

## 📦 Installation

### Cowork

Click the link below to install in one step:

[Install in Cowork](https://claude.ai/desktop/customize/plugins/new?marketplace=syncmeltd/syncme-plugins&plugin=syncme-caller-id)

Then restart Cowork to ensure the MCP server starts correctly.

### Claude Code

#### 1. Add this plugin's marketplace

```
/plugin marketplace add syncmeltd/syncme-plugins
```

#### 2. Install the plugin

```
/plugin install syncme-caller-id@syncme-plugins
```

#### 3. Restart Claude Code

This ensures the MCP server starts correctly.

---

## 🔑 Authentication

The Sync.me MCP Server supports **OAuth**:

1. After installation, run `/mcp` in Claude Code or connect to the Sync.me connector from settings
2. Select the **Sync.me** server and click **Authenticate**
3. Complete the Sync.me login in your browser
4. Done — all commands are now ready to use

---

## ⚠️ Credits and Safety

Phone number lookups consume credits from your Sync.me account. Each successful lookup costs 1 credit.

To buy credits: [mcp.sync.me/pricing](https://mcp.sync.me/pricing)

---

## Quickstart Examples

Try these in Claude:

- "Is +1 (415) 555-XXXX safe to call back?"
- "Got a missed call from +972 3 XXX XXXX — who is it?"
- "Check if this number is spam: +39 02 XXXX XXXX"
- "How many credits do I have left?"

---

## 🙌 Credits

- **MCP Server** by [Sync.me Technologies Ltd](https://sync.me/)
- **Plugin Specification** by [Anthropic](https://docs.anthropic.com/)

---

## License

MIT — Sync.me Technologies Ltd
