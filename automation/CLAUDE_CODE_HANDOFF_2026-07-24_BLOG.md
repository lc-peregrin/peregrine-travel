# Claude Code handoff — Peregrin blog (beautiful, SEO-first)

Goal: a beautiful, fast, SEO-optimised blog that publishes the visa / onward-travel guides so they
rank on Google and funnel high-intent readers into the reservation tool. This is THE traffic engine
(mirrors how onwardticket.com/blog drives their business). Highest-priority growth build.

## Content source & format
- Articles are markdown files in `site/content/blog/*.md`, one per article, YAML front-matter:
  `title, description, slug, keyword, date, lang, readingTime`.
- Two launch articles are ALREADY in place: `proof-of-onward-travel-thailand.md`,
  `proof-of-onward-travel-bali-indonesia.md`. Launch the blog with these two.
- New articles = drop a new `.md` file here (from an approved Notion guide), same front-matter.
- Render markdown -> HTML server-side with a small maintained lib (`marked` or `markdown-it`),
  sanitised. No heavy framework.

## Routes
- `/blog` — index: all articles, newest first, as clean cards (title, excerpt, reading time,
  destination chip).
- `/blog/:slug` — article page (renders the markdown body).
- Add both to `sitemap.xml` (article lastmod = front-matter date); ensure `robots.txt` allows /blog.
- Link `/blog` from the site footer (e.g. "Guides") and ideally a header nav item, so it's
  discoverable and passes internal link equity to/from the homepage.

## Design — make it genuinely beautiful (use the existing design system; NO new brand colours)
- Typography-forward reading experience. Headings in the site serif (Source Serif 4), body in Public
  Sans. Comfortable measure (~66ch), line-height ~1.7, body 18-19px.
- Index: elegant editorial grid/list. Each card: serif title, one-line excerpt in `--muted`, a small
  reading-time + destination chip in `--accent`, generous whitespace, subtle hover lift. Tasteful use
  of the wing-mark.
- Article page: large serif H1, meta line (date · reading time), then the body with well-styled
  h2/h3, lists, and a refined **blockquote / pull-quote** treatment (the in-line "this is where
  Peregrin fits" quote must read as a considered pull-quote, not a code block). Breadcrumb
  (Home > Guides > Title) and a back-to-all-guides link.
- End-of-article CTA block: a prominent on-brand card — "Need proof of onward travel? Get a
  verifiable reservation in minutes." with a button to the homepage / search tool. Keep the subtle
  inline text CTA where the article already mentions Peregrin.
- Fully responsive, fast, consistent in light/dark if the site supports it. Reuse header/footer.

## SEO (this is the whole point — do it properly)
- Per article: `<title>` = front-matter title; meta description; canonical to
  `${SITE_ORIGIN}/blog/${slug}`; OG + Twitter card tags (title, description, site name; reuse
  `peregrin-og-image` as the default OG image).
- JSON-LD per article: `Article` (headline, description, datePublished from front-matter, author
  "Peregrin", publisher) + `BreadcrumbList`. On `/blog`, optionally `Blog` / `ItemList`.
- Clean semantic HTML — one `<h1>`, proper heading hierarchy, `<article>`. Mobile-first, fast load.

## Rules
- Don't touch the search tool, checkout, PDF, or pricing. Reuse tokens/fonts/wing-mark.
- Keep the legal noun rules in any blog chrome/CTA copy: core noun "reservation", never "fake";
  "ticket" only in "ticket reservation" / "e-ticket".
- Add tests: `/blog` and each `/blog/:slug` return 200 and include the title + canonical tag.
- Commit + push; update `STATE.md`.

## Nice-to-have (after the above)
- Related-guides links at the foot of each article (same region).
- A simple destinations filter on the index.
