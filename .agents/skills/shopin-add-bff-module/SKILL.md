---
name: shopin-add-bff-module
description: Add a new NestJS module to the BFF with contracts, controller, service, validation, and tests. Use when exposing a new API endpoint or domain.
---

# Add BFF Module

Add a new NestJS module to the BFF, including contracts, controller, service, validation, and tests.

## Process

Scale process to the size and risk of the change — pick the lightest tier that fits, and escalate if scope grows mid-task:

- **Trivial** (rename, copy tweak, single prop, obvious one-liner, or following an existing pattern verbatim): just make the change and run the quality checks. No spec, plan, or approval gate.
- **Small / low-risk** (a self-contained addition with a clear shape): post a short proposal in chat (what + which files + acceptance criteria), get one approval, implement, then verify.
- **Non-trivial** (cross-cutting, ambiguous, many files, or real risk to existing behavior): use the full staged flow — Discover → Spec (get approval) → Plan (get approval) → Execute. Save artifacts to `.agents/output/<skill-name>/<summary>_<date>_{spec,plan}.md`.

When unsure, pick the lower tier and escalate if the change turns out bigger than it looked.

## When to use

When exposing a new API endpoint or domain through the BFF (e.g., a new resource like wishlists, reviews, or a new operation on an existing domain).

## Discovery checklist

Ask these questions during the Discover stage:

- What domain/resource does this module serve?
- What endpoints are needed? (HTTP methods, paths, request/response shapes)
- Does it need data from an existing integration, or does it require a new one?
- Does it need authentication/authorization?
- Are there existing BFF modules with a similar pattern to follow?
- What validation rules apply to the inputs?
- Does it need to orchestrate data from multiple data sources?

## Key paths

- `apps/bff/src/` — BFF modules, controllers, services
- `apps/bff/src/app.module.ts` — module registration
- `core/contracts/` — Zod schemas and TypeScript types

## Conventions

### Module structure
Every BFF module follows this pattern:
```
apps/bff/src/features/<domain>/
  <domain>.module.ts       — module definition
  <domain>.controller.ts   — route handlers with Swagger decorators
  <domain>.service.ts      — business logic, calls data source via DataSourceFactory
  <domain>.service.spec.ts — unit tests (service-focused)
```

- Prefer `features/<domain>/` to match existing BFF domain organization.
- Keep unit tests aligned with existing patterns (for example, `product.service.spec.ts`).
- Not every existing domain has all four files (some lack a spec; `csrf/` diverges with guards/decorators) — `features/product/` is the complete reference.

### Contracts
- Define Zod schemas in `core/contracts/` before implementing the module
- Naming: `*Schema` suffix for Zod schemas, `*Response`/`*Request` for API types
- Infer types: `type MyType = z.infer<typeof MyTypeSchema>`
- Run `npm run setup` after adding/changing contracts — it runs each workspace's `setup` task via Turbo, which rebuilds `@core/contracts` (and any codegen)

### Validation
- Validate all inputs against `core/contracts` schemas
- Never leak internal error details to the client

### Security
- BFF is stateless — no server-side sessions
- HTTP-only cookies for tokens
- CSRF double-submit pattern for state-changing operations

## References

See [references/examples.md](references/examples.md) for existing BFF modules to study before creating a new one.

## Quality checks

- `npm run check-types`
- `npm run test -w @apps/bff`
- `npm run lint`
- Verify Swagger docs at `http://localhost:4000/api` (the `bff` global prefix applies to API routes, not the Swagger UI)
