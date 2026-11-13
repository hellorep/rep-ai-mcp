# REP AI MCP Server

Ask Claude about your Shopify store and get answers from real shopper conversations.

The REP AI MCP server connects your [REP AI](https://www.hellorep.ai) account to Claude and any AI assistant that supports remote [MCP (Model Context Protocol)](https://modelcontextprotocol.io) servers. Ask how REP is performing, what shoppers ask about and where they get stuck, in plain language. Spot a wrong answer? Ask Claude to fix it. Nothing goes live until you approve it.

| | |
|---|---|
| **Server address** | `https://mcp.hellorep.ai/mcp/v1` |
| **Transport** | Streamable HTTP |
| **Sign-in** | OAuth 2.1 with PKCE, approved in the REP AI Console |
| **Who can connect** | A store admin in REP AI |
| **Default access** | Read only. Change permissions stay off until you turn them on |
| **Tools** | 13: 6 read store data, 6 read or suggest changes, 1 applies an approved change |
| **Listed in** | [Claude Connectors Directory](https://claude.ai/directory/rep-ai) |
| **Manage connections** | [REP AI Console](https://app.hellorep.ai/api) > API > Connections (MCP) |

> This is a hosted, remote MCP server operated by REP AI. There is nothing to install or run locally. This repository holds the public documentation, client configuration examples and the place to report issues.

## Contents

- [Connect](#connect)
- [What you can ask](#what-you-can-ask)
- [Fix a wrong answer](#fix-a-wrong-answer)
- [Permissions](#permissions)
- [Tools](#tools)
- [Manage or disconnect](#manage-or-disconnect)
- [Troubleshooting](#troubleshooting)
- [Security and privacy](#security-and-privacy)
- [Support](#support)

## Connect

**Before you start:** you need a REP AI account and the admin role on your store. On a Claude Team or Enterprise plan, your organization owner may need to allow the REP AI connector first.

### Claude (web, desktop and mobile)

1. In Claude, open **Settings > Connectors**.
2. Search the directory for **REP AI** and click **Connect**.
   Or click **Add custom connector**, name it `REP AI`, paste `https://mcp.hellorep.ai/mcp/v1`, click **Add**, then **Connect**.
3. The REP AI sign-in page opens. Sign in to the REP AI Console and, if you manage more than one store, pick the store.
4. Choose what Claude can see and change. Leave the change permissions off if Claude should only read.
5. Click **Connect**, then ask your first question in any Claude chat.

Connectors added on claude.ai also work in the Claude desktop and mobile apps.

### Claude Code

```bash
claude mcp add --transport http rep-ai https://mcp.hellorep.ai/mcp/v1
```

Then run `/mcp` in Claude Code, select `rep-ai` and choose to authenticate. Sign in, choose permissions and click **Connect**.

To share the server with everyone working in a project, commit a `.mcp.json` file like the one in [`.mcp.json`](.mcp.json) to the project root.

### Other AI assistants

Any assistant that supports remote MCP servers with OAuth sign-in works the same way: add `https://mcp.hellorep.ai/mcp/v1` as a server and sign in to the REP AI Console when asked. Configuration examples are in [`examples/`](examples).

## What you can ask

Claude reads your REP AI data and answers in the chat. It can handle several questions in one request.

| Ask about | What it covers | Example question | Permission it needs |
|---|---|---|---|
| Performance | Conversations, engagement, AI-generated sales, conversion and orders, compared with the previous period | "How did REP AI perform last month compared with the month before?" | How Rep AI is performing |
| Conversation topics | What shoppers talk about, and how often REP helped | "What did shoppers ask about most in the last 30 days?" | How Rep AI is performing |
| Customer problems | Where shoppers get stuck, like high shipping costs or missing product information | "Where do shoppers get stuck on my store?" | How Rep AI is performing |
| Support workload | How many support emails REP handled and how they were resolved | "How many support emails did REP AI resolve this week?" | How much support email Rep handled |
| Specific conversations | Filtered by date, topic, problem, shopper emotion, satisfaction or channel | "Show me conversations from frustrated shoppers last week." | What shoppers talked about, and how it went |
| One shopper's history | Every conversation with one shopper, found by email address | "What happened with alex@example.com?" | What shoppers talked about, and how it went + Who the shopper was |
| Fix a wrong answer | Add or edit a global AI instruction, a custom FAQ or a skill's instructions | "Shoppers keep asking about free shipping and the AI answers wrong. Fix it." | Global AI instructions, Custom FAQs, or Sales & Support skill instructions |

**Questions to start with**

- How did REP AI perform last month compared with the month before?
- What did shoppers ask about most in the last 30 days?
- Where do shoppers get stuck on my store?
- How many support emails did REP AI resolve this week?
- Show me low-satisfaction conversations from Instagram this week.
- Find the top three customer problems this month and show me two conversations for each. Then fix the answer for the most common one.

## Fix a wrong answer

The server can suggest changes to three things: your global AI instructions, your custom FAQs and the instructions of your Sales and Support skills. Every change works the same way:

1. Ask Claude to fix something, like a wrong answer about shipping.
2. Claude reads what's there now and suggests the smallest change that fixes it.
3. You see the text before and after the change.
4. Say yes to apply it. Say no, or ask for something else, and nothing changes.
5. The change goes live on the next shopper message. It's saved in your Audit Log under your name, with the assistant's name, and you can revert it in one click.

Each suggestion is single use. It expires after a few minutes, and REP refuses it if someone edited the same item in the Console first. Ask Claude to suggest it again.

**What an assistant can never change.** An AI assistant connected to REP AI can never delete anything, turn a skill on or off, or change triggers, targeting, chips, product links, refund or other action permissions, API endpoints, widget design, billing or integrations. You make those changes in the REP AI Console.

## Permissions

You choose on the REP AI consent screen, and an admin can change it later without reconnecting. REP sends data only when the assistant asks for it to answer your question, and only within these permissions.

**What the assistant can see**

| Permission | Scope | What the assistant can see |
|---|---|---|
| How Rep AI is performing | `analytics.read` | Store-wide performance figures, plus topics and customer problems summarized across all conversations. On by default. |
| What shoppers talked about, and how it went | `tickets.read` | For each conversation: topics, the shopper's problem, emotion, satisfaction, timings, tags and whether an order followed. Not the messages themselves. On by default. |
| The actual messages | `messages.read` | The messages between shoppers and REP. Shown only when the separate "Let the assistant read what shoppers wrote" switch is also on. |
| Who the shopper was | `customers.read` | The name, email address and phone number of shoppers in your conversations. |
| How much support email Rep handled | `support.read` | How many support tickets REP handled and how they were resolved. |

**What the assistant can change (off until you turn it on)**

| Permission | Scope | What the assistant can do |
|---|---|---|
| Global AI instructions | `instructions.write` | Read, add and edit global AI instructions. Can't remove them. |
| Custom FAQs | `faqs.write` | Read, add and edit custom FAQ questions and answers. Can't delete them. |
| Sales & Support skill instructions | `skills.write` | Read and edit the instructions of existing Sales and Support skills. Can't create, delete, turn on or turn off a skill, or change refund and other permissions. |

## Tools

Read tools are marked read-only. Change tools only suggest a change and return the before and after. `apply_approved_change` applies it after the merchant approves it in the conversation.

| Tool | What it answers | Scope |
|---|---|---|
| `get_rep_performance` | Performance figures for a period, compared with the previous period | `analytics.read` |
| `get_conversation_topics` | What shoppers talk about, with counts | `analytics.read` |
| `get_customer_problems` | Where shoppers get stuck, with counts | `analytics.read` |
| `get_support_workload` | Support emails REP handled and how they were resolved | `support.read` |
| `search_conversations` | Conversations filtered by date, topic, problem, emotion, satisfaction or channel | `tickets.read` (message text also needs `messages.read`, shopper details `customers.read`) |
| `get_shopper_conversations` | Every conversation with one shopper, by email address | `tickets.read` + `customers.read` (message text also needs `messages.read`) |
| `get_ai_instructions` | Reads the global AI instructions | `instructions.write` |
| `propose_instruction_change` | Suggests adding or rewording one global AI instruction | `instructions.write` |
| `find_faqs` | Searches the custom FAQs | `faqs.write` |
| `propose_faq_change` | Suggests a new FAQ, or a new question or answer for an existing one | `faqs.write` |
| `get_skill_instructions` | Reads the instructions of the Sales and Support skills | `skills.write` |
| `propose_skill_change` | Suggests new instruction text for one existing skill | `skills.write` |
| `apply_approved_change` | Applies one suggestion the merchant approved. Single use, expires after a few minutes, refused if the item changed since | The scope of the suggestion's object type |

## Manage or disconnect

In the [REP AI Console](https://app.hellorep.ai/api), open the account menu, then **API > Connections (MCP)**. Each connection shows what it can see and change, who approved it and when it was last used.

- **Edit** changes permissions or the message-text switch. You don't need to reconnect.
- **Revoke** cuts access right away. A revoked connection can't be turned back on; to connect again, start from the assistant and approve a new request.
- **Settings > Audit Log** lists every change an assistant made. Click **Revert** to undo one.

Every member of the store can see the list of connections. Only an admin can edit or revoke them. You can also remove REP AI in Claude under **Settings > Connectors**.

## Troubleshooting

| Message or symptom | What to do |
|---|---|
| "Only an account admin can connect an AI assistant to this store." | Ask a store admin to connect, or to give you the admin role. |
| "AI assistant connections are not on this store's plan." | Email support@hellorep.ai and we'll help you turn them on. |
| "This connection request is no longer valid." | The request expired or was already answered. Start the connection again from the assistant. |
| No data for a period | REP AI reports from the day your store started using REP AI. Topics and customer problems update once a day. Conversation search goes back one year; a lookup of one shopper by email has no date limit. |
| Support email figures are empty | Support workload covers REP AI's AI email answering. If it isn't connected for your store, there is nothing to report. |
| The assistant can't answer a question | The connection is missing the permission that question needs. Click **Edit** on the connection in the Console, add it and ask again. |
| The assistant says it can't change your AI | Tick the matching change permission with **Edit**. If changes through assistants are switched off for your store, contact us. |
| A proposal expired or the item changed | Ask the assistant to propose the change again. |
| Numbers differ slightly from the Console | The MCP server counts whole days in UTC. The Console may show your local time. |

## Security and privacy

- Shopper messages can't give orders. REP treats anything a shopper writes as information to report on, never as an instruction to the assistant or as your approval.
- Every change is saved in your Audit Log under your name and can be reverted in one click.
- Tokens can be revoked by the merchant at any time in the REP AI Console.
- REP AI is SOC 2 Type II compliant.
- Read the [REP AI privacy policy](https://www.hellorep.ai/legal/privacy-policy).

To report a vulnerability, see [SECURITY.md](SECURITY.md).

**Is REP AI the same company as rep.ai?** No. REP AI's website is hellorep.ai and its MCP server runs at mcp.hellorep.ai. REP AI does not operate the rep.ai domain. To connect the right server, use the REP AI listing in the Claude Connectors Directory or the address `https://mcp.hellorep.ai/mcp/v1`.

## Support

- Product page: https://www.hellorep.ai/mcp
- Setup guide: [Connect Rep AI to Claude](https://help.hellorep.ai/hc/en-us/articles/39602299418130-Ask-Claude-About-Your-Store-Connect-Rep-AI-to-Claude)
- Email: support@hellorep.ai
- Documentation issues or client configuration problems: [open an issue](../../issues)

New to REP AI? [Start a free trial or book a demo](https://www.hellorep.ai/book-a-demo).

---

© Rep AI Technologies Inc. The documentation and examples in this repository are released under the [MIT License](LICENSE). The REP AI service is provided under REP AI's terms of service.
