# Architecture Decision Records

The decision register in `../decision-register.md` is the initial consolidated record. As implementation begins, create individual ADRs for decisions that are likely to change or require a rationale trail.

Do not create one ADR for every trivial library setting. Prioritize:
- Authentication and session model.
- Tenant isolation and RLS.
- Backend modular monolith.
- KMP/native UI boundary.
- API contract bootstrap and OpenAPI generation.
- Database migration and recovery strategy.
- Deployment topology.
- Payment and subscription invariants.
- Data ownership/privacy.

Use `../../templates/adr-template.md`.


New ADRs should be created for material decisions such as:
- Platform-admin global role/permission and cross-tenant access design.
- Browser authentication/session integration for the admin SPA.
- Any future introduction of shared packages, pnpm workspaces, orchestration tooling or repository restructuring.
- OpenAPI TypeScript client generation and static admin hosting.
