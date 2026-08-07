# Zoom Plugin

A Claude plugin for planning, building, and debugging Zoom integrations. It helps choose the right Zoom surface, shape implementations, debug failures, and route into the right Zoom references without making the user read the whole doc tree first.

## Installation

Install this directory as a local Claude plugin. The plugin manifest is at [`.claude-plugin/plugin.json`](.claude-plugin/plugin.json) and the bundled Zoom MCP connectors are defined in [`.mcp.json`](.mcp.json).

Authentication path depends on the Claude product:

- **Claude Cowork**: use the published Zoom connector in Claude's connector directory and complete OAuth there.
- **Claude Code**: complete Zoom user-level OAuth yourself, export bearer tokens for the Zoom surfaces you want Claude to use, then reconnect the plugin and use `/setup-zoom-mcp`.

For Claude Code, export the bearer tokens before using the bundled MCP servers:

```bash
export ZOOM_MCP_ACCESS_TOKEN="your_zoom_user_oauth_access_token"
export ZOOM_MEETINGS_MCP_ACCESS_TOKEN="your_zoom_user_oauth_access_token"
export ZOOM_CANVAS_MCP_ACCESS_TOKEN="your_zoom_user_oauth_access_token"
export ZOOM_CHAT_MCP_ACCESS_TOKEN="your_zoom_user_oauth_access_token"
export ZOOM_TASKS_MCP_ACCESS_TOKEN="your_zoom_user_oauth_access_token"
export ZOOM_REVENUE_ACCELERATOR_MCP_ACCESS_TOKEN="your_zoom_user_oauth_access_token"
export ZOOM_WHITEBOARD_MCP_ACCESS_TOKEN="your_zoom_user_oauth_access_token"
```

The same access token can be assigned to each variable when the General App includes the
scopes required by all selected MCP servers. Use separate tokens if you want to keep server
permissions isolated.

## Slash Workflows

Explicit slash workflows implemented as skills under `skills/`:

| Workflow | Description |
|---|---|
| [`/start`](skills/start/SKILL.md) | Start with a Zoom app idea and get routed to the right product and build path |
| [`/setup-zoom-marketplace-app`](skills/setup-zoom-marketplace-app/SKILL.md) | Select, create, update, or validate the app model, manifest, scopes, events, and credentials for a Zoom integration |
| [`/setup-zoom-oauth`](skills/setup-zoom-oauth/SKILL.md) | Choose the auth model, scopes, and redirect flow for a Zoom app |
| [`/build-zoom-meeting-app`](skills/build-zoom-meeting-app/SKILL.md) | Build an embedded or managed Zoom meeting flow |
| [`/build-zoom-bot`](skills/build-zoom-bot/SKILL.md) | Build bots, recorders, and real-time meeting processors |
| [`/debug-zoom`](skills/debug-zoom/SKILL.md) | Triage a broken Zoom integration and isolate the failing layer |
| [`/setup-zoom-mcp`](skills/setup-zoom-mcp/SKILL.md) | Decide when Zoom MCP fits and set up a safe Claude workflow |
| [`/build-zoom-rest-api-app`](skills/rest-api/SKILL.md) | Route into Zoom REST endpoints, scopes, and resource patterns |
| [`/build-zoom-meeting-sdk-app`](skills/meeting-sdk/SKILL.md) | Route into embedded Zoom meeting implementation details |
| [`/build-zoom-video-sdk-app`](skills/video-sdk/SKILL.md) | Route into custom video-session implementation details |
| [`/setup-zoom-webhooks`](skills/webhooks/SKILL.md) | Set up Zoom webhook subscriptions, signature verification, and handlers |
| [`/setup-zoom-websockets`](skills/websockets/SKILL.md) | Set up Zoom WebSocket event delivery when it fits better than webhooks |
| [`/build-zoom-team-chat-app`](skills/team-chat/SKILL.md) | Build Team Chat user or chatbot integrations |
| [`/build-zoom-phone-integration`](skills/phone/SKILL.md) | Build Zoom Phone integrations around Smart Embed, APIs, and events |
| [`/build-zoom-contact-center-app`](skills/contact-center/SKILL.md) | Build Contact Center app, web, or native integrations |
| [`/build-zoom-virtual-agent`](skills/virtual-agent/SKILL.md) | Build Virtual Agent web or mobile wrapper integrations |

## Internal Routing Skills

These remain in the plugin as automatic routing helpers, but they are no longer part of the public slash-command surface:

- [`start`](skills/start/SKILL.md)
- [`plan-zoom-product`](skills/plan-zoom-product/SKILL.md)
- [`plan-zoom-integration`](skills/plan-zoom-integration/SKILL.md)
- [`choose-zoom-approach`](skills/choose-zoom-approach/SKILL.md)
- [`design-mcp-workflow`](skills/design-mcp-workflow/SKILL.md)
- [`debug-zoom-integration`](skills/debug-zoom-integration/SKILL.md)

## Deep References

The plugin also keeps the original Zoom product-specific reference library under `skills/`. These are supporting references, not the primary entry surface:

- [`skills/general/`](skills/general/)
- [`skills/rest-api/`](skills/rest-api/)
- [`skills/meeting-sdk/`](skills/meeting-sdk/)
- [`skills/video-sdk/`](skills/video-sdk/)
- [`skills/webhooks/`](skills/webhooks/)
- [`skills/websockets/`](skills/websockets/)
- [`skills/oauth/`](skills/oauth/)
- [`skills/team-chat/`](skills/team-chat/)
- [`skills/scribe/`](skills/scribe/)
- [`skills/summarizer/`](skills/summarizer/)
- [`skills/translator/`](skills/translator/)
- [`skills/zoom-mcp/`](skills/zoom-mcp/)
- [`skills/zoom-mcp/team-chat/`](skills/zoom-mcp/team-chat/)
- [`skills/zoom-mcp/whiteboard/`](skills/zoom-mcp/whiteboard/)
- [`skills/zoom-mcp/references/servers.md`](skills/zoom-mcp/references/servers.md)

## Example Workflows

### Starting from a Zoom app idea

```text
/start Build an internal meeting assistant that joins calls, extracts action items, and stores summaries
```

### Planning a new app

```text
/start Build a React app that lets customers schedule and join Zoom meetings from our product
```

### Setting up the Marketplace app

```text
/setup-zoom-marketplace-app Create the minimum user-managed General App manifest for a meeting API workflow with webhooks
```

For programmatic Marketplace app creation from Claude Code, register the separately hosted helper
MCP server first:

```bash
claude mcp add --transport http \
  zoom-marketplace-helper \
  https://marketplacehelper.asdc.cc/mcp
```

Then run `/setup-zoom-marketplace-app`. When the helper tools are connected, the skill selects
`zoom-marketplace-helper` automatically, previews the intended account change, asks for
confirmation, and verifies the resulting app after the write.
The helper is external to this plugin and its MCP endpoint must already be reachable by Claude Code.
If the helper endpoint changes, re-register the Claude Code MCP server with the new `/mcp` URL.
This helper URL only connects Claude Code to the helper. Never use it, the helper operator's
OAuth client ID, or the helper operator's OAuth callback in a Marketplace app created for a user.
That app must use the user's own credentials and HTTPS endpoints.

#### Test a new app without deploying it

If you do not have an existing HTTPS server for the app yet, expose your local app with a
temporary tunnel before creating the Marketplace app. This lets you test the OAuth redirect,
Zoom App home URL, and webhook endpoints through the app you are running locally.

Start the app locally, for example on port `3000`, then choose one tunnel:

```bash
# ngrok
ngrok http 3000

# Or Cloudflare Tunnel
cloudflared tunnel --url http://localhost:3000
```

Use the HTTPS tunnel URL in the app configuration, with the actual routes implemented by your
local server. For example:

```text
Home URL:              https://YOUR_TUNNEL_HOST/
OAuth redirect URL:    https://YOUR_TUNNEL_HOST/oauth/callback
Webhook endpoint URL:  https://YOUR_TUNNEL_HOST/webhooks/zoom
```

If the user already has a hosted HTTPS service, use its routes instead. Build the OAuth
authorization URL with the client ID Zoom issues for the user's app and the exact redirect URL
configured for that app. Do not copy a maintainer, helper, sample, or previous user's OAuth URL.

The tunnel only forwards requests; it does not create these routes or handle OAuth and webhook
validation for you. Free or ephemeral tunnel URLs can change, so update the Marketplace app
configuration and any generated manifest values whenever the tunnel hostname changes. Use a
deployed HTTPS service or a reserved tunnel hostname when the URL must remain stable.

Before asking `zoom-marketplace-helper` to create or update the app, provide the current tunnel
host and route paths. After every tunnel restart, update the development OAuth redirect URL and
OAuth allow list, development home URL, and development webhook URL before testing. Keep the
production URLs unchanged unless you intentionally want to test the tunnel as production.

The app tunnel and the helper MCP endpoint are separate concerns and their URLs are not
interchangeable. If both are running locally,
each service needs a publicly reachable HTTPS URL, or the helper can be deployed while only the
app uses a local tunnel.

### Debugging a broken webhook

```text
/debug-zoom My Zoom webhook signature verification fails in production but not locally
```

### Designing an MCP flow

```text
/setup-zoom-mcp I want Claude to search meetings, pull recording resources, and create follow-up docs
```

## Connectors

See [CONNECTORS.md](CONNECTORS.md). The plugin works standalone from the bundled skills, and gets supercharged when Claude can use the bundled Zoom MCP servers from [`.mcp.json`](.mcp.json).

The current official Zoom MCP catalog contains the main Zoom MCP, Meetings, Canvas, Chat, Tasks,
Revenue Accelerator, and Whiteboard servers. All 7 current server definitions are bundled in
[`.mcp.json`](.mcp.json); the complete inventory and current endpoints are in
[`skills/zoom-mcp/references/servers.md`](skills/zoom-mcp/references/servers.md).
For new document workflows, prefer Zoom Canvas MCP. For new chat workflows, prefer Zoom Chat MCP.

## Cross-Platform Notes

This repo is packaged first as a Claude plugin, but it also includes [AGENTS.md](AGENTS.md) for agent ecosystems that use a repo-level discovery file. The reusable core remains the `skills/` tree and its `SKILL.md` files.

## Structure

```text
Zoom Plugin/
├── .claude-plugin/plugin.json
├── .mcp.json
├── CONNECTORS.md
├── skills/
│   ├── plan-zoom-product/
│   ├── plan-zoom-integration/
│   ├── debug-zoom/
│   ├── setup-zoom-mcp/
│   ├── scribe/
│   ├── summarizer/
│   ├── translator/
│   ├── start/
│   ├── choose-zoom-approach/
│   ├── setup-zoom-oauth/
│   ├── build-zoom-meeting-app/
│   ├── build-zoom-bot/
│   ├── design-mcp-workflow/
│   ├── debug-zoom-integration/
│   └── ... existing Zoom reference skills
└── README.md
```

## License

MIT
