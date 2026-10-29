# Memory

Memory means saved notes that carry useful information from one conversation to the next. Without it your assistant starts every
session knowing nothing about your project. This starter uses the **memory ladder**, a small system that keeps those notes short enough to trust.

## Where it lives, and why outside the project

The notes are a folder of Markdown files kept **outside** your project folder, with their own Git history and their own private backup.
By default the folder is inside a folder called `.claude/projects` in your home folder (`~` means your own user folder, such as `/Users/you` on a Mac or
`C:\Users\you` on Windows). Its name is your project's full path with every character that is not a letter or digit turned into `-`, followed by `/memory`.

To see exactly where it is (or would be) without creating anything, run in the project folder:

```text
node scripts/memory/memory-init.mjs --dry-run
```

The first line, `memory: <folder> (default)`, is the folder (leave out the `(default)`). `node scripts/memory/memory-check.mjs` prints the same `memory:` line once the folder exists.

Why outside: your project may be shared or public one day; your notes are private. The notes also survive if you delete or recreate the project folder.
Because the name comes from the project's path, **moving or renaming the project folder makes the notes look missing**. Do not run `memory-init` then: move the old notes folder to the new name.

## What is in it

- `MEMORY.md`: the router. A short list of hard rules (things the assistant must know before it knows the task), what is in progress, and links to indexes.
- `INDEX-*.md`: generated lists of lessons, one line each. Never edit them by hand.
- `lesson-NNN-*.md`: one lesson per file. What happened, why, and how to apply it.
- `current-state.md`: one block that says where the work stands. It is overwritten at each save; history lives in Git.

## The two commands you will see

- `node scripts/memory/memory-check.mjs` (the **gate**): checks size and structure. `0 FAIL` is good. Claude Code runs it for you at the start of each session and the first reply starts with `memory gate: ok`.
- `node scripts/memory/memory-recall.mjs "<what you are about to do>"`: names the notes worth opening, so the assistant does not read everything. Empty output on a new project is normal.

## Saving: `/prepare`

Before you close a session or shorten a long conversation, ask for `/prepare`. It updates the notes and the roadmap, runs the gate, commits, and pushes the notes to your private backup.

## Adding a lesson

When something goes wrong or goes right in a way worth remembering, ask: "Capture that as a lesson." The assistant runs
`node scripts/memory/memory-lesson.mjs new --kind <mistake|practice|observation> --route <family> "<title>"` and fills in the why and how. You never edit an index or a count.

## Moving to another computer

Find the folder name with `node scripts/memory/memory-init.mjs --dry-run` (first line), then clone your private backup into it:

```text
git clone <your backup address> "<that folder>"
```

Then run `node scripts/memory/memory-check.mjs`. Do not run `memory-init` for real first: it would start a new, separate memory.

## If the gate says "memory dir not found"

The notes folder is missing on this computer. If you have never set it up: `node scripts/memory/memory-init.mjs`. If you have a backup: clone it as above. Never both.

More: the kit's own guide is https://github.com/AgntBldr/memory-ladder (see `WHY.md` and `docs/HOW-IT-WORKS.md` there).
