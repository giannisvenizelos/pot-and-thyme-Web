# Project record

## Purpose

PoT & Thyme TEST is a Greek-language meal-planning web application. It combines a recipe catalogue, household meal plans, an aggregated shopping list, community recipe submissions, moderation, account/privacy controls and an AI-assisted “what is in my fridge?” flow.

## Architecture

- **Frontend:** dependency-free HTML, CSS and browser JavaScript.
- **Hosting:** static files on Vercel, plus server-side `/api/*` endpoints in the deployed project.
- **Data and authentication:** Supabase project `ccvdkbdnykfhenhqkicm` using Auth, PostgREST RPCs and Realtime.
- **Offline support:** service worker with a versioned application-shell cache and IndexedDB catalogue/bootstrap cache.
- **Language/UI:** Greek interface; Google Fonts variable families `Literata` (display/serif) and `Inter` (UI/sans), both loaded with their optical-size axis. Both ship a Greek subset — the previous `Playfair Display`/`Cormorant Garamond` pairing did not, so Greek headings silently fell back to Georgia.

## Frontend modules

| File | Responsibility |
| --- | --- |
| `app1.js` | Shared state, Supabase client helpers, IndexedDB cache, catalogue loading and recipe details |
| `app2.js` | Authentication, household membership, meal plan, shopping state and Supabase Realtime |
| `app3.js` | Community recipe creation, recipe modal and moderation UI |
| `app4.js` | Main rendering, events, navigation and service-worker registration |
| `ai.js` | AI fridge search UI and `/api/ai-fridge` client |
| `legal.js` | Registration consent, legal/privacy controls, account export and deletion UI |
| `moderation-edit.js` | Editing pending community recipes |
| `account-settings.js` | Display-name and password management |
| `enhancements.js` | Additional UI/UX enhancements |
| `app.css` | Main responsive application styling |
| `legal-pages.css` | Shared legal-page styling |
| `sw.js` | Offline application-shell cache |

## External API surface used by the frontend

The frontend calls these Vercel endpoints:

- `GET /api/catalog`
- `GET /api/recipe?id=...`
- `POST /api/ai-fridge`

It also calls Supabase Auth, REST and Realtime endpoints. Supabase RPC names visible in the client include `create_household`, `join_household`, `get_app_bootstrap`, `remove_meal_plan_item`, `create_community_recipe`, `moderate_community_recipe`, `edit_pending_community_recipe`, `update_my_display_name`, `delete_my_account`, `export_my_data` and `get_current_legal_versions`.

## Recovery provenance and limits

This repository snapshot was reconstructed on 2026-08-29 from the public assets served by `https://pot-and-thyme-test.vercel.app/`. The repository was empty before the recovery.

The deployed frontend, service worker and public legal pages are included byte-for-byte as served. Vercel serverless function source, Supabase database schema/migrations, row-level-security policies, Edge Functions, seed data and private environment variables cannot be recovered from public browser assets and are therefore not included. The Supabase publishable key in `app1.js` is designed for browser use; authorization must remain enforced by Supabase RLS policies.

To make the project fully reproducible, export the missing backend sources from the original Vercel/Supabase projects and add them under `api/` and `supabase/` respectively.

## Suggested next repository structure

```text
.
├── api/                 # Vercel function sources (not recovered)
├── supabase/
│   ├── migrations/      # Database schema/RLS/RPC migrations (not recovered)
│   └── seed.sql         # Optional catalogue seed (not recovered)
├── *.html               # Static pages
├── *.js                 # Browser modules
├── *.css                # Styles
└── sw.js                # Service worker
```

## Security notes

- Never commit Supabase service-role keys, AI-provider keys or Vercel secrets.
- Keep privileged operations behind server-side endpoints or RLS-protected RPCs.
- Review the account deletion/export RPCs and moderation RPCs before production use.
- Treat this deployment as a recovery snapshot until backend sources and migrations are restored and reviewed.
