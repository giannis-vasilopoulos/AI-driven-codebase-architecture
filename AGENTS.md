# AGENTS.md

## What this is

[One-line description of the product/app.]

## Stack

- Next.js (App Router) + TypeScript (strict mode)
- Apollo Client for GraphQL, React Query for REST endpoints
- [Styling solution — Tailwind / CSS Modules / etc.]
- Vitest + React Testing Library (unit/integration), Playwright (e2e)
- [package manager: pnpm (recommended)]

## Commands

- See [package.json](package.json) for all commands.

## Architecture

See docs/ARCHITECTURE.md for folder layout and data-flow rules (Apollo vs React Query, feature boundaries). Don't restate that here — read it once per session, not per instruction.

## Non-negotiable rules

1. No `any` without a `// TODO(reason):` comment.
2. GraphQL responses are always typed via codegen — never hand-write response interfaces.
3. Server state (GraphQL or REST) stays in Apollo cache / React Query cache. Never copy it into useState or a global store "for convenience."
4. New hooks, utilities, and non-trivial components need a test. See docs/TESTING.md.
5. A new data-fetching pattern, folder convention, or state-management choice gets an ADR in docs/ADRs/ before it's implemented — not documented after the fact.
6. No new dependency without checking package.json first for something that already does the job.

## File boundaries

- Edit freely: src/features/**, src/components/**, src/hooks/**, src/lib/**, tests/\*\*
- Regenerate, don't hand-edit: src/graphql/generated/\*\*
- Ask first: next.config.js, CI workflows, tsconfig.json
- Never touch: .env\*, /secrets

## Workflow expectations

- For anything touching more than one file or introducing a new pattern: state a short plan first, then implement.
- Run typecheck + lint + test before calling a task complete.
- When there's more than one reasonable architectural approach, ask rather than silently picking one.

## Reference

- Architecture & data flow: docs/ARCHITECTURE.md
- Testing strategy: docs/TESTING.md
- Decision log: docs/ADRs/
