# Labs

Two phases. The warm-up is **load-bearing** — everyone runs the same
local Docker target so we know the agent talks to a real service. The
HTB phase is where you see what your agent does against something more
realistic.

If the agent setup falls over for you, **the warm-up still delivers
value**. Don't skip it to chase HTB.

## Phase 1 — Warm-up (15 min, everyone)

Pick **one** of two targets. Both are local Docker containers — no
account, no VPN, deterministic, well-known vulns. Use it to verify your
agent can fetch the target and reason about it.

### Option A — DVWA

```bash
docker run -d --rm -p 80:80 --name dvwa vulnerables/web-dvwa
```

Default credentials (once you click through to the login):
`admin / password`.

**Reachability check from Kali:**

Pattern A — Docker on host, Kali in a NAT'd VM:

```bash
ip route                              # default gateway = your host IP
curl -s http://<host-ip>/ | grep -i dvwa
```

Pattern B — Docker inside the Kali VM:

```bash
curl -s http://localhost/ | grep -i dvwa
```

**Goal:** get a shell on the underlying container, or demonstrate
exploitation of one vulnerability class (SQLi, command injection, file
upload, etc.) end-to-end.

**Facilitator nudges, in order, if the agent gets stuck:**

1. "Have you tried the default credentials?"
2. "Look at the page list at `/vulnerabilities/`."
3. "Try the SQL Injection page first — set security level to low."
4. "What does the security cookie look like?"

### Option B — OWASP Juice Shop

```bash
docker run -d --rm -p 3000:3000 --name juice bkimminich/juice-shop
```

Reachability check:

```bash
curl -s http://localhost:3000/ | grep -i juice
```

**Goal:** solve at least one challenge end-to-end. Easy starters: the
score board (find the hidden link), the admin section (broken access
control), or the first SQLi on the login page.

**Facilitator nudges, in order:**

1. "There is a score board page — can the agent find it from the source?"
2. "Try the login form with classic SQLi payloads."
3. "What does the JWT look like? Decode it."
4. "Browse the API at `/rest/` and `/api/`."

### Why pick one over the other

- **DVWA** is older and simpler; classic web vulns with explicit
  difficulty levels. Best if you want clean, predictable failure modes.
- **Juice Shop** is modern (single-page JS app, JWT, REST API). Best if
  you want to see how the agent handles SPA-ish targets.

## Phase 2 — Hack The Box (45 min, optional)

Pre-requisite: you already have an active HTB session (account + VPN
running on your machine). This workshop does not cover HTB setup.

Pick **one** retired free-tier easy box and try to solve it with your
agent. Box availability on the free pool changes — check
`https://app.hackthebox.com/machines/list/retired` filtered to
**Free + Easy** on the morning of the workshop and pick from what is
live.

**Candidate boxes (verify they are on the free pool before you start):**

- **Lame** — FreeBSD, Samba `usermap_script` RCE. Linear, fully
  one-shot. Smallest possible win.
- **Legacy** — Windows XP, MS08-067. Classic, but agents need to
  pick the right Metasploit module.
- **Devel** — Windows, anonymous FTP write + ASPX webshell. Two
  stages, no pivot.
- **Bashed** — Linux, exposed `phpbash`, cron-based privesc.

If your chosen box has reference Claude / Agent Smith runs that the
facilitator has access to, they can demo it side-by-side if your local
agent stalls.

**Goal:** capture both `user.txt` and `root.txt`.

## Invocation per path

How you actually kick the agent off depends on your path:

- [Raw agent → `paths/raw-agent/README.md`](paths/raw-agent/README.md)
- [Build your own → `paths/build-your-own/README.md`](paths/build-your-own/README.md)
- [Agent Smith → `paths/agent-smith/README.md`](paths/agent-smith/README.md)
- [Skill fork → `paths/skill-fork/README.md`](paths/skill-fork/README.md)
