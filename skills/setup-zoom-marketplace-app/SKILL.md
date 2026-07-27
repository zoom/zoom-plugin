---
name: setup-zoom-marketplace-app
description: Create, update, select, or validate the Zoom Marketplace app that owns an integration. Use for General App manifests, Server-to-Server OAuth apps, Meeting SDK apps, scopes, event subscriptions, app-owned credentials, or choosing a scenario template before product implementation.
argument-hint: "<integration scenario or existing app configuration>"
---

# /setup-zoom-marketplace-app

Set up the Zoom Marketplace app boundary before implementing OAuth, API, SDK, webhook, WebSocket, RTMS, Team Chat, Contact Center, Phone, Zoom Apps, or MCP workflows.

## Workflow

1. Identify whether the task creates a new app or updates an existing app, then classify the product scenario, actor, account ownership, and app model.
2. Inspect the machine-readable template index and verify `app_type`, `usage`, `unsupported_app_types`, and `supports_manifest_update` before selecting the narrowest template.
3. For a new app, replace sample names, URLs, domains, contacts, commands, and scopes. Keep only scopes required by the exact operations.
4. For an existing General App, export its complete manifest, apply the requested changes in memory, validate with its `app_id`, replace it with `PUT`, and export it again. Never apply a static template directly.
5. For a new General App, validate the inner `manifest` and check both HTTP status and the response `ok` value before creating it through `/v2/marketplace/apps`.
6. Create native S2S and Meeting SDK apps through `/v2/accounts/{accountId}/marketplace/apps` with the required master scope; these are create requests, not General App manifests.
7. Complete post-create setup that the public schema does not reliably encode, including WebSocket delivery and some webhook or RTMS event subscriptions.
8. Store generated secrets in a secret manager, record only safe environment-variable names, and return to the owning product workflow.

## Optional Claude Code Marketplace Helper

To let Claude Code create or validate Marketplace apps programmatically, register a separately
hosted Marketplace helper MCP server. This plugin does not bundle or host that helper server.
Start the helper first, expose its `/mcp` endpoint over HTTPS, and register the current endpoint:

```bash
claude mcp add --transport http \
  zoom-marketplace-helper \
  https://YOUR_NGROK_HOST.ngrok-free.app/mcp
```

Replace `YOUR_NGROK_HOST` with the hostname for the running helper. A temporary ngrok URL can
change when the tunnel restarts, so update the Claude Code server registration when that happens.
Only register a helper endpoint that you trust because it can create or modify Marketplace apps
and may handle app credentials. After registration, use `/setup-zoom-marketplace-app` and tell
Claude Code to use the `zoom-marketplace-helper` MCP server for the requested Marketplace
operation.

## Primary References

- [Marketplace app management](../rest-api/references/marketplace-apps.md)
- [Marketplace template selector](../rest-api/references/marketplace-app-templates.md)
- [Machine-readable template index](../rest-api/assets/marketplace-apps/marketplace-manifest-template-index.json)
- [General App manifest update workflow](../rest-api/references/marketplace-manifest-update-workflow.md)
- [OAuth](../oauth/SKILL.md)
- [REST API](../rest-api/SKILL.md)

## Guardrails

- Do not mix user and admin scope suffixes in one General App.
- Do not use S2S for Meeting/Webinar RTMS or Team Chat chatbot subscriptions.
- Do not invent `webhook` or `webhook_only` app types.
- Do not use a General App manifest update workflow for native `s2s_oauth` or `meeting_sdk` apps.
- Never remove existing scopes or event subscriptions during an update unless the user explicitly requests it.
- Do not print, commit, or retain generated client secrets in logs or test artifacts.
- Treat schema validation as necessary but not sufficient; licensing, entitlements, authorization, and Marketplace review can still block runtime use.
