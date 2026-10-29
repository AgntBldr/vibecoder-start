---
name: codex-build
description: Have Codex build one approved plan step in its workspace-write sandbox, then verify the result yourself. Use when executing a plan step, or when the owner says "build this", "codex build", "let Codex do it".
---

# codex-build

**When:** one plan step at a time, from a plan whose review log says APPROVED.

**Preconditions** (stop and say which failed)
- `git status --short` is empty.
- The plan's own log (`docs/reviews/…-plan.md`, plan rounds only): its latest round says `Verdict: APPROVED.` and its `Hash:` equals `git hash-object <plan>`.
- Everything the step needs is installed: the sandbox has no network.
- `codex login status` succeeds. Otherwise stop and say so; the owner decides whether Claude builds the step itself (then `cross-review`'s no-Codex branch reviews it).

**Steps**
1. Write the prompt to `<scratch>/prompt-<N>.txt` (`<scratch>`: a folder outside the repo) with a quoted heredoc (`<<'EOF'`), so backticks and `$` stay text: the step text and its acceptance criteria; the path fence ("touch only: <files>"); the AGENTS.md rules that apply; three standing orders: *do not commit or touch git*; *for a bug fix, write the failing test first, run it, show it failing, then fix and show it passing, and include both outputs*; *end with the list of files you changed*.
2. Run:
   `codex exec -s workspace-write -C "<repo>" -o "<scratch>/build-<N>.txt" "$(cat "<scratch>/prompt-<N>.txt")" < /dev/null`
3. Trust nothing in the output. `git status --porcelain`: any path outside the fence → stop and report it; do not restore or delete anything yourself (another session may own it); the owner decides. Every expected file exists.
4. Run the tests YOURSELF with the project's test command (AGENTS.md §1, or the step's own check). Paste the result.
5. Failing → one fix round: steps 1–4 again, with the failure output and "fix only this" in the prompt. Max 2 fix rounds, then stop and report.
6. Passing → stage the fenced files by name and run `cross-review` on the diff.

**Report:** the step, files changed, fence check, your own test output, fix rounds used.

**Never:** believe "done" from Codex (it exits 0 on failure); let Codex commit; omit `< /dev/null`; install dependencies inside a step; build from an unapproved or changed plan; touch a file outside the fence yourself to "help".
