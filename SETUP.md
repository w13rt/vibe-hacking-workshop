# Setup

Arrive with a working AI agent that you can point at a target. Local
models can take a long time to download — do this before the day, not on
the morning of.

## 1. What you need

**Hardware:**

- Personal laptop with admin / sudo rights
- For local AI: 16 GB RAM minimum, 32 GB strongly recommended

**Software:**

- Docker
- A pentesting environment — Kali VM strongly recommended
- An agentic CLI — Claude Code, Codex CLI, Aider, or Cline (VS Code)
- For local AI: Ollama + a tools-capable model

**Optional:**

- Hack The Box account (free tier) if you want to do the main lab phase.
  This workshop does not walk you through HTB account or VPN setup —
  bring an already-active session if you want to play.

## 2. Cloud path (recommended)

Pick one. Any of these works for at least the warm-up.

| Provider | Subscription | $/mo | Tool |
|---|---|---|---|
| Anthropic | Claude Pro | $20 | Claude Code (one-box runs) |
| Anthropic | Claude Max 5× | $100 | Claude Code (comfortable for the lab phase) |
| OpenAI | ChatGPT Plus | $20 | Codex CLI (~2h coding per 5h window) |
| OpenAI | ChatGPT Pro 5× | $100 | Codex CLI (heavier runs) |
| Google | Google AI Pro | $19.99 | Aider + Gemini |
| Cursor | Pro | $20 | Cursor IDE |

Install your CLI of choice and sign in. Verify with a one-line prompt
("say hello and exit") before the workshop.

### Claude Code

```bash
# Install (see https://docs.anthropic.com/claude/docs/claude-code for the latest)
npm install -g @anthropic-ai/claude-code
claude --version
claude    # follow the auth prompt
```

### Codex CLI

```bash
npm install -g @openai/codex
codex --version
```

### Aider

```bash
pip install aider-chat
aider --version
```

## 3. Local path (no subscription)

Hard floor: 16 GB unified RAM or 8 GB VRAM. Below that, use the cloud
path — the workshop is too short to fight a swapping model.

### Install Ollama

```bash
# https://ollama.com/download
ollama --version
ollama serve   # leave running in another terminal
```

### Pull a model (pick your tier)

| Tier | Hardware | Model | Size |
|---|---|---|---|
| Low | 16 GB unified / 8 GB VRAM | `qwen3:8b` (default) or `gemma4:e4b` | ~5–10 GB |
| Mid | 32 GB unified / 24 GB VRAM | `qwen3-coder:30b` (default) or `gemma4:26b` | ~18 GB |
| High | 64 GB unified / 48 GB+ VRAM | `llama3.3:70b` or `qwen3:72b` | ~40 GB |

```bash
# example — Tier 1
ollama pull qwen3:8b
```

### Verify the agent can talk to Ollama

```bash
aider \
  --model ollama_chat/qwen3:8b \
  --no-auto-commits \
  --message "say 'hello workshop' and exit"
```

If Aider cannot see Ollama:

```bash
export OLLAMA_API_BASE=http://127.0.0.1:11434
```

**Want a GUI instead?** Install Cline in VS Code (search "Cline" in the
extensions marketplace). Set API Provider to "OpenAI-compatible", Base
URL to `http://127.0.0.1:11434/v1`, Model to your Ollama model name.

### Realistic expectations per tier

- **Low**: the agent can issue a couple of `nmap` / `curl` commands and
  parse them. Will stall on multi-stage chains. Demo-grade.
- **Mid**: completes DVWA reliably. Makes real progress on an HTB easy
  box. This is the default tier the workshop assumes for the local path.
- **High**: comparable to a cheap cloud model on simple boxes. Honest
  delta vs cloud at this tier is tokens/sec, not capability.

## 4. End-to-end smoke test

```bash
# Spin up the DVWA target
docker run --rm -d -p 80:80 --name dvwa vulnerables/web-dvwa

# Verify it is reachable
curl -s http://localhost/ | grep -i dvwa

# Ask your agent to look at it
aider --message "use curl to fetch http://localhost/ and describe the page"
# or, for Claude Code:
# claude
# > use curl to fetch http://localhost/ and describe the page
```

If the agent fetches the page and describes "Damn Vulnerable Web
Application", **you are ready for the workshop**.

## 5. Pre-flight checklist

- [ ] Docker installed, `docker run hello-world` passes
- [ ] Kali VM up, updated, networked
- [ ] At least one cloud subscription **or** Ollama + a tools-capable
      model pulled
- [ ] Agentic CLI installed and authenticated (Claude Code / Codex /
      Aider / Cline)
- [ ] End-to-end smoke test from §4 passes
- [ ] (Optional) Active HTB session if you want to do the main lab phase

If every box is ticked, you are ready. See you on the day.
