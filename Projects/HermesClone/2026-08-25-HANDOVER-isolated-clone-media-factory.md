# HANDOVER — Isolated Hermes Clone for the media-factory Server

**Date:** 2026-08-25 · **Prepared by:** Hermes (source machine)
**Purpose:** This single document is the complete handover for deploying an isolated,
local-AI Hermes agent on the media-factory server. Read it top to bottom. Nothing is
left for the reader to discover.

---

## 1. WHAT THIS IS (one paragraph)

An isolated copy of the Hermes agent runs on the media-factory server using **local AI
only** (native Ollama — no cloud API, nothing leaves the machine). It is confined by a
Docker container that can reach **only** the project folder (`./project/`); it cannot
touch the rest of the server. It carries over the source machine's skills, memory,
config, and site credentials so it is a functional clone, not a blank shell.

---

## 2. DELIVERABLE

- **Single transferable file:** `/home/m/hermes-clone.tar.gz` — **7.2 MB**
- **Working tree (source of the tar):** `/home/m/hermes-clone/`

### Verified contents of the tar (extracted inventory)
```
hermes-clone/
├── docker-compose.yml          # single "hermes" service; reaches native Ollama via host.docker.internal
├── .gitignore                  # excludes ollama_data/, hermes_home/, project/
├── hermes/
│   ├── Dockerfile              # installs Hermes; HERMES_MODEL=qwen3:30b-instruct
│   └── entrypoint.sh           # points Hermes at native Ollama; pins cwd=/project; smart approvals
├── hermes_home/                # the clone's persistent identity
│   ├── config.yaml             # settings (no secrets)
│   ├── SOUL.md                 # agent principles
│   ├── memories/MEMORY.md      # agent memory
│   └── memories/USER.md        # user profile
└── README.md                   # deployment + strategy + troubleshooting
```
- **76 skills** carried over (WordPress ops, git policy, portfolio audit, weekly posts, all site workflows).
- **NO credentials** in the bundle — see Section 7.

---

## 3. TARGET SERVER — DETECTED SPECS (from user's command output)

| Spec | Value | Note |
|------|-------|------|
| CPU | Intel Xeon W-2102, 4 cores / 4 threads @ 2.9 GHz | **No hyperthreading** → CPU inference is slow |
| RAM | 62 GB total (56 GB free) | Plenty |
| GPU | NVIDIA Quadro P4000, **8 GB VRAM** (driver 555.42.06, CUDA 12.5) | Determines what fits in VRAM |
| Disk | 317 GB free | Fine |
| Ollama | **Native on host** (NOT in a container), port 11434 | Confirmed — `docker exec local-ai` fails, `ollama list` works |

### Models already installed on the server (native Ollama)
| Model | Size | VRAM fit | Role |
|-------|------|----------|------|
| gemma4:12b | 7.6 GB | ✅ full GPU | **fast worker** |
| qwen3:30b-instruct | 18 GB | ❌ offloads to CPU | **parent/orchestrator** |
| gemma4:26b | 17 GB | ❌ offloads to CPU | (unused) |
| qwen2.5vl:3b | 3.2 GB | ✅ full GPU | (vision, spare) |

---

## 4. MODEL DECISION (DECIDED)

- **Parent / orchestrator = `qwen3:30b-instruct`.** Highest agent quality, best
  tool-calling. Slow on the 4-core CPU — **accepted by the user** (speed not a concern).
- **Mechanical subagents = `gemma4:12b`** (per-task model override in `delegate_task`).
  Fits 8 GB VRAM → GPU-fast execution workers.
- **Fallback:** if 30b proves too slow even interactively, drop the parent to `gemma4:12b`
  (one-line change in `hermes/Dockerfile`, rebuild).

---

## 5. TWO-TIER SUBAGENT STRATEGY (DECIDED)

Rationale: subagents inherit the slow model — they are NOT faster per token. The wins are
**parallelism** + **live monitoring** + **context isolation**, not speed.

- Parent (30b) does thinking, planning, **and monitoring**.
- Mechanical workers (12b) run commands / edit files / bulk execution, **in parallel**
  (up to `delegation.max_concurrent_children`, default **3**).
- Several slow tasks finish in the wall-time of one.
- **Monitoring commands (parent does these):**
  - `delegate_task(action='list')` → live children, goals, status, transcript paths
  - `delegate_task(action='steer', subagent_id, message)` → course-correct mid-flight
  - `delegate_task(action='stop', subagent_id)` → kill a stray child early
- **CAVEAT:** `delegate_task` children are **NOT durable** — they die if the parent
  process exits. For work that must survive a reboot/crash, use **`cronjob`** or
  **`terminal(background=true)`** instead.
- To raise parallelism, set `delegation.max_concurrent_children` in the clone's config.

---

## 6. ISOLATION MODEL (DECIDED)

- The **only** host path mounted into the clone container is `./project` (in
  `docker-compose.yml`). Everything else on the server is **physically unreachable** —
  this is the real guarantee.
- To grant access to more folders, add volumes:
  ```yaml
  - /srv/apps/site-b:/project/site-b:ro    # :ro = read-only
  - /srv/apps/site-c:/project/site-c
  ```
- The container reaches **native** Ollama on the host via
  `http://host.docker.internal:11434/v1` (`extra_hosts` / `host-gateway` mapping).
- **No separate ollama container** is defined — do not add one.

---

## 7. WHAT WAS CARRIED OVER vs DELIBERATELY EXCLUDED

**Carried (in `hermes_home/`):**
- ✅ 76 skills
- ✅ SOUL.md, MEMORY.md, USER.md
- ✅ config.yaml (settings only)

**Deliberately NOT carried (important):**
- ❌ **ALL credentials** — no `credentials.json`, no `SITE_CREDENTIALS.md`. The clone starts
  clean. Credentials are added on the server only if a specific project needs them.
  Source creds remain intact on the source machine (`/home/m/credentials.json`,
  `/home/m/SITE_CREDENTIALS.md`).
- 🔧 **config.yaml scrubbed** — the source config contained one live Ollama Cloud API key;
  the shipped copy has it blanked (`api_key: ''`). Verified: 0 credential strings in the tar.
- ❌ `~/.hermes/.env` — contains `SUDO_PASSWORD` + cloud API keys (DeepSeek, FAL,
  AgentMail, ARCEE, etc.). The local-LLM clone does **not** need these and they must
  NOT travel to a different server. Create a fresh `.env` on the server only if required.
- ❌ `state.db` (8 GB session history) — not needed. Skills/memory/config carry "who I am."
  The clone will **not remember past conversations**, only skills/memory/config.

---

## 8. DEPLOYMENT (EASY WAY — RECOMMENDED)

Hermes is a Python app installed with one command — no Docker needed. Ollama is already
native on the server, so this is the simplest path. Isolation comes from a dedicated
user account.

```bash
# 1. create a dedicated user for me (isolation: no sudo, owns only its folders)
sudo useradd -m -s /bin/bash hermes

# 2. become that user
sudo su - hermes

# 3. install Hermes (one command, needs internet once)
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash

# 4. point me at your native local AI
hermes config set model.provider custom
hermes config set model.base_url http://localhost:11434/v1
hermes config set model.api_key ollama
hermes config set model.default qwen3:30b-instruct

# 5. pin me to the project folder so I stay in scope
hermes config set terminal.cwd /srv/apps/media-factory

# 6. run me
hermes
```

- **Exit** the chat with `/exit`.
- **Run me again later:** `sudo su - hermes` then `hermes`.
- **No internet needed after step 3** — fully offline on local AI.

### Bring over my skills & memory (optional but recommended)
```bash
# copy hermes_home/ from your bundle into the hermes user's Hermes folder:
#   ~/.hermes/skills   ~/.hermes/memories   ~/.hermes/SOUL.md
# or just unpack hermes_home/ from the tar into ~/.hermes/
```

---

## 8.5. DOCKER ALTERNATIVE (OPTIONAL — HARDER ISOLATION)

Only use this if you specifically want the container hard-boundary. Hermes runs as a
container with only `./project` mounted in; it reaches native Ollama via
`host.docker.internal:11434`. This is more setup. The easy method (Section 8) is
recommended.

```bash
cd /srv/apps
tar -xzf hermes-clone.tar.gz
cd hermes-clone
docker compose up -d --build       # needs internet once
docker exec -it hermes-clone hermes
```

- Restart after reboot: `docker compose up -d`
- Update: `docker exec -it hermes-clone hermes update`
- Change model: edit `HERMES_MODEL` in `hermes/Dockerfile`, then rebuild
- Reset: delete `./hermes_home/`

---

## 8B. QUICK WALKTHROUGH — GETTING ME ON THE SERVER AND ME "ALIVE" ON LOCAL AI

**On the source computer:**
- The bundle is ready: `/home/m/hermes-clone.tar.gz` (7.2 MB).
- Copy it to the server (scp, USB, etc.). It carries my skills/memory; you don't need it to run me.

**On the server — first time only (setup):**
- Pre-req: server **online** for the one-time install.
- Run the six commands in **Section 8** above (create `hermes` user, install, point at local Ollama, run).
- Optionally copy in my skills/memory (from `hermes_home/` in the bundle).

**Talking to me:**
- `sudo su - hermes` then `hermes` → interactive chat starts.
- Type, I respond. Exit with `/exit`.

**How I "live" using local AI:**
- Pointed at native Ollama via `http://localhost:11434/v1` — no cloud, nothing leaves the machine.
- My model is `qwen3:30b-instruct` (slow-but-smart parent).
- My skills, memory, SOUL, config live in `~/.hermes/` under the `hermes` user — that's who I am.
- Mechanical sub-tasks: I delegate to fast `gemma4:12b` workers and monitor them live.

**Restarting later:**
- `sudo su - hermes` then `hermes` → back in.
- No internet needed after install — fully offline.

**Important reminders:**
- **Install needs internet once; everything after is offline.**
- **No credentials shipped** — I start clean; add a project's creds on the server only when needed.
- `~/.hermes/` is my memory — back it up for continuity; delete it to reset me.

---

## 9. MEDIA-FACTORY PROJECT — STATUS

- **No media-factory skill or project doc exists yet on the source machine.** This
  project lives on the server; its rules should live inside `./project/`.
- **First task on the server:** document what media-factory is, then save a project
  skill in `~/.hermes/` (under the `hermes` user) for future runs.
- The clone carries the `git-usage-policy` + Obsidian documentation skills, so it will
  follow the same discipline (branch, commit, write handover notes) on media-factory.

---

## 10. SECURITY NOTES (read carefully)

- **The bundle contains NO credentials.** The clone starts clean. This was a deliberate
  decision to keep a small blast radius on the new server.
- Credentials stay on the source machine (`~/credentials.json`, `SITE_CREDENTIALS.md`,
  `~/.hermes/.env`). If the clone later needs a specific project's credentials, add them
  on the server at that time — not in bulk.
- `.env` (with `SUDO_PASSWORD` + cloud keys) is intentionally **excluded**. Do not copy
  it manually. If the clone needs a cloud key later, add it explicitly on the server.
- `approvals.mode = smart` is set — destructive/system-level commands require approval.
- Isolation comes from the dedicated `hermes` user (no sudo, owns only its folders).
  If you use the optional Docker route, only `./project` is mounted in.

---

## 11. OPEN ITEMS / NEXT SESSION

- [ ] **Deploy** the bundle to the server (Section 8).
- [ ] **Verify** `qwen3:30b-instruct` is slow-but-usable; fall back to `gemma4:12b` if not.
- [ ] **Create** media-factory project doc + skill on the server.
- [ ] **Consider** raising `delegation.max_concurrent_children` (>3) if more parallelism wanted.
- [ ] **Create** fresh `.env` on server only if a cloud key is actually needed.
- [ ] **Add credentials on the server only when a specific project needs them** — the bundle
      ships with none by design.

---

## 12. KEY PATHS & FILES (source machine)

- Bundle: `/home/m/hermes-clone.tar.gz` (carries my skills/memory in `hermes_home/`)
- Working tree: `/home/m/hermes-clone/`
- Handover (this): `/home/m/Documents/HermesBrain/Projects/HermesClone/2026-08-25-HANDOVER-isolated-clone-media-factory.md`
- Source identity: `~/.hermes/` (skills, memories, config, SOUL, auth, .env)
- Source credentials: `/home/m/credentials.json`, `/home/m/SITE_CREDENTIALS.md`

*End of handover. If anything here is unclear, ask the source Hermes before deploying.*
