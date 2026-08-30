# Media Factory — Right-Side Context Rail + V&S Template Answer (2026-08-27)

**Server:** 192.168.7.100 · `/srv/apps/media-factory/`
**Branch:** `feature/design-system-crm`

## Navigation fix
Added a **persistent right-side context rail** to `base.html` that appears automatically on every event-scoped page (whenever `event` is in context). It lists all the event's sub-pages:
- Event Overview
- Content Factory
- Content Library
- Assets
- Image Factory
- Knowledge
- Date Reminders
- Publishing

The current page is highlighted. This solves the "no menus on the right side" problem — the user always knows where they are and can jump to any event sub-page. Hidden on mobile (<1024px).

## Template question (V&S)
Vine & Spirit Awards has **no programme-specific content-studio**, so it falls back to the shared factory template at `/srv/ai/factory/content-studio/03-award-winner-announcements.md`. This is **byte-for-byte identical** to the Aurora template the user liked — same editorial style, tone guidance, and "no invented facts" rules. So the AI writes in the same voice for V&S.

If V&S needs its own tweaks later, we create a programme copy at `/srv/ai/programmes/vine-and-spirit-awards/content-studio/03-award-winner-announcements.md`.

## How the system knows what to generate
When the user selects a participant in Content Factory → Award Winner Announcement, the system:
1. Loads the participant's verified facts (name, product, award, category, social fields).
2. Loads the editorial template (shared factory for V&S).
3. Loads any event knowledge documents.
4. The local AI (gemma4:26b writer + gemma4:12b checker) writes the post.
5. Auto-composites the matching award overlay onto the product image.
6. Preview shows post + overlay + social fields.
