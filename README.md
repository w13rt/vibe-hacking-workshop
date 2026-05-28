# Vibe Hacking workshop

Using AI for human-in-the-loop and agentic offensive security.

A 2.5-hour hands-on + discussion workshop. You pick one of four paths,
verify your agent works, then point it at a vulnerable target.

## What this repo gives you

The minimum content you need to walk in, pick a path, and run an agent
against the workshop labs. It is not the slide deck; it is the working
material.

- [`SETUP.md`](SETUP.md) — what to install before the day
- [`LABS.md`](LABS.md) — the warm-up target (DVWA or Juice Shop) and the
  main HTB phase
- [`paths/`](paths/) — one folder per path, with setup and invocation
- [`REFERENCES.md`](REFERENCES.md) — external links: agentic CLIs,
  frameworks, vulnerable targets, and methodology reading

## The four paths

Pick one based on what you want to learn and what you have available.

| Path | What it is | When to pick it |
|---|---|---|
| [Raw agent](paths/raw-agent/) | Claude Code / Codex / Aider with shell access, no framework | You want to learn the operator loop. Minimal setup. Default. |
| [Build your own](paths/build-your-own/) | Project `CLAUDE.md` plus your own skills | You already have a methodology you want to encode |
| [Agent Smith](paths/agent-smith/) | Full MCP-based pentest framework | You want to test a real framework end-to-end |
| [Skill fork](paths/skill-fork/) | Stripped-down Agent Smith — skills only, no MCP | You want methodology without the MCP overhead |

If you have no preference, pick Raw agent.

## Local vs cloud — which paths support which

Hard constraint: **fully local must work**. Not every path supports it.

| Path | Cloud (Claude Code / Codex) | Local (Ollama + Aider/Cline) |
|---|---|---|
| Raw agent | yes | yes |
| Build your own | yes | yes |
| Agent Smith | yes | technically yes via OpenCode, but needs a high-tier local model (70B+) |
| Skill fork | yes | no (needs Claude Code) |

If you have no AI subscription and a 16 GB laptop, you are constrained
to **Raw agent** or **Build your own**, both running Aider+Ollama.
See [`SETUP.md`](SETUP.md) for the local-model picks.


## Before the day

Run through [`SETUP.md`](SETUP.md) and finish the pre-flight checklist.
Cold-installing on the day eats the whole lab phase.
