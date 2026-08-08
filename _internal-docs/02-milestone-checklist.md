# GraceSoft Proverbs — microsite milestone checklist

Tracks the site from its current draft state through to launch. Grouped in the order you'd realistically work through them; re-run "Cross-page consistency" and "Pre-launch" after any content change.

## Content readiness

- [ ] Legal review: `privacy.html` and `terms.html` reviewed by a lawyer (or someone qualified) for your actual jurisdiction; remove the "not reviewed" callout once done.
- [ ] Confirm `davinaleong@gracesoft.com` is the correct, actively-monitored support address across all four pages.
- [ ] Confirm launch order ("iOS first, Android to follow") still matches the real plan; update hero CTA, closing CTA, and FAQ together if it changes.
- [x] Confirm all five translation credit lines match the design brief exactly (WEB 2000, KJV 1611, ASV 1901, YLT 1898, Douay-Rheims 1610/Challoner 1752).
- [ ] Proofread all four pages for typos and tone consistency in one pass, read aloud if possible.

## Brand & asset readiness

- [ ] Confirm Google Fonts (`Playfair Display`, `Montserrat`) load correctly in a network-restricted or offline test — add a self-hosted fallback if the site must work without external font requests.
- [x] Favicon and app icon added — using `logo-square.png` from the brand asset set; swap for the final app icon if it ever differs from this mark.
- [ ] Open Graph / social preview image created for link unfurling (link shares to Slack, iMessage, X, etc. currently have no preview image).
- [x] `<title>` and meta description reviewed per page for search/share quality — each of the four pages sets its own via `Layout.astro` props.

## Page-by-page build

- [ ] **Home** — hero calendar strip and "today's chapter" logic verified against the real device date, including near-midnight and month-end (28–31 day) edge cases.
- [ ] **Home** — all 18 theme swatches render their correct hex values; spot-check against `04-app-themes.md`.
- [x] **Home** — all 5 translation cards match the source descriptions and credit lines.
- [x] **Privacy** — table of contents anchors all scroll to the correct section (verified each `href="#id"` matches a heading `id` 1:1; `html{scroll-behavior:smooth}` is set globally).
- [x] **Terms** — table of contents anchors all scroll to the correct section (same verification as Privacy).
- [ ] **Support** — FAQ answers double-checked against current app behavior (notification permission timing, offline behavior, settings reset on reinstall).

## Cross-page consistency

- [x] Nav links on every page point to the correct route (`/`, `/privacy`, `/terms`, `/support`) with no broken paths.
- [x] Footer links match the nav links on every page (Privacy, Terms, Support, plus the feedback `mailto:`).
- [x] "Current page" nav highlight (`.nav-link.current`) is set correctly on each page — Home doesn't need it (nav has no separate "Home" link; the wordmark serves that role), the other three do. Verified via rendered HTML on all three inner pages.
- [x] Light/dark toggle produces the same look on every page when clicked — all four pages share one `Layout.astro` and one toggle script, so there's nothing to drift.
- [x] Global styles are loaded consistently from every page — N/A in the Astro-built version of this site (styles are imported into `Layout.astro` and bundled at build time rather than linked as a static relative path), and no console/network errors were observed on any of the four pages.

## Responsive & accessibility QA

- [x] Test at 1440px, 860px (tablet breakpoint), and 375px (small phone) widths on Home, and at 860px/375px on the three inner pages — checked programmatically (no horizontal overflow) at each width; found and fixed two real bugs in the process (see log).
- [x] Calendar strip and translation grid remain legible (no overlapping text) at the smallest tested width — confirmed no element overflow at 375px after the nav and contact-card fixes.
- [x] All interactive elements (nav links, theme toggle, buttons) are reachable and operable by keyboard alone — every interactive element is a native `<a>` or `<button>` (no custom click-handler `div`s), so this holds by construction. FAQ text on Support isn't interactive (no accordion, per the brief), so there's nothing to reach there.
- [ ] Color contrast spot-checked in both light and dark mode, particularly Text-muted on Page background in light mode (2.5:1 by design — confirm nothing essential relies on it, per the app design brief's own caveat).
- [ ] Screen reader pass (VoiceOver or a browser extension) on Home and one legal page — confirm heading order and link text make sense out of context.

## Pre-launch

- [ ] Real App Store / Google Play URLs swap in for the `mailto:` notify-me CTAs once listings are live — update Home hero and closing CTA together.
- [ ] Domain and hosting confirmed, HTTPS enabled.
- [ ] 404 handling checked (even a plain fallback) if the host doesn't provide one by default.
- [ ] Final read-through of Privacy and Terms dates ("Last updated") bumped to the actual publish date.
- [ ] Site checked in at least one non-Chromium browser (Safari or Firefox) for font and layout parity.

## Post-launch

- [ ] Support inbox monitored for the first week after the app itself launches, since that's when FAQ gaps tend to surface.
- [ ] Revisit the FAQ after the first round of real user questions and add anything that comes up more than once.
- [ ] Re-run "Cross-page consistency" and "Pre-launch" checks after any copy or link change post-launch.