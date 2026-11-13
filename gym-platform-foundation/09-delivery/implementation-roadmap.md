# V1 Implementation Roadmap

## Strategy

Complete the foundation first, implement the NestJS backend and its contracts, then build the root SolidJS + Vite Platform Admin frontend against real administrative APIs. The current repository contains the frontend at its root and the backend in `gym-backend/`; the two applications are independently managed with pnpm and no workspace/orchestration layer. Build the Android app baseline and migrate to KMP as a later workstream.

This is a backend-led contract bootstrap followed by contract-governed mobile development.

## Milestone 0 — Foundation
Deliver:
- Product overview and V1 scope.
- Functional requirements and personas.
- Domain model and lifecycle rules.
- System/backend/mobile architecture.
- Database and tenant isolation.
- API conventions and endpoint capability map.
- Infrastructure, security, testing and delivery strategy.
- POC acceptance criteria.
- ADR register.

Exit: foundation reviewed and no unresolved product-level ambiguity blocks backend implementation.

## Milestone 1 — Technical POCs
POCs:
- Better Auth in NestJS with Bearer sessions.
- RBAC library suitability.
- PostgreSQL RLS + transaction context under connection pooling.
- KMP API generator with two separate OpenAPI specs/packages.
- Android/iOS secure token storage and Ktor auth.
- PostGIS geofence behavior.
- pg-boss + transactional outbox.
- Backup/PITR restore proof.

Exit: critical risks verified or alternative decisions documented by ADR.

## Milestone 2 — Backend bootstrap
- NestJS project and module boundaries.
- PostgreSQL/PostGIS Docker Compose.
- TypeORM and migrations.
- Database roles and RLS baseline.
- Pino/Sentry, config validation.
- pg-boss.
- CI, Docker image build and GHCR.
- Caddy/Cloudflare deployment path.
- Bruno collection baseline.

Exit: repeatable local environment and CI.

## Milestone 3 — Identity and tenant foundation
- Better Auth integration.
- Email verification and Resend.
- Session management.
- User profile.
- Organization and branch onboarding.
- Owner invitation/co-ownership.
- Authorization and RLS.
- Audit log.

Exit: verified organization isolation and onboarding workflows.

## Milestone 4 — Membership
- Reference code lookup.
- Join request lifecycle.
- Membership lifecycle.
- Owner and Member API workflows.

## Milestone 5 — Subscriptions and payments
- Plan management.
- Subscription assignment/renewal.
- State machine and reconciliation.
- Manual payments, installments and immutable confirmation.
- Balances and revenue calculations.

## Milestone 6 — Attendance
- Branch geofence.
- PostGIS check-in validation.
- Local-day uniqueness.
- Attendance views and reporting.

## Milestone 7 — Fitness
- Exercise catalogue.
- Personal profile.
- Workout session lifecycle and history.
- Ownership/privacy enforcement.

## Milestone 8 — Notifications and dashboard
- Notification records and push.
- Expiry reminders.
- Owner dashboard and simple trends.
- App version policy.

## Milestone 9 — Platform Admin Dashboard
- Complete browser-authentication and platform-admin authorization POC.
- Add only the approved platform-admin backend capabilities.
- Generate/review admin API contracts.
- Build SolidJS + Vite application shell and authentication.
- Implement overview, organization/user read views, health and audit logs as APIs become available.
- Verify authorization, accessibility, responsive behavior and independent static deployment.

Exit: admin dashboard uses real APIs, has no fabricated production data and cannot exceed backend permissions.

## Milestone 10 — API contract freeze and mobile
Generate and review:
- Better Auth OpenAPI.
- Gym business API OpenAPI.

Commit approved specs into foundation. Then:
- Generate KMP clients.
- Build shared KMP core.
- Build Owner Android/iOS.
- Build Member Android/iOS.
- Complete vertical workflows.

## Milestone 11 — Stabilization and release
- End-to-end testing.
- Security and privacy review.
- Compatibility tests.
- Performance/resource tests.
- Backup/PITR drills.
- Play Internal Testing and TestFlight.
- Production rehearsal and release.

## Vertical slice policy

Do not build the entire backend and defer all mobile work until the end. After API contracts stabilize, complete end-to-end vertical slices across backend, shared KMP and the relevant native apps. However, the full mobile implementation begins after the approved API specifications are available, as required by the chosen workflow.


## Milestone 12 — Repository evolution (only when justified)

- Reassess repository structure after the backend and Platform Admin workflows are stable.
- Add the KMP project at an explicitly selected path when mobile implementation begins; keep Gradle/Xcode authoritative.
- Consider shared packages or workspace tooling only when a real shared dependency or measured workflow problem exists.
- If restructuring is approved, update agent instructions, scripts, CI paths, deployment contexts and this foundation together.
- Preserve Git history where practical and verify clean-checkout builds for each application.

Exit: any approved evolution preserves independent application ownership, reproducible builds and clear API boundaries. This milestone does not presume a pnpm workspace or Turborepo adoption.
