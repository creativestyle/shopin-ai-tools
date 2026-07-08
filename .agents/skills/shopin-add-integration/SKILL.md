---
name: shopin-add-integration
description: Add a new data source adapter connecting an external API to the BFF via shared contract interfaces. Use when integrating a new e-commerce, CMS, or service provider.
---

# Add Integration

Add a new data source adapter that connects an external API to the BFF via the shared contract interfaces.

## Process

Scale process to the size and risk of the change — pick the lightest tier that fits, and escalate if scope grows mid-task:

- **Trivial** (rename, copy tweak, single prop, obvious one-liner, or following an existing pattern verbatim): just make the change and run the quality checks. No spec, plan, or approval gate.
- **Small / low-risk** (a self-contained addition with a clear shape): post a short proposal in chat (what + which files + acceptance criteria), get one approval, implement, then verify.
- **Non-trivial** (cross-cutting, ambiguous, many files, or real risk to existing behavior — most new integrations): use the full staged flow — Discover → Spec (get approval) → Plan (get approval) → Execute. Save artifacts to `.agents/output/<skill-name>/<summary>_<date>_{spec,plan}.md`.

When unsure, pick the lower tier and escalate if the change turns out bigger than it looked.

## When to use

When connecting a new external API (e-commerce platform, CMS, payment provider, or other service) as a pluggable data source behind the BFF.

## Discovery checklist

Ask these questions during the Discover stage:

- What external API/service is being integrated?
- Which contract interfaces does it need to implement? See `core/contracts/src/core/data-source-interfaces.ts` — product, product-search, product-collection, cart, customer, auth, content/page, navigation, order, store-config, wishlist, etc.
- What authentication does the external API require? (API keys, OAuth, tokens)
- What is the API format? (REST, GraphQL, SDK)
- Are there existing integrations with a similar pattern to follow?
- What data mapping is needed between the external API's format and the shared contract types?
- What environment variables are required?

## Key paths

- `integrations/` — all integration packages
- `integrations/commercetools-api/` — reference implementation (REST/SDK pattern)
- `integrations/contentful-api/` — reference implementation (GraphQL pattern)
- `integrations/mock-api/` — reference implementation (mock pattern)
- `core/contracts/` — target types all integrations must map to
- `apps/bff/src/` — DataSourceFactory registration

## Conventions

### Package structure
```
integrations/<provider>-api/
  package.json                      — @integrations/<provider>-api
  src/
    <provider>-api.module.ts        — main NestJS module
    <provider>-service-provider.ts  — registers services
    index.ts                        — package entry point
    interfaces.ts                   — provider-specific interfaces
    client/                         — API client module + services
    services/                       — service implementations
    mappers/                        — external format → contract types
    schemas/                        — provider response schemas
    helpers/                        — utility functions
```

File names are fully hyphenated and include the package suffix (e.g. `commercetools-api.module.ts`, `commercetools-service-provider.ts`). Client configuration lives in the `client/` directory, not a single config file.

### Contracts
- Every integration must implement the contract interfaces from `core/contracts`
- All external data must be mapped to shared types — never leak provider-specific shapes
- If the domain requires new contract types, add them to `core/contracts` first and run `npm run setup`

### Isolation
- Avoid cross-integration imports — keep each integration self-contained. The one sanctioned exception is `commercetools-auth`, which builds on `@integrations/commercetools-api`; don't add new cross-integration dependencies without a similarly tight coupling reason
- Keep provider-specific logic inside the integration package

### Registration
- Register the new integration in the BFF's `DataSourceFactory`
- Add required environment variables to `.env.example`

## References

See [references/examples.md](references/examples.md) for existing integrations to study before creating a new one.

## Quality checks

- `npm run check-types`
- `npm run test`
- `npm run lint`
- End-to-end verification via BFF with the new data source selected
