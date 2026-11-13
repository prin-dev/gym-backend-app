# AI-Assisted Development Workflow

## Purpose

AI coding tools assist implementation but must treat this foundation as the primary project specification, not permission to invent missing product behavior.

## Current repository and application layout

The current working repository contains two independently managed applications:

1. Repository root: SolidJS + Vite Platform Admin frontend.
2. `gym-backend/`: NestJS modular-monolith API.

The root is the agent's normal entry point. Start the frontend with `pnpm dev`; start the API separately with `pnpm dev:api`. Build and test aliases are documented in `03-architecture/monorepo-and-tooling.md`.

The applications use pnpm independently. There is no pnpm workspace, Turborepo or shared package graph. Do not add any of these without an explicit architecture decision. Future KMP mobile work is not part of the current bootstrap.

## Rules for AI coding tools

- Read the relevant foundation documents and repository-level `AGENTS.md` before editing code.
- Read `gym-backend/AGENTS.md` before backend changes.
- Work in one bounded milestone/feature at a time.
- Do not implement deferred V1 features.
- Do not invent business rules, entity relationships, API fields or permissions.
- If a requirement is ambiguous or contradictory, stop and report the exact conflict.
- Do not silently change architecture, libraries or database schema strategy.
- Do not edit generated API client code manually.
- Do not use TypeORM `synchronize` outside Development.
- Platform Admin is SolidJS + Vite; do not substitute React, Astro or SolidStart.
- Platform Admin is a client of NestJS, never a second backend or direct database client.
- Platform-admin authority is separate from organization Owner/Member roles.
- Do not invent admin APIs or show fabricated production metrics.
- Keep frontend and backend dependencies/configuration/builds independently owned.
- Do not introduce pnpm workspace configuration or Turborepo as an agent convenience workaround.
- Never bypass RLS or use database superuser credentials at runtime.
- Never log tokens, passwords, private workout data, payment secrets or precise location.
- Add tests for lifecycle invariants, tenant isolation and concurrency.
- Update documentation/ADRs when a decision changes.
- Run tests and report actual results; do not claim tests passed without executing them.
- Do not claim deployment or external-service actions succeeded unless verified.

## Suggested task prompt format

```text
Read the relevant documents in gym-platform-foundation before making changes.
Read the applicable AGENTS.md files.

Task:
[One bounded capability]

In scope:
[Exact requirements and modules]

Out of scope:
[Explicit exclusions]

Constraints:
[Architecture, tenancy, API, database, security rules]

Acceptance criteria:
[Observable tests and expected behavior]

Before coding:
1. Summarize the relevant requirements.
2. Identify files/modules likely to change.
3. Report ambiguities or conflicts.
4. Propose implementation steps.

Then implement only after the scope is clear.
Run relevant tests and report:
- Files changed
- Behavior implemented
- Tests actually executed and results
- Migrations added
- API contract changes
- Risks or unresolved items
```

## Human review gates

AI-generated code must be reviewed for:
- Tenant isolation and RLS.
- Authentication/session validation.
- Financial invariants.
- Subscription and attendance lifecycle.
- SQL migrations and data safety.
- API compatibility.
- Secrets and privacy.
- Dependency and license changes.
