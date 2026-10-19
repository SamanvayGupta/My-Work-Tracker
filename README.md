# SOS — Samanvay Operating System

Responsive personal productivity app with a local-first mode and optional Supabase cloud account/sync.

## Run locally

Use a local server (recommended; required for predictable script loading and auth redirects):

```bash
python -m http.server 8000
```

Open http://localhost:8000. The app continues to work in local mode before Supabase is configured.

## Enable the backend (Supabase)

This version uses Supabase Auth plus a private JSONB workspace row per account. It is a real cloud database connection, while preserving the app's existing single-source-of-truth state model. It is not yet a normalized multi-table schema for each topic/task/session.

1. Create a project at https://supabase.com/.
2. In Supabase Dashboard, open **SQL Editor**, paste the contents of `supabase-setup.sql`, and run it.
3. Open **Project Settings → API** (or the API keys section) and copy the Project URL and publishable/anon key.
4. Edit `config.js` and replace `https://YOUR_PROJECT_REF.supabase.co` and `YOUR_SUPABASE_PUBLISHABLE_OR_ANON_KEY` with those values.
5. In **Authentication → URL Configuration**, add your hosted site's exact URL to the allowed redirect/site URLs. For local testing, add `http://localhost:8000`.
6. In **Authentication → Providers → Email**, decide whether email confirmation is required. If enabled, sign-up asks the user to confirm their email before signing in.
7. Run the app from `http://localhost:8000` or host the folder as a static site. Do not open it with `file://` for cloud-auth testing.
8. Click **Connect account** (or go to **Settings & Data → Cloud database & account**) and sign up/sign in.

### Important security rules

- Use only the browser-safe **publishable/anon** key in `config.js`; never put a `service_role` or secret key in frontend code.
- The SQL enables Row Level Security and policies restricting each account to its own `user_id` row.
- Keep your project URL and key configured correctly, and test with two separate accounts to confirm they cannot read one another's workspace.
- Cloud sync is debounced and keeps a local browser copy. A visible sync error means the latest edit may not have reached the cloud; export a backup and check the browser console/network and Supabase policies.
- Existing local data is uploaded when signing into a new account with no cloud workspace. If a cloud workspace already exists, the app prompts whether to load cloud data or replace it with the current browser data. Export a JSON backup before choosing.

## What syncs

The app stores courses, topics, tasks, study sessions, reviews, milestones, projects, journal entries, career checklist and settings together in `sos_workspaces.data`. The browser continues to keep a local copy for resilience. Signing into the same account on another device loads the cloud workspace.

The focus timer's transient countdown is still kept locally in that browser; the actual study session must be explicitly logged. This version does not yet implement live real-time collaboration or conflict merging between two devices edited simultaneously; the most recent workspace save wins. For a single-user personal planner, use one active editing device at a time until conflict handling is added.

## Architecture note

The backend is connected to PostgreSQL through Supabase, with one JSONB document per authenticated user. This is a pragmatic migration for the existing single-file app and enables account-based cloud persistence. If the app grows, the next architectural step is to migrate the JSONB workspace into normalized relational tables (`courses`, `topics`, `tasks`, `study_sessions`, etc.) and use transactions/migrations for individual records.

## Existing features

- Command Center, Topic Planner and bulk topic entry
- Daily tasks, priorities and rescheduling
- Course Tracker and lecture progress
- Study session logging and focus timer
- Weekly reviews and monthly milestones
- Projects & Career, placement decision log and readiness checklist
- Reflection journal, Minimum Viable Day settings
- CSV exports, JSON backup/restore, dark/light mode

## Before production

Test signup/signin, email confirmation, persistence after refresh, same-account access on another device, sign-out behavior, two-account isolation, failed-network behavior, and backup restore. Set sensible Supabase Auth rate limits and email settings. This bundle does not deploy itself or create your Supabase project automatically; you must complete the dashboard setup above.
