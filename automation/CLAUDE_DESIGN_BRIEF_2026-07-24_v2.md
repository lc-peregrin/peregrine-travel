# Claude Design brief — site evolution v2 (2026-07-24)

Evolution of the existing live site, NOT a redesign. Keep it beautiful and unmistakably the
same product, with minimal changes so Claude Code integration stays surgical.

## Study first (current baseline)
- Live site: https://www.peregrin.travel and FAQ https://www.peregrin.travel/faq
- ../docs/BRAND.md (voice, colour tokens, typography, wing-mark)
- ../CLAUDE.md and ../docs/MARKETING_PLAN.md (§3 SEO strategy)
- Approved copy: ../automation/FAQ_AND_TRUST_COPY_2026-07-24.md

## Hard constraints (so integration needs no major code changes)
- Reuse the EXISTING design system exactly: palette tokens (--accent #1c6f8c, --accent-dark
  #124a5e, --gold #c9922e, --ink #16283a, --success #1f7a5c, tints), Source Serif 4 + Public
  Sans, the wing-mark motif, generous whitespace, calm tone. No stock photography.
- Single-file frontend (public/index.html — inline HTML/CSS/JS, no framework, no build).
  Design so specs map onto that existing markup; reuse current components/classes/spacing.
  Goal: small, surgical diffs.
- Keep homepage direction 1a: the search tool stays the hero, above the fold.
- Works across all four live languages (en/es/ru/hi).

## What to change
1. Homepage — surface price + trust, tool stays hero:
   - Clear, tasteful flat-fee line near the hero/search: "One flat fee — US$14.99 (US$19.99
     return). No airfare, no hidden charges." Visible without scrolling.
   - Strengthen the existing footer trust row ("Real reservations · Independently verifiable ·
     Delivered in minutes · Secured by Stripe") into a more prominent on-brand trust band;
     keep the "held reservation, not a ticket" disclosure visible.
2. Header / nav: make "Help" an obvious, clearly-styled, working link to /faq, plus the
   language switcher. It must read as a real destination — currently easy to miss.
3. FAQ / Help page (/faq): already good — refine only for consistency; keep the pricing
   callout, How-it-works, and trust pillars.
4. SEO programmatic landing-page template (new, reusable):
   - ONE reusable country/visa template: H1 "Proof of onward travel for {{Country}}", intro,
     How-it-works block, trust signals, short FAQ block, internal links (tool + /faq), and a
     prominent CTA into the search tool.
   - Use {{ tokens }} for all variable content. Do NOT write real visa/immigration text —
     content is supplied separately; template + structure only.
   - SEO structure: clean semantic headings (one H1, logical H2/H3), title + meta-description
     pattern, lightweight markup (no heavy assets) so pages stay fast.

## Deliverables (export to ../design-exports/)
- Homepage refinement, header/nav, FAQ polish, and SEO template as .dc.html mockups.
- RATIONALE.md: for each change, note exactly how it maps onto the existing index.html (which
  component/section it modifies) so Claude Code integrates with minimal edits. Flag anything
  needing more than a small change.

## Voice
Legal-framed and reassuring; always "a real, verifiable held reservation, not a ticket";
never "fake" or "dummy".
