---
created: 2026-06-09
tags: [hermes, workflow, obsidian, knowledge-base]
---

# Obsidian Integration Workflow

## MANDATORY RULE
**Every project and website task MUST produce an Obsidian note.** This is as non-negotiable as git commits. See AGENTS.md principle #9.

## Vault Location
`/home/m/Documents/HermesBrain/`

## When to Use Obsidian vs Memory

| Use **Obsidian** | Use **Memory Tool** |
|-----------------|---------------------|
| Rich documentation | Single declarative facts |
| Multi-page topics | User preferences |
| Code snippets + context | Environment quirks |
| Project reference | Stable conventions |
| Linked knowledge | Things that prevent corrections |

## Workflow Patterns

### 1. Discover New Information
```
1. Learn something durable (API quirk, workflow, config)
2. Ask: "Will this matter in 2 weeks?"
3. If yes → Create/update Obsidian note
4. If it's a simple preference → memory tool
5. If it's a procedure → save as skill
```

### 2. Before Starting Work
```
1. session_search for past context
2. Check Obsidian for project docs
3. Load relevant skills
4. Review memory for preferences
```

### 3. After Completing Work
```
1. Did we discover a new workflow? → skill_manage
2. Did we learn a durable fact? → Obsidian or memory
3. Was it task-specific? → session_search (not memory)
```

## File Structure

```
HermesBrain/
├── 00-Index.md
├── 0-Inbox/          # Quick captures
├── 1-Projects/       # Active projects
├── 2-Areas/          # Ongoing responsibilities
├── 3-Resources/      # Reference material
├── 4-Archives/       # Completed work
└── Templates/        # Note templates
```

## Commands

```bash
# Open vault in Obsidian
obsidian://open?vault=HermesBrain

# Quick note from terminal
echo -e "---\ncreated: $(date +%Y-%m-%d)\n---\n\n# $(date +%H:%M)" >> ~/Documents/HermesBrain/0-Inbox/quick-$(date +%s).md
```

## Related
- [[Hermes-Memory-Rules]]
- [[00-Index]]
