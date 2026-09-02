---
name: amtrust-cyber-quote-to-bind
description: Quote, underwrite and bind AmTrust cyber insurance, including underwriter submission for
  risks that fall outside straight-through appetite.
api: amtrust-financial-services:amtrust-financial-services-digital-cyber-api
generated: '2026-09-02'
method: generated
source: openapi/amtrust-financial-services-digital-cyber-api-openapi.json (harvested verbatim from
  AmTrust Azure API Management, 2026-09-02)
base_url: https://gateway.amtrustgroup.com/digital-cyber
operations:
  - get-api-v1-agent-contacts
  - post-api-v1-insured-clearance
  - get-api-v2-reference-resources-class-codes-state-state
  - get-api-v1-reference-resources-limits-retention
  - post-api-v1-quotes
  - get-api-v1-quotes-quoteid
  - patch-api-v1-quotes-quoteid
  - get-api-v2-reference-resources-underwriting-questions
  - get-api-v2-quotes-quoteid-underwriting-questions
  - post-api-v2-quotes-quoteid-underwriting-questions
  - post-api-v2-quotes-quoteid-underwriter-submission
  - get-api-v1-quotes-quoteid-proposal
  - get-api-v1-quotes-quoteid-payment-plans
  - post-api-v1-quotes-quoteid-payment-plans
  - post-api-v1-quotes-quoteid-terrorism-statement
  - post-api-v1-quotes-quoteid-terms-of-agreement
  - post-api-v2-quotes-quoteid-bind
  - get-api-v1-policies-policynum-document
---

# AmTrust cyber: quote, underwrite, bind

Base URL: `https://gateway.amtrustgroup.com/digital-cyber`. Auth as elsewhere — `subscriber_id` header
plus a 4-hour bearer token from `https://auth.amtrustgroup.com/AuthServer/OpenIDConnect/Token`.

## 1. Clearance and reference data

- `post-api-v1-insured-clearance` — clear the insured.
- `get-api-v2-reference-resources-class-codes-state-state` — eligible class codes by state.
- `get-api-v1-reference-resources-limits-retention` — the limit and retention options you may offer.
  Read this before quoting; cyber limits are constrained and guessing produces validation errors.

## 2. Create a rated quote

`post-api-v1-quotes` (`POST /api/v1/quotes`, "Create a rated Quote") returns the `quoteId` with premium
already attached. Amend with `patch-api-v1-quotes-quoteid` (targeted) or `put-api-v1-quotes-quoteid`
(whole body); read with `get-api-v1-quotes-quoteid`.

## 3. Underwriting questions

- `get-api-v2-reference-resources-underwriting-questions` — the catalogue.
- `get-api-v2-quotes-quoteid-underwriting-questions` — what this quote needs.
- `post-api-v2-quotes-quoteid-underwriting-questions` — answers.

## 4. If the risk needs a human

`post-api-v2-quotes-quoteid-underwriter-submission` refers the quote to an AmTrust underwriter. This is
the branch that separates cyber from the other lines: not every cyber risk binds straight through, and
an agent workflow should handle the referral path rather than treating it as a failure.

## 5. Proposal, payment, acknowledgements

- `get-api-v1-quotes-quoteid-proposal`
- `get-api-v1-quotes-quoteid-payment-plans` then `post-api-v1-quotes-quoteid-payment-plans`
- `post-api-v1-quotes-quoteid-terrorism-statement`
- `post-api-v1-quotes-quoteid-terms-of-agreement`

## 6. Bind

`post-api-v2-quotes-quoteid-bind` (`POST /api/v2/quotes/{quoteId}/bind`). Note this API binds by quote
alone — unlike WC and BOP, there is no agent-contact path segment here.

No idempotency key. Re-read the quote before any bind retry.

Retrieve the policy document with `get-api-v1-policies-policynum-document`. Direct debit is set up
post-bind via `post-api-v1-policies-policy-paymentplans-directdebit` and its Documents sibling.

## Errors

Cyber is the only AmTrust API that documents **500** alongside the usual 400/401/403/404 set. Treat 500
and 424 as retryable with backoff; 400 as a data fix; 401 as token-or-subscription-key; 403 as
entitlement. Log `CorrelationID`.
