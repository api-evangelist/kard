---
name: Subscribe to reward notifications
description: Register a webhook subscription for earned-reward events and verify the
  HMAC signature on delivery.
api: openapi/kard-api-reference-openapi.yaml
operations:
- create
- get
- list
- replay
operation_paths:
- create (POST /v2/issuers/{organizationId}/subscriptions)
- get (GET /v2/issuers/{organizationId}/subscriptions)
- list (GET /v2/issuers/{organizationId}/notifications)
- replay (POST /v2/issuers/{organizationId}/notifications/{eventId}/replay)
scopes:
- notifications:write
- notifications:read
---

# Subscribe to reward notifications

Register a webhook subscription for earned-reward events and verify the HMAC signature on delivery.

## Steps

1. **Authenticate** to get a Bearer token.
2. **Create a subscription.** `create` (`POST /v2/issuers/{organizationId}/subscriptions`) with your HTTPS webhook URL to receive `earnedRewardApproved` and `earnedRewardSettled` events. Requires `notifications:write`.
3. **Verify each delivery.** Compute `HMAC-SHA256(webhook_body, webhook_key)` and compare (case-insensitive) to the `notify-signature` header. Reject on mismatch.
4. **Reconcile.** Use `list` (`GET /v2/issuers/{organizationId}/notifications`) to audit events and `replay` (`POST /v2/issuers/{organizationId}/notifications/{eventId}/replay`) to redeliver a missed event. Requires `notifications:read`/`notifications:write`.

## Rules
- Each notification carries a unique idempotency identifier — dedupe on it; deliveries may repeat.
- The payload includes a suggested push `message` and `attribution` tracking links (see `asyncapi/kard-notifications-webhooks.yml`).
