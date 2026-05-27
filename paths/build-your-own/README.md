# Path: Build your own

Project `CLAUDE.md` (or `AGENT.md`) plus your own skills. You encode
your methodology into files the agent loads, so the same scaffold
works across engagements.

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
path you pick. The skills you write **save** tokens (the agent reaches
for them only when relevant) compared to a giant prompt.

## The building blocks

### `CLAUDE.md` — persistent project context

Markdown loaded recursively from the directory tree like a system prompt.
Lives at the root of your engagement directory (or your home directory,
or both — Claude Code loads them in order).

Typical contents:

- Tooling — what tools are installed, which versions
- Work style — how you want the agent to keep notes, when to ask
- Environment — VM details, network constraints, paths
- Personality — terse, evidence-driven, no flattery

A minimal starter is in [`starter-CLAUDE.md`](starter-CLAUDE.md). Fork it.

### Skills — methodology as code

Markdown files with YAML frontmatter, dynamically loaded by description
match. One file per skill (e.g. `recon.md`, `web-exploit.md`,
`reporting.md`).

Two things make a skill useful:

1. The **description** triggers loading. A vague description is a dead
   skill. Use "use when X" phrasing.
2. The **content** is methodology, not theory. Tools, flags you prefer,
   exact commands, gotchas.

If you want a worked example, look at the upstream Skill fork
(`https://github.com/w13rt/skills`) — it is exactly this pattern.

### Cross-engagement lessons store

Keep a lessons file you reload at the start of each engagement (e.g.
`~/.engagements/lessons.md`) and append to it at the end. Index by
target class (HTB Linux easy, internal AD, web monolith, mobile, …)
rather than by client name. Helps the next engagement; helps you train
your own skills.

## Setup

```bash
mkdir -p ~/engagements/workshop
cd ~/engagements/workshop
cp <path-to-this-repo>/paths/build-your-own/starter-CLAUDE.md ./CLAUDE.md
$EDITOR CLAUDE.md       # adjust identity, environment, work style
mkdir skills            # optional — drop your own per-skill files here
```

Then start your agent from this directory.

## Invocation

Because the `CLAUDE.md` already carries identity, state model, and tool
philosophy, the first prompt collapses to one line:

```text
Target: <target-url-or-ip>. Get a shell and capture proof.
```

Lab targets and reachability checks: [`../../LABS.md`](../../LABS.md).

## Workshop-day discipline

You are using the lab phase to find out **which conventions of yours
survive contact with a real target**. Note what would change and
mention it in the closing discussion. Do not spend more than 10
minutes building scaffolding on the day — that is a between-engagements
task.
