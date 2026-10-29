# VibeCoder Start

A ready-made project folder for building software with an AI coding assistant, without needing to be a programmer.
Your assistant reads a rulebook every session, keeps private notes so it remembers decisions between conversations, and
has a few ready-made commands such as `/first-session`, `/new-session` and `/prepare`.

It works with Claude Code first. Codex and Cursor can read the same rulebook. It runs on **Mac and Windows**; the guides show both.

## Get it

- **Easiest:** on the GitHub page for this project click the green **Code** button, then **Download ZIP**, and unzip it (Mac: double-click the zip in Downloads). The folder is called `vibecoder-start-main`.
- Or use the `vibecoder-start.zip` someone sent you. The folder is called `vibecoder-start`.
- Developers: **Use this template** creates a repository of your own on GitHub; clone it to your computer, then follow the guide from section 3. If you used a zip, follow section 2 first.

## Set up (about 20 minutes once the tools are installed)

1. Install what [docs/00-first-session.md](docs/00-first-session.md) lists: Git, Node.js 22 or newer, Claude Code (needs a paid Claude plan), a GitHub account.
2. Follow steps 2 and 3 of the guide (unzip, rename the folder, open a terminal in it, `git init -b main` and the first commit, start `claude`), then paste three lines
   into Claude Code (put your own sentence after "I want to build"):

   ```text
   /first-session
   I want to build: <one sentence>.
   Set up this project and its memory. Stop and tell me when a step needs me.
   ```

3. Answer four short questions, then create a private GitHub repository for your notes when it asks (click-by-click steps are in the guide).

Every session after that starts with three lines, see [docs/01-every-session.md](docs/01-every-session.md).

## Reading order

| Guide | What it answers |
| --- | --- |
| [docs/00-first-session.md](docs/00-first-session.md) | Setting up, what you put in and where, what a good result looks like, what to do when it goes wrong |
| [docs/01-every-session.md](docs/01-every-session.md) | The three lines that start every session |
| [docs/02-how-work-flows.md](docs/02-how-work-flows.md) | Plan, check, build, review, save |
| [docs/03-models-and-tokens.md](docs/03-models-and-tokens.md) | Which model for which job, and what costs what |
| [docs/04-git-without-tears.md](docs/04-git-without-tears.md) | Saving and undoing work |
| [docs/05-memory.md](docs/05-memory.md) | How the notes work and how to restore them |
| [docs/06-upgrades.md](docs/06-upgrades.md) | Optional add-ons, and updating this template |
| [docs/GLOSSARY.md](docs/GLOSSARY.md) | Every term the guides use |

## Commands (skills) in `.claude/skills/`

| Command | Use it to |
| --- | --- |
| `/first-session` | Set up a brand-new project (once) |
| `/new-session` | Start every later session |
| `/prepare` | Save everything before you close or shorten a conversation |
| `/cross-review` | Have a different model check a plan or a change |
| `/codex-build` | Hand one plan step to Codex to build |
| `/save-progress` | Save work in progress |
| `/secret-check` | Look for keys and passwords before you push |
| `/explain-changes` | Get a change explained in plain words |
| `/find-skills` | Look for an existing skill before building one |
| `/memory-review` | Check and repair the notes |
| `/memory-ladder` | The rules for capturing, finding and saving notes (the assistant uses it; you rarely type it) |

## What is where

- `AGENTS.md`: the rulebook. Section 1 describes your project; the setup fills it in for you.
- `docs/ROADMAP.md`: what has been done and what is next. `docs/DECISIONS.md`: decisions and why. `docs/reviews/` and `docs/superpowers/` start empty; your assistant fills them as you work.
- `.env.example`: the names of keys a project may need. Real keys go in a file called `.env`, which is never saved to Git.
- `scripts/memory/`: the memory kit (version in `scripts/memory/KIT.json`). Your notes themselves live in a separate private folder.

## Built on

The [memory ladder kit](https://github.com/AgntBldr/memory-ladder). The kit's own scripts need no npm packages and make no network calls; only the git commands you approve (such as saving your notes to your own private backup) use the network.

## License

See [LICENSE](LICENSE).
