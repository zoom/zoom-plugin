---
name: start
description: "Entry point for Zoom app development. Classifies a user's Zoom integration goal, recommends a specific Zoom surface (REST API, Meeting SDK, Video SDK, Zoom Apps SDK, webhooks, WebSockets, or MCP), and routes to the matching implementation skill. Use when building a Zoom app, choosing between Meeting SDK and Video SDK, setting up Zoom OAuth, adding Zoom webhooks, creating a Zoom chatbot, or planning any Zoom marketplace integration."
triggers:
  - "zoom app"
  - "zoom integration"
  - "zoom api"
  - "zoom oauth"
  - "zoom webhook"
  - "zoom bot"
  - "zoom meeting sdk"
  - "zoom video sdk"
  - "zoom marketplace"
  - "zoom mcp"
  - "build zoom"
  - "zoom chatbot"
  - "zoom embed"
---

# Start

Default entry skill for the Zoom plugin. Classifies the user's integration goal by job-to-be-done and routes to the correct implementation skill.

## Workflow

1. **Classify the request** — identify the user's job-to-be-done from their description. Match against the routing table below.
2. **Confirm the route** — if the request maps clearly to one row, proceed. If it spans multiple routes or is ambiguous, ask one short clarifying question before routing.
3. **Route to implementation skill** — hand off to the matched skill with the user's context.
4. **Pull in references** — attach supporting Zoom references only after the route is confirmed and implementation is underway.

## Routing Table

| If the user wants to... | Route to |
|---|---|
| Choose the right Zoom surface for a new project | [plan-zoom-product](../plan-zoom-product/SKILL.md) |
| Set up OAuth, tokens, scopes, or app credentials | [setup-zoom-oauth](../setup-zoom-oauth/SKILL.md) |
| Embed or customize a Zoom meeting flow | [build-zoom-meeting-app](../build-zoom-meeting-app/SKILL.md) |
| Build a bot, recorder, or real-time meeting processor | [build-zoom-bot](../build-zoom-bot/SKILL.md) |
| Use Zoom-hosted MCP for AI workflows | [setup-zoom-mcp](../setup-zoom-mcp/SKILL.md) |
| Debug a broken integration | [debug-zoom](../debug-zoom/SKILL.md) |

## Examples

**User:** "I want to build a React app where customers can join Zoom meetings without leaving our product."
**Route:** [build-zoom-meeting-app](../build-zoom-meeting-app/SKILL.md) — this is a Meeting SDK web embed.

**User:** "I need a bot that joins our daily standup, records it, and posts a summary to Slack."
**Route:** [build-zoom-bot](../build-zoom-bot/SKILL.md) — this is a headless meeting bot with RTMS media access.

**User:** "Should I use the Meeting SDK or Video SDK for a telehealth app?"
**Route:** [plan-zoom-product](../plan-zoom-product/SKILL.md) — needs surface comparison before implementation.

## Supporting Zoom References

Use these only after selecting the workflow:

- [general](../general/SKILL.md)
- [rest-api](../rest-api/SKILL.md)
- [meeting-sdk](../meeting-sdk/SKILL.md)
- [video-sdk](../video-sdk/SKILL.md)
- [webhooks](../webhooks/SKILL.md)
- [websockets](../websockets/SKILL.md)
- [oauth](../oauth/SKILL.md)
- [zoom-mcp](../zoom-mcp/SKILL.md)

## Operating Rules

1. Prefer one clear recommendation over a product catalog dump.
2. Ask a short clarifier only when the route is genuinely ambiguous.
3. Keep the first response architectural and actionable, then go deep.
4. Pull in deeper references only when they directly help the current decision or implementation.
