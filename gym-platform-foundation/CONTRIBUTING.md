# Contributing

## Scope

This repository contains specifications, diagrams, templates and decision records only. Application source code currently lives in the repository root (SolidJS + Vite Platform Admin) and `gym-backend/` (NestJS API). The future KMP mobile project is not part of the current repository bootstrap. This foundation remains the canonical specification source.

## Change rules

- Use concise, descriptive Markdown.
- Prefer updating an existing canonical document over duplicating rules.
- Link related documents instead of copying long sections.
- Mark new assumptions as `POC required`, `proposed` or `open`; do not present guesses as confirmed.
- Significant architecture changes require an ADR.
- Keep examples fictional. Do not commit credentials, secrets, production records or personally identifiable member data.
- When changing a business rule, update its lifecycle diagram and acceptance tests where relevant.
- When changing an API contract, update the relevant OpenAPI specification after it has been generated and approved.
- Admin UI requirements belong in `06-admin/`; do not mix platform-administration permissions with organization Owner/Member roles.
- Repository-structure changes must update `03-architecture/monorepo-and-tooling.md`, agent instructions, scripts, CI expectations and the relevant ADR.
- The current applications are independently managed with pnpm; do not add a workspace or Turborepo without an explicit decision.
- Gradle and Xcode remain authoritative for future native/KMP builds.

## Review checklist

- Does this change alter V1 scope or user-visible behavior?
- Does it affect tenant boundaries or personal-data ownership?
- Does it affect API compatibility with released mobile versions?
- Does it require a migration or backfill?
- Does it alter financial, subscription, attendance or audit history?
- Are tests and acceptance criteria updated?
- Is an ADR required?
