# Architecture

## Folder structure

```
app/                    # Next.js App Router routes, layouts, server components
e2e/                    # End-to-end tests with Playwright
features/               # Feature-scoped code: components, hooks, graphql ops, tests
└── <feature>/
    ├── components/
    ├── hooks/
    └── __tests__/
components/              # Shared, feature-agnostic UI components
hooks/                    # Shared hooks not tied to one feature
lib/                       # Framework-agnostic utilities, REST clients
types/                      # Shared types not generated from GraphQL
```

## Data flow: Apollo vs React Query

- REST endpoints → React Query. One hook per endpoint in the feature's hooks/ folder, wrapping fetch/axios.
- Don't mix: a feature that needs both GraphQL and REST data keeps them as separate hooks —
  consuming components compose the two, they don't get merged into one custom hook that hides
  which is which.

## State management boundaries

- Server state (anything from GraphQL/REST): lives in Apollo cache / React Query cache only.
- UI state (modal open, form draft, tab selection): local useState/useReducer, or
  [Zustand / Context — fill in] if shared across a feature.
- Never lift server data into global UI state — re-fetch or read from cache instead.

## Feature boundaries

- Features don't import from each other's internals (features/checkout shouldn't reach into
  features/cart/hooks/useCartItems). Shared logic gets promoted to hooks/, lib/, or
  components/.
- This is enforced by the `boundaries/dependencies` rule in [eslint.config.mjs](../eslint.config.mjs)
  (eslint-plugin-boundaries), not just this doc — the doc explains why, the linter makes it stick.

## Naming

- Components: PascalCase file + folder (Button/Button.tsx)
- Hooks: useX.ts, one hook per file

## When this file goes stale

Update this file when the folder structure or a data-flow rule actually changes. Don't restate
things an AI agent can already see by reading the codebase (exact dependency versions, file
counts) — keep this to decisions and rules that aren't obvious from the files themselves.
