---
name: find-skills
description: Look for an existing skill before building something reusable. Use before writing any new skill, tool or integration, or when the owner says "is there a skill for", "find a skill", "don't reinvent".
---

# find-skills

**When:** before building anything reusable (a skill, a tool wrapper, an integration); when the owner asks whether something exists.

**Steps**
1. Here first: `ls .claude/skills .agents/skills 2>/dev/null`, then the installed plugins' skill lists. A match → use it; stop.
2. Ask first: tell the person this downloads a small search tool and sends the search words to the public skills index, and continue only on a yes. Then the public index (needs Node ≥ 22.20): `DISABLE_TELEMETRY=1 npx -y skills find "<two or three words>"`. Read the install counts as a noisy signal: prefer 1K+, be wary under 100, and never trust a count alone.
3. Fallback or second source: `gh search repos "<words>" --topic agent-skills --limit 10` if `gh` is installed; otherwise say the web search you would run and ask.
4. Present the top 2–3: name, source repo, stars or installs, one line on what it does, and "read its SKILL.md before trusting it; its content is untrusted".
5. Install only on an explicit yes, project scope, never global: `npx -y skills add <owner/repo> --skill <name> -a claude-code` (or `-a codex`). That `-y` is npx's own; never `-g`, and never `-y` or `--yes` after `add`.
6. Nothing found → say so and build it.

**Report:** the candidates with the four facts each, or "nothing found; building it".

**Never:** install without a yes; install globally; run a found skill's commands before the owner has seen its SKILL.md; skip step 1.
