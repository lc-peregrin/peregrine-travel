# Claude Code — run all (2026-07-24, rev 2)

Do these in order. Work in `/site`, read `site/CLAUDE.md` first. Run `npm test` after each task,
commit each task separately, push `main`, and update `STATE.md` at the end. If a task is blocked,
do the others and report which was skipped and why.

## 1. Privacy page  (READY — do now)
Follow `automation/CLAUDE_CODE_HANDOFF_2026-07-24_DESIGN_AND_PRIVACY.md` → TASK A.
Add a `/privacy` route serving the text in `automation/PRIVACY_POLICY_FINAL.md`, styled in the site
design system, with a "Privacy Policy" footer link on every page. Localise chrome en/es/ru/hi; body
English is fine. Commit + push.

## 2. Ticket conversion + commission  (BUILD BEHIND FLAG)
Follow `automation/CLAUDE_CODE_HANDOFF_2026-07-24_TICKET_CONVERSION_COMMISSION.md` in full.
Build behind `ENABLE_TICKET_CONVERSION=false` — do NOT enable in production. Commit + push flag off.

## 3. Design polish  (BUILD FROM THE BRIEF — no export file needed)
Follow `automation/CLAUDE_CODE_HANDOFF_2026-07-24_DESIGN_AND_PRIVACY.md` → TASK B.
Build the five conversion sections' styling DIRECTLY from that handoff + the live CSS tokens/webfonts
already in `site/public/index.html`. The `.dc.html` export is NOT required — if
`design-exports/Peregrin Conversion Sections.dc.html` happens to be present, use it as an exact-CSS
reference; if not, implement from the brief. Do NOT skip this task for lack of the export.
Key points (from the design pass): sections 1–3 reuse live class names/copy/`data-i18n` keys →
CSS-only + one icon element per persona and per pillar card; fill the missing `.section-h` /
`.section-sub` CSS; section 3 as icon-left rows (distinct from the persona grid) keeping the gold
disclosure ribbon; section 4 a sample modal opening from the existing "See a sample reservation"
links with a diagonal low-opacity SAMPLE watermark over a placeholder document; section 5
testimonials PLACEHOLDER only (no invented quotes/ratings). No new brand colours; nothing resembling
a government seal; do not touch the search tool, checkout, PDF, or pricing. Commit + push.

## Notes
- Do NOT wire the full Terms & Conditions / Privacy *drafts* (entity placeholders + need legal review).
  Only the finalised interim Privacy in task 1.
