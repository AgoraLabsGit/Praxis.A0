# Praxis Developer – Autonomy and Notifications

Goal: Praxis should be able to run GMAD → PR autonomously once GitHub, Vercel, and Supabase are correctly wired, while keeping a human in the loop via messaging.

## Autonomy target

Given a FeatureTask:

1. User creates or approves a FeatureTask in the Praxis UI.
2. Praxis backend advances through GMAD steps:
   - Calls Agent0 for each step.
   - Stores each step's output in Supabase via Prisma.
   - Updates FeatureTask status.
3. For the Sync step:
   - Creates a git branch.
   - Applies generated diffs.
   - Commits and pushes.
   - Opens a GitHub PR.
4. Sends the user a notification with:
   - Current step status.
   - PR URL.
   - Any questions requiring input.

This loop should work without the developer's laptop being online.

## Runtime separation

- Praxis app:
  - Runs on Vercel (Node runtime).
  - Talks to Supabase (Postgres) and GitHub.
  - Talks to Agent0 via HTTP.

- Agent0:
  - Runs in Docker on a VPS (e.g., Hostinger).
  - Exposes HTTP endpoints for GMAD operations.
  - Uses its own `.a0proj` for memory and knowledge.
  - Never directly touches Praxis repo or DB.

## Notifications (Telegram / WhatsApp)

Praxis will define a notification layer, for example:

- `NotificationService.notify(channel, message, metadata)`

Channels:
- Telegram bot (via Bot API).
- WhatsApp (via Twilio / WhatsApp Cloud API).

Typical events to notify:
- "Research complete – approve PRD?"
- "Build complete – review code?"
- "PR created – link: <url>"
- "Error in step X – summary: ..."

Agent0 may be allowed to trigger notifications by calling Praxis backend endpoints, but it never holds messaging credentials directly.
