# Praxis Developer MVP – Tech Stack and Hosting

## Core stack

Frontend:
- Next.js 14
- App Router
- TypeScript
- Tailwind CSS

Backend:
- Next.js server components + server actions / API routes (Node runtime, NOT Edge)
- Prisma ORM (provider = "postgresql")
- Supabase Postgres as the primary database

Infrastructure:
- GitHub for source control
- Vercel for hosting the Next.js app
- Supabase for Postgres (and later auth, storage, realtime)
- Agent0 running in Docker on a VPS (not on Vercel)

## Runtime responsibilities

Praxis (Vercel):
- Serves the web UI.
- Owns the application database via Prisma + Supabase.
- Implements the GMAD workflow endpoints and logic:
  - FeatureTask + GMADStepOutput CRUD.
  - Calls Agent0 for Research / PRD / Build / Review / Sync steps.
  - Integrates with GitHub to create branches, commits, and PRs.

Agent0 (Docker/VPS):
- Runs independently on a server (e.g., Hostinger VPS with Docker).
- Orchestrates tool usage and complex reasoning for GMAD steps.
- Interacts with Praxis via HTTP APIs.
- Does NOT touch the Praxis git repo or database directly.

Supabase:
- Provides the Postgres database for all Praxis application data.
- Prisma connects to Supabase using `DATABASE_URL` and migrations.
- Future: Supabase auth + RLS for multi-tenancy.

Vercel:
- Builds and deploys the Next.js app from the GitHub repo.
- Provides runtime environment variables (DATABASE_URL, SUPABASE_URL, GITHUB_TOKEN, etc.).
- No Agent0 processes run inside Vercel.
