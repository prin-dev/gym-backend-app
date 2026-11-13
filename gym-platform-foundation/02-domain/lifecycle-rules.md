# Domain Lifecycle Rules

## Join request

States: `PENDING`, `APPROVED`, `REJECTED`, `CANCELLED`.

- One pending request per user/organization.
- Approval creates membership, not an automatic paid subscription.
- Rejection permits immediate reapplication.
- A user already in the organization cannot duplicate a request.
- Owners are equal approvers.

## Organization membership

Conceptual states: `ACTIVE`, `SUSPENDED`, `TERMINATED`.

- Suspension blocks attendance/access where applicable but does not pause subscription validity.
- Termination revokes organization membership access and terminates eligible subscription access.
- Historical payments, attendance, membership and subscription records remain.
- Rejoining creates a new membership record.
- Member and owner can initiate departure/removal as defined by permissions.
- Last owner cannot leave or be removed.

## Subscription

States: `PENDING_PAYMENT`, `SCHEDULED`, `ACTIVE`, `EXPIRED`, `TERMINATED`.

Transitions:
- `PENDING_PAYMENT` -> `SCHEDULED`: first payment confirmed and start date is in the future.
- `PENDING_PAYMENT` -> `ACTIVE`: first payment confirmed and start date has arrived.
- `SCHEDULED` -> `ACTIVE`: start date reached.
- `ACTIVE` -> `EXPIRED`: end date has passed.
- Eligible subscription -> `TERMINATED`: membership terminates.

Rules:
- Backend clock is authoritative.
- Calendar validity is inclusive.
- Example: a 30-day subscription starting 13 November ends 12 December and expires at the beginning of 13 December in the organization's timezone.
- Organization timezone defaults to `Asia/Kolkata`.
- Timestamps are stored in UTC; local calendar calculations use the organization's IANA timezone.
- Month/year duration uses calendar arithmetic, not a fixed number of days.
- Only one active subscription per organization membership; a scheduled renewal may follow it.
- Early renewal begins the day after the current period ends.
- After expiry, owner supplies the new start date.
- Complimentary subscriptions are supported with the same general lifecycle.
- Partial payments are allowed; the first confirmed payment activates/schedules according to start date. Outstanding balance remains tracked.

## Payment

- Payment recording and confirmation are distinct.
- A pending payment is not collected revenue.
- Confirmed payment is immutable.
- Pending payment may be edited or cancelled.
- Corrections to confirmed payment require explicit audited adjustment/reversal records.
- No full double-entry accounting or refund workflow in V1.
- Revenue is based on confirmed collection date, not subscription face value.
- Example: ₹3,000 agreed, ₹1,500 confirmed and ₹1,500 pending means collected revenue is ₹1,500.
- Store amount as integer paise. Currency is explicit and V1 supports INR only.

## Membership departure/removal

- Member can leave; owner can remove.
- Organization access is revoked.
- Subscription is terminated according to membership lifecycle.
- Payments, attendance and history are retained.
- No refund workflow is provided in V1.
- Outstanding balances and unused periods remain historical information; do not silently erase them.

## Owner invitation

- Invitation is to an email address.
- Invitee must authenticate with that verified email to accept.
- Expiry: seven days.
- One active invitation per email per organization.
- Owner may cancel.
- Expired invitation may be resent.
- Creator has no permanent special privileges.
- All owners have equal permissions.
- Last owner cannot leave or be removed.

## Workout session

States: `IN_PROGRESS`, `COMPLETED`, `DISCARDED`.

- Start session creates in-progress record.
- Exercises/sets are saved as logged (online-only).
- Interrupted in-progress session is resumable.
- Discard is explicit.
- Completed sessions can be edited or deleted by the owner of the data.
- Gym owners cannot view personal workout history.
