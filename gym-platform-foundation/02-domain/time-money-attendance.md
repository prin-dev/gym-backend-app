# Time, Money and Attendance Rules

## Time
- Store absolute instants as PostgreSQL `TIMESTAMPTZ`.
- Store business calendar dates as `DATE`.
- API timestamps use ISO 8601 UTC, for example `2026-11-13T10:30:00Z`.
- API calendar dates use `YYYY-MM-DD`.
- Organization timezone defaults to `Asia/Kolkata` and is stored as an IANA timezone identifier.
- Organization timezone controls subscription validity dates, expiry reminders, attendance day boundaries and dashboard reporting.
- Backend/database time is authoritative; never trust device clock for subscription or attendance eligibility.

## Money
- V1 currency is INR.
- Store amounts as integer paise; ₹1,250.50 = `125050`.
- Never use floating-point arithmetic for money.
- Currency code is explicit.
- Collected revenue is confirmed payments received in the selected period.
- Pending payments and subscription face value are not collected revenue.

## Attendance
- Member-initiated check-in only.
- One successful check-in per member per organization per local calendar day.
- No checkout/duration tracking in V1.
- Eligibility: active subscription, membership not suspended/terminated, and location inside configured branch geofence.
- Default geofence radius: 50m.
- Backend PostGIS calculation is authoritative.
- GPS spoofing cannot be fully prevented; do not claim otherwise.
- A configurable GPS accuracy threshold is required, with exact threshold selected during implementation/POC.
- Failed attempt raw coordinates are not retained.
- DIGIPIN is supplementary only and not the authoritative geofence or security control. If used, verify the official specification and implementation before adoption.
