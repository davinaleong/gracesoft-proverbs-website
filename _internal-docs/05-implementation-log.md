# Implementation log

Tracks what's been built against `01-design-brief.md` and `02-milestone-checklist.md`, and any decisions made where the brief was ambiguous. Updated after each work session, in the order the work happened.

## Framework choice

The brief describes four static HTML files sharing `assets/styles.css` / `assets/theme.js`. The repo is an Astro project (Astro starter, not touched since init), so the same outcome is built as Astro pages instead: one `Layout.astro` (nav, footer, theme toggle script) wrapping `src/pages/index.astro`, `privacy.astro`, `terms.astro`, `support.astro`. Astro emits static HTML at build time, so the "four static files, one stylesheet, one script, no CMS" intent from the brief's non-goals still holds — it's just authored as components instead of hand-duplicated HTML.

## Session 1 — scaffold + Home page

**Built:**

- `src/layouts/Layout.astro` — shared `<head>`, sticky nav (wordmark + Privacy/Terms/Support links + theme toggle), footer, and the light/dark toggle script (defaults to `prefers-color-scheme`, per-page state, no persistence — matches the brief).
- `src/styles/global.css` — color tokens transcribed verbatim from `03-brand-guidelines.md` / the brief's light-dark table, plus shared nav/button/section/footer/legal-article/FAQ component styles.
- `public/assets/` — `wm.svg`, `wm-b.svg`, `wm-w.svg`, `logo.svg`, `logo-square.png` copied in from `_internal-docs/assets/`. Favicon now points at `logo-square.png` (was the default Astro placeholder icon) — closes one item on the milestone checklist's "Brand & asset readiness" list.
- `src/pages/index.astro` — full Home page ported from `_internal-docs/gracesoft-proverbs.html`: hero with live date → chapter mapping, 31-cell calendar strip, phone mockup (styled bars, no real verse text, per the brief's content principle), five translation cards, all 18 theme swatches across three groups, six-item feature grid, closing CTA.

**Decision — nav link set:** The brief's component list says nav is "wordmark, page links, theme toggle," and the milestone checklist says "Home doesn't need [the current-page highlight], the other three do." Read literally, that only works if the nav's page links are Privacy/Terms/Support and the wordmark itself is the way back to Home (no separate "Home" link to highlight). Implemented it that way: nav = wordmark (→ `/`) + Privacy + Terms + Support + toggle, same set on all four pages, footer mirrors the same three links plus the feedback `mailto:`. The original single-page mockup's nav (Translations/Themes/Get the app anchors) only made sense for a one-page prototype and isn't part of the four-page nav.

**Verified:** dev server (`astro dev --background`), loaded Home in the browser pane, checked the accessibility tree renders all sections with real content (date-driven hero text picked up today's date correctly) and no console errors.

**Not done yet:** Privacy, Terms, Support pages; cross-page consistency pass; responsive/a11y QA pass; anything under "Pre-launch" or "Post-launch" in the milestone checklist (those depend on real store URLs, legal review, a real publish date — out of scope for this implementation pass).

## Open questions for the site owner (not blocking, tracked here so they aren't lost)

- Legal review of Privacy/Terms hasn't happened — both pages will ship with the "not reviewed by a lawyer" notice the brief calls for, per `02-milestone-checklist.md`'s first item.
- No `02-technical-document.md` or `04-app-themes.md` exist in `_internal-docs/` (both are referenced by the brief/checklist). Theme hex values were instead spot-checked against `03-brand-guidelines.md` and the mockup HTML directly.
- Support page FAQ content (notification permission timing, offline behavior, settings-reset-on-reinstall) is being drafted from the app description in the brief, since no separate app-behavior spec exists in this repo. Flagged for a real QA pass once the app itself exists to check against.
