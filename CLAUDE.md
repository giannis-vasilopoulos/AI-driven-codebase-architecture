# CLAUDE.md

@AGENTS.md

This project's canonical instructions live in AGENTS.md (shared with Cursor and other tools).
The import line above pulls them into every Claude Code session automatically — don't duplicate
content here, extend it.

## Claude Code specific

- Use plan mode for anything touching more than two files or introducing a new pattern.
- If you find yourself correcting Claude on the same thing twice in a session, that's a signal
  to add it to AGENTS.md, not just remember it for this session.

## Personal layer

Individual workflow preferences (verbosity, commit message habits, etc.) belong in
CLAUDE.local.md — gitignored, never shared with the team.

Keep this file itself short. If it starts accumulating project rules, move them to AGENTS.md.
