# Savee MCP Server

> Your saved visual inspiration, in the conversation.

[Savee](https://savee.com) is a curated visual-inspiration platform for designers
and creative professionals. Members save images and video from anywhere on the
web into boards, and discover work through color, tags, and visual similarity.

Savee runs a **hosted remote MCP server** at `https://mcp.savee.com/mcp`. There
is nothing to clone, install, or run locally — this repository holds the
plugin manifest, the registry manifest, and setup docs. The server itself is
closed-source and operated by Savee.

## Setup

Add the server to your MCP client:

```json
{
	"mcpServers": {
		"savee": {
			"type": "streamableHttp",
			"url": "https://mcp.savee.com/mcp"
		}
	}
}
```

The transport is Streamable HTTP. Some clients — Cline among them — fall back to
the legacy SSE transport when `type` is omitted, which fails to connect; setting
`"type": "streamableHttp"` explicitly avoids that. Clients that only accept a URL
(Claude, VS Code) negotiate the transport themselves and need nothing extra.

### Claude Code

```sh
claude mcp add --transport http --scope user savee https://mcp.savee.com/mcp
```

Then run `/mcp`, select **savee**, and choose **Authenticate**.

### Cursor

**Settings → MCP → Add new MCP server**, pick the HTTP type, and paste
`https://mcp.savee.com/mcp`. Or drop the snippet above into `~/.cursor/mcp.json`.

### Claude

**Settings → Connectors → Add custom connector**, and paste
`https://mcp.savee.com/mcp`.

### Everything else

Per-client instructions live at
[docs.savee.com/mcp/install](https://docs.savee.com/mcp/install).

## Authorization

Authentication is OAuth 2.1 with PKCE — Client ID Metadata Documents preferred,
Dynamic Client Registration supported. On first connection your browser opens
savee.com, you sign in, and a consent screen lists exactly what you're granting.
There is no API key to copy anywhere in the flow.

You can disconnect at any time from **Settings → Developers** on savee.com,
which takes effect immediately.

## Tools

All seven tools are read-only and annotated `readOnlyHint`. No write tools
exist, so nothing here can save, edit, move, or delete anything in your account.

| Tool | What it does |
| --- | --- |
| `get_current_user` | The signed-in Savee profile |
| `list_saves` | Your own saves, newest first |
| `list_feed` | Your home feed |
| `list_boards` | The boards you own or belong to |
| `list_board_saves` | The saves on one board |
| `search` | Savee's public library — everything on the platform, not just your saves |
| `view_saves` | The actual images for specific saves, so the assistant can see them |

Every save comes back with its source URL, dimensions, and dominant colors, so
an assistant can reason about what's in a collection rather than just list
links. `view_saves` goes further and returns the images themselves, for
questions about composition, layout, type, or how a set of images sit together.

Full reference: [docs.savee.com/mcp](https://docs.savee.com/mcp).

## What it can see

Private saves and private boards are included — the connector sees what you see
when signed in. The Savee setting *"Hide private board saves from profile feed"*
applies here too. Scopes are granted individually on the consent screen, and a
tool whose scope you declined returns an error rather than data.

## Requirements

A Savee account with an active subscription.
[Check your plan](https://savee.com/upgrade/).

## Links

- Documentation — <https://docs.savee.com/mcp>
- Privacy policy — <https://savee.com/privacy/>
- Support — <hey@savee.com>

## License

The contents of this repository are MIT licensed. Use of the hosted service is
governed by Savee's [terms](https://savee.com/terms/).
