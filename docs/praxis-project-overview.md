# Praxis Developer – Project Overview

## What Praxis Developer Is

Praxis Developer is a web app that lets a developer:

- Describe a feature they want (a "FeatureTask").
- Run a fixed GMAD workflow:
  - Research → PRD → Build → Review → Sync
- End up with a GitHub Pull Request containing the code changes for that feature.

The core promise: **go from idea to draft PR in about 30 minutes** with minimal manual glue work.

## Target users

- Solo developers, startups, and small teams.
- They already have code in GitHub.
- They are comfortable reviewing PRs and merging code, but want help with:
  - Researching patterns and best practices.
  - Writing a clear PRD.
  - Generating boilerplate and wiring code.
  - Creating high‑quality PRs consistently.

## High-level flow

1. User creates a FeatureTask:
   - Title, description, target repo/branch.
2. Praxis runs the GMAD workflow:
   - Research: understand context and constraints.
   - PRD: write a concise, implementable spec.
   - Build: propose implementation plan and code changes.
   - Review: run adversarial review and prepare PR.
   - Sync: create branch, commit, push, and open GitHub PR.
3. User reviews the PR on GitHub and merges or requests changes.

The UI focuses on:
- A dashboard of FeatureTasks.
- A detail page showing the GMAD stepper and outputs.
- A right‑hand activity log of agent actions and events.

## Out of scope (MVP)

- Custom workflows (GMAD is fixed and linear).
- Multi‑repo features in one workflow.
- Deep team collaboration features (roles, comments, etc.).
- Non‑GitHub VCS.

The MVP is about proving that **hardcoded GMAD → PR** is valuable and reliable.
