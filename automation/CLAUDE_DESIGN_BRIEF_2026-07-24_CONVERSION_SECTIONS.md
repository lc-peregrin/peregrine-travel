# Claude Design brief — Peregrin home-page conversion sections

Prepared 2026-07-24. **Run this AFTER Claude Code has landed the copy/rework pass (Prompt 1)** — the
copy and section scaffolding will already be on the page; your job is to make these sections look
as trustworthy and polished as the market leader's, inside Peregrin's existing design system.

## How to use this (bridging — important)
Claude Design runs in a separate project and is **read-only** on the Peregrin repo. So:
1. Design against the system below; **export a `.dc.html` bundle to `/design-exports`.**
2. Because you can't write into the live repo, Liam downloads the export and drops it into the repo's
   `/design-exports` folder, then Claude Code integrates it into `site/public/index.html`.
3. Do NOT rewrite the copy — Claude Code has already placed the exact strings. Style what's there.

## What Peregrin is (context)
Peregrin sells genuine, verifiable held airline reservations (real PNRs via Duffel) as proof of
onward/return travel for visas, immigration, and check-in. It's a real reservation, never a "fake"
ticket, and never claimed as a purchased ticket. The design job is to make that legitimacy *feel*
obvious and premium. The market leader (onwardticket.com) wins partly on a confident, benefit-led,
trust-heavy homepage — we want to match that polish while keeping our stronger "verifiable" story.

## Design system — match this exactly (source of truth: live `site/public/index.html` CSS)
Colours (use these tokens, do not introduce new brand colours):
- `--ink #16283a` (primary text) · `--muted #5c6b7c` (secondary) · `--line #e2e7ec` (borders)
- `--bg #f8f9fb` (page) · `--accent #1c6f8c` (primary teal) · `--accent-bg #e8f2f5` (accent tint)
- `--accent-dark #124a5e` (hover/dark) · `--gold #c9922e` (secondary, sparing) · `--gold-bg #faf1e0`
- `--success #1f7a5c` (confirmed states) · `--success-bg #e7f4ee`

Type: the site currently uses a system font stack; the team has decided to move to **Source Serif 4
(headings) + Public Sans (body)** — both free/OFL. Design with that pairing if you're introducing
webfonts, but keep it tasteful and fast; otherwise the system stack is acceptable. Confirm with the
live CSS before assuming.

Motif & tone: the brand uses a calm "wing-mark" motif, generous whitespace, warm-but-structured (not
cold/corporate, not travel-marketing fluff). Reuse the existing logo/wing assets in `/design-exports`
and `site/public/` — don't redesign the mark.

Must-haves: responsive (mobile-first), light AND dark rendering consistent, accessible contrast,
fast (no heavy libraries), and visually continuous with the existing hero/search tool (which stays
the hero — do not restyle the tool).

## Sections to design (5)

**1. Embassy / authority pull-quote — "Why a reservation, not a ticket"**
The centrepiece trust move. Style the section Claude Code placed: intro line + ONE verified pull-quote
and a framing paragraph. Give the quote an elegant pull-quote treatment — larger serif, small-caps or
muted attribution line ("Royal Norwegian Embassy — official tourist-visa document checklist"), a
restrained divider or subtle seal/emblem motif built from the wing-mark or a simple line device.
**Do NOT fabricate or imply an official government logo/crest** — keep it clearly an editorial quote,
not a forged endorsement. Accent or gold used sparingly for the quote mark. This should read
"authoritative and calm," not "salesy."

**2. "Perfect for" persona cards (4-up)**
Polish the four persona cards (Visa applicants / Digital nomads / Last-minute trips / Frequent flyers).
Each: a simple line icon (travel-appropriate, consistent stroke weight, in `--accent`), a short title
in `--ink`, one line in `--muted`. Even 4-up on desktop, 2×2 on tablet, stacked on mobile. Cards on
`--bg` with `--line` borders or a soft shadow — keep them light, not boxy.

**3. Benefit grid (the four trust pillars)**
A clean grid treatment of: Real reservations · Verify it yourself · Delivered in minutes · Secured by
Stripe. Icon + title + one line each. Visually distinct from the persona cards (e.g. horizontal
icon-left rows, or a 4-column strip) so the page has rhythm rather than two identical card rows.

**4. Sample-reservation viewer**
A tasteful modal / lightbox that opens from the "See a sample reservation" buttons. It shows the static
sample document (Claude Code provides the asset) with a clear but non-defacing `Sample` watermark
treatment (diagonal, low-opacity, `--muted`). The modal chrome should feel like a document preview:
subtle shadow, close affordance, "This is an example" caption. Mobile: full-screen sheet.

**5. Testimonials / social-proof slot (placeholder only)**
Design a testimonials section — cards or a simple quote row — styled and ready, but populated with
**clearly-placeholder** text. **Do NOT write or invent testimonials or star ratings** — Liam will
supply real ones. Make it obvious in the export that these are placeholders (e.g. "[Real testimonial
to be added]").

## Out of scope
Don't touch the search/booking tool, the checkout flow, the PDF, pricing, or copy wording. Don't add
new brand colours, stock photography, or a redesigned logo. Don't build the embassy quote as anything
resembling an official government seal.

## Deliverable
A `.dc.html` export bundle in `/design-exports` covering sections 1–5, using the tokens above, ready
for Liam to bridge into the repo for Claude Code to integrate. Note any assumptions you made about the
live CSS at the top of the export.
