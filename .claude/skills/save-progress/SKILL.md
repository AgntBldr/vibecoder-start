---
name: save-progress
description: Commit and push work safely, in plain language, with the undo commands. Use when the owner says "save my work", "save progress", "commit this", or after a diff is APPROVED.
---

# save-progress

**When:** the owner asks to save; a reviewed diff is ready to commit; bookkeeping or docs-only edits are ready.

**Steps**
1. `git status --short` and `git log --oneline -5`. Say what is modified, staged and untracked.
2. Decide what may be committed:
   - Builder work (anything under a plan's fence, or any file outside `docs/`) needs an approval: the latest `## Round` in its review log, which must say `Verdict: APPROVED.` Do not re-stage. Run `git write-tree`; it must equal that round's `Hash:`. Different → stop: "the staged tree changed after approval; run cross-review again".
   - Without an approval, only bookkeeping (`docs/reviews/**`, `docs/ROADMAP*.md`, `scripts/memory/memory.config.json`) and docs-only edits under `docs/` may be staged by name, each kind in its own commit that names it; `git diff --cached --name-only` must then list only those files, else stop (never unstage others' work). Anything else waits.
3. `secret-check` on the staged snapshot. A finding stops here.
4. Commit with a plain-English message: first line what, body why, ending `Co-Authored-By: Claude <model> <noreply@anthropic.com>`.
5. `git push`. No remote → say so and give the owner the exact `git remote add origin <url>` and `git push -u origin main` to run.
6. Show the three undo moves and which destroy work:
   - undo this saved commit, keeping history: `git revert <sha>` (safe)
   - get one file back as it was in a commit: `git restore --source <sha> <file>` (overwrites unsaved edits to that file; confirm first)
   - throw away unsaved edits to one file: `git restore <file>` (overwrites unsaved edits; confirm first)

**Report:** the commit SHA, the files, the tree hash matched (for builder work), the three undo commands with their warnings.

**Never:** `git add -A`, `git add .`, `commit -a`; commit builder work without a matching approval hash; re-stage after approval; push with a secret-check finding; run a destructive undo without confirmation.
