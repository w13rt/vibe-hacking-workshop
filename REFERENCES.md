# References

External pointers for going further after the workshop. One flat list,
one line of annotation per entry.

## Agentic CLIs

- [Claude Code](https://docs.anthropic.com/claude/docs/claude-code) — default cloud agent in this workshop; native skill + MCP support.
- [Codex CLI](https://github.com/openai/codex) — OpenAI's agentic CLI; ChatGPT Plus/Pro path.
- [Aider](https://aider.chat/) — model-agnostic; the only listed CLI that drives local Ollama smoothly on tier-1 hardware.
- [Cline (VS Code extension)](https://github.com/cline/cline) — GUI alternative; OpenAI-compatible endpoint config makes Ollama wiring easy.
- [OpenCode](https://github.com/sst/opencode) — MCP-capable client for Agent Smith on bring-your-own LLM.
- [Cursor](https://cursor.com/) — IDE-based alternative listed in `SETUP.md`.

## Local model stack

- [Ollama](https://ollama.com/download) — the local-path runtime; install once, pull a model from §3 of `SETUP.md`.

## Pentest frameworks / skill libraries

- [Agent Smith](https://github.com/0x0pointer/agent-smith) — MCP-backed framework behind `paths/agent-smith/`.
- [Skill fork](https://github.com/w13rt/skills) — skill-only fork; upstream for `paths/skill-fork/`.

## Vulnerable targets

- [DVWA](https://github.com/digininja/DVWA) — Phase-1 warm-up option A in `LABS.md`.
- [OWASP Juice Shop](https://owasp.org/www-project-juice-shop/) — Phase-1 warm-up option B in `LABS.md`.
- [Hack The Box — retired free-tier machines](https://app.hackthebox.com/machines/list/retired) — Phase-2 lab; filter Free + Easy on the day.

## Further reading

- [Critical Thinking — Bug Bounty Podcast](https://www.criticalthinkingpodcast.io/) — Justin Gardner & Joel Margolis. Weekly bug-bounty methodology in spoken form; pattern source for several skill-fork skills.
- [METR](https://metr.org/) — Model Evaluation & Threat Research. Their work on how long a task AI agents can complete autonomously is the empirical backdrop for "can this agent actually finish an HTB box on its own?".
- [Black-hat LLMs — Nicholas Carlini, [un]prompted 2026](https://youtu.be/1sd26pWhfmg?si=UhmZnWavh60i2J5z) — talk on adversarial techniques and security implications of LLMs; useful framing for the offensive side of the workshop.
