# PoT & Thyme TEST

Two independently deployable browser applications with shared data modules.

| Edition | Vercel project | Root Directory | Build command | Output directory |
| --- | --- | --- | --- | --- |
| Web | pot-and-thyme-test | repository root | npm run build:web | apps/web/public |
| Mobile | pot-and-thyme-test-mobile | apps/mobile | npm run build | public |

The existing web URL remains `https://pot-and-thyme-test.vercel.app/`. The dedicated mobile URL is `https://pot-and-thyme-test-mobile.vercel.app/`.

## Source layout

- `apps/web/`: web entry point, stylesheet and renderer; does not load mobile modules.
- `apps/mobile/`: mobile entry point, stylesheets and renderer. The mobile presentation remains active at every viewport width, including phone rotation.
- `shared/`: data/authentication modules, feature helpers, legal pages and image assets.
- `scripts/build.mjs`: dependency-free build, copies shared and edition assets and generates a separate versioned offline shell for each edition.

Both apps use the existing Supabase project and accounts. Browser login sessions, offline caches and themes are stored separately on each origin; users sign in separately on each URL. There is no automatic device redirect.

## Local use

```bash
npm run build:all
npm run dev          # web, port 3000
npm run dev:mobile   # mobile, port 3001
```

Build output is generated and ignored by Git. Edit source files rather than `public/`.

## Git deployments

Connect both Vercel projects to this repository and use the settings above. For a mobile project rooted at `apps/mobile`, enable access to source files outside the Root Directory so the build can read `shared/` and `scripts/`. Alternatively keep its Root Directory at the repository root, set Build Command to `npm run build:mobile`, and Output Directory to `apps/mobile/public`.

## Existing backend limitation

This repository was recovered from public frontend assets on 2026-08-29. Serverless `/api/*` source, database migrations and private environment variables are absent. `/api/catalog` returned HTTP 404 on the existing deployment during the split. Catalogue and recipe details retain their existing direct Supabase fallback. AI fridge search still requires the missing `/api/ai-fridge` endpoint; splitting the frontends does not restore it. See `PROJECT.md` for recovery provenance.
