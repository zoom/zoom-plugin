# Connectors

This plugin works in two modes:

- Standalone: Claude uses the bundled Zoom skills and reference material included with this plugin.
- Supercharged: Claude can also use the bundled Zoom MCP servers from [`.mcp.json`](./.mcp.json) for live tool access.

## Included MCP Servers

| Connector | Endpoint | Use For |
|---|---|---|
| `zoom-mcp` | `https://mcp.zoom.us/mcp/zoom/streamable` | Zoom MCP Server workflows for meeting search, cross-Zoom search, recordings, meeting assets, Docs, and Hub content |
| `zoom-docs-mcp` | `https://mcp.zoom.us/mcp/docs/streamable` | Legacy dedicated Docs compatibility surface; prefer Zoom Canvas MCP for new workflows |
| `zoom-whiteboard-mcp` | `https://mcp.zoom.us/mcp/whiteboard/streamable` | Whiteboard-specific MCP workflows |

The complete current Zoom server inventory is in
[skills/zoom-mcp/references/servers.md](./skills/zoom-mcp/references/servers.md).

## Current Official MCP Surfaces Not Bundled by Default

| Surface | Endpoint | Notes |
|---|---|---|
| Zoom Meetings MCP | `https://mcp.zoom.us/mcp/meeting/streamable` | Product-specific meeting search, meeting assets, and recording tools. |
| Zoom Canvas MCP | `https://mcp.zoom.us/mcp/canvas/streamable` | Current Docs/Canvas file, block, collaborator, and access tools. |
| Zoom Chat MCP | `https://mcp.zoom.us/mcp/chat/streamable` | Read/write Chat messages, channels, contacts, files, and sessions. The child guidance remains under [`skills/zoom-mcp/team-chat/`](./skills/zoom-mcp/team-chat/). |
| Zoom Tasks MCP | `https://mcp.zoom.us/mcp/tasks/streamable` | Task, step, comment, assignee, and collaborator tools. |
| Zoom Revenue Accelerator MCP | `https://mcp.zoom.us/mcp/revenue_accelerator/streamable` | Conversation, deal, analysis, scorecard, CRM, and team tools. |

## Legacy Compatibility Paths

The bundled `zoom-docs-mcp` definition and the former Team Chat path may still respond, but they
are not current entries in Zoom's published MCP server directory. Use Zoom Canvas for new Docs
workflows and Zoom Chat at `/mcp/chat/streamable` for new Chat workflows. Use only the current
`mcp.zoom.us` production host.

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
- `ZOOM_DOCS_MCP_ACCESS_TOKEN` is used for the legacy dedicated Docs MCP definition.
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
- Whiteboard, Canvas, Chat, Tasks, and Revenue Accelerator tool access when those servers are registered
- Cross-Zoom search through the main `search_zoom` tool when the token has `ai_companion:read:search`

## Notes

- If a command or skill mentions connectors and you are not connected, continue in standalone mode using the reference docs.
- If you are unsure which connector is relevant, start with [`/setup-zoom-mcp`](./skills/setup-zoom-mcp/SKILL.md).
