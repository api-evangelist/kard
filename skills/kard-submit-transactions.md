---
name: Submit transactions for reward matching
description: Send cardholder transactions to Kard for CLO matching and read back earned
  rewards.
api: openapi/kard-api-reference-openapi.yaml
operations:
- create
- get-earned-rewards
- create-bulk-transactions-upload-url
operation_paths:
- create (POST /v2/issuers/{organizationId}/transactions)
- get-earned-rewards (GET /v2/issuers/{organizationId}/users/{userId}/earned-rewards)
- create-bulk-transactions-upload-url (POST /v2/issuers/{organizationId}/transactions/uploads)
scopes:
- transaction:write
- transaction:read
---

# Submit transactions for reward matching

Send cardholder transactions to Kard for CLO matching and read back earned rewards.

## Steps

1. **Authenticate** (see the enroll skill) to get a Bearer token.
2. **Send transactions.** `create` (`POST /v2/issuers/{organizationId}/transactions`) with the correct `type`: `transaction` (Kard matches it), `matchedTransaction` (pre-matched, Kard validates), or `coreTransaction` (core-banking, limited card data). Requires `transaction:write`.
3. **Bulk upload.** For large volumes use `create-bulk-transactions-upload-url` (`POST /v2/issuers/{organizationId}/transactions/uploads`) to get a signed upload URL, then upload the file.
4. **Read earned rewards.** `get-earned-rewards` (`GET /v2/issuers/{organizationId}/users/{userId}/earned-rewards`) once matching completes. Requires `transaction:read`.

## Rules
- Bulk/batched writes may return `207 Multi-Status`; per-item failures carry the offending resource id in `errors[].id`.
- Reward settlement is asynchronous — subscribe to `earnedRewardApproved`/`earnedRewardSettled` webhooks rather than polling (see the webhooks skill).
