# Migrate All DeepSeek v4-flash Processes to v4.1-flash

**Date:** 2026-09-14
**Scope:** Global agent default + all pinned cron jobs + failover watchdog scripts

## Why
User requested all processes using DeepSeek v4 flash move to **DeepSeek v4.1 flash**.
`deepseek-v4.1-flash` confirmed live on ollama-cloud endpoint (`GET /v1/models`). Note:
bare `deepseek-v4-flash` (untagged) is no longer listed on the endpoint — only
`deepseek-v4-flash:0731` remains. Cutover was therefore well-timed / necessary.

## What Changed

### 1. config.yaml (agent-protected → via `hermes config set`)
- `model.default`: `deepseek-v4-flash` → `deepseek-v4.1-flash`
- `providers.ollama-cloud.default_model`: `deepseek-v4-flash` → `deepseek-v4.1-flash`

### 2. Cron jobs (all pinned, via `hermes cron edit --provider ollama-cloud --model deepseek-v4.1-flash`)
| Job | ID | Old → New |
|-----|----|-----------|
| Weekly Blog Posts | b054e0d5026d | v4-flash → v4.1-flash |
| weekly-git-backup | e953e977e4dc | v4-flash → v4.1-flash |
| website-guardian | 2df4e7130a25 | v4-flash → v4.1-flash |
| cp47-server-monitor | 6a62ffefd737 | v4-flash → v4.1-flash |
| CEO of Domains | eb66b3bea877 | v4-flash → v4.1-flash |
| IES-finish-backgrounds | e1dd28e26c39 | v4-flash → v4.1-flash |
| IES-watchdog | 70214450d1b4 | snapshot v4-flash → v4.1-flash (was unpinned w/ stale snapshot) |

Jobs with `model: null` (Daily Self-Update, GitHub Backup, 2 ZAFacebook, failover-watchdog)
follow the NEW global default automatically. Failover-watchdog is `no_agent` (script-only).

### 3. Scripts
- `/home/m/.hermes/scripts/ollama-failover-watchdog.py` — `ACCOUNT_FALLBACK_MODEL["ollama-cloud"]`,
  `ACCOUNT_FALLBACK_MODEL.get(..., default)`, and usage-comment → `deepseek-v4.1-flash`
- `/home/m/.hermes/scripts/failover.py` — same 3 replacements
- Both `python3 -m py_compile` clean.

## Verification
- `GET /v1/models` returns `deepseek-v4.1-flash` (confirmed live)
- `jobs.json`: all 7 job model/snapshot fields = `deepseek-v4.1-flash`
- `config.yaml` grep: default + default_model = `deepseek-v4.1-flash`
- Scripts compile clean
- `hermes-backup` script re-run → pushed to github.com/openclawjohn/hermes-backup
  (config/ + cron/jobs.json now carry v4.1 values)

## Notes
- Backups taken: `config.yaml.bak.deepseek-v4.1`, `cron/jobs.json.bak.deepseek-v4.1`
- Skill docs under `hermes-provider-config` / `hermes-agent` reference only
  `deepseek-chat-v3.1` (fallback doc) and the generic `deepseek` provider — no active default
  to change, left as-is.
- `.hermes` is NOT a git repo (backup repo is hermes-backup/). No local commit needed.
