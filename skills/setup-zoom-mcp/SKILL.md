---
name: setup-zoom-mcp
description: Decide when Zoom MCP is the right fit and produce a safe setup plan for Claude. Use when planning AI workflows over Zoom data, deciding between MCP and REST, or defining a hybrid MCP architecture.
argument-hint: "<AI workflow or MCP use case>"
---

# /setup-zoom-mcp

> If you see unfamiliar placeholders or need to check which tools are connected, see [CONNECTORS.md](../../CONNECTORS.md).

Plan a Zoom MCP workflow and decide when to use MCP alone versus a hybrid REST API + MCP architecture.

## Usage

```text
/setup-zoom-mcp $ARGUMENTS
```

## Workflow

1. Determine whether the goal is deterministic automation, AI tool orchestration, or a hybrid.
2. Identify whether the client uses Claude's published connector or requires a manually registered General App.
3. For manual registration, select the default, Team Chat, or Whiteboard MCP template through [setup-zoom-marketplace-app](../setup-zoom-marketplace-app/SKILL.md).
4. If MCP is appropriate, identify the likely Zoom MCP surface and transport assumptions.
5. If MCP alone is not enough, define the REST API responsibilities separately.
6. Call out auth, scope, and client capability constraints, especially the difference between Claude Cowork and Claude Code auth paths.
7. End with a minimal proof-of-concept sequence.

## Output

- Recommended MCP strategy
- Connector expectations
- Hybrid boundaries if REST is also required
- Risks and setup notes
- Relevant skill links

## Auth Rules

- **Claude Cowork**: use the published Zoom connector and complete OAuth in Claude's connector flow.
- **Claude Code**: for manual OAuth, start from the matching Marketplace MCP template, complete user-level OAuth, export `ZOOM_MCP_ACCESS_TOKEN`, reconnect the plugin, then continue with this skill.
- Scope requirements differ by MCP server. Use the server-specific scope sets below and the detailed tables in [../zoom-mcp/concepts/oauth-setup.md](../zoom-mcp/concepts/oauth-setup.md).

## Server-Specific Scope Sets

Main Zoom MCP server: `https://mcp.zoom.us/mcp/zoom/streamable`

Required scopes, accurate as of 10 Apr 2026:
- `ai_companion:read:search`
- `meeting:read:search`
- `meeting:read:assets`
- `cloud_recording:read:list_user_recordings`
- `cloud_recording:read:content`
- `docs:write:import`
- `docs:read:export`

Zoom Docs MCP server: `https://mcp.zoom.us/mcp/docs/streamable`

Documented tool scopes, accurate as of 10 Apr 2026:
- `create_file_with_content` → `docs:write:import`
- `get_file_content` → `docs:read:export`

Zoom Whiteboard MCP server: `https://mcp.zoom.us/mcp/whiteboard/streamable`

Documented tool scopes, accurate as of 10 Apr 2026:
- `add_a_whiteboard_collaborator` → `whiteboard:write:collaborator:admin`
- `create_a_whiteboard` → `whiteboard:write:whiteboard`
- `create_a_whiteboard_by_script` → `whiteboard:write:whiteboard`
- `create_a_whiteboard_for_brainstorming` → `whiteboard:write:whiteboard`
- `create_a_whiteboard_for_meeting_summary` → `whiteboard:write:whiteboard`
- `create_a_whiteboard_for_strategy_analysis` → `whiteboard:write:whiteboard`
- `delete_a_whiteboard_collaborator` → `whiteboard:delete:collaborator:admin`
- `get_a_whiteboard` → `whiteboard:read:whiteboard:admin`
- `get_a_whiteboard_collaborator` → `whiteboard:read:list_collaborators:admin`
- `list_whiteboards` → `whiteboard:read:list_whiteboards:admin`
- `update_a_whiteboard_collaborator` → `whiteboard:update:collaborator:admin`

Optional Zoom Team Chat MCP server: `https://mcp.zoom.us/mcp/team_chat/streamable`

This plugin documents Team Chat MCP, but does not register it in `.mcp.json` by default.
Use it only when the user explicitly wants write-capable Team Chat MCP tooling.

- `zoom_chat_message_send` → `team_chat:write:user_message`
- `zoom_chat_message_update` → `team_chat:update:user_message`
- `zoom_chat_contact_add` → `team_chat:write:contact_information`
- `zoom_chat_channel_create` → `team_chat:write:user_channel`
- `zoom_chat_channel_update` → `team_chat:update:user_channel`
- `zoom_chat_channel_members_add` → `team_chat:write:members`

## Related Skills

- [design-mcp-workflow](../design-mcp-workflow/SKILL.md)
- [setup-zoom-marketplace-app](../setup-zoom-marketplace-app/SKILL.md)
- [choose-zoom-approach](../choose-zoom-approach/SKILL.md)
