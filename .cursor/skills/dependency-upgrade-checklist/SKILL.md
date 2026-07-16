---
name: dependency-upgrade-checklist
description: Checklist for safely upgrading a dependency, especially major versions of Next.js, React, TypeScript, or Apollo Client. Use when bumping a package version, running npm/pnpm update or upgrade, or when asked to update a library.
---

# Dependency upgrade checklist

## When to use this

- Bumping any package to a new major version
- Bumping multiple packages at once ("update everything")
- Upgrading a core piece of the stack: Next.js, React, TypeScript, Apollo Client, React Query, ESLint

## Steps

1. Check the current version and the target version. If it's a major bump, read the changelog /
   migration guide for that specific version jump — don't assume "it's probably fine."
2. Upgrade one package (or one tightly-coupled group, e.g. `@apollo/client` + its codegen plugins)
   at a time. Never bump the whole dependency tree in one commit — if something breaks, you want
   to know which upgrade caused it.
3. After each upgrade: run `pnpm install`, then check for peer dependency warnings. Don't ignore
   them — resolve or consciously accept them.
4. Run the full definition-of-done: `pnpm typecheck`, `pnpm lint`, `pnpm test`,
   `pnpm build`. A version bump that "just installs" without running these isn't done.
5. Check for newly-deprecated APIs the linter or compiler flags, even if the build still passes —
   deprecated today is broken next major.
6. For framework upgrades (Next.js, React): check bundle size before/after
   (`pnpm build` output) and skim the migration guide's "breaking changes" section against
   your actual usage (e.g. removed APIs, changed defaults for Server Components / caching).
7. Commit each package upgrade separately with a clear message (`chore: upgrade apollo-client to
3.9`). Separate commits make it trivial to bisect or revert one upgrade without touching others.
8. If a major upgrade is genuinely risky or changes an architectural pattern (e.g. Next.js
   App Router caching semantics), write an ADR per docs/adr/ before merging — not after something
   breaks in production.

## What not to do

- Don't run a blanket `pnpm update` / `pnpx npm-check-updates -u` and commit the result as one diff.
- Don't skip the changelog because "semver says it's a minor" — mistakes happen, and frameworks
  in particular sometimes ship behavior changes in minors.
- Don't upgrade a dependency to unblock an unrelated task without flagging it as a separate,
  reviewable change.
