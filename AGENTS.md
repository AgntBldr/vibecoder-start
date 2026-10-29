# AGENTS.md — rules for anyone, human or AI, working in this repo

## 0. This file
- Single source of truth. CLAUDE.md only imports it (`@AGENTS.md`); never write rules there.
- If something this file points at doesn't exist yet, say so; don't invent it, don't assume it's gone.
- Skills: `.claude/skills/<name>/SKILL.md`. Claude Code loads them; Codex, read the one named for your task.

## 1. Project
- What this is: <one sentence>
- Who it's for: <one sentence>
- Stack: <languages, frameworks, hosting — or "not chosen yet">
- How to run it: <command>
- How to test it: <command>

## 2. Who does what
- Claude plans, designs, orchestrates, reviews. Codex builds from a written plan. Reviewer ≠ builder, always.
- Premium model: planning, architecture, security, final review. Cheaper models: bulk edits, scaffolding,
  renames, docs. Say which model you're using for a step and why; push back if asked for the wrong one.

## 3. Every session
- Reply 1 in the owner's session is `/new-session` (the one exception: a brand-new project's first session starts with `/first-session`): the hook's gate line first, then path and branch,
  a one-line pre-flight (HEAD short SHA · `git status` incl. untracked · commits ahead of origin),
  the goal restated, the first concrete step; then wait for go. A `codex exec` call or a subagent is
  not the owner's session: it completes its assigned step and reports.
- End of every piece of work: path + branch + the exact git command you ran (or would run).
- After each delivery: verify → `/prepare` → report → STOP and wait.

## 4. How work flows
- Plan before build (superpowers: brainstorm → spec → plan; without that plugin, talk through the requirements, write a short spec and a step-by-step plan). `/cross-review` the plan before building.
- `/codex-build` one plan step at a time; Claude runs the tests itself.
- A model that didn't build it reviews the diff before it is committed. Report built-by / reviewed-by.
  Only bookkeeping (`docs/reviews/`, the roadmap, memory pins) and docs-only edits commit without one, and so do the person's own answers that `/first-session` writes into section 1.
- Every bug fix lands with a test verified to FAIL without the fix.
- After 3 failed fixes for one bug, stop and report what you tried.
- Before building anything reusable: `/find-skills` first.

## 5. Git
- One branch, `main`, pushed straight. No feature branches until you have a reason.
- `git status` + `git log --oneline -5` before touching git, and again before committing.
- Stage by name. Never `git add -A`, `git add .`, or `commit -a`.
- Never merge, rebase, reset, checkout or delete what you didn't create. Merge needed? Stop and ask.
- Another session may be editing this repo at the same time.

## 6. Secrets
- Keys live in `.env` (gitignored) or server config. Never commit, push or print one.
  Found one committed? Stop and say so. `/secret-check` before every push.

## 7. Stopping points
- `/prepare` before any compact, handoff or session end. Say **Safe to compact** - `<sha>`, clean tree,
  nothing in flight — only when all four are done and listed: memory · learning with the WHY · roadmap ·
  committed and pushed, gate green. Otherwise **Not safe to compact - <what is unwritten>**. Say it
  unprompted at every boundary.

## 8. Roadmap
- `docs/ROADMAP.md`: `Last updated:` replaced, `## Open work` edited, `## Latest changes` gains a dated entry
  (`### YYYY-MM-DD - <title>`: what shipped and what it taught), never rewritten; older entries move to
  `docs/ROADMAP-HISTORY.md`. `docs/ROADMAP-OVERVIEW.md` is generated
  (`node scripts/memory/roadmap-overview.mjs --write`), never hand-edited.

## 9. Outside the repo
- Propose before installing tools or creating repos, projects, issues. Take the work to the exact step only
  the owner can do, then give click-by-click steps with full URLs and literal values.
- If `docs/boards.md` lists a decision board: decisions the owner must make go on it, never as chat
  questions; read `decisions/` before acting on anything it covers; update it at every checkpoint.

## 10. Working with the owner
- Spar: if they're making a mistake, say so plainly and say what you'd do instead. Failing tests are failing.

## Memory gate

- At session start and every checkpoint run `node scripts/memory/memory-check.mjs`; fix or report every FAIL.
- Claude Code uses SessionStart; other agents run the gate manually. Memory pre-commit runs `--hook --scope memory`.
- If the private memory folder is absent: restore it from its private remote (`git clone <address> <the folder named by node scripts/memory/memory-init.mjs --dry-run>`); only if there is no remote, run `node scripts/memory/memory-init.mjs`. Never run both.
- Before opening indexes run `node scripts/memory/memory-recall.mjs "<question>"`; honor the router's activity triggers.
- State is overwritten, facts are moved, history is git: one current block, one lesson per file, project facts in `brain/` with memory pointers.
- Never raise a pin or allowlist ceiling to pass; move detail down a layer. Write the destination before shortening a pointer.
- Capture with `node scripts/memory/memory-lesson.mjs new --kind <mistake|practice|observation> --route <family> "<title>"`; fill the TODO description and body.
- At checkpoints run `node scripts/memory/memory-checkpoint.mjs`: generate indexes, rotate history, refresh overviews, gate, commit and push. Use `--no-push` or `--dry-run` when needed.
