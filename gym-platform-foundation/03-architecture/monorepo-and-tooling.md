# Repository Structure and Application Tooling

## Status

**Selected implementation layout:** One Git repository containing two independently managed JavaScript applications.

**Current repository name:** `gym-backend-app` (working name; the root package is named `gym-platform`).

**Orchestration:** No pnpm workspace and no Turborepo. Each application is bootstrapped and run using its native toolchain. Root scripts provide convenient entry points.

This is a pragmatic single-repository layout. It is not a conventional package-workspace monorepo, and no workspace tooling is required to preserve application boundaries.

## Current repository layout

```text
gym-backend-app/
├── AGENTS.md                    # Repository-wide agent instructions (to add/maintain)
├── README.md
├── package.json                 # Root SolidJS + Vite Platform Admin application
├── pnpm-lock.yaml               # Root application's dependency lockfile
├── index.html
├── vite.config.ts
├── tsconfig.json
├── src/                          # Platform Admin frontend
│   ├── App.tsx
│   ├── index.tsx
│   └── ...
│
└── gym-backend/
    ├── AGENTS.md                # NestJS-specific agent instructions
    ├── README.md
    ├── package.json             # NestJS application dependencies and scripts
    ├── pnpm-lock.yaml           # Backend lockfile, if backend is managed with pnpm
    ├── nest-cli.json
    ├── tsconfig.json
    ├── tsconfig.build.json
    ├── src/
    ├── test/
    └── ...
```

The exact generated files may vary. Keep the two applications' source, TypeScript configuration, dependencies, tests and build lifecycle independently owned.

## Application responsibilities

### Root application — Platform Admin

- SolidJS + Vite single-page application.
- Owns the internal Platform Admin user interface.
- Consumes NestJS APIs; it does not connect directly to PostgreSQL.
- Does not contain authoritative business rules or platform authorization decisions.
- Root `package.json` owns frontend dependencies and exposes convenience scripts for backend tasks.

### `gym-backend/` — API

- NestJS modular monolith.
- Owns authentication integration, authorization, domain logic, persistence, migrations, jobs and business APIs.
- Owns backend dependencies, tests, TypeScript configuration and build lifecycle.
- Must not import frontend UI code.

### Future mobile application

- Owner and Member mobile experiences remain in the planned Kotlin Multiplatform architecture.
- Mobile is not part of the current repository bootstrap and must not be added as an empty placeholder merely to match a diagram.
- When introduced, its Gradle and Xcode project files remain authoritative for Android/KMP and iOS builds. Its repository placement is a separate decision/ADR; do not assume it must become a pnpm package.

## Package management

Use pnpm consistently for the JavaScript/TypeScript applications, but manage them independently:

- Run root dependency installation from the repository root.
- Run backend dependency installation from `gym-backend/`.
- Keep a lockfile owned by each independently installed application.
- Do not add `pnpm-workspace.yaml` or use workspace protocol dependencies.
- Do not use npm in one application and pnpm in the other. Remove a stale `package-lock.json` only after confirming the backend dependencies have been installed and locked with pnpm.
- Pin and document compatible Node.js and pnpm versions when the runtime baseline is finalized.

There is no shared dependency graph today. If a genuinely reusable package is later required, first document the use case and evaluate whether a workspace solves a real problem. Revisit this decision through an ADR rather than introducing workspace configuration pre-emptively.

## Root command interface

The root package should expose predictable commands for the two applications. The current intended script interface is:

| Command | Responsibility |
|---|---|
| `pnpm dev` | Start root SolidJS/Vite application |
| `pnpm build` | Build root SolidJS/Vite application |
| `pnpm serve` | Preview the root Vite production build |
| `pnpm dev:api` | Start NestJS in watch mode from `gym-backend/` |
| `pnpm build:api` | Build NestJS |
| `pnpm test:api` | Run backend unit tests |
| `pnpm test:e2e:api` | Run backend end-to-end tests |

These root scripts delegate to script names defined in `gym-backend/package.json`. If the backend script names change, update the root aliases and this document together. Do not claim that `pnpm dev` starts both applications; it starts the frontend only.

Run both applications in separate terminal sessions during development. A concurrent runner may be added later only if it works reliably in the target development environment and does not obscure application logs or process failures.

## Application boundaries and contracts

- The frontend communicates with the backend through HTTP APIs.
- Approved OpenAPI documents are the contract source after they are generated and reviewed.
- Do not import NestJS DTOs, TypeORM entities, repositories or backend source files into the frontend.
- Do not share persistence models across the application boundary.
- A generated TypeScript API client may be introduced after a generator POC; generated code must be reproducible and must not be hand-edited.
- Better Auth's supported browser integration is used for browser authentication; do not assume native Bearer-session behavior automatically defines browser authentication.

## Build, test and CI

- Use Vite's native commands for the root frontend.
- Use NestJS's native scripts for the backend.
- CI should detect changes under root frontend paths and `gym-backend/` and run the relevant checks independently.
- Backend integration tests must use PostgreSQL/PostGIS where required; SQLite or mocks are not substitutes for RLS/PostGIS acceptance tests.
- Keep backend and frontend build/test results independently observable.
- Do not introduce Turborepo caching or task-graph configuration unless a later measured need justifies it.

## Future evolution

Possible future additions include a mobile project, generated API client, infrastructure scripts and shared tooling. Add them only when implementation requires them and record their ownership and build commands. A future repository restructuring is allowed, but it must preserve application boundaries and be documented before paths and scripts are changed.
