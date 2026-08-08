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

## Session 3 — DPO section + contact form embed space

**Requested by the site owner:** name a Data Protection Officer on the Privacy page, and reserve layout space on Support for an embedded contact form (in addition to the existing `mailto:` contact card).

**Built:**

- `src/pages/privacy.astro` — new "Data Protection Officer" section (between "Data retention and deletion" and "Changes to this policy"), naming Davina Leong as DPO and the contact point for access/correction/deletion requests. Added to the TOC.
- `src/pages/support.astro` — new "Contact form" area between the existing contact card and the FAQ. Controlled by a `contactFormEmbedUrl` constant at the top of the file: empty (current state) renders a dashed-border placeholder explaining what goes there; setting it to a real embed URL (Google Form, Tally, Typeform, etc.) swaps in a live `<iframe>` automatically — no other code changes needed.
- `src/styles/global.css` — `.iframe-embed` / `.iframe-embed--placeholder` styles, sized so the reserved space (320px min-height) reads intentionally rather than like a layout gap. Also added Source Code Pro (the brand's monospace font, per `03-brand-guidelines.md`) to the Google Fonts import, since the placeholder text references a variable name in `<code>`.
- Updated `01-design-brief.md`'s "Non-goals" section — it previously said "no contact form or backend," which this reverses. Noted that the iframe is a client-side embed of a third-party form, not a backend the site itself hosts, so the "no CMS/backend" spirit still holds. Also added a content-principle line naming the DPO.

**Caught and fixed while writing this:** the DPO paragraph's mailto link was drafted with a line break right before the `<a>` tag — the same Astro whitespace-collapse issue from Session 2. Fixed by keeping it on one line before verifying.

**Verified:** both pages render with no console errors, no whitespace-collapse artifacts (checked via the same raw-HTML grep as Session 2), and no horizontal overflow at 375px or desktop width.

**Still open:** `contactFormEmbedUrl` is empty — the actual form (and which service to use) is up to the site owner to set up and paste in.

## Session 4 — Wordmark logo in the nav

**Requested:** replace the "GraceSoft Proverbs" text wordmark in the nav with the `wm` logo asset.

**Built:** `Layout.astro`'s wordmark link now renders two `<img>`s — `wm-b.svg` (black, for light theme) and `wm-w.svg` (white, for dark theme) — with CSS driven by the existing `data-theme` attribute swapping which one is visible (`.wordmark-logo-dark` hidden by default, `html[data-theme='dark'] .wordmark-logo-light` hidden / `.wordmark-logo-dark` shown). No JS changes needed since the theme toggle already sets `data-theme`. Removed the now-unused `.wordmark .g` / `.wordmark .p` text-span styles from `global.css`, including their mobile override.

**Also fixed, same session:** two commits landed from outside this conversation while this was in progress (`ee9d8b6` "Added an Capture form" and `437e469` "Reimported the assets"). The second one flattened `public/assets/*` into `public/*` directly (so assets are now at `/wm-b.svg` etc. instead of `/assets/wm-b.svg`) and swapped the favicon from `favicon.ico`/`favicon.svg` to `logo-square.png` — but it appears to have picked up this session's in-progress `Layout.astro`/`global.css` edits (the wordmark-swap code) without updating the asset paths inside them to match the new flat layout, so the wordmark images and favicon were 404ing. Fixed by updating the three references in `Layout.astro` from `/assets/wm-b.svg`, `/assets/wm-w.svg`, `/assets/logo-square.png` to `/wm-b.svg`, `/wm-w.svg`, `/logo-square.png`. Verified all three now return 200 and render on all four pages, in both themes, with no console errors.

**Also noticed, not yet acted on:** the "Added an Capture form" commit replaced the `contactFormEmbedUrl`-placeholder pattern on `support.astro` with a live embed (`https://capture.gracesoft.dev/form/frm_a799c8335185b4d7ec026ddbdd8905df`). That's a real third-party form now collecting name/email/message, which `privacy.astro`'s "What we collect" and "Third-party services" sections don't yet mention — both currently say the site collects nothing server-side. Flagged to the site owner directly; not edited here since it's a content/disclosure decision, not a layout one.

## Session 5 — bug: today's calendar cell wasn't highlighted

**Reported:** the current day of the month wasn't visually highlighted in the Home hero's calendar strip.

**Root cause:** Astro scopes a page's `<style>` block by adding a hashed `data-astro-cid-*` attribute to every element written in that page's template, and compiling each selector to require that attribute. The calendar cells and phone-mockup verse bars aren't in the template — they're created at runtime by the page's `<script>` (`document.createElement('div')`, per Session 1) — so they never get the scope attribute, and every rule targeting them (`.cal-cell`, `.cal-cell.today`, `.verse-line`, `.verse-num`, `.verse-bar`) silently failed to match. Confirmed via computed styles: the "today" cell and a plain cell were pixel-identical (transparent background, `0px` border-radius) before the fix. This bug shipped in Session 1 and affected the whole calendar strip and verse-bar mockup the entire time, not just the highlight — it just wasn't obvious because the surrounding layout (grid, gaps) still worked and the missing border/rounding read as "plain."

**Fix:** wrapped those five selectors in `:global(...)` inside `index.astro`'s `<style>` block, which tells Astro's compiler to emit them unscoped. Left every other rule in that block alone, since their target elements are static template content and scoping already worked correctly for them (including `.progress-fill`, which is a static element whose inline `width` is set by JS — no scoping issue there).

**Verified:** today's cell now has the accent background/border and bold text; other cells have their border and radius back; verse bars render as rounded gray bars again. No console errors.

**Worth knowing for future JS-inserted DOM in this project:** any element created client-side in an Astro page needs its styling rules either in `global.css` or wrapped in `:global()` inside the page's scoped `<style>` block — scoped selectors never match runtime-created nodes.

## Session 6 — bigger calendar numbers

**Requested:** make the day numbers in the Home calendar strip bigger — they were hard to read at `0.58rem`.

**Built:** bumped `.cal-cell` to `0.72rem`.

**Bug introduced and caught in the same pass:** first attempt also bumped the font size further (to `0.9rem`) specifically at the 860px/15-column breakpoint, on the theory that wider cells there had room for it. That broke 375px: "31" at 0.9rem no longer fit in a 15-column row at that width, and because CSS Grid's `1fr` tracks default to `min-width: auto` (never shrinking below their content's min-content size), the oversized digit pushed the whole grid — and the page — 13px past the viewport, reintroducing exactly the horizontal-overflow bug fixed in Session 2. Caught by the same overflow check used in Session 2 (`document.documentElement.scrollWidth` vs `clientWidth`) before it shipped.

**Fix:** dropped the breakpoint-specific size (0.72rem now applies everywhere and reads fine at all three tested widths), and changed `.cal-grid`'s columns from `repeat(N, 1fr)` to `repeat(N, minmax(0, 1fr))` at both the 31-column and 15-column definitions. `minmax(0, 1fr)` overrides Grid's automatic content-based minimum, so a track can shrink to fit its allotted space even if that's narrower than its content's natural width — this is the standard defense against exactly this failure mode, and makes the calendar strip robust against future font/content-size changes without needing to re-verify every breakpoint by hand.

**Verified:** no horizontal overflow at 375px, 860px, or desktop width; "31" renders cleanly at all three; today's highlight (Session 5's fix) still applies since `:global(.cal-cell)` wasn't touched.

## Open questions for the site owner (not blocking, tracked here so they aren't lost)

- Legal review of Privacy/Terms hasn't happened — both pages will ship with the "not reviewed by a lawyer" notice the brief calls for, per `02-milestone-checklist.md`'s first item.
- No `02-technical-document.md` or `04-app-themes.md` exist in `_internal-docs/` (both are referenced by the brief/checklist). Theme hex values were instead spot-checked against `03-brand-guidelines.md` and the mockup HTML directly.
- Support page FAQ content (notification permission timing, offline behavior, settings-reset-on-reinstall) is being drafted from the app description in the brief, since no separate app-behavior spec exists in this repo. Flagged for a real QA pass once the app itself exists to check against.
- Terms' "Governing law" section is a placeholder (no jurisdiction named anywhere in `_internal-docs/`) — needs a real value from the site owner before legal review.
