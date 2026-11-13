# Security and Privacy Requirements

## Baseline
- HTTPS/TLS for all external traffic.
- Secure password hashing managed by Better Auth.
- Native sensitive token storage: Android Keystore, iOS Keychain.
- Encrypted backups where supported.
- Infrastructure/storage encryption as available.
- No custom application-level field encryption planned for V1 unless a specific requirement emerges.
- Do not interpret the earlier “no encryption” shorthand as permission to omit TLS, secure credential storage or encrypted backups.

## Authorization
- Authenticate first, resolve organization relationship and permission, then establish transaction-scoped tenant context.
- RLS mandatory for organization-owned tables.
- Runtime DB role cannot bypass RLS.
- Never trust client organization IDs or resource IDs without authorization.
- Resource-level authorization for every operation.
- Test tenant isolation with pooled connections and background jobs.

## Location privacy
- Request location only after member taps check-in.
- No background location.
- Backend/PostGIS is authoritative.
- Do not retain failed-attempt raw coordinates.
- Successful attendance stores attendance event and branch, not a location trail.
- GPS spoofing cannot be fully prevented; do not claim complete protection.

## Personal fitness privacy
- Workout and fitness history is user-owned.
- Gym owners cannot access it.
- Member export includes only the user's own data.
- Exports are protected, scoped and time-limited.

## Contact data disclosure
- Owner can see member name, photo, email, phone, membership status, subscription, payment and attendance history.
- Email and phone are visible by default and cannot be hidden in V1.
- This must be disclosed during gym onboarding.

## Account deletion
- Support-assisted only in V1.
- Verify identity.
- Resolve last-owner relationship first.
- Handle active membership/subscription relationships.
- Delete/anonymize personal data as appropriate.
- Retain records where legally required.
- Audit support actions.
- Exact retention periods require legal/accounting review in India.

## Security hygiene
- Dependency and secret scanning.
- Static analysis and lint.
- OWASP API security review.
- Authentication/authorization tests.
- Cross-tenant tests.
- Regular dependency updates.
- Never log secrets, credentials, tokens, precise location or sensitive personal data.
