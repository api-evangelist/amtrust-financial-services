---
name: amtrust-wc-quote-to-bind
description: Take a small-business workers' compensation risk from class-code eligibility check through
  quote creation, rating and eligibility review to a bound AmTrust policy.
api: amtrust-financial-services:amtrust-financial-services-digital-wc-api
generated: '2026-09-02'
method: generated
source: openapi/amtrust-financial-services-digital-wc-api-openapi.json (harvested verbatim from AmTrust
  Azure API Management, 2026-09-02)
base_url: https://gateway.amtrustgroup.com/DigitalAPI
operations:
  - get-api-v1-state-classes-eligibility-state
  - get-api-v1-state-classes-eligibility-state-classcode
  - get-api-v1-quotes-agent-contacts
  - post-api-v2-quotes
  - post-api-v2-quotes-quoteid-classcodes
  - put-api-v2-quotes-quoteid-classcodes
  - get-api-v1-quotes-quoteid-classcodes
  - post-api-v2-quotes-quoteid-additional-information
  - get-api-v1-quotes-quoteid-eligibility
  - get-api-v2-quotes-quoteid
  - get-api-v2-quotes-quoteid-paymentplans
  - post-api-v2-quotes-quoteid-paymentplans
  - get-api-v1-terms-of-agreement
  - post-api-v1-quotes-quoteid-terms-of-agreement
  - post-api-v2-quotes-quoteid-bind-agent-contact-agentcontactid
  - post-api-v1-quotes-quoteid-documents
  - delete-api-v1-quotes-quoteid-classcodes-indexid
---

# AmTrust workers' compensation: quote to bind

## Before you start

Every request needs **two** credentials, not one:

1. `subscriber_id: <your APIM subscription key>` — issued to you after AmTrust's partner vetting.
2. `Authorization: Bearer <access token>` — from
   `POST https://auth.amtrustgroup.com/AuthServer/OpenIDConnect/Token`,
   `Content-Type: application/x-www-form-urlencoded`,
   `grant_type=client_credentials&scope=openid profile`.

Tokens live **4 hours** and are reusable inside that window. Cache the token; do not mint one per call.
The OpenAPI document declares only the subscription key, so a generated client will 401 until you add
the bearer header yourself.

Base URL: `https://gateway.amtrustgroup.com/DigitalAPI`.

## 1. Check the risk is writable before you build anything

Class-code eligibility is state-specific and it is the cheapest possible rejection.

- `get-api-v1-state-classes-eligibility-state` — every class code writable in a state.
- `get-api-v1-state-classes-eligibility-state-classcode` — one specific class code in one state.

Class codes are **NCCI codes**: four numeric characters in every state except `DE` and `PA`. If the
class code is not returned for the state, stop here; nothing downstream will succeed.

## 2. Resolve the producer of record

`get-api-v1-quotes-agent-contacts` returns the agent contact IDs, names and emails tied to your
credentials. Keep the `agentContactId` — **the bind operation is addressed by agent contact**, and you
cannot bind without one.

## 3. Create the quote

`post-api-v2-quotes` (`POST /api/v2/quotes`) creates the quote and returns a `quoteId`.

Use v2, not v1. Versions are mixed per operation in this API and v2 is the current quote-creation path.

## 4. Build up the risk

- `post-api-v2-quotes-quoteid-classcodes` — add class codes with payroll exposure.
- `put-api-v2-quotes-quoteid-classcodes` — replace the class-code set wholesale.
- `get-api-v1-quotes-quoteid-classcodes` — read back what is saved.
- `post-api-v2-quotes-quoteid-additional-information` — officers, insured locations, partners.

Everything you add here is reversible before bind — `delete-api-v1-quotes-quoteid-classcodes-indexid`
and the sibling delete operations remove officers, partners, third-party notices and voluntary
compensation. Use that rather than starting a new quote.

## 5. Check eligibility, then read the rated quote

`get-api-v1-quotes-quoteid-eligibility` returns the quote's current eligibility and status. Treat this
as the gate: it is the API's own answer to "will this bind?".

`get-api-v2-quotes-quoteid` returns the rated quote with premium.

## 6. Payment plan and terms

- `get-api-v2-quotes-quoteid-paymentplans` then `post-api-v2-quotes-quoteid-paymentplans`.
- `get-api-v1-terms-of-agreement` then `post-api-v1-quotes-quoteid-terms-of-agreement` to record the
  acknowledgement.

## 7. Bind

`post-api-v2-quotes-quoteid-bind-agent-contact-agentcontactid`
(`POST /api/v2/quotes/{quoteId}/bind/agent-contact/{agentContactId}`).

**This is the one irreversible step in the flow and there is no idempotency key.** AmTrust publishes no
`Idempotency-Key` header on any operation. Before you retry a bind whose response you did not receive,
re-read the quote with `get-api-v2-quotes-quoteid` and check its status. Do not blind-retry a bind.

Undoing a bind is a separate, heavier path — a `cancellation` endorsement via the policy endorsements
operation, and reinstatement afterwards
(`post-api-v1-specialty-program-policies-policyid-reinstatement`). AmTrust states no time window for
either, so do not assume one exists.

## 8. Documents

`post-api-v1-quotes-quoteid-documents` for quote documents; `get-api-v1-policies-policy-documents` and
`get-api-v1-policies-policy-available-documents` once the policy exists.

## Error handling

Responses are wrapped in a shared envelope: `{ ID, Timestamp, CorrelationID, HttpStatus, Messages[],
Errors[], IsOk, Paging }`. Errors arrive in `Errors[]` as `{ MessageCode, Title, Detail, Reference }`.

- **400** — validation. Read `Errors[].Detail`, fix the named fields, retry.
- **401** — expired token *or* missing `subscriber_id`. Check both before re-authenticating.
- **403** — your subscription is not entitled to this API.
- **404** — bad identifier, or you hit the gateway rather than an operation.
- **424** — an internal AmTrust dependency failed. **Retryable**; back off and retry.
- No **429** is documented anywhere and the gateway emits no rate-limit headers, so implement your own
  conservative pacing.

Always log `CorrelationID` — it is what AmTrust support asks for. Note there is no request-side
correlation header, so a request that never returns cannot be traced by you.
