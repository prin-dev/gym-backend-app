# Technical Proof-of-Concept Plan

A POC must answer a concrete risk, include a reproducible test and have a pass/fail outcome. Do not build production features inside a POC.

## POC-01 Better Auth + NestJS
Verify:
- Better Auth runs within the NestJS application/HTTP adapter.
- Email/password registration and verification.
- Bearer plugin issues opaque native-compatible tokens.
- NestJS request authentication resolves session/user.
- Session expiry, update threshold, revocation and password-change behavior.
- Resend delivery integration.
- Better Auth OpenAPI plugin generates a usable specification.

Pass when the complete flow works locally and behavior matches the chosen policy. Document any Better Auth semantic differences before implementation.

## POC-02 Authorization library
Verify `@rbac/rbac`:
- NestJS compatibility.
- Supported persistence adapter and PostgreSQL integration.
- Organization-specific role resolution.
- Resource-level authorization.
- Permission checks that do not trust client organization identifiers.
- Maintainability and activity/support status.

Pass only if the library can express the required tenant-scoped Owner/Member rules without awkward workarounds. Otherwise select a simpler explicit authorization implementation by ADR.

## POC-03 PostgreSQL RLS
Verify:
- Runtime role does not own tables and cannot bypass RLS.
- Transaction-scoped tenant context is applied.
- Missing context fails closed.
- Connection pool reuse does not leak tenant context.
- Background jobs set tenant context.
- Insert/update/delete policies work, not just SELECT.
- Global tables are intentionally excluded.

Use real PostgreSQL, not SQLite or mocks.

## POC-04 KMP API generation
Verify:
- NestJS business OpenAPI spec generates KMP client/models.
- Better Auth OpenAPI spec generates a separate package.
- Multiple spec declarations produce separate package namespaces.
- Shared Ktor transport and Bearer authentication work.
- Error envelope and RFC 9457 errors map correctly.
- Android and iOS targets compile.
- Regeneration does not require manual edits to generated files.

The specific generator/plugin and Gradle DSL are not finalized until this POC passes. Do not copy illustrative configuration as guaranteed-valid.

## POC-05 Native session storage
Verify:
- Android Keystore-backed secure storage.
- iOS Keychain.
- Ktor attaches Bearer token.
- Logout clears local token and revokes server session.
- Multiple sessions and remember-me behavior.
- No token in logs, analytics or crash payloads.

## POC-06 Geofence
Verify:
- PostGIS distance calculation in metres.
- Boundary behavior at radius.
- Configurable GPS accuracy threshold.
- Organization timezone daily uniqueness.
- Duplicate concurrent check-ins.
- No raw failed-coordinate retention.
- GPS spoofing limitations are documented.

## POC-07 Jobs and outbox
Verify:
- Business transaction writes state and outbox record atomically.
- Worker dispatches through pg-boss.
- Worker restart/retry behavior.
- Duplicate event handling and idempotent consumers.
- Expiry scheduling and reconciliation.
- Failed jobs visible for operations.

## POC-08 Backup/PITR
Verify:
- Daily backup can be restored.
- WAL archives are continuous and complete.
- Point-in-time restore to a selected timestamp works.
- Restore process meets target RPO/RTO under realistic data volume.
- R2 permissions, encryption, retention and failure alerts work.


## POC-09 Platform Admin browser authentication and authorization

Verify:
- Better Auth browser session integration supported by the selected version/configuration.
- Secure cookie/session behavior, CSRF protections, CORS and trusted origins.
- Login, session retrieval, expiry, logout and revocation.
- Explicit global platform-admin provisioning and permission checks.
- Owner/Member accounts cannot call admin APIs.
- Cross-tenant administrative access is narrowly controlled and audited without global RLS bypass or runtime superuser privileges.

Pass when browser auth and backend authorization work end-to-end in local and staging-like environments, with automated negative tests.

## POC-10 Independent application toolchains and root scripts

Verify:
- Root SolidJS/Vite application installs, runs, builds and tests from the repository root.
- NestJS application installs, runs, builds and tests from `gym-backend/`.
- Root aliases correctly invoke the backend scripts without changing its working directory assumptions.
- Each application has an independently reproducible pnpm install/lockfile.
- CI path filters run the correct checks for root frontend changes and `gym-backend/` changes.
- No pnpm workspace or Turborepo is required for the current two-application workflow.

Pass when a clean checkout can install, build and test both applications independently using documented commands. Reopen this POC only if shared packages, mobile integration or measured orchestration needs justify a different structure.

## POC-11 Admin frontend API client and hosting

Verify:
- SolidJS + Vite builds and serves as a static SPA.
- API client/types can be regenerated from the approved Gym Business OpenAPI contract.
- Better Auth browser integration is not incorrectly replaced by the native mobile Bearer client.
- SPA history fallback, environment API origins, secure headers and cache behavior work on the selected hosting path.

Pass when the dashboard can authenticate, call a protected admin API, handle unauthorized/error states and deploy/rollback independently.
