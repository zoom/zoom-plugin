---
name: setup-zoom-marketplace-app
description: Create, select, or validate the Zoom Marketplace app that owns an integration. Use for General App manifests, Server-to-Server OAuth apps, Meeting SDK apps, scopes, event subscriptions, app-owned credentials, or choosing a scenario template before product implementation.
argument-hint: "<integration scenario or existing app configuration>"
---

# /setup-zoom-marketplace-app

Set up the Zoom Marketplace app boundary before implementing OAuth, API, SDK, webhook, WebSocket, RTMS, Team Chat, Contact Center, Phone, Zoom Apps, or MCP workflows.

## Workflow

1. Identify the product scenario, actor, account ownership, and whether the app is user-managed, admin-managed, S2S, Meeting SDK, or Build Platform.
2. Select the narrowest matching template from the Marketplace template selector.
3. Replace sample names, URLs, domains, contacts, commands, and scopes. Keep only scopes required by the exact operations.
4. For General Apps, extract the inner `manifest`, validate it, and check both HTTP status and the response `ok` value.
5. Create General Apps through `/v2/marketplace/apps`; create S2S and Meeting SDK apps through `/v2/accounts/{accountId}/marketplace/apps` with the required master scope.
6. Complete post-create setup that the public schema does not reliably encode, including WebSocket delivery and some webhook or RTMS event subscriptions.
7. Store generated secrets in a secret manager, record only safe environment-variable names, and return to the owning product workflow.

## Primary References

- [Marketplace app management](../rest-api/references/marketplace-apps.md)
- [Marketplace template selector](../rest-api/references/marketplace-app-templates.md)
- [OAuth](../oauth/SKILL.md)
- [REST API](../rest-api/SKILL.md)

## Guardrails

- Do not mix user and admin scope suffixes in one General App.
- Do not use S2S for Meeting/Webinar RTMS or Team Chat chatbot subscriptions.
- Do not invent `webhook` or `webhook_only` app types.
- Do not print, commit, or retain generated client secrets in logs or test artifacts.
- Treat schema validation as necessary but not sufficient; licensing, entitlements, authorization, and Marketplace review can still block runtime use.
