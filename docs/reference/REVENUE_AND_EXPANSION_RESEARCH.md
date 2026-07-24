# Peregrin — Revenue, UI, and Expansion Research

Prepared for Liam Conroy, 2026-07-24 (overnight Cowork research session, extended same night with
a wider pass). Companion to `BUSINESS_PLAN.md` and `MARKETING_PLAN.md` — this doc doesn't repeat
their positioning/channel strategy, it adds: revenue-maximizing feature ideas, UI/interface
recommendations, a deeper SEO plan, subscription/pricing models for two different Peregrin
subscriber types, a ranked shortlist of adjacent travel-related low-overhead businesses, and a
wider look at a "many small commission-taking sites" portfolio strategy across other industries
(crypto, iGaming, marketing/SaaS affiliate).

**Status: research and recommendations only.** Nothing here has been built. Anything worth
shipping needs a separate, scoped Claude Code session per file — same discipline as tonight's
safety-pass handoff (`automation/CLAUDE_CODE_HANDOFF_2026-07-24.md`).

## 1. Revenue-maximizing features, ranked

**1. Validity-period / multi-use upsell.** The single highest-leverage, lowest-effort addition:
offer a second hold on the same route (or a longer hold window where the fare allows it) as a
one-click upsell at checkout, for travellers whose plans might shift before their embassy
appointment or border crossing. Zero new infrastructure — it's the existing hold flow called
twice with a small price bump. Direct precedent: onwardticket.com-style competitors already
upsell "date change flexibility" as a paid add-on.

**2. Return / multi-city as a default option, not an afterthought.** Already priced in
`BUSINESS_PLAN.md` ($14–18) but currently secondary to the one-way flow. Many onward-travel
requirements specifically ask for a *return* ticket, not just an outbound one — surfacing this
as the default or a prominent toggle (not a separate flow) likely lifts average order value
without new backend work, since the hold mechanic is identical.

**3. Bundled affiliate add-ons at the confirmation step.** Insurance (SafetyWing/World Nomads,
~10% commission) and eSIM (Airalo-style, 8–10%, some programs to 20–30%) are near-zero-effort
revenue once those affiliate accounts are live (per `MARKETING_PLAN.md` Phase 2) — the only build
work is a "Recommended for your trip" module on the confirmation/PDF-download screen. This is
genuinely close to pure margin: no fulfillment obligation on Peregrin's side at all.

**4. B2B self-serve API tier, not just human-sales white-label.** `BUSINESS_PLAN.md` already
scopes the `/reservations` endpoint as "not yet started" — worth flagging that a self-serve
version (API key signup, usage-based billing via Stripe metered billing) could run in parallel
with the human-sales B2B motion already underway, capturing smaller agencies/apps that won't
justify a sales conversation but would self-serve if the docs and pricing page existed. Lower
priority than closing the sales-led deals already in motion, but cheap to scope once the core
API exists.

**5. Group/bulk booking flow.** Visa consultancies and relocation firms processing multiple
travellers at once (a family, a group tour) are an explicit `MARKETING_PLAN.md` target segment;
a CSV-upload or multi-passenger-in-one-session flow removes real friction for exactly that
segment without being a new product.

**6. "Verify" as a public, shareable trust page.** The re-verification call against Duffel
(`BUSINESS_PLAN.md` §4 step 7) is the core differentiator versus a forged PDF — making that a
public URL (`peregrin.travel/verify/{ref}`) that immigration officers or airline staff can check
directly, rather than only an authenticated in-app action, turns the trust mechanic into a
marketing asset and a checkout-page trust signal simultaneously (see UI section below).

## 2. UI / interface recommendations

The category (high-anxiety, one-time, "will this actually work" purchase) has well-documented
UX patterns worth matching rather than reinventing:

- **Trust signals belong at the payment field, not just the homepage.** Sites that place
  security/verification signals immediately adjacent to the payment form see materially higher
  payment-completion rates than sites that only show trust badges elsewhere on the page. For
  Peregrin specifically, that means the "real, independently verifiable PNR" message and a link
  to the public verify page (see feature #6 above) should sit directly beside the Stripe payment
  button, not just in the hero copy.
- **All-in pricing, no surprise fees at payment.** Unexpected charges at the final step are one
  of the single biggest abandonment triggers in travel bookings generally; Peregrin's flat,
  simple pricing is already well-suited to this, so the UI job is just making sure the full price
  is visible from the offer-selection screen, not revealed only at Stripe checkout.
- **One-page, single-scroll checkout.** Consolidating search → offer selection → passenger
  details → payment into as few discrete "steps" as possible (already close to true in the
  current flow) measurably reduces abandonment versus a multi-page wizard with a visible progress
  bar and back-button risk.
- **Recency-dated social proof.** Review-based trust matters most when it's recent — most
  travel-category buyers specifically look for reviews from the last few months, not
  all-time review counts. Worth keeping in mind once Peregrin has any live reviews to display:
  a small "reviewed this month" indicator will outperform a large but stale total count.
- **Explicit "what this is / isn't" framing stays close to the CTA.** Already the compliance
  posture (`BUSINESS_PLAN.md` §5); it also does double duty as a trust-building UI element for
  the same reason disclosure-forward messaging is a documented conversion driver elsewhere in
  the anxiety-purchase category — hiding the caveat in fine print reads as evasive.

## 3. SEO — programmatic pages, made specific

`MARKETING_PLAN.md` Phase 3 already commits to skipping head terms and going long-tail/
non-English. The mechanism to actually do that at scale without linear content-writing effort is
**programmatic SEO**: one page template, driven by a structured dataset, generating a page per
country × visa-type × language combination (e.g. `/onward-ticket/thailand`,
`/prueba-de-viaje-thailand`, `/onward-ticket/schengen-tourist-visa`).

Concretely, that means building a small structured dataset (country, visa type, typical embassy/
border requirement wording, relevant airline hold windows) once, then a single Next.js-style
template that pulls from it — not hundreds of hand-written blog posts. The thin-content risk
(the classic failure mode of programmatic SEO) is avoided by including genuinely different content
per page: country-specific requirement wording, an FAQ block built from real questions in that
market (Reddit/VisaJourney threads per `MARKETING_PLAN.md` Phase 4 are also a direct source of
real FAQ questions to seed this), and a comparison/context section — not just the template with
the country name swapped in.

FAQ schema markup (structured JSON-LD `FAQPage` data) is worth adding to these pages specifically
because it targets featured-snippet placement in Google and is increasingly what AI answer
engines cite directly when responding to "do I need an onward ticket for X" — relevant given
`BUSINESS_PLAN.md`'s risk #6 already flags AI-search interception of these exact queries as a
runway-shortening risk; a well-structured FAQ page is the most direct mitigation available.

Sequencing: this is a genuinely large build (dataset + template + initial page set), so it's a
Claude Code implementation task once scoped, not something to start speculatively tonight — but
worth prioritizing early per Phase 3's own logic (longest lead time of any channel in the plan).

## 4. Subscription / pricing models — two distinct subscriber types

Peregrin actually has two different subscription opportunities, not one, because it has two
different repeat-buyer types: the individual frequent traveller, and the visa agent/agency
buying on behalf of many clients. Worth pricing and building separately rather than one generic
"Peregrin Plus" tier.

**A. B2C frequent-flier plan.** Direct precedent exists in the exact adjacent category: iVisa
Plus sells an annual subscription for unlimited standard-speed visa/eVisa processing — validating
that recurring-revenue pricing works for this kind of anxiety-purchase, repeat-need travel
document. NomadLife's $29/month membership (bundling visa assistance, residency guidance, and
travel logistics for digital nomads) is a second, broader precedent for the same buyer bundling
several travel-adjacent needs into one subscription. SafetyWing's insurance already bills in
4-week cycles to this exact audience, so a Peregrin subscriber is very likely already paying at
least one other travel-adjacent monthly fee — a familiar buying pattern, not a new one.

The natural subscriber is the same person `MARKETING_PLAN.md` already identifies as the core B2C
segment — a digital nomad or long-term traveller doing repeated visa runs (Thailand/Bali/
Philippines corridor), needing a fresh onward-ticket proof every 30–90 days. Illustrative
structure (needs real modeling before pricing, see caveat below): a monthly or annual plan priced
at roughly 3–4x a single standard hold, covering unlimited standard holds for the period. Premium/
ticketed reservations stay pay-per-use regardless of plan, since that tier carries real cash float
risk (`BUSINESS_PLAN.md` §3) and shouldn't be bundled into a flat subscription.

**B. B2B agent/agency plan.** This is the stronger near-term fit, because `MARKETING_PLAN.md`'s
B2B wedge is already the primary go-to-market motion, and visa-consultancy software already
prices per-seat subscriptions in this exact buyer category — legacy immigration CRM tools run
$150–500+/user/month, newer entrants like Visas.AI price $100–130/seat/month. That's a useful
anchor: an agency already budgets for recurring software spend in this range, so a Peregrin
agent subscription isn't introducing a new kind of cost to their business, just a new line item
inside a familiar one. Two structures worth modeling against each other rather than picking blind:

- **Tiered monthly subscription with included volume** — a "Starter"/"Agency"/"Enterprise"
  ladder (the pattern travel-API vendors already use: e.g., a flat monthly fee including a set
  number of holds, then metered overage past that). Predictable revenue, budget-friendly for the
  partner, but requires forecasting each partner's volume reasonably accurately to price tiers
  right.
- **Prepaid credit-pack model** — the partner buys a block of hold-credits (e.g., 30/60/120)
  that draw down as used, with a defined expiry window (30–90 days is the common range) rather
  than unlimited rollover, which the credit-pricing research flags as the standard way vendors
  avoid the revenue-recognition and use-it-or-lose-it problems of open-ended rollover. This
  probably fits visa agents better than a flat subscription, since their volume is genuinely
  bursty (client surges before consulate deadlines, not a steady monthly rate) — a fixed monthly
  seat fee risks either overcharging quiet months or under-provisioning busy ones.

**Recommendation:** model the B2B credit-pack structure first — it's the better fit for the
buyer's actual usage pattern, and it slots directly into the B2B wedge already underway rather
than requiring new acquisition. Treat the B2C frequent-flier plan as the second priority, since
it needs real repeat-purchase-frequency data (how often does an actual Peregrin customer rebook)
before a price point is safe to set — that data doesn't exist yet with a live product, so it's
better sequenced after some real order history exists, not before.

**Why this stays a recommendation, not a spec:** subscription pricing is a decision with real
downside if wrong — undercutting per-unit margin at volume, or pricing out the exact
price-sensitive segment that's the target buyer. This section gives structures and anchors, not
final numbers; a numbers pass against actual repeat-purchase and partner-volume data is the
required next step before either plan launches.

## 5. Adjacent low-overhead, fast-to-build businesses — ranked

All four below share Peregrin's actual edge: API-first thinking, Duffel/travel-data familiarity,
and a demonstrated ability to ship a working transactional flow fast. Ranked by fit + speed to
first dollar, not by ultimate size.

**1. Flight delay compensation claims (strongest fit).** The AirHelp/Skycop/ClaimFlights model:
customer forwards a confirmation email or flight number, the service checks EU261/equivalent
eligibility, files the claim, and takes a no-win-no-fee cut (typically ~25% of the payout). The
whole intake-and-eligibility-check step is automatable via email-parsing APIs extracting
structured itinerary data — a near-exact reuse of the flight-data handling Peregrin already has
via Duffel. Genuinely minimal outgoings (no inventory, no float, pure service fee), and it's a
plausible cross-sell to Peregrin's existing traveller base rather than a cold-start audience.
Honest caveat: the actual claims-filing/legal-follow-up side (disputing airline rejections) is
the hard, non-automatable part competitors have built real operational teams around — worth
scoping as "eligibility checker + lead-gen to a claims partner" before committing to running
full in-house claims processing.

**2. Hotel/accommodation dummy booking confirmations.** Already partially built (`BUSINESS_PLAN.md`
§6 — blocked only on Duffel Stays access approval, not on new engineering). Once that access
lands, this is close to zero incremental build cost and directly extends the existing product
rather than being a separate business — arguably this should be thought of as feature #7 above,
not a separate venture, given how little new work it requires.

**3. Visa document / cover-letter generator.** Competitors like traveldocgenerator.com and
biberks.com run a template-driven generator (itinerary summaries, invitation letters,
cover letters) as a lightweight, high-margin digital product. Reuses Peregrin's existing
itinerary-PDF generation (`pdfkit`) infrastructure almost directly. Lower revenue ceiling than
the claims idea, but very fast to build and a natural upsell at the point Peregrin already has
a customer's itinerary data in hand.

**4. B2B/white-label visa API reseller.** VisaHQ/SimpleVisa-style revenue-share partnerships
with visa processing services, offered as a bundled add-on to Peregrin's existing B2B pitch
(`MARKETING_PLAN.md` already lists visa consultancies as both a customer segment and a natural
two-way referral partner). This isn't really a new business so much as a deeper version of the
B2B wedge already underway — ranked last because it's more of an extension of Phase 1 than a
distinct venture, but flagged since it was part of the original ask.

**Not recommended to pursue as separate ventures right now:** anything requiring its own float
or inventory (matches the same capital-intensive pattern `BUSINESS_PLAN.md` already flags as a
weakness of the premium ticketed tier), and anything outside travel/documents where none of
Peregrin's existing infrastructure, trust position, or audience transfers.

## 6. Beyond travel — a portfolio of minimal-overhead commission sites

Liam's ask here was specifically wider than travel: is "build a nice simple site that resells or
takes commission on an existing API/affiliate program, and run many of them" a real strategy, and
if so, in which industries. Short answer: yes, it's a real and well-documented model — but it has
two failure modes that show up repeatedly in the research, worth understanding before picking
verticals, not after.

**The model is real and has real precedent, across several industries:**

- **AI API arbitrage** (buying model API access at developer rates, reselling it packaged as a
  vertical tool) is currently producing solo-operator revenue in the $3–15K/month range, with
  some reaching $50K+/month by going deeper into one vertical rather than staying generic. The
  "boring niche + clean execution" pattern shows up repeatedly — one cited indie-hacker portfolio
  reached $28K/month across several small SaaS products by reusing the same tech stack for each
  new site rather than building each from scratch.
- **iGaming affiliate sites** run on well-established commission structures — CPA (fixed $50–400+
  per depositing player) or revenue share (20–45% of a referred player's net gaming revenue,
  recurring for as long as they're active) — and comparison/review sites in this space are a
  mature, proven category, not a novel idea.
- **Crypto affiliate/referral sites** are similarly mature: exchanges like Binance (up to 50% of
  referred trading fees) and ChangeNOW (lifetime revenue share on every completed exchange, not
  just signup) pay real, ongoing commissions for simple referral traffic — no license or capital
  required to participate as an affiliate, just traffic.
- **Marketing/SaaS tool affiliate sites** (email marketing, automation, CRM tools) pay recurring
  commissions of 40–60%+ for the first 12 months per referred customer — a lower-drama, lower-risk
  category than iGaming or crypto, though also lower ceiling per site.
- **VIN lookup / package tracking / document-generator style utility sites** are the closest
  cousin to Peregrin itself: pay-per-lookup or prepaid-credit reseller access to an underlying
  data API (VinAudit, API Ninjas, Vincario for VIN data, as one concrete example), marked up and
  resold through a simple branded front end — structurally identical to what Peregrin already
  does with Duffel.

**The two failure modes worth taking seriously before scaling to "100 sites":**

1. **No moat, because anyone can call the same API.** The core critique of thin-wrapper
   businesses generally: if a site is genuinely just a rebranded API call, the underlying
   provider (or any other reseller) can replicate it in a weekend, and there's a real documented
   case of a wrapper product losing 70% of revenue in 60 days once the underlying provider shipped
   the same feature natively. The sites that survive this long-term add something the raw API
   doesn't have — Peregrin's own answer to this is the "independently verifiable real PNR" trust
   layer (section 1, feature #6) rather than just cheaper/faster search; any new site in a
   portfolio needs its own version of that same question answered before it's built, not after.
2. **Content-driven sites in this genre are exactly what recent Google core updates have been
   targeting.** The 2025–2026 core updates specifically hit thin affiliate and comparison content
   hard — 96% of betting-affiliate sites saw significant traffic declines after the March 2025
   update, and AI Overviews are increasingly answering the exact "which is best" queries these
   sites used to rank for, before the user ever clicks through. The portfolio-diversification
   logic (one Google update can't sink 100 independent sites the way it sinks one) is real and is
   the standard argument for running many small sites instead of one big one — but it doesn't
   protect any individual site from this pressure, it only limits how much of the whole portfolio
   goes down at once. Sites with genuine first-hand data, a real product (not just a review page),
   or a direct transactional function (like Peregrin) are faring measurably better than
   pure content/comparison plays.

**Regulatory note, since this spans jurisdictions Peregrin hasn't operated in before:** pure
referral/affiliate participation (sending traffic, earning a commission) in iGaming and crypto is
generally lower-friction than operating the underlying casino or exchange — but several
jurisdictions (the UK and Malta notably) do require affiliate-specific licensing or registration
for gambling affiliates specifically, which a generic "just build the site" approach would miss.
This is genuinely worth a specific compliance check before committing real effort to an iGaming
site, not something to assume is fine because it's "just an affiliate link" — flagging it here
rather than treating it as settled, consistent with how `BUSINESS_PLAN.md` §5 already treats
Peregrin's own legal/compliance posture as something requiring real verification, not assumption.

**Ranked recommendation, given Peregrin's actual current position (small team, one live product,
real API-integration and trust-layer experience, no existing licensing infrastructure):**

1. **Marketing/SaaS-tool affiliate site(s)** — lowest regulatory friction, direct skill transfer
   from Peregrin's own positioning-and-content work, and the least likely to get flattened by a
   Google core update if built with genuine comparative depth rather than thin listicles. Good
   first test of the "second site" model with the least new risk surface.
2. **A second Duffel-adjacent utility site** (flight delay compensation checker, from section 5,
   or a VIN-lookup-style data-reseller site in a completely different vertical) — reuses actual
   technical infrastructure/skills already built, same "real transactional function beats content
   page" defensibility Peregrin itself relies on.
3. **Crypto affiliate site** — mature, well-paying commission structures and comparatively low
   regulatory friction as a pure affiliate (not an exchange operator), but more competitive and
   more volatile (commission terms and regulatory attention both shift quickly in crypto) than
   the two above — worth treating as a "third site once the model is proven" pick, not a starting
   point.
4. **iGaming affiliate site** — real money in this category, but ranked last given the licensing
   question above and the fact that it was the hardest-hit category in the exact Google
   core-update data just cited — the highest-effort, highest-compliance-overhead option of the
   four, best revisited once there's spare capacity to do the licensing check properly rather than
   as a first move.

The "100 sites" framing is directionally right as a risk-management strategy (diversify away from
any single Google update, any single API provider, any single niche going stale) — but the
research doesn't support treating any of these as truly passive. Each site still needs its own
defensibility answer and, per the regulatory note, its own compliance check in higher-friction
verticals. Realistic sequencing is one or two proven sites first, not launching many at once.

## 7. Suggested next steps

1. Liam reviews this doc and picks which of section 1's features (if any) to scope for a real
   Claude Code implementation session — each should get its own four-block prompt, same pattern
   as tonight's safety-pass handoff, not a single "build everything" instruction.
2. Programmatic SEO (section 3) is the one item worth starting soon regardless of what else gets
   picked, given its long lead time — but it's a real build (dataset + template), not a quick
   add, so it needs its own scoped session too.
3. Subscription pricing (section 4): model the B2B credit-pack structure first against real
   partner-volume assumptions — it's the better near-term fit. Hold the B2C plan until there's
   real repeat-purchase data from live orders.
4. Of the adjacent travel businesses (section 5), flight delay compensation is the strongest fit
   if Liam wants a second venture in the same space — validate the eligibility-checker/lead-gen
   angle first, not a full in-house claims operation.
5. Of the beyond-travel portfolio ideas (section 6), start with one marketing/SaaS-tool affiliate
   site as the lowest-risk test of the "many small sites" model, and treat iGaming as the last
   vertical to pick up, pending an actual licensing check in the relevant jurisdictions.

Sources:
- [Trust Signals for Travel: 2026 Social Proof & Conversion Guide](https://atlasperk.com/guides/website-conversion-for-travel/trust-signals/)
- [eCommerce Checkout Optimization: UX Guide 2026](https://www.digitalapplied.com/blog/ecommerce-checkout-optimization-2026-ux-guide)
- [Top 15 UX Tips to Improve Conversion Rates in Travel Booking](https://ulansoftware.com/blog/ux-tips-improve-travel-booking-conversion)
- [Ecommerce Checkout UX Guide - Baymard](https://baymard.com/learn/checkout-flow-ux-optimization)
- [Programmatic SEO for Travel: Scaling Destination Pages Without Going Thin (2026)](https://firestorm-internet.com/insights/programmatic-seo-travel/)
- [Programmatic SEO: 24 Templates That Generated 50K+ Leads](https://founderpath.com/blog/programmatic-seo)
- [FAQ Schema for Travel: How to Win Featured Snippets?](https://petersawicki.com/blog/content-strategy/faq-schema-travel-websites/)
- [Best Practices for API Monetization in Travel and Hospitality](https://zuplo.com/learning-center/api-monetization-in-travel-and-hospitality)
- [Travel API Pricing Models: Per-Request vs Revenue Share](https://tripgic.com/playbook/travel-api-pricing-models/)
- [Guide to credit pricing in SaaS — m3ter](https://www.m3ter.com/guides/saas-credit-pricing)
- [Credit-Based vs Subscription Pricing for B2B Data APIs](https://www.explorium.ai/building-ai-agents/credit-based-vs-subscription-pricing-for-b2b-data-apis/)
- [Visas.AI: Pricing, Free Demo & Features](https://softwarefinder.com/legal/visas-ai)
- [Visa Immigration CRM Software: Complete Guide 2026](https://smartxcrm.com/visa-immigration-crm-software-complete-guide-to-transform-your-immigration-consultancy-in-2026/)
- [Nomadlife Basic Membership Plan](https://nomadlife.webflow.io/pricing/basic-membership-plan)
- [Best Travel Insurance for Digital Nomads 2026](https://nomadsbeyond.com/best-travel-insurance-for-digital-nomads/)
- [Learning to code and building a $28k/mo portfolio of SaaS products — Indie Hackers](https://www.indiehackers.com/post/tech/learning-to-code-and-building-a-28k-mo-portfolio-of-saas-products-OA5p18fXtvHGxP9xTAwG)
- [The AI API Arbitrage Play — Bet on AI](https://betonai.net/the-ai-api-arbitrage-play-how-developers-are-making-3k-15k-month-reselling-ai-apis-2026-breakdown/)
- [VIN Lookup API — VinAudit](https://www.vinaudit.com/vin-lookup-api)
- [A case against AI wrapper companies — BudEcosystem](https://blog.budecosystem.com/a-case-against-ai-wrapper-companies/)
- [Affiliate Payout Models in Online Gambling: CPA vs. Revenue Share](https://irev.com/blog/affiliate-payout-models-in-online-gambling-cpa-vs-revenue-share/)
- [Casino Affiliate Programs: Complete 2026 Operator Guide](https://track360.io/blog/casino-affiliate-programs-complete-guide-2026)
- [Best Crypto Affiliate Programs — 2026 Guide and Comparison](https://changehero.io/blog/best-crypto-affiliate-programs/)
- [Best 9 Crypto Affiliate Programs in 2026](https://changenow.io/blog/best-9-crypto-affiliate-programs-in-2026)
- [10 Most Profitable Affiliate Marketing Niches](https://elementor.com/blog/most-profitable-affiliate-marketing-niches/)
- [Google's March 2026 Core Update Hit Affiliate Sites Harder Than Any Other Category](https://www.affiversemedia.com/googles-march-2026-core-update-hit-affiliate-sites-harder-than-any-other-category/)
- [Niche Website Portfolio Strategy — Rubab's Digital](https://rubabsdigital.com/blog/niche-website-portfolio-strategy)
