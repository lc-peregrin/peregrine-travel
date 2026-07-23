# Peregrin Travel — state

Last updated: 2026-07-23, end of tonight's Cowork session (project consolidation + site-readiness
sequencing decision).

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

- Nothing actively in progress — tonight's consolidation is fully wrapped. `peregrine-travel` repo
  is live on GitHub (`lc-peregrin/peregrine-travel`), pushed, `liamconroy96-cell` has accepted
  collaborator access.

## Next — sequencing decision made 2026-07-23

**Liam's explicit call: hold BD outreach (both pipelines) until the site itself is properly
polished and "clean looking."** Don't start on `docs/BD_PIPELINE.md` open decisions until this
readiness list is closed out. Site-readiness checklist, in rough priority order:

1. ~~**Real logo/brand asset**~~ — **done 2026-07-23.** Full asset set built and wired in:
   favicon (with a separate simplified mark for 16px legibility), apple-touch-icon, OG/social
   preview image, transparent + solid mark variants, wordmark lockup. All in `/design-exports` and
   `site/public/`, verified live via local server (200s, correct tags in rendered HTML). See
   `docs/BRAND.md` for details, including a real rendering bug that was caught and fixed
   (open-path strokes need explicit `fill="none"` or some renderers fill them black).
2. **Visual QA on tonight's changes** — the Stripe "Pay with card" button and the hold-button
   double-submit fix were built/tested functionally but not re-screenshotted across the full flow
   and all 4 languages the way the original redesign was. Worth a fresh pass before calling the
   site "clean."
3. **Live re-test of email delivery** — code fix (verified `send.peregrin.travel` domain) was
   pushed tonight but not re-confirmed live end-to-end since.
4. **Favicon / link preview polish** — no favicon, no Open Graph image confirmed, both visible the
   moment someone shares or bookmarks the link.
5. Not blocking "clean looking" but worth knowing: Duffel is still test-mode (go-live pending) and
   Stripe is still test/sandbox-mode (business verification not started) — these affect whether
   the site can take real money/issue real tickets, not how it looks, so lower priority for this
   specific readiness push but will matter before real BD outreach converts into paying partners.

Once this list is closed, return to `docs/BD_PIPELINE.md`'s open decisions (Pipeline A fate,
Pipeline B outreach copy).

Automation roadmap (`automation/ROADMAP_PROPOSAL.md`) is written and ranked, waiting on Liam's
go-ahead — independent of the BD-hold decision, could be picked up in parallel if useful.

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
   `docs/BD_PIPELINE.md`. **Deferred** — Liam wants the site-readiness checklist above closed out
   before touching either BD pipeline.
3. **Google Drive / email identity** — confirmed business content is correctly under
   `lc@peregrin.travel`; worth deciding if Liam wants his default browser profile / Drive account
   on this Mac switched to that identity permanently, or just used situationally.
4. **Claude Code setup** — discussed, not yet installed. Liam chose to set up the Cowork Project
   route first tonight instead.
