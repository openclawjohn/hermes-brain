# Hermes Clone — Isolated Agent on media-factory Server

**Date:** 2026-08-25
**Task:** Package an isolated clone of the Hermes agent to run on the media-factory server, using local AI, confined to a specific project folder.
**Status:** Bundle built and packaged; not yet deployed to server.

## Goal
Clone Hermes to the media-factory server with three properties:
1. Runs on **local AI** (no cloud API, nothing leaves the machine).
2. **Isolated** — can only reach the project folder, cannot touch the rest of the server.
3. Portable — everything needed travels as one file.

## Target server specs (detected)
- CPU: Intel Xeon W-2102, 4 cores / 4 threads @ 2.9 GHz (no hyperthreading)
- RAM: 62 GB (56 GB free)
- GPU: NVIDIA Quadro P4000, 8 GB VRAM (driver 555.42.06, CUDA 12.5)
- Disk: 317 GB free
- Ollama: installed **natively on host** (not in a container), port 11434

## Models installed on the server (native Ollama)
| Model | Size | Notes |
|-------|------|-------|
| gemma4:12b | 7.6 GB | Fits 8 GB VRAM (fully GPU) |
| gemma4:26b | 17 GB | Exceeds VRAM, would offload to CPU |
| qwen3:30b-instruct | 18 GB | Exceeds VRAM, offloads to CPU |
| qwen2.5vl:3b | 3.2 GB | Vision model |

## Architecture decision
- **Ollama runs natively on the host** (not in a container). The Hermes clone container
  reaches it via `host.docker.internal:11434` using `extra_hosts` / `host-gateway`.
- **No separate ollama service** in docker-compose.
- The clone is a single container with **only `./project` mounted in** — that is the
  isolation boundary. Everything else on the server is physically unreachable.

## What was carried over from the source machine (`hermes_home/`)
- 76 skills (WordPress ops, git policy, portfolio audit, weekly posts, all site workflows)
- SOUL.md + memory (MEMORY.md, USER.md)
- config.yaml (settings only)

**Deliberately NOT carried:**
- **All credentials** (credentials.json, SITE_CREDENTIALS.md) — clone starts clean;
  creds added on server only if a specific project needs them.
- `~/.hermes/.env` — contains `SUDO_PASSWORD` + cloud API keys (DeepSeek, FAL, AgentMail).
  The local-LLM clone doesn't need them.
- `state.db` (8 GB session history) — not needed.

## Bundle location
- Source: `/home/m/hermes-clone/` (working tree)
- Package: `/home/m/hermes-clone.tar.gz` (7.2 MB, single transferable file)

## Deployment steps (on the server)
```bash
cd /srv/apps && tar -xzf hermes-clone.tar.gz && cd hermes-clone
# put media-factory project files in ./project/
docker compose up -d --build
docker exec -it hermes-clone hermes
```

## Media-factory project
- No media-factory skill or project doc exists yet on the source machine.
- The project's rules (AGENTS.md / PROJECT_STATE.md / DESIGN_SYSTEM.md / RULES.md)
  should live inside `./project/` on the server; the clone picks them up automatically.
- First task on the server: document what media-factory is and save a project skill.

## Open decisions
- [x] Model: **`qwen3:30b-instruct`** chosen as parent/orchestrator (highest agent quality;
      slow on 4-core CPU is acceptable — user confirmed speed not a concern).
- [x] Two-tier strategy: parent = 30b (thinking/monitoring), mechanical subagents pinned
      to fast `gemma4:12b` via per-task `delegate_task` model override (GPU-fast).
- [x] Deploy the bundle to the server — pending.
- [ ] Create media-factory project doc/skill.
- [ ] Add Obsidian + git discipline (git-usage-policy skill) inside the clone.
- [ ] On server: confirm `qwen3:30b-instruct` slow-but-usable in practice; fall back to
      `gemma4:12b` if unusable. Raise `delegation.max_concurrent_children` if more parallelism wanted.

## Follow-ups for next session
- Confirm handover model choice with user before deploy (30b parent + 12b workers).
- Ensure fresh `.env` on server if the clone needs any cloud key (it shouldn't for local AI).
- Two-tier subagent pattern documented in README — mechanical work delegated to fast 12b workers while parent monitors on 30b.
