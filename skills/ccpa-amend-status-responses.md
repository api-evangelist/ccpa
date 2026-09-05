---
name: ccpa-amend-status-responses
description: >-
  Correct a status you already reported to CalPrivacy DROP — the reversal path for a prior
  upload. Use when you discover a misreported work item (wrong code, wrong match, a
  deletion that had not actually completed) after POST /data/upload was accepted.
api: ccpa:drop-data-broker-api
base_url: https://api.drop.privacy.ca.gov
sandbox_url: https://api.drop.privacy.ca.gov/sandbox
operations:
  - uploadAmend
generated: '2026-09-05'
method: generated
source: >-
  openapi/ccpa-drop-databroker-api.yml (operationId uploadAmend verified against the spec)
  plus conventions/ccpa-conventions.yml and
  https://privacy.ca.gov/drop-for-data-brokers/technical-specifications/api-operations/
---

# Amend a DROP status response

## What this can and cannot undo

`POST /data/amend` is the only reversal operation on this API, and it reverses **the
report**, not the action.

- It **can** change the status code you filed for a work item.
- It **cannot** restore personal information you deleted after reporting `3`.
- It **cannot** un-opt-out a consumer.

Decide which of those you actually need before you call. If a deletion was performed in
error, amending the status does not fix the underlying problem.

## No published window

CalPrivacy documents that amendment exists but publishes **no deadline** for it. The
technical specifications point at Civil Code 1798.99.80 et seq. and CCR title 11 section
7600 for timelines without restating them. Do not assume a window, and do not tell a user
one exists — amend as soon as you find the error, and check the statute if the timing
matters.

## Call it

```
POST https://api.drop.privacy.ca.gov/data/amend
X-API-KEY: <your key>
Content-Type: multipart/form-data
```

Same shape as an upload:

- Form field name `files`.
- CSV only, header exactly `Id,Status`.
- `Id` is the original work item id. `Status` is the **corrected** code: 2, 3, 4 or 5.
- Include only the rows you are correcting.

## Responses

| Code | Meaning | Do this |
| --- | --- | --- |
| `202` | At least one file accepted and queued for validation | Read `rejected[]` anyway — a 202 can carry rejections |
| `400` | Malformed, or no files accepted | Fix the CSV header, file type, or status values |
| `409` | The amend request cannot be processed in the current state | The cycle is not in an amendable state; check the portal |
| `401` / `403` | Key missing/invalid, or account not authorized | Do not retry; fix the credential or the registration |
| `429` | Throttled | Wait 30 seconds, retry |
| `500` / `503` | Server error | Retry later |

## Guardrails

- There is **no idempotency key**. Submitting the same amendment file twice submits it
  twice. Track what you have sent yourself.
- An amendment is itself amendable, so an incorrect amendment is recoverable by the same
  route — again with no published window.
- Watch for the `amendment.received` and `amendment.processed` notifications (webhook or
  email) to confirm the correction landed. Verify the HMAC-SHA256 signature on any webhook
  before trusting it; see `asyncapi/ccpa-drop-webhooks.yml`.
- If a human is in the loop, surface what changed: the work item ids, the old code and the
  new code. This is a regulatory filing, and the audit trail matters more than the call.
