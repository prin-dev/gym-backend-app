# Domain Overview

## Core distinction

The model separates global identity and personal fitness data from organization-owned gym operations.

### Global/user-owned concepts
- User
- Personal fitness profile
- Workout session
- Workout exercise and set records
- Global exercise catalogue

### Organization-owned concepts
- Organization
- Branch
- Organization owner relationship / role assignment
- Organization membership
- Join request
- Owner invitation
- Subscription plan
- Subscription
- Payment
- Attendance
- Notification
- Audit event

## Core entities

| Entity | Ownership | Purpose |
|---|---|---|
| User | Global | Identity and account |
| Organization | Tenant root | Gym business |
| Branch | Organization | Physical location |
| OrganizationMembership | Organization + user | Relationship and member status |
| OwnerMembership/RoleAssignment | Organization + user | Owner authorization relationship |
| JoinRequest | Organization | Request to join |
| OwnerInvitation | Organization | Invitation to co-own |
| SubscriptionPlan | Organization | Reusable plan definition |
| Subscription | Organization membership | Snapshot of purchased/assigned terms |
| Payment | Organization/subscription | Payment collection record |
| Attendance | Organization/membership/branch | Successful check-in |
| PersonalFitnessProfile | User | Optional fitness profile |
| Exercise | Platform catalogue | Standard exercise metadata |
| WorkoutSession | User | User-owned workout |
| WorkoutExercise/Set | User/workout | Performed exercise and sets |
| Notification | User, optionally organization-scoped | In-app notification |
| AuditLog | Actor and organization context | Append-only audit record |

## Relationship principles

- A user may have many organization relationships.
- An organization has one or more owners.
- An organization has one or more branches in the data model; V1 UI creates one.
- Organization membership is not the same as a subscription.
- A subscription belongs to an organization membership.
- A subscription contains a snapshot of agreed terms.
- A payment belongs to a subscription.
- Attendance belongs to a membership and branch.
- Workout data belongs to the user, not to a gym.
- Organization-owned rows carry `organization_id` and are protected by RLS.
