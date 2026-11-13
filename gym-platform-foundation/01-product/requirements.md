# Functional Requirements

Requirement identifiers are stable references for implementation and acceptance tests.

## Identity and onboarding
- `ID-001`: A person has one global user account.
- `ID-002`: Registration uses email and password.
- `ID-003`: Email verification is mandatory before protected functionality.
- `ID-004`: Password reset and password change are supported.
- `ID-005`: Multiple device sessions are allowed and independently revocable.
- `ID-006`: Password change revokes other active sessions.
- `ID-007`: User can change email only after verifying the new address.
- `ID-008`: Account recovery after loss of email access is support-assisted.
- `ID-009`: Owner onboarding can be resumed after interruption.
- `ID-010`: Member onboarding is minimal; profile and gym joining are optional.

## Organizations and owners
- `ORG-001`: An owner can create at most one organization in V1.
- `ORG-002`: First branch is created as part of organization onboarding.
- `ORG-003`: Organization becomes active after required onboarding completes.
- `ORG-004`: No platform-admin approval is required.
- `ORG-005`: An organization can have multiple equal owners.
- `ORG-006`: Owners can invite/remove other owners and leave, subject to last-owner protection.
- `ORG-007`: Owner invitations are sent to registered email identity, expire after seven days and require authenticated matching verified email.
- `ORG-008`: Only one active invitation per email/organization is allowed; cancellation and resend are supported.
- `ORG-009`: Organization reference code is generated automatically and can be viewed, shared and regenerated.
- `ORG-010`: Reference code is for discovery only and is never an authentication credential.

## Branch and discovery
- `BR-001`: V1 organization has exactly one branch, while the data model supports multiple branches.
- `BR-002`: Branch stores physical address, coordinates and geofence configuration.
- `BR-003`: Owner selects location through map/address search and may adjust a marker; manual coordinates are an advanced fallback.
- `BR-004`: V1 gym discovery is by reference code only.

## Membership
- `MEM-001`: One pending join request per user/organization.
- `MEM-002`: Existing members cannot create duplicate join requests.
- `MEM-003`: All owners may approve or reject join requests.
- `MEM-004`: Rejected users may reapply immediately.
- `MEM-005`: Approval creates organization membership but does not automatically create a paid subscription.
- `MEM-006`: Members may belong to multiple organizations.
- `MEM-007`: Member can leave; owner can remove; owner can suspend/reinstate.
- `MEM-008`: Rejoining after termination creates a new membership record.
- `MEM-009`: Membership and subscription are separate concepts.

## Subscription and payments
- `SUB-001`: Organizations define reusable subscription plans.
- `SUB-002`: Individual assignment may override plan price and duration.
- `SUB-003`: Assigned subscription stores a snapshot of agreed terms.
- `SUB-004`: Renewals create new subscription records.
- `SUB-005`: Only one active subscription per organization membership; a future scheduled renewal may exist.
- `SUB-006`: Early renewal starts the day after current subscription ends.
- `SUB-007`: Post-expiry renewal uses owner-defined start date.
- `SUB-008`: Calendar-day inclusive validity uses organization timezone.
- `SUB-009`: First confirmed payment plus reached start date activates; future start produces Scheduled.
- `SUB-010`: Subscription state is separate from member access/suspension state.
- `PAY-001`: V1 supports manual payment methods only.
- `PAY-002`: Payment recording and payment confirmation are separate.
- `PAY-003`: Partial payments and installments are supported.
- `PAY-004`: Only confirmed payments count as collected revenue.
- `PAY-005`: Confirmed payment records are immutable; corrections use audited adjustment/reversal records.
- `PAY-006`: Pending payments may be edited or cancelled.
- `PAY-007`: INR only, stored as integer paise.

## Attendance
- `ATT-001`: Check-in is member-initiated; no background location tracking.
- `ATT-002`: One successful check-in per member/organization/local calendar day.
- `ATT-003`: Check-in requires active eligible subscription and non-suspended/non-terminated membership.
- `ATT-004`: Check-in must be within branch geofence.
- `ATT-005`: Default radius is 50 metres and is branch-owned/configurable.
- `ATT-006`: Backend/PostGIS is authoritative for distance validation.
- `ATT-007`: Raw coordinates from failed attempts are not retained.
- `ATT-008`: Successful attendance stores the attendance event and branch, not a location trail.

## Workouts
- `FIT-001`: Personal workout tracking works without gym membership.
- `FIT-002`: Personal fitness profile is optional.
- `FIT-003`: Workout history is user-owned and private from gym owners.
- `FIT-004`: V1 supports free-form workout logging, not workout plans/routines.
- `FIT-005`: Exercise catalogue is platform-managed and English-only in V1.
- `FIT-006`: Sessions can be in progress, completed or discarded.
- `FIT-007`: Interrupted in-progress sessions can be resumed; explicit discard is separate.
- `FIT-008`: Members may edit/delete completed sessions.

## Notifications and dashboard
- `NOT-001`: In-app notification records are persisted in PostgreSQL.
- `NOT-002`: Push delivery uses FCM, including APNs delivery through FCM for iOS.
- `NOT-003`: Notification failure never rolls back the business operation.
- `NOT-004`: Subscription expiry reminders are scheduled 7 days before, 1 day before and on expiry.
- `DASH-001`: Owner dashboard includes total members, active subscriptions, current-month collected revenue and today's attendance.
- `DASH-002`: Operational views include pending join requests, upcoming expiries, outstanding balances and recently joined members.
- `DASH-003`: Simple revenue and attendance trends are derived from transactional data.


## Platform Admin Dashboard

These requirements define the proposed minimal V1 internal dashboard. They do not authorize backend capabilities until the corresponding permission model and API contract are implemented.

- `ADM-001`: The Platform Admin Dashboard is a separate SolidJS + Vite web application, distinct from Owner and Member applications.
- `ADM-002`: Only explicitly provisioned platform administrators may access platform-admin APIs and dashboard routes.
- `ADM-003`: Organization Owner/Member roles do not grant platform-admin permissions.
- `ADM-004`: Authorized administrators can view platform overview metrics only when the backend exposes defined, real metrics.
- `ADM-005`: Authorized administrators can search/list and inspect organizations using privacy-filtered fields exposed by the backend.
- `ADM-006`: Authorized administrators can search/list and inspect global user accounts using privacy-filtered fields exposed by the backend.
- `ADM-007`: Authorized administrators can view sanitized service and background-job health information where available.
- `ADM-008`: Authorized administrators can search/list and inspect audit events; the dashboard cannot modify or delete audit records.
- `ADM-009`: Platform-admin API access is enforced by the backend and audited according to the approved access policy.
- `ADM-010`: The dashboard must not expose private workout data, credentials, session tokens, password hashes or unnecessary personal data.
- `ADM-011`: V1 does not include user impersonation, unrestricted cross-tenant access, arbitrary organization mutation, organization deletion/suspension or SaaS billing administration.
- `ADM-012`: Dashboard metrics and lists must use real backend data; missing capabilities are reported rather than replaced with fabricated production values.
