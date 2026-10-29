---
name: new-session
description: Reply 1 of every session in the owner's chat, gate line first. Use at session start, after a compact, or when the owner says "new session", "where are we", "pick up where we left off".
---

# new-session

**When:** the first reply of a session in the owner's chat. Not for `codex exec` calls or subagents: they do their step and report.

**Steps**
1. The hook's line `memory gate: ok …` → continue. No hook line (a session without the SessionStart hook), or anything other than `memory gate: ok` (a FAIL line, `memory dir not found`, an error) → run `node scripts/memory/memory-check.mjs`, fix or report every FAIL, before anything else.
2. Recall before opening any index: `node scripts/memory/memory-recall.mjs "<the goal in the owner's words>"`. Open what it returns; nothing → open `MEMORY.md`. Read the hard rules that match the goal. Then open `current-state.md`.
3. If `docs/boards.md` exists, read `decisions/` on each board it lists for answers since the last session.
4. Pre-flight, one line: `pwd` · `git branch --show-current` · `git rev-parse --short HEAD` · count of `git status --short` lines (incl. untracked) · `git rev-list --count origin/main..HEAD` (say "no remote" if that errors).
5. Restate the goal in one sentence. No goal given → ask for one in one line and stop.
6. Name the first concrete step. Wait for go. New work → `superpowers:brainstorming` if it is installed; otherwise talk through the requirements, write a short spec and a step-by-step plan, and wait for a go.

**Report**, in this order: gate line · path and branch · pre-flight · goal · first step · new board answers, if any.

**Never:** start work before go; bulk-read topic files; skip recall; treat a FAIL gate line as fine; invent a goal.
