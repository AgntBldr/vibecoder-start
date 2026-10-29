---
name: prepare
description: The checkpoint before any compact, handoff or session end, ending in Safe or Not safe to compact. Use when the owner says "prepare for compact", "checkpoint", "can I compact", "hand off".
---

# prepare

**When:** before any compact, handoff or session end; after each delivery. Say the verdict unprompted at every boundary.

**Steps**
1. Git state: `git status --short`, `git rev-list --count origin/main..HEAD` ("no remote" if it errors), `git worktree list`. Files outside the bookkeeping set (`docs/reviews/**`, `docs/ROADMAP*.md`, `scripts/memory/memory.config.json`) modified or staged without an approval → say "unreviewed changes present: run cross-review first", never commit them; mark them PAUSED.
2. Memory capture, per `memory-ladder`: if something was learned, one `lesson-NNN-<slug>.md` with what happened, **Why:**, **How to apply:**, and its index line `- [#NNN](lesson-NNN-<slug>.md) — <summary ≤ 110 chars>` in the matching `INDEX-craft-*.md`. Update, don't duplicate.
3. Overwrite the single `## CURRENT` block in `current-state.md`: what this is, what is in flight, what waits on the owner, the next action.
4. Roadmap: replace `Last updated:`, edit `## Open work`, add `### YYYY-MM-DD - <title>` under `## Latest changes` with what shipped and what it taught (link the review log). If `brain/` exists, append `## [YYYY-MM-DD] <op> | <title>` to `brain/00-system/log.md`. Then, where they exist: `node scripts/memory/memory-rotate.mjs log --write`, `node scripts/memory/brain-index.mjs --write`, `node scripts/memory/roadmap-overview.mjs --write`.
5. If `docs/boards.md` lists a board: finished items to Completed, new decisions, a new stamp.
6. Gate: `node scripts/memory/memory-check.mjs` to `0 FAIL`. Never raise a pin or allow entry; move detail down a layer.
7. Memory repo: `git -C "<memory>" add <files by name>`; `git -C "<memory>" diff --cached --name-only` must list only those, else stop; `secret-check` steps 1–3 and 6 inside the memory folder; commit; `git -C "<memory>" push` if it has a remote, else say "memory committed locally, no backup remote".
8. Bookkeeping: `save-progress` the roadmap, overview and review log by name, own commit; with step 1's PAUSED files staged, leave it uncommitted and list it.
9. Verdict: `git status --short` in both repos, step 1's ahead count, `git -C "<memory>" status -sb | head -1` (`ahead N`, any branch). Safe only when both are clean, each repo that has a remote pushed, nothing in flight.

**Report:** branch and sha · shipped · remaining · owner decisions · the four-item checklist with evidence (memory · learning with the why · roadmap · committed and pushed, gate green) · then **Safe to compact** - `<sha>`, clean tree, nothing in flight. or **Not safe to compact - <what is unwritten>**, each item marked LOST (only in chat or in flight) or PAUSED (in files, not pushed).

**Never:** claim Safe with a FAIL, an unpushed repo that has a remote, or an unwritten learning; write history into the state block; skip step 1.
