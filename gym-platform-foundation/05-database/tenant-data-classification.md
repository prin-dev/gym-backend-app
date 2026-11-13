# Data Ownership and Classification

## Global account data
Examples: user identity, verified email, authentication/session records.

Owner: the global user account/authentication subsystem.

## Organization business data
Examples: organization, branch, join requests, memberships, plans, subscriptions, payments, attendance, organization notifications and audit records.

Owner: the organization tenant. Access requires organization authorization and RLS.

## User-owned personal fitness data
Examples: optional fitness profile, workout sessions, exercise performance and workout history.

Owner: the individual user. Gym owners cannot access these records, even if the user is a gym member.

## Member-visible organization data
Members may view their own relationship and history for each organization: membership status, subscription details, payment history and attendance history.

## Contact data visible to owners
Owner can see member name, profile photo, email, phone, organization membership status, subscriptions, payment history and attendance history.
Email and phone are visible by default and cannot be hidden in V1. This must be disclosed during gym onboarding.

## Account deletion and retention
- No self-service deletion in V1; support-assisted only.
- Support verifies identity and reviews organization relationships.
- Last-owner relationship must be resolved before account deletion.
- Active memberships/subscriptions must be handled appropriately.
- Personal data is deleted/anonymized where appropriate.
- Financial, attendance and audit records are retained where legally required.
- Exact retention periods require India legal/accounting review; do not invent arbitrary retention durations.
- All support deletion actions are audited.

## Data export
Members can self-service export their personal account data, workout history, membership history, attendance and subscription/payment history associated with their memberships. Exports must exclude other users' data and organization-wide confidential data. Downloads should be protected and time-limited.
