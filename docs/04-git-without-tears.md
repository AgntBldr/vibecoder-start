# Git without tears

Git records your project's change history. You do not need to learn every Git command to use this template. Type slash commands such as `/save-progress` in Claude Code; the assistant follows the template's instructions for you. The Git commands below show what each undo does, so you can choose knowingly.

## 1. Save points

A **commit** is a save point: staged project changes saved together with a message explaining what changed and why; to stage a change is to choose it for the next commit. Saving a file in your editor does not make a commit. A **push** sends commits to the project's online copy.

`/save-progress` makes a commit and pushes it. It first checks what changed, what is allowed to be saved, and whether secrets (passwords and keys) are present. A key (also called an API key) is a secret string, like a password, that lets a program use a service on your account. It reports the commit's **SHA**, the identifier you can use to refer to that save point. If no online copy is connected, it says so and gives you the commands to connect one; the commit remains local until pushed.

`/save-progress` comes last in the loop explained in [How work flows](02-how-work-flows.md), section 1.

Most work needs a reviewer's APPROVED before `/save-progress` will commit it. Only bookkeeping (the review logs, the roadmap and the memory size limits) and docs-only edits that were not built under a plan skip that review (see [How work flows](02-how-work-flows.md)). `/save-progress` checks which applies, and stops if what is staged no longer matches what was approved.

## 2. Undo: the three moves

`/save-progress` shows these three moves after saving. In these commands, replace `<sha>` with a commit's identifier and `<file>` with the file's path. Do not type the angle brackets. Ask the assistant to identify the right commit and file if you are unsure.

'Unsaved' here means not yet in a commit. Edits you saved in your editor but never committed count as unsaved, and the two restores overwrite them.

- **Undo this saved commit, keeping history:** `git revert <sha>` (safe). Here, `<sha>` is the commit you want to undo (often the one just saved). It records a new commit that undoes the earlier change, keeping the record of what happened.
- **Get one file back as it was in a commit:** `git restore --source <sha> <file>` (overwrites unsaved edits to that file; confirm first). Here, `<sha>` is a commit where the file was the way you want it, not the one you just saved. It swaps this one file for its older version. Any edits you have not saved in a commit are lost for good. The commits themselves are untouched, and the older version is only an unsaved edit until you ask Claude to save it.
- **Throw away unsaved edits to one file:** `git restore <file>` (overwrites unsaved edits; confirm first). Those edits are discarded.

The assistant must get confirmation before either destructive restore. Ask it to explain what would be overwritten before agreeing.

## 3. One branch, `main`

A **branch** is a named line of saved project history. This template uses one branch, `main`, and pushes straight to it. One line of save points is easier to follow while you learn.

The rule is no feature branches until you have a reason. Direct saves to `main` still require the applicable tests, review and secret checks. If combining branches becomes necessary, the assistant stops and asks; it must not combine branches, rewrite or move saved history, or switch branches (`git merge`, `git rebase`, `git reset`, `git checkout`) on work it did not create. It must not delete work it did not create either.

## 4. Another session may be working here

Another session may be editing the same project. A changed file is not automatically part of your current task.

Stage by name: use `git add` with the specific file names that belong to the task. Never use `git add -A` or `git add .`; they can collect another session's work too. Never use `commit -a` either.

The assistant checks the changed and staged files before saving. It stops if unexpected files are staged, or if the staged set no longer matches what was approved; it never unstages someone else's work. After approval, it must not re-stage: the approved snapshot, the exact set of staged changes the reviewer approved, is what gets committed.

## 5. Going public safely

`/save-progress` already runs `/secret-check` for you before each push, so you rarely run it by hand. Before making the repository (the project folder and its whole saved history, on GitHub) public, tell Claude "secret check, I'm making it public" so it checks all of the history, not only the commits not yet pushed. A public repository makes its published files and history visible to everyone. Deleting a key from today's file does not remove it from earlier commits.

`/secret-check` checks your files, what is staged, the commits about to be pushed, that no `.env` file was committed by mistake, and that `.gitignore` covers `.env`. Before going public, it checks all history. A finding stops the push; an error means the check did not run, not that it passed. If it finds a key in a file, move the value into `.env` and run the check again.

**`.gitignore`** lists files and folders Git should leave out, such as private settings and temporary files. It does not remove files or secrets already recorded in history.

**`.env`** holds private settings, including secret access keys, and must stay ignored by Git. `.gitignore` keeps `.env` out of normal commits and pushes, so keys belong there and nowhere else. Never commit, push or print a key.

**`.env.example`** is the shareable guide to which settings someone needs. It is committed and pushed with the project, so anyone who can see the repository can read it, and it must hold no keys or other secrets: use empty values or obvious placeholders such as `replace-me`. Copy `.env.example` to a new file named `.env` (ask Claude to make the copy), then type your own values into `.env` yourself; do not paste a key into the chat.

If a key is already committed, stop and report it. The key is compromised and must be rotated, meaning replaced so the old one no longer works. History must be cleaned before pushing, meaning rewriting the saved history so the key is gone from every commit; you decide how, with Claude explaining the options; the assistant must not rewrite history on its own.
