# Peregrin — Business Model & Operations

Prepared for Liam Conroy, 2026-07-20. Synthesizes prior research (competitor/market analysis, GDS access review, compliance review, revenue modelling) with the current state of the build. Market-size figures are third-party estimates and should be validated before being used in a pitch or financial plan; nothing here is legal or financial advice.

## 1. The business in one paragraph

Peregrin sells travellers genuine, verifiable flight reservations — real PNRs held directly with an airline via Duffel's documented Hold Order feature — to satisfy "proof of onward/return travel" requirements at check-in, immigration, or in a visa application. Most customers never intend to fly; the hold lapses automatically per the fare's own terms if unpaid. Customers who do want to travel can pay to confirm before it expires. Marginal cost per reservation is a few dollars (Duffel's order fee); the product is fully automatable; and demand is structural, not seasonal — it exists as long as onward-travel proof rules exist. The constraint isn't technology or demand, it's distribution (trust/SEO) and, for the B2B wedge, sales relationships.

## 2. Market size and opportunity

This is a real, proven market, not a novel idea. Direct competitors (onwardticket.com, dummyfares.com, dummy-tickets.com, volticket.com, flyinghelpline.com) collectively draw an estimated 370–470K monthly visits, with onwardticket.com alone at roughly 170K/month. At typical high-intent conversion rates for this category (4–10%, versus 1–3% for generic e-commerce) and a $15–20 average order value, back-of-envelope revenue for the category leader is plausibly $1.5–4M/year at 60–80% operating margin; tier-2 players (50K monthly visits) plausibly clear $35–90K/month. Total addressable direct market is roughly **$6–12M/year today**, growing with visa-run tourism, digital nomadism, and stricter airline document checks — plus a larger adjacent market in visa agencies, eSIM apps, and travel insurers who could embed this as a feature rather than build it.

The honest strategic read: this is a "margin machine constrained not by demand or technology but by supply access and distribution trust." Going head-on against onwardticket.com's multi-year Trustpilot/Reddit reputation on their own SEO terms is a slow, capital-intensive fight not worth having yet. The three viable wedges, in priority order, are:

1. **B2B/API wedge** — white-label reservations sold wholesale to travel agencies, visa consultancies, eSIM apps, and insurers who already own the customer relationship. No SEO war, higher defensibility, recurring volume — five signed partners can match a year of organic content traffic.
2. **Geographic/language wedge** — non-English markets (Spanish, Russian, Hindi to start) are demonstrably underserved by incumbents' SEO. The demo already supports this.
3. **Premium trust wedge** — an "actually ticketed, verifiable, refund-if-it-fails" tier at $29–49 for the risk-averse segment incumbents leave anxious.

## 3. Product & unit economics

| Tier | Mechanic | Est. cost to Peregrin | Suggested price | Est. margin |
|---|---|---|---|---|
| Reservation Hold (standard) | Duffel hold order, real PNR, lapses automatically if unpaid | ~$3 (Duffel order fee) + ~3% payment processing ≈ $3.30 | $9.99–12 | ~65–70% |
| Confirmed Ticket (premium) | Real ticketed booking, refunded/cancelled within the fare's own window | Fare cost (temporary cash float) + 1% Managed Content fee + refund friction | $39–49 | Thin, capital-intensive — priced to match this tier's competitors |
| Return / multi-city | Same mechanic, more segments | ~$5–6 (2x order fee) | $14–18 | Similar % margin to standard |
| Accommodation proof add-on | Real, freely-cancellable property booking (Duffel Stays, pending access) | Cost of the booking minus refund | $10–15 markup | Thin but near-zero marginal effort |

Near-zero-incremental-cost add-ons already scoped: travel insurance referral (SafetyWing ~10% of premium, World Nomads via CJ), visa document referral (iVisa ~20% commission), eSIM/connectivity affiliate (Airalo-style). At $9.99 retail and ~$3.30 cost, each standard reservation clears roughly $6.70 before payment processing and marketing — competitive with the low end of the market and better-margin than several competitors.

## 4. How a booking actually works (operations)

1. Traveller (or partner, via API) searches a route on the site.
2. Peregrin's backend calls Duffel's offer search, filters out any fare that requires instant payment (Peregrin's whole model depends on holdable fares), and returns real, live offers.
3. Customer selects an offer and enters passenger details; the backend creates a real Duffel order with `type: "hold"` — a genuine PNR is created directly with the airline, no payment taken.
4. Peregrin generates a branded PDF (and, once fully live, emails it) containing the booking reference, itinerary, and a disclosure that the document is a held reservation, not a ticket, until the status shows confirmed.
5. The hold lapses automatically at the fare's `payment_required_by` deadline if the customer takes no action — this is a standard, airline-sanctioned feature (deferred ticketing), not a workaround.
6. If the customer wants to actually fly, they pay before the deadline and Peregrin's backend calls Duffel's payment API to confirm and ticket the order for real.
7. The reservation can be independently re-verified at any time via a "Verify" call straight to Duffel's live order status — this is the core trust mechanic that differentiates a real PNR from a forged document.

Everything above is already built and working end-to-end against Duffel's test environment. What's manual today: nothing in the booking flow itself: it's a fully automated pipeline once Duffel go-live is approved. What remains manual is partner sales/support and content production.

## 5. Supply chain and legal positioning

Early exploratory outreach to retail travel agencies asked them to create GDS reservations "most of which will intentionally never be ticketed" and to distribute churn across carriers to stay under airline fraud-monitoring thresholds. A compliance review flagged this as a real problem: it isn't a product pitch, it's asking an agency to help evade the systems designed to catch exactly this behaviour, which creates both contractual exposure for the agency (ADMs, GDS suspension) and potential legal exposure for Peregrin (inducement of breach, and — depending on jurisdiction — the more serious question of producing a document known in advance to misrepresent intent to a government official or carrier).

The model actually being built avoids this entirely: **direct API integration via Duffel**, using their official, documented Hold Order & Pay Later feature, with the use case disclosed to Duffel directly (the go-live application states plainly that most holds are never ticketed and describes the disclosed customer terms). This removes the retail-agency intermediary and the associated churn-detection problem, and puts Peregrin on its own first-party contractual relationship with the GDS/airlines rather than asking a third party to quietly carry the risk. It is the more durable position of the two, and the one the market leader is presumably already running.

Recommended posture going forward, consistent with the compliance review: keep terms of service explicit ("a real, verifiable reservation — not a purchased ticket, and not travel"), get country-specific legal input before scaling into any new jurisdiction (the "known-fake evidence sold to be relied on as real" pattern is the one regulators look for, and it depends on end-use, not on the booking mechanic), and treat the refundable/ticketed premium tier and the B2B/affiliate wedge as the primary growth paths rather than rebuilding a retail-agency-churn supply chain.

## 6. Current build status (as of 2026-07-20)

**Live:** demo site (peregrin-demo.vercel.app), deployed via GitHub → Vercel auto-deploy. Real Duffel test-mode search, hold, verify, and confirm-and-pay flow. Branded PDF generation (recently rebuilt to a professional itinerary-document standard, benchmarked directly against a competitor's sample). White-label branding toggle (partner name/colour). Four-language UI (English, Spanish, Russian, Hindi). Accommodation proof-of-booking flow, blocked on Duffel Stays access approval (a separate access request from Duffel Flights, submitted). Domain email sending verified (send.peregrin.travel, DKIM/SPF/MX configured in Resend and Namecheap).

**In progress / pending:**
- Duffel go-live approval (moves the account off test-mode API keys) — application submitted, status not yet confirmed.
- Duffel Stays access (separate product) — contact request submitted.
- Stripe payment collection — account onboarding started, integration into the site not yet built. This is the single missing piece between "working demo" and "able to take real money," since Duffel's fees are what Peregrin pays Duffel, not how Peregrin charges its own customers.
- Business registration (ABN via business.gov.au) and Wise Business account — both in progress.
- Email delivery of the PDF — code complete, domain now verified; needs a redeploy to confirm end-to-end.

**Not yet started:** B2B partner API (the `/reservations`, `/reservations/{id}`, `/reservations/{id}/confirm` endpoints and webhook described in the architecture doc) — the consumer demo exists, but the wholesale/reseller integration layer for partners doesn't yet. Affiliate program signups (iVisa, SafetyWing) — prepped and ready, pending the user completing identity/financial fields personally. SEO content (the highest-leverage, longest-lead-time channel every competitor relies on) — not started.

## 7. Team and capital

Based on the original market sizing work, a credible path to launch realistically needs $25–60K and 6–9 months if building the full retail-facing business (site/automation, agency deposits or working capital, SEO content, part-time support). Given the pivot toward a B2B-first strategy, the capital requirement is materially lower up front — the technology is close to complete, and the primary spend becomes partner sales effort (largely the user's own time, already underway via the agency shortlist and outreach work) rather than SEO content spend or working capital for a buy-and-cancel float. A support VA (Philippines/Thailand-based, part-time) becomes relevant once order volume is real rather than a day-one requirement.

## 8. Risks (ranked, adapted to the current B2B-first strategy)

1. **Duffel go-live rejection or delay** — the single biggest near-term dependency; there's no fallback supply path currently built if it's rejected.
2. **Payment processor risk** — this category is plausibly "high-risk" from a payments-underwriting perspective (Stripe may apply extra scrutiny or restrict the account); worth having a second processor path in mind rather than depending on one.
3. **Regulatory/policy shift** — airlines or GDSs tightening hold windows on some fares, or a destination starting to verify ticketed status rather than just a reservation. Mitigation: the premium ticketed tier already exists as a fallback offer.
4. **Legal/reputational grey zone** — not clearly illegal anywhere major on the current disclosed-reservation model, but genuinely jurisdiction-dependent; a paid legal consult before scaling into new markets is the standing recommendation from the compliance review, not yet done.
5. **B2B sales cycle length** — agency and partner sales, even when receptive, move slower than a self-serve checkout; the go-to-market plan should not assume fast conversion.
6. **SEO/AI-search dependence for the retail channel** — deprioritised in favour of B2B for now, which is itself a mitigation, but worth being aware AI answer engines are increasingly intercepting "do I need an onward ticket"-style queries, shortening the runway on that channel if it's revisited later.
