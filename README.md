# AI-driven codebase bootstrap

Starter files for AI-assisted development. Copy them into a new project so agents and humans share the same architecture, testing, and workflow rules.

## What this is

This repo is **not** a runnable application. It ships tooling config (lint, format, hooks, CI)
and conventions, but no framework dependencies or application source yet. The layout is
root-level (`app/`, `features/`, `components/`, ...) — there is no `src/` directory.

It **is** a bootstrap kit of:

- Shared agent instructions
- Architecture and testing docs
- An ADR template
- Scoped Cursor rules
- Project Cursor skills (task workflows)

Use it to boot a new codebase with conventions that coding agents can follow from day one.

## What's included

| Path                                         | Role                                                                                  |
| -------------------------------------------- | ------------------------------------------------------------------------------------- |
| [AGENTS.md](AGENTS.md)                       | Canonical agent instructions (stack, commands, non-negotiable rules, file boundaries) |
| [CLAUDE.md](CLAUDE.md)                       | Claude Code entry that imports `AGENTS.md`                                            |
| [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) | Folder layout, data-flow, and feature boundaries                                      |
| [docs/TESTING.md](docs/TESTING.md)           | Test pyramid, coverage expectations, mocking conventions                              |
| [docs/ADRs/](docs/ADRs/)                     | Decision log template — record new patterns before implementing them                  |
| [.cursor/rules/](.cursor/rules/)             | Scoped Cursor rules (`.mdc`) for components and tests                                 |
| [.cursor/skills/](.cursor/skills/)           | Project skills (`.md`) for repeatable agent workflows                                 |

## How to use

1. **Adopt the files** — copy this tree into a new app, or scaffold your app on top of this repo.
2. **Fill placeholders** — in `AGENTS.md` (product one-liner, styling, package manager) and `docs/ARCHITECTURE.md` (shared UI state choice).
3. **Keep one source of truth** — put product-specific agent rules in `AGENTS.md`. Tool-specific files (e.g. `CLAUDE.md`) should import or point there, not duplicate rules.

## Where to go next

- [Architecture](docs/ARCHITECTURE.md)
- [Testing strategy](docs/TESTING.md)
- [ADRs](docs/ADRs/)
