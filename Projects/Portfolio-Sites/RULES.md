# RULES.md — Portfolio of 7 South African Domains

**Last Updated:** 2026-08-03
**Purpose:** Mandatory development rules, quality gates, and workflow constraints for all 7 portfolio sites.

---

## 🚨 CRITICAL RULES (Never Violate)

### 1. Fix Everything You Find
**The CEO does not "report issues." The CEO fixes them.** Every issue found must be resolved in the same run. The only exception is issues requiring the user's personal login (Google, GitHub OAuth) — and even then, document the exact 30-second steps needed.

### 2. Pivot, Don't Stop
If a promotion platform is blocked (CAPTCHA, OAuth, Cloudflare), immediately try the next alternative. Never save a draft and move on. Execute something publishable every day.

### 3. Finish in This Session
Never defer work to "next time" or "the CEO will handle it tomorrow." If something can be done now, do it now. Before reporting completion, list everything still incomplete and ask: "Am I deferring anything I could do right now?"

### 4. Visual Verification Required
**BEFORE** marking any task complete:
1. Navigate to the page as a logged-out user
2. Take a screenshot with `browser_vision()`
3. Actually examine the screenshot — does it show the thing working?
4. Fix autonomously if issues found
5. Re-verify until perfect

**NEVER** claim "fixed" without screenshot proof.

### 5. Articles = "Articles" Not "Blog"
On ALL websites, blog posts are ALWAYS called "Articles" — never "posts," "blog posts," or "content pieces." Navigation menus must say "Articles."

### 6. Every Article Must Have 2 Images
Non-negotiable. Every article gets exactly 2 real CC-licensed photos. Featured at top, in-content at ~55% through. Different subjects. Visually inspected for text/logos/watermarks before deployment.

### 7. Git + Obsidian + State Files (Mandatory)
Every task requires:
- Git commit with meaningful message
- Obsidian doc in `/home/m/Documents/HermesBrain/`
- PROJECT_STATE.md, DESIGN_SYSTEM.md, RULES.md updated

---

## 📋 Per-Domain Quality Gates

### whippetqr.com
- Articles must be tech/business focused
- Images: tech screenshots, devices, QR codes in use
- No thin content — every article 1,500+ words
- Categories: Business Tools, Marketing Tips, QR Code Guides

### howzitza.co.za
- Articles must be SA culture focused
- Images: SA people, places, food, music, culture
- Categories: South African Culture, Games & Quizzes, Personality Types

### sumza.co.za
- Articles must be finance/tools focused
- Images: professional, financial, calculator/document themes
- Categories: Personal Finance, Tax, Business, Property, Solar & Electricity, Vehicles, Student Advice

### zadocs.co.za
- Articles must be legal/document focused
- Images: legal/professional themes
- Categories: Business, Education, Employment Documents, Events, Personal, Property

### saymyname.co.za
- Articles must be name/culture focused
- Images: people, families, diversity, cultural
- Categories: Baby Names, Business Names, Naming Guides, Pet Names

### 5minutes.co.za
- Articles must be game/fun focused
- Images: playful, game-related, SA culture
- Categories: South African Games, Game Guides & Tips, SA Culture & Humour

### beanel.com
- Articles must be tech/privacy focused
- Images: tech, network, security, privacy themes
- Categories: IP & Networking, Internet Security, Online Privacy, VPN Reviews, Digital Tools

---

## 🛠️ CEO of Domains — Daily Workflow

### Phase 1 — Site Health (5 min)
Check all 7 sites are up. If any is down, diagnose and fix.

### Phase 2 — Daily Promotion (25 min)
Weekly rotation:
| Day | Strategy |
|-----|----------|
| Monday | Cross-Link Audit & Enhancement |
| Tuesday | Reddit Engagement |
| Wednesday | Quora Answers |
| Thursday | Pinterest Pins |
| Friday | LinkedIn Articles & Quora Blog |
| Saturday | SEO Tune-Up |
| Sunday | Research & Strategy |

**Pivot rule:** If the day's platform is blocked, try the next. Never stop without executing something.

### Phase 3 — Maintenance & Fixes (20 min)
Run ALL checks. Fix EVERY issue found in the same run:
1. **IndexNow key files** — if missing, create via FTP
2. **Broken slugs (-2/-3)** — delete duplicates, rename unique content. Do NOT dismiss as "false positives" without proper verification
3. **Sitemaps** — if lastmod > 7 days, force regeneration
4. **ads.txt** — if missing, create via FTP
5. **Image alt text** — if REST API blocks, use PHP via FTP
6. **Article quality** — find articles under 1,500 words, expand them
7. **Cron jobs** — verify all are running

### Phase 4 — Research (5 min)
Find new free promotion opportunities, check competitors, document findings.

---

## 🚫 Forbidden Patterns

### Never Do These
- ❌ Defer work to "next time" or "the CEO will handle it"
- ❌ Report issues without fixing them
- ❌ Save a draft and stop (pivot instead)
- ❌ Use AI-generated images (real CC photos only)
- ❌ Skip visual inspection of images
- ❌ Call articles "blog posts" or "content pieces"
- ❌ Leave articles in "Uncategorized"
- ❌ Use -2/-3 URL suffixes
- ❌ Work on main branch without a feature branch
- ❌ Skip Obsidian documentation

---

## ✅ Required Patterns

### Always Do These
- ✅ Fix everything you find in the same run
- ✅ Pivot when blocked — try the next platform
- ✅ Finish in this session — no deferrals
- ✅ Visually verify with `browser_vision()` before reporting
- ✅ Use 2 real CC photos per article, visually inspected
- ✅ Write 1,500+ words per article
- ✅ Commit git + write Obsidian doc + update state files
- ✅ Ping IndexNow after every content change
- ✅ Check sitemap freshness weekly

---

## 📞 Communication Standards

### When Reporting Progress
```
✅ GOOD:
"I've expanded 3 articles on whippetqr from 250w to 1,900w each.
IndexNow pinged. Obsidian doc written. PROJECT_STATE updated."

❌ BAD:
"The CEO will handle the rest tomorrow."
```

### When Something Goes Wrong
```
✅ GOOD:
"The PHP script hit a memory limit. I switched to a file-based
approach and the update succeeded."

❌ BAD:
"I can't do this because of X. Let me know if you want me to try Y."
```

---

## 📁 File Organization

### Project Files
```
/home/m/Documents/HermesBrain/
├── CEO-of-Domains/           # Daily run docs
│   ├── 2026-07-31-*.md
│   └── 2026-08-03-*.md
└── Projects/
    └── Portfolio-Sites/
        ├── PROJECT_STATE.md  # Current state of all 7 sites
        ├── DESIGN_SYSTEM.md  # Per-domain design specs
        └── RULES.md          # This file
```

### Skill Files
```
~/.hermes/skills/promotion/ceo-of-domains/SKILL.md  # CEO v2.1
```

---

## 🔄 Continuous Improvement

### After Every CEO Run
- [ ] All issues found were fixed (not just reported)
- [ ] If a platform was blocked, a pivot was executed
- [ ] Nothing was deferred to "next time"
- [ ] Obsidian doc written for the day's work
- [ ] PROJECT_STATE.md updated if anything changed
- [ ] DESIGN_SYSTEM.md updated if design patterns changed
- [ ] RULES.md updated if new rules discovered
