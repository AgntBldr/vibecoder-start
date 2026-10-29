# Models and tokens

## 1. The routing table

A model is the AI system doing the work. Choose it for the step ahead. A premium model is a bigger one that costs more per token; a cheaper model costs less per token. This template gives bigger models planning, architecture (how the parts of the project fit together), security and final review, and less costly models bulk edits, scaffolding (creating the starting files), renames and docs. Section 4 explains why.

The names and commands were read from these official pages on 2026-10-26 and may change, so check them again before relying on a name. They are a dated reference, not a promise that every account offers every model.

- https://code.claude.com/docs/en/model-config
- https://platform.claude.com/docs/en/about-claude/models/overview
- https://learn.chatgpt.com/docs/models

| Claude work | Model alias | Model it means today |
| --- | --- | --- |
| Planning, architecture, security and final review | `opus` | Claude Opus 5.5 |
| Long, demanding work in those roles | `fable` | Fable 5.1 |
| Bulk edits, scaffolding and docs | `sonnet` | Sonnet 5.5 |
| Quick lookups and renames | `haiku` | Haiku 5.5 |

| Codex work | Model |
| --- | --- |
| Complex coding and agent work | `gpt-6.1-sol` |
| Focused, repeatable tasks | `gpt-6-luna` |

Agent work means work where Codex runs commands and edits files by itself. If you choose a Codex model yourself, this guide suggests `gpt-6.1-sol` for demanding build steps and `gpt-6-luna` for focused, repeatable ones; `/codex-build` itself uses your Codex default (section 2).

Choosing a model does not change who does what: Claude plans, designs, orchestrates and reviews, and Codex builds from a written plan; the builder never reviews its own work. See [How work flows](02-how-work-flows.md).

## 2. How to switch

Type slash commands inside the assistant's session. Type commands starting with `claude` or `codex` in your terminal, the window where you run commands. Replace `<alias>`, `<model>` and `<prompt>` with your chosen alias, model name and task text; do not type the angle brackets.

| Claude command | What it does |
| --- | --- |
| `/model` | Opens the model picker. |
| `/model <alias>` | Switches model and keeps it as your default. |
| `claude --model <alias>` | Sets the model for one session. |

The aliases are `opus`, `sonnet`, `haiku`, `fable` and `opusplan`. The `opusplan` alias uses Opus while planning and Sonnet while building. It does not replace this template's Claude-to-Codex handoff or its separate reviewer rule.

Claude's `/effort` controls thinking effort (how hard the model thinks before it answers), with `low`, `medium`, `high`, `xhigh` or `max`.

| Codex command | What it does |
| --- | --- |
| `codex -m <model>` | Sets the model for one run. |
| `codex exec -m <model> "<prompt>"` | Sets the model for one task. |
| `/model` | Lets you choose a model inside a session. |

`/codex-build` and `/cross-review` run `codex exec` for you without `-m`, so they use your Codex default (the `config.toml` setting below) or Codex's own recommended pick; you rarely type `codex exec` yourself. To change what they use, change the default, or ask Claude to. `codex -m` and `codex exec -m` are for when you run Codex directly.

For Codex's default, set `model = "<model>"` in `~/.codex/config.toml`. That is a setting in a file, not a terminal command; `~` means your home folder. You can also set thinking effort, for example `model_reasoning_effort = "medium"`. You can ask Claude to make either `config.toml` edit for you. With no model set, Codex picks a recommended one.

## 3. Ask “tell me which model and why”

Before each big step, say: **“tell me which model and why”**. Expect an answer before work begins: the model name, the job it will do, and why that choice fits. For example, planning needs careful decisions; a rename follows decisions already made.

The assistant should push back if you ask for the wrong model for the work. When your change is reviewed, it reports who built it and who reviewed it.

## 4. The one-line cost model

Every word in and out costs tokens, and a bigger model costs more per token, so give it the thinking and give the typing to the cheaper one.

In the routing table, the thinking is planning, architecture, security and final review; the typing is bulk edits, scaffolding, renames and docs.

A token is a small piece of text, sometimes only part of a word. Your request, the material the assistant reads and its answer all contribute to usage. This is a simple budgeting rule; whether usage creates a separate bill depends on how you signed in.

## 5. How your plan limits work

Here, “plan” means your paid subscription, not the written plan for building a feature. The billing pages are:

- https://code.claude.com/docs/en/costs
- https://learn.chatgpt.com/docs/auth

In short (paraphrased from the pages above):

> on a Claude Pro or Max plan, Claude Code usage is included in the subscription up to its limits; with a Console account or API key it is billed per token

> Codex signed in with ChatGPT uses your plan's included usage; signed in with an API key it is billed at API rates on your OpenAI Platform account

A Console account is a developer account billed by use; an API key is a secret code that bills usage to such an account and belongs in `.env`, never in a file you commit.

Included usage has limits; it does not mean unlimited work. These pages give no numeric allowance, so this guide does not promise a number of tasks or tokens. If you signed in with your Claude or ChatGPT plan, the plan route applies; ask Claude if you are unsure. If Codex runs out of included usage, see [How work flows](02-how-work-flows.md), section 5.
