# Praxis Developer – Database and Supabase Protocol

Praxis uses Supabase Postgres as its database, accessed exclusively via Prisma.

## ORM and database

- ORM: Prisma
- Prisma datasource provider: `postgresql`
- Main models (MVP):
  - `FeatureTask`
  - `GMADStepOutput`

## Prisma schema

Core of `prisma/schema.prisma`:

```prisma
datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

generator client {
  provider = "prisma-client-js"
}

model FeatureTask {
  id          Int                @id @default(autoincrement())
  title       String
  description String
  status      FeatureTaskStatus  @default(RESEARCH)
  repoRef     String

  createdAt   DateTime           @default(now())
  updatedAt   DateTime           @updatedAt

  steps       GMADStepOutput[]
}

enum FeatureTaskStatus {
  RESEARCH
  PRD
  BUILD
  REVIEW
  SYNC
  DONE
  FAILED
}

model GMADStepOutput {
  id            Int            @id @default(autoincrement())
  featureTask   FeatureTask    @relation(fields: [featureTaskId], references: [id])
  featureTaskId Int

  step          GMADStep
  content       Json
  approved      Boolean        @default(false)
  approvedAt    DateTime?
  metadata      Json?
  createdAt     DateTime       @default(now())
  updatedAt     DateTime       @updatedAt
}

enum GMADStep {
  RESEARCH
  PRD
  BUILD
  REVIEW
  SYNC
}
```

## Environment strategy

Local development:
- `.env` at repo root (git-ignored) contains a Postgres `DATABASE_URL` (local Postgres or a Supabase dev instance).

Example:

```env
DATABASE_URL="postgresql://user:pass@localhost:5432/praxis_dev?schema=public"
```

Vercel (preview & production):
- `DATABASE_URL` is set via Vercel project environment variables.
- In staging/production, `DATABASE_URL` points to Supabase Postgres.

The same Prisma schema is used in all environments; only `DATABASE_URL` changes.

## Migration protocol

- All schema changes flow through Prisma migrations.
- Local development:
  - `npx prisma migrate dev --name <migration-name>`
- Deployed environments (Supabase Postgres):
  - `npx prisma migrate deploy` (run via CI/CD or Vercel build step if configured).

Agent0 must:
- Treat Supabase strictly as "Postgres behind Prisma".
- Not attempt to run Supabase migrations or admin operations directly.
- Not modify `DATABASE_URL` env vars itself; instead, expect them to be configured in Vercel for each environment.
