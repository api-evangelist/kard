---
name: Enroll a user and fetch their live offers
description: Enroll a cardholder in a Kard rewards program and read the personalized
  offers and locations available to them.
api: openapi/kard-api-reference-openapi.yaml
operations:
- create
- offers
- locations
operation_paths:
- create (POST /v2/issuers/{organizationId}/users)
- offers (GET /v2/issuers/{organizationId}/users/{userId}/offers)
- locations (GET /v2/issuers/{organizationId}/users/{userId}/locations)
scopes:
- user:write
- rewards:read
---

# Enroll a user and fetch their live offers

Enroll a cardholder in a Kard rewards program and read the personalized offers and locations available to them.

## Steps

1. **Get a token.** POST to `https://{client-subdomain}.getkard.com/v2/auth/token` with `grant_type=client_credentials` and HTTP Basic `base64(client_id:client_secret)`. Use the returned `access_token` as a `Bearer` token (expires in 3600s). Optionally scope to an issuer with `X-Kard-Target-Issuer`.
2. **Enroll the user.** `create` (`POST /v2/issuers/{organizationId}/users`) with the cardholder's referring-partner user id and card metadata. Requires scope `user:write`.
3. **Read offers.** `offers` (`GET /v2/issuers/{organizationId}/users/{userId}/offers`) to fetch the live, personalized offers for that user. Requires `rewards:read`.
4. **Read locations.** `locations` (`GET /v2/issuers/{organizationId}/users/{userId}/locations`) for in-store redemption locations.

## Rules
- All requests are Bearer-authenticated; a `403` means the token is missing the required scope.
- Errors come back as JSON:API `{errors:[{status,title,detail,source}]}` — inspect `source.pointer` for the bad field (see `errors/kard-problem-types.yml`).
