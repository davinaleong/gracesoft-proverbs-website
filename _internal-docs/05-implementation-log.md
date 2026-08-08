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

## Session 2 — Privacy, Terms, Support + cross-page QA

**Built:**

- `src/pages/privacy.astro` — 10-section policy (overview, what we collect, device-only settings, notifications, no tracking/analytics, third-party services, children's privacy, retention/deletion, changes, contact), legal-article layout with a two-column TOC, "not reviewed by a lawyer" notice per the brief.
- `src/pages/terms.astro` — 11-section terms (acceptance, license, the translations, no warranty, limitation of liability, cost, acceptable use, termination, governing law, changes, contact), same layout and legal notice. Governing law is left as an explicit placeholder pending real legal review — no jurisdiction was specified anywhere in `_internal-docs/`.
- `src/pages/support.astro` — contact card (`mailto:`) plus a 10-item FAQ, hairline-divided, no accordion, per the brief. Covers the three topics the milestone checklist calls out by name (notification permission timing, offline behavior, settings reset on reinstall) plus platform/cost/account/translation basics.

**Bugs found and fixed during verification, both real:**

1. **Astro whitespace collapse:** wherever a paragraph's source had a line break immediately before an inline `<a>`/`<code>` tag, Astro's compiler dropped the whitespace entirely instead of collapsing it to one space (confirmed via raw HTML: `sent to<a href=...>`, `for<code>...`). Fixed in `privacy.astro` and `terms.astro` by keeping inline links on the same source line as the text before them, rather than trusting HTML's normal newline-to-space collapsing.
2. **Mobile nav overflow:** the four-page nav (wordmark + Privacy/Terms/Support + toggle) overflowed the viewport at 375px — `nav-actions` needed 242px but only had ~186px available. Fixed with a `520px` media query in `global.css` that tightens the nav gap, link font-size, and wordmark size, plus reduces `.wrap` padding slightly. Verified zero horizontal overflow afterward via `document.documentElement.scrollWidth`.
3. **Support page mobile overflow:** the contact card's `<h2>davinaleong@gracesoft.com</h2>` is one unbroken string with no spaces, so it didn't wrap at 375px and pushed the page 13px wider than the viewport. Fixed with `overflow-wrap: break-word` on `.contact-card h2`.

**Verified:** all four routes return 200 with no console errors; nav `.current` highlight confirmed correct on all three inner pages and absent on Home via raw HTML; light/dark toggle confirmed working; TOC `href`s on Privacy/Terms confirmed to match heading `id`s 1:1; zero horizontal overflow confirmed at 375px, 860px, and 1440px on all four pages after the fixes above; every interactive element is a native `<a>`/`<button>`, so keyboard reachability holds by construction.

**Updated `02-milestone-checklist.md`** to check off everything verified this session (see the file for the full list) — legal review, jurisdiction, the launch-order business decision, font offline-loading, the OG image, and color-contrast/screen-reader audits are explicitly left unchecked since they need either a real lawyer, a real decision from the site owner, or tooling beyond what was used here.

## Open questions for the site owner (not blocking, tracked here so they aren't lost)

- Legal review of Privacy/Terms hasn't happened — both pages will ship with the "not reviewed by a lawyer" notice the brief calls for, per `02-milestone-checklist.md`'s first item.
- No `02-technical-document.md` or `04-app-themes.md` exist in `_internal-docs/` (both are referenced by the brief/checklist). Theme hex values were instead spot-checked against `03-brand-guidelines.md` and the mockup HTML directly.
- Support page FAQ content (notification permission timing, offline behavior, settings-reset-on-reinstall) is being drafted from the app description in the brief, since no separate app-behavior spec exists in this repo. Flagged for a real QA pass once the app itself exists to check against.
- Terms' "Governing law" section is a placeholder (no jurisdiction named anywhere in `_internal-docs/`) — needs a real value from the site owner before legal review.
