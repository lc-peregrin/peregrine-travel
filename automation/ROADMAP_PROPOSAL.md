# Automation roadmap — proposal, not yet built

Read from `docs/BUSINESS_PLAN.md` and `docs/MARKETING_PLAN.md`. Nothing here is built — this is
the list to approve before implementation starts, top-down by priority. Per `CLAUDE.md` standing
rule 3, anything that sends external communication stays approval-gated regardless of how "safe"
the automation otherwise is.

| # | Automation | What it does | Connectors needed | Frequency | Mode |
|---|---|---|---|---|---|
| 1 | BD prospect research | Expand `docs/BD_PIPELINE.md` Pipeline B (currently 14 targets) — find more affiliate/wholesale candidates matching the same profile, log them to the tracking sheet. | None required (web search); Google Sheets connector if writing directly to a live sheet instead of the local file. | Weekly | Fully automatic (research only, no outreach) |
| 2 | Competitor/price monitoring | Track onwardticket.com and peers (dummyfares.com, volticket.com, flyinghelpline.com) for pricing changes, new features, SEO content shifts. | None required (web search) | Weekly | Fully automatic (digest only) |
| 3 | Weekly SEO/content drafting | Draft long-tail, non-English content pages per Marketing Plan Phase 3 (country+visa-type pages, language-wedge markets). | None required for drafting; a CMS/hosting connector once `/site` gets a content section. | Weekly | **Approval-gated** — drafts only, Liam reviews before anything publishes |
| 4 | Social post drafting | Draft posts for the "creator/affiliate program" and general brand-building content per Marketing Plan Phase 5. | Social connector (Twitter/X, LinkedIn) for scheduling once approved — none needed for drafting. | Weekly/bi-weekly | **Approval-gated** — queued, never auto-posted |
| 5 | Email triage | Categorize incoming mail to `hello@peregrin.travel` (partner inquiry / support / spam), draft suggested replies. | Gmail/Google Workspace connector under `lc@peregrin.travel`. | Daily | Triage automatic; **any reply approval-gated** |
| 6 | Invoicing | An existing `Liam_Conroy_Invoicing_System` Google Sheet and `Invoicing` folder already exist in Drive (found during tonight's audit, not yet reviewed in detail) — automate generating invoices for confirmed B2B/consulting work against that system. | Google Sheets connector; Stripe (already integrated) for payment status. | As-needed / monthly | Generation automatic; **sending to a client approval-gated** |
| 7 | Duffel/Stripe status monitoring | Check Duffel go-live application status, Stripe test-balance/account state, flag anything needing action. | None required (already-authenticated API access from `/site`). | Weekly | Fully automatic (digest only) |
| 8 | STATE.md discipline check | Nudge/remind if a work session ended without `STATE.md` being updated, so the Cowork↔Code handoff doesn't silently rot. | None | Per-session or weekly | Fully automatic (reminder only) |

## Recommended build order

1. **#7 (Duffel/Stripe monitoring)** and **#8 (STATE.md check)** — trivial to build, zero risk,
   immediately useful, no new connectors needed.
2. **#2 (competitor monitoring)** — same reasoning, no connector dependency, directly informs the
   marketing plan.
3. **#1 (BD prospect research)** — highest leverage given Pipeline B currently has zero send-ready
   copy; research can run ahead of that gap closing.
4. **#5 (email triage)** — needs a Gmail connector approved for `lc@peregrin.travel` first.
5. **#3 and #4 (content/social drafting)** — highest ongoing value per the marketing plan, but
   lowest urgency right now since Pipeline B outreach copy is a bigger current gap.
6. **#6 (invoicing)** — lowest priority until there's an actual client/partner to invoice; worth
   reviewing what already exists in the Drive sheet first rather than building blind.

Not proposing anything for paid ad automation — the marketing plan explicitly deprioritises paid
acquisition for now.
