# Dana Point

Knowledge base for **Dana Point** (dana-point.42init.com), built on the
[sekai-kb](https://github.com/wilsonkichoi/sekai-kb) framework. Open-source, AI-friendly knowledge base for Dana Point.

> This file is **instance-owned** (`merge=ours` in `.gitattributes`): framework
> upgrades never overwrite it. Edit it freely to describe your instance.

## Where things live

- **Place identity (the one file to edit):** `place.config.ts` — name, tagline,
  domain, categories, map, feature toggles, links, and home-page copy.
- **Content (single source of truth):** `knowledge/{Category}/*.md` — plain Markdown.
  Everything the site renders is derived from this at build time.
- **Media:** `public/media/` and other `public/` assets.
- **Architecture diagrams (engineering SSOT):** `docs/diagrams/*.drawio`.
- **Engineering rules:** `.claude/rules/` — framework-owned lessons that keep the
  build green (Astro/Vite gotchas, prebuild ordering, shell portability, lockfile).

## How the site builds

`knowledge/` → `sync.sh` → parallel prebuild (kb-index, search, content-dates,
git-info, related, changelog, map-markers, dashboard-lite) → `astro build` →
post-build contract checks. `src/content/` and `src/data/` are derived, gitignored
projections of `knowledge/` — never edit them directly.

## Iron rules

1. **SSOT:** `knowledge/` is the only content source of truth; `src/content/` is
   derived (gitignored, written by sync) and never edited directly.
2. **Genericity + English-only:** zero place-specific strings and zero CJK/multi-language
   code paths in any code tree — `src/`, `scripts/`, `tests/`, future `workers/`/plugin
   code; test fixtures are code. Place identity flows only from `place.config.ts` +
   `knowledge/` + `public/media/`. Machine-gated by `npm run genericity`
   (`scripts/ci/check-genericity.sh` + `scripts/ci/check-english-only.mjs`).
3. **Framework vs instance:** `src/` and `scripts/` are framework-owned — customize
   through config, content, and media. Anything more is upstreamed to sekai-kb and
   pulled back as a tagged release. The genericity gate is the structural guarantee.
