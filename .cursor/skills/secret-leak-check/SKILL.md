---
name: secret-leak-check
description: Scan staged or recent changes for accidentally committed secrets, API keys, tokens, or .env values before committing or opening a PR. Use before running git commit, before opening a PR, or after any multi-file agent edit that touched config, env handling, or API clients.
---

# Secret leak check

## When to use this
- Right before `git commit`, especially after an agent made changes across several files
- Right before opening a PR
- After touching anything related to API clients, auth, config loading, or `.env*` files

## Steps
1. Run `git status` and confirm no `.env`, `.env.local`, or similar file is staged. If one is,
   unstage it immediately — don't just "remember not to commit it," actually `git restore --staged`.
2. Run `git diff --staged` (or `git diff` for uncommitted work) and scan for:
   - Hardcoded API keys, tokens, or client secrets (long random-looking strings assigned to a
     variable, especially near words like `key`, `token`, `secret`, `password`, `auth`)
   - URLs with embedded credentials (`https://user:pass@...`)
   - Private key blocks (`-----BEGIN PRIVATE KEY-----` or similar)
   - `console.log` / debug output of anything that looks like a credential, session token, or
     full user object that might contain one
3. If something suspicious is found: stop, don't commit. Ask whether it's a real secret or a
   fixture/mock value used only in tests. Real secrets never get committed, even to fix it "for
   now" — move it to an environment variable and confirm it's in `.gitignore`.
4. Prefer environment variables loaded via the project's existing config pattern over introducing
   a new way to read secrets.
5. For anything that touches third-party API credentials specifically, double check it isn't
   being sent to the client bundle (no `NEXT_PUBLIC_` prefix on anything secret — that exposes it
   in the browser).

## Note on relying on this alone
A skill is a checklist the agent follows when it remembers to — it's not a guarantee. For real
protection, pair this with a deterministic pre-commit hook (`gitleaks`, `truffleHog`, or similar)
that blocks the commit outright regardless of whether anyone remembered to check.
