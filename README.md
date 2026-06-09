# SHOPin AI Tools

Agent skills for working in the SHOPin codebase — adding components, features, BFF
modules, and integrations the way this project expects.

These are **project-specific** skills: they encode SHOPin's paths, conventions, and
quality checks. They're meant to be used inside (or alongside) the SHOPin monorepo,
not as general-purpose skills.

## Install

These skills follow the open [Agent Skills](https://skills.sh) layout, so you can
install them into any project with the [`skills` CLI](https://github.com/vercel-labs/skills) —
no separate package to publish, and nothing to install globally (`npx` runs it on demand):

```bash
# install all skills into the current project
npx skills add creativestyle/shopin-ai-tools
```

The CLI detects your agent and installs to the right place — `.claude/skills/` for
Claude Code, `.agents/skills/` for Cursor, Codex, GitHub Copilot, Gemini CLI, and ~70 others.

Install a single skill with the `--skill` flag:

```bash
npx skills add creativestyle/shopin-ai-tools --skill shopin-add-component
```

Other useful commands: `skills list`, `skills update`, `skills remove`.

## Using these in the SHOPin monorepo

These are companions to the [`creativestyle/shopin`](https://github.com/creativestyle/shopin)
storefront accelerator. Installing them is **optional and per-developer** — the monorepo
does not require them. Whoever wants them runs the command above inside their checkout;
whoever doesn't, does nothing.

## Skills

| Skill | Use when |
|-------|----------|
| `shopin-add-component` | Adding a UI component (Storybook story, test, translations). |
| `shopin-add-bff-module` | Exposing a new API endpoint/domain through the BFF. |
| `shopin-add-integration` | Connecting a new external data source behind the BFF. |
| `shopin-add-feature` | A full-stack feature spanning frontend, BFF, contracts, and integrations. |

Each skill is a self-contained folder under [`.agents/skills/`](.agents/skills/):

```
.agents/skills/<name>/
  SKILL.md         — domain guidance + an inlined Process section
  references/      — examples to study before building
```

## Workflow

Every skill inlines a compact, tiered process: **trivial** changes just get done,
**small/low-risk** changes get one approval, and **non-trivial** work goes through a
staged spec → plan → execute flow with approval gates.

## Contributing

`.agents/skills/` is the source of truth. Edit the `SKILL.md` files directly —
each is self-contained, including its `## Process` section. When you change the
shared process, update the `Process` section in every skill so they stay
consistent. Skills are discovered automatically by the `skills` CLI from
`.agents/skills/` — no manifest to maintain.

## License

[MIT](LICENSE) © creativestyle GmbH

---

Brought to life by<br/>
<a href="https://creativestyle.de" target="_blank" rel="noopener noreferrer"><picture><source media="(prefers-color-scheme: dark)" srcset="https://creativestyle.com/images/logos/creativestyle-logo-white-1x.png"><source media="(prefers-color-scheme: light)" srcset="https://creativestyle.com/images/logos/creativestyle-logo-1x.png"><img src="https://creativestyle.com/images/logos/creativestyle-logo-1x.png" width="211px"></picture></a>
