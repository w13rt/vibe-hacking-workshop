# Path: Build your own

Project `CLAUDE.md` (or `AGENT.md`) that you author yourself by having
the agent interview you. You encode your methodology into a file the
agent loads, so the same scaffold works across engagements.

**Why pick this:** you already have a methodology — a tool you always
reach for, a workflow that works, a writeup format — and you want the
agent to follow it without re-prompting every time.

**Weakness:** you can burn the lab budget building abstractions. Time-box
yourself.

## What you need

- Same as [Raw agent](../raw-agent/README.md): an agentic CLI, a Kali
  VM, the setup from [`../../SETUP.md`](../../SETUP.md)
- A clear sense of what you want to encode. If you do not have one yet,
  pick [Raw agent](../raw-agent/README.md) for the workshop and come
  back to Build-your-own later.

## Cost expectation

Same as Raw agent. Subscription cost is driven by tokens, not by which
path you pick. A focused `CLAUDE.md` **saves** tokens compared to
re-prompting context every run.

## The building block

### `CLAUDE.md` — persistent project context

Markdown loaded recursively from the directory tree like a system prompt.
Lives at the root of your engagement directory (or your home directory,
or both — Claude Code loads them in order).

Typical contents:

- Personality — terse, evidence-driven, no flattery
- Work style — how you want the agent to keep notes, when to ask
- Environment — VM details, network constraints, paths
- Tooling — some might need mentioning, special non-standard Kali tools


You author this file by running the prompts in
[`prompts.md`](prompts.md) — the agent interviews you, drafts a
`CLAUDE.md`, then critiques and trims its own draft. Do not paste a
generic template; what survives an engagement unchanged is yours.

## Setup

```bash
mkdir -p ~/H4CKINGB0T
cd ~/H4CKINGB0T
# CLAUDE.md will live here. Start your agent in this directory,
# then open paths/build-your-own/prompts.md and paste the first prompt.
```

## Invocation

Once `CLAUDE.md` carries identity, state model, and tool philosophy, the
first prompt of an engagement collapses to one line:

```text
Target: <target-url-or-ip>. Get a shell and capture proof.
```

Lab targets and reachability checks: [`../../LABS.md`](../../LABS.md).

## Optional ideas

Once your `CLAUDE.md` works, these are common next steps. None are
required for the workshop.

### Skills — methodology as code

Markdown files with YAML frontmatter, dynamically loaded by description
match. One file per skill (e.g. `recon.md`, `web-exploit.md`,
`reporting.md`).

Two things make a skill useful:

1. The **description** triggers loading. A vague description is a dead
   skill. Use "use when X" phrasing.
2. The **content** is methodology, not theory. Tools, flags you prefer,
   exact commands, gotchas.

Prompt 5 in [`prompts.md`](prompts.md) bootstraps a first skill. For a
worked library, look at the upstream Skill fork
(`https://github.com/w13rt/skills`) — it is exactly this pattern.

### Cross-engagement lessons store

Keep a lessons file you reload at the start of each engagement (e.g.
`~/.engagements/lessons.md`) and append to it at the end. Index by
target class (HTB Linux easy, internal AD, web monolith, mobile, …)
rather than by client name. Helps the next engagement; helps you train
your own skills.

### Interactive shells via tmux

Agents drive non-interactive shells well, but choke on `msfconsole`,
`gdb`, interactive SSH, anything with a TUI. The workaround: have the
agent drive a long-lived tmux session — `tmux send-keys` to type,
`tmux capture-pane` to read. Encode the pattern as a skill so the agent
reaches for it automatically. The upstream Skill fork has a `tmux`
skill as a worked example.

## Workshop-day discipline

You are using the lab phase to find out **which conventions of yours
survive contact with a real target**. Note what would change and
mention it in the closing discussion. Do not spend more than 10
minutes building scaffolding on the day — that is a between-engagements
task.
