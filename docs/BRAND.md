# Peregrin — Brand

Status: **partial — real gaps flagged below.** This reflects what's actually been decided and
built, not an aspirational brand system.

## Voice & tone

Direction chosen (2026-07): **"Hybrid — warm but structured."** Concretely: plain, direct
language (no travel-marketing fluff), but not cold or purely corporate — the redesign leaned into
generous whitespace, a human wing-mark motif, and calm color rather than dense financial-services
styling. Messaging leads with the three trust signals validated by competitors: real/verifiable
reservation, transparency about hold-vs-ticket status, speed. See `MARKETING_PLAN.md` section 4
for the full messaging framework.

## Color palette

Taken directly from the live site's CSS (`site/public/index.html`), not a separately maintained
design file — this **is** the source of truth:

| Token | Hex | Use |
|---|---|---|
| `--ink` | `#16283a` | Primary text |
| `--muted` | `#5c6b7c` | Secondary text |
| `--line` | `#e2e7ec` | Borders/dividers |
| `--bg` | `#f8f9fb` | Page background |
| `--accent` | `#1c6f8c` | Primary brand blue/teal |
| `--accent-bg` | `#e8f2f5` | Accent tint background |
| `--accent-dark` | `#124a5e` | Accent hover/dark state |
| `--gold` | `#c9922e` | Secondary accent, used sparingly |
| `--gold-bg` | `#faf1e0` | Gold tint background |
| `--success` | `#1f7a5c` | Confirmed/ticketed states |
| `--success-bg` | `#e7f4ee` | Success tint background |

## Typography

System font stack, no custom webfont licensed or loaded:
`-apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, Arial, sans-serif`

## Logo — real gap

**No standalone logo asset file exists anywhere** (not in this repo, not in Drive, not in
uploads). What exists instead: an inline SVG "wing mark" (three curved strokes, accent blue +
gold) hand-coded directly into `site/public/index.html`, used as the header icon. It was designed
to riff on a mock logo image Liam shared once in chat as inspiration — that image was never saved
as a retrievable file, so it can't be reproduced or refined outside that one conversation.

**Open gap:** no exported PNG/SVG logo file, no favicon set, no wordmark treatment beyond the
plain "PEREGRIN" text in the header. If Claude Design gets used, this is the first thing worth
producing — export finished assets into `/design-exports`.

## Name & domain

- Brand name: **Peregrin**
- Domain: `peregrin.travel` (Namecheap)
- Sending subdomain: `send.peregrin.travel` (verified in Resend for transactional email)
- Root domain `peregrin.travel` deliberately left unverified in Resend to avoid disturbing its
  existing Google Workspace DKIM/DMARC setup.

## Open decision

Google Drive / browser identity split flagged 2026-07-23: business Drive content lives under
`lc@peregrin.travel` (confirmed — the real docs are there), separate from Liam's personal
`liam.conroy.96@gmail.com`. No brand impact, but worth keeping all future business-facing accounts
(Drive, email, socials) consistently under the `peregrin.travel` identity rather than personal.
