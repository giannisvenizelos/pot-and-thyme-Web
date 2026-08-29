# PoT & Thyme TEST

Recovered source snapshot of the live [PoT & Thyme TEST](https://pot-and-thyme-test.vercel.app/) application.

## What is included

- Complete publicly served frontend (HTML, CSS and JavaScript)
- Legal/privacy/storage pages
- Offline service worker
- Minimal local-run and Vercel configuration
- Detailed architecture and recovery notes in [`PROJECT.md`](./PROJECT.md)

## Run locally

```bash
npm run dev
```

Then open `http://localhost:3000`.

The UI loads locally, but features that call `/api/catalog`, `/api/recipe` or `/api/ai-fridge` require the unrecovered Vercel serverless functions. Supabase-backed features also depend on the existing remote project and its policies.

## Deployment

The root directory can be deployed as a static Vercel project. Before changing the production deployment, restore and verify the missing `/api/*` sources and all required environment variables.

## Recovery status

The GitHub repository was empty when this snapshot was created. Public deployment assets were recovered on 2026-08-29. Private backend implementation, database migrations and secrets are not exposed by a deployed website and could not be copied from it. See [`PROJECT.md`](./PROJECT.md) for the exact boundary and follow-up checklist.
