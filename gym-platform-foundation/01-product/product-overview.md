# Product Overview

## Product concept

A gym operations and personal fitness platform with two customer-facing experiences and one internal operations interface:

- **Owner application:** operate a gym business, manage members, plans, subscriptions, manual payments, attendance and operational performance.
- **Member application:** join gyms, view memberships, check in, track workouts and maintain personal fitness history.
- **Platform Admin Dashboard:** an internal web interface for explicitly authorized platform operators to inspect platform operations, organizations, users, system health and audit events. It is separate from gym-owner workflows.

The V1 objective is reliable gym operations plus member consistency. It is not intended to be an all-in-one fitness super-app.

## Product principles

1. Gym operations must be dependable and straightforward.
2. Member fitness tracking must remain useful without gym membership.
3. A person's identity and personal fitness history are global; gym relationships are organization-scoped.
4. Financial and membership history must not be silently overwritten or deleted.
5. Owners should get actionable operational visibility without an elaborate analytics subsystem.
6. Separate Owner and Member apps should have distinct workflows, not a role-switching universal app.
7. The product should be affordable to operate initially and avoid infrastructure that has no demonstrated need.

## Applications

| Application | Platform | Primary audience |
|---|---|---|
| Owner Android | Android / Jetpack Compose | Gym owners |
| Member Android | Android / Jetpack Compose | Gym members and independent fitness trackers |
| Owner iOS | iOS / SwiftUI | Gym owners |
| Member iOS | iOS / SwiftUI | Gym members and independent fitness trackers |
| Platform Admin | Web / SolidJS + Vite | Explicitly authorized platform operators |

The four native apps are independently released. They share one KMP codebase for business logic and data layers, not UI. The Platform Admin Dashboard is a separate SolidJS + Vite SPA and consumes the NestJS backend; it is not a second backend.

## Product release direction

- **V1:** gym operations and personal fitness tracking.
- **V1.1:** staff management and engagement/retention features such as streaks, badges, leaderboards and challenges.
- **V2:** AI insights, fitness intelligence and advanced business analytics.
- **V3:** advanced training ecosystem and SaaS expansion.

Release labels are planning targets, not promises that every deferred item will ship in that exact version.

## Initial business model posture

The initial release is not a fully commercialized SaaS platform:
- No platform billing or premium tiers in V1.
- One organization creation per owner account in V1.
- A user may co-own multiple organizations through invitations.
- Architecture should not prevent future SaaS billing, organization limits or premium plans.
