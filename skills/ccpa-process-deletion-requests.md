---
name: ccpa-process-deletion-requests
description: >-
  Run one complete CalPrivacy DROP deletion cycle for a registered California data broker —
  download the consumer deletion archive, hash and match identifiers against your own
  records, and report a status for every work item. Use when a download.ready notification
  arrives or on your regular processing cadence.
api: ccpa:drop-data-broker-api
base_url: https://api.drop.privacy.ca.gov
sandbox_url: https://api.drop.privacy.ca.gov/sandbox
operations:
  - downloadData
  - uploadData
generated: '2026-09-05'
method: generated
source: >-
  openapi/ccpa-drop-databroker-api.yml (operationIds verified against the spec) plus
  conventions/ccpa-conventions.yml, errors/ccpa-problem-types.yml and
  https://privacy.ca.gov/drop-for-data-brokers/technical-specifications/
---

# Process a DROP consumer deletion cycle

## Before you start

- You need an `X-API-KEY` issued in the [DROP Data Broker Portal](https://databroker.drop.privacy.ca.gov/).
  It only exists after your organization is registered as a California data broker and the
  annual fee is paid. There is no self-service key.
- Rehearse against the sandbox base `https://api.drop.privacy.ca.gov/sandbox` with a sandbox
  key before touching production. CalPrivacy publishes no test fixtures, so build your own
  from the standardization rules.
- Send `X-API-KEY` on every request. A missing or bad key returns `401` with a plain-text
  body, not JSON.

## 1. Download the archive — `downloadData`

```
GET https://api.drop.privacy.ca.gov/data/download
X-API-KEY: <your key>
```

Branch on the response, and disambiguate `200` by **Content-Type**, not by the status code:

| Response | Meaning | Do this |
| --- | --- | --- |
| `200` + `application/zip` | Archive is ready | Save it; read `Content-Disposition` for the filename |
| `200` + `application/json` | No new consumer request data | Stop; there is nothing to process this cycle |
| `202` + `Retry-After` | Archive is being prepared | Wait for `Retry-After`, then call `GET /data/download` again |
| `409` | Previous download is not complete | Finish the open cycle first — do not start a new one |
| `429` | Throttled | Wait 30 seconds, retry |
| `500` / `503` | Server error | Retry later |

Poll on `Retry-After`. Do not poll on a fixed interval: there are no rate-limit headers to
tell you how much budget is left, so the only signal you get is a `429`.

## 2. Read the CSVs

The ZIP contains one CSV per selected list type. A selected list with no new records may be
absent from the archive.

- List types: `NDZ`, `Email`, `Phone`, `MAID`, `NameVIN`, `CTVID`.
- A `Removed` file may also be present. `Removed` is a **file** type, not a deletion list
  type — those identifiers have come **off** the list and must not be actioned.
- File names follow `YYYYMMDD_DataBrokerId_DataType[_OptionalSuffix].csv`.
- Every identifier is a SHA-256 hash, Base64 encoded. Nothing arrives in the clear.

## 3. Match against your records

Standardize your own identifiers **exactly** as CalPrivacy specifies, then hash with SHA-256
(UTF-8 in, Base64 out). If your standardization differs by one character, your hash will not
match and you will report `5 Not found` for a consumer you actually hold.

- **Email** — remove all whitespace, then lowercase. Do **not** strip dots or plus signs.
- **DOB** — `YYYYMMDD`, no separators.
- **Phone** — digits only, keep the last 10 (or all, if fewer).
- **ZIP** — alphanumeric only; drop the +4; lowercase; remove leading zeros; take the first
  five characters present.
- **Names** — normalize Unicode, lowercase, transliterate supported Greek and Cyrillic, fold
  supported Latin to ASCII, then keep letters and digits only.
- **MAID** — hex characters only, lowercase, must be 32 characters.
- **VIN** — alphanumeric only, lowercase, must be 17 characters.
- **CTVID** — alphanumeric only, lowercase, 8-32 characters.

Composite lists hash twice: standardize and hash each field separately, concatenate the
Base64 hashes in the required order, then hash the result.

- `NDZ` = FirstName + LastName + DOB + ZIP
- `NameVIN` = FirstName + LastName + VIN

Full rules: <https://privacy.ca.gov/drop-for-data-brokers/technical-specifications/working-with-data/>

## 4. Take the required action, then pick a status

| Code | Label | When |
| --- | --- | --- |
| `2` | Exempted | Match found and all the personal information is exempt |
| `3` | Deleted | Match found and non-exempt personal information was deleted |
| `4` | Opted out | Multiple consumers are linked to the one identifier and all were opted out of sale or sharing |
| `5` | Not found | No match found **after completing** the matching process |

There is no code `1`. Report `5` only after the match actually ran — not because a lookup
timed out.

**This is the irreversible step.** `POST /data/amend` can correct the report you file; it
cannot restore data you deleted. Delete first, then report, and never report `3` for a
deletion you have not performed.

## 5. Upload the statuses — `uploadData`

```
POST https://api.drop.privacy.ca.gov/data/upload
X-API-KEY: <your key>
Content-Type: multipart/form-data
```

- Form field name must be `files`.
- Only CSV files are accepted.
- CSV header must be exactly `Id,Status`.
- `Id` is the work item id from the downloaded row. `Status` is 2, 3, 4 or 5.
- Split a large response across files with the optional suffix, e.g.
  `20260312_4821_Email_part01.csv`.

### Read the receipt, not just the status code

A `202` means *at least one* file was accepted. Per-file rejections ride inside the body:

```json
{ "message": "...", "acceptedCount": 1, "rejectedCount": 1,
  "accepted": [ ... ],
  "rejected": [ { "fileName": "notes.txt", "message": "Only CSV files are accepted." } ] }
```

Always iterate `rejected[]` and resubmit those files. Treating `202` as success is how a
batch goes silently unreported.

Other outcomes: `400` malformed or nothing accepted; `409` no active download is waiting for
responses (call `GET /data/download` first); `403` your account is not authorized for that
list.

## 6. Do not retry blindly

There is **no idempotency key** on this API. A retried upload is a new upload, not a
deduplicated one. Retry only `429`, `500` and `503`. Never retry `400`, `401`, `403`, `404`
or `409` — fix the cause instead.

## 7. Repeat

This is an ongoing pull-process-respond cycle, not a one-time integration. Subscribe to the
`download.ready` webhook (see `asyncapi/ccpa-drop-webhooks.yml`) or the equivalent email
notification, verify the HMAC-SHA256 signature, and start again at step 1. The webhook body
carries only a message — switch on `X-Webhook-Event-Type`.

Statutory timelines live in Civil Code 1798.99.80 et seq. and CCR title 11 section 7600.
Data brokers are required to begin processing DROP requests from 2026-08-01.
