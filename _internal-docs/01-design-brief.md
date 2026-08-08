# GraceSoft Proverbs — microsite design brief

This brief covers the four-page marketing microsite (Home, Privacy, Terms, Support) that sits alongside the app. It inherits the brand system from the app's own design brief rather than defining a new one — same typefaces, same color tokens, same light/dark chrome behavior — so the site reads as a continuation of the app, not a separate skin.

## Scope

| Page | File | Purpose |
|---|---|---|
| Home | `index.html` | Explain the app's core mechanic, show the translation and theme range, convert to an install / notify-me action. |
| Privacy Policy | `privacy.html` | Plain-language account of what is and isn't collected. |
| Terms & Conditions | `terms.html` | License, content disclaimer, no-warranty, cost, termination. |
| Support | `support.html` | Official contact channel plus a self-serve FAQ. |

All four share one stylesheet (`assets/styles.css`) and one script (`assets/theme.js`) so tokens and the light/dark toggle never drift out of sync between pages.

## Typography

Unchanged from the app brief:

- **Playfair Display** — page headlines, section headings, the "Proverbs {N}" hero number, translation abbreviations. Reserved for moments that echo the reading experience.
- **Montserrat** — everything else: nav, body copy, buttons, labels, legal text, FAQ.
- Two weights only, regular and medium. No bold.

## Color tokens

Reused verbatim from the app's light/dark chrome tables — the site does not introduce new tokens:

| Token | Light | Dark |
|---|---|---|
| Page background | `#fafafa` | `#0e0e0e` |
| Surface / card | `#ffffff` | `#1e1e1e` |
| Text primary | `#0e0e0e` | `#fafafa` |
| Text secondary | `#4c4c4c` | `#d6d6d6` |
| Text muted | `#a3a3a3` | `#a3a3a3` |
| Border | `#d6d6d6` | `#4c4c4c` |
| Border strong | `#a3a3a3` | `#6f6f6f` |
| Accent | `#5544ed` (Indigo 600) | `#8d80fe` (Indigo 400) |
| Accent tint bg / text | `#f6f5ff` / `#4636d3` | `#1f1a51` / `#b1a8ff` |

The site defaults to the visitor's OS preference (`prefers-color-scheme`) and exposes a manual toggle in the nav, mirroring the app's own "system setting with override" pattern. The toggle state is per-page (no persistence needed for a marketing site) but every page reads the same tokens, so switching mid-browse never looks inconsistent.

## Components

- **Nav** — wordmark, page links, theme toggle. Current page gets the Accent color on its link (`.nav-link.current`) instead of a colored underline, keeping it consistent with the app's "hue communicates state sparingly" rule.
- **Buttons** — fill (Accent + on-accent text), outline (border + surface), disabled (muted border + muted text, non-interactive). Same three states as the app brief's button rules — one filled action per screen at most.
- **Calendar strip** — 31 cells, today filled in Accent. This is the site's one signature device: it makes the day-of-month → chapter mechanic legible in under a second, before any copy is read.
- **Phone mockup** — reading-screen silhouette. Verse content is rendered as gray line-bars, not real scripture text, so the marketing page can never misquote a translation or drift out of sync with the actual bundled text.
- **Translation cards** — abbreviation in Playfair italic, name, one-line description, muted credit line. Matches the design brief's translation-picker card shape.
- **Theme swatches** — background/surface/accent chips grouped exactly as the in-app picker (Defaults → Reading classics → Expressive), using the real hex values from the theme spec.
- **Feature grid** — hairline-divided 3-column grid, short label + one sentence. No icon library; a 2px accent mark stands in for an icon, keeping the same restrained visual language as the rest of the brand.
- **Legal article layout** — narrow (720px) column, a table-of-contents card at the top, Playfair subheadings, Montserrat body at 1.75 line-height for long-form legal reading.
- **FAQ list** — hairline-divided question/answer pairs, no accordion — everything visible, since the list is short enough to scan.

## Content principles

- **No fabricated store links.** Until the App Store and Play Store listings exist, CTAs point to a `mailto:` notify-me action instead of a dead or placeholder store URL.
- **Launch status stays accurate.** Copy currently reads "iOS first, Android to follow" — update this in both the hero and the closing CTA together if the rollout order changes again, and check `02-technical-document.md` for consistency.
- **No quoted scripture in mockups.** The phone mockup uses styled placeholder bars rather than real verse text, so the marketing page can't misquote a translation or go stale if wording is refined later.
- **Legal pages are a drafted starting point, not counsel.** Both `privacy.html` and `terms.html` carry a visible note that they haven't been reviewed by a lawyer — keep that note until they have been.

## Layout & responsiveness

- Breakpoints at 860px (hero and grids collapse to single/double column) and 520px (buttons stack full-width, translation grid goes single column).
- Max content width 1120px for marketing sections, 720px for legal prose — matches typical comfortable line length for long-form reading.

## Non-goals for this version

- No CMS or templating — four static files are simpler to maintain than the alternative, in keeping with the app's own "minimize ongoing maintenance" philosophy.
- No analytics or tracking scripts, matching the app's privacy stance.
- No contact form or backend — `mailto:` links only, so the site has nothing to host beyond static files.