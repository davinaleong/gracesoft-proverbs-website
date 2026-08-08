# GraceSoft Proverbs — microsite milestone checklist

Tracks the site from its current draft state through to launch. Grouped in the order you'd realistically work through them; re-run "Cross-page consistency" and "Pre-launch" after any content change.

## Content readiness

- [ ] Legal review: `privacy.html` and `terms.html` reviewed by a lawyer (or someone qualified) for your actual jurisdiction; remove the "not reviewed" callout once done.
- [ ] Confirm `davinaleong@gracesoft.com` is the correct, actively-monitored support address across all four pages.
- [ ] Confirm launch order ("iOS first, Android to follow") still matches the real plan; update hero CTA, closing CTA, and FAQ together if it changes.
- [ ] Confirm all five translation credit lines match the design brief exactly (WEB 2000, KJV 1611, ASV 1901, YLT 1898, Douay-Rheims 1610/Challoner 1752).
- [ ] Proofread all four pages for typos and tone consistency in one pass, read aloud if possible.

## Brand & asset readiness

- [ ] Confirm Google Fonts (`Playfair Display`, `Montserrat`) load correctly in a network-restricted or offline test — add a self-hosted fallback if the site must work without external font requests.
- [x] Favicon and app icon added — using `logo-square.png` from the brand asset set; swap for the final app icon if it ever differs from this mark.
- [ ] Open Graph / social preview image created for link unfurling (link shares to Slack, iMessage, X, etc. currently have no preview image).
- [ ] `<title>` and meta description reviewed per page for search/share quality.

## Page-by-page build

- [ ] **Home** — hero calendar strip and "today's chapter" logic verified against the real device date, including near-midnight and month-end (28–31 day) edge cases.
- [ ] **Home** — all 18 theme swatches render their correct hex values; spot-check against `04-app-themes.md`.
- [ ] **Home** — all 5 translation cards match the source descriptions and credit lines.
- [ ] **Privacy** — table of contents anchors all scroll to the correct section.
- [ ] **Terms** — table of contents anchors all scroll to the correct section.
- [ ] **Support** — FAQ answers double-checked against current app behavior (notification permission timing, offline behavior, settings reset on reinstall).

## Cross-page consistency

- [ ] Nav links on every page point to the correct file (`index.html`, `privacy.html`, `terms.html`, `support.html`) with no broken relative paths.
- [ ] Footer links match the nav links on every page.
- [ ] "Current page" nav highlight (`.nav-link.current`) is set correctly on each page — Home doesn't need it, the other three do.
- [ ] Light/dark toggle produces the same look on every page when clicked (no page loads with a different default than the others).
- [ ] `assets/styles.css` and `assets/theme.js` are referenced with correct relative paths from every page — verify after any folder restructuring.

## Responsive & accessibility QA

- [ ] Test at 1440px, 860px (tablet breakpoint), and 375px (small phone) widths on Home, and at 860px/375px on the three inner pages.
- [ ] Calendar strip and translation grid remain legible (no overlapping text) at the smallest tested width.
- [ ] All interactive elements (nav links, theme toggle, buttons, FAQ text) are reachable and operable by keyboard alone (Tab / Enter).
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