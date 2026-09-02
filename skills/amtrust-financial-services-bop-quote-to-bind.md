---
name: amtrust-bop-quote-to-bind
description: Quote and bind an AmTrust Businessowners Policy — insured clearance, locations and
  buildings, class codes, underwriting questions, loss history, rating, payment plan and bind.
api: amtrust-financial-services:amtrust-financial-services-digital-bop-api
generated: '2026-09-02'
method: generated
source: openapi/amtrust-financial-services-digital-bop-api-openapi.json (harvested verbatim from AmTrust
  Azure API Management, 2026-09-02)
base_url: https://gateway.amtrustgroup.com/digital-bop
operations:
  - get-api-v1-agent-contacts
  - post-api-v1-insured-clearance
  - post-api-v1-quotes-rate
  - get-api-v3-quotes-quoteid
  - post-api-v1-quotes-quoteid-locations
  - post-api-v3-quotes-quoteid-locations-locationid-buildings
  - patch-api-v2-quotes-quoteid-locations-locationid-buildings-buildingid
  - get-api-v3-reference-resources-class-codes-state
  - get-api-v1-quotes-quoteid-underwriting-questions
  - post-api-v1-quotes-quoteid-underwriting-questions
  - post-api-v1-quotes-quoteid-loss-history
  - post-api-v1-quotes-quoteid-prior-carrier
  - post-api-v2-quotes-quoteid-additional-insured
  - get-api-v2-quotes-quoteid-rate
  - get-api-v1-quotes-quoteid-eligibility
  - get-api-v1-quotes-quoteid-proposal
  - get-api-v1-quotes-quoteid-payment-plans
  - post-api-v1-quotes-quoteid-payment-plans
  - post-api-v1-quotes-quoteid-terrorism-statement
  - post-api-v1-terms-of-agreement-quoteid
  - post-api-v1-quotes-quoteid-bind-agent-contact-agentcontactid
  - get-api-v1-policies-policy-binder
  - delete-api-v1-quotes-quoteid-additional-insured-additionalinsuredid
---

# AmTrust Businessowners Policy: quote to bind

Base URL: `https://gateway.amtrustgroup.com/digital-bop`. Auth is identical to every other AmTrust API:
`subscriber_id` header **and** `Authorization: Bearer <token>` from
`https://auth.amtrustgroup.com/AuthServer/OpenIDConnect/Token` (4-hour lifetime).

## 1. Clear the insured first

`post-api-v1-insured-clearance` checks the prospective insured against AmTrust's existing book. Run it
before you build a quote — clearance failures are cheap to discover and expensive to hit at bind.

`get-api-v1-agent-contacts` gives you the `agentContactId` the bind will need.

## 2. Look up eligible class codes for the state

`get-api-v3-reference-resources-class-codes-state` (v3 is current; v1 and v2 variants exist and are
older). Class codes drive both eligibility and the underwriting question set.

## 3. Create the rated quote

`post-api-v1-quotes-rate` (`POST /api/v1/quotes/rate`) creates and rates the quote in one call, and
returns the `quoteId`. Read it back with `get-api-v3-quotes-quoteid`.

## 4. Locations and buildings

BOP is a property product, so the location/building tree is where most of the work is.

- `post-api-v1-quotes-quoteid-locations` — add a location.
- `post-api-v3-quotes-quoteid-locations-locationid-buildings` — add a building at that location (v3 is
  current; v2 exists).
- `patch-api-v2-quotes-quoteid-locations-locationid-buildings-buildingid` — amend a building.

## 5. Underwriting

- `get-api-v1-quotes-quoteid-underwriting-questions` — the question set for this risk.
- `post-api-v1-quotes-quoteid-underwriting-questions` — answers.
- `post-api-v1-quotes-quoteid-loss-history` — prior claims.
- `post-api-v1-quotes-quoteid-prior-carrier` — incumbent carrier.
- `post-api-v2-quotes-quoteid-additional-insured` — additional insureds
  (`delete-api-v1-quotes-quoteid-additional-insured-additionalinsuredid` removes one).

## 6. Rate, check, propose

- `get-api-v2-quotes-quoteid-rate` — current premium.
- `get-api-v1-quotes-quoteid-eligibility` — the gate.
- `get-api-v1-quotes-quoteid-proposal` — the client-facing proposal document.

## 7. Payment, terrorism statement, terms

- `get-api-v1-quotes-quoteid-payment-plans` then `post-api-v1-quotes-quoteid-payment-plans`.
- `post-api-v1-quotes-quoteid-terrorism-statement` — TRIA acknowledgement. Required.
- `post-api-v1-terms-of-agreement-quoteid` — terms acknowledgement.

## 8. Bind and retrieve the binder

`post-api-v1-quotes-quoteid-bind-agent-contact-agentcontactid`, then
`get-api-v1-policies-policy-binder`.

There is **no idempotency key on the bind**. If a bind response is lost, re-read the quote before
retrying — never blind-retry.

Post-bind billing lives on the policy: `get-api-v1-policies-policyid-balance-summary`,
`post-api-v1-policies-policyid-payment`, and the direct-debit pair
`post-api-v1-policies-policy-payment-plans-direct-debit` plus
`post-api-v1-policies-policy-paymentplans-directdebit-documents` for the EFT form or voided check.

## Errors

Same shared envelope and the same codes as the rest of the estate — 400 validation, 401 token or
missing `subscriber_id`, 403 entitlement, 404 not found. Log `CorrelationID` from every response.
