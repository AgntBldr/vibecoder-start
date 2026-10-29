# Every session

A session is one sitting with your assistant, from opening it to shortening the conversation or closing it.

## 1. The whole prompt is three lines

A prompt is the instruction you give your assistant. To start a session: open Terminal (Windows: PowerShell), type `cd ` and drag your project folder into the window, press Enter, type `claude` and press Enter. Then, in Claude Code, paste these three lines, replacing `<one sentence>` with what you want to achieve:

```text
/new-session
Today I want to: <one sentence>.
Report where you are and what you intend, then wait for my go.
```

`/new-session` starts a skill: a saved set of instructions for this task. Paste the three lines into Claude Code's chat once Claude Code is open, not at your computer's plain command prompt.

## 2. Why it is short

The rules load from [AGENTS.md](../AGENTS.md), the project's rulebook file that Claude Code reads at the start of every session. You supply today's goal; you do not need to paste the rules again.

The sequence of planning, checking, building and saving is explained in [How work flows](02-how-work-flows.md).

## 3. What reply 1 looks like

The first reply in your session reports these items in order:

1. **The memory gate line.** Memory means saved notes that carry useful information between conversations. The gate checks that those notes are tidy and complete. A hook, an action that runs automatically when the session starts, normally supplies `memory gate: ok`. If that line is missing or reports a problem, the assistant runs the check and fixes or reports every failure before going further.
2. **Path and branch.** The path tells you which project folder it is working in. A branch is a named line of saved project history; this template normally uses `main`.
3. **The one-line pre-flight.** Git is the tool that records project changes; each saved checkpoint is a commit. A push means sending commits to the online copy. This is a short check of the starting position: the latest saved checkpoint's short identifier (`HEAD short SHA`, where HEAD means the latest saved checkpoint), the count of changed-file entries from `git status` including new files not yet recorded (untracked files), and how many saved checkpoints you have that are not yet pushed to the online copy (`origin/main`). If the online comparison fails, the report says `no remote`, meaning no online comparison is available.
4. **Your goal restated.** One sentence confirming what you want. If you have not supplied a goal, it asks for one and stops.
5. **The first concrete step.** It says what it intends to do first.
6. **New answers on a decision board, if any.** A decision board is a private page for decisions waiting on you.

Then it waits for your go before starting work.

Your go is a short reply such as `go` that tells the assistant to start. Until you give it, it only reports and plans.

Helper assistants that Claude sends off to do one step, such as a Codex build or a subagent (a separate assistant Claude starts for one job), skip this opening. They do their assigned step and report.

## 4. Ending a session

Type `/prepare`, or say 'prepare for compact', before ending the session, handing work to another assistant, or compacting. Compacting means shortening the conversation into a summary when the assistant cannot hold more of it at once. `/prepare` saves your notes and records, and checks that everything else is safe.

Its report includes this four-item checklist, with evidence for each item:

- **Memory:** the saved notes describe the current work, what is still underway, what needs your decision and the next action. Your memory lives in its own private folder outside the project, so `/prepare` checks and pushes two places: the project and the memory folder.
- **Learning with the why:** anything learned is saved with what happened, why it matters and how to apply it.
- **Roadmap:** the record of open work and latest changes is updated with what shipped and what it taught.
- **Committed and pushed, gate green:** changes are recorded as commits and sent to the connected online copies; the memory check reports no failures. Sending commits online is called a push.

For how commits and pushes work, see [Git without tears](04-git-without-tears.md).

**Safe to compact** means all four items are done and listed, both the project and memory folders have no changes left outside their saved checkpoints (a clean tree), every connected online copy has been pushed, and nothing is still underway (nothing in flight). The report includes the saved checkpoint's identifier. If memory has no online backup connected, it must explicitly say `memory committed locally, no backup remote`; that is not an online backup.

After you compact or reopen, paste the same three lines again.

**Not safe to compact** means something still needs attention. The report names what remains and marks each item with one of two words:

- **LOST:** it is only in this conversation or still in flight. The next session cannot rely on finding it in saved files.
- **PAUSED:** it is in files on your computer but has not been pushed. This includes edited project files that still need a review before they can be committed; `/prepare` never commits those for you.

On Not safe, do not compact or close yet. Ask the assistant to save the LOST items and push the PAUSED items that are already committed. Edited project files that still need a review are reviewed first by a model that did not build them, then committed and pushed. Then run `/prepare` again.

These words explain where unfinished work stands; they do not claim the checkpoint is complete. A failed gate, an unwritten learning, or work not pushed to an existing online copy prevents a Safe result.
