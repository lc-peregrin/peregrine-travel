# Peregrin Supplier Outreach — Compliance Review & Alternative Model

*Prepared for Liam Conroy. Not legal advice — the legal-risk points below are general framework, not jurisdiction-specific counsel. Given the immigration-document angle, it's worth a short paid consult with a lawyer who covers travel/aviation and immigration law before this goes further.*

## 1. What in the previous emails creates concern

The AA Travel and Anyways Travel emails asked the agency to:

1. Create GDS reservations "most of which will intentionally never be ticketed."
2. Accept payment from Peregrin specifically to cover the resulting ADM (Agency Debit Memo) exposure and GDS unproductive-segment fees.
3. Distribute the churn across carriers "to keep... churn below enforcement attention."

Point 3 is the one that turns this from an aggressive pitch into a real problem. It's not proposing a product — it's proposing to help the agency stay under the radar of the exact systems (airline fraud monitoring, GDS audit thresholds) designed to catch this behavior. That's evasion, stated openly, in writing, from a company email address.

## 2. GDS/contractual rules vs. actual legal exposure

These are two different categories and it's worth keeping them separate, because they carry very different consequences.

**Contractual (private, between the agency and the GDS/airline/IATA):** GDS subscriber agreements and IATA resolutions generally prohibit "speculative" or "duplicate" bookings — reservations made with no intent to complete a purchase, held only to occupy inventory or generate a PNR. Agencies that do this face ADMs, segment fees, and in serious cases suspension of GDS access or IATA accreditation. This is a breach of contract, not a crime. It's the agency's contract, not Peregrin's — but a company that knowingly induces another party to breach its contract with a third party can itself be exposed to a claim for tortious interference or inducement of breach, depending on jurisdiction. Paying the agency specifically to absorb the penalty for breaching its own agreement is the part that makes intent hard to argue around.

**Actual legal exposure (criminal/regulatory):** This depends on what the end customer does with the document, not on the GDS mechanics. If a customer presents a reservation to an immigration officer or airline check-in desk as evidence of onward travel, and everyone involved in producing that document knew in advance it would never be honored, that starts to look like producing a document intended to support a false representation to a government official or carrier — the legal characterization varies a lot by country (Thailand, Indonesia, Philippines, Australia, and destination countries could all reach different conclusions), but "known-fake evidence, sold for the purpose of being relied on as real" is the pattern regulators and prosecutors look for. This is a materially different — and larger — risk than the GDS contract issue.

The reason this distinction matters commercially: the existing proof-of-onward-travel industry (onwardticket.com and similar) generally operates on the *contractual* side of this line, not the *legal* side. They use real, bookable reservations obtained through channels that don't require anyone to lie about intent — which is exactly the gap in the previous outreach.

## 3. Alternative supplier models

**Refundable/cancellable real ticket.** Book an actual ticket on a fully refundable or low-penalty fare, hold it through the customer's travel date or visa interview, then cancel/refund inside the airline's own stated window. Cost = fare minus refund minus any cancellation fee. Higher COGS than a churn model, but it's a real ticket, honestly cancelled under the fare rules the airline itself published. No one is deceived about intent because the fare product explicitly permits cancellation.

**Airline/GDS-sanctioned hold products.** Some carriers and GDSs already offer official "hold without payment" windows (longer ticketing time limits on certain fare classes, or programs some airlines market directly as fare holds). Building on a hold window the airline itself publishes is structurally different from manufacturing one through undisclosed churn — worth a direct conversation with airlines or a GDS aggregator (see below) about whether such a product exists or could be negotiated for this named use case.

**Consolidator/agency partnership, fully disclosed.** Approach agencies with the real use case stated plainly, and ask what compliant product they can offer — many consolidators already have refundable-fare inventory priced for exactly this kind of reseller. Let the agency price and structure the product within its own supplier agreements, rather than asking it to work around them.

**White-label / affiliate of an existing provider.** onwardticket.com and comparable providers have already solved the supply problem, presumably on the compliant side of it, at real scale. A reseller or affiliate deal (Peregrin handles the customer-facing brand, documentation formatting, and support; the partner handles supply) gets Peregrin to market fast with none of the supplier-compliance risk, at a lower margin than owning supply directly.

**Direct API integration.** Duffel, Travelport Universal API, Amadeus for Developers, and similar aggregators support programmatic booking of real fares, including flexible/refundable ones. Contracting directly and disclosing the use case removes the retail-agency intermediary and the associated churn-detection problem entirely; margin is set by fare cost and Peregrin's own markup rather than a risk premium paid to an agency.

**Bundled insurance/travel document product.** Pair a low-cost refundable ticket with travel insurance (which many visa applications also require) and sell the bundle. This adds a second legitimate revenue line and can offset the cost of holding a real refundable fare instead of a throwaway PNR.

## 4. Rewritten outreach

This version discloses the real use case, asks what the agency can legitimately offer, and doesn't ask anyone to breach their own supplier agreements or hide volume from anyone.

> **Subject: Partnership enquiry — refundable/hold-fare reservations for visa documentation**
>
> Hi [Name],
>
> I found [Agency] through the [TTAA/PTAA/ASTINDO] directory and saw your IATA accreditation ([number]).
>
> I'm building Peregrin, a service that provides travellers with genuine flight reservations to meet visa and immigration proof-of-onward-travel requirements — similar in concept to onwardticket.com. The product only works commercially if the underlying reservations are ones your agency is comfortable issuing under your own GDS and airline agreements, so I want to be upfront about the use case rather than have it come up later.
>
> Specifically, I'm interested in whether you can offer:
> - Fully refundable or low-cancellation-fee fares that we'd hold through a traveller's documentation deadline and then cancel within the fare's own rules, or
> - Any official hold-without-payment / long ticketing-time-limit product your GDS or airline partners already support for this kind of use case.
>
> Volume would start modest (a few dozen bookings/month) and scale with demand on our side. I'm happy to structure this however keeps you clearly within your supplier agreements — including paying a service fee that reflects the extra handling, if that's the cleaner way to price it.
>
> Open to a 20-minute call this week or next if useful.
>
> Regards,
> Liam Conroy
> lc@peregrin.travel | linkedin.com/in/liam-conroy-5b906a144

## 5. Recommendation

Between the models above, I'd start with the **white-label/affiliate route in parallel with direct API integration**, not the retail-agency-partnership route the original outreach was built on — for a practical reason as much as a compliance one. Retail-agency supply built on any kind of churn or risk-pricing arrangement is a treadmill: each agency eventually gets flagged, penalized, or drops out, and you're constantly re-selling the same risky pitch to new agencies as old ones fall away. It doesn't scale, and now you also know it carries the legal exposure described above.

The refundable-fare and API-aggregator models scale with capital and fare-cost negotiation instead of with how many agencies you can talk into taking on hidden risk — that's a much more durable position, and it's the model the market leader in this space is presumably already running. A white-label or affiliate deal gets you to market fastest with the least new risk while you build direct airline/aggregator relationships in parallel; direct API integration becomes the long-term margin play once volume justifies it.

One more thing worth doing before any outreach goes out again: get the specific immigration-fraud question (point 2, legal exposure) answered by counsel for the jurisdictions Peregrin actually operates in and sells into. The GDS-contract risk is manageable through supplier choice; the legal-exposure question is not something to resolve by business-model design alone.
