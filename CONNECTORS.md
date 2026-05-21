# Connectors

This plugin works in two modes:

- Standalone: Claude uses the bundled Zoom skills and reference material included with this plugin.
- Supercharged: Claude can also use the bundled Zoom MCP servers from [`.mcp.json`](./.mcp.json) for live tool access.

## Included MCP Servers

| Connector | Endpoint | Use For |
|---|---|---|
| `zoom-mcp` | `https://mcp.zoom.us/mcp/zoom/streamable` | Zoom-hosted MCP workflows for meeting search, cross-Zoom search, recordings, summaries, meeting assets, and main-server Zoom Docs tools |
| `zoom-docs-mcp` | `https://mcp.zoom.us/mcp/docs/streamable` | Zoom Docs creation, retrieval, and Markdown-based document workflows |
| `zoom-whiteboard-mcp` | `https://mcp.zoom.us/mcp/whiteboard/streamable` | Whiteboard-specific MCP workflows |

## Optional MCP Surfaces Not Bundled by Default

| Surface | Endpoint | Notes |
|---|---|---|
| Team Chat MCP | `https://mcp.zoom.us/mcp/team_chat/streamable` | Write-capable Team Chat MCP tools are documented in [`skills/zoom-mcp/team-chat/`](./skills/zoom-mcp/team-chat/), but this server is not registered in [`.mcp.json`](./.mcp.json) by default. |

## Authentication

The bundled MCP definitions expect bearer tokens in these environment variables:

```bash
export ZOOM_MCP_ACCESS_TOKEN="your_zoom_user_oauth_access_token"
export ZOOM_DOCS_MCP_ACCESS_TOKEN="your_zoom_docs_mcp_access_token"
export ZOOM_WHITEBOARD_MCP_ACCESS_TOKEN="your_zoom_user_oauth_access_token"
```

- **Claude Cowork**: use the published Zoom connector in Claude's connector directory and complete OAuth in the connector flow.
- **Claude Code**: do not rely on the built-in `Authenticate` button for `zoom-mcp`; complete Zoom user-level OAuth yourself, export the token, reconnect the plugin, then use [`/setup-zoom-mcp`](./skills/setup-zoom-mcp/SKILL.md).
- `ZOOM_MCP_ACCESS_TOKEN` is used for the main Zoom MCP server.
- `ZOOM_DOCS_MCP_ACCESS_TOKEN` is used for the Zoom Docs MCP server.
- `ZOOM_WHITEBOARD_MCP_ACCESS_TOKEN` is used for the Whiteboard MCP server.
- If one OAuth token includes both the main Zoom MCP scopes and the Zoom Docs MCP scopes, both variables can use the same value.
- After setting or rotating any of these tokens, restart Claude Code or re-enable the plugin so the MCP servers restart with the new environment.

## What You Can Do Without Connectors

- Choose the right Zoom surface for a new integration
- Plan SDK, REST API, webhook, OAuth, and MCP implementations
- Compare Meeting SDK vs Video SDK vs Zoom Apps vs REST API
- Debug architecture, auth, event-delivery, and integration mistakes
- Use the deep Zoom reference library bundled in `skills/`

## What Connectors Add

- Live MCP tool discovery and execution against Zoom-hosted MCP servers
- Real meeting-search, recording-resource, and document workflows
- Whiteboard-specific tool access when applicable
- Cross-Zoom search through the main `search_zoom` tool when the token has `ai_companion:read:search`

## Notes

- If a command or skill mentions connectors and you are not connected, continue in standalone mode using the reference docs.
- If you are unsure which connector is relevant, start with [`/setup-zoom-mcp`](./skills/setup-zoom-mcp/SKILL.md).
