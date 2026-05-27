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

- One MCP-capable client — cloud only:
  - Claude Code (best UX, native skill support), **or**
  - OpenCode (bring-your-own LLM: OpenAI, Anthropic, Google,
    OpenRouter, etc.), **or**
  - a custom MCP client — Cursor, Continue, Zed, or a custom Agent SDK
    app
- **Docker Desktop, running.** All scanners run in sandboxed
  containers — there is no requirement to install pentest tooling on
  the host.
- **Poetry** — `curl -sSL https://install.python-poetry.org | python3 -`
- Node.js v18+ — optional, enables server-side Mermaid pre-rendering
- An active API key for your chosen client
- Setup from [`../../SETUP.md`](../../SETUP.md) complete

Kali Linux is recommended as the workshop pentesting environment but
is **not a hard requirement of Agent Smith itself** — any
Docker-capable host works, since scanner tooling lives in containers.

**Local models:** Agent Smith technically runs Ollama-only via
OpenCode (which supports Ollama, llama.cpp, vLLM, and custom
endpoints). In practice, driving a 27-skill MCP framework needs a
strong tool-use model — the workshop's high-tier local pick
(`llama3.3:70b` / `qwen3:72b`) is the realistic floor. The low and
mid tiers (8B–30B) will install but tend to stall on multi-skill
chains. If you're on a 16 GB laptop, pick
[Raw agent](../raw-agent/README.md) or
[Build your own](../build-your-own/README.md) instead.

## Cost expectation

MCP servers add token overhead per turn. Recommend Claude Max 5×
($100/mo) at minimum for comfortable runs; Claude Pro ($20) will work
for one box but you may hit the limit on the HTB phase.

## Install

Follow the upstream install steps — they are the source of truth and
change between releases:

**→ <https://github.com/0x0pointer/agent-smith#installation>**

The short version: clone with `--recursive`, run the installer for
your client (`install.sh` for Claude Code, `install_opencode.sh` for
OpenCode, or `poetry install` + `poetry run python -m mcp_server` for
a custom MCP client like Cursor/Continue/Zed), then optionally
prebuild the `kali-mcp` and `metasploit` scanner containers.

Heavy install — multi-GB Docker images and Poetry deps. **Do this
before the workshop**, not on the day. See [`../../SETUP.md`](../../SETUP.md)
§1 and the pre-flight checklist.

**Critical:** fully restart your client after install. The MCP server
connects at startup, not on the fly.

## Invocation

The framework exposes a single orchestrator slash command and a large
catalogue of specialized sub-skills (~27 at time of writing).

### Orchestrator

```text
/pentester <target-url-or-ip>
```

This runs the full chain: OSINT → discovery → web exploit →
post-exploit → reporting. The agent decides each pivot from the
previous step's output and generates `findings.json`, Burp-ready PoCs
in `pocs/`, topology diagrams, a coverage matrix, and patch-ready code
fixes.

For scope notes, creds, or focus hints, just say so in plain prose
after the command — for example:

```text
/pentester <target-ip>
> provided creds: admin:password. focus on SQLi and command injection pages.
```

Run `/pentester --help` inside the client for the current flag set —
it changes between releases.

### Sub-skills (when you want to drive)

Invoke a specific phase instead of the full chain. Most-useful for the
workshop labs:

- `/osint` — passive discovery, subdomain takeover, cert transparency,
  leaked creds
- `/network-assess` — VLAN hopping, LLMNR/NBT-NS, SNMP, segmentation
- `/web-exploit` — SQLi, XSS, SSRF, SSTI, deserialization
- `/api-security` — OWASP API Top 10 across REST/GraphQL/gRPC/SOAP/MCP
- `/param-fuzz` — auth stripping, type confusion, mass assignment
- `/business-logic` — value/quantity abuse, workflow bypass, BOLA/BFLA
- `/ad-assessment` — ADCS (ESC1–ESC8), BloodHound, GPO, LAPS
- `/credential-audit` — brute force, spraying, defaults, lockout, MFA
  bypass
- `/post-exploit` — Linux/Windows privesc and persistence
- `/lateral-movement` — PTH, PTT, Kerberoasting, NTLM relay
- `/metasploit` — exploit validation in an isolated Docker container
- `/reverse-shell`, `/pivot-tunnel` — post-RCE shells and SOCKS5
  tunneling
- `/codebase` — OWASP ASVS 5.0 white-box review (bring source)
- `/ai-redteam` — OWASP LLM Top 10 + AITG + MCP runtime attacks
- `/remediate` — auto-generated code patches for confirmed findings

Upstream also ships `/cloud-security`, `/container-k8s-security`,
`/email-security`, `/ssl-tls-audit`, `/threat-modeling`,
`/analyze-cve`, `/request-cves`, `/gh-export`, `/colang-gen`, and
`/aikido-triage`. See the upstream README for the full table.

## Against the workshop labs

Warm-up (DVWA or Juice Shop) — see [`../../LABS.md`](../../LABS.md):

```text
/pentester http://localhost/
# or, Juice Shop:
/pentester http://localhost:3000/
```

HTB:

```text
/pentester <box-ip>
```

Watch the run. If a sub-skill stalls or starts looping, interrupt the
agent — without a human circuit breaker, an Agent Smith run can burn
through a workshop budget on a stuck sub-agent.

## When it gets stuck

Same five steering moves as the [Raw agent](../raw-agent/README.md)
path — state summary, focus narrowing, evidence forcing, hypothesis
listing, reset to checkpoint. Framework or not, the model is still the
model.
