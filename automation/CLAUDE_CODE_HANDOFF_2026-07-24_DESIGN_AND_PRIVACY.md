# Claude Code handoff — integrate design polish + add Privacy page (2026-07-24)

Two tasks. Work in `/site`, read `site/CLAUDE.md` first. Commit each task separately, run `npm test`,
push `main`, update `STATE.md` at the end.

## TASK A — Privacy page (do this first; it's self-contained and ready)
- Add a `/privacy` route (Express) serving a Privacy Policy page.
- Content: use the exact text in `automation/PRIVACY_POLICY_FINAL.md` (operator "Liam Conroy",
  contact hello@peregrin.travel, last updated 24 July 2026).
- Style the page with the existing site design system (same header/footer/type/tokens as the rest of
  the site) — a simple readable legal-page layout.
- Add a **"Privacy Policy" link in the footer** next to the existing disclaimer, on every page.
- Localise the page chrome (nav/footer) across en/es/ru/hi; the policy body can stay English for now.
- Optional: mark the page `noindex` (fine either way).
- Commit + push.

## TASK B — Integrate the conversion-sections design polish
BUILD DIRECTLY FROM THIS BRIEF — the `.dc.html` export is optional. If
`design-exports/Peregrin Conversion Sections.dc.html` is present, use it as an exact-CSS reference and
strip its notes panel on integration; if it is NOT present, implement the sections from this brief plus
the live tokens/webfonts already in `site/public/index.html`. Do NOT skip this task for lack of the
export. Implement per these points:
- Sections 1–3 reuse the live class names / copy / `data-i18n` keys → integration is CSS-only, plus
  one added icon element per persona card and per pillar card (markup shown in the export).
- Fill the real gap the export flagged: `.section-h` / `.section-sub` are used in the live markup but
  have no CSS live — add the supplied rules.
- Section 3 (trust pillars) rebuilt as icon-left rows (distinct from the persona grid); keep the gold
  disclosure ribbon.
- Section 4: sample-reservation modal opening from the EXISTING "See a sample reservation" links, with
  the diagonal low-opacity SAMPLE watermark over the placeholder document. Swap in the real sample
  asset if available; otherwise keep the placeholder.
- Section 5: testimonials section is PLACEHOLDER ONLY — do not invent quotes or ratings.
- Reuse existing tokens/webfonts. Do NOT touch the search tool, checkout, PDF, or pricing. No new
  brand colours. Nothing resembling an official government seal.
- Keep/extend tests as needed; run `npm test`; commit + push; update `STATE.md`.

## Notes
- All prior copy (hero, pillars, personas, embassy section, disclaimer) is already live from Prompt 1
  — this pass only styles it and adds the two new sections (4 modal, 5 testimonials placeholder).
- Do NOT wire the full Terms & Conditions / Privacy *drafts* with entity placeholders — only the
  finalised interim Privacy in Task A. Terms waits on the entity decision + legal review.
