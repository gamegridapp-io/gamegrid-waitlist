# GameGrid — Waitlist Landing Page

Standalone marketing/waitlist page for the GameGrid pre-launch. A single static `index.html` — no build step, no framework, no dependencies.

## Deploy

Deploys directly to Vercel as a static site. Vercel auto-serves `index.html` from the repo root — no `vercel.json` config required.

## What it does

Captures sign-ups for the GameGrid waitlist. The form POSTs `full_name`, `email`, `discord`, `primary_sport`, `source` to the Supabase `email_list` table via the project's anon REST API.

- Fresh submission → HTTP 201 → success state shown
- Duplicate email → HTTP 409 → also treated as success ("already on the list")
- Network/Supabase error → inline error under the email field with retry

## Data target

- **Supabase project:** `nvpkibvadoalgzkavtvz`
- **Table:** `public.email_list`
- **Key in `index.html`:** legacy anon public JWT. Safe to ship in browser code — that's its purpose. (The newer `sb_publishable_*` key format had role-mapping issues on this project; legacy JWT is deterministic.)
- **Schema source of truth:** [`gamegridapp-io/gamegrid`](https://github.com/gamegridapp-io/gamegrid) → `supabase/migrations/0021_email_list.sql` (base) + `0022_email_list_waitlist_columns.sql` (extra fields + anon grants)

## Independent of the main app

This page intentionally bypasses the main Next.js app at `gamegridapp-io/gamegrid`. Separate deploy, separate Vercel project, separate repo. The page writes directly to Supabase so it doesn't depend on the main app being up — useful for early-access marketing while the main product is still pre-launch.
