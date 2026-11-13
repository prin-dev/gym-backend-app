# Backend Architecture

## Style

Pragmatic Domain-Driven Design in a NestJS modular monolith.

- Thin controllers.
- Application services orchestrate use cases and transactions.
- Domain entities/services encapsulate meaningful invariants.
- Module-owned repositories isolate TypeORM persistence.
- TypeORM entities may also serve as domain entities where appropriate; avoid duplicate domain/persistence models unless justified.
- Use explicit foreign-key ID properties as the default; selective relation decorators are allowed.
- Avoid eager relation loading and N+1 queries.
- Use direct application-service calls within a transaction; use domain events/outbox for independent asynchronous reactions.

## Module boundaries

- Each module owns its entities, repositories and domain logic.
- Other modules use exported application services or documented domain events.
- Do not access another module's internal repository/entity directly.
- Shared modules should contain genuinely shared infrastructure, not become a dumping ground.
- Cross-module atomic operations use one transaction and transaction-aware repositories.

## Suggested module structure

```text
src/
  modules/
    memberships/
      memberships.module.ts
      presentation/
      application/
      domain/
      infrastructure/
    subscriptions/
    payments/
    attendance/
  shared/
    database/
    auth/
    tenancy/
    observability/
    errors/
  jobs/
  config/
```

The exact folder layout may evolve, but module ownership and boundaries must remain.

## Persistence and transactions

- TypeORM with PostgreSQL.
- One centralized migration directory and one migration history.
- Native TypeORM PostgreSQL connection pool for V1; configure limits and timeouts.
- pg-boss connections must be included in total connection budgeting.
- Use PostgreSQL transactions for atomic business operations.
- External side effects occur after commit through outbox/jobs.
- Database constraints are defense in depth.
- Restrict deletion by default; cascade only for genuinely disposable child data.
- PostgreSQL ENUM types are selected for well-defined lifecycle states; evolve through reviewed migrations.
- JSONB is selective for flexible metadata, not core relationships or frequently queried business fields.

## Validation and API
- NestJS DTOs with class-validator and class-transformer.
- Global validation: whitelist, forbid non-whitelisted, transform.
- OpenAPI decorators align with actual DTO behavior.
- Standard success envelope for business APIs.
- RFC 9457 Problem Details for errors with stable machine-readable `code` and `traceId`.
- Better Auth endpoints retain Better Auth's own contract.

## Code quality
- TypeScript strict mode.
- ESLint and Prettier.
- Avoid `any` and unsafe assertions.
- Jest unit, integration and E2E tests.
- Real PostgreSQL + PostGIS for integration tests involving RLS, constraints and spatial behavior.
