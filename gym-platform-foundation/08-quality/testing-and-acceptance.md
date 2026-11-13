# Testing Strategy and Acceptance

## Backend
- Jest unit, integration and E2E tests.
- Real PostgreSQL + PostGIS integration database.
- Test migrations against a clean database and upgrade path.
- Test RLS with runtime role, multiple tenants and pooled connection reuse.
- API workflow tests.
- Contract validation against approved OpenAPI.
- Bruno collections for manual API exploration.

## Mobile
- KMP shared-code tests.
- Android platform tests and Compose UI tests for critical journeys.
- iOS shared/platform tests and SwiftUI UI tests for critical journeys.
- Build checks for all four app targets.
- Secure token storage tests.
- Deep-link/auth redirect tests.
- Accessibility checks for key flows.

## Platform Admin frontend
- Solid component/unit tests with Vitest.
- Browser end-to-end tests with Playwright.
- Login/session expiry/logout tests using the actual browser auth strategy.
- Platform-admin authorization tests; regular Owner/Member must be denied by backend APIs.
- Organization/user list search, filters and cursor pagination.
- Loading, empty, error, stale-data and retry states.
- Audit log read-only behavior.
- Accessibility and responsive layout checks.
- Production build and static hosting/SPA fallback verification.

## Critical test priorities
1. Better Auth sessions and revocation.
2. Tenant isolation and RLS.
3. Owner invitation and last-owner invariants.
4. Join request lifecycle.
5. Subscription calendar-date calculations and state transitions.
6. Payment installment math and confirmed-payment immutability.
7. Idempotent payment/attendance/renewal writes.
8. Attendance eligibility, geofence and local-day uniqueness.
9. Membership suspension/termination behavior.
10. Workout ownership and privacy.
11. Notification failure does not roll back business operation.
12. Outbox/job retry and duplicate delivery.
13. API compatibility with supported mobile versions.
14. Backup restoration and PITR.
15. Optimistic concurrency conflict handling.
16. Platform-admin privilege separation from organization roles.
17. Cross-tenant administrative access path and audit coverage.
18. Admin browser session, CSRF/CORS and unauthorized access behavior.

## Acceptance principle

A feature is complete only when:
- Business rules are implemented.
- API contract is documented.
- Tenant authorization is verified where applicable.
- Database constraints/migrations exist.
- Concurrency/idempotency protections are implemented where needed.
- Backend tests pass.
- KMP shared logic and tests pass.
- Admin frontend tests and production build pass for changed admin features.
- Android and iOS experiences are implemented for the relevant app.
- Loading, empty, error and success states are handled.
- Observability is present where relevant.
- Documentation is updated.

## Release readiness
- All critical workflows pass end-to-end.
- No known cross-tenant access.
- Payment and subscription invariants pass.
- Backup/restore drill completed.
- Sentry events and environment tags verified.
- Staging deployment rehearsed.
- Play Internal Testing and TestFlight builds distributed.
- Production deployment manually approved.
