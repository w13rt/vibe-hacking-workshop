# Path: Skill fork

A stripped-down version of Agent Smith. Keeps the skill library, drops
the MCP server. Skills become slash commands; the agent calls bash and
files directly.

Upstream: `https://github.com/w13rt/skills`

**Why pick this:** you want the methodology Agent Smith encodes
(`/pentester`, `/web-exploit`, `/recon`, …) without the MCP install
burden. Lighter, faster, fewer moving parts.

**Weakness:** no MCP-backed consistency. Skills depend on the agent
reading and following them — descriptions matter.

## What you need

- Claude Code (cloud only) — the fork is canonical Claude-Code-native;
  a Codex variant exists via `migrate_codex.py`
- Kali Linux with standard offensive tooling
- `uv`, `tmux`, `jq` for some skills
- An active API key
- Setup from [`../../SETUP.md`](../../SETUP.md) complete

This path **cannot run on Ollama-only.** Pick
[Raw agent](../raw-agent/README.md) or
[Build your own](../build-your-own/README.md) if you only have local
models.

## Cost expectation

Lower than Agent Smith for the same work (no MCP overhead). Claude Pro
($20/mo) is enough for the warm-up and a comfortable HTB run; Claude
Max 5× ($100) gives you headroom.

## Install

For Claude Code:

```bash
git clone https://github.com/w13rt/skills ~/skills
ln -s ~/skills ~/.claude/skills
```

For Codex CLI:

```bash
git clone https://github.com/w13rt/skills ~/skills
cd ~/skills && uv run python migrate_codex.py
ln -s ~/skills/codex-build ~/.codex/skills
```

Both symlinks can coexist on the same machine. The source tree is
Claude-Code-native; the Codex tree is generated.

Then start your agent from the working directory where you want
artifacts written:

```bash
mkdir -p ~/engagements/workshop
cd ~/engagements/workshop
claude            # or: codex
```

## Invocation

### Orchestrator

```text
/pentester scan <target-url-or-ip> depth=thorough
```

Optional arguments (check the upstream README for the canonical list):

- `depth=recon|standard|thorough`
- `max_cost_usd=N`
- `max_time_minutes=N`
- `context="<freeform>"`

For a known authenticated target:

```text
/pentester scan <target-ip> depth=thorough context="provided creds: admin:password. focus on SQLi and command injection pages."
```

### Specialized skills

Each skill is a slash command in its own right:

- `/recon`
- `/web-exploit`
- `/api-security`
- and others — check `~/skills/` to see what is installed

## Against the workshop labs

Warm-up — see [`../../LABS.md`](../../LABS.md):

```text
/pentester scan http://localhost/ depth=standard
# or, Juice Shop:
/pentester scan http://localhost:3000/ depth=standard
```

HTB:

```text
/pentester scan <box-ip> depth=thorough max_time_minutes=40
```

## Skill fork vs Agent Smith — when to pick which

Both expose `/pentester`. The difference is what is behind it.

| | Skill fork | Agent Smith |
|---|---|---|
| Install | clone + symlink | clone + installer + client restart |
| MCP server | none | yes |
| Token overhead | low | higher (MCP frames every call) |
| Consistency across runs | slightly lower | slightly higher |
| Best for | "I want methodology, not infrastructure" | "I want to test a real framework end-to-end" |

If you are unsure, start with Skill fork. You can move to Agent Smith
later if you want the orchestration layer.
