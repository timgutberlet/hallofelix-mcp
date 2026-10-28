# HalloFelix von InsurMagic – MCP Server

Official [Model Context Protocol](https://modelcontextprotocol.io) server for [HalloFelix von InsurMagic](https://www.hallofelix.de), the AI phone and WhatsApp assistant for insurance brokers in Germany, Austria and Switzerland.

Connect Claude, ChatGPT, Cursor, VS Code or any MCP client to your HalloFelix account and let your AI agent work with everything the HalloFelix web app offers: calls, e-mails and their tasks, customers (notes, phone numbers, customer memory), outbound call campaigns, voice, e-mail and WhatsApp assistants, Vorgänge (task types), calendars and appointment types, company settings, knowledge documents, statistics and the whole WhatsApp inbox.

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
| `list_calls` | Lists phone calls handled by the HalloFelix voice AI (inbound and outbound) with transcript, summary, extracted data points and task status | read |
| `get_call` | Full call record: transcript, summary, summary data points, customer, campaign and assistant | read |
| `update_call` | Rates a call and sets its task: status, assignee (team member user id) and task type | write, destructive |
| `bulk_update_call_status` | Sets the task status of up to 1000 calls at once | write, destructive |
| `delete_call` | Permanently deletes a call with its recording, transcript, task and notes | write, destructive |
| `start_outbound_call` | Starts a real phone call: the AI call assistant phones the customer's primary number now (or at scheduled_at) | write |
| `list_call_notes` | Internal notes on a call's task, newest first | read |
| `add_call_note` | Adds an internal note to a call's task | write |
| `list_emails` | Lists e-mails handled by the HalloFelix e-mail assistant, newest first, with summary, customer and task status | read |
| `get_email` | Full e-mail record: sender, recipients, subject, body, summary, customer and task | read |
| `update_email` | Rates an e-mail and sets its task: status, assignee (team member user id) and task type | write, destructive |
| `list_email_notes` | Internal notes on an e-mail's task, newest first | read |
| `add_email_note` | Adds an internal note to an e-mail's task | write |
| `update_note` | Changes the text of a call, e-mail or customer note | write, destructive |
| `delete_note` | Permanently deletes a call, e-mail or customer note | write, destructive |
| `list_customers` | Lists customers (contacts the AI calls or chats with) with phone numbers, call count and last call time | read |
| `get_customer` | Full customer record | read |
| `create_customer` | Creates a customer | write |
| `update_customer` | Edits a customer: names, e-mail, language, external id, call handling and WhatsApp AI exclusion | write, destructive |
| `delete_customer` | Permanently deletes a customer with phone numbers, notes, appointments and memory | write, destructive |
| `add_customer_phone_number` | Adds a phone number to a customer | write |
| `update_customer_phone_number` | Makes a customer's phone number the primary one (is_primary: true) and/or sets its type | write, destructive |
| `delete_customer_phone_number` | Removes a phone number from a customer (not the customer's last one) | write, destructive |
| `list_customer_notes` | Internal notes on a customer, newest first | read |
| `add_customer_note` | Adds an internal note to a customer | write |
| `get_customer_memory` | What the AI remembers about a customer (vehicles, address, preferences, contact windows, notes), with origin and status of each fact | read |
| `add_customer_memory_fact` | Adds a fact the AI should remember about a customer | write |
| `update_customer_memory_fact` | Corrects a fact (value) or rejects it (status "rejected", also hides a CRM fact) | write, destructive |
| `delete_customer_memory_fact` | Permanently deletes a memory fact (CRM facts cannot be deleted, only rejected) | write, destructive |
| `list_campaigns` | Lists outbound call campaigns with success and answer rates | read |
| `get_campaign` | Full campaign record with computed statistics | read |
| `create_campaign` | Creates an outbound call campaign | write |
| `start_campaign` | Starts or resumes a campaign: the AI begins calling its customers | write |
| `set_campaign_status` | Changes a campaign run status | write, destructive |
| `update_campaign` | Edits a draft campaign (name, assistant, first sentence, goal, attempts, retry interval, customers, schedule, business hours; customer_ids replaces the customer list) and/or archives or unarchives any campaign | write, destructive |
| `delete_campaign` | Permanently deletes a campaign (its calls stay, without the campaign link) | write, destructive |
| `list_call_assistants` | Lists the voice AI phone assistants (id, name, phone number, voice, usage) | read |
| `get_call_assistant` | Full editable configuration of a call assistant (greeting, voice, routing, SMS/email, booking, opening hours, …) | read |
| `update_call_assistant` | Changes settings of a live call assistant | write, destructive |
| `list_whatsapp_assistants` | Lists the WhatsApp AI assistants | read |
| `get_whatsapp_assistant` | Full editable configuration of a WhatsApp AI assistant | read |
| `update_whatsapp_assistant` | Changes settings of a live WhatsApp assistant | write, destructive |
| `create_call_assistant` | Creates a new call assistant | write |
| `list_email_assistants` | Lists the e-mail AI assistants (id, name, usage) | read |
| `get_email_assistant` | Name, instructions and Vorgänge (task_type_ids) of an e-mail assistant | read |
| `create_email_assistant` | Creates an e-mail assistant with the account default Vorgänge | write |
| `update_email_assistant` | Changes an e-mail assistant | write, destructive |
| `create_whatsapp_assistant` | Creates a WhatsApp assistant (counts against the assistant limit) | write |
| `delete_whatsapp_assistant` | Permanently deletes a WhatsApp assistant with its macros, documents and rules; numbers using it get no AI replies until another assistant is set | write, destructive |
| `list_whatsapp_macro_runs` | Macro runs of a WhatsApp assistant that stopped and need a person: state unknown (the document may or may not have arrived) or failed_terminal (it did not) | read |
| `resolve_whatsapp_macro_run` | Closes a macro run by hand (state resolved_manually); nothing is sent | write |
| `retry_whatsapp_macro_run` | Sends the macro run document to the customer again over WhatsApp | write |
| `test_whatsapp_assistant` | Sends a test message to a WhatsApp assistant and returns its simulated reply (or the macro it would fire) | write |
| `list_users` | Lists the team members a WhatsApp chat can be assigned to (their id is the assigned_user_id of a chat) | read |
| `list_whatsapp_accounts` | Lists the connected WhatsApp business numbers (inboxes) with their inbox settings | read |
| `get_whatsapp_account` | One connected WhatsApp business number with its inbox settings (AI, read receipts, automated messages, opening hours, AI schedule, assistant) | read |
| `list_whatsapp_conversations` | Lists WhatsApp chats with labels, customer, assignment and last message | read |
| `get_whatsapp_conversation` | One WhatsApp chat with labels, customer, assignment and last message | read |
| `update_whatsapp_conversation` | Close/reopen, snooze, assign (assigned_user_id = team member user id; null hands the chat back to the AI), set partner control, pause the AI for the customer, mark read/unread, name the contact, link a customer or set the notes the AI reads about the contact | write, destructive |
| `list_whatsapp_messages` | Messages of a WhatsApp chat, oldest first | read |
| `send_whatsapp_message` | Sends a message into an existing WhatsApp chat on behalf of the business: text, reply buttons (1-3, titles max 20 chars), a list (1-10 rows) or a template | write |
| `start_whatsapp_conversation` | Sends an approved template to a phone number, creating the contact and chat if needed (new chats always need a template) | write |
| `list_whatsapp_notes` | Internal team notes of a WhatsApp chat (never sent to the customer) | read |
| `add_whatsapp_note` | Adds an internal team note to a WhatsApp chat | write |
| `list_whatsapp_labels` | All WhatsApp conversation labels of the account | read |
| `add_whatsapp_conversation_labels` | Adds labels to a chat and keeps the existing ones | write |
| `set_whatsapp_conversation_labels` | Replaces all labels of a chat (an empty list removes all) | write, destructive |
| `list_whatsapp_templates` | Message templates of a WhatsApp number with their live Meta review status | read |
| `get_whatsapp_template` | One template with its current Meta review status | read |
| `create_whatsapp_template` | Creates a template and submits it to Meta for review; it is sendable once status is `approved` | write |
| `list_task_types` | Lists the task types ("Vorgänge") the assistants recognise and file: own task types and HalloFelix base templates, with name, description, instruction, emoji, active flag and whether they are editable (base templates must be detached first) | read |
| `get_task_type` | One task type (Vorgang) with its instruction and flags | read |
| `create_task_type` | Creates an own task type (Vorgang) | write |
| `update_task_type` | Changes a task type: name, description, instruction, emoji (own or detached task types only) and/or active | write, destructive |
| `detach_task_type` | Turns an active HalloFelix base template into an own, editable copy ("<name> (Individuell)") that takes over its assistants; the base template is deactivated | write |
| `delete_task_type` | Deletes an own or detached task type and removes it from all assistants | write, destructive |
| `list_appointment_types` | Lists the appointment types (Termintypen) the call and WhatsApp assistants can book, with duration, buffers and the condition when to use them | read |
| `create_appointment_type` | Adds an appointment type to exactly one call or WhatsApp assistant | write |
| `update_appointment_type` | Changes an appointment type | write, destructive |
| `delete_appointment_type` | Deletes an appointment type; the assistant can no longer book it | write, destructive |
| `list_calendars` | Lists the connected calendars (Google, Microsoft, iCloud, …) the assistants check and book into | read |
| `get_calendar` | One connected calendar with the appointments HalloFelix booked into it | read |
| `update_calendar` | Renames a connected calendar in HalloFelix | write, destructive |
| `delete_calendar` | Disconnects a calendar: assistants stop checking and booking it, and the provider connection is revoked when no other calendar uses it (reconnecting needs the dashboard) | write, destructive |
| `list_calendar_events` | Busy times of a connected calendar between from and to (ISO-8601, at most 92 days, max | read |
| `book_calendar_appointment` | Books an appointment into the real connected calendar of the business (fails with 409 slot_taken if the slot is busy) | write |
| `get_account` | Company profile and locations the assistants use, recording settings (save_audio_recording, recording_retention_days) and the customer memory switch | read |
| `update_account` | Changes company settings | write, destructive |
| `get_crm_ai_access` | Whether the WhatsApp AI may look up customer data in the connected CRM, which parts (contracts, damages, Vorgänge, master data, health lines) and the CRM connections | read |
| `update_crm_ai_access` | Changes the CRM data access of the AI. Send only the fields to change | write, destructive |
| `list_documents` | Lists the knowledge documents (PDF/DOCX) the assistants answer from, with their status (processing, done, failed) | read |
| `delete_document` | Deletes a knowledge document; the assistants no longer answer from it | write, destructive |
| `get_insights` | Dashboard KPIs for a date range (default: last 30 days) compared with the period before: calls, call length and minutes, e-mails, WhatsApp messages (all / AI), a daily chart, open items, company figures and the Vorgang distribution | read |
| `get_webhook` | Whether an API webhook destination is configured, and its SHA-256 fingerprint (the URL itself is never returned) | read |
| `set_webhook` | Sets the API webhook URL (public HTTPS) | write, destructive |
| `update_whatsapp_account` | Changes the inbox settings of a WhatsApp number; only sent fields change | write, destructive |
| `list_whatsapp_template_folders` | Template folders of a WhatsApp number, each with its templates | read |
| `create_whatsapp_template_folder` | Creates a template folder (exists only in HalloFelix, not at Meta) | write |
| `rename_whatsapp_template_folder` | Renames a template folder | write, destructive |
| `delete_whatsapp_template_folder` | Deletes a template folder; its templates move back to the default folder (the templates themselves stay) | write, destructive |
| `move_whatsapp_template_to_folder` | Files a template in a folder of the same WhatsApp number | write, destructive |
| `list_whatsapp_ai_exclusions` | Customers the WhatsApp AI never answers; their chats go to the team | read |
| `set_whatsapp_ai_exclusion` | excluded=true: the AI stops answering this customer in all chats and the team takes them over; false: their chats go back to the AI | write, destructive |
| `open_whatsapp_conversation` | Finds or creates the chat with a phone number on a WhatsApp number without sending anything | write |
| `delete_whatsapp_conversation` | Permanently deletes a chat with all its messages, notes and files for the whole team | write, destructive |
| `search_whatsapp_messages` | Searches the messages of all WhatsApp chats (archived too), newest first: text, captions and voice transcriptions | read |
| `react_to_whatsapp_message` | Reacts to a customer message with an emoji the customer sees at once (emoji null removes the reaction) | write |
| `retry_whatsapp_message` | Sends a failed outbound text or template again (within 24h of the failure) as a new message to the customer | write |
| `list_scheduled_whatsapp_messages` | Templates scheduled to be sent into a chat that have not gone out yet, soonest first | read |
| `schedule_whatsapp_template` | Schedules an approved template to be sent into a chat at a future time | write |
| `cancel_scheduled_whatsapp_message` | Cancels a scheduled template before it is sent | write, destructive |
| `send_whatsapp_media` | Sends an image, video, document or audio from a public URL into a chat (within the 24h window, WhatsApp size limits apply) | write |
| `list_whatsapp_media` | Files of a chat (both directions), newest first, 50 per page | read |
| `create_whatsapp_label` | Creates a conversation label (at most 20 per account) | write |
| `update_whatsapp_label` | Renames or recolors a label | write, destructive |
| `delete_whatsapp_label` | Deletes a label; it disappears from every chat that had it | write, destructive |
| `remove_whatsapp_conversation_label` | Takes one label off a chat and keeps the others | write, destructive |

## Notes

- Free-form WhatsApp messages only work within 24 h of the customer's last message; outside that window send an approved template.
- Call recording links are signed and valid for 60 minutes.
- Full REST API and webhook reference: <https://app.hallofelix.de/docs/external-api.md>

## Support

support@hallofelix.de · [hallofelix.de](https://www.hallofelix.de) · InsurMagic GmbH
