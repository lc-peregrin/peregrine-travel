# Peregrin design — rationale & handoff

> **Two passes in this doc.** The **Evolution pass** (below, newest) is the current deliverable:
> the persistent Help nav, stronger trust band, FAQ polish, and the single reusable SEO template,
> mapped onto the live `index.html`. The original **Trust/inviting pass** follows it for reference.

---

# Evolution pass — 2026-07-24 (v2 brief)

Scope: evolve the live site, not redesign it — surgical changes that reuse existing tokens,
components, Source Serif 4 + Public Sans, and the wing-mark. Homepage direction 1a is kept (tool
stays the hero). The flat-fee price line and the Help→/faq route are already live; I designed
around them. Files: **Peregrin Homepage Chrome**, **Peregrin FAQ Page**, **Peregrin SEO Template v2**.

## Change 1 — Trust band (strengthens the existing footer trust row)
- **What:** the single-line `Real reservations · Independently verifiable · Delivered in minutes ·
  Secured by Stripe` row becomes four pillar cards on a soft `--accent-bg` gradient, with the
  "held reservation, not a ticket" disclosure as a prominent gold ribbon directly beneath.
- **Maps onto:** the footer trust-row `<div>` in `index.html` (the element currently rendering that
  bullet string) plus the existing disclosure line under it. Wrap the string in a 4-card grid using
  the existing card pattern; move the disclosure into the gold ribbon just above the dark footer.
- **Surgical?** Yes — reuses the `.card` box style and existing tint tokens; no new component. The
  four pillar copy lines are the approved microcopy already in `FAQ_AND_TRUST_COPY` §D.

## Change 2 — Persistent Help / FAQ header nav
- **What:** Help becomes a pilled, accent-tinted nav item (distinct from a plain "How it works"
  text link), persistent in the header on every screen, with the language switcher to its right.
- **Maps onto:** the `header` `.header-right` cluster in `index.html`. The `/faq` anchor already
  exists there as a small link — restyle it to the pill (reuse the `.tag`/countdown pill styling)
  and, ideally, promote it out of the demo-only area so it shows for all visitors, not just `?demo=1`.
- **Surgical?** Yes — it's a class/style change on an anchor that already routes. Must appear in the
  header on both `/` and `/faq` (the FAQ mock uses the identical header).

## Change 3 — FAQ / Help page polish
- **What:** re-lay the existing `/faq` content to match the homepage system — How-it-works 3-step
  cards, the flat-fee callout, three trust pillars, an accordion FAQ, and a dark CTA back to the tool.
- **Maps onto:** the client-rendered FAQ view. Copy is verbatim from `FAQ_AND_TRUST_COPY` §A/B/C
  (only the first FAQ item is shown expanded in the mock to keep it scannable; ship all items).
- **Surgical?** Mostly. Two things to confirm: (a) all four languages need these strings (the copy
  doc flags the English-only "Ticketed" heading gap — close it here too); (b) keep accommodation
  FAQ copy out until Duffel Stays is approved (copy doc §C flag).

## Change 4 — SEO programmatic landing template (new)
- **What:** one reusable template, `H1 = Proof of onward travel for {{Country}}`, with intro,
  quick-answer snippet box, requirement table, how-it-works, trust signals, a 4-item FAQ (emits
  `FAQPage` JSON-LD), internal links (to the tool and /faq), and a prominent CTA into search. Every
  variable is a `{{ token }}`; no real visa/immigration text (supplied separately). The right-hand
  legend lists the title/meta pattern, heading hierarchy, and the full dataset field list.
- **Maps onto:** a **new** server-rendered route in `server.js` (e.g. `/onward-ticket/:country`)
  rendering this template from a per-country data object — not a change to `index.html`. This is
  the one item that is **more than a small change**: it needs a new Express route + a data source.
  Keep it a static/server-rendered HTML page (no SPA nav) so it's fast and crawlable.
- **Language wedge:** the same template drives non-English pages (MARKETING_PLAN §3) via a `{{ lang }}`
  token — the highest-leverage SEO play per the plan, since incumbents' non-English SEO is thin.

## Flag for Liam (from the copy doc, not a design issue)
`FAQ_AND_TRUST_COPY` §E raises an economics point worth a decision: the Duffel hold is created at
`/api/hold` *before* the fee is paid, so unpaid holds may still incur Duffel's per-order cost.
Not a design call — surfacing it so it isn't lost.

---

# Trust/inviting pass — 2026-07-24 (v1 brief, for reference)

Prepared for Liam, 2026-07-24. Companion to the four design files (Claude Design DCs). This covers
what changed and why for each goal, and — flagged separately at the end — the decisions that need
your sign-off before Claude Code implements.

All work reuses the approved wing-mark and the live CSS palette (`--ink`, `--accent`, `--gold`,
etc.) unchanged. This is a refinement/extension of the current identity, not a rebrand. Every
screen is a static layout spec for Claude Code to translate into the existing single-file inline
CSS — no framework, no build step assumed.

## Files

- **Peregrin Homepage** — two homepage structures side by side (1a, 1b).
- **Peregrin Booking Flow** — in-flow trust treatments: all-in offer selection (1a) and the
  held-reservation / pay screen with trust beside the CTA (1b).
- **Peregrin Verify Page** — the public, shareable verification page.
- **Peregrin SEO Template** — the programmatic landing-page template, shown as two real examples
  (`/onward-ticket/thailand` and `/schengen-tourist-visa`).

---

## Goal 1 — more inviting & more trustworthy

The core insight from the research (`REVENUE_AND_EXPANSION_RESEARCH.md` §2) is that this is a
conversion/trust problem, not a decorating one: trust signals move the needle most when they sit
**at the point of decision**, not in top-of-page marketing. Every treatment below follows that.

- **Trust signal beside the payment button** (Booking Flow 1b). Directly under "Pay with card":
  secure-payment note, "no charge until you confirm", and a one-line "genuine, independently
  verifiable PNR" statement with a link to the public verify page. This is the single
  best-evidenced conversion lever in the research.
- **All-in pricing, visible early** (Booking Flow 1a). The offer card shows the full "all-in to
  confirm" price with a transparent breakdown (hold = free, fare, taxes) *before* any details are
  entered — no surprise at Stripe. Every alternative offer also carries an "all-in" label.
- **Public Verify page** (Verify Page). Turns the re-verification call (the real differentiator vs.
  a forged PDF) into a shareable URL an officer can check. Includes an explicit "for officials"
  note that an unknown reference returns "no reservation found" — which is what makes it credible.
- **Disclosure-forward, not fine print** (both homepages + Booking Flow). "What this is — and what
  it isn't" is a confident two-column panel, and the pay screen leads with "You don't have to pay
  to use this." Framing the caveat as a feature reads as trustworthy, not evasive.
- **Recency-dated reviews component** (both homepages). Structural placeholder only, as requested —
  built to lead with a "reviewed this month" label per card rather than an all-time star count,
  because recency is what moves trust in this category.
- **One-page / low-friction feel.** No progress bar or step counter anywhere; the held-reservation
  screen keeps verify, download, email, disclosure and pay in one view so confirming never feels
  like starting a new wizard.

**Homepage direction — my recommendation: 1a (reassurance-first).** A large share of visitors
arrive stressed and time-pressured; keeping the booking tool as the hero (with the trust strip
built into the search card) gets them moving fastest while still carrying every trust element down
the page. 1b (trust-story-first) is the better choice if paid/marketing traffic — people who don't
yet know what Peregrin is — becomes the dominant channel. Both are complete; pick one, or A/B them.

## Goal 2 — SEO-ready

- **Programmatic landing template** (SEO Template). One layout, genuinely different content per
  page: a country/visa-specific quick-answer box (featured-snippet target), a requirement-detail
  table, a context/comparison section, and a real 4-question FAQ block that doubles as `FAQPage`
  JSON-LD for Google snippets and AI-answer-engine citation. Dataset-driven fields are marked with
  `{{ token }}` (e.g. `stay_window`, `hold_window`, `enforcement_point`) — that token list is
  effectively the schema for the structured dataset Claude Code needs to build.
- **Not thin.** The two examples (Thailand vs. Schengen) share zero body copy — different quick
  answers, tables, comparisons, and FAQs — which is the whole defence against the 2025–26 core-update
  thin-content penalty. The prefilled tool on each page (route/dates matched to the page's context)
  also gives each page a real transactional function, not just content.
- **Homepage ↔ SEO cross-linking.** Both homepages carry an "Onward-ticket & visa guides" grid and
  a footer with Destinations / Visa-types columns, so the two efforts reinforce each other.
- **On-page hygiene** is baked in: single `<h1>` per page, ordered `<h2>`s, breadcrumbs, descriptive
  link text, "updated" date. Image alt text isn't shown because these layouts use no photos — a
  deliberate choice (see below).

---

## Needs your decision before Claude Code implements

1. **Typeface (biggest one).** The mockups use a proposed pairing — **Source Serif 4** for
   headlines + **Public Sans** for body/UI — instead of the current system stack. This is the
   single highest-impact "inviting, real-company-not-a-demo" lever in the whole pass. Both are free
   (SIL Open Font License), so no licensing cost, and both self-host cleanly into the single-file
   app. But it's a genuine identity shift from the all-system look, so it's your call, not mine.
   **If you'd rather stay conservative**, everything here works unchanged on the current system
   stack — the layouts don't depend on the fonts. Recommendation: adopt it; it's the cheapest way
   to close the "reads like a demo" gap.

2. **Homepage direction:** 1a vs. 1b (see recommendation above — I'd ship 1a).

3. **Imagery.** I deliberately used no stock photography — the "hero visual" on homepage 1b is a
   built UI mock of a reservation document, not a photo. If you want photography, that's a content
   decision (and asset sourcing) I'd want your steer on rather than picking stock unilaterally.

## Out of scope / flagged, not changed

Per the brief I made no pricing, product, or positioning changes. The reviews component ships as a
placeholder only. The SEO dataset itself (the actual country/visa requirement data behind the
tokens) is a Claude Code content/build task, not a design deliverable — the template just defines
the shape it needs to fill.
