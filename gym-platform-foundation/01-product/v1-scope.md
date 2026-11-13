# V1 Scope

## In scope

### Owner experience
- Account registration, verification and session management.
- Create and configure one organization and its first branch.
- Invite and manage co-owners.
- Gym reference code generation, viewing, sharing and regeneration.
- Member directory and join-request processing.
- Subscription plan creation and assignment.
- Manual payment recording, confirmation, installments and balances.
- Member suspension, reinstatement, removal and departure handling.
- Attendance overview and history.
- Operational dashboard and simple revenue/attendance trends.
- Organization settings and branch geofence configuration.

### Member experience
- Account registration and verification.
- Optional personal profile.
- Join a gym using its reference code.
- Manage multiple organization memberships.
- View membership, subscription, payment and attendance history.
- Check in when eligible and within the branch geofence.
- Independent personal workout tracking.
- Personal data export.

### Platform Admin Dashboard (proposed V1 baseline)
- Separate internal SolidJS + Vite web application.
- Explicitly provisioned platform administrator access.
- Platform overview using real backend metrics where available.
- Read-only organization search/list/detail.
- Read-only global user search/list/detail with privacy-filtered fields.
- Sanitized system health information.
- Read-only audit log search and inspection.
- No self-service platform-admin registration.
- No user impersonation or arbitrary cross-tenant mutation.

The exact dashboard capabilities remain subject to backend API availability, platform-admin authorization design and security review. Unsupported metrics/actions must be omitted or shown as unavailable, never fabricated.

### Platform capabilities
- One global user identity.
- Organization/branch model.
- Role and permission enforcement.
- Tenant isolation.
- Notifications and push delivery.
- Media storage for supported profile/media assets.
- Audit logging.
- Backups, observability and recovery processes.
- App version policy and API compatibility.

## Explicitly out of scope for V1

- Staff management and granular staff permissions.
- Public gym directory or location-based gym discovery.
- Online payment gateway.
- AI assistant, recommendations or AI insights.
- Workout planner, routines or adaptive training plans.
- Diet and nutrition tracking.
- Gamification, challenges, streaks, badges and leaderboards.
- Social networking.
- Advanced analytics subsystem.
- Multi-branch management UI (schema supports future multi-branch).
- Offline support and synchronization.
- SaaS billing, subscriptions and premium feature limits.
- Platform-admin user impersonation, unrestricted cross-tenant data access, arbitrary tenant mutation, organization deletion/suspension and billing administration.
- Multilingual UI and multilingual exercise catalogue.
- Self-service account deletion.
- Refund workflow.
- Email/SMS notifications other than required transactional email for authentication.
- Background or automatic attendance tracking.
- GPS spoofing prevention claims beyond reasonable validation.

## Scope guardrail

A feature is not V1 merely because it is technically easy to add. It must directly support reliable gym operations or personal fitness tracking. Any proposed addition must state which existing V1 outcome it improves and what it displaces.
