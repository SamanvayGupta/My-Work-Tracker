# SOS — Samanvay Operating System

A local-first, responsive personal productivity application based on the supplied SOS specification. The app is implemented as a self-contained HTML/CSS/JavaScript application so it can be run immediately without installing Node packages, configuring a database, or creating API keys.

## Run it

1. Extract the ZIP.
2. Open `index.html` in a modern browser.
3. If the browser restricts storage for `file://` pages, open a terminal in this folder and run `python -m http.server 8000`, then visit `http://localhost:8000`.

No internet connection or build step is required for the local app.

## Included modules

- **Command Center:** today's topics and tasks, recorded study time, deadline countdown, course progress, weekly pulse, priorities, recent sessions.
- **Topic Planner:** single and bulk topic creation, course/date/status filters, one-click completion, editing, deleting with confirmation, rescheduling by editing the existing record, ordering controls, overdue view, estimates.
- **Daily Planner:** task creation/edit/delete/duplicate, categories, priorities, status, scheduled dates, overdue highlighting, and confirmed carry-forward without duplication.
- **Course Tracker:** six preloaded courses, editable total and completed lecture minutes, course deadlines, topic history/search, independent topic and lecture progress.
- **Study Sessions & Focus Timer:** manual logging, edit/delete, 25/50/custom timer, pause/resume/stop, timer state persistence across refresh, optional linked topic, notes and practical-exercise flag. A timer does not automatically create study records or complete topics.
- **Weekly Reviews:** Monday–Sunday date grouping, daily topic counts, course breakdown, planned estimates vs actual logged sessions, saved reflection prompts.
- **Monthly Goals:** monthly topic counts, course breakdown, weekly breakdown, editable milestones with separate completion state.
- **Projects & Career:** project records, repository/deployment URLs, career-readiness checklist, placement decision log for fees, terms, risks, deadlines and evidence.
- **Reflection Journal:** dated entries, energy/focus ratings, search, edit and delete.
- **Settings & Data:** deadline and required pace, Minimum Viable Day settings, dark/light appearance, CSV exports for topics/tasks/sessions, JSON backup and restore, reset.

## Data and calculations

- The single `topics` collection is the source of truth for daily, weekly, monthly and course topic counts.
- The single `sessions` collection is the source of truth for actual study time.
- Topic estimates and task estimates are planned time, not actual time.
- Rescheduling updates the existing topic record and therefore moves its grouping automatically.
- Completion percentages are calculated from saved records; an empty period displays 0% without dividing by zero.
- Dates use local calendar dates for planning and Monday–Sunday week grouping.
- Existing browser data is migrated with default fields when the app version adds settings.

## Important deployment/security note

This is a **complete local-first functional build**, not a cloud-hosted multi-user production deployment. It stores data in the current browser profile via `localStorage`; data does not sync across devices and is not protected by account authentication. Supabase, authentication, database migrations, Row Level Security, and hosted deployment credentials are not configured in this environment, so the app does not pretend those features are active. Use **Settings & Data → Full JSON backup** regularly. The code is structured as a standalone local app to keep it immediately runnable; moving to the requested Next.js + TypeScript + Supabase production stack is a separate migration requiring Supabase project credentials and deployment access.

## Acceptance test checklist

1. Add ten newline-separated topics for one date and course.
2. Mark four complete; daily, weekly, monthly and course views should show 4/10 for the relevant grouping.
3. Refresh; the records and statuses should persist in the same browser.
4. Edit one topic's planned date; it should disappear from the old date and appear on the new date without duplicating its ID.
5. Log a study session; actual hours should update in session, daily, weekly and monthly summaries.
6. Edit/delete the session; derived actual-time totals should recalculate.
7. Mark a task complete and verify task totals; try carry-forward and confirm it updates existing records.
8. Export CSVs and a JSON backup; restore the backup only after confirmation.
9. Test at desktop and mobile widths; switch dark/light appearance in Settings.
