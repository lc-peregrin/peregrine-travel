# Claude Design brief — make Peregrin more inviting + SEO-ready

Paste this into Claude Design. Exports land in `/design-exports`; Claude Code integrates them into
`/site` — same handoff pattern already used for the logo/favicon/OG asset set.

## What Peregrin is

Peregrin sells travellers genuine, verifiable flight and accommodation reservations — real PNRs
held directly with an airline via Duffel's Hold Order API — to satisfy "proof of onward/return
travel" requirements at check-in, immigration, or in a visa application. Most customers never
intend to fly; the hold lapses automatically if unpaid. The whole pitch rests on one anxiety: "will
this actually work at the border/embassy/check-in desk" — the site's entire job is to resolve that
anxiety fast, for someone who is often stressed and time-pressured when they land on it.

## Where the site is today

Live at `peregrin.travel`. Functionally complete (search → hold → verify → PDF → email → payment),
already redesigned once this year into a warm-but-structured visual identity (blue/teal/gold,
generous whitespace, a wing-mark motif) — it works and looks reasonably clean, but it still reads
more like a working demo than a site someone would trust with a real purchase decision under
stress. That gap — functional and tidy, not yet genuinely *inviting* or maximally trust-building —
is what this brief is asking Claude Design to close. This is a refinement/extension pass on the
existing identity, not a from-scratch rebrand.

## Brand constraints — already decided, treat as fixed inputs

Full detail in `docs/BRAND.md`; the essentials:

- **Voice**: "warm but structured" — plain, direct language, no travel-marketing fluff, not cold
  or corporate either.
- **Colour palette** (source of truth is the live CSS, not a separate design file):
  `--ink #16283a`, `--muted #5c6b7c`, `--line #e2e7ec`, `--bg #f8f9fb`, `--accent #1c6f8c`,
  `--accent-bg #e8f2f5`, `--accent-dark #124a5e`, `--gold #c9922e`, `--gold-bg #faf1e0`,
  `--success #1f7a5c`, `--success-bg #e7f4ee`.
- **Typography**: system font stack currently (no licensed webfont) — open to a real typeface
  recommendation as part of this pass if it meaningfully helps the "inviting" goal, but flag it as
  a distinct proposal rather than assuming it's already approved.
- **Logo**: a real asset set already exists (wing-mark, transparent/solid variants, wordmark
  lockup, OG image) in `/design-exports` and wired into the live site. Reuse it — this pass is
  about layout, content structure, and trust-building UI, not a new mark.

## Goal 1 — more inviting and more trustworthy

This is a conversion/trust problem more than a decorating problem. Grounded in research already
done this project (`docs/reference/REVENUE_AND_EXPANSION_RESEARCH.md` §2 — UI/interface
recommendations, worth reading before starting), the specific, concrete things to design for:

- **Trust signals belong at the point of decision, not just the homepage hero.** Specifically:
  next to the payment button, and next to the "hold" confirmation — not only in top-of-page
  marketing copy. Design a treatment for "this is a real, independently verifiable reservation"
  that sits directly beside the CTA, plus a design for a public "Verify" link/badge (checkable
  against the airline, not just Peregrin's word).
- **All-in pricing, visible early.** No surprise costs revealed only at final payment — design the
  offer-selection screen so the full price is obvious before the customer commits to entering
  details.
- **One-page, low-friction checkout feel.** Minimize the sense of "steps" and progress-bar
  anxiety between selecting a fare and completing payment, even though the current flow already
  isn't a long wizard — this is about how it *feels*, not just how many actual pages exist.
- **Recency-dated social proof placeholder.** Design a component for reviews/testimonials once
  they exist, emphasizing *recency* ("reviewed this month") over raw star-count, since that's what
  actually moves trust in this category. Doesn't need real reviews yet — just the component and
  where it lives.
- **Disclosure-forward messaging, not fine print.** "A real held reservation, not a ticket, unless
  confirmed" needs a design treatment that reads as transparent and confident, not as a legal
  caveat buried at the bottom.

## Goal 1b — pricing/fee display (added 2026-07-24)

A specific, researched addition to the trust/inviting goal above, since fee display is one of the
highest-leverage trust signals in this category and has real compliance implications, not just a
visual preference. Full reasoning and sourcing in `docs/BUSINESS_PLAN.md` §9 — the short version
Claude Design needs:

- **One flat, all-in price. No itemised fee breakdown, no "service fee" or "processing fee" line
  added at checkout.** This matches exactly how the direct competitors in this category
  (onwardticket.com, dummyticket24.com) already price — a single number, no drip pricing — and
  it's also the only pattern that's cleanly compliant everywhere Peregrin actually sells (the EU
  bans card surcharging outright; Australia — Peregrin's own home jurisdiction — bans it from
  1 October 2026). Don't design a "price breakdown" or "fees" expandable section; design for one
  clear number instead.
- **Show that one price early and keep it visible**, not just at final checkout — before the
  customer enters passenger details, ideally on the offer-selection screen itself. This is the
  single biggest documented abandonment fix in the checkout-UX research already done (§2 of the
  revenue research doc).
- **The "not a ticket, a real held reservation" disclosure sits close to the price**, not in
  separate fine print — same trust-forward treatment as the rest of goal 1, applied specifically
  to the moment the price is shown.

## Goal 2 — SEO-packed

Flag up front: most of the actual SEO work is a content/technical build (a templated system that
generates country × visa-type × language landing pages at scale, per `REVENUE_AND_EXPANSION_
RESEARCH.md` §3) — that part is Claude Code's job, not a design deliverable. What Claude Design
*can* contribute, and what this brief is actually asking for:

- **A reusable landing-page template design** for those programmatic pages (e.g.
  `/onward-ticket/thailand`, `/schengen-tourist-visa`) — needs to hold country-specific
  requirement text, a genuine FAQ block (not filler — this doubles as FAQ-schema content for
  Google/AI-answer-engine citation), and a comparison/context section, without looking like a
  thin templated page stamped out 200 times. This is the single highest-leverage design
  deliverable in this brief — it's the difference between the SEO plan working and it reading as
  spam to Google's 2025–2026 core-update quality signals.
- **A homepage structure that supports both goals at once** — the trust-building elements above,
  plus clear internal linking/navigation into the country/visa-type page set once it exists, so
  the two efforts reinforce each other rather than living in separate parts of the site.
- Standard on-page SEO hygiene (clear heading hierarchy, descriptive alt text placeholders,
  scannable structure) baked into whatever layouts are proposed — not a separate deliverable, just
  a constraint to design within.

## Technical constraints — read before designing

`/site` is a genuinely single-file, no-build-step, no-framework app: `public/index.html` contains
all HTML/CSS/JS inline, and `server.js` is a plain Express backend. Practical implications:

- Deliverables should be static exports (SVG/PNG mockups, or a clearly-specified component/layout
  spec) that Claude Code can translate into the existing inline CSS — not React components, not
  anything assuming a build pipeline or component framework.
- Don't propose a rebuild onto a framework as part of this brief; if there's a strong case for one
  later, that's a separate, bigger conversation with Liam, not something to fold into this pass.
- New pages (the programmatic template) need to work as plain server-rendered or static HTML
  Express can serve — keep that in mind for anything interactive proposed in the design.

## What's out of scope for this pass

No pricing changes, no new product features, no copy/positioning changes beyond what's needed to
implement the trust-signal and disclosure treatments above — those are already decided in
`docs/BUSINESS_PLAN.md` and `docs/MARKETING_PLAN.md`. If something in this brief seems to require
a positioning change to work, flag it rather than deciding it unilaterally — same rule Claude Code
worked under for its own overnight pass.

## Output format

Exports to `/design-exports`, plus a short written rationale covering: what changed and why for
each of the two goals above, and anything flagged as needing Liam's decision before Claude Code
can implement it (e.g. the typeface question). Claude Code integrates from there — don't touch
`/site` directly.
