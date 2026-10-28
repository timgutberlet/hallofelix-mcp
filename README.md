# HalloFelix von InsurMagic – MCP Server

Official [Model Context Protocol](https://modelcontextprotocol.io) server for [HalloFelix von InsurMagic](https://www.hallofelix.de), the AI phone and WhatsApp assistant for insurance brokers in Germany, Austria and Switzerland.

Connect Claude, ChatGPT, Cursor, VS Code or any MCP client to your HalloFelix account and let your AI agent read calls and transcripts, manage customers and outbound call campaigns, tune your voice and WhatsApp assistants, and work your WhatsApp inbox.

**Server URL:** `https://app.hallofelix.de/api/mcp` (remote, Streamable HTTP)

## Authentication

- **OAuth (recommended):** clients with connector support (Claude.ai, Claude Desktop, ChatGPT, Cursor, VS Code) only need the URL. You sign in with your HalloFelix account and allow access (admins of the account only).
- **API token:** send your token as `Authorization: Bearer <api_access_token>`. Create it in the HalloFelix app under **Integrationen → API**.

## Setup

### Claude.ai / Claude Desktop

Settings → Connectors → **Add custom connector** → URL `https://app.hallofelix.de/api/mcp` → Connect.

### ChatGPT

Settings → Apps & Connectors → Advanced → enable **Developer mode** → **Create** → URL `https://app.hallofelix.de/api/mcp`, authentication **OAuth**.

### Claude Code

```bash
claude mcp add --transport http hallofelix https://app.hallofelix.de/api/mcp
```

Then run `/mcp` to sign in. With a token instead: append `--header "Authorization: Bearer <api_access_token>"`.

### Cursor

[![Add to Cursor](https://cursor.com/deeplink/mcp-install-dark.svg)](https://cursor.com/en/install-mcp?name=hallofelix&config=eyJ1cmwiOiJodHRwczovL2FwcC5oYWxsb2ZlbGl4LmRlL2FwaS9tY3AifQ==)

or in `~/.cursor/mcp.json`:

```json
{ "mcpServers": { "hallofelix": { "url": "https://app.hallofelix.de/api/mcp" } } }
```

### VS Code (GitHub Copilot)

[Install in VS Code](https://insiders.vscode.dev/redirect/mcp/install?name=hallofelix&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A//app.hallofelix.de/api/mcp%22%7D)

or `code --add-mcp '{"name":"hallofelix","type":"http","url":"https://app.hallofelix.de/api/mcp"}'`

### Windsurf

`~/.codeium/windsurf/mcp_config.json`:

```json
{ "mcpServers": { "hallofelix": { "serverUrl": "https://app.hallofelix.de/api/mcp" } } }
```

### OpenAI Codex CLI

```bash
codex mcp add hallofelix --url https://app.hallofelix.de/api/mcp
codex mcp login hallofelix
```

### Gemini CLI

`~/.gemini/settings.json`:

```json
{ "mcpServers": { "hallofelix": { "httpUrl": "https://app.hallofelix.de/api/mcp" } } }
```

### Any stdio-only client

```json
{
  "mcpServers": {
    "hallofelix": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://app.hallofelix.de/api/mcp"]
    }
  }
}
```

## Tools

| Tool | What it does | Access |
|---|---|---|
| `list_calls` | Lists phone calls handled by the HalloFelix voice AI (inbound and outbound), oldest change first, with transcript, summary and extracted data points | read |
| `get_call` | Full call record: transcript, summary, summary data points, customer, campaign and assistant | read |
| `list_customers` | Lists customers (contacts the AI calls or chats with) with phone numbers, call count and last call time | read |
| `get_customer` | Full customer record | read |
| `create_customer` | Creates a customer | write |
| `list_campaigns` | Lists outbound call campaigns with success and answer rates | read |
| `get_campaign` | Full campaign record with computed statistics | read |
| `create_campaign` | Creates an outbound call campaign | write |
| `start_campaign` | Starts or resumes a campaign: the AI begins calling its customers | write |
| `set_campaign_status` | Changes a campaign run status | write |
| `list_call_assistants` | Lists the voice AI phone assistants (id, name, phone number, voice, usage) | read |
| `get_call_assistant` | Full editable configuration of a call assistant (greeting, voice, routing, SMS/email, booking, opening hours, …) | read |
| `update_call_assistant` | Changes settings of a live call assistant | write |
| `list_whatsapp_assistants` | Lists the WhatsApp AI assistants | read |
| `get_whatsapp_assistant` | Full editable configuration of a WhatsApp AI assistant | read |
| `update_whatsapp_assistant` | Changes settings of a live WhatsApp assistant | write |
| `list_users` | Lists the team members a WhatsApp chat can be assigned to (use their id as assigned_user_id) | read |
| `list_whatsapp_accounts` | Lists the connected WhatsApp business numbers (inboxes) | read |
| `get_whatsapp_account` | One connected WhatsApp business number | read |
| `list_whatsapp_conversations` | Lists WhatsApp chats (oldest change first) with labels, customer, assignment and last message | read |
| `get_whatsapp_conversation` | One WhatsApp chat with labels, customer, assignment and last message | read |
| `update_whatsapp_conversation` | Close/reopen, snooze, assign (assigned_user_id from list_users; null hands the chat back to the AI) or set partner control | write |
| `list_whatsapp_messages` | Messages of a WhatsApp chat, oldest first | read |
| `send_whatsapp_message` | Sends a message into an existing WhatsApp chat on behalf of the business: text, reply buttons (1-3, titles max 20 chars), a list (1-10 rows) or a template | write |
| `start_whatsapp_conversation` | Sends an approved template to a phone number, creating the contact and chat if needed (new chats always need a template) | write |
| `list_whatsapp_notes` | Internal team notes of a WhatsApp chat (never sent to the customer) | read |
| `add_whatsapp_note` | Adds an internal team note to a WhatsApp chat | write |
| `list_whatsapp_labels` | All WhatsApp conversation labels of the account | read |
| `add_whatsapp_conversation_labels` | Adds labels to a chat and keeps the existing ones | write |
| `set_whatsapp_conversation_labels` | Replaces all labels of a chat (an empty list removes all) | write |
| `list_whatsapp_templates` | Message templates of a WhatsApp number with their live Meta review status | read |
| `get_whatsapp_template` | One template with its current Meta review status | read |
| `create_whatsapp_template` | Creates a template and submits it to Meta for review; it is sendable once status is `approved` | write |
| `get_webhook` | Whether an API webhook destination is configured, and its SHA-256 fingerprint (the URL itself is never returned) | read |
| `set_webhook` | Sets the API webhook URL (public HTTPS) | write |

Tools that send WhatsApp messages, start campaigns or change live assistants act on real customers; good clients ask you before running them (they carry MCP `readOnlyHint`/`destructiveHint` annotations).

## Notes

- Free-form WhatsApp messages only work within 24 h of the customer's last message; outside that window send an approved template.
- Call recording links are signed and valid for 60 minutes.
- Full REST API and webhook reference: <https://app.hallofelix.de/docs/external-api.md>

## Support

support@hallofelix.de · [hallofelix.de](https://www.hallofelix.de) · InsurMagic GmbH
