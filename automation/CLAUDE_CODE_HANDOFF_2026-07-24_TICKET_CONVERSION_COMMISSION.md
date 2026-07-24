# Claude Code handoff — "Honour the flight" paid conversion + commission

Purpose: monetise the case where a customer with a held reservation decides to actually fly. Charge
the airfare + a Peregrin service fee (commission) via Stripe, THEN issue the ticket via Duffel. This
converts the current `/api/order/:id/confirm` path from a money-LOSER (it previously ticketed out of
Peregrin's own Duffel balance for free — that bug is gated to test mode now) into a revenue path.

## Priority / sequencing (READ FIRST)
- This spends REAL money (buys a real ticket). Build behind a flag `ENABLE_TICKET_CONVERSION=false`.
- DO NOT enable in production until ALL of these are true:
  1. Duffel live **hold orders** are enabled (current blocker — support request pending).
  2. Live ticket **issuance** is tested end-to-end with Peregrin's real Duffel payment method.
  3. The charge-before-issue + refund-on-failure logic below is tested (test mode).
- Lower priority than shipping the /privacy page and design polish. Build in parallel, ship last.

## Flow
1. Customer clicks "Confirm & issue my ticket" on their held reservation.
2. Server **re-validates/re-prices** the held Duffel order (confirm still ticketable; get current
   fare). If the fare changed since the hold, surface the NEW total before charging — never silently
   charge a different amount.
3. Compute `total = airfare + service_fee`, where
   `service_fee = max(CONVERSION_FEE_FLAT, CONVERSION_FEE_PCT * airfare)`.
   Add env/config constants alongside the existing `HOLD_FEE_*`:
   `CONVERSION_FEE_FLAT` (recommend 29.00 USD) and `CONVERSION_FEE_PCT` (recommend 0.07).
4. Create a Stripe Checkout Session for `total`, `purpose:"ticket_conversion"`, metadata:
   `{ orderId, airfare, service_fee }`. Show line items: **Airfare**, **Service fee**, **Total**.
5. On `checkout.session.completed` webhook where `purpose === "ticket_conversion"`:
   mark paid, THEN call Duffel to pay/issue the order. **Only issue after payment has cleared.**
6. On success: issue e-ticket, deliver confirmation + email, set order status to "E-ticket issued".
7. On issuance failure AFTER payment: automatically refund the customer in full (Stripe refund) and
   alert (log + notify). Never leave a charged-but-not-issued order silently.

## Guardrails (non-negotiable)
- NEVER issue a ticket without a completed Stripe payment tied to THAT order. Keep issuance gated on
  the paid session id; do not reuse the old free `/confirm`.
- Idempotency: one ticket per order — guard against double-issue on webhook retries.
- Price integrity: re-price at checkout creation; charge exactly what's shown. If Duffel's fare rose
  between display and pay, re-confirm and show the new total rather than absorbing or overcharging.
- Transparency (consumer law): always show the itemised breakdown (Airfare / Service fee / Total)
  BEFORE payment. Do not bury the fee.
- Fraud/chargeback: enable Stripe Radar; large airfare charges are higher-risk than the $14.99 hold
  fee. Consider a manual-review threshold or an amount cap for very high fares initially.

## Copy (legal-safe; keep the noun rule)
- CTA: `Confirm & issue your ticket`
- Explainer: `Decided to fly? We'll issue your e-ticket for this reservation — the airfare plus a
  small service fee. We re-check your fare live before you pay, so there are no surprises.`
- Breakdown labels: `Airfare`, `Service fee`, `Total`.
- Success heading: `E-ticket issued` (consistent with the rest of the site).
- Localise across en/es/ru/hi.

## Economics (internal — for Liam, not customer copy)
With `max($29, 7%)`:
- $200 fare → $29 fee; Stripe ~$6.60 on $229; Duffel per-order ~$3 → net ≈ $19.
- $600 fare → $42 fee (7%); Stripe ~$18.60; Duffel ~$3 → net ≈ $20.
Tune FLAT/PCT to taste. Most customers never convert (the whole product is that they don't fly), so
this is bonus revenue — price for clear margin and low risk over volume, not aggressive conversion.

## Ties in with
- The **hold-expiry reminder** automation (email: "want to actually fly? confirm before [date]") is
  the natural driver for this flow — build the email to deep-link into this conversion checkout.
