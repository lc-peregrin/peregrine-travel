# Claude Code handoff — 2026-07-24 overnight session

Paste the prompt below into Claude Code, run from `~/Projects/peregrine-travel/site`.

## Guardrails (read first, non-negotiable)

- Work on a branch prefixed `claude/` — never commit or push directly to `main`. Open the changes
  for Liam to review in the morning; don't merge them yourself.
- No pricing changes, no new payment/subscription logic, no new customer-facing features, no
  changes to what the site charges or how. This session is about safety and code quality, not
  product changes — those need Liam's sign-off first (see the research doc from tonight for
  candidates).
- Don't touch `.env`, API keys, or Vercel/Stripe/Duffel account settings.
- If you hit a decision that materially changes behavior a customer would notice, stop and leave a
  note instead of guessing.

## The prompt

Structured as instructions / context / task / output format, per the four-block pattern —
worth using this shape for any cold, complex ask from here on.

```
INSTRUCTIONS
Read CLAUDE.md in both this directory and the parent directory (~/Projects/peregrine-travel/CLAUDE.md),
plus STATE.md in the parent directory, before touching anything. Work only on a new branch named
claude/overnight-safety-pass — never commit or push to main. This is a safety/quality pass, not a
product change: no pricing changes, no new payment or subscription logic, no new customer-facing
features, no UI copy changes. Don't touch .env, API keys, or any Vercel/Stripe/Duffel account
settings. If a fix would materially change behavior a customer would notice, don't guess — log it
in NOTES-FOR-LIAM.md at the repo root instead of making the change.

CONTEXT
Tonight (2026-07-24) a real production bug shipped: three places in public/index.html referenced
translations[lang], assuming a global `lang` variable that never existed (it's only ever a function
parameter of applyLang(lang) elsewhere in the file). This threw a silent ReferenceError that broke
the confirmation screen for every successful hold in production, and was only caught by accident
during manual QA — no automated check would have caught it. This is a single-file, no-build-step
app (public/index.html has the entire frontend inline; server.js is the backend) — don't introduce
a heavy test framework or a build step.

TASK
Make this class of bug — a reference to something not actually in scope, shipped silently — 
structurally harder to ship again, without changing what the product does:
1. Add a lightweight test harness (plain Node --test or similarly minimal) that extracts and
   syntax-checks the inline <script> block on every run, and exercises renderOrder() (and any other
   DOM-rendering function) with representative mock order objects covering all 4 languages, asserting
   no throw and that key elements (booking-ref, stripe-pay-btn, confirmation-title) end up populated.
   If there's a reasonable way to statically catch "used but never declared" in the extracted script,
   add that too.
2. Read through public/index.html specifically hunting for other instances of the same pattern
   (assumed global that's actually only a function parameter, or any other undefined reference).
   Fix anything found using the actual source of truth (e.g. localStorage.getItem("peregrin_lang")
   || "en"), the same way the lang bug was fixed.
3. Add a short README.md to site/ — project structure, how to run locally, where the booking flow
   lives, pointer to CLAUDE.md for the gotchas list. Orientation only, don't duplicate CLAUDE.md.
4. Add an `npm test` script that runs the new harness, and document it in CLAUDE.md's "Local dev"
   section.

OUTPUT FORMAT
A pushed branch (claude/overnight-safety-pass, not main) containing the changes, plus a written
summary covering: what the test harness checks, the full list of anything found and fixed in the
step-2 audit (state explicitly if the list is empty), and the contents of NOTES-FOR-LIAM.md if you
created one. Do not merge or deploy — leave it ready for review.
```

## Why this scope and not the bigger ask

Liam's original ask tonight also included implementing new revenue features, a subscription model,
and researching adjacent business ideas to build "overnight." That research is being done by Cowork
tonight and written up separately (see `docs/reference/` for the output once ready) — but none of
it should be code Claude Code ships unsupervised, because it's all product/pricing decisions that
need Liam's review first, not implementation work with a single correct answer. The scope above is
the subset of tonight's ask that's genuinely safe to run unattended: it only adds tests, docs, and
bug-pattern fixes matching a bug that already shipped and was already fixed once — it doesn't change
what the product does or charges.
