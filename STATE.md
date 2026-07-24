# Peregrin Travel — state

Last updated: 2026-07-24, Claude Code — RUN_ALL rev 2: blog shipped (the traffic engine). Privacy
live, ticket conversion behind a flag (OFF), design polish done. Production is on LIVE Duffel keys.

## Done — 2026-07-24, Claude Code: blog images, affiliate structure, homepage tweaks

Three handoffs, pushed to `main`. 101 tests passing.

**Blog images, source links, affiliate (`a5c6c19`, `8b4fe07`).** Cowork's real SafetyWing links landed
first as their own commit. Then the template was wired for images: hero from front-matter on both the
article and the index card, and inline markdown images as lazy responsive figures. A hero is only
reported when the file is actually on disk, so a post without one degrades cleanly.

Four Unsplash heroes at 1600x800, credited in `content/blog/images/CREDITS.md`. **Unsplash now blocks
non-browser clients entirely** (401 to curl, `source.unsplash.com` retired, search API needs a key we
do not have), so these were chosen through the browser, which is also the only way the photographer
names could be obtained. Attribution had to be real, so that mattered. `heroAlt` was rewritten on
three posts to describe the image that actually shipped.

The four Schengen citations with a verified primary source now link to it, opening in a new tab with
noopener/noreferrer; all four URLs return 200. **Norway is deliberately unlinked**: the verified file
records only "norway.no" with no document URL.

Affiliate structure is a reusable recommended box driven by per-post front-matter plus the disclosure
line. A partner whose tracking URL is still "#" renders as plain text, never a live-looking button
that goes nowhere. The disclosure triggers on what a post actually links to, not a flag someone has to
remember to set.

**Homepage tweaks (`beb6860`).** Gold ribbon rewritten to lead with the benefit ("A real reservation,
held in your name") with one plane emoji, disclosure fully intact and now pinned by a test asserting
all three protective claims. Embassy section moved from gold to the soft accent palette. Every country
links to its guide, with the United States and India going to the index since they have no dedicated
guide. Guides promoted to a header pill plus a CTA under the trust band.

### Notes for Liam

- **Booking.com and Airalo tracking URLs are still placeholders.** Supply them and they light up;
  until then those placements render as text, not dead links. SafetyWing is live.
- Two new blog images are large-ish (Philippines 512K). Fine, but worth replacing with your own
  photography when you have it, per CREDITS.md.
- Thailand, Bali and Philippines guides have **no verified official source links**, because
  `EMBASSY_QUOTES_VERIFIED.md` only covers the Schengen area plus USA/India/Canada/UK/Australia. Those
  posts cite rules in prose only. If you want citations there, they need verified sources first.
- Browser screenshot capture stopped working partway through this session, so the final homepage check
  was done by inspecting the live DOM and computed styles rather than by eye. Everything asserted was
  confirmed that way (teal applied, zero gold left in the section, all links resolving), but a quick
  human look at the homepage is worth it.

## Done — 2026-07-24, Claude Code: RUN_ALL tasks 5 and 6 + PDF itinerary + sample doc

Four pieces, each committed separately and pushed to `main`. 90 tests passing.

**Task 5 — embassy authority section (`8b8c2c0`).** The single Norway pull-quote became five
featured cards drawn only from `automation/EMBASSY_QUOTES_VERIFIED.md` (Norway, Germany, Belgium,
Finland, Denmark), plus a compact "also required by" line for the seven tier-2 countries. Source
URLs are kept in a markup comment for legal backup. Spain is excluded (no primary source exists)
and no "don't buy" wording is attributed to India, which only has a real onward-ticket
requirement. The official quotes now ship in English in every language rather than being
translated, since translating a quotation misquotes it, so `embassy_quote`/`embassy_cite` were
removed from the dictionaries and only the surrounding copy is localised. Flags are emoji with
aria-labels, matching the airport picker's existing `flagEmoji()` output.

**Task 6 — blog + voice pass (`de27fa4`, `6b5b2fd`).** Cowork's rewritten Thailand and Bali
guides and the new Schengen-visa guide are published. Then roughly 230 em dashes were removed
from customer-facing copy, each read in context rather than find-and-replaced: the four i18n
dictionaries, the FAQ dataset in all four languages, markup defaults, JS fallback strings, page
titles and meta descriptions, the server-rendered sample and SEO pages, Stripe line-item names
(these show on the checkout page and receipt) and the delivery email. Russian uses a dash as a
copula, so those became a verb rather than being dropped. A test now guards against regression.

**PDF itinerary (`744fa84`).** The document is populated from the Duffel order instead of
placeholders: operating carrier and flight number (with an "Operated by" line for codeshares),
aircraft type, cabin, terminals when supplied, passenger titles and a real issue date. Airline
logos are drawn as vectors via `svg-to-pdfkit`, because **Duffel serves logos as SVG only and
pdfkit embeds PNG/JPEG only** — worth remembering. `airline-logos.js` fetches with a 2.5s timeout,
a size cap and a cache that also remembers misses; every failure falls back to an IATA chip so a
logo can never block a customer's document. A verification QR now sits by the reservation code.

**`/verify` route added (`744fa84`).** The QR needed a destination and none existed. It
deliberately does **not** resolve a booking reference to an order: the reference is printed on a
document anyone might handle, so looking it up would leak passenger data to whoever scans it. The
page explains how to check with the airline instead.

**Sample document (`45e7c62`).** `/sample-reservation` rebuilt on the real layout with an example
Singapore Airlines round trip, a bundled carrier logo, fine print cut to two lines, and a
watermark tiled across the whole document rather than one centred stamp (a single stamp can be
cropped out of a screenshot). Still static, still noindex.

### Notes for Liam

- The handoff's three PDF "corrections" (em dash, spam line, GDS wording) described strings that
  were only ever in the `.dc.html` design reference, not in the shipped `pdf.js`. There was no
  "Check spam" line and no GDS wording to remove. The intent was applied anyway: the fine print
  now splits into two sentences and credits Duffel as our airline booking provider.
- `design-exports/Peregrin Itinerary Document.dc.html` was **not on disk** (standing rule 5
  again), so the PDF was built from the handoff text. Everything specified was implemented; only
  "match the exact layout" could not be checked against the reference.
- `WRITING_STYLE.md` does not exist anywhere in the repo. Task 6 stated its rule inline so this
  did not block, but the file itself is still missing if it is meant to be a reference.
- Two new dependencies: `svg-to-pdfkit` and `qrcode`, both pure JS.
- Left deliberately: lone "—" glyphs that mark a not-yet-populated value in the UI and are
  overwritten by JS. They are placeholders, not prose.

## Done — 2026-07-24, Claude Code: RUN_ALL rev 2 — blog

`CLAUDE_CODE_RUN_ALL.md` was revised to rev 2 while tasks 1–3 were already complete and deployed;
the only new work was **task 4, the blog**. 71 tests passing (was 60).

**Blog is LIVE at `/blog` and `/blog/:slug`**, launched with the two guides already in
`site/content/blog/` (Thailand, Bali). Server-rendered so it's fast and crawlable and doesn't depend
on client JS. **Publishing a new guide is dropping a `.md` into `site/content/blog/` — no code
change, no redeploy logic.** Front-matter: `title, description, slug, keyword, date, lang,
readingTime`.

- New `site/blog.js` module (front-matter parser + renderers), kept out of `server.js` so it's
  unit-testable. Markdown via `marked`; raw HTML is dropped at the renderer, which sanitises without
  a DOM library.
- SEO done properly: per-article title/description, self-referencing canonical, OG + Twitter,
  `Article` + `BreadcrumbList` JSON-LD (datePublished from front-matter, author/publisher Peregrin),
  `Blog` + `ItemList` on the index, one `<h1>` per page, semantic `<article>`.
- Sitemap now carries `/blog`, every article with **its own** front-matter date as lastmod, and
  `/privacy`. **Fixed a bug found while doing it:** the sitemap stamped *today* on every URL, which
  is a poor crawl signal — it now honours a per-url lastmod.
- Design on existing tokens/fonts, no new colours: editorial index cards (destination chip, reading
  time, hover lift); article at ~66ch / 18px / 1.75 with serif headings; the in-article "where
  Peregrin fits" quote styled as a proper accent pull-quote (the handoff explicitly wanted it not
  reading as a code block); end-of-article CTA card, related guides, breadcrumbs, back link.
- **"Guides" added to the header nav and footer** (localised ×4) so the blog is discoverable and
  passes internal link equity both ways with the homepage.

The one h1 detail worth remembering: each article body already opens with its own `#` heading, so
that heading is lifted out and rendered as the page headline while the longer front-matter `title`
stays as the `<title>`/OG string. Two competing h1s would otherwise ship on every article.

### Next for the blog
More guides = more `.md` files (the handoff notes they come from approved Notion guides). The
handoff's nice-to-haves are still open: a destinations filter on the index, and richer related-guide
logic (currently "other guides, newest first").

## Done — 2026-07-24, Claude Code: CLAUDE_CODE_RUN_ALL pass

All three tasks done. 60 tests passing (was 41). Each committed separately and pushed.

**1. Privacy page — LIVE at `/privacy`.** The finalised text arrived in
`automation/PRIVACY_POLICY_FINAL.md`, unblocking the plumbing built the previous pass. Copied to
`site/PRIVACY_POLICY.md` (Vercel only deploys `/site`, so the text must live inside it). No code
change was needed to publish — the route reads the file per request, so adding it flipped `/privacy`
from 404 to 200 and revealed the footer link. Chrome localises en/es/ru/hi; body English as agreed.

**2. Ticket conversion — BUILT, SHIPPED DISABLED.** `ENABLE_TICKET_CONVERSION=false`; every route
404s and the UI is hidden while off. Fee is `max($29, 7% × airfare)`. Live re-price on quote AND
again at checkout, compared against the total the customer actually saw — a moved fare returns the
new quote instead of silently charging a different amount. Issuance happens ONLY from a cleared
Stripe payment, is idempotent against webhook retries (in-flight guard + a re-check of Duffel's own
state), and auto-refunds in full if issuance fails after payment, logging MANUAL INTERVENTION
REQUIRED if the refund itself fails. **Do not enable** until Duffel live hold orders are on, live
issuance is tested end-to-end, and the refund path is exercised in test mode.

**3. Design polish — SHIPPED.** Built from the Task B handoff + live tokens per Liam's instruction,
since `design-exports/Peregrin Conversion Sections.dc.html` is still not present (the RUN_ALL file
said skip; Liam overrode that mid-run). Closed the real gap the handoff flagged: `.section-h` /
`.section-sub` were used in the markup with **no CSS at all**, so two homepage headings were falling
back to browser defaults. Pillars rebuilt as icon-left rows (two-up) so they no longer read as a
repeat of the four-up persona grid; one icon per persona and pillar; sample-reservation modal with
the diagonal SAMPLE watermark, opening from the existing links while keeping the real href as a
no-JS fallback. Reviews section is **component-only**: the source array ships empty and the section
stays hidden, so no quotes or ratings are invented and nothing blank reaches a customer.

### ⚠ Live bug found and fixed mid-run
The single-page nav handler intercepted **every** internal link, but `routeView()` only renders `/`
and `/faq`. So the `/privacy` footer link shipped an hour earlier changed the URL and then silently
left the visitor on the homepage — **the page never loaded**. Same for the sample-reservation links,
and it would have hit `/onward-ticket/*`. Root-caused rather than patched per link: client-rendered
paths are now an explicit allowlist and everything else falls through to a real navigation, so
future server routes are safe by default. This is the third time this class has appeared
(`routeView` re-showing the stays flow, `renderOrder` resurrecting hidden buttons) — **any state set
at load can be undone by a function that re-runs later.**

### Still deliberately not done
Terms & Conditions remains unwired — entity/jurisdiction placeholders + legal review outstanding.
Only the finalised Privacy shipped.

## Done — 2026-07-24, Claude Code: copy/legal pass + accommodation hidden

Seven-step pass, committed step by step. 36 tests passing (was 27).

1. **Accommodation hidden behind `ENABLE_ACCOMMODATION = false`.** It was live while gated on
   unapproved Duffel Stays — advertising a free product we can't deliver. The whole tab bar is
   hidden (one product left), `switchTab("stays")` is a no-op, and a paired flag in `server.js`
   404s all `/api/stays/*` routes so it isn't reachable even if the UI is bypassed. **Trap caught:**
   `routeView()` runs on every navigation and re-showed `#stays-flow` unconditionally — it would
   have silently undone the flag. Also fixed: the `<title>`, meta description, OG/Twitter cards and
   JSON-LD all still advertised "Hotel"/"accommodation" reservations — the most public part of the
   leak, since that's what shows in search results and link previews.
2. **Customer-facing error strings** no longer leak internal terms ("check server logs", "Duffel
   test balance"); both are now inline, localised, and end with "you haven't been charged".
3–5. **Ticket-reservation copy, footer disclaimer, embassy section.** New hero (eyebrow / headline /
   sub / "See a sample reservation"), reworded trust pillars, "Flexible by design", a four-card
   "Perfect for" persona row, the softened checkout disclosure, the always-visible protective
   footer disclaimer, and "Why a reservation — not a ticket" with **exactly one** named pull-quote
   (Royal Norwegian Embassy) plus the unattributed framing paragraph.
6. **Reservation PDF restructured** — "Your trip to {City}", "Airline reservation code: {PNR}
   ({carrier})", status "Booking confirmed", and the four required fine-print lines, keeping the
   verify note.
7. **Static `/sample-reservation`** — watermarked, obviously-example data, no Duffel call, noindex.

**Legal framing enforced throughout:** core noun is always "reservation"; "ticket" only as "ticket
reservation" or "e-ticket issued once you confirm and pay"; no copy claims a ticket was purchased
(all nine "purchased ticket" occurrences are negations). The confirmation screen's paid state was
still reading "Ticketed" — already localised, but non-compliant — so it now reads "E-ticket issued".
All new/changed strings localised across en/es/ru/hi.

### ⚠ Live financial exposure found and closed (was not in the brief)
The confirm-and-pay button — labelled "Simulate payment (test balance, no card)" — calls
`/api/order/:id/confirm`, which tickets the order **out of Peregrin's own Duffel balance with no
customer payment**. Harmless fake money in test mode, but the site is on live keys, so **any
customer holding a reservation could have had Peregrin buy them a real ticket for free.** Closed on
both sides: the button ships hidden and appears only when the server reports test mode (renderOrder
would otherwise have un-hidden it), and the endpoint 404s unless the Duffel key is a test key —
hiding a button doesn't stop a direct POST. Verified 404 on a simulated live key.

### Testing
The PDF renderer was extracted to `site/pdf.js` so its wording is finally testable — the rendered
PDF compresses and font-subsets its text, so it can't be grepped, and this legally sensitive
document had **no coverage at all**. It now renders through a fake pdfkit doc asserting the header,
fine print, verification note, multi-passenger output, and that no wording claims a ticket was
purchased. Other new guards: exactly one `.pull-quote` block and no other named embassy; the
disclaimer present in all four languages; error strings free of internal terms; accommodation
hidden after load.

### Deliberately NOT done
Terms & Privacy pages are **not wired** — the drafts still carry entity/jurisdiction placeholders
and need legal review. The footer disclaimer alone was in scope and is shipped.

## Done — 2026-07-24, Claude Code: live checkout unblocked (blank family_name)

- **Symptom:** every `POST /api/hold` 422'd from Duffel — `validation_error`, "Field 'family_name'
  can't be blank". Live checkout was completely broken.
- **Actual root cause — NOT what the report assumed.** The handoff suspected the design pass broke
  the last-name input's binding. It did not: typing a last name always produced `family_name`
  correctly (verified in the browser). The real cause is that the traveller-details form shipped
  with **empty name inputs whose placeholders were real-looking names** ("Liam"/"Conroy"), so they
  read as already-filled — and **nothing validated them**. A blank last name submitted happily.
  Duffel test mode didn't enforce the field, so it only surfaced on live keys. The "first name fine,
  last name blank" split is the signature of **browser autofill** partially filling a form whose
  inputs had no `name`/`autocomplete` attributes. Fixing "the binding" would have been a no-op.
- **Fixes shipped:**
  - Inputs now carry `name` + `autocomplete` (`given-name`/`family-name`/`bday`/`email`) so browsers
    fill both names rather than one. The name-like placeholders are gone — nothing that is empty
    should look pre-filled.
  - Client-side validation blocks submit on empty first name, last name, date of birth or email,
    with inline per-field errors and focus on the first problem; values are trimmed.
    (DOB/email included deliberately: they are equally required by Duffel, and non-lead passengers
    ship with an empty DOB — the same incident one step later.)
  - Server-side guard rejects blank required fields with a 400 naming them, **before** spending a
    Duffel call, and trims on the way into the payload.
  - The blanket `alert()` ("this search result may have already been used") is replaced by an inline
    error that surfaces the **real** reason; that "expired result" copy is now used only for the
    offer-request error it actually describes. Server text is HTML-escaped on insertion.
  - New i18n keys for every new string across all four languages.
- **Verified** against live Duffel test mode: a hold with "Ada Lovelace" stores the family name on
  the order; blank and whitespace-only last names are rejected with 400; the full UI flow reaches
  the confirmation screen with `family_name` in the payload.
- **Test coverage for the gap that allowed this:** the stub DOM now parses assigned `innerHTML` into
  real elements, so tests drive the *actual generated* traveller form. Three new tests assert the
  payload carries a non-empty `family_name` from the last-name input, that a blank last name is
  blocked with no request sent, and that failures render inline with the real reason. 26 passing.
  Previously **nothing** covered the dynamically-rendered form, which is why this shipped.

- **Same defect found and fixed in the accommodation guest form.** It carried the identical hazard
  (name-like placeholders on empty inputs, no `autocomplete`, no validation) plus a customer-facing
  `alert("Booking failed — check server logs.")`. Gated on Duffel Stays approval so it wasn't
  breaking today, but it would have failed the same way the moment Stays went live. Validation is
  now shared between both forms so they can't drift. Unit-tested only — that flow can't be driven
  end-to-end until Stays access is approved.

### ⚠ Still outstanding: four other `alert()` error paths (NOT fixed — need a decision)
Found while fixing the above; left alone to keep the urgent fix tight. Two matter:
1. **`public/index.html` search failure** — shows customers *"Search failed — check server logs."*
   This is developer language on the **primary path every visitor hits first**.
2. **Confirm-and-pay failure** — shows *"this uses Duffel's test balance, which may need topping up
   in the dashboard."* On **live keys this is factually wrong** and leaks internal operations to a
   customer — the same trust problem as the test-mode badge that was just removed.
   (The other two are stays rate/quote failures.)
   All four should become inline errors using the `describeHoldError()` + `.hold-error` pattern now
   in place. Small follow-up, but it's customer-visible copy so it's Liam's call.

### Lesson worth keeping
Duffel **test mode does not enforce all required fields that live mode does**. Anything validated
only by the supplier can pass in test and fail on live keys. Required-field validation belongs in
our own code (client *and* server), not delegated to the API — see the go-live checklist.
Second lesson: the test harness had **no coverage of dynamically-rendered markup**, which is why a
form-binding-class bug shipped. The stub DOM now parses `innerHTML`, so generated forms are testable.

## Done — 2026-07-24, Claude Code: test-mode badge gated out of production

- **Problem:** `public/index.html` rendered a "Live Duffel test-mode data" badge unconditionally.
  With production now on live keys it was factually wrong and actively trust-damaging on a product
  whose entire pitch is authenticity.
- **Fix — gated at the source rather than deleted**, so it survives as a local dev aid:
  - `server.js` derives `DUFFEL_TEST_MODE` from the key prefix (`duffel_test_`) and returns it as a
    **boolean** on the existing `/api/pricing` response. The key itself never reaches the browser,
    and no extra request was added.
  - `index.html` ships the badge with inline `display:none` and reveals it **only** on an explicit
    `test_mode === true`. Live keys, a missing field, or a failed request all leave it hidden —
    the fail-safe direction. Shipping it hidden also prevents a flash before JS runs.
- **Verified** against the endpoint with a simulated live key (`false`), a test key (`true`) and no
  key at all (`false`). `.env` was neither read nor modified. Locally (still a test key) the badge
  correctly still shows, so the dev aid works.
- **Tests:** new case asserts the badge stays hidden on live keys *and* when the field is absent,
  and appears only in test mode; the stub DOM now honours inline `display`. 23 passing.

### ⚠ Key-mode status needs confirming in STATE going forward
This session was told production is on live Duffel/Stripe keys, and the badge fix assumes that.
Earlier entries in this file still describe the site as test-mode throughout — **those are now
stale**. Worth a single explicit confirmation of the live/test status of Duffel *and* Stripe (and
whether the go-live checklist items — durable hold-fee entitlement store, live webhook registration —
were completed before the switch), then correcting the older entries. The
`GO-LIVE-CHECKLIST.md` items in `site/` should be re-checked against reality rather than assumed.

## Done — 2026-07-24, Claude Code design-evolution pass

Integrated the Claude Design **evolution bundle** (`design-exports/design_handoff/
design_handoff_evolution_pass/`) — the exports finally landed on disk, unblocking the work that was
previously stuck. The `.dc.html` files were used as **visual reference only**; every change was
recreated with the site's existing tokens/classes/i18n, per the bundle's README mapping.

- **Typeface decision resolved and shipped: Source Serif 4 (h1/h2) + Public Sans (body),
  SELF-HOSTED** rather than a Google Fonts `<link>` — no third-party request, which fits the
  trust/independence positioning. 12 woff2 subsets (latin, latin-ext, + cyrillic for the serif),
  412 KB, with an OFL notice in `site/public/fonts/`. Cyrillic body text and all Devanagari fall
  through to the system stack (those subsets don't exist in these families), so the fallback stack
  is deliberately kept. **This closes the "self-host the fonts" item on `GO-LIVE-CHECKLIST.md`.**
- **Trust band** — the single-line footer trust row is now four pillar cards on a soft accent
  gradient, with the "held reservation, not a ticket" disclosure promoted into a prominent gold
  ribbon above the footer. `footer_trust` kept as a screen-reader fallback. New per-pillar i18n
  keys in all four languages.
- **Help nav** — the existing `.header-link` restyled into the accent-tinted pill with a "?" glyph
  (a CSS `::before`, since `applyLang` sets `textContent`). CSS only; the nav JS was not touched.
- **FAQ page** — added the dark "Ready when you are" CTA card back into the tool. Accommodation FAQ
  still deliberately **out** pending Duffel Stays. (The `ticketed_title`/`ticketed_sub` English-only
  gap was already closed in the previous pass — verified localised ×4.)
- **Programmatic SEO template — the one real build.** New server-rendered route
  `GET /onward-ticket/:country` in `server.js`, driven by a per-country dataset, emitting `FAQPage`
  JSON-LD from the same four Q&A pairs it renders, with the legend's title/meta pattern and exact
  H2 sequence. `sitemap.xml` is now **generated** from that dataset (the static file was removed so
  there's a single source of truth).
- **New test**: asserts all four languages define the same i18n key set (catches a key added to
  `en` but forgotten elsewhere). 22 tests passing.

### ⚠ The SEO pages are PLACEHOLDER — real content still needed
Shipped with **one clearly-placeholder example country** (`/onward-ticket/example-country`) whose
every value is still a literal `{{ token }}`. **No real visa/immigration text has been written** —
that dataset is supplied separately, as the brief required. Placeholder entries render
`noindex,nofollow` and are **excluded from the sitemap**, so an unfinished page can never be indexed
as thin content (the 2025–26 core-update risk the design brief calls out). Unseeded countries 404.
**Next step for this: supply the real per-country dataset**, then flip `placeholder: false`.

Out of scope and untouched: payment/hold ordering — the "Duffel hold created before the fee is
paid" economics flag remains a separate decision for Liam.

## Done — 2026-07-24, Claude Code quick-fix pass (Help link + homepage price)

- **Fixed the Help link not reaching the FAQ.** On the live homepage, clicking "Help" (href="/faq")
  left the URL on "/" instead of navigating, so visitors couldn't reach the FAQ. Root cause: the page
  relied on default anchor navigation for the /faq route, which wasn't taking effect from the homepage.
  Fix: added a small client-side nav handler — internal "/" links are intercepted and routed via
  `history.pushState` + the existing `routeView()` (reload-free single-page transition), with
  `popstate` handling back/forward. Anchors keep real hrefs, so they still work with JS off or on a
  direct hit. Verified with a real click: homepage Help → /faq.
- **Added a flat-fee price line to the homepage**, under the hero search, using existing styles:
  "One flat fee — US$14.99 (US$19.99 return). No airfare, no hidden charges." Localised en/es/ru/hi.
  (Previously the price only showed on /faq and in search results.)
- `npm test` green (21). Deployed to production (test mode). No redesign — a full design evolution
  is still coming separately (TASK 2, blocked on the real Claude Design exports).

## Done — 2026-07-24, Claude Code go-live + FAQ session

- **Shipped the revenue + search work to `main` (deployed to production, test mode).** Cherry-picked
  onto `main`, excluding the design commit per Liam's call:
  - **TASK 1 — hold fee is live** ($14.99 standard / $19.99 return, USD). Document (PDF/email) is
    gated behind a Stripe `hold_fee` payment; the confirm-to-fly fare path is unchanged and never
    conflated with it. Verified end-to-end in test mode.
  - **TASK 3 — search-form widgets**: airport/city type-ahead (Duffel Places API), calendar picker,
    Adults/Children/Infants stepper, plus the multi-passenger completion (hold + PDF now handle
    several travellers). Verified against live Duffel test mode.
  - Both deploys confirmed live on `peregrin-demo.vercel.app` (`/api/pricing`, `/faq` return 200).
- **TASK 2 (design) deliberately kept OFF `main`.** It's on-brand *interpretations* only — the four
  real Claude Design export files never landed on disk. Still on branch `claude/pricing-and-design-v1`
  (commits `eca9925` design + `4ce0feb` NOTES), to be reconciled against the real exports later so
  the work isn't done twice. **The regenerated Claude Design exports are the blocker for TASK 2.**
- **Help/FAQ + support page built and deployed** (`/faq`, linked from header + footer). How-it-works,
  three trust pillars, 13 FAQs from the approved `automation/FAQ_AND_TRUST_COPY_2026-07-24.md`, plus a
  sitewide footer trust bar + "held reservation, not a ticket" disclosure. Support contact is
  **hello@peregrin.travel with an honest "we'll get back to you quickly" — no 24/7 / no response-time
  SLA**. Accommodation FAQ intentionally omitted until Duffel Stays is approved. All 4 languages.
- **Closed the English-only "Ticketed" heading gap (`NOTES-FOR-LIAM.md` §3)** — the confirmation
  screen (ticketed heading/sub, countdown states, verify-status box) is now localised across en/es/ru/hi.
- **`site/GO-LIVE-CHECKLIST.md` written** — captures the deferred pre-launch items: live Stripe + Duffel
  keys and webhook, durable hold-fee entitlement store (in-memory today, not durable on serverless),
  self-hosting the fonts, support/refund ops, and the abandoned-hold Duffel cost.
- **Decisions locked in by Liam this pass** (recorded, not re-litigated): keep the hold fee in USD
  (USD-fee / AUD-fare mix is intended); keep hold-created-before-payment; defer durable entitlement
  storage and self-hosting fonts (both in the checklist).
- `npm test` green (21 tests) before every push. Nothing merged from TASK 2; `main` fast-forwarded
  `6c53efb → df80eee`.

## Done — 2026-07-24, Cowork Project-setup + repo-review session

- **Connected the local repo folder to Cowork** (`/Users/liamconroy/Projects/peregrine-travel`) and
  reviewed the live build directly. Confirmed in `site/server.js` that the **business-critical
  hold-fee fix is implemented**: `/api/order/:id/hold-checkout` charges the flat hold fee
  (`HOLD_FEE_STANDARD` $14.99 / `HOLD_FEE_MULTI` $19.99, USD) via Stripe with `purpose: "hold_fee"`,
  and the webhook calls `markHoldFeePaid()` to release the document **without paying the airline** —
  exactly the intended "sell the reservation, not the ticket" model. Search already filters to
  holdable fares. So the revenue mechanism is correct in code; the only gate to real money is
  Duffel go-live + Stripe activation.
- **Set up the "Peregrin Travel" Cowork Project.** Pasted custom **Instructions** (growth/BD partner,
  B2C-first, legal-framed voice, one-decision-at-a-time). Created a concise business-brief scaffold
  at `claude/peregrin-business-brief.md` in the Project. **Loaded 8 canonical docs into the Project
  knowledge/Context** (CLAUDE.md, STATE.md, BUSINESS_PLAN, MARKETING_PLAN, BRAND, BD_PIPELINE,
  REVENUE_AND_EXPANSION_RESEARCH, ROADMAP_PROPOSAL) so every Project chat is grounded. NOTE: the
  Project copies are point-in-time snapshots — this on-disk STATE.md remains the source of truth.
- **Drafted FAQ + trust copy** (`claude/peregrin/FAQ_AND_TRUST_COPY_DRAFT.md` in the Project; also
  delivered to Liam). Draft-and-hold, grounded in the real product + `BRAND.md` voice + `BUSINESS_PLAN.md`
  §5 legal posture (never "fake"; no guarantee of acceptance). **Two decisions flagged for Liam**:
  (a) exact refund/guarantee terms, (b) whether to commit to a specific delivery-time claim. Ready
  for Claude Code to integrate into `site/public/index.html` (all 4 languages; good time to close the
  English-only "Ticketed" heading gap from `NOTES-FOR-LIAM.md` §3) once wording is approved.
- **Automation investigation written** (`claude/peregrin/AUTOMATION_INVESTIGATION.md`; delivered).
  Reprioritised `ROADMAP_PROPOSAL.md` for Liam's B2C/SEO-first call, expanded to ~20 automations
  across ops/growth/lifecycle/BD/back-office, mapped to real connector availability (Drive live;
  Gmail/Calendar/Notion installed-but-not-enabled-in-chat; no native Stripe/GSC/social). Recommended
  a "turn on this week" set of 5 (go-live watch, competitor watch, SEO drafting, community sweep,
  STATE.md discipline) — none created yet, awaiting Liam's greenlight.

**Strategy note (important):** Liam has **explicitly chosen B2C + SEO + high-trust first** for
fastest revenue, with B2B/API partnerships as an alongside/phase-2 track. This **reverses the
B2B-wedge-first ordering** in BUSINESS_PLAN §2 / MARKETING_PLAN §1 — treat Liam's call as current.
Also: **account-level Memory is stale** — still says "B2B wedge first / avoid retail SEO" and "based
in Bangkok"; Liam is moving to Australia soon. Both lines should be updated in Memory.

## Done — 2026-07-24, later session

- **Claude Design produced 4 layout specs** (homepage x2 directions, booking-flow trust
  treatments, public Verify page, programmatic SEO template) — reviewed in full. Strong,
  closely follows `automation/CLAUDE_DESIGN_BRIEF_2026-07-24.md`. Three decisions made: ship
  homepage direction 1a (tool stays hero), adopt Source Serif 4 + Public Sans typeface (free/OFL),
  no stock photography.
- **Found a business-critical gap while reviewing the design**: the live site currently charges
  nothing for a reservation hold — only the separate "confirm and actually fly" step has a Stripe
  charge. Since most customers never convert to a real ticket, this means the site currently
  cannot earn from its primary intended product. Not a design flaw — Claude Design's mockup
  accurately mirrored the real (fee-less) code.
- **Pricing corrected**: standard hold raised from $9.99–12 to **$14.99**, return from $14–18 to
  **$19.99** — positioned just under `onwardticket.com`'s $16 (the category's actual trust
  leader), rather than underpricing it. Full reasoning in `docs/BUSINESS_PLAN.md` §3 and §9,
  including the corrected Stripe international-card cost basis.
- **New Claude Code handoff written**: `automation/CLAUDE_CODE_HANDOFF_2026-07-24_PRICING_AND_DESIGN.md`
  — three tasks: (1) hold-payment fix, business-critical, do first; (2) full Claude Design
  integration (homepage, trust treatments, SEO template, typeface); (3) search-form UX gaps Liam
  found by comparing directly against onwardticket.com's order form — airport/city autocomplete
  with flags (via Duffel's Places Suggestion API, not a new dataset), a real calendar date picker,
  and an Adults/Children/Infants passenger stepper. Also flagged (not built): a support/FAQ page
  is worth adding, but don't copy onwardticket's "24/7, 30-min response" claim without Liam
  actually staffing it. Branch + Liam-review pattern, same as the morning safety pass, but this
  one is explicitly meant to change customer-visible pricing/behavior. Not yet run.

## Done — 2026-07-24, earlier session
Duffel/Wise/Stripe KYC in progress + overnight research handoff).

## Done — 2026-07-24 session

- **Critical production bug fixed and verified live**: `renderOrder()` and two other spots
  referenced an undefined global `lang`, throwing a silent `ReferenceError` that broke the
  confirmation screen for every successful hold in production. Fixed (all three sites now use
  `localStorage.getItem("peregrin_lang") || "en"`), pushed, and verified live by directly
  invoking `renderOrder()` against a real order. Documented as a permanent gotcha in
  `site/CLAUDE.md`.
- **`peregrin.travel` custom domain connected to the live Vercel deployment** — found and
  resolved an orphaned domain attachment on an unused Vercel project via "Move Domain"; both
  `peregrin.travel` and `www.peregrin.travel` now show "Valid Configuration" and resolve to the
  real site.
- **Demo/partner-preview panel hidden from regular visitors** — gated behind `?demo=1` query
  param instead of always-visible, so the live site no longer looks like an internal demo to
  real visitors while still being usable for partner walkthroughs.
- **Baseline SEO shipped**: `robots.txt`, `sitemap.xml`, canonical tag, updated title/meta
  description, OG/Twitter tags, and JSON-LD `Service` structured data, all pointed at the real
  `www.peregrin.travel` domain.
- **Duffel go-live KYC, Wise Business account, and Stripe account activation** — walked through
  live tonight with Liam (screen-share/browser support, no credentials entered by Claude). Wise
  Business account reached completion ("Individual/Sole Trader", correct business details, all
  currencies). Stripe activation reached the "statement descriptor" step. Duffel KYC in progress
  via the Stripe Connect flow. None of these are confirmed fully approved yet — see Blockers.
  ABN (47707480318) verified directly via ABR Lookup during this process.
- **Claude transcript review** — read and summarized a user-uploaded transcript on effective
  Claude prompting technique; validated that Peregrin's existing `CLAUDE.md`/`STATE.md`/
  `docs/BRAND.md` structure already covers the "persistent context files" pattern it recommended;
  adopted the "four-block prompt" (Instructions/Context/Task/Output format) structure for the
  Claude Code handoff below.
- **Claude Code overnight handoff prepared**: `automation/CLAUDE_CODE_HANDOFF_2026-07-24.md` —
  scoped strictly to safety/test-coverage work (test harness for the class of bug above, an audit
  for similar assumed-global bugs, a `site/README.md`, `npm test`), explicitly excluding any
  pricing/feature/UI changes. Not yet run by Liam as of this writing.
- **`docs/reference/REVENUE_AND_EXPANSION_RESEARCH.md` written, then extended same night** — the
  deep-dive Liam asked for: ranked revenue-maximizing feature ideas, UI/interface recommendations,
  a programmatic-SEO plan (concrete mechanism for `MARKETING_PLAN.md` Phase 3), two distinct
  subscription/pricing models (B2C frequent-flier plan vs. B2B agent credit-pack model, with a
  clear recommendation to model the B2B credit-pack first), a ranked shortlist of adjacent
  travel-related low-overhead businesses (flight delay compensation claims ranked highest fit),
  and — per Liam's follow-up to broaden beyond travel — a wider "portfolio of many small
  commission-taking sites" section covering crypto, iGaming, and marketing/SaaS affiliate models,
  with real economics, real risk data (2025–2026 Google core updates specifically hammering thin
  affiliate/iGaming content), a regulatory flag on iGaming affiliate licensing, and a ranked
  recommendation (marketing/SaaS affiliate site first, iGaming last, pending a licensing check).
  Research/recommendations only — nothing in it has been built; next step is Liam picking what
  to scope into real Claude Code sessions.

## Done — earlier sessions

- Live demo product (`/site`) fully working end-to-end in Duffel test mode: search, hold, verify,
  PDF, email delivery, white-label branding toggle, 4-language UI (en/es/ru/hi), Stripe Checkout
  payment (test mode) with webhook auto-ticketing, accommodation-hold flow (blocked on Duffel
  Stays access).
- Visual redesign shipped (blue/teal/gold palette, wing-mark motif).
- Reservation PDF rebuilt to professional standard (was previously broken — font-encoding bug,
  benchmarked against a competitor sample).
- Email delivery domain fixed (was sending from an unverified root domain, now uses verified
  `send.peregrin.travel`).
- Double-submit bug on the hold button fixed (was causing spurious "Hold failed" errors on
  double-click — Duffel only allows one order per offer request).
- `BUSINESS_PLAN.md` and `MARKETING_PLAN.md` written (real, current documents — not drafted from
  scratch tonight, see docs for full content).
- Full project consolidated tonight into this canonical folder structure, migrated from a Cowork
  session's temporary working folder. `/site`'s git history was preserved intact (copied, not
  reinitialised) — same GitHub remote, same commits.
- `BRAND.md` and `BD_PIPELINE.md` written from real data (colour palette pulled directly from
  site CSS, BD pipeline built from the actual tracking spreadsheets, not estimated).
- Confirmed the business's real Google Drive content lives under `lc@peregrin.travel` (not
  Liam's personal Gmail) — `Peregrin_Marketing_Program.md` and `Peregrin_Supplier_Model_Review.md`
  already exist there as Google Docs, matching content confirmed identical to the local versions.

## In progress

- **Duffel go-live KYC (via Stripe Connect)** — in progress, not confirmed complete.
- **Stripe account activation** — reached the "statement descriptor" step, not confirmed complete.
- **Wise Business account** — appears complete (sole trader, business details, currencies set)
  per Liam's own confirmation, not independently re-verified this session.
- **Claude Code overnight safety-pass** — prompt prepared and handed off
  (`automation/CLAUDE_CODE_HANDOFF_2026-07-24.md`), not yet run by Liam.

## Next

From the 2026-07-24 Cowork session (do these first):
- **Confirm Duffel go-live + Stripe activation status** — Stripe KYC passport run in progress; this
  is the one gate to real revenue.
- **Make the two FAQ decisions** (refund/guarantee terms; delivery-time claim), then hand the
  approved FAQ/trust copy to Claude Code to integrate into `site/public/index.html` (all 4 languages).
- **Greenlight the "turn on this week" automation set** + pick a day/time; paste the life-automation
  chat to merge into `AUTOMATION_INVESTIGATION.md`; optionally enable the Gmail connector in-chat for
  lifecycle-email automations.
- **Update account-level Memory**: B2C/SEO-first strategy (not B2B-wedge-first), and Australia (not
  Bangkok).

1. **Run the Claude Code safety-pass session** (`automation/CLAUDE_CODE_HANDOFF_2026-07-24.md`) —
   adds test coverage for the bug class that shipped tonight, branch-only, ready to paste in.
2. **Review `docs/reference/REVENUE_AND_EXPANSION_RESEARCH.md`** and decide which feature(s),
   if any, to scope into a real (separate, reviewed) Claude Code implementation session.
3. **Confirm Duffel go-live, Stripe activation, and Wise account status** — all three were
   mid-flow as of tonight; need a plain status check next session (approved / still pending /
   blocked on something specific).
4. Once site-readiness and KYC are both closed out, resume `docs/BD_PIPELINE.md`'s open decisions
   (Pipeline A fate, Pipeline B outreach copy) per the prior sequencing call — still deferred.

Automation roadmap (`automation/ROADMAP_PROPOSAL.md`) is written and ranked, waiting on Liam's
go-ahead — independent of the BD-hold decision, could be picked up in parallel if useful.

**Site cosmetic-readiness checklist (logo, visual QA, email delivery, favicon/OG) was already
fully closed as of 2026-07-23** — see git history for that detail if needed; tonight's work was
the production bug fix, domain connection, SEO baseline, and KYC/research above.

## Blockers

- Duffel go-live (production API keys) — KYC in progress via Stripe Connect tonight, not yet
  confirmed approved. No fallback supply path if rejected.
- Duffel Stays access — separate request submitted, accommodation flow blocked on it (also why the
  accommodation FAQ is kept out of the live /faq page).
- **TASK 2 design integration** is blocked on the **regenerated Claude Design export files** — the
  four `.dc.html` specs + `RATIONALE.md` never landed on disk, so the design work stays on branch
  `claude/pricing-and-design-v1` as on-brand interpretations until the real exports exist.
- Stripe account activation in progress tonight (reached statement-descriptor step) — not yet
  confirmed fully live/able to take real charges.
- **The site cannot take real payment or issue real tickets yet even though it looks
  production-ready** — Duffel and Stripe are both still test/sandbox-mode pending the KYC above.
  This was an explicit correction to Liam's "ready to make money tonight" framing earlier this
  session: cosmetic polish (now done) and functional business-readiness (still pending) are two
  different things.

## Open decisions

1. **Git structure for this outer folder.** `/site` is already its own independent git repo with
   a live GitHub remote and Vercel deployment — nesting it inside a new outer repo risks breaking
   that if handled carelessly (embedded-repo warnings, or worse, someone force-pushing over it).
   Planned approach: give the *outer* `peregrine-travel` folder its own separate git repo/GitHub
   repo for docs+automation+specs, with `/site` explicitly excluded (`.gitignore`'d) from the
   outer repo so its own git history and Vercel linkage stay untouched. Flagging this before
   executing since it's the one step in tonight's plan with real risk to a live deployment if done
   wrong.
2. **BD Pipeline A fate** — re-approach the 12 cleanly-contacted agencies with a reframed
   (reseller, not supplier) pitch, or let it lapse and start Pipeline B fresh? See
   `docs/BD_PIPELINE.md`. **Deferred** — Liam wants the site-readiness checklist above closed out
   before touching either BD pipeline.
3. **Google Drive / email identity** — confirmed business content is correctly under
   `lc@peregrin.travel`; worth deciding if Liam wants his default browser profile / Drive account
   on this Mac switched to that identity permanently, or just used situationally.
4. **Claude Code setup** — discussed, not yet installed. Liam chose to set up the Cowork Project
   route first tonight instead.
