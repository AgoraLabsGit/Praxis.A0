# Praxis Developer – GMAD Workflow and Agent0 Contract

## GMAD workflow (fixed, linear)

Praxis uses a fixed 5-step GMAD workflow:

1. Research
2. PRD
3. Build
4. Review
5. Sync

Each step produces a structured output stored in the database and may require user approval.

## Core entities

FeatureTask:
- Represents a single feature development request.
- Fields include:
  - id
  - title
  - description
  - status (research | prd | build | review | sync | done | failed)
  - repoRef (GitHub repo identifier)
  - timestamps

GMADStepOutput:
- Represents the output of a GMAD step for a given FeatureTask.
- Fields include:
  - featureTaskId
  - step (research | prd | build | review | sync)
  - content (JSON payload with step-specific structure)
  - approved (boolean)
  - approvedAt
  - metadata (JSON for things like branchName, prUrl, impacted files)
  - timestamps

## Agent0's role

Agent0 is used as a service to generate GMAD step outputs. It does NOT:

- Directly modify the database.
- Directly modify the git repo.
- Directly call GitHub or Vercel APIs.

Instead, Agent0:

- Receives structured inputs for a single step (Research, PRD, Build, Review, or Sync).
- Returns a structured output object matching the expected shape.
- Leaves persistence, git operations, and PR creation to the Praxis backend.

## Per-step input/output expectations

Research:
- Input:
  - feature title and description
  - relevant repo context summary
  - constraints
- Output:
  - problemSummary
  - contextSummary
  - constraints
  - relevantFiles[]
  - openQuestions[]

PRD:
- Input:
  - Research output
  - feature description
- Output:
  - problemStatement
  - goals[]
  - nonGoals[]
  - userStories[]
  - acceptanceCriteria[]
  - constraints

Build:
- Input:
  - PRD output
  - repo context summary
- Output:
  - implementationPlan[]
  - fileChanges[] (path, changeType, description, code)
  - notes

Review:
- Input:
  - Build output
- Output:
  - summary
  - risks[]
  - testsSuggested[]
  - prTitle
  - prDescription

Sync:
- Input:
  - Review output
  - git metadata (such as branch naming patterns)
- Output:
  - branchName
  - commitMessage
  - prTitle
  - prDescription

Praxis backend uses Sync output to:
- Create a branch.
- Apply diffs from Build step.
- Commit and push.
- Open a Pull Request on GitHub.
