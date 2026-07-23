# Peregrin Travel — state

Last updated: 2026-07-23, end of tonight's Cowork session (project consolidation).

## Done

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

- Step 5 of tonight's setup (git init for this outer folder, GitHub repo, Vercel connection for
  `/site`) — see "Open decisions" below, this needs a call before proceeding.
- Automation roadmap (Step 6) — not yet drafted, next up.

## Next

1. Finish git/GitHub setup for the outer `peregrine-travel` folder (docs/automation, not `/site`
   itself — see open decision #1).
2. Propose automation roadmap for approval (content drafting, BD prospecting, competitor
   monitoring, social drafts, email triage, invoicing) — don't build until approved.
3. Decide what happens to the two BD pipelines (see `docs/BD_PIPELINE.md`) — Pipeline A (old
   supply-outreach, 33 agencies) is largely obsolete under the current strategy; Pipeline B
   (14 B2B/affiliate targets) is the live strategy but has zero outreach copy written yet.
4. Real logo asset doesn't exist — worth a Claude Design session once that tool is picked up.
5. Task #43 equivalent (email delivery) is code-complete and was pushed tonight — worth a live
   re-test now that the domain fix is deployed.

## Blockers

- Duffel go-live (production API keys) — application submitted, approval status unknown, no
  fallback path if rejected.
- Duffel Stays access — separate request submitted, accommodation flow blocked on it.
- Real Stripe account is only sandboxed/test-mode; going live needs Stripe's business
  verification flow completed (not started).

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
   `docs/BD_PIPELINE.md`.
3. **Google Drive / email identity** — confirmed business content is correctly under
   `lc@peregrin.travel`; worth deciding if Liam wants his default browser profile / Drive account
   on this Mac switched to that identity permanently, or just used situationally.
4. **Claude Code setup** — discussed, not yet installed. Liam chose to set up the Cowork Project
   route first tonight instead.
