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

## Logo — closed 2026-07-23

Real exported assets now exist in `/design-exports` and `site/public/`, built from the wing-mark
motif already live and approved on the site (not redesigned from scratch, to avoid drifting from
what Liam already signed off on):

- `peregrin-mark.svg` / `peregrin-mark-512.png` — square icon mark, dark background, for app-icon
  contexts.
- `peregrin-mark-transparent.svg` / `-512.png` — same mark, transparent background, for placement
  on colored surfaces.
- `peregrin-lockup.svg` / `.png` — icon + "PEREGRIN" wordmark, horizontal lockup.
- `peregrin-og-image.svg` / `.png` — 1200×630 social share preview.
- `site/public/favicon.ico`, `favicon-32.png`, `favicon-16.png`, `apple-touch-icon.png` — wired
  into `site/public/index.html`'s `<head>`, verified live (served 200, correct tags present in
  rendered HTML).

One real design decision made along the way: the full 3-stroke mark doesn't read at 16×16 (blurs
into an indistinct blob) — a simplified 2-stroke version was built specifically for the smallest
favicon sizes, while 32px+ and all other uses keep the full 3-stroke mark. Also fixed a rendering
bug where the open-path strokes needed explicit `fill="none"` — without it, some renderers
(confirmed with ImageMagick; browsers tolerate it silently) fill the implied closed shape black,
producing dark slivers between the strokes.

Still true: this was built directly rather than through Claude Design, so if a from-scratch
brand refresh is ever wanted, that's still the right tool for it — but the "no logo asset exists"
gap itself is closed.

Not yet done: a real wordmark font choice (currently system font, bold), and app icon variants for
any future native app.

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
