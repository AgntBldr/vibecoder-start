# How work flows

Claude and Codex are two AI assistants; each runs on a model, the particular AI system doing the work. You describe what you want. Claude organises the work, Codex builds one agreed piece at a time, and a separate reviewer checks it. A skill is a saved set of instructions for one of these jobs; Claude runs the skills for you.

## 1. The loop

1. **Claude writes a plan.** Before the plan, Claude talks the idea through with you (brainstorming) and writes it up as a short spec, a description of what you want; the plan comes from the spec. It turns your request into small steps, with a check that shows whether each step is finished.
2. **Codex reviews the plan, read-only.** The `cross-review` skill asks it to look for problems without changing any files. The findings are fixed or answered, and the plan is reviewed again until the verdict is APPROVED. Building waits for that approval.
3. **Codex builds one step.** The `codex-build` skill gives it the approved step, the files it may change, and the checks it must pass. Codex reports the files it changed; it does not save a commit: a saved checkpoint of selected project changes, with a message explaining them.
4. **Claude runs the tests itself.** Tests are checks that the work behaves as expected. Claude checks the actual result instead of accepting the builder's claim that it is done.
5. **A different model reviews the change.** After the tests pass, `cross-review` asks a fresh Claude reviewer to inspect what Codex built. The reviewer reports problems without editing the files. On REVISE, the change is fixed and reviewed again.
6. **`save-progress` commits it.** Once approved, this skill checks for secrets (passwords and keys, which belong in a private `.env` file and must never be committed or pushed); if it finds one, the save stops and you are told. Otherwise, it saves the approved version as a commit. It then pushes that save point to the online copy.

Every change outside `docs/`, however small, must be reviewed by a model that did not build it before it is committed. Only docs-only edits (changes to files inside the `docs/` folder and nothing else) and bookkeeping (updating project records) may be saved without that review: the review logs in `docs/reviews/`, the roadmap (the record of open work and latest changes), and the memory pins (the size limits on the memory index files, kept in `scripts/memory/memory.config.json`).

After each delivery Claude runs a checkpoint (`/prepare`), reports what is saved and what is left, and stops. It waits for your go before the next step.

## 2. Who does what and why

The planner, builder and reviewer are separate roles, never one assistant doing all three jobs on its own work. Nobody grades their own homework.

Claude plans and coordinates. Codex checks Claude's plan, then builds an approved step. A separate Claude reviewer checks Codex's change. Being allowed to review a plan does not let Codex approve its own later build: the model that wrote the work cannot review that work.

If Claude makes a change itself, Codex reviews it; if Codex built it, a fresh Claude reviewer does.

Claude should say which model is doing each job and why. This separation gives another pair of eyes a chance to catch mistaken assumptions before they become a saved change.

## 3. What you see on screen at each stage

| Stage | What the report tells you |
| --- | --- |
| Plan | What will be built, the steps, and how each will be checked. |
| Plan review | Who reviewed it, the problems found, and a verdict: `APPROVED`, `REVISE` (needs changes), or `BLOCKED` (cannot proceed). |
| Build | The step being built, the permitted files, and the files actually changed. |
| Tests | The check Claude ran, its result, and any failures still needing attention. |
| Change review | Who built and who reviewed, what the reviewer found, how each finding was handled, and the verdict. |
| Save | What was committed, its short SHA (the save point's ID), whether the push succeeded, and how to undo it, with a warning on any undo that would throw away unsaved edits. |
| Checkpoint | What is saved, what is still open, and that Claude is waiting for your go. |

A review round is one pass of a reviewer over the work, ending in a verdict. Review rounds are recorded in `docs/reviews/`. A report saying “done” is not enough on its own: the checks and review must support it. Ask Claude to explain any result you do not understand.

## 4. The caps

There are limits so the assistants cannot keep trying indefinitely:

- A plan gets at most **3 review rounds**.
- A change gets at most **2 review rounds**.
- A build step whose tests fail gets at most **2 fix rounds** after the first attempt. A fix round is one more attempt by Codex, followed by Claude running the tests again.
- A bug gets at most **3 failed fix attempts**; then Claude stops and reports what it tried.

Every bug fix must come with a test that fails without the fix, so you know the fix is what made it pass.

If the last allowed round still leaves problems, work stops. A `BLOCKED` verdict also stops the work. You are told what remains open, what was tried, and what decision is needed. No other model is tried in its place. Only you decide how to go on, including approving the work yourself, and that decision is recorded in the review log.

## 5. If Codex is missing or out of credits

For a review, Claude can use a different Claude model from the one that wrote the work. It records **“same vendor, different model”** in the review log, so you can see that both models came from the same provider. If no other model is available, it stops and tells you.

For a build, missing Codex is a stopping point too. Claude reports the problem, and you decide whether Claude should build that step instead. If it does, a different model must still review that work (a Claude model other than the one that built it), and the same caps apply.

## 6. “What did you just do?”

Ask Claude:

> What did you just do?

That runs `explain-changes`. It explains what changed in each file, why, what could break, and where to look first. It finishes with how to see the change working. If the reason was not recorded, it says so rather than guessing. Explaining changes does not edit your files.
