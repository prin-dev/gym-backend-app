# Operations, Observability and Recovery

## Background jobs
- pg-boss backed by PostgreSQL; no Redis in V1.
- Workers run in same NestJS process initially, with a path to a separate worker container.
- Database-backed scheduling and periodic reconciliation.
- Jobs are idempotent, retryable and observable.
- Use transactional outbox: business transaction writes state and outbox event; worker dispatches; consumers are idempotent.
- Failed jobs must be inspectable and recoverable.
- Concurrency settings must not starve API work.

## Notifications
- Persist in-app notification records in PostgreSQL.
- Push via FCM (Android and APNs through FCM for iOS).
- Device tokens stored per user/device; remove invalid tokens.
- Delivery failure does not roll back business operations.
- Fixed expiry reminders: 7 days before, 1 day before, on expiry; member and owner.
- No SMS/email general notifications in V1.

## Media
- Cloudflare R2.
- Separate public and private asset handling.
- Client requests upload authorization from NestJS.
- Backend validates user, type and size; generates object key and short-lived presigned URL.
- Client uploads directly to R2.
- Completion is verified by backend before metadata is finalized.
- Client never receives R2 credentials.
- Incomplete uploads cleaned up.
- Private downloads use short-lived presigned URLs after authorization.

## Observability
- Pino structured JSON logs.
- Sentry across backend and all four apps.
- Correlation/request IDs.
- Health checks.
- VPS resource/disk monitoring.
- pg-boss failure monitoring.
- PostgreSQL `pg_stat_statements`, slow-query analysis, connection and transaction monitoring.
- No Prometheus/Grafana stack in V1.
- Never log passwords, tokens, private workout data, precise location or sensitive payment details.
- SQL logging is environment-based: Development query logging available; Staging errors/slow queries; Production errors/slow queries.

## Rate limiting
- Cloudflare edge protection plus NestJS application-level limits.
- In-memory limiter acceptable for one instance, with documented limitations (resets on restart, no cross-instance coordination).
- Protect login/reset, invitations, join requests, attendance and general API.
- Standard HTTP 429 response.

## Backup and disaster recovery
- Daily PostgreSQL backups.
- Daily retention 7 days; weekly retention 4 weeks.
- Off-site R2 storage.
- Encrypted backup archives.
- Pre-migration production backup for every deployment; deployment stops if backup fails.
- Selected recovery direction: continuous WAL archiving for point-in-time recovery.
- Target RPO approximately 15 minutes; target RTO 2–4 hours.
- These are targets, not guarantees; verify via restore drills.
- Monitor backup and WAL archive failures.
- Test full restore and PITR periodically.
- Evaluate WAL storage/egress/cost and ensure backup role privileges support the chosen method.
- Recovery procedure must cover VPS replacement, database restoration, secrets/config restoration and DNS/proxy cutover.

## External service failure
- Resend/FCM failures: queue and retry; business operations continue.
- R2 outage: media operations fail/defer gracefully; do not pretend upload succeeded.
- Sentry outage: application continues without telemetry.
- Retries must be idempotent.
- Explain pending/delayed outcomes to users where relevant.
