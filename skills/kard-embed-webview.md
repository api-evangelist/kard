---
name: Embed the Kard WebView
description: Mint a short-lived WebView token so a user can see their live offers
  in a hosted, embeddable Kard surface.
api: openapi/kard-api-reference-openapi.yaml
operations:
- get-web-view-token
operation_paths:
- get-web-view-token (POST /v2/auth/issuers/{organizationId}/users/{userId}/token)
scopes:
- user:read
- rewards:read
---

# Embed the Kard WebView

Mint a short-lived WebView token so a user can see their live offers in a hosted, embeddable Kard surface.

## Steps

1. **Authenticate** with your client credentials to get a Bearer token.
2. **Mint a WebView token.** `get-web-view-token` (`POST /v2/auth/issuers/{organizationId}/users/{userId}/token`) for the target user.
3. **Load the WebView.** Hand the returned token to the Kard WebView embed so the user sees their live offers/locations without you building UI (see `components/kard-components.yml`).

## Rules
- WebView tokens are user-scoped and short-lived; mint per session, never embed long-lived client credentials in the browser.
