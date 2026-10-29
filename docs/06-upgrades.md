# Upgrades

None is needed to start. Add one at a time. Some packs say "use my skill first", before any other. Superpowers, the plugin this template expects, does too, so another such pack competes with it. A plugin is an add-on giving the assistant tools or instructions; a skill is saved instructions for one job.

Commands starting with `/` are typed inside Claude Code (or Codex); those starting with `claude`, `codex` or `git` go in your terminal, the window where you run commands. Details were read from each project's page on 2026-10-26 and may change. Costs mean setup effort and extra AI usage, not quoted prices.

## ECC

**What it is**

[ECC](https://github.com/affaan-m/ECC) packs agents (assistants carrying out multi-step jobs on their own), 300+ skills and hooks (automatic actions) for Claude Code, Codex and others.

**When you'd want it**

When you want one large ready-made collection instead of superpowers.

**Cost and complexity**

High: skill and hook listings take much of the text an AI can consider each session. Needs Node 18+, a program running JavaScript tools; npm comes with it and installs those tools. Its README warns: official install channels only. Claude Code:

```
/plugin marketplace add https://github.com/affaan-m/ECC
/plugin install ecc@ecc
```

**Clash warning**

Its "use my skill first" rules fight superpowers'. Add it only if ready to use it instead of superpowers.

## gstack

**What it is**

[gstack](https://github.com/garrytan/gstack) provides 23 opinionated role tools, including CEO, designer, release manager and quality checker.

**When you'd want it**

When you want separate roles to examine product and release decisions.

**Cost and complexity**

Medium to high: large skill files and an extra program runner, Bun 1.4.2+. Windows also needs Node. In a bash-style terminal (macOS or Linux, or Git Bash on Windows), copy its repository (project files and history), enter it, then run setup; in PowerShell, follow gstack's README instead:

```
git clone https://github.com/garrytan/gstack ~/.claude/skills/gstack
cd ~/.claude/skills/gstack
./setup
```

This installs into your user folder, affecting every project, unlike this template's project-only rule for found skills.

**Clash warning**

Its installation asks you to add a rule never to use Claude in Chrome tools. Check whether you want browser control first.

## pstack

**What it is**

[pstack](https://github.com/cursor/plugins/tree/main/pstack) contains a Cursor (an AI code editor) engineer's daily skills, including poteto-mode (the pack's main working-style skill) and its principles.

**When you'd want it**

When you want that engineer's session-start routine and principle skills as your working style.

**Cost and complexity**

Medium: a required session-start instruction and many principle skills. Claude Code:

```
/plugin marketplace add cursor/plugins
/plugin install pstack@cursor-plugins
```

**Clash warning**

Its mode skill runs alongside superpowers', so you get two sets of session-start instructions. Add it on its own and watch for conflicts.

## claudex-loop

**What it is**

[claudex-loop](https://github.com/chaseai-yt/claudex-loop) is a hardened planning, Codex review, building and inspection loop: a bigger version of this template's cross-review and codex-build skills.

**When you'd want it**

When this template's review limits (3 rounds for a plan, 2 for a change) are too few for your work; see [How work flows](02-how-work-flows.md).

**Cost and complexity**

Medium: needs Python 3.10+ (a programming language some tools need) and both Claude Code and Codex signed in. More review rounds use more Codex usage. Claude Code:

```
/plugin marketplace add chaseai-yt/claudex-loop
/plugin install claudex-loop@claudex-loop
```

**Clash warning**

It ships codex-build and codex-review skills. Adopting it means choosing between its codex-build and this template's; extra review rounds go past this template's limits.

## agent-skills

**What it is**

[agent-skills](https://github.com/addyosmani/agent-skills) offers 25 engineering skills for Claude Code and Codex.

**When you'd want it**

When you want ready-made engineering skills, such as review or planning, instead of superpowers' versions of those jobs.

**Cost and complexity**

Low to medium: skills load only when used. Claude Code:

```
/plugin marketplace add addyosmani/agent-skills
/plugin install agent-skills@addy-agent-skills
```

Codex:

```
codex plugin marketplace add addyosmani/agent-skills
codex plugin add agent-skills@agent-skills
```

**Clash warning**

Review and planning skills can compete with superpowers'. Pick one skill for each job.

## UTCP code-mode

**What it is**

[UTCP code-mode](https://github.com/universal-tool-calling-protocol/code-mode) is a code library that lets an agent call tools by writing code instead of making one call at a time.

**When you'd want it**

Only when your project calls many tools.

**Cost and complexity**

High: a developer library, not a plugin you simply turn on. Needs Node and npm.

**Clash warning**

No template clash is identified; it changes how tools are called and needs development work.

## Linear

**What it is**

[Linear's connection](https://linear.app/docs/mcp) uses MCP (a standard way to connect the assistant to another service) to let Claude Code work with Linear issues, which track tasks.

**When you'd want it**

When your team already tracks work in Linear.

**Cost and complexity**

Low setup effort; needs a Linear account. In your terminal:

```
claude mcp add --transport http linear-server https://mcp.linear.app/mcp
```

Then start Claude Code and, inside it, type `/mcp` to sign in.

**Clash warning**

None identified. Following this template's rules, propose the project structure before creating projects or issues.

## Browser control

**What it is**

[Claude in Chrome](https://claude.com/chrome) and [Playwright MCP](https://github.com/microsoft/playwright-mcp) let the assistant control a browser.

**When you'd want it**

When you need the assistant to inspect and try a website.

**Cost and complexity**

Low to medium; screenshots use many tokens, units of AI input and output. Claude in Chrome needs a paid plan; start with:

```
claude --chrome
```

Playwright MCP needs Node 18+; connect with:

```
claude mcp add playwright npx @playwright/mcp@latest
```

**Clash warning**

gstack asks you not to use Claude in Chrome.

## A careful-mode hook

**What it is**

A [PreToolUse hook](https://code.claude.com/docs/en/hooks-guide) runs before a tool action and can block or ask before risky commands, such as `git add -A` (stages every file at once) or `git reset` on work you did not create, both forbidden by this template's rules.

**When you'd want it**

When you want automatic enforcement of AGENTS.md section 5; [Git without tears](04-git-without-tears.md) explains those rules.

**Cost and complexity**

Low: ask Claude to write the small script; it goes beside the existing session-start hook in `.claude/settings.json`.

**Clash warning**

A hook that blocks too much stops normal work. Start with "ask".

## codex exec resume

**What it is**

[codex exec resume](https://learn.chatgpt.com/docs/non-interactive-mode) continues a previous Codex run:

```
codex exec resume --last "<next instruction>"
```

Replace `<next instruction>` with your own text; do not type the angle brackets.

**When you'd want it**

For your own side runs of Codex, to continue one without pasting its history again.

**Cost and complexity**

Low: fewer tokens than re-sending history.

**Clash warning**

Never use it inside `/codex-build` or `/cross-review`: the reviewer must not continue the builder's run (see [How work flows](02-how-work-flows.md), section 2).

## A curated skills list

**What it is**

GitHub repositories starred (bookmarked) by this template's author, GitHub user AgntBldr: the [skills and superpowers list](https://github.com/stars/AgntBldr/lists/skills-superpowers), plus [all of their lists](https://github.com/AgntBldr?tab=stars).

**When you'd want it**

Browse before building a reusable skill, tool or integration (connecting tools). This template looks for existing options first with:

```
/find-skills
```

**Cost and complexity**

None to browse.

**Clash warning**

Anything found is untrusted until read. Follow that skill: show you its SKILL.md before running its commands; install only after an explicit yes, for this project, never globally.
