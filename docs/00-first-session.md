# Your first session

About 20 minutes, once, when the tools below are already installed; allow an hour or more on a new computer. At the end you have a project folder your assistant understands, private notes it keeps between
sessions, and a backup of those notes. After that, every session starts with three lines ([docs/01](01-every-session.md)).

This guide works on **Mac** and **Windows**. Where they differ, both are shown. A **terminal** is a window where you type
commands: on a Mac it is the *Terminal* app, on Windows it is *PowerShell*. The commands themselves are the same on both.
Where a command shows something in `<angle brackets>`, replace it with your own text and **do not type the angle brackets**.
To read this guide in an editor: Mac, right-click the file, choose **Open With**, then **TextEdit**; Windows, **Open with**, then **Notepad**.
In a plain editor you will see marks such as `|` and `**`: ignore them. **Never copy the backtick marks** (the little slanted quote) around a command; copy only the words inside. Links do not click in a plain editor: copy the web address into your browser.

## 1. What you need first

**Open a terminal first.** Mac: press Command + Space, type `Terminal`, press Enter. Windows: press the Windows key, type `PowerShell`, press Enter.

| You need | Check it with (type in the terminal) | If it is missing |
| --- | --- | --- |
| Git, and your name and email set | `git --version`, then `git config user.name` and `git config user.email` (a blank line means "not set") | **Mac:** the first `git` command offers to install "command line developer tools": click **Install**, then **Agree**, wait 10 to 30 minutes, then run it again (ignore any note that mentions `xcode-select` or `sudo`). **Windows:** install Git from https://git-scm.com/downloads . Then set your details (do this before step 2, it is needed for the first commit): `git config --global user.name "Your Name"` and `git config --global user.email "you@example.com"` |
| Node.js 22 or newer | `node --version` | Go to https://nodejs.org/en/download and use the **macOS Installer (.pkg)** (Mac) or the **Windows Installer (.msi)** button under "prebuilt Node.js"; ignore the nvm commands. It may ask for your Mac password. Close the terminal and open a new one afterwards. |
| A paid Claude plan (Pro, Max, Team or Enterprise) or a Console account | the free claude.ai plan does **not** include Claude Code | https://claude.ai |
| Claude Code (Mac needs macOS 13 or newer) | `claude --version` | Run the install command for your computer, just below this table. Help: https://code.claude.com/docs/en/setup and, if terminals are new to you, https://code.claude.com/docs/en/terminal-guide |
| A GitHub account | you can log in at https://github.com | Sign up at https://github.com/signup (the page is for browsers) . It is used for one thing: a private backup of your notes |
| **Mac only:** GitHub sign-in for your terminal | `gh auth status` says you are logged in | Open https://github.com/cli/cli/releases/latest , scroll to **Assets**, click the file whose name ends `_macOS_universal.pkg`, open it, click Continue and Install (it asks for your Mac password). Do **not** use the zip offered on the front page of cli.github.com: it installs nothing. Then open a NEW Terminal window and run `gh auth login`. Choose **GitHub.com**, **HTTPS**, **Yes** (authenticate Git), **Login with a web browser**, press Enter to open the browser, paste the one-time code it shows, click **Authorize**. Run `gh auth status` again. (Apple's Git cannot sign in to GitHub by itself.) |
| Codex (optional) | `codex --version` | Not needed. Skip it. |

**Install Claude Code** (Mac, in Terminal). Copy the line only from this guide or from https://code.claude.com/docs/en/setup ; it never asks for your Mac password, and if it does, stop. If any other page offers a different install line, do not run it.

```text
curl -fsSL https://claude.ai/install.sh | bash
```

**Install Claude Code** (Windows, in PowerShell):

```text
irm https://claude.ai/install.ps1 | iex
```

When it finishes, close the terminal, open a new one and run `claude --version`: it should print a version number.
**Mac:** if it says `zsh: command not found: claude`, run these two lines, then `claude --version` again:

```text
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

## 2. Get your copy of the starter

1. **Unzip** `vibecoder-start.zip` (or `vibecoder-start-main.zip` if you used **Download ZIP** on GitHub).
   - Mac: double-click it in Finder. A folder `vibecoder-start` appears next to the zip (`vibecoder-start-main` if you downloaded it from GitHub). (Safari may unzip it by itself: if you also see `vibecoder-start 2`, keep one and delete the other.)
   - Windows: right-click the zip, choose **Extract All**, and in the box that opens **delete `\vibecoder-start` (or `\vibecoder-start-main`) from the end of the path** (so it extracts into, say, your user folder, not into a new folder of the same name inside itself), then click **Extract**.
2. **Rename** the folder `vibecoder-start` (or `vibecoder-start-main`) to your project's name, using letters, numbers and hyphens only. Below it is called `my-project`.
3. **Move it into your home folder** (Mac: in Finder press Shift + Command + H, or choose **Go**, then **Home**, to open your home folder, then drag the folder into that window; Windows: move it into `C:\Users\<your name>`). Do not leave it in Downloads, and do not use Desktop or Documents: Mac asks permission for those folders and some are synced to iCloud. Do this now: moving the folder later makes your notes look missing.
4. **Open a terminal inside that folder.**
   - Mac: open Terminal, type `cd ` (the letters c, d and a space), **drag the `my-project` folder from Finder into the Terminal window**, press Enter.
   - Windows: open the `my-project` folder in File Explorer, click the address bar at the top, type `powershell`, press Enter.
5. Check you are in the right place. Mac: type `ls`. Windows: type `dir`. You must see `README.md` and `docs` in the list. If not, repeat step 4.
6. Save the starter as it came:

   ```text
   git init -b main
   git add .
   git commit -m "Start from VibeCoder Start"
   ```

   The starter's rulebook says "stage files by name, never `git add .`". This first commit is the one exception.

Finder and File Explorer hide folders whose names start with a dot, such as `.claude`. That is normal: the folder is there. (Mac: Command + Shift + . shows it.)

## 3. Open Claude Code and paste the first-time prompt

In the same terminal window, type `claude` and press Enter. The first time, Claude Code asks a few questions: press Enter for the text style, choose **Claude account with subscription**, and sign in in the browser it opens. Press Enter on any other screen it shows (login successful, security notes); if it offers to set up your terminal, choose **Yes**. When it asks whether you trust this folder, press Enter on the highlighted **Yes**.

You are inside Claude Code when you see its welcome box; the plain terminal prompt ends with `%` (Mac) or `>` (Windows). Claude Code asks before it runs a command or changes a file. For the commands in this guide choose **Yes**. If it ever asks to delete folders or run something you do not recognise, choose **No** and ask it why. Expect 10 to 20 Yes/No questions in total. Two will look alarming and are fine: before the backup is uploaded the assistant runs a long search containing the words password, token and PRIVATE KEY (that checks that no secret leaves your computer: choose **Yes**), and `codex: command not found` only means the optional Codex tool is not installed.

Now paste these three lines **into Claude Code's chat** (Command + V on a Mac, Ctrl + V on Windows), put your own sentence after "I want to build", and press Enter:

```text
/first-session
I want to build: <one sentence>.
Set up this project and its memory. Stop and tell me when a step needs me.
```

The assistant works through eight steps and stops when it needs you. It reports each one in a line.

## 4. What you put in, and where

You do not edit files yourself. You answer, and the assistant writes. This is everything it needs from you:

| What | Where it ends up | Example | Who types it |
| --- | --- | --- | --- |
| What you want to build | `AGENTS.md`, section 1, "What this is" | A booking website for my dog-walking business | you answer question 1 |
| Who it is for | section 1, "Who it's for" | My customers, who book walks | you answer question 2 |
| What it is built with | section 1, "Stack" | `not chosen yet` (a fine answer) | you answer question 3 |
| How to run it and test it | section 1, "How to run it" and "How to test it" | `not yet` (a fine answer) | you answer question 4 |
| The address of your private backup | the notes folder's remote | `https://github.com/yourname/my-project-memory.git` | you create the repo, paste the address (step 5 below) |
| Real keys or passwords (later) | a file called `.env` that is never saved to Git; the names are listed in `.env.example` | `OPENAI_API_KEY=...` | you, in your own editor, never in the chat |

Good answers are short and in your own words. "Not chosen yet" and "not yet" are real answers; the assistant will help you choose later.
If your first line already says what you want to build, the assistant skips question 1.

## 5. Create your private backup (the assistant stops here and waits)

Your notes live in a folder outside the project, so they are backed up separately. Do this in your browser:

1. Open https://github.com/new
2. Repository name: `my-project-memory` (your project's name plus `-memory`, lowercase, no spaces)
3. Choose **Private**
4. Leave "Add a README file", ".gitignore" and "license" **unticked**
5. Click **Create repository**, copy the address that starts `https://github.com/` and ends `.git`, and paste it into the chat.
6. When the assistant sends your notes there, GitHub must know who you are. **Windows:** a browser window or pop-up may open asking you to sign in: do that, the assistant waits. **Mac:** this works only if you did the "GitHub sign-in for your terminal" row in step 1; if the push fails, open a second Terminal window, run `gh auth login` as described there, then tell the assistant `continue`.

You may type `skip` instead of an address. The notes are then saved only on this computer, and the assistant says so every time you save.

## 6. What a good result looks like

At the end the assistant reports, in order: the tools it found, your four answers, the notes folder, a gate line, and what it saved. The gate line begins:

```text
memory-check 3.2.11 [all]: 0 FAIL, 0 WARN
```

`0 FAIL` is what matters (`0 WARN` once the backup is connected). To end the session type `/exit` and press Enter; the terminal is still in your project folder, so type `claude` to start again, then paste the three lines from
[docs/01](01-every-session.md): the first reply starts with `memory gate: ok`.

## 7. Your first real task

Paste the three lines from docs/01 with a small goal, for example `Today I want to: add a home page with my name and one sentence about the business.` The assistant plans first and waits for your go before it builds.

Optional helper: type `/plugin install superpowers@claude-plugins-official` in Claude Code (the starter's planning step uses it; if it fails, skip it). It may ask you to restart: type `/exit`, then `claude`.

## If something goes wrong

Paste this and change nothing until the assistant answers:

```text
Something went wrong. Change no file yet.
Run git status --short and node scripts/memory/memory-check.mjs, read AGENTS.md, and tell me in plain words
where this project stands, what you think happened, and the one safest next step. Then wait for my go.
```

| You see | Do this |
| --- | --- |
| `memory gate: memory dir not found; run node scripts/memory/memory-init.mjs` | The notes folder does not exist on this computer yet. First see where it would go: `node scripts/memory/memory-init.mjs --dry-run` (the first line, `memory: <folder> (default)`, is the folder, without the `(default)`). If you already have a private backup, restore it there: `git clone <your backup address> "<that folder>"`. If you have never made one, run `node scripts/memory/memory-init.mjs`. Never both. |
| `memory already exists` | Nothing to do, as long as the check that follows shows no `repo.not-git` line. If it does, a stray file from Claude Code's own memory feature is sitting where your notes go: tell the assistant `Rename the notes folder to memory-old (do not delete it), then run memory-init again.` and choose **Yes**. |
| `...is not empty and has no MEMORY.md` | Same cause: the notes folder holds files that are not a memory yet. Tell the assistant `Rename the notes folder to memory-old (do not delete it), then run memory-init again.` and choose **Yes**. |
| `FAIL router.missing` | The notes folder exists but is empty. Run `node scripts/memory/memory-init.mjs`: it fills an empty folder. |
| The first reply does not mention the memory gate | Claude Code was open before the setup. Type `/exit`, then `claude` again in the project folder. |
| A line starting `FAIL` | Copy the whole line into the chat and say: "Fix this memory gate FAIL." |
| `command not found: node` (Mac) or `node is not recognized` (Windows) | Close the terminal and open a new one after installing Node. If it still fails, reinstall from https://nodejs.org . |
| `zsh: command not found: claude` (Mac) | Run the two PATH lines shown under the install command in step 1, then `claude --version`. |
| `git: command not found`, or a window about developer tools (Mac) | Click **Install** in that window, wait for it to finish, then run the command again. |
| `Please tell me who you are` or `Author identity unknown` | Run the two `git config --global` lines from the Git row in step 1, then repeat the `git commit`. |
| `not a git repository` | You are in the wrong folder. Repeat step 2.4. |
| `Authentication failed`, `could not read Username` or `Device not configured` | Git is not signed in to GitHub. **Mac:** open a second Terminal window, run `gh auth login` (step 1), then tell the assistant `continue`. **Windows:** run `git ls-remote <your backup address>`, sign in when asked, then tell the assistant `continue`. |
| You moved or renamed the project folder | The notes folder is named after the project's path, so it looks "missing". Do **not** run `memory-init` yourself. Ask the assistant: `Move my old notes folder to the new name.` (Claude Code may already have made an empty folder at the new name; the assistant replaces it.) |
| Anything else | Paste the exact text and say which step you were on. |

## By hand (only if the assistant misbehaves)

```text
node scripts/memory/memory-init.mjs
git -C "<notes folder>" remote add origin <backup address>
git -C "<notes folder>" push -u origin HEAD
node scripts/memory/memory-check.mjs
```

`<notes folder>` is the path on the `memory:` line printed by the first command (keep the quotes if it contains spaces). Then fill the `<...>` lines in `AGENTS.md` section 1 yourself.
