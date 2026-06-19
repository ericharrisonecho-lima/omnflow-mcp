
# Omnflow MCP Server

> Turn any content into platform-ready posts for Twitter/X, LinkedIn, Instagram, Email, and TikTok — plus Voice DNA analysis. Powered by [Omnflow](https://omnflow.com).

[![MCP](https://img.shields.io/badge/MCP-compatible-blue)](https://modelcontextprotocol.io)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## What is Omnflow?

Omnflow is an AI content repurposing tool for solopreneurs, coaches, and creators. Paste one blog post, video transcript, podcast, or idea and Omnflow rewrites it for X/Twitter, LinkedIn, Instagram, TikTok, YouTube Shorts, and email — in your voice.

The **Omnflow MCP server** brings these capabilities directly into Claude, Cursor, n8n, and any other MCP-compatible client.

## Connection

- **Server URL:** `https://omnflow.com/api/mcp`
- **Transport:** Streamable HTTP (POST)
- **Auth:** None — public endpoint
- **Name:** `omnflow`

## Tools

| Tool | Description | Parameters |
|------|-------------|------------|
| `repurpose_content` | Turn a source idea, blog post, or transcript into 5 platform-ready posts (Twitter thread, LinkedIn post, Instagram caption, email newsletter, TikTok script). | `source_text` (string, 40–8000 chars) |
| `analyze_voice` | Analyze a writing sample and return the author's Voice DNA: tone, sentence style, what they avoid, and their signature move. | `sample` (string, 100–2000 chars) |
| `list_recent_demos` | List the most recent public demo runs (source text + generated outputs + inferred niche). | `limit` (integer, 1–20, default 5) |

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

Restart Claude Desktop. The three tools will appear in the tools menu.

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
