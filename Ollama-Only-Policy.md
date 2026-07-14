---
created: 2026-06-09
tags: [hermes, policy, providers, ollama]
---

# Ollama-Only Policy

**Effective:** Repeatedly confirmed by user

## Provider Rules

1. **Use ONLY Ollama** — `ollama_kimi` and `ollama_qwen` accounts
2. **NO OpenRouter** — Unless user explicitly enables it
3. **No fallback** — If both Ollama accounts fail, report failure (do NOT fall back to OpenRouter)

## Configuration

Check `~/.hermes/config.yaml` for provider settings.

## Related
- [[Hermes-Config]]
