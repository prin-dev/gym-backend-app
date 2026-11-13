# Identity, Authentication and Authorization

## Selected direction

- Better Auth integrated directly into the NestJS application through an AuthModule.
- Email/password only in V1.
- Mandatory email verification.
- Opaque Better Auth Bearer sessions for native clients; not JWT in V1.
- Resend for transactional email.
- `@rbac/rbac` is the preferred authorization-library candidate, but adoption is POC-gated.
- Organization-specific authorization is separate from authentication.
- PostgreSQL RLS is an additional tenant boundary, not a replacement for application authorization.

## Account policy

- One global user account.
- Minimum password length: 8 characters.
- Support long passwords (at least 64 characters).
- No arbitrary composition rules.
- Block commonly compromised passwords if supported through a suitable mechanism.
- Progressive rate limiting by IP and normalized account identifier; avoid permanent account lockout.
- No social login in V1.
- No TOTP 2FA in V1.
- Multiple simultaneous sessions allowed.
- Each session can be viewed/revoked independently.
- Logging out one session does not log out others.
- Password change revokes other active sessions.
- Session target: 30-day rolling expiry with update/refresh threshold every 7 days. Exact Better Auth semantics must be verified in POC.
- Native remember-me semantics are client-controlled: persist token in Keychain/Keystore when enabled; memory-only when disabled. Do not assume browser cookie remember-me behavior maps to native clients.

## Email
- Resend integration behind an EmailModule abstraction.
- Email verification required before protected operations.
- Password reset via verified email.
- Email change verifies the new address before replacing the current address.
- Support-assisted recovery if the user loses access to their email.
- No marketing email.

## Owner invitations
- Invite by email.
- Authenticated invitee must have matching verified email.
- Seven-day expiry.
- One active invite per email per organization.
- Owners may cancel and resend expired invitations.
- All owners have equal permissions.
- Last owner cannot leave or be removed.

## Authorization
- V1 roles: Owner and Member.
- Owner permissions are equal within an organization.
- Role resolution is organization-scoped.
- Every resource operation requires resource-level authorization, not just route-level role checks.
- Do not trust client-supplied organization IDs.
- Resolve authenticated user, verify organization relationship and permission, then establish tenant transaction context.
- `@rbac/rbac` POC must verify NestJS integration, supported storage adapter, tenant-specific role resolution and resource-level checks. If unsuitable, do not force it.

## Mandatory POC acceptance
1. Better Auth runs inside NestJS.
2. Bearer plugin issues opaque session tokens suitable for native apps.
3. NestJS validates session token and resolves user.
4. KMP can securely store/send token on Android and iOS.
5. Multiple sessions, revocation and expiry behavior work as intended.
6. Organization-specific Owner/Member permissions work.
7. Last-owner invariants are enforced.
8. RLS remains effective under pooled connection reuse.
9. Cross-tenant access is blocked in API and database.
10. Email verification, password reset and email change flows work through Resend.

## Platform administrator authorization

The Platform Admin Dashboard introduces a distinct global platform-level authority. It is not an organization role and must not be inferred from Owner or Member membership.

- Platform-admin accounts must be explicitly provisioned; no public self-registration.
- Platform-admin permissions must be checked by the backend for every administrative API operation.
- Organization-scoped Owner/Member authorization remains unchanged.
- Cross-tenant/platform-wide access requires a narrowly defined, auditable backend access path. Do not solve this by using a database superuser, bypassing RLS globally, or trusting a frontend route guard.
- Administrative reads and especially high-impact mutations must produce audit records with actor, action, target, timestamp and correlation context.
- V1 admin interface is primarily read-only. Mutating operations require explicit product scope, permission design, audit behavior and confirmation UX.

**POC/ADR required:** Determine whether platform-admin identity/permissions are represented by a separate global role/permission model in the application database, a Better Auth-compatible plugin/extension, or another controlled mechanism. Verify that this remains independent of organization roles and works with the selected browser session integration.

Browser authentication is a separate validation target from native opaque Bearer sessions. Verify Better Auth's supported browser cookie/session behavior, CSRF, CORS, trusted origins, logout and revocation before implementing the admin login flow.

