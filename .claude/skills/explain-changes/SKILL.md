---
name: explain-changes
description: Explain in plain English what just changed, why, what could break and where to look first. Use when the owner says "what did you just do?", "explain the changes", "what changed", or after a build they did not follow.
---

# explain-changes

**When:** the owner asks what changed; after a build or a commit they did not follow.

**Steps**
1. Find the change. A commit the owner names comes first: `git show --stat <sha>`, then `git show <sha>`. Otherwise `git status --short --untracked-files=all`: any lines → uncommitted work; read `git diff HEAD` for tracked files and open each `??` file, because `git diff HEAD` never shows untracked (`??`) files. No lines → `git log --oneline --name-only -5`: take the newest commit that is not only bookkeeping (`docs/reviews/`, `docs/ROADMAP*`, `scripts/memory/memory.config.json`), name any bookkeeping commits above it in one line, and read it the same way (`git show --stat <sha>`, then `git show <sha>`).
2. For each file, one plain-English paragraph: what changed; why (from the plan step, the review log in `docs/reviews/` or the commit message; say "reason not recorded" rather than guess); what could break; where to look first (file and line).
3. A term the owner may not know: point to `docs/GLOSSARY.md`, or explain it in one clause.
4. More than ten files: group by folder first, then offer the per-file detail.

**Report:** one paragraph per file (or per folder), then one line: how to see the change working.

**Never:** change any file while explaining; quote a key or password (give only the file and line; a key in a commit → stop and say so: it is exposed and must be rotated, AGENTS.md §6; otherwise run `secret-check`); guess a reason; paste whole diffs; use a term without explaining it.
