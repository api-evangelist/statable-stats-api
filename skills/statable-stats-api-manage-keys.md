---
name: Manage API keys over HTTP
description: List, create, rotate and revoke stbl_ API keys programmatically, respecting the rules that stop a key granting more than it holds.
api: openapi/statable-stats-api-openapi.yml
operations: [listApiKeys, createApiKey, rotateApiKey, revokeApiKey, listApiKeyEvents]
---

# Manage API keys

Requires the **Manage API keys** permission.

1. **List keys** — `listApiKeys` (`GET /keys`).
2. **Create a key** — `createApiKey` (`POST /keys`) with a name, site access (all/one), permissions, and `expires_in_days` (1–365). Asking for a scope the acting key lacks → `409 scope_escalation` / `403`; `invalid_expiry` and `key_limit_reached` (default cap 10) also guard it. The `token` is shown **once**.
3. **Rotate** — `rotateApiKey` (`POST /keys/{id}/rotate`) mints a new token for the same key; the old one stops immediately. A key rotating itself → `409 self_modification`.
4. **Revoke** — `revokeApiKey` (`DELETE /keys/{id}`) ends a key immediately and frees a slot.
5. **Audit** — `listApiKeyEvents` (`GET /keys/{id}/events`).

Keys are server-side secrets — never in browser JS (the API sends no CORS headers anyway). Give each job its own key.
