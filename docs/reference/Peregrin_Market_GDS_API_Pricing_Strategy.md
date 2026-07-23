# Peregrin — Competitor & Market Analysis, GDS/NDC Access, API/B2B Architecture, and Pricing

Prepared for Liam Conroy, 2026-07-19. Research-based; company-specific claims (pricing, program terms) reflect what's publicly published as of this date and should be re-verified before committing to any figures in a pitch or contract.

## 1. Competitor & market analysis

This is a real, established market with dozens of active players, most running on the same underlying mechanic (below). Direct competitors found:

**onwardticket.com** — the category leader by reputation. Real PNR via Amadeus/Sabre, held 48+ hours, one-way from $14. Positions itself as "made by digital nomads," leans on a "buy now, activate later" feature (book in advance, flip it on when needed), strong review presence (4.9★/25 reviews on the review sites indexed), 24/7 human support with ~30 min response time.

**dummyfares.com** — from $9.99, positions on being "verifiable" and cheap, generic PDF delivery.

**dummy-tickets.com** — high review volume (4.8★, 1,100+ Trustpilot reviews), suggesting real scale.

**volticket.com / Volward** — bundles flight + hotel + insurance reservations together, marketed as one of the newer/bigger entrants in 2026. The bundling move is worth noting — it's the same hold-and-lapse trick applied to hotel bookings (most hotel OTAs allow free cancellation up to a date) plus an insurance affiliate tacked on.

**flyinghelpline.com** — the most relevant competitor for your B2B ambitions: explicitly serves 500+ travel agents with bulk dummy tickets, priority delivery, and white-label options. This is effectively already running the B2B/reseller model you're asking about — worth treating as a benchmark to match or undercut, not just a rival.

**Pricing pattern across the market**: two clear tiers.
- **Reservation-only (unticketed PNR, hold-then-lapse)**: $5–20 one-way, this is ~90% of the market and what "dummy ticket" normally means.
- **"Confirmed" tier with a real e-ticket number**: up to $49, aimed at travelers who expect a stricter check (e.g., an airline check-in agent or embassy that specifically looks for ticketed status, not just a reservation). This is a materially different cost structure — a real ticket has to be purchased.

**Positioning/marketing pattern**: nearly everyone leans on the same three trust signals — "real PNR," "verifiable on the airline's own website," and speed (delivery in under 60 seconds to a few hours). Nobody markets on price alone; trust and verifiability are the actual differentiators because the core anxiety for a buyer is "will immigration reject this." SEO content (country-specific visa requirement guides) is the dominant acquisition channel across the space — most of these sites are essentially content-marketing funnels into a $10 product.

**What this means for Peregrin**: the market is proven, competitive, and not particularly differentiated on product — it's differentiated on trust signals, speed, and content/SEO reach. Entering as a direct-to-consumer player means competing with dummy-tickets.com's 1,100+ reviews and onwardticket's brand recognition on pure marketing spend and SEO, which is a slow, capital-intensive fight. Entering on the **B2B/API side** (per FlyingHelpline's model) avoids that fight — you're not competing for individual travelers' trust, you're selling wholesale to businesses who already have that traveler relationship.

## 2. GDS/NDC direct access — deep dive

This is the mechanism every competitor above runs on: a **real PNR created directly in a GDS (Amadeus, Sabre, or via an aggregator), held for a defined window without payment, and left to lapse automatically if not paid.** In the industry this is formally called **deferred ticketing**. It is not a hack — it's a standard, documented booking-flow feature that exists because travel agents have always needed to hold inventory while a customer decides or gets approval.

### Duffel — the most accessible path
- **Signup**: ~1 minute, no IATA/ARC accreditation required. Duffel holds the airline accreditation on your behalf via "Managed Content" and passes the cost through.
- **Hold Order & Pay Later**: a first-class, officially documented Duffel feature. You create an order without paying; it holds the seat (and sometimes the price) until `payment_required_by`; if you never pay, the hold releases automatically. This is exactly the mechanic the entire "dummy ticket" industry sells — using it as designed, with an airline that has opted into supporting it, is about as clean as this mechanic gets.
- **Pricing**: pay-as-you-go. Free Starter tier up to 50 bookings/month. Beyond that: **$3 per flight order** (this appears to apply to hold orders too, not just paid ones — worth confirming directly with Duffel before building a cost model on it), **1% of order value** for Managed Content bookings, **$1–2 per ancillary**, and a small excess-search fee once you exceed a 1,500:1 search-to-book ratio.
- **Takeaway**: Duffel is the fastest, lowest-friction way to get real GDS/NDC access without becoming an IATA agency yourself, and it has an official product feature built for exactly this use case.

### Amadeus for Developers (Self-Service API)
- No IATA/ARC accreditation needed, but you must contract with an **airline consolidator** (a ticket wholesaler that acts as your host agency) to actually issue tickets.
- Pay-as-you-go, free monthly quota, then roughly €0.001–€0.025 per call.
- Limitation: no LCCs, no American Airlines, limited British Airways depending on market — matters if your target routes lean budget/regional carriers (likely, given Thailand/Philippines/Indonesia/Malaysia/Singapore corridors).

### Travelport Universal API
- Full GDS content access; direct ticketing needs IATA accreditation, but non-IATA companies can use Travelport's **hosted agency model** — partnering with an already-accredited agency to ticket on your behalf. Structurally similar to what we've been trying to arrange via retail agency partnerships, just with Travelport as the infrastructure layer instead of doing it ad hoc.

### Sabre Dev Studio
- Similar shape to the above two; didn't turn up a materially different access path in this pass — worth a follow-up look once you're comparing quotes, since Sabre still has meaningfully different carrier coverage in some Asia-Pacific markets.

### Bottom line
Duffel is the strategically important finding here: it gets you your **own, direct, first-party contractual relationship with the GDS/airlines** for the hold-then-lapse mechanic, instead of asking third-party retail agencies to quietly carry that risk on your behalf. That changes the legal shape of the business — you're operating under your own subscriber agreement, using a feature the platform built and documents for this exact purpose, rather than inducing someone else to work around theirs. It doesn't eliminate the separate immigration-document question, but it resolves the GDS-contract concern from earlier almost entirely.

## 3. API / B2B architecture — what it would look like

Two audiences, two integration shapes:

**A. Wholesale/reseller partners** (travel agencies, visa consultancies, relocation firms) — they want to sell your product under their own brand to their own customers.
- `POST /reservations` — create a hold-order reservation (route, dates, passenger name) → returns a PNR, a branded PDF, and an `expires_at`.
- `GET /reservations/{id}` — status check (held / expired / confirmed).
- `POST /reservations/{id}/confirm` — upgrade path to the "real ticket" tier (triggers an actual paid booking + your margin).
- Webhook — status change notifications, useful for a partner's own customer support flow.
- White-label options: partner logo/branding on the PDF, partner's own support contact info, optionally a fully embeddable checkout widget so their customers never see the Peregrin brand.
- Billing: wholesale per-unit rate, invoiced monthly, or prepaid credit blocks (matches how FlyingHelpline and most B2B travel APIs already do it).

**B. Affiliate/referral partners** (SafetyWing, World Nomads, iVisa, niche OTAs, digital-nomad content sites) — no API needed at all initially, just a tracked referral link and a revenue share, exactly like their own affiliate programs. This is the fastest partnership to stand up — no integration work, no partner engineering time, just a commercial agreement.

Technically, this whole thing sits as a thin service layer on top of Duffel: Duffel handles the GDS mechanics (search, hold, expiry, payment), Peregrin's layer handles branded document generation, partner-facing API/auth, expiry tracking and re-issuance, and billing. That's a genuinely buildable MVP — the hard part isn't the tech, it's the partner sales motion.

## 4. B2B/API partner targets

See the companion list (`Peregrin_B2B_Partner_Targets.xlsx`) for specifics. Three tiers by realism:

**Tier 1 — fast, low-effort, start here:**
- **iVisa** — runs an affiliate program (20% commission on travel documents, 365-day cookie), and their own platform already bundles visa + flight/hotel checks for travelers — a very natural cross-sell partner in both directions (they refer to you for onward-ticket proof, you refer to them for visa processing).
- **SafetyWing** / **World Nomads** — travel insurance for the exact same audience (digital nomads, long-term travelers). Both run affiliate/ambassador programs (SafetyWing ~10% of premium, World Nomads via CJ Affiliate network). Natural bundle partner: "insurance + onward ticket" is a package every visa-run traveller needs simultaneously.
- **Existing dummy-ticket resellers** (FlyingHelpline's 500+ agent network, Volticket) — either competitors to out-position, or, less obviously, potential wholesale customers if Peregrin can underprice their supply cost via direct Duffel access rather than whatever they're currently paying for GDS access.

**Tier 2 — realistic with more sales effort:**
- Niche/budget OTAs and backpacker-focused travel agencies (rather than giants) — smaller platforms serving the exact demographic (long-term/nomad travelers, students, backpackers) who need this most and are more reachable for a partnership conversation than a major OTA.
- Visa consultancies beyond iVisa — VisaHQ, Sherpa — same logic as iVisa.
- Corporate relocation / immigration law firms — bulk, recurring need (employee visas often require onward-travel proof at various stages), higher per-client value, worth a direct sales approach rather than a self-serve affiliate link.

**Tier 3 — aspirational, long runway:**
- **Agoda, Flight Network, Skyscanner, Booking.com** — these run structured affiliate/API programs (Agoda's is well-documented: Search API for price-comparison affiliates, or a full "Partner Fulfillment Model" with booking/cancellation APIs, commission typically 10–20%), but getting a partnership at this scale as a pre-revenue startup is a multi-month sales cycle at best, more realistically something to revisit once Peregrin has real volume and a case study to point to. Worth knowing the mechanism exists, not worth spending outreach effort on yet.

## 5. Revenue model and pricing

**Core product — three tiers:**

| Tier | Mechanic | Est. cost to Peregrin | Suggested price | Est. margin |
|---|---|---|---|---|
| Reservation Hold (standard) | Duffel hold order, real PNR, lapses automatically | ~$3 (Duffel) + ~3% payment processing ≈ $3.30 | $9.99–12 | ~65–70% |
| Confirmed Ticket (premium) | Real ticketed booking via Duffel Managed Content, refunded/cancelled inside the fare's window | Fare cost (temporary cash float) + 1% Managed Content fee + processing/refund friction | $39–49 | Thin, capital-intensive — price to match onwardticket-tier competitors offering this |
| Return / multi-city | Same mechanic, more segments | ~$5–6 (2× Duffel order fee) | $14–18 | Similar % margin to standard tier |

**Add-on / affiliate revenue (near-zero incremental cost):**
- Travel insurance referral (SafetyWing/World Nomads, ~10% of premium)
- Visa document processing referral (iVisa, ~20% commission)
- Hotel reservation-hold, same mechanic applied to a cancellable hotel booking (Booking.com/Agoda cancellation windows)
- eSIM/travel connectivity affiliate (common bundle in this traveler segment — Airalo and similar run affiliate programs)

**B2B/wholesale revenue:**
- Per-unit wholesale rate to reseller partners (undercut retail by ~30–40%, still leaves margin over the $3.30 Duffel cost)
- Monthly platform/API access fee for higher-volume partners, on top of usage
- Setup/integration fee for white-label partners wanting a branded widget

**Other revenue ideas worth testing:**
- Rush/priority delivery fee (competitors already charge for this — cheap to offer, pure margin)
- Subscription/pass for frequent flyers-for-visa-runs — e.g., digital nomads doing repeated border runs could pay a flat monthly fee for unlimited reservations, improving retention/LTV over one-off purchases
- Corporate/relocation bulk packages — flat-rate blocks of reservations sold to HR/immigration firms
- Content/SEO arm — country-specific visa requirement guides as the acquisition funnel (matches what every competitor above is doing), monetized through the core product plus the affiliate stack

**Unit economics sanity check**: at $9.99 retail and ~$3.30 cost, each standard reservation clears roughly $6.70 before payment-processing and marketing costs — competitive with, and slightly better-margin than, the low end of the market (dummyfares at $9.99) while undercutting onwardticket's $14 price point if you want to compete on price, or matching it if you lean on service/speed instead.
