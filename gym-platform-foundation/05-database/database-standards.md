# Database Standards

## Platform
- PostgreSQL with PostGIS.
- One shared `public` schema.
- No schema-per-module.
- TypeORM for persistence and migrations.
- PostGIS extension mandatory.
- pgcrypto only if a specific database crypto need is identified.
- UUIDv7 selected for primary keys; verify application/database generation compatibility and use one consistent implementation.
- Tables plural snake_case; columns snake_case.
- Primary key `id`; foreign key `{entity}_id`.
- `created_at`, `updated_at`; `deleted_at` only where selective soft deletion has a recovery value.
- Boolean prefixes `is_`/`has_` where helpful.

## Entity and relationship conventions
- Explicit foreign-key ID properties are the default.
- TypeORM relation decorators may be used selectively.
- No eager loading by default.
- Foreign keys enforce referential integrity.
- Restrict deletion by default; cascade only for disposable child records.
- Financial, attendance, membership, subscription and audit history must be retained.
- TypeORM entities may also carry domain behavior where appropriate; do not create duplicate model layers without need.

## Types and constraints
- PostgreSQL ENUM for well-defined lifecycle states; changes through migrations.
- JSONB only for genuinely flexible metadata.
- Relational columns for core business fields and relationships.
- CHECK, NOT NULL, UNIQUE and FK constraints provide defense in depth.
- Monetary values are integer paise with explicit currency.
- `TIMESTAMPTZ` for instants; `DATE` for calendar dates.

## Indexing
- Query-driven and tenant-aware.
- Index foreign keys and common filter/sort combinations.
- Composite indexes often begin with organization scope where appropriate.
- Unique constraints encode invariants such as one successful attendance per membership/org-local day (implementation must define local-date representation correctly).
- PostGIS spatial indexes for geofence lookup where needed.
- Validate expensive queries with `EXPLAIN ANALYZE`.

## Migrations
- One centralized migration directory and global migration history.
- Development: TypeORM `synchronize: true` allowed.
- Staging/Production: `synchronize: false`, migrations only.
- Production migrations run automatically during approved deployment after a successful pre-migration backup.
- Failed backup stops deployment before migration.
- Failed migration stops deployment.
- Use expand-and-contract for risky schema changes.
- Destructive migrations require deliberate staged release and recovery plan.
- Never assume application rollback means schema rollback is safe.

## Database roles
- Runtime role is not table/schema owner and cannot bypass RLS.
- Separate migration and backup roles.
- No runtime superuser.
- Exact grants are POC/implementation verified.
