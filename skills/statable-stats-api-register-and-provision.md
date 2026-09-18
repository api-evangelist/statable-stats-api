---
name: Register and provision a site without a browser
description: Open a Statable account from an emailed code, get the first API key, create a site, and read back its tracking snippet — all over HTTP, no dashboard.
api: openapi/statable-stats-api-openapi.yml
operations: [bootstrapSendOTP, bootstrapVerifyOTP, createSite, getSiteSnippet]
---

# Register and provision a site (no browser)

The two `/auth` routes are the only ones that take no `Authorization` header.

1. **Request a code** — `bootstrapSendOTP` (`POST /auth/send-otp`) with `{ "email": "<real inbox>" }`. Reserved/special-use domains are refused with `400 email_undeliverable`; the response is identical whether or not the address is registered.
2. **Exchange it for a key** — `bootstrapVerifyOTP` (`POST /auth/verify-otp`) with `email`, the emailed `code`, and `accept_terms: true` (show the user https://statable.com/terms first). The response carries `token` (an `stbl_` key) **once** — store it before anything else. `created:false` means the address already had an account.
3. **Create the site** — `createSite` (`POST /sites`) with `Authorization: Bearer stbl_…` and `{ "url": "https://example.com", "timezone": "Europe/Amsterdam" }`. Send an `Idempotency-Key` so a lost response is a safe retry (reuse with a different body → `409 idempotency_conflict`; same URL twice → `409 site_exists`).
4. **Read the snippet** — `getSiteSnippet` (`GET /sites/{id}/snippet`) returns the `<script>` to install.

Conventions: branch on the error `code`, never `error` text. Capture `X-Request-ID`. See conventions/ and errors/.
