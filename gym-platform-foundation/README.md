# Gym Platform Foundation

**Status:** Updated architecture baseline  
**Version:** 1.1.1-draft  
**Purpose:** Canonical product, domain, architecture, engineering and operational specification for the Gym Platform.

This repository is the shared source of truth for the Gym Platform. The current application repository uses a deliberately simple layout:

- Repository root — SolidJS + Vite Platform Admin frontend.
- `gym-backend/` — NestJS modular-monolith backend.
- Future KMP mobile project — planned separately; not part of the current bootstrap.

The frontend and backend are independently managed applications in one Git repository. Each owns its dependencies, scripts, configuration and pnpm lockfile. There is no pnpm workspace or Turborepo. Root scripts provide convenient commands for backend tasks without changing the applications' ownership boundaries.

The foundation is deliberately designed to be usable by human developers and AI coding tools (including Google AI Studio). It defines product behavior and architectural constraints without pretending that unverified implementation details have already been proven.

## Development sequence

1. Complete and review this foundation.
2. Implement `gym-backend` first, using the foundation as its specification.
3. Generate and review the Better Auth and Gym Business OpenAPI specifications from the working backend.
4. Commit the approved specifications into this repository.
5. Implement the root SolidJS + Vite application as the internal Platform Admin Dashboard using the approved backend capabilities and contracts.
6. Build the Owner and Member Android applications, then migrate shared business/data/network logic into the planned KMP architecture and implement native iOS experiences.
7. Reassess repository layout only when the mobile project or genuinely shared packages are introduced.
8. Integrate, test and release the applications independently.

This remains a **backend-led contract bootstrap**. The current repository structure is the selected implementation layout, not a temporary Turborepo migration step.

## Repository map

| Directory | Responsibility |
|---|---|
| `01-product/` | Product vision, scope, personas, requirements and roadmap |
| `02-domain/` | Domain model, business rules and lifecycle definitions |
| `03-architecture/` | System, backend, admin, monorepo, identity and tenancy architecture |
| `04-api/` | API conventions and contract governance; generated specs arrive after backend |
| `05-database/` | Relational model guidance, integrity, RLS, migrations and retention |
| `06-mobile/` | KMP architecture, app responsibilities, UX and design foundations |
| `06-admin/` | Platform Admin Dashboard scope, UX, architecture and frontend conventions |
| `07-infrastructure/` | Environments, deployment, observability, backups and security |
| `08-quality/` | Testing, acceptance criteria and definition of done |
| `09-delivery/` | Implementation roadmap, dependencies, POC plan and AI development workflow |
| `10-decisions/` | Decision register and ADRs |
| `diagrams/` | Mermaid architecture and lifecycle diagrams |
| `templates/` | Reusable ADR and feature-specification templates |

## How to interpret this repository

Every significant statement belongs to one of these categories:

- **Confirmed:** Explicitly selected product or architecture decision.
- **Recommended baseline:** An implementation direction selected as the default, but still subject to a technical POC where indicated.
- **POC required:** Must be verified before broad implementation.
- **Deferred:** Explicitly outside V1.
- **Open implementation detail:** A competent implementation team may choose a conventional solution without reopening product decisions, provided it does not violate this foundation.

Do not treat a POC-required item as proven merely because it appears in this repository.

## Source-of-truth rules

1. Product behavior and business rules are canonical here.
2. Database migrations in `gym-backend` are the executable source for the deployed schema; the database documentation here describes intended structure and invariants.
3. The approved OpenAPI specifications committed here become canonical API contracts after backend bootstrap.
4. Generated code is not manually edited. Change the source specification or generator configuration and regenerate.
5. Significant architectural changes require an ADR.
6. If implementation reveals a conflict with this foundation, stop and reconcile the specification rather than silently inventing behavior.
7. Never place credentials, production data, private member data or secrets in this public repository.

## Initial API contract status

The initial API directory intentionally contains conventions and endpoint requirements, not fabricated OpenAPI schemas. The backend must generate:
- Gym business API OpenAPI document from NestJS.
- Better Auth OpenAPI document from the configured Better Auth OpenAPI plugin.

After review, add the generated documents under `04-api/openapi/` and record their generation source/version. The admin frontend consumes the approved Gym Business contract and the supported Better Auth browser authentication integration; it must not invent endpoints.

## Project-wide non-negotiables

- V1 is online-only; no offline sync.
- One global user identity.
- Organization is the tenant; branch is a physical location.
- One branch per organization in V1; schema supports multiple branches.
- Owner and Member are separate native app experiences.
- Platform Admin is a separate internal web application, not an Owner feature or organization role.
- Platform Admin frontend uses SolidJS + Vite; no React, Astro or SolidStart in this application.
- Shared KMP business/network/data logic; native Compose and SwiftUI UI.
- NestJS modular monolith, PostgreSQL/PostGIS, TypeORM.
- Better Auth is the selected identity/session solution; integration details require POC.
- Tenant isolation uses application authorization plus mandatory PostgreSQL RLS on organization-owned tables.
- Manual payments only in V1; no online gateway.
- Workout history is user-owned and private from gym owners.
- No staff roles, public gym directory, AI features, gamification or SaaS billing in V1.
- Critical business writes are transactional and idempotent where specified.
- Mobile apps are independently released and backend compatibility must account for supported older versions.
- The current repository has a root SolidJS + Vite frontend and a nested `gym-backend/` NestJS application. They are independently managed with pnpm and native toolchains.
- No pnpm workspace or Turborepo is used. Reconsider orchestration only if future shared packages or measured workflow needs justify it.
- Gradle and Xcode remain authoritative for the future KMP mobile project.

## Change process

For a product or business-rule change:
1. Update the canonical product/domain document.
2. Review impact on API, database, security, mobile and tests.
3. Add or update an ADR if architecture is affected.
4. Update implementation repositories.
5. Regenerate and validate OpenAPI documents if contracts change.
6. Record the foundation revision used by each implementation repository.
