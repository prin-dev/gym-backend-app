# Platform Admin Experience and V1 Scope

## Product role

The Platform Admin Dashboard is an internal operational interface for the platform operator. It is not a gym management interface and is not exposed as a feature inside the Owner or Member apps.

## Users and authorization

Only explicitly provisioned platform administrators may access this application. A gym Owner or Member does not become a platform administrator by virtue of an organization role.

The exact platform-admin provisioning and permission model is POC/ADR-gated. Do not implement self-service platform-admin registration.

## V1 navigation proposal

1. Overview.
2. Organizations.
3. Users.
4. System Health.
5. Audit Logs.
6. Settings (only where a real, approved backend capability exists).

This navigation is a proposed minimal operational scope. A menu item must not imply that an unsupported backend operation exists.

## V1 capabilities

### Overview

Show platform-level operational metrics only when supported by backend endpoints. Candidate metrics include organization count, active organization count, registered user count and recent organization/user creation trends. Show data freshness and unavailable states. Never fabricate values.

### Organizations

- Search and paginated listing.
- View organization details and status.
- View permitted owner/branch summaries.
- Filter and sort only where supported by the API.

V1 is primarily read-only. Suspend, delete, edit, impersonate or otherwise mutate an organization only after an explicit product requirement, backend endpoint, authorization policy, audit behavior and confirmation UX have been approved.

### Users

- Search and paginated listing.
- View permitted account details and verification/status information.
- View organization relationships only where authorized and exposed by the API.

Do not expose password hashes, session tokens, private workout data or unnecessary personal information. User impersonation is out of scope for V1.

### System Health

Display sanitized health information for API, database connectivity, background jobs and configured integrations where the backend exposes safe operational status. Do not reveal secrets, raw environment configuration, infrastructure credentials or stack traces.

### Audit Logs

Read-only paginated audit event listing with supported filters such as actor, action, organization and date range. Do not permit editing or deleting audit events.

### Settings

No generic settings editor is implied. Implement only explicitly specified and authorized platform settings backed by real APIs.

## UX behavior

- Desktop-first responsive administration layout.
- Collapsible navigation on narrow screens.
- Consistent page title, breadcrumbs and account menu.
- Tables use server-side pagination for large collections.
- Search should be debounced and cancellable.
- Filters and sort options must reflect API capabilities.
- Every data view supports loading, empty, error and stale-data states.
- Destructive/high-impact actions require explicit confirmation and clear consequences.
- Display timestamps in a clearly identified timezone; platform-wide timezone policy is an open implementation decision.
- Provide light/dark themes and accessible keyboard/focus behavior.

## Data refresh

V1 uses REST and query invalidation after mutations. Refresh selected operational metrics periodically at a conservative interval and provide manual refresh. Do not poll every table or introduce WebSockets/SSE for the initial dashboard.

## Out of scope

- Gym Owner operations such as member subscriptions, attendance management and payment recording.
- Member workout and personal fitness experiences.
- SaaS billing, plan monetization and payment gateway administration.
- User impersonation.
- Arbitrary tenant data editing or cross-tenant support tools.
- Real-time event streaming.
- Advanced analytics warehouse.
- Unspecified platform configuration controls.
