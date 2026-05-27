# Path: Agent Smith

Full MCP-based pentest framework. Structured, orchestrated, opinionated.
Skills behind a Model Context Protocol server, with the framework
deciding when to chain into recon, web exploitation, post-exploitation,
and reporting.

Upstream: `https://github.com/0x0pointer/agent-smith`

**Why pick this:** you want to test a real framework end-to-end, see
how MCP-backed agents differ from skill-only agents, and have the
orchestration handled for you.

**Weakness:** heavy install. MCP server needs to be running and wired
into your client. Token spend is higher than Skill fork for the same
work because MCP overhead is real.

## What you need

- Claude Code (or OpenCode for any LLM provider) — cloud only
- Kali Linux (the framework expects standard offensive tooling on the
  host)
- `uv`, `tmux`, `jq`, Poetry (for the upstream installer)
- An active API key for your chosen client
- Setup from [`../../SETUP.md`](../../SETUP.md) complete

This path **cannot run on Ollama-only.** If you only have local
models, pick [Raw agent](../raw-agent/README.md) or
[Build your own](../build-your-own/README.md).

## Cost expectation

MCP servers add token overhead per turn. Recommend Claude Max 5×
($100/mo) at minimum for comfortable runs; Claude Pro ($20) will work
for one box but you may hit the limit on the HTB phase.

## Install

```bash
git clone --recursive https://github.com/0x0pointer/agent-smith
cd agent-smith
./installers/install.sh            # Claude Code
# or, for OpenCode:
# ./installers/install_opencode.sh
```

For custom MCP clients (build it yourself):

```bash
poetry install
poetry run python -m mcp_server
```

**Critical:** fully restart your client after install. The MCP server
connects at startup, not on the fly.

Read the upstream README the morning of — installer scripts evolve, and
this repo's notes can lag.

## Invocation

The framework exposes a single orchestrator slash command and several
specialized sub-skills.

### Orchestrator

```text
/pentester scan <target-url-or-ip> depth=thorough
```

This runs the full chain: OSINT → recon → web exploit → post-exploit →
reporting. The agent decides each pivot from the previous step's output
and generates `findings.json`, Burp-ready PoCs in `pocs/`, topology
diagrams, a coverage matrix, and patch-ready code fixes.

Optional arguments (check `/pentester --help` for the current set):

- `depth=recon|standard|thorough` — coverage vs time tradeoff
- `max_cost_usd=N` — hard token-spend cap
- `max_time_minutes=N` — hard wall-clock cap
- `context="<freeform>"` — provided creds, scope notes, hints

For a known authenticated target:

```text
/pentester scan <target-ip> depth=thorough context="provided creds: admin:password. focus on SQLi and command injection pages."
```

### Sub-skills (when you want to drive)

If you want to invoke a specific phase instead of the full chain:

- `/recon <target>` — discovery and fingerprinting
- `/web-exploit <target>` — web vuln class sweep
- `/ad-assessment <target>` — Active Directory
- `/lateral-movement` — post-foothold pivot
- `/post-exploit` — escalation, persistence, evidence
- `/credential-audit` — auth surface

## Against the workshop labs

Warm-up (DVWA or Juice Shop) — see [`../../LABS.md`](../../LABS.md):

```text
/pentester scan http://localhost/ depth=standard
# or, Juice Shop:
/pentester scan http://localhost:3000/ depth=standard
```

HTB:

```text
/pentester scan <box-ip> depth=thorough max_time_minutes=40
```

The `max_time_minutes` cap is your circuit breaker. Without it, an
Agent Smith run can burn through a workshop budget on a stuck
sub-agent.

## When it gets stuck

Same five steering moves as the [Raw agent](../raw-agent/README.md)
path — state summary, focus narrowing, evidence forcing, hypothesis
listing, reset to checkpoint. Framework or not, the model is still the
model.
