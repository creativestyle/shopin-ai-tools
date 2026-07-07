---
name: shopin-add-feature
description: Add a new user-facing feature spanning frontend, BFF, contracts, and optionally integrations. Use for full-stack features like wishlists, reviews, or order tracking.
---

# Add Feature

Add a new user-facing feature to the storefront, spanning frontend, BFF, contracts, and optionally integrations.

## Process

Scale process to the size and risk of the change — pick the lightest tier that fits, and escalate if scope grows mid-task:

- **Trivial** (rename, copy tweak, single prop, obvious one-liner, or following an existing pattern verbatim): just make the change and run the quality checks. No spec, plan, or approval gate.
- **Small / low-risk** (a self-contained addition with a clear shape): post a short proposal in chat (what + which files + acceptance criteria), get one approval, implement, then verify.
- **Non-trivial** (cross-cutting, ambiguous, many files, or real risk to existing behavior — most full features land here): use the full staged flow — Discover → Spec (get approval) → Plan (get approval) → Execute. Save artifacts to `.agents/output/<skill-name>/<summary>_<date>_{spec,plan}.md`.

When unsure, pick the lower tier and escalate if the change turns out bigger than it looked.

This is a composite skill. Depending on scope, it may involve other skills in this set:
- `shopin-add-component` — for new UI components
- `shopin-add-bff-module` — for new API endpoints
- `shopin-add-integration` — if a new data source is needed

Reference those skills for domain-specific guidance during each part of the implementation.

## When to use

When building a new user-facing feature that touches multiple layers of the stack (e.g., a wishlist, product reviews, order tracking, loyalty program).

## Discovery checklist

Ask these questions during the Discover stage:

- What is the feature and what user problem does it solve?
- What pages/routes does it need in the storefront?
- What data does it need? Is the data already available via existing BFF endpoints, or are new ones required?
- Does it need a new integration, or can existing integrations provide the data?
- What UI components are needed? Are any existing components reusable?
- Does it need authentication?
- What translations are needed?
- Are there similar existing features to use as a reference?

## Key paths

- `apps/presentation/features/<feature-name>/` — feature directory
- `apps/presentation/app/` — App Router pages/routes
- `apps/bff/src/` — BFF modules
- `core/contracts/` — shared types and schemas
- `core/i18n/` — translations
- `apps/storybook/src/stories/` — component stories

## Conventions

### Feature directory structure
```
apps/presentation/features/<feature-name>/
  <entry>.tsx    — entry points (anything imported from outside the feature)
  components/    — feature-specific UI components
  hooks/         — data fetching, state management
  lib/           — services and feature-specific utilities
```

Small features skip the subdirectories and keep all files at the feature root.

### Boundaries
- ESLint (`no-restricted-imports` in `apps/presentation/eslint.config.mjs`) forbids importing from feature subdirectories — code outside a feature may only import files at the feature root, so anything consumed externally must be (re-)exported from a feature-root entry point
- Shared reusables go in `components/` or `core/`, not in feature dirs
- The feature only talks to the BFF — never import from integrations

### Implementation order
1. Define contracts in `core/contracts/` (if new API data is needed)
2. Add BFF module (if new endpoints are needed)
3. Create UI components
4. Wire up pages/routes in `apps/presentation/app/`
5. Add translations in `core/i18n/`
6. Create Storybook stories for feature components
7. Write tests

## References

See [references/examples.md](references/examples.md) for existing features to study before creating a new one.

## Quality checks

- `npm run check-types`
- `npm run test`
- `npm run lint`
- `npm run build -w @apps/storybook`
- Manual testing in browser and Storybook
