---
name: amtrust-claims-medical-case
description: Read AmTrust workers' compensation medical-case claim data, summaries and notes by claim
  number, and append case notes.
api: amtrust-financial-services:amtrust-financial-services-experience-claims-medical-case-api
generated: '2026-09-02'
method: generated
source: openapi/amtrust-financial-services-experience-claims-medical-case-api-openapi.json (harvested
  verbatim from AmTrust Azure API Management, 2026-09-02)
base_url: https://gateway.amtrustgroup.com/experience-claims-medical-case
operations:
  - Mcm_GetClaimData
  - Mcm_GetClaimSummariesData
  - Mcm_GetClaimNotes
  - Mcm_SaveClaimNotes
---

# AmTrust medical case management claims

Base URL: `https://gateway.amtrustgroup.com/experience-claims-medical-case`. Same two credentials as
every other AmTrust API.

## The version segment is templated and undocumented

Every path in this API is `/api/v{version}/...`. The OpenAPI document declares `{version}` as a path
parameter with **no enum and no default**, and AmTrust publishes no list of valid values. You cannot
determine the correct value from the contract — confirm it with AmTrust before building against this
API. Do not guess.

## Operations

- `Mcm_GetClaimData` — `GET /api/v{version}/claims/{claimNumber}` — full claim record.
- `Mcm_GetClaimSummariesData` — `GET /api/v{version}/claim-summaries/{claimNumber}` — summary view.
- `Mcm_GetClaimNotes` — `GET /api/v{version}/claim-notes/{claimNumber}` — existing case notes.
- `Mcm_SaveClaimNotes` — `POST /api/v{version}/claim-notes/{claimNumber}` — append a note.

## Handling claim data

This API returns **injured-worker medical information**. Treat every response as protected health
information: do not log response bodies, do not persist them outside a system approved for PHI, and do
not pass them into a general-purpose model context. `Mcm_SaveClaimNotes` writes to a claim record that
is discoverable in litigation — write factual case-management notes only.

## Errors

This API documents **only 200**. It declares no 4xx or 5xx response at all, so error shapes are
undocumented: expect the gateway envelope (`{ statusCode, message }` on 401,
`{ httpStatusCode, messageCode, title, detail }` on 404) and code defensively against anything else.
There is no idempotency key on `Mcm_SaveClaimNotes`, so a retry after a lost response can duplicate a
note — read notes back with `Mcm_GetClaimNotes` before retrying.
