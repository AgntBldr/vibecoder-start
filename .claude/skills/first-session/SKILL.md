---
name: first-session
description: Set up a brand-new project from this template: check tools, fill AGENTS.md section 1 by asking four questions, create the private memory, start the roadmap, guide the backup, commit by name. Use only when the person types /first-session or says "set up this project".
---

# first-session

**When:** the person's very first session in a project made from this template, after they paste:

```text
/first-session
I want to build: <one sentence>.
Set up this project and its memory. Stop and tell me when a step needs me.
```

**Is it already set up?** Run `node scripts/memory/memory-check.mjs`. If it prints `memory dir not found` or a `FAIL router.missing` line (Claude Code may have created an empty notes folder), the memory is not set up: start at step 1. If the memory exists AND `AGENTS.md` section 1 has no `<...>` placeholder left, the project is set up: say so in one line, name `/new-session`, and stop. If the memory exists but a placeholder is left, continue from step 2. Not for `codex exec` calls or subagents.

**Not a git repository?** (`git rev-parse --is-inside-work-tree` fails): stop and send the person to `docs/00-first-session.md` section 2.

Plain words. One step at a time. After each step say what you did in one line. Never print a key. Stage files by name, never `git add -A`. Claude Code will ask the person to approve commands: tell them to choose Yes for the ones in this skill. Until step 3 has run, save nothing with your own memory feature: it writes into the same notes folder and would make `memory-init` skip or refuse that folder. Keep the answers in the conversation and write them to `AGENTS.md`.

**Steps**
1. **Tools.** Run `node --version`, `git --version`, `git config user.name`, `git config user.email`, `codex --version` (Codex is optional) and, on a Mac, `gh auth status` (the push in step 6 needs it). Report each as found or missing, with the install command or link from `docs/00-first-session.md` section 1. Node below 22, no Git, or no Git name/email: stop here and tell the person exactly what to run, then wait. On a Mac where `gh auth status` is not logged in, give the GitHub CLI row from section 1 now and wait, unless they have said they will type `skip` at the backup step.
2. **Interview.** Ask these four questions, one at a time, and wait for each answer. Offer the example in brackets. Write the answers into `AGENTS.md` section 1, replacing the `<...>` placeholders, in the person's own words, one short sentence each:
   1. What do you want to build? [a website for my dog-walking business]
   2. Who is it for? [my customers, who book walks]
   3. Is anything already chosen to build it with (language, framework, hosting)? If not, say "not chosen yet". [not chosen yet]
   4. How do you run it and test it today? If you do not know yet, say "not yet". [not yet]
   If the prompt already contains a real sentence after "I want to build:" (not the leftover `<one sentence>`), use it for question 1 and skip it. A leftover `<one sentence>` counts as no answer. Fill "How to run it" and "How to test it" from answer 4, or `not yet`.
3. **Memory.** Run `node scripts/memory/memory-init.mjs`. Report the `memory:` line (the folder that holds the notes; remember that path as `<memory folder>` for steps 6 and 8) and the gate summary; `0 FAIL` is required. If it prints `memory already exists`, continue only if `git -C "<memory folder>" rev-parse --is-inside-work-tree` succeeds and `<memory folder>/current-state.md` exists. Otherwise (or if it refuses with `is not empty and has no MEMORY.md`) the folder holds stray files, usually from Claude Code's own memory feature: stop, tell the person, and ask whether you may rename the folder to the same name plus `-old` (nothing is deleted; Mac `mv "<folder>" "<folder>-old"`, Windows `Rename-Item`), then run `memory-init` again. Any FAIL: stop and show the line.
4. **Roadmap.** In `docs/ROADMAP.md`: replace the date after `Last updated:` with today's date; replace the whole body of `## Open work` with a `### <short name of the goal>` heading and, under it, one `- ` bullet describing the goal from question 1 (the generated overview lists only items under a `###` heading); add `### <today> - Project started` under `## Latest changes` with one line saying what was set up. Then run `node scripts/memory/roadmap-overview.mjs --write`.
5. **Backup. STOP here.** Tell the person, as numbered click-by-click steps, to create the private backup repository themselves:
   1. Open https://github.com/new
   2. Repository name: `<project folder name>-memory` (lowercase, spaces become hyphens)
   3. Choose **Private**
   4. Leave "Add a README file", ".gitignore" and "license" **unticked**
   5. Click **Create repository**, copy the address that starts `https://github.com/` and ends `.git`, and paste it here.
   If the address they paste does not start with `https://github.com/`, ask again. They may type "skip": then say "memory committed locally, no backup remote" and go to step 7.
6. **Push.** Run `git -C "<memory folder>" remote add origin <address>` (if `origin` already exists: `git -C "<memory folder>" remote set-url origin <address>`). Before pushing, run the `secret-check` skill's steps 1 to 3 and 6 inside the memory folder (not steps 4 and 5: the memory repository has no `.env` rules); any real-looking hit: stop. Then `git -C "<memory folder>" push -u origin HEAD`. Warn the person that GitHub may open a browser window or pop-up asking them to sign in; wait for them. If the push says `Authentication failed`, `could not read Username` or `Device not configured`, point them to the matching row in `docs/00-first-session.md` ("If something goes wrong"; on a Mac they run `gh auth login` in a second Terminal window, then say `continue`). Use `git -C "<memory folder>"` for every command in that folder; never `cd` into it. Then run `node scripts/memory/memory-check.mjs`; the summary must begin `memory-check ... 0 FAIL, 0 WARN`.
7. **Commits.** No `/cross-review`: the `AGENTS.md` edit is only the person's own answers (see `AGENTS.md` section 4). Commit `AGENTS.md` alone with the message `Describe the project`. Then commit `docs/ROADMAP.md` and `docs/ROADMAP-OVERVIEW.md` together with the message `Roadmap: project started`. Stage each by name. Push the project only if it has a remote and the person said yes.
8. **Report**, in this order: tools found/missing . the four answers as written in `AGENTS.md` section 1 . the `memory:` line . the gate summary . what is committed . "Next time, paste the three lines from docs/01-every-session.md."

**Never:** skip the interview; invent an answer; make the GitHub repository for the person; continue past step 5 without the address or "skip"; leave a `<...>` placeholder in section 1; run `memory-init` when the person says they already have a backup of their notes (restore it instead, see `docs/05-memory.md`).
