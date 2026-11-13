# Decision Register

This register summarizes the selected project decisions. It is not a substitute for the detailed specifications linked below.

| ID | Decision | Status |
|---:|---|---|
| D001 | V1 focuses on gym operations and personal fitness tracking | Confirmed |
| D002 | Two distinct app experiences: Owner and Member | Confirmed |
| D003 | Four independently released apps in one KMP repository | Confirmed |
| D004 | Organization is tenant; branch is physical location | Confirmed |
| D005 | One branch/org in V1; schema supports multiple | Confirmed |
| D006 | One global user identity | Confirmed |
| D007 | Owner and Member are separate native apps | Confirmed |
| D008 | V1 roles Owner and Member; no staff roles | Confirmed |
| D009 | Multiple equal owners; last owner cannot leave/be removed | Confirmed |
| D010 | One org creation per owner account in V1; co-ownership of multiple orgs allowed | Confirmed |
| D011 | Gym reference code is discovery only | Confirmed |
| D012 | Membership and subscription are separate | Confirmed |
| D013 | Manual payments only; no gateway in V1 | Confirmed |
| D014 | Confirmed payments immutable; correction via audited adjustment | Confirmed |
| D015 | INR only, integer paise | Confirmed |
| D016 | Attendance member-initiated, 50m default geofence, PostGIS authoritative | Confirmed |
| D017 | Personal workouts private and user-owned | Confirmed |
| D018 | No offline sync in V1 | Confirmed |
| D019 | NestJS modular monolith | Confirmed |
| D020 | PostgreSQL/PostGIS and TypeORM | Confirmed |
| D021 | Shared PostgreSQL public schema; organization_id on tenant rows | Confirmed |
| D022 | Mandatory RLS for organization-owned tables | Confirmed |
| D023 | Transaction-scoped tenant context | Confirmed |
| D024 | Separate runtime/migration/backup DB roles | Confirmed |
| D025 | Better Auth integrated in NestJS; opaque Bearer sessions | Selected; POC required |
| D026 | `@rbac/rbac` candidate | POC required |
| D027 | pg-boss, no Redis in V1 | Confirmed |
| D028 | Transactional outbox for async domain events | Confirmed |
| D029 | Cloudflare R2 for media | Confirmed |
| D030 | No dedicated cache in V1 | Confirmed |
| D031 | REST + FCM; no WebSockets | Confirmed |
| D032 | Sentry backend and all four mobile apps | Confirmed |
| D033 | No dedicated product analytics SDK in V1 | Confirmed |
| D034 | TypeORM migrations; synchronize only Development | Confirmed |
| D035 | Centralized migration directory | Confirmed |
| D036 | Cursor-first pagination | Confirmed |
| D037 | RFC 9457 errors plus stable code/traceId | Confirmed |
| D038 | Business API response envelope | Confirmed |
| D039 | API camelCase; DB snake_case | Confirmed |
| D040 | ISO 8601 API dates/timestamps | Confirmed |
| D041 | Optimistic concurrency for mutable business records | Confirmed |
| D042 | PostgreSQL idempotency keys for critical writes | Confirmed |
| D043 | Hybrid deployment: conventional VPS + Cloudflare services | Confirmed |
| D044 | Staging and Production same VPS initially, isolated Compose projects/DBs/secrets | Confirmed |
| D045 | GHCR image registry | Confirmed |
| D046 | GitHub Actions SSH deployment | Confirmed |
| D047 | Staging automatic on develop; Production manual approval after main | Confirmed |
| D048 | Brief deployment downtime accepted in V1 | Confirmed |
| D049 | Pre-migration backup required; failure blocks deployment | Confirmed |
| D050 | Expand-and-contract risky migrations | Confirmed |
| D051 | Daily backups plus WAL/PITR target RPO 15m, RTO 2–4h | Selected target; POC/operations validation required |
| D052 | Resend transactional email | Selected; integration verification required |
| D053 | FCM push notifications | Confirmed |
| D054 | English-only V1; localization-ready strings | Confirmed |
| D055 | Light and Dark themes | Confirmed |
| D056 | Shared design tokens, native platform components | Confirmed |
| D057 | Minimal Member onboarding; resumable Owner onboarding | Confirmed |
| D058 | Self-service personal data export | Confirmed |
| D059 | Support-assisted account deletion/recovery | Confirmed |
| D060 | Backward compatibility for supported mobile versions | Confirmed |
| D061 | Graceful degradation for external service failures | Confirmed |
| D062 | Backend-first, then generated OpenAPI approval, then mobile | Confirmed |
| D063 | Separate internal Platform Admin web application | Confirmed |
| D064 | Platform Admin frontend uses SolidJS + Vite SPA | Confirmed |
| D065 | Platform Admin uses NestJS APIs only; no direct database access or second backend | Confirmed |
| D066 | Platform-admin authorization is distinct from organization Owner/Member roles | Confirmed; implementation POC/ADR required |
| D067 | Current source-control layout is one repository with root SolidJS + Vite Platform Admin and NestJS under `gym-backend/` | Confirmed for current implementation |
| D068 | No Turborepo or pnpm workspace in the current two-application setup; use native toolchains and root command aliases | Confirmed for current implementation |
| D069 | Use pnpm independently in the root frontend and nested backend, with independently owned lockfiles | Confirmed; verify clean-install workflow |
| D070 | Admin V1 is primarily read-only: overview, organizations, users, health and audit logs | Proposed V1 baseline; backend/API capabilities and authorization must be approved |
| D071 | Admin browser auth must be separately validated from native opaque Bearer sessions | POC required |

## Important POC/open items

- Better Auth + NestJS adapter and Bearer token semantics.
- Better Auth session refresh/update policy exact behavior.
- `@rbac/rbac` compatibility and whether it is the best fit.
- KMP OpenAPI generator and dual-spec configuration.
- PostgreSQL RLS and tenant context under pool reuse.
- PostGIS geofence accuracy threshold and edge behavior.
- pg-boss/outbox retry and reconciliation behavior.
- PITR restore process, cost and measured RPO/RTO.
- India legal/accounting retention periods.
- Actual domain, VPS provider/size and final resource budget.
- Exact API endpoints/schemas generated from backend.


## Additional POC/open items

- Better Auth browser session/cookie strategy for SolidJS admin, including CSRF/CORS/trusted origins.
- Global platform-admin provisioning and permission model, independent of organization roles.
- Controlled/audited platform-wide data access that preserves database security boundaries.
- SolidJS dependency compatibility and TanStack Query for Solid selection.
- OpenAPI-to-TypeScript client generator and generated package ownership.
- Whether future shared packages or mobile integration justify a workspace or repository restructuring.
- Future KMP build/test CI boundaries; Gradle/Xcode remain authoritative.
- Static admin hosting choice: Cloudflare Pages or Caddy/VPS.
