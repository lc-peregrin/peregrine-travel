# Handoff: Peregrin evolution pass — trust band, Help nav, FAQ polish, SEO template

## Overview
Four small, on-brand changes to the live Peregrin site (`peregrin-demo`, deployed on Vercel).
This is an **evolution, not a redesign** — reuse the existing tokens and component patterns; keep
diffs surgical so integration is low-risk. Goals: make help reachable by design, make the trust
signals more prominent at the point of decision, and add a reusable programmatic-SEO landing page.

Target codebase: `public/index.html` — a single-file, no-build, no-framework app (inline
HTML/CSS/JS) + `server.js` (Express). Tokens verified against `main` @ `2451890`.

## About the design files
The `.dc.html` files in this bundle are **design references** — HTML prototypes showing intended
look and layout. **Do not ship them or copy their markup wholesale.** Recreate each change inside
`public/index.html` using the site's *existing* classes, tokens, and i18n system. Each mockup maps
to a specific element already in the file (selectors below). `support.js` is only the preview
runtime for the mockups; it is **not** part of the deliverable.

## Fidelity
**High-fidelity.** Final colors (existing tokens), spacing, and copy. Recreate pixel-close using
the existing CSS. One deliberate exception — typography — see the flag below.

## ⚠ Flag before you start — typeface conflict
The brief lists the design system as "Source Serif 4 + Public Sans," and the mockups render in that
pairing. **The live `index.html` is still on the system stack**
(`-apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, Arial, sans-serif`) — the webfonts are
not wired in. Two options, Liam's call (not yours to decide silently):
- **Adopt the pairing:** add a Google Fonts `<link>` (or self-host) for `Source Serif 4` (h1/h2)
  + `Public Sans` (body), then set `font-family` on `body` and headings. Both are SIL OFL (free).
- **Stay on system stack:** ship all four changes unchanged — the layouts are font-agnostic.
Every layout in this bundle works either way. Do not block the other three changes on this decision.

---

## Change 1 — Trust band (strengthen the footer trust row)
**Mockup:** `Peregrin Homepage Chrome.dc.html` (bottom of the page) + callouts.
**Maps onto:** the `<footer class="site-footer">` block in `index.html` — currently
`.footer-trust` (a single bullet string), `.footer-links`, `.footer-disclosure`.
**Do:**
- Replace the single-line `.footer-trust` string with **four pillar cards** on a soft
  `--accent-bg` gradient band. Copy for the four pillars (reuse `FAQ_AND_TRUST_COPY` §B/§D):
  Real reservations · Independently verifiable · Delivered in minutes · Secured by Stripe — each
  with a one-line supporting sentence (see mockup).
- Promote `.footer-disclosure` ("A held reservation, not a purchased ticket…") into a **prominent
  gold ribbon** (`--gold-bg`, `#ecd9ad` border) sitting directly above the dark footer row — keep
  the existing `footer_disclosure` i18n key and its four translations.
**New CSS:** a `.trust-band` grid + `.trust-pillar` card (model them on the existing `.card` /
`.faq-step` patterns). **i18n:** keep `footer_trust` for accessibility/fallback, add per-pillar
keys (`pillar_real`, `pillar_verifiable`, `pillar_fast`, `pillar_secure` + `_d` sub-lines) in all
four languages. **Surgical:** yes — CSS + one footer block; no JS logic change.

## Change 2 — Persistent Help / FAQ nav (restyle only)
**Mockup:** `Peregrin Homepage Chrome.dc.html` (header) + callout.
**Reality check:** the Help link **already exists and is always visible** —
`<a class="header-link" href="/faq" data-i18n="nav_help">Help</a>` inside `.header-right`, and it
already routes correctly. So this is **purely a restyle**, not a new element or a visibility fix.
**Do:** change `.header-link` from a plain muted text link into a **pilled, accent-tinted** item:
`--accent-bg` fill, `#cfe4ea` border, `--accent-dark` text, `100px` radius, a small `?`/help glyph
before the label. Keep it left of the `.lang-select`. Apply the same treatment on the `/faq` view's
header so it's consistent. **i18n:** unchanged (`nav_help` already translated ×4). **Surgical:**
yes — one CSS rule (`.header-link`), no markup/JS change.

## Change 3 — FAQ / Help page polish (consistency refinement)
**Mockup:** `Peregrin FAQ Page.dc.html`.
**Reality check:** the `/faq` page is **already substantially built** — `#faq-flow` → `#faq-content`
rendered by `renderFaqPage()` from the `faqData` object, with CSS already present for
`.faq-price-note`, `.faq-section-h`, `.faq-steps`/`.faq-step`, `.faq-pillars`/`.faq-pillar`,
`.faq-item`/`.faq-q`/`.faq-a`, `.faq-support`. The mockup **matches this structure** — treat it as
a visual consistency reference, not a rebuild.
**Do (additive only):** (a) align spacing/type with the evolved homepage; (b) optionally add the
dark **"Ready when you are" CTA card** back to the tool (shown in the mockup) after the FAQ list —
new small block, reuse `--ink` bg + white button. Keep the existing price callout, how-it-works
steps, and trust pillars. **Watch-outs (from `FAQ_AND_TRUST_COPY` §C/§E):** keep accommodation FAQ
copy out until Duffel Stays is approved; the confirmation-screen "Ticketed" heading
(`ticketed_title`/`ticketed_sub`) is a known English-only gap — good to close in the same pass.
**Surgical:** mostly — CSS + optional one content block in `faqData`/render.

## Change 4 — SEO programmatic landing template (NEW — the one real build)
**Mockup:** `Peregrin SEO Template v2.dc.html` (+ right-hand legend: title/meta pattern, heading
hierarchy, full dataset field list).
**Maps onto:** a **new server-rendered route** in `server.js` (e.g. `GET /onward-ticket/:country`)
that renders this template from a per-country data object — **not** an `index.html` edit and **not**
the SPA view system. Serve it as static/server-rendered HTML so it's fast and crawlable.
**Structure (SEO hygiene baked in):** one `<h1>` "Proof of onward travel for {{Country}}", ordered
`<h2>`s (What {{Country}} requires · How it works · A reservation that holds up · {{Country}} FAQ),
`<h3>` per FAQ question. Emit `FAQPage` JSON-LD from the 4 Q&A pairs. `<title>`/meta pattern and the
full token list are in the mockup's legend.
**Content:** every variable is a `{{ token }}`; **do not write real visa/immigration text** — it's
supplied separately as the dataset. The template drives the non-English language wedge too
(`{{ lang }}` token; e.g. `/es/prueba-de-viaje/{{country_slug}}`), per `MARKETING_PLAN.md` §3.
**Not thin:** per-page difference lives in `requirement_body`, the quick answer, and 4 real FAQs,
plus a working tool CTA on every page — the defence against 2025–26 core-update thin-content signals.
**Effort:** this is the only item that is more than a small change — new route + data source +
sitemap wiring. Everything else is CSS/copy on existing markup.

---

## Interactions & behavior
No new client-side logic for changes 1–3 (CSS + copy on existing, already-wired elements). The Help
link's routing already works — don't touch the nav JS. Change 4's template is server-rendered with a
static CTA that links into the existing search tool at `/`.

## Design tokens (verified against live `:root`)
```
--ink #16283a   --muted #5c6b7c   --line #e2e7ec   --bg #f8f9fb
--accent #1c6f8c   --accent-bg #e8f2f5   --accent-dark #124a5e
--gold #c9922e   --gold-bg #faf1e0   --success #1f7a5c   --success-bg #e7f4ee
Pillar/ribbon borders used in mockups: #cfe4ea (accent), #ecd9ad (gold), #c3e2d1 (success)
Radius: 8px (controls), 10–14px (cards), 100px (pills). Font (mockups): Source Serif 4 / Public Sans.
```

## Assets
Wing-mark SVG is inline in the header (3 open paths, `--accent`/`--accent`@.55/`--gold`) — reuse the
existing inline SVG, don't add a file. No photography. Favicons/OG already wired. Exported logo set
lives in the outer repo's `/design-exports` if a standalone mark is ever needed.

## Files in this bundle
- `Peregrin Homepage Chrome.dc.html` — changes 1 (trust band) + 2 (Help nav), shown in 1a context.
- `Peregrin FAQ Page.dc.html` — change 3 reference.
- `Peregrin SEO Template v2.dc.html` — change 4 template + legend (title/meta, headings, tokens).
- `RATIONALE.md` — why each change, per-goal, with the same index.html mapping.
- `support.js` — preview runtime for the mockups only; NOT part of the deliverable.

## Flag for Liam (not a design/code call)
`FAQ_AND_TRUST_COPY` §E: the Duffel hold is created at `/api/hold` *before* the fee is paid, so
unpaid holds may still incur Duffel's per-order cost. Surfaced for a decision; out of scope here.
