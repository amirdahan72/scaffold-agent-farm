---
name: teams-messaging
description: 'Send and read Microsoft Teams chat messages. Use when asked to "send a Teams message", "message someone on Teams", "notify on Teams", "read Teams messages", "search Teams chats", "post to Teams", or when a farm workflow needs to notify stakeholders or read chat context. Triggers: Teams, chat, message, notify, send message.'
---

# Teams Messaging Skill

Send and read Microsoft Teams chat messages using the `microsoft-teams` MCP server. Designed for agent farm workflows where sub-agents need to notify stakeholders or gather context from Teams conversations.

## Prerequisites

- **Microsoft Teams MCP** configured in `.vscode/mcp.json` (the `microsoft-teams` server)
- **WorkIQ MCP** for person/UPN lookups

## Known Limitations

- **`ListChats` is unreliable** — it consistently returns 504 Gateway Timeout on the agent365 server. **Never use `ListChats`.**
- Use `CreateChat` instead — it's idempotent and returns the existing chat if one already exists with that person.

## Workflow

### Sending a Message to a Person

**Phase 1 — Resolve UPN**

Use WorkIQ to find the person's email/UPN:

```
workiq-ask_work_iq: "What is <person name>'s email/UPN?"
```

**Phase 2 — Get Chat ID**

Use `CreateChat` to get (or create) the 1:1 chat. This is idempotent — if a chat already exists, it returns the existing chat ID:

```
microsoft-teams-CreateChat:
  chatType: "oneOnOne"
  members_upns: ["person@contoso.com"]
```

Returns: `{ "id": "19:...", "chatType": "OneOnOne" }`

**Phase 3 — Send Message**

```
microsoft-teams-PostMessage:
  chatId: "<chat-id-from-phase-2>"
  content: "Your message here"
```

For formatted messages, use `contentType: "html"`:

```
microsoft-teams-PostMessage:
  chatId: "<chat-id>"
  content: "<b>Status Update</b><br><ul><li>Item 1 — done</li><li>Item 2 — in progress</li></ul>"
  contentType: "html"
```

### Reading Messages from a Chat

If you have a chat ID (from `CreateChat` or a previous operation):

```
microsoft-teams-ListChatMessages:
  chatId: "<chat-id>"
```

### Searching Across All Teams Conversations

For broad context gathering (e.g., "what has been discussed about topic X"):

```
microsoft-teams-SearchTeamsMessages:
  message: "topic or question in natural language"
```

This uses Microsoft 365 Copilot to search across all chats, channels, and teams. Returns a summarized response with citations and chat IDs.

### Posting to a Channel

If you have the team ID and channel ID:

```
microsoft-teams-PostChannelMessage:
  teamId: "<team-guid>"
  channelId: "<channel-id>"
  content: "Message content"
```

## HTML Formatting Reference

When sending formatted messages via `PostMessage` with `contentType: "html"`:

| Markdown | HTML |
|----------|------|
| `**bold**` | `<b>bold</b>` |
| `*italic*` | `<i>italic</i>` |
| `[text](url)` | `<a href="url">text</a>` |
| `- item` | `<ul><li>item</li></ul>` |
| `1. item` | `<ol><li>item</li></ol>` |
| `## Heading` | `<h2>Heading</h2>` |
| newline | `<br>` |

## Tool Reference

| Action | MCP Tool | Required Params |
|--------|----------|-----------------|
| Find person's UPN | `workiq-ask_work_iq` | question |
| Get/create 1:1 chat | `microsoft-teams-CreateChat` | chatType, members_upns |
| Send message | `microsoft-teams-PostMessage` | chatId, content |
| Send to channel | `microsoft-teams-PostChannelMessage` | teamId, channelId, content |
| Read chat messages | `microsoft-teams-ListChatMessages` | chatId |
| Search all messages | `microsoft-teams-SearchTeamsMessages` | message |
| Reply to channel msg | `microsoft-teams-ReplyToChannelMessage` | teamId, channelId, messageId, content |

## Anti-Patterns

- **NEVER use `ListChats`** — it 504s on the agent365 server. Use `CreateChat` to resolve chat IDs.
- **NEVER guess UPNs** — always resolve via WorkIQ first.
- **NEVER send without confirming the recipient** — always verify the UPN belongs to the intended person.
