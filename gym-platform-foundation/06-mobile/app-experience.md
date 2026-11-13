# Owner and Member App Experience Requirements

## Owner app

### Onboarding
1. Register/sign in and verify email.
2. Create organization.
3. Enter business identity/contact details.
4. Configure first branch and location/geofence.
5. Complete setup and activate organization.
6. Access dashboard.

Onboarding is resumable. Partially configured organization must not be treated as fully operational until required setup is complete. Resume must not create duplicate organizations.

### Primary navigation areas
- Dashboard
- Members
- Join requests
- Subscriptions/plans
- Payments
- Attendance
- Organization/settings
- Owner/team management (co-owners only in V1)

### Multi-organization
An owner may manage multiple organizations through co-ownership and switch organizations. V1 creation limit remains one organization per account.

### UX requirements
- Clear financial status: pending versus confirmed.
- Display outstanding balance separately from collected amount.
- Show subscription state and member access/suspension separately.
- Confirm destructive or financially significant actions.
- Detect stale edits and explain conflicts.
- Provide loading, empty, error and success states.
- Dashboard metrics must use agreed definitions.

## Member app

### Onboarding
1. Register/sign in and verify email.
2. Enter Member Home immediately.
3. Profile completion is optional.
4. Gym joining is optional.
5. Workout tracking is immediately available.

### Primary navigation areas
- Home / gym memberships
- Join gym
- Attendance/check-in
- Workouts
- Workout history
- Profile/settings

### Multiple gyms
A member may belong to multiple organizations and switch between them. Global profile and workout history remain user-owned; membership, subscription, payments and attendance are organization-specific.

### UX requirements
- Explain why check-in is unavailable (inactive subscription, suspended membership, outside geofence, location permission/accuracy).
- Request location only when the member initiates check-in.
- Never request background location.
- Keep personal workout data private from gym owners.
- Provide data export.
- Allow workout tracking without gym membership.
