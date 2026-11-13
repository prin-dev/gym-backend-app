# API Capability Map (Planning, Not an OpenAPI Contract)

This map describes capabilities expected in the backend. It does not prescribe exact paths, HTTP verbs, payload schemas or response models. Those are designed during backend implementation, reviewed, and then captured in generated OpenAPI specifications.

## Authentication and account
- Registration, verification, login, logout.
- Session listing/revocation.
- Password reset/change.
- Email change.
- Current-user profile.
- Account export request/status/download.
- Support-assisted account recovery workflow (operational process).

Most auth endpoints are Better Auth-owned. Do not duplicate them as custom business endpoints unless a demonstrated integration requirement exists.

## Organization and branch
- Organization creation and onboarding progress.
- Organization details/settings.
- Branch details/location/geofence.
- Organization reference code read/share/regenerate.
- Owner invitation create/cancel/resend/accept.
- Owner list and removal/leave.

## Gym discovery and membership
- Reference-code lookup.
- Join request create/list/cancel.
- Owner pending-request list/approve/reject.
- Member organization list.
- Membership detail/history.
- Suspend/reinstate/remove/leave.

## Plans, subscriptions and payments
- Plan CRUD/enable/disable.
- Assign subscription with terms snapshot.
- Subscription list/detail/history.
- Renewal.
- Payment record/edit pending/cancel pending/confirm.
- Payment history and outstanding balance.
- Audited correction/adjustment workflow.

## Attendance
- Member check-in.
- Member attendance history.
- Owner daily attendance.
- Member attendance history for a tenant.
- Attendance summary/trends.

## Fitness
- Exercise catalogue search/detail.
- Start/resume workout.
- Add/edit/remove workout exercise and sets.
- Complete/discard workout.
- List/detail/edit/delete own workout history.
- Personal profile read/update.

## Notifications
- List notifications.
- Mark read/unread.
- Device token registration/removal.
- Notification preferences are deferred; V1 uses fixed event rules.

## Dashboard and app policy
- Owner dashboard summary.
- Revenue/attendance trends.
- Upcoming expiries, outstanding balances and recent members.
- App version policy retrieval.

## Cross-cutting requirements
All organization-owned endpoints require organization authorization and tenant scoping. Critical writes require idempotency where specified. Lifecycle operations must be audited.


## Platform administration capabilities (new)

These are capability requirements, not endpoint definitions. Exact paths, schemas and permissions must be designed in the backend and generated into OpenAPI. Do not assume every capability is already implemented.

- Platform administrator session/profile and authorization context.
- Platform overview metrics with explicit definitions and data freshness.
- Organization search/list/detail with only approved platform-admin fields.
- Global user search/list/detail with privacy-filtered fields.
- Sanitized service/worker/integration health status.
- Read-only audit event search/list/detail with filters and cursor pagination.
- Explicitly approved platform settings, only if V1 requires them.

Every platform-admin capability requires backend authorization independent of frontend navigation. Cross-tenant reads must use the approved platform-admin access path and be audited where appropriate. V1 does not imply user impersonation, arbitrary tenant mutation, organization deletion/suspension, billing administration or direct database access.
