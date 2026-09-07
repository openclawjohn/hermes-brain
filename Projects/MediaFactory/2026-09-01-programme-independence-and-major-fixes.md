# Media Factory — 2026-09-01: Programme Independence, Educational Content, Image Factory, AI-busy framework

**Date:** 2026-09-01
**Branch:** `feature/design-system-crm` — all 43 commits today pushed to Gitea.

## One theme: programme-independent machine
The user's repeated correction: *"Our system must be programme independent. Get the system right, it must work without you. Fix every fix for the whole machine, not one event."* Every fix today went into SHARED model/form/template code, verified to apply across all 5 programmes — never a per-event patch.

## What was done

### 1. Educational Content (type 01) — AI picks its own topic
- `ContentRequest` gained a `topic` field; generate form has a **free-text Topic/Theme input** (shown only for Educational Content) — NOT a fixed dropdown. User: *"I do not want to think of all the topics, that is why I have AI. These topics cannot be fixed."*
- **Blank topic = the AI chooses its own fresh subject** from the knowledge base, and varies it week to week (prompt: "Pick a DIFFERENT subject each time… vary the angle").
- **New SOCIAL-MEDIA FORMAT block** injected into the prompt: this is IG/FB, NOT a blog — 130–200 words, short sentences, 2–4 paragraphs, gentle CTA, no hashtags/headings/bio-link. The first test (blank topic, model self-selected) produced a clean short post on "what defines a truly high-quality product."
- `prompt_builder.py`:
  - `load_editorial_guide(slug)` — reads `editorial-guide.md` (brand voice + writing style) from `engine/config.yaml defaults.editorial`. Previously configured but **never loaded** (a real gap).
  - `load_knowledge_files(slug, list)` — injects `content_types.<type>.knowledge` files as factual basis.
  - `topic_block` resolves the free-text topic; full knowledge base injected for educational.
  - Editorial guide + knowledge only load for content types that opt in via `editorial: true` in config (educational only) — keeps winner-announcement/date-reminder prompts lean (~3k–17k chars) vs educational ~35k (within gemma4 26b's 262k context).
- Aurora config: `01-educational-content` = `editorial: true` + full knowledge base list (org identity, blind judging, sensory eval, product quality, consumer trust, awards).

### 2. Knowledge → Event Participants & Results; Content re-org
- Renamed the Knowledge page/button to **"Event Participants & Results"** in the sidebar, event card, breadcrumbs, back links (base.html, event_detail.html, library/index.html, participants.html, type_index.html).
- Moved **Editorial Guide + FAQs** off that page to **Content Library** header buttons. The Results page now holds only Judges, Participants & Winning Products, Partners.
- Verified: **137 Aurora 2026 winners imported** (all with product images) via the existing CSV+images Import flow (`/knowledge/aurora/2026/participants/` → Import CSV). These feed Aurora 2027's promotional posts (this-year content uses last-year's winners).

### 3. Image Factory — winner thumbnail grid
- Replaced the messy "Product Image" **asset-path dropdown** with a **clickable thumbnail grid of the imported winners** (product photo + name + award badge). `assets/views_image_factory.py` resolves `Participant.objects.filter(event=..., active=True).select_related("asset")` with a file; falls back to the old dropdown only when no winners exist. Verified: all 137 Aurora 2026 winners render as cards.

### 4. Programme settings — two distinct URL fields (entry vs registration)
- **History:** a prior session added `register_url` then REMOVED it (migration `0032`), collapsing both actions into one `entry_url` — losing any previously-entered value. User caught this.
- **Fix (migration `0033`):** two distinct, programme-independent fields on `Programme`:
  - `entry_url` — "Link for producers to enter (submit) a product for judging."
  - `registration_url` — "Link to register / attend the event or buy tickets."
- Form + Programme Settings page show both clearly-labelled; milestone preview renders separate Enter/Register links. Both on `Programme` = one value feeds all years.

### 5. Programme Settings form no longer fails validation (name/slug required)
- The settings card declared `name`/`slug` in `ProgrammeForm` but **never rendered them** → every Save failed "this field is required" and silently discarded the typed URL fields. That is why URLs kept "disappearing" (they never saved).
- Fix: rendered an editable **Programme name** input + hidden **auto-slug** (JS sluggenerator from name). Verified a real save returns 302 with no errors.

### 6. Holidays — global + discoverable
- 12 holiday records (2027) exist at `/programmes/holidays/` (Human Rights, Good Friday, Family Day, Freedom Day, Workers', Women's, Heritage, Reconciliation + school holidays).
- Added a **🗓 Holidays link to the global sidebar under Programmes** (affects all events).

### 7. AI-busy framework — failures render through the busy screen, not legacy templates
- Root cause of the "old screen flash": when AI work failed, `except` blocks rendered a **stale legacy template** (`visual_identity_analyse.html`) instead of the busy screen. For gold-wine-awards it failed because the event has **0 active image assets** (nothing for the AI to read).
- **Framework fix (whole machine):**
  - New `content/services/ai_busy.py`: `render_ai_busy()` + `render_ai_busy_error()`.
  - `common_ai_busy.html` gained an **error state** (⚠️ reason + back button, no auto-submit).
  - Refactored brand Analyse to use both helpers on GET (spinner) and POST failure (error screen).
- Verified end-to-end: POST failure renders the error screen with the real reason + "Back to Brand Studio" — no legacy template.

## Key decisions / corrections
- **Topic = free-text, AI-chosen, never fixed.** User must not think of topics.
- **This year's content uses last year's winners** (Aurora 2027 → 2026 results; 2028 → 2027).
- **Entry and registration are different URLs** — never collapse them again.
- **Programme-independent always** — shared code, all programmes at once.

## Verification
- `manage.py check` clean; all changed pages HTTP 200 (curl + test client).
- Real AI generation test produced a valid short social post (blank-topic, model self-selected).
- Test-client save returned 302 (no more validation failures).

## Remaining / next
- The **weekly planner** (80/20 education↔real-winner, twice-weekly 08:00, holiday skip) — user has no season dates yet, so time to fine-tune. Holidays (2027) already in DB.
- Fine-tune educational post format once a couple are published.
- Upload branding images for gold-wine-awards 2026 so Analyse has assets to read.
