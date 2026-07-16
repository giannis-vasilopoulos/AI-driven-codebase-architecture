---
name: recover-from-failure
description: Playbook for recovering when a change breaks the build, fails tests, or an agent edit needs to be undone safely. Use when tests are red, the build fails after an edit, or you need to roll back AI-made changes without losing unrelated work.
---

# Recover from failure

## When to use this

- `pnpm typecheck`, `pnpm lint`, `pnpm test`, or `pnpm build` starts failing after a change
- An edit needs to be undone but there's other legitimate work-in-progress in the same branch
- You're mid-task and something clearly went wrong

## Steps

1. Stop making further edits. Don't "fix forward" blindly on top of a broken state.
2. Run `git status` and `git diff` to see exactly what changed since the last known-good commit.
3. Isolate: is the failure caused by the most recent edit, or something earlier? Use `git stash`
   to temporarily remove the latest change and re-run the failing command to confirm.
4. If the latest change is the cause and it's small: fix it directly, re-run typecheck/lint/test.
5. If the latest change is the cause and it's large or you're not confident: `git checkout -- <file>`
   for the specific files, not `git reset --hard` (that also wipes unrelated uncommitted work).
6. If you're unsure what "known-good" looks like: check docs/adr/ for whether this touches a
   documented decision, and check the last passing CI run / last green commit on the branch.
7. Once fixed: re-run the full definition-of-done from docs/TESTING.md (typecheck, lint, test,
   build) before continuing — not just the one command that was failing.
8. Never force-push (`git push --force`) to shared branches as part of a recovery. If history
   needs cleaning up, that's a separate, deliberate step — not part of "fixing the build."

## What not to do

- Don't delete and rewrite a whole file to "start clean" when only a few lines broke something —
  that destroys context (comments, other recent changes) that weren't the problem.
- Don't silently revert an ADR-documented decision to make tests pass faster. If the fix requires
  reversing an architectural decision, stop and flag it instead of routing around it.
