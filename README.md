# AmTrust Financial Services (amtrust-financial-services)
AmTrust Financial Services is a multinational specialty property and casualty insurer focused on small to mid-sized businesses. AmTrust provides APIs that enable insurance agents, brokers, and technology partners to review appetite, generate quotes, and bind policies programmatically. The API platform processes over 12 million API calls daily with 99.68% availability and supports workers' compensation, business owners' policies, general liability, and other commercial insurance products across 300+ eligible class codes.

**URL:** [Visit APIs.json URL](https://raw.githubusercontent.com/api-evangelist/amtrust-financial-services/refs/heads/main/apis.yml)

**Run:** [Capabilities Using Naftiko](https://github.com/naftiko/fleet?utm_source=api-evangelist&utm_medium=readme&utm_campaign=company-api-evangelist&utm_content=repo)

## Tags:

 - Commercial Insurance, Insurance, Property And Casualty, Small Business, Workers Compensation

## Timestamps

- **Modified:** 2026-04-19

## APIs

### AmTrust Commercial Lines API
The AmTrust Commercial Lines API enables insurance agents, brokers, and technology partners to programmatically review coverage appetite, generate quotes, and bind commercial lines policies. It supports workers' compensation, business owners' policy, general liability, and commercial package products across 300+ eligible class codes.

**Human URL:** [https://amtrustfinancial.com/api](https://amtrustfinancial.com/api)

#### Tags:

 - Commercial Lines, Commercial Package, General Liability, Insurance, Workers Compensation

#### Properties

- [Documentation](https://amtrustfinancial.com/api)
- [Portal](https://utapiportal.amtrustgroup.com)
- [Authentication](https://utapiportal.amtrustgroup.com/authentication)
- [OpenAPI](openapi/amtrust-financial-services-commercial-lines-api.yaml)

## Common Properties

- [Portal](https://amtrustfinancial.com)
- [DeveloperPortal](https://utapiportal.amtrustgroup.com)
- [Documentation](https://amtrustfinancial.com/api)
- [Authentication](https://utapiportal.amtrustgroup.com/authentication)
- [SignUp](https://amtrustfinancial.com/api)
- [Support](https://amtrustfinancial.com/contact-us)
- [TermsOfService](https://amtrustfinancial.com/terms-of-use)
- [PrivacyPolicy](https://amtrustfinancial.com/privacy-policy)
- [SpectralRules](rules/amtrust-financial-services-spectral-rules.yml)
- [JSONSchema](json-schema/amtrust-financial-services-quote-request-schema.json)
- [JSONSchema](json-schema/amtrust-financial-services-quote-response-schema.json)
- [JSONSchema](json-schema/amtrust-financial-services-policy-schema.json)
- [JSONLD](json-ld/amtrust-financial-services-context.jsonld)
- [Vocabulary](vocabulary/amtrust-financial-services-vocabulary.yaml)
- [NaftikoCapability](capabilities/amtrust-insurance-quoting-and-binding.yaml)
- [JSONStructure](json-structure/amtrust-financial-services-quote-request-structure.json)

## Features

| Name | Description |
|------|-------------|
| Appetite Check | Review coverage eligibility for specific business classes and risk profiles. |
| Instant Quoting | Generate commercial lines quotes in real time via API. |
| Online Binding | Bind policies programmatically for eligible class codes. |
| 300+ Class Codes | Access over 300 bind-online eligible class codes. |
| OAuth 2.0 Authentication | Token-based authentication with 4-hour access tokens. |
| High Availability | 12 million daily API calls with 99.68% uptime SLA. |

## UseCases

| Name | Description |
|------|-------------|
| Agent Platform Integration | Embed AmTrust quoting and binding in agent management systems. |
| Wholesale Brokerage Automation | Automate workers' compensation submissions from wholesale platforms. |
| AMS Software Integration | Connect agency management software to AmTrust for policy lifecycle management. |
| Workers Compensation Automation | Streamline small business workers' compensation from quote to bind. |

## Integrations

| Name | Description |
|------|-------------|
| Appulate | Workers' compensation digital submission integration. |
| Semsee | Commercial lines quoting platform integration. |
| Tarmika | Commercial lines quoting marketplace integration. |
| IBQ Systems | Commercial lines rating platform integration. |

## Artifacts

Machine-readable API specifications organized by format.

### OpenAPI

- [Amtrust Financial Services Commercial Lines Api](openapi/amtrust-financial-services-commercial-lines-api.yaml)

### JSON Schema

- [Amtrust Financial Services Quote Response Schema](json-schema/amtrust-financial-services-quote-response-schema.json)
- [Amtrust Financial Services Policy Response Schema](json-schema/amtrust-financial-services-policy-response-schema.json)
- [Amtrust Financial Services Appetite Request Schema](json-schema/amtrust-financial-services-appetite-request-schema.json)
- [Amtrust Financial Services Bind Request Schema](json-schema/amtrust-financial-services-bind-request-schema.json)
- [Amtrust Financial Services Quote Request Schema](json-schema/amtrust-financial-services-quote-request-schema.json)
- [Amtrust Financial Services Insured Schema](json-schema/amtrust-financial-services-insured-schema.json)
- [Amtrust Financial Services Appetite Response Schema](json-schema/amtrust-financial-services-appetite-response-schema.json)

### JSON Structure

- [Amtrust Financial Services Insured Structure](json-structure/amtrust-financial-services-insured-structure.json)
- [Amtrust Financial Services Appetite Request Structure](json-structure/amtrust-financial-services-appetite-request-structure.json)
- [Amtrust Financial Services Quote Response Structure](json-structure/amtrust-financial-services-quote-response-structure.json)
- [Amtrust Financial Services Policy Response Structure](json-structure/amtrust-financial-services-policy-response-structure.json)
- [Amtrust Financial Services Appetite Response Structure](json-structure/amtrust-financial-services-appetite-response-structure.json)
- [Amtrust Financial Services Bind Request Structure](json-structure/amtrust-financial-services-bind-request-structure.json)
- [Amtrust Financial Services Quote Request Structure](json-structure/amtrust-financial-services-quote-request-structure.json)

### JSON-LD

- [Amtrust Financial Services Context](json-ld/amtrust-financial-services-context.jsonld)

### Examples

- [Amtrust Financial Services Appetite Request Example](examples/amtrust-financial-services-appetite-request-example.json)
- [Amtrust Financial Services Policy Example](examples/amtrust-financial-services-policy-example.json)
- [Amtrust Financial Services Quote Response Example](examples/amtrust-financial-services-quote-response-example.json)
- [Amtrust Financial Services Quote Request Example](examples/amtrust-financial-services-quote-request-example.json)

## Capabilities

Naftiko capabilities organized as shared per-API definitions and workflow capabilities.

### Shared Per-API Definitions

- [Commercial Lines Api](capabilities/shared/commercial-lines-api.yaml)

### Workflow Capabilities

| Workflow | Tools | Description |
|----------|-------|-------------|
| [AmTrust Insurance Quoting and Binding](capabilities/amtrust-insurance-quoting-and-binding.yaml) | 5 | Workflow capability for reviewing appetite, generating quotes, and binding comme... |

## Vocabulary

- [AmTrust Financial Services Vocabulary](vocabulary/amtrust-financial-services-vocabulary.yaml) — Taxonomy covering commercial lines insurance quoting and binding operations

## Rules

- [AmTrust Spectral Rules](rules/amtrust-financial-services-spectral-rules.yml) — Rules enforcing AmTrust API conventions

## Maintainers

**FN:** Kin Lane

**Email:** kin@apievangelist.com
