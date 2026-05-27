# Bootstrap prompts for your `CLAUDE.md`

Paste these into your agent, in order, in the working directory where you
want `CLAUDE.md` to live. The point is not to copy a template — it is to
have the agent interview you and write down what you actually do.

Treat the output as a draft. Read it. Cut what does not match you. The
file is yours once it survives one engagement unchanged.

## 1. Interview-me draft

Use this first. The agent asks you 5–10 questions, then writes a first
`CLAUDE.md` to disk.

```text
Interview me to draft a CLAUDE.md for this engagement directory.

Ask me one question at a time, max 10 questions total. Cover:
- identity and tone you should use
- tools and environment I work in (OS, VM, network constraints)
- working files I want you to maintain (notes, findings, PoCs)
- how I want you to behave when stuck
- scope and safety rules

When you have enough, write CLAUDE.md in the current directory and
stop. Do not pad with generic advice. If I gave you nothing on a
section, leave it out — not a TODO.
```

## 2. Convert a workflow

Use this when you have a specific habit you want encoded — a recon flow,
a triage routine, a reporting format.

```text
Here is how I work:

<paste a paragraph or bullets describing the workflow>

Convert this into rules I can add to CLAUDE.md. Output only the new
section, ready to append. No preamble. If something I wrote is vague
("be thorough", "check everything"), flag it back to me as a question
instead of inventing a rule.
```

## 3. Critique the draft

Run this after the interview prompt produces a `CLAUDE.md`. The agent
reads its own output with fresh eyes.

```text
Read CLAUDE.md in the current directory. For each rule, tell me:
- is it specific enough to act on, or is it a vibe?
- does it contradict another rule?
- does it claim an environment fact (tool installed, path exists)
  that you cannot verify from this directory?

Output a list. Do not rewrite the file yet.
```

## 4. Minimal-shape check

Use this if the draft is bloating. Strips back to the smallest
`CLAUDE.md` that still does its job.

```text
Rewrite CLAUDE.md to the minimum that still covers: identity,
environment, working files, work style, scope. Drop anything that
is generic advice you would give any agent. Aim for under 40 lines.
```

## 5. Skill bootstrap (optional)

Skip unless you have a recurring task that does not belong in
`CLAUDE.md` — something you only want loaded when relevant (e.g. a
recon flow, a specific exploit class).

```text
Interview me about one recurring task I want to encode as a skill.
Ask about: when the skill should trigger (the "use when" phrase),
the exact commands or steps, flags I prefer, known gotchas.

Then draft a skill file at skills/<name>.md with YAML frontmatter
(name, description) and the methodology as the body. The description
must start with "Use when..." — that is how the agent decides to
load it.
```
