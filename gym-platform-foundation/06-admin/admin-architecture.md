# Platform Admin Frontend Architecture

## Application model

A SolidJS + Vite client-side single-page application (SPA), deployed as static assets. NestJS remains the only application backend and owns all business logic, data access, authentication integration and authorization decisions.

Astro and SolidStart are not part of this application. React is not permitted.

## Technology baseline

| Concern | Baseline |
|---|---|
| UI | SolidJS |
| Tooling | Vite |
| Language | TypeScript strict mode |
| Routing | Solid Router |
| Server state | TanStack Query for Solid, compatibility POC |
| Forms/schema validation | Zod plus a selected Solid-compatible form approach |
| Styling | Tailwind CSS |
| Charts | Apache ECharts, only for meaningful supported data |
| API | Approved OpenAPI-derived TypeScript client where useful |
| Unit/component tests | Vitest and Solid testing utilities |
| Browser tests | Playwright |

Use compatible maintained versions. Avoid adding libraries without a clear use case.

## Suggested structure

```text
gym-backend-app/  # repository root
├── public/
├── src/
│   ├── app/                 # app shell, router, providers, route guards
│   ├── components/
│   │   ├── ui/              # reusable accessible primitives
│   │   ├── layout/          # sidebar, header, page shell
│   │   ├── feedback/        # loading, empty, error, toast, dialogs
│   │   └── data-display/    # tables, pagination, status labels
│   ├── features/
│   │   ├── auth/
│   │   ├── overview/
│   │   ├── organizations/
│   │   ├── users/
│   │   ├── system-health/
│   │   ├── audit-logs/
│   │   └── settings/        # only when supported
│   ├── lib/
│   │   ├── api/
│   │   ├── auth/
│   │   ├── query/
│   │   ├── validation/
│   │   └── formatting/
│   ├── styles/
│   └── main.tsx
├── tests/
├── .env.example
├── package.json
└── vite.config.ts
```

Feature-specific components, query functions, schemas and types should live near their feature. Avoid unnecessary global stores and abstraction layers.

## Data ownership and fetching

- Backend responses are server state; use query caching/invalidation rather than duplicating them in global client stores.
- Solid signals/stores/context are for local UI state and application-level concerns only.
- Use centralized API configuration, consistent error normalization and typed request/response contracts.
- Server-side pagination/filtering/sorting is required for potentially large datasets.
- Cancel obsolete search requests where supported.
- Invalidate relevant queries after successful mutations.
- Use conservative retry behavior; do not retry authorization, validation or other permanent errors blindly.
- Periodic refetch is limited to selected overview/health data and pauses or slows when the tab is inactive where practical.

## API contract governance

- Use the approved Gym Business OpenAPI contract as the source for business API types/client generation.
- Use Better Auth's supported browser client/API integration for authentication; do not invent an independent auth protocol.
- Do not import NestJS DTO classes, TypeORM entities or backend persistence types into the browser bundle.
- Do not manually edit generated clients.
- If a needed capability is absent from OpenAPI/backend, document the gap and propose a backend change.

## Authentication and browser security

The native mobile Bearer-session strategy does not automatically define browser authentication. Verify Better Auth's supported browser session/cookie approach separately.

Before implementing login, prove:
- Browser session creation and retrieval.
- Logout and server-side revocation.
- Session expiry and unauthorized response behavior.
- Secure cookie settings in production.
- CSRF protections appropriate to the chosen cookie/auth flow.
- CORS and trusted-origin configuration.
- Local development origin strategy.
- Explicit platform-admin authorization on every protected backend operation.

Do not store passwords or long-lived sensitive session tokens in localStorage. Do not assume a frontend route guard provides security. Backend authorization is mandatory.

## Platform-admin authorization boundary

Platform administration is a global platform-level authority, separate from organization Owner/Member roles. Do not reuse Owner permissions or infer platform authority from organization membership.

Any platform-wide or cross-tenant data access must be explicitly authorized, narrowly scoped, and audited. It must not be implemented by silently bypassing PostgreSQL RLS or using a superuser database connection. The backend architecture must define a controlled and testable platform-admin access path before such endpoints are exposed.

## Visual and interaction system

- Professional, restrained SaaS administration UI.
- Light/dark/system theme support.
- Shared design tokens for color, typography, spacing, borders and focus.
- Semantic HTML, keyboard navigation, visible focus, screen-reader labels and adequate contrast.
- Responsive sidebar and tables.
- Clear loading, empty, error, stale and success states.
- Avoid decorative or fabricated charts, excessive gradients and unnecessary motion.

## Deployment

Build a static production bundle with Vite. Serve it through an approved static hosting path (Cloudflare Pages or the existing Caddy/VPS deployment are candidates; final choice is open until deployment POC). Configure SPA history fallback, environment-specific API origin and secure headers. Never embed server secrets in Vite environment variables; client-exposed variables are public.

## Testing

- Unit tests for formatting, validation and UI logic.
- Component tests for forms, tables and feedback states.
- Browser tests for login, unauthorized access, navigation, pagination, filtering and error states.
- Test that a regular Owner/Member cannot access admin APIs even when directly calling them.
- Do not use mock data as production fallback.
