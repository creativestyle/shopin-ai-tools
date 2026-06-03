---
name: shopin-add-component
description: Add a new UI component to the presentation app with Storybook story, unit test, and translations. Use when creating shared or feature-specific React components.
---

# Add Component

Add a new UI component to the presentation app, including Storybook story, unit test, and translations.

## Process

Scale process to the size and risk of the change — pick the lightest tier that fits, and escalate if scope grows mid-task:

- **Trivial** (rename, copy tweak, single prop, obvious one-liner, or following an existing pattern verbatim): just make the change and run the quality checks. No spec, plan, or approval gate.
- **Small / low-risk** (a self-contained addition with a clear shape — most single-component work): post a short proposal in chat (what + which files + acceptance criteria), get one approval, implement, then verify.
- **Non-trivial** (cross-cutting, ambiguous, many files, or real risk to existing behavior): use the full staged flow — Discover → Spec (get approval) → Plan (get approval) → Execute. Save artifacts to `.agents/output/<skill-name>/<summary>_<date>_{spec,plan}.md`.

When unsure, pick the lower tier and escalate if the change turns out bigger than it looked.

## When to use

When creating a new reusable UI component — either shared (`components/ui/` or `components/layout/`) or feature-specific (`features/<domain>/components/`).

## Discovery checklist

Ask these questions during the Discover stage:

- What is the component's purpose and where will it be used?
- Is it shared (used across features) or feature-specific?
- What variants/states does it need? (sizes, colors, disabled, loading, error)
- Does it wrap a Radix UI primitive or is it built from scratch?
- Does it need to be a Client Component or can it be a Server Component?
- Does it need translations? If so, which keys?
- Are there existing components to extend or compose with?

## Key paths

- `apps/presentation/components/ui/` — shared UI components
- `apps/presentation/components/layout/` — layout components
- `apps/presentation/features/<domain>/components/` — feature-specific components
- `apps/storybook/src/stories/` — Storybook stories
- `core/i18n/` — translation files

## Conventions

- Use `cva` (class-variance-authority) for variant definitions
- Use Radix UI primitives for interactive behavior (dialogs, dropdowns, tooltips, etc.)
- Use Tailwind design tokens — no hardcoded color or spacing values
- Export a strict TypeScript props interface
- Component file name matches the component name in kebab-case

## Translations

If the component displays user-facing text:
- Add keys to the relevant JSON file in `core/i18n/`
- Organize keys by feature domain
- Server components: use `getTranslations` from `next-intl/server`
- Client components: use `useTranslations` hook

## Storybook

- Create a story in `apps/storybook/src/stories/`
- Cover all variants and edge cases
- Verify a11y with the accessibility addon

## Testing

- Add component tests under a nearby `__tests__/` directory.
- Use `<component-name>.test.tsx` naming (for example, `button.test.tsx`).
- Prefer Testing Library patterns already used in `apps/presentation/components/ui/__tests__/`.

## References

See [references/examples.md](references/examples.md) for existing components to study before creating a new one.

## Quality checks

- `npm run check-types`
- `npm run test -w @apps/presentation`
- `npm run lint`
- `npm run build -w @apps/storybook` (story compiles)
