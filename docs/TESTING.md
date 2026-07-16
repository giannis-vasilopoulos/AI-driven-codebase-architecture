# Testing strategy

## Pyramid
- Unit: Vitest — pure functions, hooks (via renderHook), utilities. Fast, most coverage lives here.
- Integration: Vitest + React Testing Library — components with their real hooks, mocked network layer.
- E2e: Playwright — critical user flows only (auth, checkout, whatever is business-critical).
  Not every page.

## What must have a test
- Any hook with branching logic or side effects
- Any utility in src/lib
- Any component with conditional rendering, form validation, or error states
- Any GraphQL mutation's optimistic response / cache update logic

## What doesn't need one
- Pure presentational components with no logic (a Badge that just renders a label)
- Generated code (src/graphql/generated)
- One-off marketing/static pages

## Mocking conventions
- GraphQL: MockedProvider from @apollo/client/testing, one mock per test case — not a shared
  fixture that silently drifts from the real schema.
- REST: MSW (Mock Service Worker) handlers, colocated under __tests__/mocks.
- Don't mock what you're testing — if a hook wraps Apollo, test the hook through Apollo's mocks,
  not by mocking the hook itself.

## Definition of done
A change isn't done until:
1. `npm run typecheck` passes
2. `npm run lint` passes
3. `npm run test` passes, including new tests for new logic
4. For UI changes touching a critical flow, the relevant Playwright spec still passes

## Note on style rules
Don't ask an AI agent to remember formatting or lint rules — ESLint and Prettier enforce those
deterministically via `npm run lint`. Keep this file about test strategy and coverage
expectations, not code style. Sending an LLM to do a linter's job wastes context and is less
reliable than the linter.
