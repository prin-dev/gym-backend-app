# Infrastructure, Environments and Deployment

## Hosting model
- NestJS and PostgreSQL/PostGIS run in Docker on a conventional VPS initially.
- Cloudflare provides DNS/CDN/edge protection and R2 object storage.
- Do not rewrite NestJS to Hono/Workers solely for hosting cost.
- No Kubernetes or Terraform in V1.
- Caddy is the reverse proxy and automatic HTTPS provider; Cloudflare sits in front.
- NestJS trusted-proxy configuration must be explicit; do not blindly trust forwarded headers.

## Environments
Exactly three environments:
1. Development — local developer environment, Docker Compose, TypeORM `synchronize: true`, disposable synthetic data.
2. Staging — deployed, separate DB and secrets, migrations only.
3. Production — deployed, separate DB and secrets, migrations only.

Staging and Production initially share one VPS but use separate Compose projects, networks, databases, credentials, volumes, domains and resource limits. This is not host-level isolation; a VPS failure or resource exhaustion can affect both.

Domains:
- Production: `api.<owned-domain>`
- Staging: `staging-api.<owned-domain>`
Actual domain selected later.

## Containers
Initial Compose services:
- `api`: NestJS API and pg-boss workers in same process initially.
- `postgres`: PostgreSQL + PostGIS, not publicly exposed.
- `proxy`: Caddy.
Persistent volumes, restart policies, health checks, resource limits and monitored disk usage required.

## CI/CD
- GitHub Actions.
- Staging automatically deploys after merge to `develop`.
- Production deployment after merge to `main` waits for manual approval using GitHub Environments.
- GitHub-hosted runner connects to VPS via restricted SSH credentials.
- Docker images stored in GHCR (registry only, not hosting).
- Use immutable image tags, preferably commit SHA.
- Deploy sequence: pull image, pre-migration backup, migration safety checks, run migrations, replace container, health checks.
- Failed pre-migration backup or migration stops deployment.
- Brief downtime accepted in V1; stop-and-replace deployment.
- Retain previous image for application rollback.
- Do not automatically reverse destructive DB migrations.
- Expand-and-contract migrations for risky schema changes.

## Staging data
Synthetic fictional data only. Never copy production personal or financial data.

## Secrets
- Local `.env` excluded from Git.
- `.env.example` uses safe placeholders.
- Staging and Production secrets independent.
- GitHub Actions secrets/environments.
- Startup configuration validation.
- No secrets in foundation repository or logs.

## Image registry
GHCR stores versioned Docker images. It does not host or run the application.


## Platform Admin frontend deployment

The Platform Admin Dashboard is a SolidJS + Vite SPA built into static assets. It is deployed independently from the NestJS API so frontend releases do not require backend process replacement.

Candidate hosting paths:
- Cloudflare Pages/static hosting.
- Static assets served by the existing Caddy/VPS deployment.

The final choice is open until a deployment POC considers cost, custom domains, access controls, cache invalidation, SPA history fallback, environment configuration and operational simplicity.

Requirements:
- Separate Development, Staging and Production API origins/configuration.
- Client-exposed Vite variables contain public configuration only; never put secrets in the frontend bundle.
- Configure SPA route fallback and secure response headers.
- Restrict admin access through application authentication/authorization; obscurity of the URL is not security.
- Avoid public caching of authenticated/private API responses. Static hashed assets may use long-lived immutable caching.
- Keep admin deployment and rollback independent of backend and mobile releases.
