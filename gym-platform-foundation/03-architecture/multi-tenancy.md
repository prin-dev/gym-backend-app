# Multi-Tenancy and Isolation

## Tenant model

- Organization is the tenant root.
- Branch is a physical location within an organization.
- V1 creates one branch per organization; schema supports one-to-many.
- Organization-owned records carry `organization_id`.
- Global identity and personal workout data are not tenant-owned.
- A person may have multiple organization relationships and different roles per organization.

## Request authorization sequence

1. Validate Better Auth session.
2. Resolve global user identity.
3. Resolve requested organization from trusted resource/context.
4. Verify the user has the required organization relationship and permission.
5. Begin database transaction.
6. Set transaction-scoped tenant context (`SET LOCAL` or equivalent).
7. Execute tenant-scoped repository operations under RLS.
8. Commit or rollback.
9. Ensure no tenant context leaks to a reused pooled connection.

The client-supplied organization ID is a selector, not proof of authorization.

## RLS requirements

- RLS is mandatory for every organization-owned table.
- Global tables are explicitly classified and exempted.
- Policies must fail closed when tenant context is missing.
- Runtime role cannot bypass RLS and does not own protected tables.
- Application queries must still explicitly scope by organization.
- Background jobs establish tenant context before tenant-owned access.
- Global maintenance jobs use explicit restricted pathways.
- Integration tests must reuse pooled connections across different tenants and verify no context leakage.

## Database roles

Separate credentials:
- `gym_app`: runtime application, no schema ownership, no schema modification.
- `gym_migrator`: schema migrations.
- `gym_backup`: backup/maintenance privileges required for backup.
- Superuser credentials are never used by the runtime application.

Role names are illustrative; actual PostgreSQL grants must be least-privilege and verified against the selected backup/PITR approach.

## Isolation tests

- User from organization A cannot read/write organization B data.
- Missing tenant context denies tenant-owned access.
- Cross-tenant IDs cannot be used to mutate records.
- RLS applies to reads, inserts, updates and deletes.
- Background jobs cannot accidentally run without tenant context.
- Connection pool reuse cannot retain prior tenant context.
- Owner role in one organization does not confer Owner role in another.
