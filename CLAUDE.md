# Peregrin Travel — shared context

Read by Claude Cowork, Claude Code, and (where applicable) Claude Design whenever working in this
folder. This is the single source of truth for what this project is, how it's structured, and how
to work on it. If something here conflicts with what you observe in the actual code or docs, trust
the code/docs and flag the conflict — this file can go stale, treat it as a briefing, not gospel.

## What Peregrin is

Peregrin sells travellers genuine, verifiable flight and accommodation reservations — real PNRs
held directly with an airline via Duffel's documented Hold Order API — to satisfy "proof of
onward/return travel" requirements at check-in, immigration, or in a visa application. Most
customers never intend to fly; the hold lapses automatically per the fare's own terms if unpaid.
Customers who do want to travel can pay to confirm before it expires. See `docs/BUSINESS_PLAN.md`
for the full model, unit economics, and market sizing, and `docs/MARKETING_PLAN.md` for
positioning and channel strategy.

## Tech stack & deployment

- `/site` — the actual product. Node.js + Express (`server.js`), single-file frontend
  (`public/index.html`, no build step, no framework), `pdfkit` for the reservation PDF, `stripe`
  for payment. **This is its own independent git repo** (remote: `github.com/lc-peregrin/peregrin-demo`),
  nested inside this folder but tracked separately — see "Git structure" below, don't merge its
  history into the outer repo.
- Deployed on Vercel (`peregrin-demo.vercel.app`), auto-deploys on push to `/site`'s `main` branch.
- Integrations: Duffel (flights, test-mode currently — go-live application submitted, not yet
  approved), Resend (transactional email, from `send.peregrin.travel`), Stripe (test mode).
- `/site/CLAUDE.md` has the detailed engineering context (gotchas, known bugs already fixed, API
  quirks) — read that too before touching code in `/site`.

## Folder map

```
peregrine-travel/
├── site/              — the live product (own git repo + Vercel deployment, see above)
├── app/                — mobile/app, not started
├── docs/
│   ├── BUSINESS_PLAN.md
│   ├── MARKETING_PLAN.md
│   ├── BRAND.md         — voice, colours, typography, logo status (real gap: no logo file exists)
│   ├── BD_PIPELINE.md   — partner/outreach pipeline status
│   └── reference/       — supporting research, old outreach drafts, target-list spreadsheets
├── design-exports/     — Claude Design output lands here, Claude Code integrates it into /site
├── automation/         — specs for scheduled/automated tasks (see docs for the approved roadmap)
├── CLAUDE.md            — this file
└── STATE.md             — session handoff, updated every session
```

## Brand rules

Full detail in `docs/BRAND.md`. Headline: colour palette and voice are already decided and live
in `/site`'s CSS (`--accent: #1c6f8c`, `--gold: #c9922e`, etc.) — treat that CSS as the source of
truth for colour, not a separate design file. Tone is "warm but structured": direct, no
travel-marketing fluff, but not cold/corporate either. **No logo asset file exists** — this is a
known, flagged gap, not an oversight.

## Working style (Liam)

Direct, concrete deliverables, no fluff. Prefers an explicit recommendation over open-ended
options — make a call, explain the one key reason, move forward. Comfortable with technical
detail but not a developer — plain-language explanations for anything requiring manual action
(Terminal commands, browser steps) should assume no coding background.

## Standing rules

1. **End-of-session handoff.** At the end of every session (Cowork or Code), update `STATE.md`
   with what was done, what's next, and any blockers. This is the only way context survives
   between surfaces and sessions — don't skip it, even for small sessions.
2. **Role division.**
   - **Cowork**: planning, research, marketing, BD, docs, connector-based automation
     (scheduled tasks, browser/dashboard work like Stripe/Vercel/DNS configuration).
   - **Claude Code**: all implementation, git operations, deployment.
   - **Claude Design**: visual design work; exports land in `/design-exports`, Claude Code
     integrates them into `/site`.
3. **No unapproved external communications.** Never send emails, social posts, or outreach of any
   kind without Liam's explicit approval first — draft and hold, always.
4. **Commit and push after meaningful changes.** Don't let real work sit uncommitted.

5. **Design outputs must reach the LOCAL repo before Claude Code runs.** Claude Design runs in
   a SEPARATE project and can only READ this repo, not write to it — so its mockups and
   `RATIONALE.md` do NOT appear in the local `/design-exports/` automatically. They must be
   brought across (Liam downloads them into the local `/design-exports/`, or Cowork writes them
   via the connected folder) and confirmed present on disk BEFORE Claude Code integrates. A
   design pass is NOT done until its files are verified in the LOCAL `/design-exports/`. (This
   has silently blocked Claude Code twice with 'missing' design files.)

## Open items worth knowing about

- Google Drive / account identity: business Drive content lives under `lc@peregrin.travel`
  (confirmed 2026-07-23) — keep using that identity for Peregrin-related Drive/email/browser work,
  not Liam's personal Gmail.
- Duffel go-live (production keys) is pending approval — everything currently runs in test mode.
- Duffel Stays access (separate from Flights) is pending — accommodation flow is built but
  blocked on this.
- See `STATE.md` for what's actively in progress right now.
