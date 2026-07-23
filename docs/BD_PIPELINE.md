# Peregrin — BD Pipeline

Status snapshot as of 2026-07-23, built from the actual tracking files (`reference/peregrin_supply_target_shortlist.xlsx`, `reference/Peregrin_B2B_Partner_Targets.xlsx`), not estimates.

## Important: two different pipelines exist, don't conflate them

**Pipeline A — the original supply-side outreach (33 SE Asian travel agencies).** This was built
under the *old* strategy: asking agencies to act as ticket suppliers (create GDS holds directly).
A compliance review (`reference/Peregrin_Supplier_Model_Review.md`) found real legal/contractual
risk in that ask, so the model was rewritten to a more compliant "refundable/hold-fare" version
(v2) before most sends went out. The current product strategy (direct Duffel integration) no
longer needs agencies as *suppliers* at all — so this pipeline's original purpose is obsolete.

**Pipeline B — B2B/affiliate partner targets (14 companies)**, from `Peregrin_B2B_Partner_Targets.xlsx`.
This is the *current* strategy's actual pipeline: affiliate deals (iVisa, SafetyWing, World
Nomads, Airalo), wholesale/reseller conversations, and long-shot OTA partnerships. **No outreach
copy has been written for this pipeline yet** — the existing drafted emails (Pipeline A) pitch
agencies to *supply* reservations, not to *resell or affiliate with* Peregrin's product. These are
different asks and need different copy.

## Pipeline A status (33 targets, supply-outreach — largely obsolete, salvage what's useful)

| Status | Count |
|---|---|
| Contacted (v2, compliant model) — clean send | 12 |
| Contacted (v2) — bounced | 2 |
| Contacted (v1, superseded model) — follow-up pending | 2 |
| Not contacted — no email on file | 2 |
| Not contacted | 14 |

Countries: Thailand (TTAA directory), Philippines (PTAA), Indonesia (ASTINDO).

**Recommendation:** don't continue this pipeline as-is. The 12 agencies who received a clean v2
send may still be worth a follow-up, but reframed entirely — not "will you supply us reservations"
but "would you white-label resell Peregrin to your own customers" (Pipeline B's actual pitch).
The 2 "v1 superseded" contacts (AA Travel, Anyways Travel) specifically need a decision on whether
to re-approach at all, given the original ask to them is now off-strategy.

## Pipeline B status (14 targets, B2B/affiliate — this is the live strategy)

| Tier | Count | Examples |
|---|---|---|
| 1 — Start here | 5 | iVisa, SafetyWing, World Nomads, FlyingHelpline (benchmark), Volticket (benchmark) |
| 2 — More effort | 5 | VisaHQ, Sherpa, relocation/immigration firms (segment), niche OTAs (segment), Airalo |
| 3 — Aspirational | 4 | Agoda, Flight Network, Skyscanner, Booking.com |

**No outreach has been sent on this pipeline. No email copy exists for it.** This is the real
next-action gap — see automation roadmap for a proposed "BD prospect + outreach drafting"
workflow.

## Open decisions

1. Do we re-approach the 12 clean-sent Pipeline A agencies with a Pipeline B (reseller) pitch, or
   let that pipeline lapse and start Pipeline B fresh?
2. AA Travel / Anyways Travel (v1, superseded) — re-approach or drop?
3. Pipeline B outreach copy doesn't exist yet — needs writing before any sends can go out (and per
   standing rule in `CLAUDE.md`, needs Liam's explicit approval before sending regardless).
