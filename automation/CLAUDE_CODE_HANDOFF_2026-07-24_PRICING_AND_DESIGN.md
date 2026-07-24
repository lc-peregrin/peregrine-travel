# Claude Code handoff — pricing fix + design + search-UX (2026-07-24, evening)

Paste the prompt below into Claude Code, run from `~/Projects/peregrine-travel/site`. Unlike this
morning's safety pass, this session is meant to change customer-visible behavior and pricing —
that's the point of it — so the guardrail here is "branch + Liam's review before merge," not "no
customer-visible changes."

## Guardrails (read first)

- Work on a new branch, e.g. `claude/pricing-and-design-v1` — never commit or push directly to
  `main`. Open for Liam's review; don't merge or deploy yourself.
- Everything runs against Duffel and Stripe **test mode** (already the case for the whole site) —
  no real money involved regardless of what gets built.
- Priority order matters: TASK 1 (pricing) is business-critical, TASK 3 (search-form UX) is a
  quick, low-risk win, TASK 2 (full design integration) is the largest and can slip to a follow-up
  session if time runs short. Do TASK 1 first regardless.
- If a design/copy decision isn't already resolved in the rationale doc or this prompt, log it in
  `NOTES-FOR-LIAM.md` rather than guessing — same practice as the morning safety pass.

## The prompt

```
INSTRUCTIONS
Read CLAUDE.md in both this directory and the parent directory, STATE.md, docs/BUSINESS_PLAN.md
(especially sections 3 and 9), and automation/CLAUDE_DESIGN_BRIEF_2026-07-24.md before touching
anything. Work only on a new branch (e.g. claude/pricing-and-design-v1) — never commit or push to
main. If time is limited, do TASK 1 fully first, then TASK 3 (quick), then as much of TASK 2 as
you can — in that order.

CONTEXT
Peregrin's real business model (docs/BUSINESS_PLAN.md §1, §3) depends on charging for a held
flight reservation itself — most customers never proceed to a real ticket, so the hold+PDF is the
actual product, not a loss leader. A design review today (Claude Design, working from the real
codebase) surfaced that the live site does NOT currently charge anything for a hold — /api/hold
creates a real Duffel order and the customer gets the document for free; the only Stripe Checkout
path that exists today (/api/order/:id/checkout) fires at the separate "confirm and actually fly"
step, charging the airline fare. This means the site currently cannot earn from its primary
intended product. Separately, a competitor pricing check today updated the target prices: standard
hold $14.99 (was $9.99–12), return/multi-city hold $19.99 (was $14–18) — see docs/BUSINESS_PLAN.md
§3 for the full reasoning (positioned just under onwardticket.com's $16, the category's trust
leader, rather than underpricing it). Separately, Claude Design produced four static layout specs
today (design-exports/, referenced in automation/CLAUDE_DESIGN_BRIEF_2026-07-24.md) covering a
redesigned homepage, booking-flow trust treatments, a public Verify page, and a programmatic SEO
landing-page template — approved for integration with three decisions already made by Liam:
ship homepage direction 1a (reassurance-first, tool stays the hero), adopt the proposed typeface
pairing (Source Serif 4 headlines + Public Sans body, both free/OFL), and no stock photography.

TASK 1 — charge for the hold (do this first, it's the business-critical fix)
Add a real payment step for the standard Reservation Hold and Return/multi-city products, priced
at $14.99 / $19.99 respectively (USD, matching the site's current display currency — confirm
against how offers are currently priced/displayed and flag if a currency mismatch needs resolving
rather than guessing). Reuse the existing Stripe Checkout integration pattern from
/api/order/:id/checkout as the model, but this is a separate charge for the hold/document itself,
not the fare — don't conflate the two payment paths or break the existing confirm-to-fly flow.
Decide, using your read of the existing order lifecycle, whether to gate the Duffel hold creation
itself behind payment or gate only the document reveal/download/email after a hold already exists
— pick whichever fits the existing code shape better and note which you chose and why. Update
translations/UI copy for all 4 languages to reflect that a hold now has a price (reuse the existing
i18n pattern in index.html — don't hardcode English strings). Test end-to-end in Duffel/Stripe test
mode: search → hold → pay → document delivered, confirming the price shown matches $14.99/$19.99
and that the existing confirm-to-fly path still works unchanged afterward.

TASK 2 — integrate the approved design
Using the four Claude Design files and design-exports/RATIONALE.md as the source of truth for
exact layout/copy/structure, integrate into the existing single-file index.html (no framework, no
build step — same constraint the design was built under):
1. Homepage: ship direction 1a from "Peregrin Homepage.dc.html" — keep the booking tool as the
   hero, add the trust-badge row under search, the "how it works" 1-2-3, the public Verify
   callout, and the "what this is/isn't" disclosure box.
2. Typeface: load Source Serif 4 (headlines) + Public Sans (body) — both free/OFL, check
   design-exports/RATIONALE.md for the exact weights/usage specified.
3. Booking flow trust treatments from "Peregrin Booking Flow.dc.html" — all-in price shown early
   on the offer card (this now needs to correctly show the new $14.99/$19.99 price from TASK 1,
   not the old figures), and the held-reservation screen with the verify link + disclosure sitting
   next to the pay button.
4. Public Verify page from "Peregrin Verify Page.dc.html" as a new route — wire it to actually
   query live order status the way the existing verify logic already does (CLAUDE.md's "How a
   booking actually works" step 6), don't just build the static shell.
5. SEO landing-page template from "Peregrin SEO Template.dc.html" — build the templating mechanism
   (the {{ token }} fields map to a small structured dataset per design-exports/RATIONALE.md) and
   ship the two example pages shown (Thailand onward ticket, Schengen tourist visa) as real,
   working pages, including the FAQPage JSON-LD schema. Don't ship placeholder/lorem pages — the
   two examples already have real content in the design file, use it.

TASK 3 — search-form UX gap vs. the category leader
Liam compared the live search form directly against onwardticket.com's order form and found three
concrete gaps, all standard patterns in this category that the current plain inputs lack:
1. From/To airport fields are free-text with no matching — replace with a type-ahead that shows
   matching airports/cities as the user types, each with its country flag and IATA code (this is
   exactly what onwardticket.com's picker does). Duffel already exposes this data — use the Places
   Suggestion API (GET https://api.duffel.com/places/suggestions?query=...), which returns
   airports and cities matched by name or IATA code from Duffel's own dataset, rather than
   building or licensing a separate airport database.
2. Departure/return dates are native `<input type="date">` — replace with an actual calendar
   picker UI (month grid, click a date) matching the style shown in onwardticket.com's date field,
   styled to match Peregrin's palette, not theirs.
3. Passenger count is a bare number — replace with a stepper control (Adults / Children / Infants,
   +/- buttons per category, respecting each type's age bands) instead of a single free-entry
   field, matching the pattern shown. Confirm how Duffel's offer search actually wants passenger
   types passed and wire the stepper's output to match — don't just change the UI without checking
   the API contract still lines up.
None of this needs new visual design exploration — these are standard, well-understood interaction
patterns, not brand/trust decisions, so build directly against the existing palette/typography
rather than looping back to Claude Design.

Also flagged for awareness, not required this session: onwardticket.com has a dedicated support
page (FAQs + contact form, advertised as "24/7 human support, responses within 30 minutes"). A
lightweight FAQ + contact form page is worth adding at some point and is easy — but don't copy the
"24/7, 30-minute response" claim onto Peregrin's version unless Liam actually intends to staff that;
advertising responsiveness that isn't real would undercut trust worse than not having the page,
and runs against the "no over-promising" brand voice already set in docs/BRAND.md. Log this as a
NOTES-FOR-LIAM.md item rather than building the support page this session — it's a genuine gap but
not urgent, and the support-page copy specifically needs Liam's call.

OUTPUT FORMAT
A pushed branch (not main) containing all tasks' changes, clearly separated in commits (TASK 1
commits before TASK 2 before TASK 3, so Liam can review/merge them independently if he wants one
without the others). A written summary covering: how the hold-payment gate works and why you chose
that approach, confirmation the existing confirm-to-fly path still works, what was integrated from
the design files vs. simplified/adapted and why, how the three search-form widgets were built and
confirmation the Duffel offer-search call still works correctly with the new passenger-stepper
input, and the contents of NOTES-FOR-LIAM.md. Do not merge or deploy — leave it ready for review.
```
