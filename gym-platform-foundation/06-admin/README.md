# Platform Admin Dashboard

## Purpose

The Platform Admin Dashboard is an internal web application for authorized operators of the Gym Platform SaaS. It is distinct from the Gym Owner and Member applications.

## Technology

- SolidJS.
- Vite.
- TypeScript.
- Solid Router.
- TanStack Query for Solid for server state, subject to dependency compatibility verification.
- Zod for runtime validation where useful.
- Tailwind CSS for styling, subject to design-system implementation choices.
- Apache ECharts for data visualization where supported by real API data.
- Vitest and Playwright for testing.

Do not use React, Astro or SolidStart for this application. The intended rendering model is a client-side SPA served as static assets. NestJS remains the only application backend.

## Canonical specifications

- `admin-experience.md` defines the V1 scope and operator journeys.
- `admin-architecture.md` defines frontend architecture, data fetching, authentication integration and UI conventions.
- `03-architecture/identity-and-authorization.md` defines platform-admin authorization boundaries.
- `04-api/endpoint-capability-map.md` defines required admin API capabilities.
- `04-api/openapi/` (or another explicitly approved contract path if repository structure changes) contains generated API contracts when available.

## Scope discipline

Only implement a screen or operation when its backend capability and authorization policy exist. Missing backend capabilities must be reported and proposed; do not fake them with static data.
