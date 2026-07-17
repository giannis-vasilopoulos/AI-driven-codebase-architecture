---
name: checkpoint-long-task
description: Persist progress notes for a task that spans multiple sessions or risks context compaction, so it can be resumed accurately. Use for multi-step migrations, large refactors, or any task explicitly expected to take more than one session.
---

# Checkpoint a long-running task

CLAUDE.md and auto memory hold project-wide facts, but they don't track "where was I" on a
specific multi-step task. This skill fills that gap.

## When to use this

- The task is explicitly expected to span multiple sessions (e.g. "migrate all feature folders
  to the new data-fetching pattern")
- The conversation is getting long and context compaction is likely
- You're about to hand off to a fresh session or another person

## Steps

1. Create or update `docs/progress/<task-slug>.md` with:
   - Goal (one sentence)
   - Done so far (bullet list, be specific — file paths, not "some components")
   - Next step (the single next concrete action, not a vague plan)
   - Anything discovered that changes the original plan
   - Any blockers or open questions
2. Commit this file alongside the actual work, not as an afterthought at the end.
3. When resuming: read `docs/progress/<task-slug>.md` first, verify "done so far" against the
   actual codebase (git log, file contents) before trusting it, then continue from "next step."
4. When the task is fully complete, delete the progress file — or fold it into an ADR if the
   task represented a real architectural decision worth keeping a permanent record of.

## Why a file instead of relying on memory

Auto memory captures preferences and corrections, not task state, and isn't guaranteed to
survive a fresh session the way a committed file does. For anything you'd be upset to lose
track of mid-task, write it to disk.
