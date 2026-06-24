# Omnflow MCP Server

> Turn any content into platform-ready posts for Twitter/X, LinkedIn, Instagram, Email, and TikTok — save them to your library, and schedule them on your content calendar. Powered by [Omnflow](https://omnflow.com).

[![MCP](https://img.shields.io/badge/MCP-compatible-blue)](https://modelcontextprotocol.io)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## What is Omnflow?

Omnflow is an AI content repurposing tool for solopreneurs, coaches, and creators. Paste one blog post, video transcript, podcast, or idea and Omnflow rewrites it for X/Twitter, LinkedIn, Instagram, TikTok, YouTube Shorts, and email — in your voice.

The **Omnflow MCP server** brings these capabilities directly into Claude, Cursor, n8n, and any other MCP-compatible client — including saving generated posts to your Omnflow library and scheduling them on your content calendar.

## Connection

- **Server URL:** `https://omnflow.com/api/mcp`
- **Transport:** Streamable HTTP (POST)
- **Auth:** OAuth 2.1 (PKCE) — sign in with your Omnflow account on connect
- **Name:** `omnflow`

## Tools

| Tool | Description | Parameters |
|------|-------------|------------|
| `repurpose_content` | Turn a source idea, blog post, or transcript into 5 platform-ready posts (Twitter thread, LinkedIn post, Instagram caption, email newsletter, TikTok script). | `source_text` (string, 40–8000 chars) |
| `analyze_voice` | Analyze a writing sample and return the author's Voice DNA: tone, sentence style, what they avoid, and their signature move. | `sample` (string, 100–2000 chars) |
| `list_recent_demos` | List the most recent public demo runs (source text + generated outputs + inferred niche). | `limit` (integer, 1–20, default 5) |
| `save_to_library` | Save a piece of content (post, caption, thread, script) directly into your Omnflow library so it shows up on the Library page. | `content` (string), `platform` (`twitter` \| `linkedin` \| `instagram` \| `tiktok` \| `email`), `title?` (string), `tags?` (string[]) |
| `add_to_calendar` | Schedule a post on your Omnflow content calendar at a specific time. | `content` (string), `platform` (same enum), `scheduled_at` (ISO 8601 datetime), `title?` (string) |

## Installation

### Claude Desktop

Edit `claude_desktop_config.json`:

- **macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Windows:** `%APPDATA%\Claude\claude_desktop_config.json`

```json
{
  "mcpServers": {
    "omnflow": {
      "url": "https://omnflow.com/api/mcp"
    }
  }
}
```

Restart Claude Desktop. On first tool call, a browser window opens for you to sign in to Omnflow — after that the five tools are available in the tools menu.

### Cursor

In Cursor Settings → MCP → Add new MCP server:

```json
{
  "mcpServers": {
    "omnflow": {
      "url": "https://omnflow.com/api/mcp"
    }
  }
}
```

Cursor will trigger the same OAuth sign-in flow on first use.

### Claude Code CLI

```bash
claude mcp add --transport http omnflow https://omnflow.com/api/mcp
```

### n8n

Use the **MCP Client** node (or HTTP Request node):

- **Endpoint:** `https://omnflow.com/api/mcp`
- **Method:** POST
- **Headers:**
  - `Content-Type: application/json`
  - `Accept: application/json, text/event-stream`
  - `Authorization: Bearer <access_token>` (obtain via the OAuth flow described below)

### Claude.ai (web) — Custom Connector

In **Claude.ai → Settings → Connectors → Add custom connector**, paste:

```
https://omnflow.com/api/mcp
```

Claude discovers OAuth via `/.well-known/oauth-protected-resource`, opens a browser
window to `https://omnflow.com/auth`, asks you to sign in or create an Omnflow account,
and then stores the access token. After that all five tools are available in any chat.

## Authentication

This server uses the **OAuth 2.1 Authorization Code flow with PKCE**. You don't need
an API key — when you connect Omnflow, your MCP client opens a browser window so you
can sign in to your Omnflow account. **All five tools require authentication.**
Unauthenticated calls return `401 Unauthorized` with a `WWW-Authenticate` header
pointing at the discovery document so compliant clients can re-trigger the sign-in flow.

Discovery endpoints:

- `https://omnflow.com/.well-known/oauth-authorization-server`
- `https://omnflow.com/.well-known/oauth-protected-resource`

Endpoints used by the flow:

- **Authorization:** `https://omnflow.com/auth`
- **Token:** `https://omnflow.com/api/auth/token`
- **Scopes:** `read`, `write` (granted together — both are required for save / schedule tools)

## Usage Examples

### Repurpose a blog post

```
Use omnflow's repurpose_content tool on this blog post:

"The biggest mistake I see founders make is shipping features
nobody asked for. Here's the 3-step process I use instead..."
```

Returns: a Twitter thread, LinkedIn post, Instagram caption, email newsletter, and TikTok script.

### Save a generated post to your library

```
Use omnflow's save_to_library to save this LinkedIn post to my library
with the tag "founder-lessons":

"The biggest mistake I see founders make is..."
```

The post appears on your Omnflow Library page immediately.

### Schedule a post on the calendar

```
Use omnflow's add_to_calendar to schedule this tweet for
2026-07-01T09:00:00Z:

"Ship the boring thing. The exciting thing can wait."
```

The post shows up on your Omnflow Calendar at the scheduled time.

### Analyze writing voice

```
Use omnflow's analyze_voice tool on this sample:

"[paste 100-2000 characters of your writing]"
```

Returns Voice DNA: `tone`, `sentence_style`, `avoids`, `signature_move`.

### Inspect recent demo runs

```
Use omnflow's list_recent_demos to show the last 10 demo runs.
```

## Links

- Website: [omnflow.com](https://omnflow.com)
- Pricing: [omnflow.com/pricing](https://omnflow.com/pricing)
- Content repurposing tool: [omnflow.com/content-repurposing-tool](https://omnflow.com/content-repurposing-tool)
- MCP spec: [modelcontextprotocol.io](https://modelcontextprotocol.io)

## License

MIT
In Cursor Settings → MCP → Add new MCP server:

```json
{
  "mcpServers": {
    "omnflow": {
      "url": "https://omnflow.com/api/mcp"
    }
  }
}
```

### Claude Code CLI

```bash
claude mcp add --transport http omnflow https://omnflow.com/api/mcp
```

### n8n

Use the **MCP Client** node (or HTTP Request node):

- **Endpoint:** `https://omnflow.com/api/mcp`
- **Method:** POST
- **Headers:**
  - `Content-Type: application/json`
  - `Accept: application/json, text/event-stream`

### Claude.ai (web)

The web "Custom Connectors" UI requires OAuth discovery, which this server does not yet expose. Use Claude Desktop instead.

## Usage Examples

### Repurpose a blog post

```
Use omnflow's repurpose_content tool on this blog post:

"The biggest mistake I see founders make is shipping features
nobody asked for. Here's the 3-step process I use instead..."
```

Returns: a Twitter thread, LinkedIn post, Instagram caption, email newsletter, and TikTok script.

### Analyze writing voice

```
Use omnflow's analyze_voice tool on this sample:

"[paste 100-2000 characters of your writing]"
```

Returns Voice DNA: `tone`, `sentence_style`, `avoids`, `signature_move`.

### Inspect recent demo runs

```
Use omnflow's list_recent_demos to show the last 10 demo runs.
```

## Links

- Website: [omnflow.com](https://omnflow.com)
- Pricing: [omnflow.com/pricing](https://omnflow.com/pricing)
- Content repurposing tool: [omnflow.com/content-repurposing-tool](https://omnflow.com/content-repurposing-tool)
- MCP spec: [modelcontextprotocol.io](https://modelcontextprotocol.io)

## License

MIT
