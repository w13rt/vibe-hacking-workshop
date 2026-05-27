# Path: Raw agent

A standard agentic CLI with shell access. No framework, no skills,
no MCP. You write the prompt, the agent runs the loop.

**Why pick this:** lowest setup cost, easiest to debug (one chat, one
terminal, one set of notes), and the best path for learning the
operator loop. This is the default unless you have a specific reason
to pick another path.

**Weakness:** no reusable methodology. Every engagement is a new prompt.

## What you need

- Any one of: Claude Code (cloud), Codex CLI (cloud), Aider + Ollama
  (cloud or local), Cline in VS Code (cloud or local)
- A Kali VM (or equivalent) where the agent can run shell commands
- Setup from [`../../SETUP.md`](../../SETUP.md) complete

## Cost expectation

- Claude Pro ($20/mo) is enough for the warm-up and probably one HTB
  box. Claude Max 5× ($100) is safer if you want headroom.
- ChatGPT Plus ($20/mo) covers the warm-up; expect to hit the weekly
  cap if you push.
- Aider + Gemini on Google AI Pro ($20/mo) is the cheapest cloud path.
- Local: free, slower, and demo-grade on Tier 1 hardware.

## Setup

You already did the install in [`../../SETUP.md`](../../SETUP.md). The
only thing left is to pick a working directory:

```bash
mkdir -p ~/engagements/workshop
cd ~/engagements/workshop
```

Start the agent from this directory so your `notes.md`, `findings.md`,
and `pocs/` land here.

## Starter prompt

Paste this into the agent on first message. Replace `<target-url-or-ip>`
with the warm-up target (e.g. `http://localhost/` for DVWA on the host,
`http://localhost:3000/` for Juice Shop).

```text
You are hunting <target-url-or-ip>. Deliberately vulnerable target.
You have shell access on a Kali VM with the standard pentest toolkit
(plus an active HTB session if this is an HTB box).

Goal: get a shell on the underlying host (or solve a challenge
end-to-end) and capture proof of exploitation.

Working files (create them under the current directory if they don't
exist):
- notes.md       — working memory. Hypotheses, what you tried, dead
                   ends worth remembering. Reverse-chronological.
                   Update when something is worth remembering, not
                   before every command.
- findings.md    — paste-ready confirmed issues. One section per
                   finding. Include impact, repro, remediation hint.
- pocs/<bug>.http — paste-ready proof artefact per finding.

Ask before destructive actions.
```

If you find yourself wanting to write a `CLAUDE.md` to make this
reusable across runs, you have slid into **Build your own**. See
[`../build-your-own/README.md`](../build-your-own/README.md).

## Invocation

### Against the warm-up (DVWA or Juice Shop)

```bash
cd ~/engagements/workshop
claude            # or: codex, or: aider --no-auto-commits
```

Paste the starter prompt above, with `<target>` set to the warm-up URL.
Lab targets and reachability checks: [`../../LABS.md`](../../LABS.md).

### Against HTB

Same flow. Make sure your HTB session is active first, then point the
prompt at the box IP from the HTB UI.

```bash
ping -c 1 <box-ip>     # sanity check VPN routing
cd ~/engagements/workshop
claude
```

## Steering moves when the agent stalls

In rough order to reach for them:

1. **State summary** — "list what you tried, what worked, what is
   still untested"
2. **Focus narrowing** — "stop SQLi; spend the next 20 minutes on
   authz and file upload"
3. **Evidence forcing** — "before continuing, write the exact request
   that proves this"
4. **Hypothesis listing** — "give me five hypotheses for why this
   returns 403, ranked by likelihood"
5. **Reset to checkpoint** — "we are off track. Go back to the nmap
   output and pick a different branch"

Anti-pattern, do not say: "try harder", "be more creative", "you can
do this".
