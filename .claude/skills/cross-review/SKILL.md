---
name: cross-review
description: Review a plan or a staged diff with a model that did not write it, and record the verdict. Use after writing a plan, after a build step passes its tests, or when the owner says "review this", "cross-review", "second opinion".
---

# cross-review

**When:** a plan is ready; a build passed its tests and is staged; the owner asks.

**Who reviews:** a model that did NOT write it.
- A plan, or a diff Claude built → Codex (step 2).
- A diff Codex built → a fresh Claude subagent (Agent tool).
- Codex missing or out of quota → a Claude subagent on a DIFFERENT model from the builder (the Agent tool's `model`); log it as "same vendor, different model". No other model → stop and tell the owner.

**Steps**
1. Record the hash BEFORE the review. Plan: `git hash-object <plan>`. Diff: stage the fenced files by name (`git add <files>`), then `git write-tree`. Write the round block (below) with the hash in its own log: `docs/reviews/YYYY-MM-DD-<topic>-plan.md` (a plan) or `…-<topic>.md` (its diffs).
2. Write the prompt to `<scratch>/prompt-<N>.txt` (`<scratch>`: a folder outside the repo) with a quoted heredoc (`<<'EOF'`) so backticks and `$` stay text. Run:
   `codex exec -s read-only -C "<repo>" -o "<scratch>/review-<N>.txt" "$(cat "<scratch>/prompt-<N>.txt")" < /dev/null`
   The prompt: what to review (the plan path, or "run `git diff --cached` and `git status`"); the AGENTS.md rules that apply; for round 2+, the previous findings and your arbitration; the contract: numbered findings `file:line — what — why it matters — suggested fix`, then a last line `VERDICT: APPROVED | REVISE | BLOCKED`. A Claude subagent gets the same prompt and contract, read-only tools.
3. Parse the last line. Empty file, no verdict line, or APPROVED above unresolved material findings = not approved: stop and report.
4. Check the hash again, as in step 1. Changed → the round does not count; say what changed; back to step 1.
5. REVISE → arbitrate each finding in the table: accept (the builder makes the change; Codex-built → `codex-build` steps 1–5 only, the log is no fence breach; re-stage by name) or reject (with the reason). Back to step 1 as round N+1 (the count goes on). Caps: plan 3 rounds, diff 2. Cap reached, or BLOCKED → stop, report what is open, no fallback model; the owner decides. Owner approves → a round block: `Reviewed by: the owner`, the hash they approved, what changed since the last review, `Verdict: APPROVED.`
6. APPROVED → the pre-review hash is the approved snapshot. Plan: `save-progress` it and the log. Diff: `save-progress` commits that exact tree, then the log alone.

**Log block**, one per round:
`## Round N — YYYY-MM-DD` / `Built by: <model>. Reviewed by: <model>. Hash: <hash> (before review). Verdict: <VERDICT>.` / `| # | Finding | Arbitration | Change |`

**Report:** built by, reviewed by, verdict, findings and arbitration, log path, rounds used.

**Never:** commit before APPROVED; write the hash into the plan; let the builder's model review; omit `< /dev/null`.
