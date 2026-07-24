# Peregrin Travel — state

Last updated: 2026-07-24, end of tonight's Cowork session (production bug fix + domain live +
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
- Duffel Stays access — separate request submitted, accommodation flow blocked on it.
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
